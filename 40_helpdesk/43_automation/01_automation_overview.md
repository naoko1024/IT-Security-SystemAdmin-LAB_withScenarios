# automation_overview.md

## Objectives / 目的
- Automate repetitive manual tasks in daily operations to improve work efficiency and quality.  
- Script routine processes such as ticket handling, account management, and system monitoring to prevent dependency on individuals and ensure consistent operations.  
- 日常業務の中で繰り返し発生する手作業を自動化し、業務効率と作業品質を向上させます。
- チケット対応、アカウント管理、稼働状況の確認などの定常業務をスクリプト化し、属人化を防ぎながら安定した運用を実現します。

---
## Scenario  / シナリオ
- Handle large volumes of daily helpdesk tickets, many of which involving routines such as VPN connection issues, password resets, or initial PC setup. 
- Automate frequent tasks to reduce response time and minimize errors caused by manual processing.  
- ヘルプデスクでは日々大量のチケットを処理しており、その中には「VPN接続不可」「パスワード忘れ」「PC初期設定」など、対応が定型化されたものも多いようです。
- これらの作業をすべて手作業で行うと対応時間の増加やミスにつながるため、よく発生する作業は以下のようにスクリプト化して自動化します。

- Scripts in coming files includes:
	- auto_ticket_reply.py ... Auto-reply to common inquiries / よくある質問への自動返信
	- check_status.py ... Check server and service status / サーバーやサービスの稼働確認
	- csv_bulk_create.ps1 ... Bulk account creation and deactivation / アカウントの一括登録・停止処理
---
## Target Areas / 自動化内容

| Category | Task | Script | Notes |
|-----------|------|--------|-------|
| Ticket Handling | Auto-reply to common inquiries | `auto_ticket_reply.py` | Integrates with Notion or Teams |
| Account Management | Bulk create or disable users | `csv_bulk_create.ps1` | Supports Active Directory integration |
| System Monitoring | Check server and service status | `check_status.py` | Slack notifications available |
| Process Governance | Manage automation policies and documentation | `automation_overview.md` | Shared as team reference |

---

## Implementation Notes Example / 運用ルールの例
- On Windows, use **Task Scheduler**, and on macOS/Linux, use **cron** to run tasks periodically.  
- Execution logs should be stored in a shared directory for team-wide visibility.  
- Notifications are sent via Slack or Teams Webhooks for important events.  
- All scripts are version-controlled in GitHub, and updates are reviewed and merged via Pull Requests.
- Windowsでは **Task Scheduler**、macOS/Linuxでは **cron** を利用して定期実行します。  
- 実行ログは共通ディレクトリに保存し、チーム全員が参照できるようにします。  
- 通知は Slack または Teams の Webhook 機能を使い、重要なイベントは自動通知します。  
- スクリプトは GitHub でバージョン管理し、更新は Pull Request 経由で承認します。 
