# 03_incident_response.md

## Objective / 目的

- Minimize the impact of security incidents such as cyberattacks or system failures and ensure rapid recovery.  
- Follow ISO/IEC 27001 (ISMS) and NIST SP 800-61 principles.  
- サイバー攻撃やシステム障害などのインシデント発生時に被害を最小化し、迅速な復旧を実現する。
- ISO/IEC 27001（ISMS）および NIST SP 800-61 の原則に基づいて策定する。

---

## Scenario / シナリオ

- Multiple unauthorized SSH login attempts are detected via Syslog on a Linux server.  
- The monitoring script sends an automated alert to the security team, initiating the response workflow.  
- Linux サーバーで Syslog により複数回の不正 SSH ログイン試行が検知される。
- 監視スクリプトが自動アラートを通知し、対応ワークフローを開始する。

---

## Incident Response Lifecycle / 対応ライフサイクル

| フェーズ / Phase                        | 説明 / Description                                     |
| ----------------------------------- | ---------------------------------------------------- |
| Preparation / 準備                    | 体制整備、連絡先、手順、ツール準備 / Build team, tools, documentation |
| Detection & Analysis / 検知・分析        | アラート確認、範囲・深刻度の分析 / Validate alerts, determine scope  |
| Containment & Eradication / 封じ込め・根絶 | 被害拡大防止、原因排除 /Stop spread, remove threat              |
| Recovery / 復旧                       | 復旧作業、正常性確認、監視再開 / Restore systems, validate          |
| Post-Incident Activity / 事後対応       | 報告書、改善策、教訓共有 / Reporting, lessons learned            |

- You can also check  ->
	- Understanding the Incident Response Life Cycle by EC-Council https://www.eccouncil.org/cybersecurity-exchange/incident-handling/what-is-incident-response-life-cycle/
---

## Preparation / 準備

### Team Structure / 体制

|役割 / Role|説明 / Responsibility|
|---|---|
|Incident Manager|全体統括、意思決定、外部調整|
|Security Analyst|ログ分析、原因特定、技術調査|
|System Administrator|隔離、修正、復旧操作|
|Communication Lead|報告連絡、関係部署/外部対応|

### Example Tools / 使用ツール例

- Syslog / Rsyslog
- fail2ban
- Nmap / OpenVAS
- Bash / Python automation scripts

---

## Detection & Analysis / 検知・分析

### Alert Example / アラート例
```txt
Jan 15 10:33:14 server1 sshd[2231]: Failed password for invalid user admin from 203.0.113.5 port 55214 ssh2
```

### Automated Alert Script / 自動通知スクリプト

- `/usr/local/bin/incident_alert.sh`
	- `-q` ...  quietly without output / 静かに出力なし
	- `-s` ... subject/件名
```bash
#!/bin/bash
LOGFILE="/var/log/central/server1/auth.log"

if grep -q "Failed password" "$LOGFILE"; then
    MSG="Security Alert: Multiple failed SSH logins detected on server1"
    echo "$MSG" | mail -s "Incident Alert" ir_team@example.com
fi
```

- Cron schedule / 定期実行設定 
```txt
*/10 * * * * /usr/local/bin/incident_alert.sh
```

---

## Containment & Eradication / 封じ込め・根絶

### Temporary Containment / 一時的封じ込め

- Add a rule on iptables to block the source IP / 攻撃元 IP の即時ブロックをiptablesに追加：
	`sudo iptables -A INPUT -s 203.0.113.5 -j DROP`
- Enable fail2ban / fail2ban による自動防御：
	`sudo apt install fail2ban -y sudo systemctl start fail2ban`

### Eradication / 根絶

- Check for malicious accounts / 不正アカウント確認：
	`sudo cat /etc/passwd | grep SuspiciousKeyword`
- Check recently modified files / 改ざんファイル確認：
	`sudo find / -mtime -1 -type f -exec ls -l {} \;`

- Search example of suspicious keywords / 不正アカウントの検索例

| Keyword   | 意図 / Meaning |
|-----------|----------------|
| nologin   | ログイン不可のシステムアカウント / System account with login disabled |
| test      | テスト用アカウントの検出 / Test account (often temporary or leftover) |
| admin     | 管理者権限の可能性があるアカウント / Account with possible administrative privileges |
| backup    | バックアップ用アカウント / Account used for backup operations |
| temp      | 一時的なアカウント（削除漏れの可能性） / Temporary account (may have been left undeleted) |


---

## Recovery / 復旧

- Reboot the affected system / システムを再起動
- Verify no tampering via diff/checksum/AIDE / diff・AIDE による改ざん検証
- Validate system operations / 正常稼働テスト
- Resume monitoring / 監視の再開

---

## Post-Incident Activity / 事後対応

### Report Template / 報告書テンプレート

|項目 / Item|内容 / Description|
|---|---|
|発生日 / Date|2025/11/01|
|検知元 / Detected by|Syslog Alert|
|概要 / Summary|SSH 不正ログイン試行|
|対応内容 / Actions Taken|IP ブロック、fail2ban 導入|
|根本原因 / Root Cause|弱いパスワード使用|
|再発防止策 / Preventive Measures|強力パスワード化、鍵認証の強制|

### Lessons Learned / 教訓

- 監視しきい値調整
- fail2ban ルールの定期見直し
- SSH ポート変更、VPN 経由ログインの検討

---

## Integration with ISMS / ISMS との統合

| 管理策（ISO/IEC 27001 Annex A） | 適合内容 / Alignment |
| -------------------------- | ---------------- |
| A.5.29                     | インシデント報告の実施・迅速化  |
| A.5.30                     | インシデント対応プロセスの実行  |
| A.5.31                     | 教訓共有と改善活動        |
| A.8.16                     | 監査ログの保全と改ざん防止    |
| A.8.23                     | 脆弱性管理と対応強化       |
- You can also check ->
	- by isms online:
	- ISO 27001:2022 Annex A 5.29 Checklist Guide https://www.isms.online/iso-27001/checklist/annex-a-5-29-checklist/
	- ISO 27001:2022 Annex A 5.30 Checklist Guide https://www.isms.online/iso-27001/checklist/annex-a-5-30-checklist/
	- ISO 27001:2022 Annex A 5.31 Checklist Guide https://www.isms.online/iso-27001/checklist/annex-a-5-31-checklist/
	- ISO 27001:2022 Annex A 8.16 Checklist Guide https://www.isms.online/iso-27001/checklist/annex-a-8-16-checklist/
	- ISO 27001:2022 Annex A 8.23 Checklist Guide https://www.isms.online/iso-27001/checklist/annex-a-8-23-checklist/

|NIST SP 800-61 Phase|対応内容 / Mapped Activities|
|---|---|
|Preparation|体制構築・ツール整備|
|Detection & Analysis|Syslog・スクリプト検知|
|Containment|IP 遮断・fail2ban 導入|
|Eradication|改ざん排除|
|Recovery|復旧・監視再開|
|Post-Incident|レポート・改善|

---

## Expected Outcome / 期待される成果

- Early detection and rapid containment   / 早期検知と迅速封じ込め
- Standardized and repeatable response processes   / 標準化された対応プロセス
- Continuous improvement aligned with ISMS   / ISMS と整合した継続的改善
