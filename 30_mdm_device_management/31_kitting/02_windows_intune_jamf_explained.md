# windows_intune_jamf_explained
## Objective / このファイルの目的
- Understand about Microsoft Intune and JAMF, that are endpoint management solutions used by organizations to manage and secure devices.
- 組織がデバイスを管理・保護するために使用するエンドポイント管理ソリューションである、Microsoft Intune や JAMF について理解する。
  
- You can also check -> 32_mdm_policy/
## Microsoft Intune
- Target / 対象: Mainly Windows PCs, Surface devices, iOS, and Android. 
- Role / 役割: Mobile Device Management (MDM) + Mobile Application Management (MAM).  
- モバイルデバイス管理（MDM）＋モバイルアプリ管理（MAM）
- Capabilities / できること:
	- Device enrollment and configuration (password policies, encryption, VPN settings).  /デバイス登録・構成（パスワードポリシー、暗号化、VPN設定）
	- App distribution (Office, internal apps).  /アプリ配布（Office、社内アプリ）
	- Remote wipe / lost device protection.  /リモートワイプ／紛失対策
	- Apply permissions and security policies.  /権限・セキュリティポリシー適用
- Features / 特徴: Easily integrates with Microsoft 365; commonly used in Windows-centric enterprises.  
- Microsoft 365 と統合されやすく、Windows中心の企業で標準的に使われます。

    
- You can also check ->
	- Youtube -> https://www.youtube.com/watch?v=CnACjDAvStk by 日本マイクロソフト公式チャンネル, 5分でわかる Intune Suite
## JAMF
- Target / 対象: Mainly macOS and iOS devices.  
- Role / 役割: MDM specialized for managing Apple devices. 
- Capabilities / できること:
	- Distribute configuration profiles for macOS/iOS.  
	- macOS/iOS の設定プロファイル配布
	- App deployment and update management.  
	- アプリ配布、アップデート管理
	- Security settings (FileVault encryption, screen lock, password policies).  
	- セキュリティ設定（FileVault暗号化、スクリーンロック、パスワードポリシー）
	- Remote control and device wipe.  
	- リモート制御・消去
- Features / 特徴: Essential tool for companies with many Apple devices; not integrated with Windows.  
- Apple端末が多い企業で必須級のツール。Windowsとは統合されていません。

    
- You can also check ->
	- Youtube -> https://www.youtube.com/watch?v=ZW5nfcb-u3M by Jamf, Jamf Pro Overview
## Practical Perspective / 実務視点:
- In IT management, use Intune for Windows-centric environments, and JAMF for environments with many Macs/iPhones.  
- Mixed environments are common, so using both Intune + JAMF is possible.  
- “Standard Config Profiles” often refer to common configuration templates distributed via MDM, including password rules, encryption, USB restrictions, and VPN settings.
  
- 情シスで端末管理をする場合、Windows主体の環境ならIntune、Mac/iPhoneが多い環境ならJAMF が中心。
- 最近は混在環境も多いので、Intune + JAMF の併用もあり。
- 「Standard Config Profiles」は、パスワードルールや暗号化、USB制限、VPN設定など、MDMで配布する共通設定のテンプレートのことを指すことが多いです。
