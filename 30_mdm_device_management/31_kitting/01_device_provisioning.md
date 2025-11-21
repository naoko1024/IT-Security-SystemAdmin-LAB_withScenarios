# device_provisioning.md

## Objective / 目的
- Understand the overall flow in preparation and configuration for new corporate devices (PCs, Macs, or mobile devices) with standard settings and security policies.
- 新規社用端末（PC、Mac、モバイル端末）の標準設定およびセキュリティポリシーに沿った準備・構成について、おおよその流れを理解する。
## Scenario / シナリオ
- A new employee will start next week. The IT team receives the device from stock, installs required software, applies standard security configurations, and enrolls it in the company’s MDM system so the employee can start work safely.
- 来週新入社員が入社予定です。ITチームは在庫から端末を受け取り、必要なソフトウェアをインストールし、標準セキュリティ設定を適用し、会社のMDMシステムに登録して、安全に業務開始できる状態にします。
## Steps / 手順

### Step 1: Device Preparation / 端末準備
- Unbox and verify hardware.
   / 端末を開封し、ハードウェアを確認します。
- Connect to corporate network (wired or Wi-Fi) for provisioning (to install internal policies and scripts.)
   / プロビジョニング用に社内ネットワーク（有線またはWi-Fi）に接続します.
### Step 2: Standard OS Setup / OS初期設定
 - In enterprise environments, configurations may also be automated through MDM platforms like Intune or JAMF. /　企業環境では、Intune や JAMF のような MDM プラットフォームを通じて、設定が自動化されてる場合もあります
##### Windows example:
   - Run as Administrator, after completing the initial setup. / 「管理者として実行」
   - Execute commands via PowerShell (Windows) / パワーシェルにてコマンドを実行

  ```powershell
# powershell example

# Set computer name
Rename-Computer -NewName "CORP-PC-001"

#  Enable automatic updates
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\Auto Update" -Name "AUOptions" -Value 4

# -Name - Specifies the registry entry (setting) you want to modify. 
# "AUOptions" controls how Windows Update behaves.
# -Value 4 — Sets the value of that entry. The number `4` means “Automatically download and install updates.”
```
	  
##### macOS example:
 - Require sudo in some commands / 一部は管理者権限（sudo）が必要
 - Execute commands via Terminal (macOS). / termina.app にて実行
```bash
# bash example

# Set computer name / コンピュータ名を設定
sudo scutil --set ComputerName "CORP-MAC-001"

# Enable FileVault encryption prompt / ディスク暗号化（FileVault）を有効化
sudo fdesetup enable
```
### Step 3: MDM Enrollment / MDM登録
- Enroll device in company MDM (Intune, JAMF, or Google Endpoint).  
    - / 会社のMDM（Intune、JAMF、Google Endpoint）に端末を登録します。
- Verify enrollment status via MDM console.  
    / MDMコンソールで登録状況を確認します。
### Step 4: Security Configuration / セキュリティ設定
1. Enable disk encryption (BitLocker/FileVault). / ディスク暗号化（BitLocker／FileVault）を有効にする
2. Install antivirus and ensure latest definitions. / ウイルス対策ソフトをインストールし、最新の定義ファイルを適用
3. Apply standard configuration profiles via MDM (password policy, screen lock, firewall settings). / MDM を通じて標準の構成プロファイル（パスワードポリシー、画面ロック、ファイアウォール設定など）を適用
##### What is BitLocker/FileVault?
- Built-in disk encryption feature
- In **personal (non-corporate)** use, you can enable them manually in **Settings**
- In **corporate or managed environments**, they are often **automatically enforced or configured through MDM** (like Intune or JAMF).


- You can also check: 32_mdm_policy/device_encryption_bitlocker.md
### Step 5: Software Installation / ソフトウェア導入
- Install standard applications (Office, VPN client, browser, collaboration tools). / 標準アプリケーション（Office、VPN クライアント、ブラウザ、コラボレーションツール）をインストール
- Verify application licenses and connectivity. / アプリケーションのライセンスと接続性を確認
### Step 6: Final Verification / 最終確認

- Confirm network access, MDM compliance, and antivirus activity. / ネットワークアクセス、MDM コンプライアンス、ウイルス対策ソフトの動作を確認
- Take a snapshot or record device serial for inventory management. / スナップショットを取得するか、デバイスのシリアル番号を記録して資産管理を行う
- Document provisioning completion. / 構成作業（プロビジョニング）の完了を記録
