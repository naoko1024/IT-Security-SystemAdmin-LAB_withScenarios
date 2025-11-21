# 03_log_collection_guide.md

## Objective / 目的

- To define consistent methods and tools for collecting system, network, and application logs during troubleshooting or incident response. 
- I didn't prepare macOS for this demonstration. The codes are example for facilitate the future handling.
- トラブルシューティングやインシデント対応時における、システム・ネットワーク・アプリケーションログの収集方法を統一し、迅速かつ再現性のある調査を可能にする。
- macOSは用意しませんでしたので、コードは実際に実行しておらず、参考目的です。
#### Related TryHackMe rooms with demo machine
- Some may require subscriptions.
	- Logs Fundamentals -> https://tryhackme.com/room/logsfundamentals
	- Windows Event Logs -> https://tryhackme.com/room/windowseventlogs
	- Windows Fundamentals 2 ->https://tryhackme.com/room/windowsfundamentals2x0x
	- Core Windows Processes -> https://tryhackme.com/room/btwindowsinternals

---
## Scenario / シナリオ
- An employee reports that Outlook keeps crashing after the latest update. 
- The helpdesk collects Windows Event Viewer logs for the past 24 hours.
- 社員から「最新アップデート後、Outlookが何度もクラッシュする」と報告が入りました。
- ヘルプデスクが過去24時間分のイベントログを収集することになりました。

---
## Scope / 対象範囲
- Windows / macOS client endpoints / Windows / macOS クライアント端末
- Server logs (Active Directory, Web, Mail, etc.) / サーバーログ（Active Directory, Web, Mail 等）
- Network logs (Firewall, VPN, Proxy) / ネットワークログ（Firewall, VPN, Proxy）
- Cloud audit logs (Google Workspace, AWS CloudTrail, etc.) / クラウド監査ログ（Google Workspace, AWS CloudTrail 等）
---
## Log Collections / ログ収集
### 1. Windows Log Collection / Windowsでのログ収集

#### Steps / 手順
1. Launch Event Viewer / イベントビューアーを起動
    - `Win + R` → `eventvwr.msc`
    - Target: `Application` / `System` / `Security` / 対象：`Application` / `System` / `Security`
2. Filter by time period / 特定期間でフィルタ
    - Go to Filter Current Log
    - Logged > extract only the last 24 hours / 「過去24時間」など期間を限定して抽出
    - Event Sources > filter by Application Name / イベントソースで絞り込み: `Outlook` や `MSOffice` でクラッシュ関連イベントを探す
3. Export logs / エクスポート
    - Save Filtered Log File (on right pane) →  Save as `.evtx` / Save Filtered Log File (右ウィンドウ) → .evtx ファイル形式で保存
4. or export logs via PowerShell / パワーシェルで該当のデータを取得
    - Sample code for/サンプルコード:
	    - 過去24時間に記録されたOutlook関連エラーイベントを抽出
		- `Level=2` = Error、`Level=1` = Critical
		- `Where-Object` で過去24時間だけ抽出
		- CSV出力でExcelでも簡単に確認可
		- note:
		- copy the code to notepad first, if you want to prevent the code will be printed backwards. / コピーしたコードが逆向きに貼り付けられてしまう場合はまずノートアプリなどに貼り付けて再度コピペしてください。
```powershell
# powershell

$StartTime = (Get-Date).AddHours(-24)

Get-WinEvent -LogName Application `
  -FilterXPath "*[System[Provider[@Name='Outlook'] and (Level=2 or Level=1)]]" `
  | Where-Object { $_.TimeCreated -ge $StartTime } `
  | Select-Object TimeCreated, Id, LevelDisplayName, Message `
  | Export-Csv "C:\Logs\Outlook_Error_24h.csv" -NoTypeInformation
```

#### Outlook Crash Event Extraction Sample /Outlookクラッシュイベントの抽出サンプル
- **Event ID 1000** indicates an application error (crash)./ アプリケーションエラー（クラッシュ）を示します
- **Faulting application name** shows `OUTLOOK.EXE`, it means Outlook terminated abnormally. / **Faulting application name** に `OUTLOOK.EXE` があるため、Outlookの異常終了
- Look for **Faulting module name** and **Exception code** for causes / 原因を推測できます
	- If `AddinX.dll` is mentioned frequently → likely an Outlook add-in causing crashes / `AddinX.dll` が頻出 → Outlookアドインがクラッシュ原因の可能性
```yaml
# yaml

Log Name:      Application
Source:        Application Error
Event ID:      1000
Level:         Error
Date:          2025-11-07 10:42:33
Task Category: (100)
Faulting application name: OUTLOOK.EXE, version: 16.0.17726.20100, time stamp: 0x66d91a22
Faulting module name: ucrtbase.dll, version: 10.0.19041.546, time stamp: 0x43cbc11d
Exception code: 0xc0000409
Fault offset: 0x000000000007286e
Faulting process id: 0x3e98
Faulting application start time: 0x01da37f9d493d243
Path: C:\Program Files\Microsoft Office\root\Office16\OUTLOOK.EXE
```
#### Resolution Exampe / 解決例
- Recommend disabling the add-in and testing Outlook stability / アドイン無効化→Outlook安定化を確認
#### Notes / 補足
- Security logs require admin privileges / セキュリティログは管理者権限が必要
- Note the timestamp immediately after issue occurs, but follow the protocols if available. / 不具合発生直後の時刻をメモすると、後でクラッシュ原因の予測に役立ちます。記録プロトコルがあればそれに従います。
- Look for recurring error codes or faulting modules / 繰り返し出るエラーコードやFaulting Module Nameを確認すると、原因の推測に有効

---

### 2. macOS Log Collection / macOSでのログ収集

#### Steps / 手順
1. Launch Console.app / Console.app を起動
    - `Applications > Utilities > Console`
2. Set filters / フィルタ設定
    - Filter by process name, time period, message type, etc. / プロセス名、期間、メッセージタイプなどで絞り込み
    - **Filter for Outlook / Outlookのクラッシュ関連ログを抽出**
        - 例: `process == "Microsoft Outlook"`
3. Export logs / ログエクスポート
    - Save via File → Save as `.log` / 「ファイル → 保存」から `.log` 形式で保存
    - CLIの場合:  
        `log show --predicate 'process == "Microsoft Outlook"' --last 24h > ~/Desktop/outlook_crash.log`

- You can also check ->
	- Youtube -> https://www.youtube.com/watch?v=_D6oHm-371A by 13Cubed, New Course! Investigating macOS Endpoints
#### Notes / 補足

- FileVault encryption may encrypt some logs, check permissions / FileVault 有効環境では一部ログが暗号化対象になるため、権限確認が必要
- Use `log show --info --debug` for detailed output / `log show --info --debug` で詳細レベルの出力も可能
- Look for crash reason / termination reason or exception codes to predict cause / クラッシュ理由や例外コードを確認すると、原因予測に役立つ

---

### 3. Network Device Logs Location / ネットワーク機器のログ保存先(例)
- Location may vary depending on your company, so please confirm./ 保管先は会社により異なる可能性がありますので要確認です。

| Device / 機器種別                    | Collection Method / 収集方法                                  | Example Path / 保存先例                  |
| -------------------------------- | --------------------------------------------------------- | ------------------------------------ |
| Firewall (FortiGate / Cisco ASA) | Syslog, export via WebUI / Syslog, WebUIからエクスポート          | `/var/log/firewall/`                 |
| VPN Server                       | Check connection and authentication logs / 接続履歴・認証ログ確認    | `/var/log/vpn/`                      |
| Proxy / Gateway                  | Check URL filtering and transfer errors / URLフィルタ・転送エラー確認 | `/var/log/squid/`, `/var/log/nginx/` |

#### Notes / 補足
- Network devices typically centralize their logs to a Syslog server, allowing you analysis across multiple devices perform easier.
- Recommended to review and define log rotation and retention periods, as logs can be a large volumes of data from centralized devices.
- Refer down the page : Retention & Deletion / 保持と削除
- ネットワーク機器は通常、Syslogサーバーにログを集約され、複数機器間のイベントを一元管理・相関分析しやすくされています。
- ログが集中すると大量データが溜まりやすいので、ローテーションや保持期間を明確に設定することが推奨されます。
- 下の Retention & Deletion / 保持と削除

### 4. Cloud Services Logs Location / クラウド監査ログ保管先(例)
- Location may vary depending on your company, so please confirm./ 保管先は会社により異なる可能性がありますので要確認です。

|Service / サービス|Log Source / ログソース|Method / 手順|
|---|---|---|
|Google Workspace|Admin Console → Reports → Audit log|CSV / BigQuery export possible / CSV / BigQuery出力可能|
|AWS|CloudTrail / CloudWatch Logs|Use AWS CLI: `aws logs get-log-events` / AWS CLIで取得|
|Microsoft 365|Compliance Center → Audit Search|Extract by period, user, or action / 期間・ユーザー・操作別で抽出可能|

---

## Secure Transfer & Storage Example / 安全なログ転送・保管例

- Encrypted transfer: `SCP` / `SFTP` / `Azure Storage SAS URL` recommended / 暗号化転送を推奨
- File naming convention: `{system}_{date}_{incidentID}.log` / ファイル命名規則
- Restrict sharing: Only investigators and managers can access / 共有範囲制限
- Record transfer history: who / when / what / 転送履歴の記録

---

## Retention & Deletion / 保持と削除

- Normal logs: retain 30–90 days / 通常ログ：30〜90日保持
- Incident-related logs: retain at least 180 days post-resolution / インシデント関連ログ：解決後180日以上保管
- Deletion: follow corporate policy and data protection, use `shred` or `sdelete` for secure deletion / 削除時の注意：社内ポリシー・個人情報保護方針に従い、完全削除
- About recommended retention periods, refer to documents from : PCI DSS, NISC, IPA, SANS / データ保持期間について参照できるフレームワーク: PCI DSS, NISC, IPA, SANS, ISMS

---
## Operational Notes / 運用メモ

- Do not reboot or clean up before collection (may lose evidence) / 収集前に「端末再起動」や「クリーンアップ」を行わない
	- Information stored in `C:\Windows\Temp` or `%AppData%\CrashDumps` is often lost after a reboot. / `C:\Windows\Temp` や `%AppData%\CrashDumps` にある情報は再起動後に失われることが多いです。
	- Best practice to instruct users **not to operate the device** until initial evidence is collected, after an incident repot.障害報告後は「初動で収集するまで端末操作を控える」旨をユーザーにも伝える運用が望まれます。
- Record exact timestamp within 5 minutes of issue occurrence if possible / 発生から5分以内の時刻を特定して記録
	- Event logs on Windows or macOS may have time lag by 1–3 minutes from actual time. / WindowsやmacOSのイベントログは時刻が1分〜数分ずれる場合がある。
	- Taking a ±5-minute in record-taking on your side helps align data with Syslog or cloud audit logs later. / そのため、発生時刻を「±5分」でマークしておくと、後でSyslogやクラウド監査ログなど他ソースと突き合わせやすくなります。
	- Recording the **occurrence, discovery, and report times** improves accuracy of analysis./「発生時刻・発見時刻・報告時刻」をすべて記録しておくと分析がより正確になります。
- Apply password when compressing logs (ZIP) / ログ圧縮時はパスワードを付与
	- Logs often contain sensitive information such as usernames, IP addresses, hostnames, and file paths.
	- Protect ZIP archives with encryption (AES-256 recommended) as a standard security practice. / ログにはユーザー名・IP・端末名・ファイルパスなど、**機微情報（sensitive data）**が含まれることが多いため、  ZIP暗号化（AES-256推奨）で保護するのが標準的な対応です。
- For major incidents, immediately hand over to forensic team / 重大なインシデント時はフォレンジックチームへの即時引き継ぎします。
