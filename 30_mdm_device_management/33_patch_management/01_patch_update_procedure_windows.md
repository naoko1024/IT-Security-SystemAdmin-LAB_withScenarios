# patch_update_procedure_windows.md
## Objective
- To properly apply security updates to Windows OS and major applications in order to maintain system security and stability.
- Windows OSおよび主要アプリケーションに対して、セキュリティ更新プログラムを適切に適用し、システムの安全性と安定性を維持することを目的とする。

## Scope
- 対象：社内で運用される Windows 10 / 11 クライアント、Windows Server（検証環境含む）  
- 適用範囲：OS 更新、Microsoft 製品（Office、Defender など）、および一部サードパーティソフトウェア（Adobe Reader, Chrome など）

## 手順

### 1. Check for Updates / 更新確認

1. Start Menu → “Settings” → “Windows Update”  /スタートメニュー → 「設定」 → 「Windows Update」
2. Click “Check for updates”  /「更新プログラムのチェック」をクリック
3. If “Updates are available” is displayed, review the details  /「利用可能な更新プログラムがあります」と表示された場合、内容を確認
	- **Security updates**: apply with priority  / **セキュリティ更新**：優先的に適用
	- **Feature updates**: test in a validation environment before rolling out to production / **機能更新（Feature Update）**：検証環境でテスト後、本番環境へ順次展開
### 2. Apply Updates / 更新適用

1. Click “Install now”  / 今すぐインストール」をクリック
2. After installation completes, restart automatically or manually  / インストール完了後、自動または手動で再起動
3. After reboot, verify success from “View update history”  / 再起動後、「更新履歴の表示」より成功を確認
### 3. Check Logs / ログ確認
- Run the following in PowerShell:  
    PowerShell で以下を実行：
    `Get-WindowsUpdateLog`
    - To read .etl file, covert file to .txt, export events and saves to the C:\ETL\Logs folder:
    `Export-Trace -ETLFile C:\Windows\Logs\WindowsUpdate\WindowsUpdate.20211013.074054.819.1.etl -LogsDirectoryPath C:\ETL\Logs`
    - Go to `C:\ETL\Logs\Logs.20211013.074054.819.1\Registry`
- Attach the log to a shared drive or ticket as needed  
    ログを共有ドライブまたはチケットに添付（必要に応じて）
### 4. Deployment via WSUS / Intune (Managed Devices)

- WSUS administrators configure automatic distribution after approving updates  
	- WSUS 管理者は、更新承認後に自動配信を設定
		- **WSUS (Windows Server Update Services)** is a Microsoft-provided server for managing and distributing Windows updates. It allows centralized management, approval, and deployment of updates (Windows Update) to devices within an organization. / WSUS（Windows Server Update Services）とはMicrosoft が提供する、Windows 向けの更新プログラム配信管理サーバー。企業内の端末に対して、更新プログラム（Windows Update）を 一元管理・承認・配布。
		- note:
		- Windows announced a part of service deprecation for WSUS / 将来的な一部サービス停止を示唆: https://learn.microsoft.com/en-us/windows-server/get-started/removed-deprecated-features-windows-server?tabs=ws25
		- One of reported CVE; [CVE-2025-59287](https://www.cve.org/CVERecord?id=CVE-2025-59287)
		- Youtube -> https://www.youtube.com/watch?v=2jJUnd_nFTU by Tech With Emilio, # What is WSUS (How to PATCH COMPUTERS on a network)
- In Intune environments, verify policy application under Device configuration > Windows Update for Business  
	- Intune 環境では、Device configuration > Windows Update for Business にてポリシー適用を確認
### 5. Troubleshooting / トラブルシューティング

| Situation / 状況                     | Action / 対応策                                                                                    |
| ---------------------------------- | ----------------------------------------------------------------------------------------------- |
| Update stops midway  / 更新が途中で止まる   | `sfc /scannow` to repair system files, run as administrator                                     |
| Specific update errors  / 特定更新がエラー | Note the KB number and manually download from Microsoft Update Catalog                          |
| Issues after update  / 更新後に不具合発生   | Uninstall the last update (`wusa /uninstall /kb:XXXXXXX`) and investigate, run as administrator |

## Notes / 注意事項

- Perform updates **after business hours** or during **maintenance windows** 
    更新作業は 業務終了後 または メンテナンスウィンドウ中 に実施
- Take backups of **important data** before applying updates
    適用前に 重要データのバックアップ を取得
- Escalate unknown updates or unexpected behavior to IT department  
    不明な更新や挙動については、情報システム部門へエスカレーション
## Implementation Memo / 導入メモ

- If automatic Windows Update is disabled, manually run updates periodically  / Windows Update の自動更新を無効化している場合は、定期的に手動実行を推奨
- Be cautious of policy conflicts when using WSUS/Intune together   / WSUS/Intune 併用時はポリシー競合に注意
- Automating update status tracking with scripts (e.g., `Get-HotFix`) improves efficiency  
    更新状況を一覧化するスクリプト（例：Get-HotFix）を運用自動化に組み込むと効率的
