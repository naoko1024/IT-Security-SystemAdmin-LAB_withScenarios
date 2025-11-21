# create_user_gworkspace.md
## Objective / 目的
- To create new user accounts in Google Workspace following standard procedures.  
	標準手順に従ってGoogle Workspaceで新規ユーザーアカウントを作成する。

## Prerequisite / 前提条件
- Google Workspace subscription with super admin account. / Google Workspaceへの登録と管理者権限
	- Please refer to README.md / README.mdを参照ください。
## Scenario / シナリオ

- A new employee is joining the company. IT staff need to create a Google Workspace account, assign the user to the appropriate department OU, and provide initial login credentials and access rights.  
	新入社員が入社することになりました。
	情報システム担当者は、Google Workspaceでまずはアカウントを作成します。

---
## Steps / 作業手順

### Step 1: Log in to Admin Console / 管理コンソールにログイン

- Open the Google Admin console at [admin.google.com](https://admin.google.com) and log in with an administrator account.  
- Google Adminコンソール（[admin.google.com](https://admin.google.com)）に管理者アカウントでログイン。
### Step 2: Navigate to Users / ユーザー管理画面へ

- Click on "Users" to view all current accounts. 
    「ユーザー」をクリックして、現在のアカウント一覧を表示。
### Step 3: Create New User / 新規ユーザー作成
- Click **Add new user**.
- Fill in:
    - First Name / 名
    - Last Name / 姓
    - Primary Email / メールアドレス
- Initial password is generated automatically (Users can change at first login).  
    初期パスワードは自動で設定されています（ユーザーは初回ログイン時に変更できます）
- Sent log-in instructions to users at their reachable email addresses. / ユーザー任意のメールアドレスを入力すると、ログイン手順がメ記載されたメールが送ることができます。
### Step 4: Assign Organizational Unit (OU) / 組織単位への割当

- After creating the user, assign them to the appropriate OU according to department or role.　/ ユーザー作成後、部署や役職に応じて適切なOUに割り当てます。 
- Click the name of user > Organizational Unit > select the target OU.  
    ユーザーをクリック → 「組織単位」 → 対象OUを選択。


- You can also check -> 21_google_workspace/access_control_gworkspace.md, Step 3: Apply OU-specific Policies / OUごとのポリシー適用

### Step 5: Verify User Settings / 設定確認

- Confirm that the user appears in the correct OU.
- Check initial roles and access rights (e.g., Gmail, Drive, Shared Drives).  
- ユーザーが正しいOUに表示されていることを確認。  
- 初期権限（Gmail、Drive、共有ドライブなど）も確認。
