# 01_license_management.md

## Objective / 目的

- Centrally manage all software licenses used within the organization to prevent compliance risks caused by overuse or expiration.  
- Contribute to audit readiness and cost optimization by proper license management.
- 組織で利用しているソフトウェアのライセンスを一元管理し、過剰利用や期限切れによるコンプライアンスリスクを防止します。
- 適切なライセンス管理は、監査対応やコスト最適化にもつながります。

---

## Scenario / シナリオ

- As the number of applications and tools used across departments has increased, it has become difficult to track upon which licenses are used where.  
- The IT team decided to build a license registry and periodic review system to organize license issuance, usage status, and renewal schedules.  
- 社内で利用する各種アプリケーションやツールの数が増え、どの部署でどのライセンスを使用しているか把握しづらくなってきました。
- ITチームは、ライセンスの発行・利用状況・更新期限を整理するため、ライセンス台帳の整備と定期的なチェック体制を構築することにしました。

---

## Steps / 手順

### Step 1: Create a License Registry / ライセンス台帳の作成

1. **Prepare Template / テンプレート準備**  
    Copy `/license_tracking.xlsx` and use it as the current registry.  
    `/license_tracking.xlsx` をコピーし、最新の台帳として利用します。
    
2. **Input Basic Information / 基本情報の入力**  
    Fill in the following fields:  
    以下の項目を入力します：
    
	- Software Name / ソフトウェア名
	- Vendor / ベンダー
	- Department / 使用部門
	- Contract Type (Perpetual, Annual, Monthly, etc.) / 契約形態（永続・年間・月額など）
	- Start Date / 契約開始日
	- Expiry Date / 契約終了日
	- Number of Licenses / ライセンス数
	- Licenses in Use / 利用中数
	- Person in Charge / 管理担当者


### Step 2: Define Management Rules / 管理ルールの設定

1. **Update Cycle / 台帳更新サイクル**  
    - Update and review the registry monthly.  
    - ITチームが各部署から最新の使用状況を収集します。  月次で内容を確認・更新します。
    
2. **Expiry Alerts / 期限切れアラート設定**  
    - Set up alerts (e.g., via script or Power Automate) to notify the responsible person 30 days before expiration.  
    - 契約終了日が近い場合、自動で通知されるようにスクリプトやPower Automateを設定します。  
    - 目安：30日前に担当者へメール通知。
    
3. **Enforce Usage Policy / 利用制限の徹底**  
    - To prevent overuse, require approval before installation.  
    - Integrate a software installation request form into your workflow.  
    - ライセンス数以上のインストールを防止するため、導入時に承認プロセスを設けます。  
    - ソフトウェア導入申請書を運用に組み込みます。
    


### Step 3: Regular Review and Reporting / 定期監査と報告

1. **Quarterly Review / 四半期ごとの棚卸し**  
    - Compare actual usage with the registry every quarter and reduce unused licenses before renewal.  
    - 実際の利用数と台帳上の数を突き合わせて確認します。  
    - 不要なライセンスは契約更新前に削減を検討します。
    
2. **Generate Audit Reports / 監査レポートの作成**  
    - Export the records to PDF for internal or external audits, and visualize usage data with charts.  
    - 管理記録をPDF化し、社内監査や外部監査に備えます。  
    - 各ソフトウェアの利用実績をグラフ化して可視化します。
    

---

## Operational Notes / 運用ノート

- Treat `license_tracking.xlsx` as the master registry and avoid simultaneous edits by multiple users.
  / `license_tracking.xlsx` はマスター台帳として扱い、複数人で同時に編集しないようにします。
- Regularly back up the registry to prevent data loss or tampering.  
	/ 定期的にバックアップフォルダにコピーを保存し、改ざんや消失に備えます。
- When using a cloud-based registry, define access rights clearly and ensure edit history tracking. 
	/ クラウドベースの台帳管理を行う場合、アクセス権限を明確にし、編集履歴を追跡可能にします。
- To automate expiry alerts, consider integrating with cron jobs or Microsoft Power Automate.  
	/ 期限切れ通知を自動化するため、cronジョブやMicrosoft Power Automateとの連携を推奨します。
