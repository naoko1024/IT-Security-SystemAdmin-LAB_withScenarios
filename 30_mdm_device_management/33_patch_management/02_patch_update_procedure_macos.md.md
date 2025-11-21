# patch_update_procedure_macos.md

## Objective / 目的
- To get familiar with the overall flow, ensuring macOS and Apple applications are properly updated for security to maintain device safety and stability.  
- **Note** : macOS has not been installed on my side for this demonstration purposes. Its behavior is being confirmed via videos and other materials.
- 全体の流れとサンプルコードを見ることにより、macOSおよびApple製アプリケーションのセキュリティ更新を適切に実施し、端末の安全性・安定性を維持の方法を学びます。
- **macOSはデモ用にインストールしていません。** 動画等で挙動を確認しています。

---

## Steps / 手順

### 1. Check for Updates (Manual) / 更新確認（手動実行）

1. Apple Menu → System Settings → General → Software Update  
    Apple メニュー → 「システム設定」 → 「一般」 → 「ソフトウェアアップデート」
2. Review available updates via Update Now or More Info
    - **macOS Updates**: Prioritize security patches  
        **macOSアップデート**：セキュリティ修正を含む場合は優先適用
    - **Feature Upgrades** (e.g., Sonoma → Sequoia): Test on a validation device first  
        **機能アップグレード**（例：Sonoma → Sequoia）：検証端末で事前テストを実施
### 2. Apply Updates / 更新適用

1. Click Update Now  
    「今すぐアップデート」をクリック
2. Enter admin password if prompted  
    管理者パスワードを求められた場合は入力
3. Restart system after installation  
    インストール完了後、システム再起動
4. Verify version under About This Mac → More Info  
    「このMacについて」→「詳細情報」でバージョンを確認

### 3. Terminal Verification (For IT Staff) / ターミナル経由の確認（IT担当者用）

- List available updates:
    `softwareupdate -l`
    更新可能項目の一覧を表示
- Install specific update:
    `sudo softwareupdate -i "macOS Ventura 13.6.4 Update"`
    指定アップデートのインストール
- Apply security updates only:
    `sudo softwareupdate -ia --include-config-data`
    セキュリティアップデートのみ適用

### 4. Update Management via Jamf / Intune / Jamf / Intune 経由の更新管理

**Jamf Pro environment / Jamf Pro 利用環境**
- Configure auto-deployment using Patch Management policy  
    Jamf の「Patch Management」ポリシーで自動配信設定
- Update schedule can be set via script + trigger policy  
    更新タイミングは「スクリプト + トリガーポリシー」で指定可能
- Example: Automatically download & restart after 22:00 when work is finished  
    例：業務終了後22:00以降に自動ダウンロード＆再起動
**Intune / MDM environment / Intune / MDM 利用環境**
- Configure delivery in Device Configuration → macOS Software Update Policy  
    「デバイス構成 > macOS ソフトウェア更新ポリシー」で配信設定
- Verify status from Reports in Intune portal  
    適用ステータスは Intune 管理ポータルの「レポート」から確認可能
### 5. Troubleshooting / トラブルシューティング

|Situation / 状況|Action / 対応策|
|---|---|
|Update freezes|Restart by holding power button, then retry / 電源ボタン長押し後再起動、再試行|
|Download stalls|Temporarily disconnect VPN, reconnect / 一時的にVPNを切断し再接続|
|App issues post-update|Reinstall affected app or roll back to previous version / 該当アプリを再インストール、または以前のバージョンに戻す|

---

## Notes / 注意事項

- Backup via Time Machine / OneDrive / iCloud recommended before updating  
更新実施前に Time Machine / OneDrive / iCloud などでバックアップを推奨
- Disconnect external devices (USB, external SSD) during update  
外部デバイス（USB, 外部SSD）は更新中に取り外
- After update, IT or Security team should verify with softwareupdate -l  
更新完了後、セキュリティチームまたはIS担当者が softwareupdate -l で状態確認を実施

## Implementation Notes / 導入メモ

- Security updates and feature upgrades are separate; apply security updates first  
    macOS はセキュリティアップデートと機能アップグレードが分離しているため、セキュリティ更新を優先して適用
- Integration with Jamf or Intune allows automatic restart timing and update reminders  
    Jamf または Intune との連携により、再起動タイミングや更新リマインダーをユーザーに自動通知可能
- Periodically verify Automatic Update is enabled (System Settings → General → Software Update)  
    定期的に「自動アップデート設定」の有効化を確認（システム設定 > 一般 > ソフトウェアアップデート）
