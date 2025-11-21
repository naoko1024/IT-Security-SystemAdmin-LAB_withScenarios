# gdpr_comparison.md


## Objective / 目的

- Map internal privacy policies to GDPR requirements to assess compliance and identify gaps.    
- Demonstrate the ability to prepare documentation for audit readiness and international data protection compliance.
- 社内プライバシーポリシーをGDPRの要件に対応付け、遵守状況やギャップを明確化します。
- 監査対応や国際データ保護遵守のための文書作成能力を示します。

## Scenario / シナリオ

- The company processes both domestic (Japan) and EU personal data.
- GDPR applies to EU data, so internal policies must demonstrate alignment.
- Compliance team requests a mapping table showing how existing internal rules meet GDPR obligations and where additional measures are needed.
- 企業は国内（日本）とEUの個人情報を取り扱います。
- EUデータに対してGDPRが適用されるため、社内規程がGDPRに適合していることを示す必要がでてきました。
- それに伴って、コンプライアンスチームから既存規程がGDPRの義務をどこまでカバーしているか、また追加対応が必要な箇所はどこかを示すマッピング表の作成が依頼されました。

---

## Implementation Steps / 実施ステップ

1. Extract key GDPR requirements (e.g., personal data definition, consent, data subject rights, cross-border transfer, breach notification).  
    　GDPRの主要要件（個人情報の定義、同意、本人権利、国際データ転送、漏洩通知など）を抽出。
    
2. Review internal policies and control documents (e.g., Data Protection Policy, Access Control Guidelines).  
    　社内規程や管理文書（データ保護方針、アクセス権限管理ガイドラインなど）を確認。
    
3. Map internal policy items to GDPR requirements, noting alignment or gaps.  
    　社内規程の項目をGDPR要件に対応付け、整合状況やギャップを記録。
    
4. Document findings in a clear table for auditor reference.  
    　監査員向けに分かりやすい表形式で整理・文書化。
    

---

## Sample of GDPR Mapping Table / 社内規程-GDPR対応表（サンプル）

| GDPR Requirement / GDPR要件                 | Internal Policy / 社内規程                                          | Alignment / 整合状況   | Notes / 注記                                      |
| ----------------------------------------- | --------------------------------------------------------------- | ------------------ | ----------------------------------------------- |
| Personal Data Definition / 個人情報の定義        | Data Protection Policy / データ分類方針                                | Partial / 部分的      | GDPRは「識別可能な個人情報」が対象。国内法ベースの分類では補足説明が必要。         |
| Consent / 同意                              | Data Protection Policy / 同意管理手順                                 | Aligned / 適合       | 社内規程で明示的同意を取得するルールあり。EU基準に合わせ、撤回可能性も明示するとより安全。  |
| Data Subject Rights / 本人権利                | Data Protection Policy & Access Control Guidelines / 開示・訂正・削除手順 | Partial / 部分的      | 一部権利（開示・訂正）対応済。GDPRの「消去権」「データポータビリティ」対応は追加必要。   |
| Cross-Border Transfer / 国際データ移転           | Data Protection Policy / 国際送信手順                                 | Partial / 部分的      | 社内規程に暗号化・アクセス制御あり。GDPR準拠の安全措置（契約条項・適切性判断）を追加検討。 |
| Data Breach Notification / データ漏えい通知       | Incident Response Policy / インシデント対応手順                           | Aligned / 適合       | インシデント報告フローあり。EUへの通知期限（72時間）を追加明示するとより完全。       |
| Data Protection Officer / DPO / 個人情報保護責任者 | Organizational Structure / 組織図                                  | Not Mandatory / 任意 | 社内規程上は必須ではないが、大規模EUデータ処理の場合は指名が推奨される。           |

---

## Observations Example / 考察例

- Most internal policies partially align with GDPR but require minor adjustments for full compliance.
- 社内規程は大部分がGDPRに部分的に適合しているが、完全遵守には細かい調整が必要。
    
- Areas such as data portability, erasure, and formal DPO appointment need attention.
- データポータビリティ、消去権、DPO指名などの領域に注意が必要。
    
- Mapping tables like this demonstrate the organization’s readiness for international audit and help prioritize compliance actions.
- このような対応表は、国際監査への準備状況を示し、優先的な対応策の整理に役立つ。

---
## Reference
- You can also check ->
	- GDPRとは？個人情報保護法との違い、日本の立場、会社がすべきことをわかりやすく解説  by ADGs COMPASS  https://sdgs-compass.jp/column/2113
	- Japan APPI vs. EU GDPR by TermsFeed  https://www.termsfeed.com/blog/appi-vs-gdpr/
	- GDPR第37条～第39条のデータ保護監督者(DPO)に関する規定について by Corporate Legal Navigation https://www.corporate-legal.jp/matomes/4581
