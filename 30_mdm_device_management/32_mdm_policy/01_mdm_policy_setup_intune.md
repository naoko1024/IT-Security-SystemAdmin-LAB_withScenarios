# mdm_policy_setup_intune.md

## Objective / 目的
- Get familiar with a sample policy, configuring corporate Windows devices using Intune for security compliance, software deployment, and operational consistency.
- サンプルポリシーを用いて、Intuneを使った企業Windows端末のセキュリティ準拠、ソフトウェア配布、運用の一貫性を確保に触れる。
---

## Steps / 手順

### 1. Create Device Configuration Profile / デバイス構成プロファイル作成
1. Navigate to Microsoft Endpoint Manager Admin Center. / Microsoft Endpoint Manager 管理センターにアクセス。
2. Go to Devices → Configuration profiles → Create profile. / デバイス → 構成プロファイル → プロファイルの作成 に進む。
3. Select Platform: Windows 10 and later and Profile type: Templates → Device restrictions.
   / プラットフォーム：Windows 10 以降、プロファイル種類：テンプレート → デバイス制限 を選択。
4. Name the profile and provide a description.
   / プロファイル名と説明を入力。
### 2. Configure Security Settings / セキュリティ設定
- **Password Policy / パスワードポリシー**
	  - Minimum length: 12 characters / 最小文字数：12
	  - Require complex passwords / 複雑なパスワード必須
- **BitLocker / デバイス暗号化**
	  - Enable BitLocker on all fixed drives / 固定ディスクにBitLockerを有効化
- **Firewall / ファイアウォール**
	  - Enable Windows Firewall / Windowsファイアウォールを有効化
- **Windows Defender / Defender設定**
	  - Real-time protection on / リアルタイム保護を有効化

### 3. Configure Network and VPN / ネットワーク・VPN設定
- **Wi-Fi Profiles / Wi-Fiプロファイル**
  - Corporate SSID auto-connect / 企業SSID自動接続
  - WPA2/WPA3 Enterprise / WPA2/WPA3 Enterprise
- **VPN Profile / VPNプロファイル**
  - Auto-connect VPN / VPN自動接続
  - Per-app VPN (if needed) / 必要に応じてアプリ単位VPN

### 4. Configure App Deployment / アプリ配布設定
- Add required corporate applications (MS Office, Teams, custom apps) / 必要な社内アプリを追加
- Assign apps to device groups / デバイスグループに割り当て
- Configure installation behavior (mandatory, optional) / インストール方式（必須、任意）を設定

### 5. Assign Profiles / プロファイル割り当て
- Assign configuration profile to device groups or OUs / デバイスグループやOUにプロファイルを割り当て
- Confirm target device membership / 対象デバイスが正しくグループに所属していることを確認

### 6. Monitor Compliance / 準拠状況監視
- Go to **Devices → Monitor → Compliance** / デバイス → 監視 → 準拠状況
- Check which devices have applied the profile successfully / プロファイルが適用されたデバイスを確認
- Investigate non-compliant devices and apply remediation / 非準拠デバイスを調査し、修正を適用
### 7. Update Policies / ポリシー更新
- Update profiles as security requirements evolve / セキュリティ要件の変更に応じてプロファイルを更新
- Version control changes and document modifications / 変更履歴を管理し、文書化
