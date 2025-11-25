# 01_db_postgres_basics.md

## Objective / 目的

- Provide the basic setup and operational procedures for PostgreSQL, focusing on installation, initial configuration, and user management.  
- Demonstrate practical understanding of database administration and secure operation within an internal IT environment.
- List up the differences in code between using PostgreSQL locally on Linux or on Docker as needed.
- I used PostgreSQL container on Docker for the hands-on exercises.
- 本ドキュメントでは、PostgreSQLのインストールから初期設定、ユーザー管理までの基本的な手順をまとめます。  
- 社内IT環境におけるデータベース運用・管理の理解と、安全な設定の実践力を示すことを目的としています。
- PostgreSQLをローカルLinux環境へインストールした場合と、Dokcerで使用した場合の違いを適宜併記しています。
- ハンズオンではDocker上に構築したPostgreSQLを使用しています。

---
## Scenario / シナリオ

- The company has introduced PostgreSQL as part of its internal reporting and analytics system.  
- You are assigned to set up a PostgreSQL instance on a Linux server, create users with limited privileges, and ensure that connection and access controls meet internal policy standards.
- 社内レポート・分析システムの一部としてPostgreSQLが導入されることになりました。  
- あなたはLinuxサーバー上でPostgreSQLを構築し、権限を制限したユーザーを作成し、接続設定やアクセス制御が社内ポリシーに準拠していることを確認します。

---
## 1. Install PostgreSQL / PostgreSQL のインストール

- Ensure the service is active / サービスがアクティブであることを確認してください
```console
# Update package repository and install PostgreSQL / リポジトリ更新とPostgreSQLインストール
sudo apt update
sudo apt install postgresql postgresql-contrib -y

# Check PostgreSQL service status / PostgreSQLサービスの状態確認
sudo systemctl status postgresql
```

- When using docker / Docker使用時
```console
docker run -d --name pg-test \
  -e POSTGRES_PASSWORD=StrongPass123! \
  -p 5432:5432 \
  postgres:14
```

- For Docker configuration, you can also check -> 11_network/network_setup_lab.md
---

## 2. Access PostgreSQL Shell / PostgreSQL シェルにアクセス

```console
# Switch to postgres user and open psql shell / postgresユーザーに切り替えてpsqlシェル起動
sudo -i -u postgres psql
```
- For default, docker username is `postgres` /Docker イメージでは通常、初期ユーザーは `postgres`
- Logged in successfully when `postgres=#` appears /プロンプトが出れば成功

- When using Docker from previous demo / 前章のデモのDockerを使っている場合
```console
docker exec -it clinetA bash
psql -U postgres
```
- Use `sh` for Alpine OS / Alpineベースなら `sh`
- Run SQL command once logged in container. / データベース作成やユーザー作成のコマンドは コンテナ内でそのまま SQL を実行可能

---

## 3. Create Database and User / データベースとユーザー作成

- Limit privileges improves security / 最小権限の付与でセキュリティ向上
```sql
-- Create a new database / 新しいデータベース作成
CREATE DATABASE company_db;

-- Create a user with a strong password / 強力なパスワードでユーザー作成
CREATE USER analyst_user WITH ENCRYPTED PASSWORD 'StrongPass123!';

-- Grant basic connect privileges / データベース接続権限を付与
GRANT CONNECT ON DATABASE company_db TO analyst_user;

-- Create table / テーブルを作成
CREATE TABLE sample_table (
  id SERIAL PRIMARY KEY,
  name TEXT
);

-- Assign limited schema and table privileges / スキーマとテーブルに限定的権限を付与
GRANT USAGE ON SCHEMA public TO analyst_user;
GRANT SELECT, INSERT, UPDATE ON ALL TABLES IN SCHEMA public TO analyst_user;
```

- When using Docker / Dockerの場合
```sql
CREATE DATABASE company_db;
CREATE USER analyst_user WITH ENCRYPTED PASSWORD 'StrongPass123!';
GRANT CONNECT ON DATABASE company_db TO analyst_user;
CREATE TABLE sample_table (id SERIAL PRIMARY KEY, name TEXT);
GRANT SELECT, INSERT, UPDATE ON ALL TABLES IN SCHEMA public TO analyst_user;
```
- Commands are basically same as using directory both on Linux host or Docker
  / SQL実行文は、Linux上で直接PostgreSQL使用するときと、Docker上で使用するときは、ほぼ同じです
- Note for network access settings to edit files from host OS / ただしファイル編集やホストからのアクセスは Docker ネットワーク設定を意識する必要があります
- Set least privileges / 権限は最小限に絞ることでセキュリティ向上

- Verify settings / 確認
```sql
# For database (company_db)
\l
or
SELECT datname FROM pg_database;

# For user (analyst_user)
\du
or
SELECT rolname FROM pg_roles;

# For permission to table
SELECT datacl FROM pg_database WHERE datname = 'company_db';

# For permission to command (SELECT, INSERT, UPDATE)
\z
or
SELECT grantee, privilege_type
FROM information_schema.role_table_grants
WHERE table_schema = 'public' AND grantee = 'analyst_user';

# For scheme
SELECT nspname, has_schema_privilege('analyst_user', nspname, 'USAGE')
FROM pg_namespace
WHERE nspname = 'public';
```
- One of verifications (For permission to command) / コマンド権限確認 (For permission to command) <br>
![screenshot](../../images/14_verify_postgres_CommandPermission.png)

---

## 4. Configure Remote Access on Linux host/ Linux上でのリモート接続設定

```console
# Edit main PostgreSQL configuration / PostgreSQLメイン設定を編集
sudo nano /etc/postgresql/14/main/postgresql.conf

# Set listen_addresses to allow remote connections / リモート接続を許可するためlisten_addressesを設定
listen_addresses = '*'

# Edit pg_hba.conf to allow internal subnet access / 社内ネットワークからの接続を許可
sudo nano /etc/postgresql/14/main/pg_hba.conf

# Add rule for analyst_user on internal subnet / 内部サブネット向けルール追加
host    all             analyst_user     192.168.10.0/24       md5

# Restart PostgreSQL to apply changes / 設定反映のためPostgreSQLを再起動
sudo systemctl restart postgresql
```

- When using Docker, SSH connection settings is strongly discouraged. / DockerではSSH接続は非推奨です
- You can also check-> 
	- 3.1.2 Image configuration defects by NIST Application Container Security Guide chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-190.pdf


---

## 5. Verify Connection / 接続確認

- Ensure the connection works and data can be inserted / 接続とデータ操作が可能であることを確認
```console
# Connect from another host / 別ホストから接続
psql -h 192.168.10.50 -U analyst_user -d company_db

# Create a test table and insert data / テストテーブル作成とデータ挿入
CREATE TABLE test_data (
  id SERIAL PRIMARY KEY,
  value TEXT
);

INSERT INTO test_data (value) VALUES ('Connection successful!');

# Confirm the data / データを確認
SELECT * FROM test_data;
```

- When using docker, varify connectivity from Linux host. /接続確認（ホストから）
	-  `192.168.10.50` is Linux host IP / `192.168.10.50` はLinuxホストIP
	- Create table and insert data for a test / テストテーブル作成・データ挿入で確認
```console
# Look for status, 5432->5432/tcp
docker ps

# Verify connection
psql -h 192.168.10.50 -U analyst_user -d company_db
```
---

## 6. Maintenance and Backup / メンテナンス・バックアップ
- Consider automating backups with cron / cronでバックアップ自動化を検討
```console
# Run a manual backup / 手動バックアップ
pg_dump company_db > /var/backups/company_db_$(date +%F).sql

# Check disk usage of PostgreSQL / ディスク使用量を確認
sudo du -sh /var/lib/postgresql/data
```

- When using docker from previous task./ 
```console
# Copy backup from posgres to linux host
docker exec -t clientA pg_dump -U postgres -F c -d company_db > /FullPathToDireectory/backup.dump

# Check the location of PGDATA / 
docker exec -it clientA printenv | grep PGDATA

# Varify the disk usage /ディスク使用量確認：
docker exec -it clientA du -sh /PathToPGDATAfile/

# output example
63M      /PathToPGDATAfile/

```
---

## 7. Security Hardening / セキュリティ強化
- Disable unused roles / 未使用ロールの無効化
- Use strong passwords or IAM authentication / 強力なパスワードまたはIAM認証使用
- Restrict external access via firewall / ファイアウォールで外部アクセスを制限
- Enable log rotation and query auditing / ログローテーションとクエリ監査を有効化
