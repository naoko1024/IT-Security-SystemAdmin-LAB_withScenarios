# 02_escalation_flow.md
## Objective / 目的

- To define a clear process for escalating technical or operational issues that cannot be resolved at the helpdesk level, ensuring timely communication, minimal downtime, and accountability.  
- ヘルプデスクレベルで解決できない技術的・運用的な課題を、適切な担当者・チームへ迅速に引き継ぎ、影響範囲を最小化しつつ、責任の所在を明確にするためのプロセスを定義する。

## Scope / 対象範囲

- Internal system and application issues / 社内システム・アプリケーション障害
- Network / account / device-related issues / ネットワーク / アカウント / デバイス関連トラブル
- High urgency or high-impact inquiries / 緊急性・影響度が高い問い合わせ
- Including escalation to external vendors or development teams / 外部ベンダーや開発チームへのエスカレーションも含む
---
## Standard Escalation Levels / 標準エスカレーションレベル

| Level / レベル | Role / 対応者                          | Description / 内容                                                                        | Time to Resolution / 目安対応時間                  |
| ----------- | ----------------------------------- | --------------------------------------------------------------------------------------- | -------------------------------------------- |
| L1          | Helpdesk Operator / ヘルプデスク担当        | Initial intake, hearing user issue, log checking, first response / 初期受付、ヒアリング、ログ確認、一次対応 | Immediate to 30 min / 即時〜30分以内               |
| L2          | System/Network Admin / IT運用チーム      | Configuration changes, permission updates, impact analysis / 設定変更・権限修正・影響範囲調査           | 1–2 hours / 1〜2時間以内                          |
| L3          | Specialist / Vendor / 専門チーム or ベンダー | Critical issues, replication tests, fix requests / 深刻な障害、再現テスト、修正依頼                     | Same day to 24h / 当日中〜24時間以内                 |
| L4          | Manager / Exec / 管理職・経営層            | Major incident reporting, PR or executive decisions / 重大インシデント報告、広報判断                   | (Response by immediate reporting / 直ちに報告・指示) |

---
## Escalation Criteria / エスカレーション判断基準
- Go through the list from top to bottom, and if any item applies, escalate to the next level. / 上から順番に見て、どれか1つでも当てはまれば次のレベルにエスカレーションします。
	- **Technical limits / 技術的限界**: If first-line response cannot resolve the issue / 一次対応で解決見込みがない場合
	- **Impact scope / 影響範囲**: If multiple users or departments are affected / 複数ユーザー / 部署に影響が及ぶ場合
	- **Recurrence / persistence / 再発・継続**: If similar issues occur repeatedly over a period / 同様の障害が一定期間に複数回発生している場合
	- **Insufficient information / 情報不足**: If cause is unknown and additional log analysis or privileges are required / 原因が不明で、追加ログ解析・権限が必要な場合
	- **Security concern / セキュリティ懸念**: Suspicious access or virus detection / 不審なアクセス・ウイルス検出など
### Example / 判断基準の使われ方

1. Receive the ticket (L1) / 一次対応（L1）でチケットを確認
    - For user inquiries, check logs or try first-line support protocol such as restart. / ユーザーからの問い合わせに対してログ確認や簡単な再起動などを試す
2. **Compare against escalation criteria / 上記の判断基準に照らし合わせる**
    -  Impact Scope/ 影響範囲: If the issue affects multiple users or critical systems / 複数ユーザーや重要システムに影響があるか
    - Technical limits / 技術的限界: If L1 cannot resolve the issue / 一次対応で解決できないか
	- Recurrence / persistence / 再発・継続: If the issue occurs repeatedly / 同様の障害が繰り返し発生しているか
3. **Take actions according to escalation destination / エスカレーション先の役割に応じた対応**
	- **L2**: Handles configuration or permission changes, investigates scope of impact / 設定や権限変更、影響範囲の調査を行う
	- **L3**: Handles severe incidents via specialized teams or vendors / 専門チームやベンダーによる深刻障害対応
	- **L4**: Reports as major incident and decides on public communication / 重大インシデントとして報告・広報判断
### References for escalation framework / 他のフレームワーク参考先
- NIST SP 800-61 Rev. 3, Incident Response Recommendations and Considerations for Cybersecurity Risk Management: A CSF 2.0 Community Profile https://csrc.nist.gov/pubs/sp/800/61/r3/final

---
## Overall Escalation to Resolution Sample / エスカレーションフロー具体例

- User / ユーザー  
↓ Report via ticket/chat: "Cannot connect to VPN" / チケット・チャットで報告：「VPN接続できない」  
- Helpdesk (L1) / ヘルプデスク  
↓ If unresolved within 30 min / 30分以内に未解決の場合
- Checks logs → Cause unknown / ログ確認 → 原因不明  
    IT Admin (L2) / IT運用チーム  
    ↓ Requires system-level fix / システムレベルの修正が必要
- Investigates → Authentication server failure identified / 調査 → 認証サーバー障害を特定  
    Vendor / Specialist (L3) / 外部ベンダー・専門チーム  
    ↓ If critical / 重大な場合
- Contact if needed → Issue resolved / 必要に応じて連絡 → 復旧完了  
    Manager / Exec (L4) / 管理職・経営層
- Receive incident report → Update FAQ / 障害報告受領 → FAQ更新
---
## Notes / 運用メモ

- Define an **escalation matrix** in advance (who makes decisions, when, and to what extent)./ 事前に**エスカレーションマトリックス**（誰が、いつ、どこまで判断するか）を定めておく。
- Document procedures and decision criteria so that the entire team can use the same process based on incident classification and priority. / インシデント分類および優先度に応じて、同じ手順・判断基準をチーム全体で使えるようドキュメント化しておく。
- Conduct regular **drills and tabletop exercises** to practice escalation decision-making. / 定期的に**演習・テーブルトップ**で「エスカレーション判断の訓練」を行う。
- Clearly define **communication channels, authority, and responsibility boundaries (RACI)** after escalation. / エスカレーション後の **コミュニケーションチャネル・権限・責任分界点（RACI）** も明確にしておく。
- Consider automation and AI, designing a workflow where **simple alerts are handled automatically** while **high-risk cases are escalated for human judgment**. /自動化・AI導入を検討して、「単純アラートは自動で処理、リスクの高いケースは人間判断へ」という流れを設計。
- Record **when, to whom, and what was requested during escalation　/ 「いつ」「誰に」「何を依頼したか」** のエスカレーションマトリックスを記録する。
- Set SLA (Service Level Agreement) to visualize response time / SLA（Service Level Agreement）を設定し、対応時間を可視化する
- After response, update FAQ or internal knowledge base to reduce similar inquiries / 「同様の問い合わせを減らす」ため、対応後はFAQまたは社内ナレッジベースへ反映。
