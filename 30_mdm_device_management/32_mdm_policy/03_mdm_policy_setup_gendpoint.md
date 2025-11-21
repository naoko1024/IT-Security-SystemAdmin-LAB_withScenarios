# mdm_policy_setup_gendpoint.md
## Objective / 目的
- Configure corporate devices (Windows, macOS, Android, iOS) using Google Endpoint for security compliance, policy enforcement, and operational consistency.
- Google Endpointを使用して企業端末（Windows, macOS, Android, iOS）のセキュリティ準拠、ポリシー適用、運用の一貫性を確保する。

---
## Steps / 手順

### 1. Access Google Admin Console / 管理コンソールにアクセス
1. Log in to [Google Admin Console](https://admin.google.com) with admin privileges / 管理者権限でGoogle管理コンソールにログイン
2. Navigate to **Devices → Endpoint Management** / デバイス → エンドポイント管理に進む
3. Enable **Advanced Management** if not already enabled / まだ有効化されていない場合は「高度な管理」を有効化
### 2. Create Device Profiles / デバイスプロファイル作成
- Navigate to **Devices → Configuration → Profiles** / デバイス → 構成 → プロファイルに進む
- Select **Platform** (Windows, macOS, Android, iOS) / プラットフォームを選択（Windows, macOS, Android, iOS）
- Configure **Password Policy / パスワードポリシー**
  - Minimum length 12 / 最小文字数12
  - Require complex passwords / 複雑なパスワード必須
- Configure **Encryption / 暗号化**
  - Windows: BitLocker / macOS: FileVault
  - Enable enforced encryption / 強制暗号化を有効化
### 3. Configure Security Settings / セキュリティ設定
- **Screen Lock / 画面ロック**
  - Auto-lock after inactivity / 非操作時自動ロック
  - Require PIN/password / PIN/パスワード必須
- **Device Updates / 端末アップデート**
  - Enforce automatic OS updates / OSの自動更新を強制
- **Firewall / ファイアウォール**
  - Enable platform-specific firewall rules / プラットフォームごとのファイアウォールルールを有効化
- **App Management / アプリ管理**
  - Whitelist or blacklist applications / 許可・禁止アプリを設定
  - Deploy corporate apps automatically / 企業アプリを自動配布
### 4. Configure Network Settings / ネットワーク設定
- **Wi-Fi Profiles / Wi-Fiプロファイル**
  - SSID auto-connect / SSID自動接続
  - WPA2/WPA3 Enterprise / WPA2/WPA3 Enterprise
- **VPN Profiles / VPNプロファイル**
  - Enforce VPN connection for corporate resources / 企業リソースアクセスにVPN接続を必須化
  - Support split tunneling if required / 必要に応じてスプリットトンネル対応
### 5. Assign Profiles and Policies / プロファイル・ポリシー割り当て
- Assign to organizational units (OUs) / 組織単位（OU）に割り当て
- Test assignment on pilot devices / テスト端末で割り当て確認
- Roll out to all devices gradually / 全端末に段階的に展開
### 6. Monitor Compliance / 準拠状況監視
- Go to **Devices → Reports → Endpoint Management** / デバイス → レポート → エンドポイント管理
- Verify compliance status / 準拠状況を確認
- Investigate non-compliant devices / 非準拠端末を調査
- Apply remediation actions as necessary / 必要に応じて修正対応
