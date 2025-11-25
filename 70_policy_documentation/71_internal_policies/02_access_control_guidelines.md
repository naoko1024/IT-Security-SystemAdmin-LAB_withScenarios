# 02_access_control_guidelines.md
## Objective／目的
- To establish practical access control and account management rules that align with ISMS A.9, ensuring the protection of information assets, maintaining operational efficiency, and providing clear procedures for IT and operational teams when introducing new SaaS services.
- ISMS A.9 に基づき、新規 SaaS 導入時のアクセス制御およびアカウント管理に関する実務的なルールを定め、情報資産の保護と業務効率の維持、ならびに IT・運用担当者が参照できる具体的な手順を明確化することを目的とする。

## Scenario / シナリオ
- To introducing a new SaaS (e.g., Notion, Google Workspace, AWS), IT team is going to design access control and account management rules.  
- This guideline is prepared with reference to ISMS A.9 (Access Control).
- 新規SaaSを導入する際、アクセス権限設計・アカウント管理ルールを策定することになりました。  
- ISMS A.9（Access Control）を参考に、運用部門向けのガイドラインを作成します。
---
## ISMS A.9（Access Control）Alignment Points／ISMS A.9（アクセス制御）との対応ポイント

| ISMS Clause／ISMS 条項                                                       | Requirements／要求内容                                                                                                          | Corresponding Measures in This Guideline／本ガイドラインでの対応                                                                                        |
| ------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **A.9.1 Access control policy**／A.9.1 アクセス制御方針                            | Establish an access control policy that reflects business and legal requirements.／アクセス制御に関する方針を策定し、ビジネス要件・法的要件を反映させること。    | Defined in **“1. Objective”** as protecting information assets and maintaining operational efficiency.／「1. 目的」で情報資産保護と業務効率維持を目的に方針を明記。      |
| **A.9.2 User access management**／A.9.2 ユーザーアクセス管理                         | Properly manage user registration, deletion, and privilege changes.／ユーザー登録・削除・権限変更などを適切に管理すること。                            | Addressed in **“2. Account Management”**, defining approval flow, deletion deadlines, and periodic reviews.／「2. アカウント管理」で承認制・削除期限・定期棚卸しを定義。 |
| **A.9.3 User responsibilities**／A.9.3 ユーザーの責任                             | Ensure users properly manage their authentication information.／ユーザーが認証情報を適切に管理すること。                                        | Not explicitly described (a “User Responsibilities” section can be added if needed).／明示的には記載なし（必要なら「ユーザー責任」章を追加可能）。                         |
| **A.9.4 System and application access control**／A.9.4 システム・アプリケーションアクセス制御 | Control access to systems and applications, applying technical measures such as MFA.／システム・アプリへのアクセスを制御し、MFAなどの技術的対策を講じること。 | Covered in **“4. MFA”** and **“3. Access Rights Assignment.”**／「4. MFA」「3. 権限付与基準」で対応。                                                      |
| **A.9.2.5 Review of user access rights**／A.9.2.5 ユーザーアクセス権の見直し            | Regularly review user access rights.／定期的にアクセス権限を見直すこと。                                                                     | Included in **“2. Account Management”** and **“5. Logging and Audit.”**／「2. アカウント管理」「5. ログ・監査」で対応。                                          |

--- 
## Example of Access Control Guidelines   / アクセス権限管理ガイドラインサンプル 
#### Tips / 文書作成のコツ
- **For operational-level policy**: define the **actionable guideline** such as timeframes (e.g., within 24 hours, quarterly), procedures, and workflows.
- **For organization-wide or executive-side policy** (formal documents): preferable to **use higher-level, abstract language** that clarifies the scope of protection, the overarching objectives, and the allocation of responsibilities, presenting them as overall principles.
- 運用寄りポリシーには、具体的な数値（例：24時間以内、四半期ごと）、手順、ワークフローなどを含む“実践できる”レベルに落とし込む
- 組織全体／経営層向けの **ポリシー文書（正式版）** では、**抽象的な言葉で**「何を守るか」「何が目的か」「誰が責任者か」を明示し、全体方針として記述する

---

#### 1. Objective ／目的
- To define access control rules that prevent unauthorized access to information assets while maintaining operational efficiency.  
- 情報資産への不正アクセスを防止し、業務効率を維持するためのアクセス制御ルールを定める。


#### 2. Account Management／アカウント管理
- New user registration requires approval from the department head.／新規ユーザー登録は所属部門長の承認を要する。  
- Access rights must be removed within 24 hours upon resignation or transfer.／退職・異動時は24時間以内に権限を削除する。  
- Conduct regular account reviews (at least quarterly).／定期的にアカウント棚卸しを実施する（少なくとも四半期ごと）。


#### 3. Access Rights Assignment／権限付与基準

| Role／ロール | Scope of Privilege／権限範囲 | Conditions／付与条件 |
|---------------|-------------------------------|----------------------|
| Administrator／管理者 | Global settings／audit logs／全体設定・監査ログ | IT Department／情報システム部 |
| General User／一般ユーザー | View and edit within own department／自部門内の閲覧・編集 | All employees／全社員 |
| External Contractor／外部委託 | View only／閲覧のみ | During contract period／契約期間中 |


#### 4. Multi-Factor Authentication (MFA)／多要素認証
- MFA must be enabled for all SaaS and cloud services.／すべてのSaaSおよびクラウドサービスでMFAを必須化する。  
- Hardware tokens are recommended for administrator accounts.／管理者権限にはハードウェアトークンの利用を推奨する。


#### 5. Logging and Audit／ログ・監査
- IAM logs shall be reviewed monthly to detect any abnormal access.／IAMログを月次でレビューし、異常アクセスを検出する。  
- Audit results must be recorded in `audit_docs/internal_audit_report_sample.md`.／監査結果は `audit_docs/internal_audit_report_sample.md` に記録する。

---

## Reference
- You can also check ->
- ISO 27001 – Annex A.9: Access Control by isms online https://www.isms.online/iso-27001/annex-a-2013/annex-a-9-access-control-2013/
- ざっくり学ぶ ISO 27001 A.9 アクセス制御  https://qiita.com/kusunoki576/items/56e804279254b802750e
