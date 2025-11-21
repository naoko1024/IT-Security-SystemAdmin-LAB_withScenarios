# 01_faq_ticket_management.md

## Objective / 目的
- To standardize FAQ management and ticket-based support operations within the helpdesk, ensuring consistent issue tracking, visibility, and continuous improvement.  
- FAQ整備とチケットベースのサポート運用を標準化し、問い合わせ対応の一貫性・可視化・継続的な改善を実現する。

- You can also check -> 43_automation/
## Tutorial for Jira Software
- Jira -> https://www.youtube.com/watch?v=8jWKwiIcWPI by Stewart Gault, Ultimate Jira Tutorial for Beginners (2025)
- Zendesk, Notion and more  / 他にもZendeskやNotionなど

## Scenario / シナリオ
- A new employee was unable to connect to the company VPN on their first day of remote work, and contacts the helpdesk. 
- 新入社員が初めての在宅勤務日にVPN接続ができなかったために業務に着手できず、ヘルプデスクに問い合わせがありました。  

---

## 1. Ticket Submission / チケット登録

|Method / 方法|Description / 概要|Example / 使用例|
|---|---|---|
|Jira Service Portal|Register from internal web portal / 社内ポータルから登録|Department queue, auto assignment / 部署別キュー、自動担当割り当て|
|Google Form Integration|Lightweight setup / 軽量運用向け|Auto record to spreadsheet / スプレッドシート自動記録|
|Email Integration|Register via email / メールから登録|For small environments, CC managers / 小規模環境向け、上長CC共有|

### Recommended Template / 推奨テンプレート
```text
Subject: <Department> Issue Summary / <部門名> 問題内容
Body:  
Occurred at / 発生日時: 
Device / 使用端末:
Details / 詳細:
Reproduction Steps / 再現手順: 
Attached Logs / 添付ログ:
```
- or refer to templates available online / もしくはオンライン資料を参考
---

## 2. Ticket Handling Flow / チケット処理フロー

|Status / ステータス|Action / 担当者アクション|Notes / 補足|
|---|---|---|
|New|Review content, set priority / 内容確認・優先度設定|Recommend auto rule / 自動ルール化推奨|
|In Progress|Investigation and fix / 調査・対応実施|May involve other teams / 他部署連携あり|
|Waiting for User|Await user info / 追加情報待ち|Auto reminder / 自動リマインド設定|
|Resolved|Fix completed, verify / 対応完了・再現確認|Attach QA log / QA記録添付|
|Closed|User confirmed / ユーザー承認済み|Triggers FAQ update / FAQ反映トリガー|

---

## 3. FAQ Knowledge Base Management Example / FAQナレッジ管理(例)
- Knowledge management is the process of **collecting, organizing, sharing, and utilizing** the knowledge and know-how gained through helpdesk operations.　/ ナレッジ管理とは、ヘルプデスク業務で発生した知識やノウハウを**収集、整理、共有、活用するプロセス**です。

| Item / 項目                | Details / 内容                                                             |
| ------------------------ | ------------------------------------------------------------------------ |
| Update Frequency / 更新頻度  | Monthly or every 100 tickets / 月次またはチケット100件毎                            |
| Article Structure / 記事構成 | 1. Symptom / 症状<br>2. Cause / 原因<br>3. Fix / 対処<br>4. Prevention / 再発防止策 |
| Tools / 管理ツール            | Notion, Confluence, Google Sites                                         |
| Access Control / 権限管理    | Edit: IT team / 編集：ITチーム  <br>View: All employees / 閲覧：全社員               |
| Quality Metrics / 品質指標   | Search rate, view rate, repeat inquiry rate / 検索率・閲覧率・再問合率をKPI化          |

---

## 4. Regular Review / 定期レビュー

|Item / 項目|Details / 内容|
|---|---|
|Weekly Review / 週次レビュー|Share main issue trends, discuss improvements / トラブル傾向共有と改善策検討|
|Monthly Review / 月次レビュー|Submit analytical report / チケット分析レポート提出|
|KPI Indicators / KPI指標|Avg. response time, reopen rate, FAQ update rate / 平均対応時間・再開票率・FAQ更新率|

---

## Operational Notes / 運用メモ

- Maintain the one-way cycle: “FAQ Search → Ticket → Resolution → FAQ Update.”  
    　「FAQ検索 → チケット登録 → 解決 → FAQ更新」の**一方向サイクル**を徹底する。
- If 3 or more similar tickets occur, auto-register as FAQ candidate. 
    　同様チケットが3件以上発生した場合は「FAQ候補」として自動登録する。
- Display “Top 3 Latest Updates” on the employee portal to increase usage.  
    　社員ポータルに「最新更新記事トップ3」を自動表示させると利用率が上がる。
- Tag tickets (e.g. `#vpn`, `#printer`, `#login`) for easier trend analysis.  
    　チケットにタグを付けることで（例：`#vpn`、`#printer`、`#login`）、傾向分析が容易になる。
- SLA example: Low priority → respond within 24h / High priority → respond within 1h.  
### SLA  Example / SLA例 

|Priority|Description|Response Time|Resolution Target|
|---|---|---|---|
|P1 (Critical)|System down / 全社業務停止|within 1h|4h以内に暫定対応|
|P2 (High)|部分的影響 / 主要業務影響|within 4h|8h以内|
|P3 (Medium)|一部機能 / 単一ユーザー影響|within 8h|24h以内|
|P4 (Low)|一般問い合わせ / 要望|within 24h|3営業日以内|
