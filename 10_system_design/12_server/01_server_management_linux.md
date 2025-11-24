# 01_server_management_linux.md
## Objective / 目的
- Understand the basic steps to install and configure a Linux server
- Set up secure SSH access and user privileges
- Configure network interfaces and hostname
- Perform routine administrative tasks (package updates, logs, service management)
- Linuxサーバー構築・設定の基本手順を理解します。  
- 安全なSSHアクセスとユーザー権限設定を行います。  
- ネットワークインターフェースとホスト名を設定します。  
- パッケージ更新、ログ確認、サービス管理などの基本運用を実施します。
---
## Scenario / シナリオ

- A new internal system is being prepared for deployment, and you’ve been assigned to set up a Linux server to host several core services. 
- Until now, different teams have managed their own servers separately, leading to inconsistent configurations and maintenance issues.  
- Your goal is to build a unified, secure, and maintainable Linux environment that can serve as a standard for future internal deployments.
- 新しい社内システムの導入を控え、あなたはその基盤となるLinuxサーバーの構築を任されました。  
- これまでは各部署がそれぞれ独自にサーバーを管理しており、設定のばらつきや保守の属人化が問題になっていました。  
- あなたの目的は、今後の社内展開の標準となる「統一された、安全で、保守しやすいLinux環境」を構築することです。
- ユーザー管理、ネットワーク設定、SSHリモートアクセス、基本的なサービス構築を行います。

---
## Step 1: System Preparation / システム準備

```bash
# Update system packages / パッケージ更新
sudo apt update && sudo apt upgrade -y

# Set hostname / ホスト名設定
sudo hostnamectl set-hostname internal-sv01

# Check network interfaces / ネットワークインターフェース確認
ip a
```

---

## Step 2: User and SSH Setup / ユーザーと SSH 設定

```bash
# Create a new user with sudo privileges / 管理者権限ユーザー作成
sudo adduser sysadmin
sudo usermod -aG sudo sysadmin

# Install and start SSH server / SSH導入・起動
sudo apt install openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh

# Edit SSH configuration / SSH設定編集
sudo nano /etc/ssh/sshd_config
# PermitRootLogin no
# PasswordAuthentication yes

# Restart SSH service / SSH再起動
sudo systemctl restart ssh

# Test SSH login from another machine / 別端末から接続確認
ssh sysadmin@192.168.x.x
```

---

## Step 3: Basic Network Configuration / ネットワーク設定

```bash
# Edit Netplan configuration for static IP / 固定 IP 設定
sudo nano /etc/netplan/01-netcfg.yaml
```

Example / 例:
```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: no
      addresses: [192.168.10.50/24]
      gateway4: 192.168.10.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

```bash
# Apply configuration / 設定適用
sudo netplan apply

# Confirm connectivity / 接続確認
ping -c 4 google.com
```
---

## Step 4: Service Installation / サービス導入

```bash
# Install Nginx / Nginx導入
sudo apt install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx

# Check service status / サービス稼働確認
sudo systemctl status nginx
```

- Verify via browser / ブラウザで確認: `http://192.168.10.50`
---

## Step 5: System Maintenance / システム保守
```bash
# Update packages regularly / 定期パッケージ更新
sudo apt update && sudo apt upgrade -y

# Check logs / ログ確認
sudo tail -f /var/log/syslog

# Manage services / サービス管理
sudo systemctl list-units --type=service
```
---

## Step 6: Security Best Practices / セキュリティ対策

```bash
# Enable automatic security updates / 自動セキュリティ更新
sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades
```

- Use key-based SSH authentication (recommended) / SSH鍵認証を推奨
- Disable unused services / 不要なサービス停止
---

## Notes / 補足

- This lab provides a practical foundation for managing Linux servers in small corporate or hybrid environments.  
    本演習は、小規模な企業環境やハイブリッド構成でLinuxサーバーを運用するための実践的基礎となります。
- You can extend this lab later by adding monitoring tools (Prometheus, Grafana) or log collection systems.  
    後続の演習として、監視ツール（Prometheus, Grafana）やログ収集システムを追加して拡張することができます。
