# mfa_enforcement_example.md
## Objective / 目的  
- Demonstrate how to enforce MFA for Google Workspace users at an organizational level, including exceptions management, user notification, and audit logging.  
- Google Workspaceユーザーに対するMFAの組織単位での強制適用、例外管理、ユーザー通知、監査ログ取得までを含めた運用例を示しています。

---

## Scenario / シナリオ

- After a few recent phishing attempts, IT team decided to enforce MFA across the organization.
 - The team sends out clear instructions, helps users with setup, and keeps an eye on adoption through audit logs.
- 最近のフィッシング被害を受けて、ITチームは組織全体でMFAを強制適用することにしました。 
-  ITチームはユーザーに丁寧な案内を送り、設定サポートを行い、導入状況を監査ログで確認しています。

## MFA Policy Overview / MFA適用方針概要


| Group / グループ                       | OU            | MFA Method / 多要素認証方法                                                                     | Notes / 備考                                                                      |
| ---------------------------------- | ------------- | ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| IT & Security Teams / IT・セキュリティチーム | `admin`       | **Hardware Security Key / セキュリティキー**                                                     | Strongest protection; required for admin access / 管理者アクセスには必須                   |
| Department Employees / 一般社員        | `departments` | **Authenticator App / 認証アプリ**  <br>(Google Authenticator, Microsoft Authenticator, etc.) | Standard security level / 標準的なセキュリティ対策                                          |
| External Contractors / 外部契約者       | `shared`      | **Conditional Exemption / 条件付き例外**                                                       | Allowed only when technical or contractual limits exist / 技術的・契約上の制約がある場合のみ例外許可 |


## Step 1: Organizational Unit (OU) Setup / 組織単位の準備

1. Access Admin Console / 管理コンソールにアクセス
2. Create or confirm OUs for user groups / ユーザーグループごとのOUを作成または確認

---
## Step 2: MFA Enforcement Policy / MFA強制ポリシー設定

1. Go to Security > Authentication
2. Select OU  / OUを選択
3. Specify authentications  / 認証の設定
	- Enforceme 2-step verification /選択し二段階認証を強制
	- New user enrollment period:  select time frame / 2SVなしでログインできる猶予期間
	- Frequency (No verification required from same device): check on or off /  同じデバイスからのログインは認証をスキップするか
	- Methods / 認証方法を指定
		- Any
		- Any except verification codes via text, phone call  <- (apply for OU: departments)
		- Only security key  <- (apply for OU: admin)
		

- Example of authentications  / 認証の設定 
    - Recommended: Security key (FIDO2), Software token / 推奨：セキュリティキー（FIDO2）、ソフトウェアトークン
    - Practical: Combine security key + Authenticator for stronger protection / 実務ではセキュリティキー＋Authenticatorの組み合わせが強力

---

## Step 3: Exception User Management / 例外ユーザー管理

- Create exception OU or groups with conditional policies for VPN/endpoint restrictions / VPNや端末制限によりMFA回避が必要な場合は、例外OUやグループを設定し条件付きポリシーを作成
- Log all exceptions for audit / 例外ユーザーはログに記録して監査対応

---

## Step 4: User Notification and Training / 導入前の通知・教育

- Notify users via email / メールで通知
- Provide setup guide and pre-test / 設定手順と事前テストを提供
- Follow up individually for management or critical systems staff / 管理職や重要システム担当者は個別フォロー

- You can also check -> Google Workspace Admin Help "Step 1: Notify users of 2-Step Verification deployment" https://support.google.com/a/answer/9176657?hl=en

---

## Step 5: Audit and Monitoring / 監査・モニタリング

- Check adoption status / 導入状況確認
- Review failed login attempts and device changes / ログイン失敗・端末変更ログ確認
- Track exceptions and update policies / 例外管理とポリシー更新

- You can also check -> "User login attempts report" https://support.google.com/a/answer/9039184?hl=en

---

## 管理職昇格時のMFA設定フロー
(Promotion Event) 昇格情報  
  ↓  
(Move to Management OU) 管理職OUに移動  
  ↓  
(Update MFA Requirement) 管理職向けMFA強制（Security Key必須）  
  ↓  
(Notify User & Guide Setup) ユーザー通知＋設定案内  
  ↓  
(Verify Setup) IT部が設定完了を確認  
  ↓  
(Audit Logs) 導入状況確認・記録

## Conditional Exception Sample Flow for VPN and Device Restrictions / VPNアクセスや端末制限など条件付き例外フロー(例)
### New Employee Onboarding/ 新入社員入社フロー
```text
(HR System) 新入社員情報登録
        v
(Create Google Workspace Account) GWSアカウント作成
        v
(Assign to OU) 適切なOUに配置
        v
(Enforce MFA Policy) MFAポリシー適用
        v
(Check Conditional Exception) 条件付き例外の確認
        |-----> (External Contractor OU) 外部契約者 → 条件付きMFA
        |-----> (VPN Access Required) VPNアクセス必要 → MFA一時免除
        |-----> (Restricted Device) 制限端末 → MFA回避条件設定
        v
(Send MFA Setup Email) MFA設定案内メール送付
        v
(User Configures MFA) ユーザーがMFA設定
        v
(Verify Setup) IT部が設定完了を確認
        v
(Audit Logs) 導入状況・例外ユーザーを監査ログで確認
        v
(Regular Review) 定期的に確認・更新
```
### Management Promotion / 管理職昇格時
```text
(Promotion Event) 昇格情報
        v
(Move to Management OU) 管理職OUに移動
        v
(Update MFA Requirement) 管理職向けMFA強制（Security Key必須）
        v
(Check Conditional Exception) 条件付き例外確認
        v-----> (VPN / Restricted Device) 設定例外の適用
        v
(Notify User & Guide Setup) ユーザー通知＋設定案内
        v
(Verify Setup) IT部が設定完了を確認
        v
(Audit Logs) 導入状況・例外状況を監査
```
