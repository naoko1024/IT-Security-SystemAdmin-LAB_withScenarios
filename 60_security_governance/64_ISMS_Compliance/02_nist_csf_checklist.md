# 02_nist_csf_checklist.md

## Objective / 目的

- Provide a concise checklist aligned with the NIST CSF to help teams assess current security controls and identify priority improvements.
	NIST CSFに沿った簡潔なチェックリストを提供し、現行のセキュリティ管理策を評価して優先改善点を特定できるようにする。

## Scenario / 想定シナリオ

- Teams periodically review each CSF function, record evidence, and use the results to guide improvements and support ongoing compliance.
	各CSF機能を定期的に確認し、証跡を記録し、結果を改善計画や継続的なコンプライアンス維持に活用する。

---
## Steps / 手順

1. Review each function area (Identify, Protect, Detect, Respond, Recover).  
    各機能領域（特定・防御・検知・対応・復旧）を順に確認する。
    
2. Answer “Yes / Partial / No” based on current operational status.  
    現行運用状況に応じて「Yes / Partial / No」で回答する。
    
3. Record supporting evidence or reference documents.  
    裏付けとなる証跡または参照文書を記録する。
    
4. Use the result to identify priority improvement actions.  
    結果を基に優先的な改善アクションを特定する。
    

---

## Checklist Table

|Function|Category|Control Example|Status|Evidence / Notes|
|---|---|---|---|---|
|Identify|Asset Management (ID.AM)|All devices and systems are inventoried and classified.|Yes|`asset_inventory.xlsx`|
|Identify|Risk Assessment (ID.RA)|Periodic risk assessment is conducted and documented.|Partial|`risk_assessment_report.md`|
|Protect|Access Control (PR.AC)|IAM and MFA are enforced for all administrative accounts.|Yes|`access_control_guidelines.md`|
|Protect|Data Security (PR.DS)|Encryption at rest and in transit are implemented.|Yes|`s3_encryption.md`, `https_setup_nginx.md`|
|Detect|Anomalies & Events (DE.AE)|Syslog and SIEM monitor key systems.|Partial|`linux_log_monitoring.md`|
|Respond|Response Planning (RS.RP)|Incident response plan is documented and tested.|Yes|`incident_response_plan.md`|
|Recover|Recovery Planning (RC.RP)|Backup restoration tests are performed quarterly.|Yes|`backup_restore_test.md`|

---

## Example Script: Automated Status Summary / 自動化スクリプト例

```python
import pandas as pd  # Load checklist CSV

# Read the checklist file
checklist = pd.read_csv("nist_csf_checklist.csv")

# Summarize by status
summary = checklist['Status'].value_counts()

# Calculate compliance ratio
total = len(checklist)
compliance = (summary.get('Yes', 0) / total) * 100

# Print summary
print("=== NIST CSF Compliance Summary ===")
print(summary)
print(f"Compliance Ratio: {compliance:.2f}%")
```

- This script generates a quick summary of CSF control statuses and calculates overall compliance ratio.  
	このスクリプトはCSF管理項目のステータスを集計し、全体の準拠率を算出します

- Output example / 出力例
```text
=== NIST CSF Compliance Summary ===
Yes        4
Partial    2
No         0

Compliance Ratio: 66.67%
```

---

## Expected Results

- Provides an objective overview of security maturity by function area.  
    機能領域ごとにセキュリティ成熟度を客観的に把握できる。
    
- Enables management to identify weak areas and allocate improvement resources.  
    管理層が弱点を特定し、改善リソースを適切に配分できる。
    
- Supports periodic internal audits and continuous compliance.  
    定期的な内部監査および継続的なコンプライアンス維持を支援する。

## Reference
- You can also check ->
	- The NIST Cybersecurity Framework (CSF) 2.0 by IPA chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://www.ipa.go.jp/security/reports/oversea/nist/ug65p90000019cp4-att/begoj9000000d400.pdf
	- 【解説】NIST サイバーセキュリティフレームワークの実践的な使い方 by NRI SECURE https://www.nri-secure.co.jp/blog/nist-cybersecurity-framework
