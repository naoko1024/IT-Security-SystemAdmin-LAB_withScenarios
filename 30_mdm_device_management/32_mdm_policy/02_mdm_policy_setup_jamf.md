# mdm_policy_setup_jamf.md

## Objective / 目的
- Configure corporate macOS and iOS devices using JAMF for security compliance, application deployment, and operational consistency.
- JAMFを用いて企業のmacOSおよびiOS端末のセキュリティ準拠、アプリ配布、運用の一貫性を確保する。

---

## Steps / 手順

### 1. Create Configuration Profiles / 構成プロファイル作成
1. Log in to the JAMF Pro console / JAMF Proコンソールにログイン。
2. Navigate to **Computers → Configuration Profiles → New** (for macOS) or **Devices → Configuration Profiles → New** (for iOS) / macOSの場合は **コンピュータ → 構成プロファイル → 新規作成**、iOSの場合は **デバイス → 構成プロファイル → 新規作成** に進む。
3. Name the profile and provide a description / プロファイル名と説明を入力。
4. Select **Platform** and **Payloads** (e.g., Security, Wi-Fi, VPN, Restrictions) / プラットフォームとペイロード（例：セキュリティ、Wi-Fi、VPN、制限）を選択。
### 2. Configure Security Settings / セキュリティ設定
- **Password Policy / パスワードポリシー**
  - Minimum 12 characters / 最小文字数：12
  - Require alphanumeric and special characters / 英数字＋特殊文字必須
- **FileVault / ディスク暗号化**
  - Enable FileVault on macOS / macOSでFileVaultを有効化
  - What is FileVault?
	  - FileVault is **Full Disk Encryption** feature — a mechanism that prevents the data inside from being read if the device is lost or stolen.
	  - It may be **automatically enabled via MDM (e.g., JAMF)**, or if manual setup is required, there are cases where you run `sudo fdesetup enable` in the terminal.
	  - **フルディスク暗号化機能** で、  端末を紛失・盗難されたときに、中のデータを読めなくする仕組み。
	  - **MDM（例：JAMF）で自動的に有効化** されるか、手動作業が必要な場合はターミナルで `sudo fdesetup enable` を実行すケースもあります。
- **Firewall / ファイアウォール**
  - Enable macOS Application Firewall / macOSアプリケーションファイアウォールを有効化
- **Gatekeeper / ゲートキーパー**
  - Allow apps from App Store and identified developers / App Storeおよび認証済み開発者からのアプリを許可
	  - What is Gatekeeper?
	  - GateKeeper is a Application Execution Control feature for macOS.
	  - Blocks unknown apps obtained from the Internet
	  - macOSのアプリ実行制御機能
	  - インターネット上から入手した不明なアプリをブロックします。
### 3. Configure Network / ネットワーク設定
- **Wi-Fi Profiles / Wi-Fiプロファイル**
  - Corporate SSID auto-join / 企業SSID自動接続
  - WPA2/WPA3 Enterprise / WPA2/WPA3 Enterprise
- **VPN Profiles / VPNプロファイル**
  - On-demand VPN or per-app VPN / オンデマンドVPNまたはアプリ単位VPN
  - Include split tunneling if needed / 必要に応じてスプリットトンネル設定
	  - **Split Tunneling** refers to a setting that separates traffic using the VPN from traffic that goes directly to the Internet.
### 4. Configure App Deployment / アプリ配布設定
- Add required apps (Office 365, Teams, proprietary apps) / 必要なアプリを追加
- Assign apps to device groups / デバイスグループに割り当て
- Configure deployment method (managed, optional) / 配布方式を管理下配布または任意に設定
### 5. Assign Profiles / プロファイル割り当て
- Assign to device groups, smart groups, or individual devices / デバイスグループ、スマートグループ、個別デバイスに割り当て
- Verify correct target membership / 対象デバイスが正しく割り当てられているか確認
### 6. Monitor Compliance / 準拠状況監視
- Go to **Devices → Reports → Configuration Profiles** / デバイス → レポート → 構成プロファイル
- Confirm devices have applied the profile successfully / デバイスに正しくプロファイルが適用されていることを確認
- Investigate non-compliant devices and take remediation action / 非準拠デバイスを調査し、修正対応を実施
### 7. Update Policies / ポリシー更新
- Update profiles as security or operational requirements change / セキュリティや運用要件の変更に応じてプロファイルを更新
- Document changes and maintain version history / 変更内容を文書化し、バージョン管理
