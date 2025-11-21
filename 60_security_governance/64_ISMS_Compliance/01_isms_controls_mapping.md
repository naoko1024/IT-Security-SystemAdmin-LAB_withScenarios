# 01_isms_controls_mapping.md

## Objective / 目的

- To align operational security controls with ISO/IEC 27001 Annex A and NIST Cybersecurity Framework (CSF) categories, ensuring consistency, traceability, and audit readiness across all organizational layers.  
	運用上のセキュリティ管理策をISO/IEC 27001付属書AおよびNISTサイバーセキュリティフレームワーク（CSF）のカテゴリに整合させ、全社的な一貫性・追跡性・監査対応性を確保することを目的とする。

---

## Scenario / シナリオ

- The compliance officer is tasked with verifying whether existing technical and procedural controls meet both ISO/IEC 27001 and NIST CSF requirements.  
コンプライアンス担当者は、既存の技術的・手続的管理策がISO/IEC 27001およびNIST CSFの要件を満たしているかを確認することになりました。

- They need to map controls across both frameworks, identify overlapping areas, and highlight gaps that require remediation.  
	両フレームワーク間で管理策をマッピングし、重複部分と改善が必要なギャップを明確にする必要があります。

- This mapping table becomes the foundation for internal audits, external certification, and continuous compliance monitoring.  
	このマッピング表は、内部監査・外部認証・継続的コンプライアンス監視の基盤となります。

---

## Procedure / 手順

1. Extract all relevant control domains from ISO/IEC 27001 Annex A and NIST CSF.  
    ISO/IEC 27001付属書AおよびNIST CSFから関連する管理領域を抽出する。
    
2. Identify equivalent or related controls between the two frameworks.  
    両フレームワーク間で等価または関連する管理策を特定する。
    
3. Map each control to operational documents or implemented configurations (e.g., firewall, IAM, backup).  
    各管理策を運用文書または実装済みの設定（例：ファイアウォール、IAM、バックアップ）にマッピングする。
    
4. Store the mapping in a centralized Markdown or spreadsheet for version tracking and audit reference.  
    マッピング結果をMarkdownまたはスプレッドシート形式で集中管理し、バージョン追跡および監査参照に利用する。
    

---

## Example Control Mapping Table / 管理策マッピング表（例）

| ISO/IEC 27001 Annex A / ISO/IEC 27001 附属書A                                           | NIST CSF Category / NIST CSFカテゴリ | Example Implementation / 実装例                                  | Evidence/Reference / 証跡・参考資料             |
| ------------------------------------------------------------------------------------ | -------------------------------- | ------------------------------------------------------------- | ---------------------------------------- |
| A.9.2 User Access Management / A.9.2 ユーザーアクセス管理                                      | PR.AC-1, PR.AC-4                 | IAM Policy in AWS, AD Group Management / AWSのIAMポリシー、ADグループ管理 | `access_control_guidelines.md`           |
| A.12.4 Logging and Monitoring / A.12.4 ログ管理・監視                                       | DE.CM-1, DE.CM-7                 | Syslog aggregation, SIEM alerts / Syslog集約、SIEMアラート           | `linux_log_monitoring.md`                |
| A.12.6 Technical Vulnerability Management / A.12.6 技術的脆弱性管理                          | ID.RA-1, PR.IP-12                | Regular OpenVAS scans, patch tracking / 定期的なOpenVASスキャン、パッチ追跡 | `vuln_scan_report.md`, `patch_update.md` |
| A.17.1 Information Security Continuity / A.17.1 情報セキュリティ継続                           | RC.IM-1, RC.CO-2                 | DR plan and periodic restoration tests / DR計画と定期的な復旧テスト       | `disaster_recovery_plan.md`              |
| A.18.1 Compliance with Legal and Contractual Requirements / A.18.1 法的および契約上の要求事項への準拠 | ID.GV-3, ID.RA-6                 | ISMS documentation, GDPR mapping / ISMS文書、GDPRマッピング           | `gdpr_comparison.md`,                    |


---

## Example of Automated Mapping Validation / 自動マッピングコード例

```python
# python
# Load the control mapping CSV
import pandas as pd

# Read the mapping file
mapping = pd.read_csv("isms_nist_mapping.csv")

# Identify unmapped controls
unmapped = mapping[mapping['NIST_CSF_Category'].isnull()]

# Export unmapped list for review
unmapped.to_csv("unmapped_controls_review.csv", index=False)

# Print summary
print(f"{len(unmapped)} controls require mapping review.")
```

- This script identifies any ISO controls not yet mapped to a NIST CSF category and outputs them for manual review.  
	このスクリプトは、NIST CSFカテゴリーにまだ対応付けられていないISO管理策を特定し、手動レビュー用に出力する。


- Output Example / 出力例
```console
3 controls require mapping review. Generated: unmapped_controls_review.csv
```

---

## Expected Results

- Unified cross-framework compliance visibility.  
    フレームワーク横断的なコンプライアンス可視化が実現できる。
    
- Reduced duplication and better alignment between technical and organizational controls.  
    重複の削減および技術的・組織的管理策の整合性向上を図れる。
    
- Streamlined audit preparation through traceable evidence mapping.  
    証跡の追跡可能なマッピングにより監査準備を効率化できる。
