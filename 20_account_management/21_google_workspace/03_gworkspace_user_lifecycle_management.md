# gworkspace_user_lifecycle_management.md

## Objective / 目的

- To understand the overall lifecycle management of Google Workspace accounts, including onboarding, role changes, and offboarding, ensuring access control and security policies are consistently applied.  
- Google Workspaceアカウントのライフサイクル（入社時設定、役職変更、退職時処理）を通して、アクセス権とセキュリティポリシーを維持した管理が一貫して行える流れを理解します。


## Scenario / シナリオ

- The IT team is going to onboard a new employee, adjust access when an employee changes roles, and securely offboard departing employees. Each step must ensure proper access, department settings, and security policies like MFA remain enforced.  
- ITチームは、新入社員のアカウント作成、役職変更時の権限調整、退職者の安全なアカウント削除を行うことになりました。各ステップで、適切なアクセス権、部署設定、MFAなどのセキュリティポリシーが維持されることが求められます。

---

## Steps / 作業手順

### Step 1: Onboarding New Employee / 新入社員のアカウント作成

1. Navigate to **Admin Console → Users → Add User**  
    管理コンソールの「ユーザー」から「ユーザーを追加」
2. Enter user information (name, email, department)  
    ユーザー情報（氏名、メールアドレス、部署）を入力
3. Assign initial OU and roles  
    初期OUとロールを割当
4. Enforce security policies (MFA, password requirements)  
    セキュリティポリシー（MFA、パスワード要件）を適用
5. Notify the user via email with initial credentials and setup instructions  
    初期認証情報と設定手順をメールで通知

- You can also check -> 21_google_workspace/create_user_gworkspace.md

---
### Step 2: Role/Department Change / 役職・部署変更

1. Identify the user whose role or department has changed  
    役職または部署が変更になったユーザーを特定
2. Update OU assignment if department changes  
    部署が変わる場合はOUを更新
3. Adjust roles and access permissions accordingly  
    ロールやアクセス権を変更後の役職に合わせて調整
4. Communicate changes to the user  
    ユーザーに変更内容を通知

---

### Step 3: Offboarding / 退職者アカウント削除

1. Locate the departing user in Admin Console  
    管理コンソールで退職者のユーザーを特定
2. Suspend the account temporarily to prevent access  
    一時的にアカウントを停止しアクセスを防止
3. Transfer ownership of Drive files, emails, and calendars as needed  
    必要に応じてDriveファイル、メール、カレンダーの所有権を移行
4. Delete or archive the account according to company policy  
    会社ポリシーに従いアカウントを削除またはアーカイブ
5. Update auditing and document the offboarding  
    監査ログを更新し、退職処理を記録
