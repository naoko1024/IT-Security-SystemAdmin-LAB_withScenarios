# 02_incident_response_plan.md

## Objective / 目的

- Defines the standard procedure for detecting, reporting, and responding to IT security and operational incidents within the organization.  
- Minimize damage, restore normal service quickly, and prevent recurrence through structured post-incident review.
- 組織内で発生するITセキュリティおよび運用上のインシデント（障害・不正アクセス・情報漏えいなど）に対して、検知・報告・対応を行う標準手順を定義します。  
- 被害の最小化、迅速な復旧、および再発防止を目的とします。

---

## Scenario / シナリオ

- A system administrator detects unusual outbound traffic from a file server.  
- An initial check shows a suspicious process running under an unexpected user.  
- The incident response team must contain the issue, investigate the scope, and restore secure operation.
- システム管理者が、ファイルサーバーからの異常な外向き通信を検知しました。  
- 初期調査により、想定外のユーザーで実行されている不審なプロセスが確認されます。  
- インシデント対応チームは、影響の封じ込め・原因特定・安全な運用復旧を行う必要があります。

---

## Step 1: Detection and Initial Assessment / 検知と初動対応

- Monitor system logs, IDS/IPS alerts, and user reports.  
- When unusual activity is detected, collect initial evidence (timestamps, process IDs, IP logs).  
- Classify the event by severity (Critical / Major / Minor).
- システムログ、IDS/IPSアラート、ユーザー報告などを監視します。  
- 異常が検出された場合は、時刻・プロセスID・通信ログなどの初期証跡を取得します。  
- 事象を重大度（Critical / Major / Minor）で分類します。

---

## Step 2: Containment / 封じ込め

- Immediately isolate affected systems from the network.  
- Disable compromised user accounts or API keys.  
- Prevent the spread of the incident before performing detailed analysis.
- 影響範囲にあるシステムをネットワークから即時切り離します。  
- 不正アクセスが疑われるアカウントやAPIキーを無効化します。  
- 詳細調査の前に、被害拡大を防止します。

---

## Step 3: Investigation and Analysis / 調査と分析

- Review system and application logs for root cause analysis.  
- Identify entry points, affected users, and compromised data.  
- Preserve forensic evidence in secure storage.
- システム・アプリケーションログを解析し、根本原因を特定します。  
- 侵入経路、影響を受けたユーザー、改ざん・漏えいデータを明確化します。  
- 取得した証跡データを改ざん防止のため安全に保管します。

---

## Step 4: Eradication and Recovery / 根絶と復旧

- Remove malicious code, unauthorized files, and modified configurations.  
- Rebuild or restore affected systems from trusted backups.  
- Validate restored services with test accounts before resuming operations.
- 不正なコード、改ざんファイル、設定変更を除去します。  
- 影響を受けたシステムを信頼できるバックアップから再構築・復旧します。  
- 本稼働前にテストアカウントで正常性を確認します。

---

## Step 5: Communication and Reporting / 報告と記録

- Notify the IT manager and information security officer immediately.  
- If personal or confidential data may be affected, escalate to compliance and management.  
- Prepare a concise incident report including timeline, root cause, and actions taken.
- ITマネージャーおよび情報セキュリティ担当者に速やかに報告します。  
- 個人情報や機密情報への影響が疑われる場合は、コンプライアンス部門・経営層へエスカレーションします。  
- 発生時系列・原因・対応内容を含むインシデント報告書を作成します。

---

## Step 6: Post-Incident Review / 事後対応

- Conduct a review meeting within one week of incident resolution.  
- Identify gaps in monitoring, access control, or procedures.  
- Update policies, training materials, and detection rules accordingly.
- インシデント解決後1週間以内にレビュー会議を実施します。  
- 監視・アクセス制御・手順上の改善点を特定します。  
- ポリシー、教育資料、検知ルールなどを更新します。

---

## Incident Severity Classification / インシデント深刻度分類表

|Severity|Description|説明|Example|
|---|---|---|---|
|Critical|Direct impact on service availability or data integrity|サービス停止・データ破損など|システム全停止、情報漏えい|
|Major|Partial service degradation or unauthorized access|一部機能停止、不正アクセス|管理画面侵入|
|Minor|No major operational impact|影響軽微、報告のみ|ログイン失敗の急増|

---

## Summary Table / 対応フロー概要

|Phase / フェーズ|Owner / 主担当|Purpose / 目的|
|---|---|---|
|Detection & Initial Assessment / 検知・初動|Operations / 運用担当|Rapid anomaly detection and impact assessment / 速やかな異常検知と影響評価|
|Containment / 封じ込め|Admin / 管理者|Prevent further damage / 被害拡大防止|
|Investigation & Eradication / 調査・除去|Security / セキュリティ担当|Identify root cause and eliminate threat / 根本原因特定と除去|
|Recovery & Reporting / 復旧・報告|All Teams / 全体チーム|Restore normal operations and share findings / 正常化と共有|
|Post-Incident Improvement / 事後改善|ISMS Manager / ISMS管理者|Implement preventive measures / 再発防止策策定|

---

## Notes / メモ

- All incident logs must be stored for at least 180 days.  
- Evidence should be hashed (SHA256) before archiving.  
- If the incident involves a vendor or external partner, a joint response plan must be documented.
- インシデント関連ログは180日以上保管します。 
- 証跡データはアーカイブ前にSHA256でハッシュ化します。  
- 外部ベンダー・委託先が関与する場合は、共同対応計画を策定します。
  
---

## Reference
- You can also check ->
	- Incident Response Plan (IRP) Basics by CISA chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://www.cisa.gov/sites/default/files/publications/Incident-Response-Plan-Basics_508c.pdf
	- What Is an Incident Response Plan for IT? by CISCO https://www.cisco.com/site/us/en/learn/topics/security/what-is-an-incident-response-plan.html#jump-anchor-3
	- ISO 27001:2022 Annex A 5.26 Response To Information Security Incidents Explained by HighTable https://hightable.io/iso-27001-annex-a-5-26-response-to-information-security-incidents/?gad_source=1&gad_campaignid=21732226205&gbraid=0AAAAACbxnjrMTAUN5di1gQovh9AO_q2ES&gclid=Cj0KCQiA5uDIBhDAARIsAOxj0CHhMZ9PkIVHwpfhHcirheDA9ux--56gvg7PVongW_O_odCv0wNzVIUaAtmXEALw_wcB
	- インシデント対応の基本から実践まで | 効果的な対処と体制構築の方法 by 三井物産セキュアディレクション https://www.mbsd.jp/column/incident/
