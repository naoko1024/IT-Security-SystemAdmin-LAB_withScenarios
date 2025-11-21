# README.md
## Objective / 目的

- Demonstrate the ability to configure user roles, organizational units, and access controls in Google Workspace for secure account management.  
- Google Workspace におけるユーザー役割、組織単位、アクセス権限を設定し、安全なアカウント管理ができることのデモンストレーションをします。
## Scenario / シナリオ

- A new employee joins the Sales department. The IT team must assign the user to the appropriate organizational unit, configure role-based access to Google Drive, Gmail, and internal SaaS apps, and ensure that MFA is enforced.  
- 新入社員が営業部に配属されます。
- 情報システム部は、ユーザーを適切な組織単位に割り当て、Google Drive、Gmail、および社内SaaSアプリへの役割ベースアクセスを設定し、MFAが適用されていることを確認するることになりました。
## Prerequisites to demonstrate this lab / 前提条件

You have :
1. Google Workspace Subscription
2. Super Admin Account

Google Workspaceアカウントがあり、管理者として登録されていることが必要です。

#### How to sign up Google Workspace? / Google Workspaceのアカウントの作り方は？
- Select the plan, start as trial. / 契約プランを選びます。トライアルから始めることも可能です。
- Register with Email address of your choice. / メールアドレスで登録します。
- Private Address is ok. /　プライベートアドレスでも大丈夫です。

#### How to obtain Super Admin Account? /管理者権限でGoogle Workspaceを使用するには？
- Obtain your own domain. /　ドメインを取得する必要があります。
- Register your domain to Google Workpspace. / ドメインを紐づけます

####  How to obtain your domain?
- Select domain provider of your choice. / ドメイン登録サービスを選びます
- The cheapest one starts low as free for a first year./最初の一年は使用料無料のものもあります
  
#### **NOTE: Carefully review your contract, to avoid unexpected charges if you sign up for the platforms for your practice. / 練習目的で登録する場合は、予期していなかった料金がかからないよう、登録の解除は入念に管理してください。**
---

## OU and Users Architecture

```text
Root
├── admin (OU)
│   ├── it (group)  :MikeIT King, JoeIT Moore, RayIT Hill
│   └── security(group) :AnnSec Wood, MattSec Smith,
│
├── departments (OU)
│   ├── sales (group) :LuSales Baker, KateSales Hunter,
│   ├── marketing (group) :BruceMar Fisher, PoelMar Page,
│   ├── research
│   └── hr
│
├── shared (OU)
│   ├── project-x
│   └── general
│
└── suspended (OU)
```

---
## OU Operational Breakdown / OU の意図と運用イメージ

| OU              | Main Purpose / 主な用途                                                     | Key Points / ポイント                                                                                                                                                                               |
| --------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **admin**       | For system administrators and technical staff / システム管理者・技術者用            | Designed for high-privilege accounts. Enforce MFA and apply stricter policies such as outbound Gmail restrictions. / 高権限アカウント用。MFA強制、Gmail外部送信制限など強め設定。                                         |
| **departments** | For regular users in each department / 各部門の通常ユーザー                       | Department-based OUs such as _Sales_ or _Marketing_. Control Drive sharing, Gmail filters, and Shared Drive settings per department. / Sales、Marketingなど部門別OU。DriveやGmailフィルタ、共有ドライブ設定を部門単位で制御。 |
| **shared**      | For common projects or temporary teams / 共通プロジェクトや一時チーム                 | Used for temporary collaborations or cross-department shared documents to keep them isolated from department OUs. / 一時的なコラボや、複数部門で共有するドキュメントを分離。                                                |
| **suspended**   | For storing accounts of resigned or inactive employees / 退職・休職者のアカウント保管 | Simplifies automation of mail forwarding and license removal for offboarded users. / メールメール転送やライセンス削除を自動化しやすくする。                                                                                |
---
## Reference
- You can also check ->
	- Google Workspace Admin Help  https://support.google.com/a/?hl=en#topic=4388346
