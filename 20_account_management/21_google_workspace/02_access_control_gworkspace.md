
# 02_access_control_gworkspace.md 
## Overview / 概要

- Configure initial access permissions and security settings to ensure secure onboarding.
- 安全な初期運用を確保するため、初期アクセス権限とセキュリティ設定を構成します。

## Scenario / シナリオ

 - After creating a new user in Google Workspace, the IT staff must assign the appropriate roles, apply department-specific settings, and enforce security measures like MFA.  
- 新規ユーザー作成後、情報システム担当者は適切な役割を割り当て、部署ごとの設定を適用し、MFAなどのセキュリティ対策を実施します。
---

## OU and Users Architecture
- From README.md in this directory
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

## Steps / 作業手順
### Step 1: Create OU / OU作成
- Admin Console > Organizational Unit in left pane > Create Organizational Unit > configure below /以下を設定します:
	- Name of Organizational Unit
	- Description
	- Parent organizational unit
  - > Create / Createで作成

- You can also check -> 
	- Google Workplace Organizational Units (OUs) according to Parks and Rec https://offthewire.crosswire.io/google-workplace-organizational-units-ous-according-to-parks-and-rec-fed59f08273a
### Step 2: Change or Verify OU of Users / ユーザーのOU確認

- Go to Directory in left pane > Users. / DirectoryからUsersをクリック
- Click User Name > look for "CHANGE ORGANIZATIONAL UNIT" in left pane > select the applicapable one.
 / ユーザーネームをクリックし、ORGANIZATIONAL UNITの変更を選択、OUを選びます
---
### Step 3: Assign Roles / 役割割当

- Go to Directory in left pane > Users > click name of user >   → **Admin roles and privileges**. > scroll down the main page and find "Admin roles and privileges" > assign roles
	-  Choose pre-defined roles from a list /予め用意あれているロールリストから選ぶこともできます
	- or
	- Create Custom Roles / もしくはカスタムもできます

- You can also check / 詳細はこちらも-> Google Workspace Admin Help
	- "Assign specific admin roles" https://support.google.com/a/answer/9807615?sjid=10606643028397934573-NC
	- "Create, edit, and delete custom admin roles" https://support.google.com/a/answer/2406043?sjid=10606643028397934573-NC

---
### Step 3: Apply OU-specific Policies / OUごとのポリシー適用

- A Google Workplace Organizational Unit (OU) is a grouping of users under the **sharing a common purpose**.
- You can manage administrative information and tasks within Google tools and are used to create departments, teams, projects, and other organizations with a common purpose.
- Google WorkspaceのOUは、**「どの設定・ポリシーを、どのグループのユーザーに適用するか」**  を分けるための階層です。
- 「人事部」「営業部」など**部署単位**、「外部委託」「アルバイト」など**契約形態単位**、「テストユーザー」「旧システム用」など**運用目的単位**など、方針に分けて設計することもできます。


- Check **Apps & Services → Settings for OU**.
- Enforce restrictions such as Drive sharing, external access, and email routing based on department.  
    「アプリとサービス」→「OU設定」から、部署単位での制限（Drive共有、外部アクセス、メールルーティング）を適用。

- You can also check ->
    - Google Workspace Organizational Units (OUs) according to Parks and Rec https://offthewire.crosswire.io/google-workplace-organizational-units-ous-according-to-parks-and-rec-fed59f08273a

---
### Step 4: Enforce MFA / 多要素認証の適用

- Go to Security in left pane > Authentication > 2-step verification.
- Require MFA for all users in the OU. / OU内のユーザー全員にMFAと適用します
    - Select OU in left pane / OUを選択
    - Check all components you want to turn on. / 適応したい機能を選択
    - Override / 適応ボタンを押す

- You can also check -> Google Workspace Admin Help - "Deploy 2-Step Verification" https://support.google.com/a/answer/9176657?hl=en
	- Step 1: Notify users of 2-Step Verification deployment
	- Step 2: Allow users to turn on 2-Step Verification
	- Step 3: Tell your users to enroll in 2-Step Verification
	- Step 4: Track users' enrollment
	- Step 5: Enforce 2-Step Verification (Optional)

---
### Step 5: Verify Access Control / アクセス権確認

- Test a user login and ensure:
    - Correct permissions are applied
    - MFA is enforced
    - OU-specific restrictions are working  
- ユーザーログインをテストし、以下を確認：
	- 適切な権限が適用されている
	- MFAが強制されている
	- OU単位の制限が有効になっている

- You can also check -> Google Workspace Admin Help
	- "View role assignments & privileges" https://support.google.com/a/answer/7519580?hl=en#:~:text=As%20an%20administrator%20for%20your,super%20administrator%20for%20this%20task.
	- "Control which apps access Google Workspace data" https://support.google.com/a/answer/7281227?hl=en#:~:text=Sign%20in%20with%20an%20administrator,%2C%20internal%2C%20or%20Google%20owned.
	- "Control which third-party & internal apps access Google Workspace data" https://support.google.com/a/answer/13152743?hl=en
