## Objective / 目的

- To provide standardized and secure configuration templates for commonly used Linux server components (SSH, Nginx, Apache), enabling faster deployment and consistent security practices.  
- Linux サーバーの代表的な構成要素（SSH・Nginx・Apache）について、標準化されたセキュアな設定テンプレートサンプルを提供し、構築・復旧作業の迅速化とセキュリティ水準の統一を図ります。
---
## Scenario / シナリオ

- To support rapid recovery during new builds or incident handling, we will provide templates for common configurations such as SSH, Nginx, and Apache.
- IT team organizes the fundamental configuration templates for Linux servers operating inside the internal corporate network.
- 新規構築時や障害対応時に迅速に復旧できるよう、SSH・Nginx・Apache など代表的な設定のテンプレートを共有することになりました。
- 社内ネットワーク上で動作する Linux サーバーにおいて、基本的な設定テンプレートを整理します。
---
## File Summary / ファイル表

|File / ファイル名|Purpose / 用途|Key Security Point / セキュリティ要点|
|---|---|---|
|1. `ssh_config.conf`|SSH設定|root禁止・鍵認証必須|
|2. `nginx.conf`|Nginxサイト設定|Security Headers / 404制御|
|3. `apache2.conf`|Apacheサイト設定|別ポート運用・Header防御|

---

## 1. ssh_config.conf

- SSH Configuration Example / SSH設定サンプル

### Goal / 目的

- Disabling root login and requiring public key authentication, to enforce secure remote access to the management server.
- 管理サーバーへのリモートアクセスをより安全にするため、rootログイン禁止と鍵認証必須を徹底する。

### Sample Configuration / サンプル設定

```nginx
# /etc/ssh/sshd_config

Port 22
Protocol 2
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PermitEmptyPasswords no
ChallengeResponseAuthentication no
UsePAM yes
ClientAliveInterval 300
ClientAliveCountMax 2
```

### Notes / メモ

- Enable key-based authentication to enhance security / 鍵認証を有効化することで安全性を向上
- Automatically disconnect the session after a period of inactivity / 一定時間操作がない場合、自動的に切断
- Ensure that new users always have a `.ssh/authorized_keys` file / 新規ユーザーには `.ssh/authorized_keys` を必ず用意
    
---
## 2. nginx.conf

- Nginx Configuration Example / Nginx設定サンプル

### Goal / 目的

- To configure a basic and secure internal website using Nginx, including static content hosting and security headers.  
- Nginx を用いて社内向け静的サイトを公開し、**セキュリティヘッダー**を適用した安全な構成例を提供する。

### Sample Configuration / サンプル設定

```nginx
# /etc/nginx/sites-available/default

server {
    listen 80;
    server_name intranet.local;

    root /var/www/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    # Security Headers
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";
    add_header X-XSS-Protection "1; mode=block";

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;
}
```

### Notes / メモ

- Use `try_files` to return 404 for unauthorized requests / try_files により不正リクエストを 404 に制御
- Add basic protection with security headers / セキュリティヘッダーで簡易防御を追加
- Standardize log paths for easier auditing / ログパスを統一し、監査で確認しやすくする
    

---

## 3. apache2.conf

- Apache Configuration Example / Apache設定サンプル

### Objective / 目的

- To run Apache for PHP testing on a separate port, enabling coexistence with other services while applying security headers and structured logging.  
- PHP の検証用途として Apache を利用しつつ、Nginx など他サービスと共存できるよう **別ポート運用**と**セキュリティヘッダー**を追加する。


### Sample Configuration / サンプル設定
```pgsql

# /etc/apache2/sites-available/000-default.conf

<VirtualHost *:8080>
    ServerAdmin admin@local
    DocumentRoot /var/www/html

    <Directory /var/www/html>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    # Security Headers
    Header always set X-Frame-Options "SAMEORIGIN"
    Header always set X-Content-Type-Options "nosniff"
    Header always set X-XSS-Protection "1; mode=block"
</VirtualHost>
```
### Notes / メモ

- Operate on port 8080 to coexist with Nginx / ポート 8080 運用で Nginx と共存可能
- Install `libapache2-mod-php` as needed for PHP execution / PHP 実行には libapache2-mod-php を適宜導入
- Prevent `.htaccess` overrides with `AllowOverride None` / AllowOverride None により .htaccess の設定上書きを防止
    
