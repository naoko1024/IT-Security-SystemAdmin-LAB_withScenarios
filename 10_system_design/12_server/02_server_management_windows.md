# 02_server_management_windows.md

## Objective / 目的

- Outline the sample of setup and operational management of Windows Server environments, including account management, Group Policy, and service monitoring.  
- Regarding to hands-on practice on Windows System management, instead of using my host machine, I chose practice on third-party learning resources with pre-configured Windows Virtual Machines (TryHackMe).
- The TryHackMe rooms related to this task are on reference below (some rooms are no subscription).
- Windows Server 環境の構築、および運用管理、アカウント管理、グループポリシー設定、サービス監視など、運用管理に関する基本手順のサンプルを示します。
- WindowsシステムのHands-on演習は、自身のマシンではなく、TryHackMeの環境構築済みのWindowsマシンを使用しました。
- このタスクに関連するTryHackMeのroomは以下です。(一部無料)

---

## References

- TryHackMe
	- Windows Fundamentals 1 https://tryhackme.com/room/windowsfundamentals1xbx
	- Windows Fundamentals 3 https://tryhackme.com/room/windowsfundamentals3xzx
	- Windows Forensics 2 https://tryhackme.com/room/windowsforensics2
	- Active Directory Basics https://tryhackme.com/room/winadbasics

---

## Scenario / シナリオ

- The IT department is consolidating authentication and file-sharing systems, which have been inconsistently managed across departments.  
- You have been assigned to build a Windows Server–based domain to centralize account management, implement Group Policies, and enhance security monitoring.
- 社内の認証やファイル共有が部署ごとにバラバラに管理されており、統合プロジェクトが発足しました。  
- あなたはIT部門の担当者として、Windows Serverベースのドメイン環境を構築し、アカウント管理の統一、グループポリシーの適用、セキュリティ監視の強化を行うことになりました。

---

## Environment Overview / 環境概要

|項目 / Item|内容 / Details|
|---|---|
|OS|Windows Server 2022 (Standard Edition)|
|Management Tools|Active Directory, PowerShell, Group Policy Management|
|Services|File Sharing, RDP, Domain Authentication|
|Access Control|Role-based via AD groups|

---

## Steps / 手順

### Step 1. Initial Setup / 初期設定

- After installing Windows Server, set a static IP and hostname to prepare for domain configuration.  / Windows Serverをインストール後、ドメイン設定の準備として静的IPとホスト名を設定する。

```powershell
Rename-Computer -NewName "SRV-AD01"
New-NetIPAddress -InterfaceAlias "Ethernet0" -IPAddress 192.168.10.10 -PrefixLength 24 -DefaultGateway 192.168.10.1
Set-DnsClientServerAddress -InterfaceAlias "Ethernet0" -ServerAddresses 192.168.10.10
```

- Confirm network settings / ネットワーク設定の確認
```powershell
Get-NetIPAddress
```

---
### Step 2. Domain Controller Promotion / ドメインコントローラへの昇格

- Install AD DS role and promote the server as a domain controller.  / AD DSロールをインストールし、ドメインコントローラとして昇格
```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
Install-ADDSForest -DomainName "corp.local" -InstallDns
```

- After reboot, verify the domain /再起動後、ドメイン状態を確認
```powershell
Get-ADDomain
```
---

### Step 3. Organizational Unit (OU) and User Management / OU・ユーザー管理

- Create OUs for departments and add sample users and groups.  /部署ごとにOUを作成し、ユーザー・グループを追加する。
```powershell
New-ADOrganizationalUnit -Name "IT" -Path "DC=corp,DC=local"
New-ADGroup -Name "IT_Admins" -GroupScope Global -Path "OU=IT,DC=corp,DC=local"
New-ADUser -Name "Yamada Taro" -Path "OU=IT,DC=corp,DC=local" -AccountPassword (ConvertTo-SecureString "P@ssw0rd!" -AsPlainText -Force) -Enabled $true
Add-ADGroupMember -Identity "IT_Admins" -Members "Yamada Taro"
```

---
### Step 4. Group Policy Configuration / グループポリシー設定

- Enforce password policy and restrict RDP access.  /パスワードポリシーを強制し、RDPアクセスを制限

1. Open **Group Policy Management Console (GPMC)**  /「グループポリシー管理コンソール」を開く
2. Navigate to:  
	Default Domain Policy → Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy
3. Set:  
    　- Example:
    　- Minimum password length: 15  
    　- Password expiration: 90 days  
    　- Lockout after 5 failed attempts
   
- You can also check ->
	- How Do I Create a Good Password? by NIST https://www.nist.gov/cybersecurity/how-do-i-create-good-password
    
- To restrict RDP access (example PowerShell):
```powershell
Set-NetFirewallRule -DisplayGroup "Remote Desktop" -Enabled True
Add-LocalGroupMember -Group "Remote Desktop Users" -Member "corp\IT_Admins"
```

---

### Step 5. Security Monitoring and Automation / 監視と自動化

- Enable Windows Defender, configure logging, and create an automated PowerShell maintenance task.  /Windows Defenderを有効化し、ログ監視とPowerShell自動化タスクを設定
```powershell
Set-MpPreference -ScanScheduleDay 1 -ScanScheduleTime 03:00
Get-WinEvent -LogName Security -MaxEvents 10 | Format-List
```

- Schedule a daily backup script (example) /日次バックアップスクリプトのスケジュール設定
```powershell
schtasks /create /tn "DailyBackup" /tr "C:\scripts\daily_backup.ps1" /sc daily /st 02:00
```
---

## Verification / 動作確認

- Domain login test from a client PC.  
    　クライアントPCからのドメインログインテスト。
- Check user policies applied correctly.  
    　ポリシー適用の確認。
- Review event logs for authentication success/failure.  
    　認証ログ（成功・失敗）を確認。
