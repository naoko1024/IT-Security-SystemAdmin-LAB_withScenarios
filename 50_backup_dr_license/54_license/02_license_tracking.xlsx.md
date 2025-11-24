# 02_license_tracking.xlsx.md

## Objective / 目的

- This spreadsheet serves as a centralized license registry for managing all software used within the organization.  
- It consolidates contract details, usage status, and renewal dates to prevent overuse or expired licenses.
- 社内で利用するソフトウェアライセンスを一元管理する台帳として使用します。  
- 各製品の契約情報・利用状況・更新期限を統合し、過剰利用や期限切れを防止することを目的とします。

---

## Structure Example / 構造例

|Column / 列名|Description / 説明|Example / 例|
|---|---|---|
|**Software Name / ソフトウェア名**|管理対象のソフトウェア名|Microsoft 365 Business Standard|
|**Vendor / ベンダー**|提供元または販売代理店|Microsoft Japan|
|**Department / 部署**|使用部署|General Affairs / 総務部|
|**Contract Type / 契約形態**|契約種別（永続・年間・月額など）|Annual Subscription|
|**License Count / 契約ライセンス数**|契約上の利用可能ライセンス数|100|
|**In Use / 利用中ライセンス数**|現在使用中のライセンス数|87|
|**Start Date / 契約開始日**|契約開始日|2025-01-01|
|**Expiry Date / 契約終了日**|契約満了日または次回更新日|2025-12-31|
|**Renewal Notice / 更新通知日**|通知設定やリマインダー日|2025-12-01|
|**License Key / ライセンスキー**|シリアル番号など（暗号化または非表示管理推奨）|******-*****-*****|
|**Managed By / 管理担当者**|管理責任者の名前または部署|IT Operations|
|**Remarks / 備考**|特記事項（契約更新メモ、価格変更など）|Price to increase in 2026 renewal|

---

## Usage Example / 利用例

1. Each department’s software coordinator reports the latest license usage monthly.  
    各部署のソフトウェア担当者から最新の利用状況を月次で報告してもらいます。
    
2. The IT team updates the “In Use” and “Expiry Date” columns accordingly.  
    ITチームが受け取った情報をもとに、`In Use` 列と `Expiry Date` を更新します。
    
3. Use filters or conditional formatting to highlight licenses nearing expiration.  
    契約期限が近いものをフィルターや条件付き書式で可視化します。
    
4. Coordinate with the purchasing team to renew contracts for expiring licenses.  
    更新対象ソフトウェアは、購買部門と連携して契約更新手続きを行います。
    

---

## Maintenance Tips / 運用上の注意

- Back up the registry at least monthly to a secure folder.  / バックアップは少なくとも月1回、セキュアフォルダに保存します。
    
- Sensitive data such as license keys should be encrypted or stored in a separate, restricted sheet.  / 機密性の高い情報（ライセンスキーなど）は暗号化または別シートで管理します。
    
- If managed in Google Sheets or another cloud tool, regularly review edit logs and access permissions.  / Google Sheetsなどクラウドツールで管理する場合、編集履歴とアクセス権限を定期的に監査します。
    
- During annual audits, include contract file paths and update status for all listed software.  / 年1回の監査レポート作成時に、全ソフトウェアの更新状況と契約書ファイルパスを追記します。

---

## Verification Points / 確認事項

- All software entries are complete and up-to-date in the registry.  / 台帳の全ソフトウェア情報が完全かつ最新であること。
    
- Expiry dates and renewal notices are correctly set.  /契約終了日と更新通知日が正しく設定されていること。
    
- Monthly updates from departments are reflected accurately.  / 部署からの月次報告が正確に反映されていること。
    
- Conditional formatting or filters correctly highlight near-expiry licenses.  /条件付き書式・フィルターが期限切れ間近のライセンスを正しく可視化していること。
    
- Backup copies of the registry exist and are securely stored.  /台帳のバックアップコピーが存在し、安全に保存されていること。
    

---

## Expected Outcome / 期待される成果

- Centralized and clear visibility of all software licenses.  / 全ソフトウェアライセンスの一元可視化。

- Prevention of overuse or expired licenses.  / 過剰利用・期限切れの防止。

- Streamlined audit process and compliance readiness.  /監査対応の効率化とコンプライアンス準備の向上。

- Reduced risk of license-related operational or financial issues.  / ライセンス関連の運用上・コスト上のリスク低減。

- Traceable and secure license management operations.  / 追跡可能で安全なライセンス管理運用の確立。
