# standard_config_profiles.md

## Objective / 目的
- Define and deploy standardized configuration profiles across corporate devices to ensure security compliance and operational consistency.
- 企業端末全体で標準設定プロファイルを定義・配布し、セキュリティ遵守と運用の一貫性を確保する。

## Scenario / シナリオ
- A new batch of laptops and mobile devices has arrived for the sales and engineering teams.
- The IT team needs to apply standardized security and operational configurations automatically, including password policies, encryption, VPN access, and USB restrictions, before handing devices to users.
- 新しいノートPCやモバイル端末が営業・技術チーム向けに配備されました。
- ITチームは、ユーザーに渡す前に、パスワードポリシー、暗号化、VPNアクセス、USB制限などの標準設定を自動的に適用する必要があります。
## Configuration Examples (Sample) / 端末に強制される安全・運用ルールの一覧(サンプル)

| Device / デバイス    | MDM Tool / 管理ツール | Config / 設定内容                                                  | Description / 説明                                                                |
| ---------------- | ---------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Windows 10/11 PC | Intune           | Password Policy: min 12 chars, 1 uppercase, 1 number, 1 symbol | Enforces secure passwords across all corporate Windows PCs / 企業PC全体で安全なパスワードを強制 |
| Windows 10/11 PC | Intune           | BitLocker Encryption enabled                                   | Ensures disk encryption for data protection / データ保護のためディスク暗号化を有効化               |
| macOS Laptop     | JAMF             | FileVault Encryption enabled                                   | Encrypts system volume to protect sensitive data / 重要データ保護のためシステムボリュームを暗号化      |
| macOS Laptop     | JAMF             | Password Policy: min 12 chars, 1 uppercase, 1 number           | Apply strong password rules for all Mac users / Macユーザー全体に強力なパスワードルールを適用        |
| All Devices      | Intune/JAMF      | VPN Profile: Auto-connect to corporate VPN                     | Ensures secure network access for remote work / リモートワーク時の安全なネットワーク接続を保証         |
| All Devices      | Intune/JAMF      | USB Restriction Policy                                         | Block unauthorized removable storage / 会社に許可されたUSBのみ使用可能                        |

## Steps / 手順

1. **Prepare Standard Profiles / プロファイル準備**
	- Define security and operational requirements for each device type.
	- デバイス種別ごとのセキュリティ・運用要件を定義します。
2. **Create Profiles in MDM / MDMでプロファイル作成**
	- In Intune or JAMF, create new configuration profiles according to defined requirements.
	- IntuneやJAMFで、定義済み要件に沿った新しい設定プロファイルを作成します。
3. **Assign Profiles to OU or Device Groups / OUやデバイスグループに割り当て**
	- Assign the profiles to organizational units (OU) or device groups.
	- プロファイルを組織単位（OU）またはデバイスグループに割り当てます。
4. **Deploy Profiles / プロファイル配布**
	- Push profiles to all targeted devices.
	- 対象デバイス全てにプロファイルを配布します。
5. **Verify Deployment / 配布確認**
	- Check that each device has applied the profile settings correctly.
	- 各デバイスでプロファイル設定が正しく適用されているか確認します。
6. **Monitor and Update / 監視・更新**
	- Regularly review compliance and update profiles as requirements evolve.
	- 定期的に準拠状況を確認し、要件の変化に応じてプロファイルを更新します。
## Reference
- You can also check ->
	- Microsoft Intune でデバイス構成プロファイルを作成する by Microsoft Intune https://learn.microsoft.com/ja-jp/intune/intune-service/configuration/device-profile-create?tabs=sc
	- Creating Jamf Connect Configuration Profiles Using Jamf Pro by jamf Learning 
