# 03_data_protection_policy.md
## Objective / 目的
- Ensure the confidentiality, integrity, and availability of all company data, to prevent unauthorized access or disclosure, and to comply with applicable legal and regulatory requirements.
- Follow the CIA triad (confidentiality, integrity, and availability), aligning with the fundamental principles of ISMS.
- 社内データの機密性・完全性・可用性を確保し、不正アクセスや情報漏洩を防止するとともに、関連する法令および規制を遵守します。
- 機密性・完全性・可用性（CIA triad） を明示し、ISMSの基本原則に対応しています。


## Scenario / シナリオ
- The company plans to use cloud storage (e.g., Google Drive, AWS S3) for both internal documents and client data.  
- The IT team is going to define classification rules and to handle procedures to prevent accidental disclosure or unauthorized access.  
- 社内文書および顧客データをクラウドストレージ（例：Google Drive, AWS S3）で管理することになりました。  
- IT部門は、誤送信や不正アクセスを防ぐためのデータ分類ルールと取扱手順を策定することになりました。

---

## ISMSとの対応ポイント / ISMS Alignment Points

|ポリシー項目 / Policy Item|ISMS参照例 / Annex|説明 / Description|
|---|---|---|
|**Data Classification / データ分類**|**A.8.2 Classification of Information**|Classifying information assets according to confidentiality levels (e.g., Confidential / Internal / Public). / 情報資産を機密性に応じて分類すること。Confidential / Internal / Public の分類がこれに対応。|
|**Access Control / アクセス制御**|**A.9 Access Control**|Responsibilities of data owners and IT, access permissions, and approval rules align with A.9 controls. / データオーナーやIT部門の責任、アクセス権限設定、承認ルールはA.9の制御群に合致。|
|**Encryption & Transfer / 暗号化・転送**|**A.10 Cryptography**|Rules for using encryption to protect data correspond to cryptographic controls under A.10. / データの保護に暗号化を使用するルールは、暗号化管理のA.10と関連。|
|**Retention & Disposal / 廃棄・ライフサイクル**|**A.8.3 Media Handling / A.12.3 Backup**|Managing data lifecycle and secure disposal relate to media handling and operational controls. / データライフサイクル管理や安全な廃棄は、媒体管理や運用管理に対応。|
|**Logging & Audit / ログ・監査**|**A.12.4 Logging and Monitoring**|Logging access to sensitive information and periodic reviews align with A.12.4. / 機密情報アクセスのログ取得や定期レビューはA.12.4に対応。|
|**Training & Awareness / 教育・啓発**|**A.7.2 Awareness, Education and Training**|Improving security awareness through staff training corresponds to A.7.2. / 従業員教育を通じて情報セキュリティ意識を向上させる点はA.7.2に対応。|
## Implementation Steps / 実施ステップ:
1. Identify all types of data handled and assign classification levels.  
　取り扱うすべてのデータを特定し、分類レベルを割り当てる。  
2. Map storage locations and ensure encryption where required.  
　保存場所を確認し、必要に応じて暗号化を実施。  
3. Define sharing rules and approval workflows for confidential/internal data.  
　機密・社内データの共有ルールと承認ワークフローを定義。  
4. Implement logging and periodic audits for sensitive data access.  
　機密データアクセスのログ取得と定期監査を実施。  
5. Train employees on proper handling and reporting procedures.  
　従業員に対して取扱手順と報告手順の教育を実施。  

---

## Example of Data Protection Policy / データ保護ポリシーサンプル

### 1. Purpose / 目的
- This policy establishes the principles and rules for handling, storing, and disposing of company data to ensure confidentiality, integrity, and availability, and to comply with legal and regulatory requirements.  
- 本方針は、社内データの取扱い・保管・廃棄に関する基本的な原則およびルールを定め、機密性・完全性・可用性の確保および法令・規制遵守を目的とする。



### 2. Scope / 適用範囲
- This policy applies to all data created, stored, processed, or transmitted by the company, including digital and physical formats, across all employees and contractors.  
- 本方針は、当社が作成、保管、処理、または送信するすべてのデータ（デジタル・紙媒体を問わず）に適用され、全従業員および委託先が対象となる。


### 3. Responsibility / 責任
- Data owners are responsible for classifying their data according to its sensitivity level.  
　データオーナーは、データの機密性レベルに応じた分類に責任を負う。  
- IT department is responsible for implementing appropriate access controls and protection mechanisms.  
　IT部門は、適切なアクセス制御および保護策の実施に責任を負う。  
- Individual users must handle data in compliance with this policy and report any incidents promptly.  
　各ユーザーは本方針に従いデータを扱い、インシデントが発生した場合には速やかに報告する。


### 4. Data Classification / データ分類

| Classification / 区分 | Description / 内容 | Storage / 保存場所 | Sharing / 共有ルール |
|-----------------------|------------------|-----------------|-------------------|
| Confidential / 機密 | Personal info, financial records / 個人情報、財務情報 | Encrypted cloud storage / 暗号化クラウドストレージ | Department head approval required / 部長承認必須 |
| Internal / 社内秘 | Contracts, client lists / 契約書、顧客リスト | Limited-access drives / 限定アクセスドライブ | Internal team only / 部内限定 |
| Public / 一般 | Marketing materials / 広報資料 | Public wiki or website / 社内Wiki・Webサイト | No restrictions / 制限なし |


### 5. Data Handling / データ取扱い
- Confidential data must be encrypted at rest and in transit (AES-256 recommended).  
　機密データは保管時および送信時に暗号化すること（AES-256推奨）。  
- Data transfers outside the organization require secure channels and, if necessary, password-protected files.  
　社外送信には安全な通信チャネルを使用し、必要に応じてパスワード保護を施す。  
- Retention and deletion policies must be applied according to the data lifecycle.  
　データの保管期間・廃棄ルールはデータライフサイクルに従って適用する。

### 6. Logging and Audit / ログ管理と監査
- Access to confidential and internal data must be logged and periodically reviewed.  
　機密・社内データへのアクセスはログを取り、定期的にレビューする。  
- Incidents involving data breaches must be reported immediately to the CISO and handled according to incident response procedures.  
　データ漏洩インシデントが発生した場合、速やかにCISOに報告し、インシデント対応手順に従う。

### 7. Review and Revision / 改定
- This policy shall be reviewed annually or whenever significant regulatory or organizational changes occur.  
- 本方針は、法規制や組織に重大な変更が生じた際、または年1回を目安に見直しを行う。

