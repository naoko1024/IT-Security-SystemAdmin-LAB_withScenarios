# 01_troubleshooting_examples.md

## Objective
- To summarize standard procedures to respond quickly and reproducibly to routine internal issues (network connection, login, printer, etc.). 
- 日常的に発生する社内トラブル（ネットワーク接続、ログイン、プリンタなど）に対して、迅速かつ再現性のある対応を行うための標準的な手順をまとめます。
## Scenario / シナリオ

- On Monday morning, right after arriving at the office, the support team receives inquiries such as:
- "Cannot connect to the network; Teams and email are unavailable."  
- "Printer shows offline."  
- "Cannot sign in after changing password over the weekend."  
- The helpdesk checks tickets to prioritize and verifies whether the same issues are reported by other users.  
-  Determining that it is not a network-wide outage, the team proceeds with individual troubleshooting.  
- 月曜の朝に出社してすぐに、以下のような問い合わせを受けました。
- 「ネットワークに繋がらず、Teamsもメールも使えません」
- 「プリンタがオフラインになっています」
- 「週末にパスワードを変更した後、サインインできません」
- ヘルプデスク担当は、チケットを確認しながら優先度を判断し、同様の問い合わせが他のユーザーからも来ていないか確認。
- ネットワーク全体の障害ではないと判断し、個別対応に入ります。
## Troubleshooting Examples

### 1. Network connection issues

**Symptom:** Wi-Fi is connected but cannot access the Internet.  
**症状**: Wi-Fiは接続されているが、インターネットにアクセスできない。

**Resolution Steps:**
1. Check IP status via command / コマンドでIP取得状況を確認
    - Windows: `ipconfig /all`  
    - macOS: `ifconfig`
2. Confirm DHCP address allocation (if 169.254.x.x → reconnect needed)
/ DHCPでアドレスが取得できているか確認（169.254.x.xの場合は要再接続）
3. `ping 8.8.8.8` → if responsive, likely DNS issue /応答ありならDNS問題
	- Connection is successful with IP address , but not with a domain name → Name resolution is not working. /IPアドレスでの通信は可能なのに、ドメイン名で通信できない → 名前解決ができていない
4. Manually set DNS (e.g., 8.8.8.8 / 1.1.1.1) / DNSを手動で設定
5. Reconnect Wi-Fi → Reset network settings if necessary /ネットワーク設定リセット（必要時）

**Notes:** If other users report similar issues, consider restarting the AP or checking VLAN settings.  
**補足**: 他ユーザーの同様報告があれば、AP再起動 or VLAN設定確認も検討。

---

### 2. Login issues

**Symptom:** "Authentication error" or "Cannot connect to server" after password change.  
**症状**: パスワード変更後に「認証エラー」または「サーバーに接続できません」と表示される。

**Resolution Steps:**
1. Check AD or Google Workspace password change history
2. ADまたはGoogle Workspaceのパスワード変更履歴を確認。
3. Try signing in via VPN in case cached credentials are used
4. キャッシュ認証の可能性があるため、VPN経由で再サインインを試す。
5. Sign in with local account → reconnect to network and re-login
6. ローカルアカウントで一度サインイン → ネットワークに接続して再ログイン。
7. If MFA (2FA) is enabled, guide user to re-sync device
8. MFA設定（2段階認証）が有効化された場合はデバイス登録の再同期を案内。
    
**Notes:** Especially for remote workers, password changes while disconnected from VPN are common.  
**補足**: 特に社外勤務のユーザーでは、VPN切断状態でパスワードを変更したケースが多い。

--- 
### 3. Printer issues

**Symptom:** Printer shows "Offline" and cannot print.  
**症状**: プリンタが「オフライン」と表示され印刷できない。

**Resolution Steps:**
1. Check printer power and network lights / プリンタ本体の電源・ネットワークランプを確認。
2. On Windows: Devices and Printers → Right-click → Set online / Windowsなら「デバイスとプリンタ」→ 右クリックで「オンラインにする」。
3. Clear print queue and perform test print / 一度キューをクリアし、テスト印刷を実施。
4. For IP-specified printing, check if address changed (DHCP lease expired) / IP指定印刷の場合、アドレス変更がないか（DHCPリース切れ）を確認。
5. For server-based printing, check print server status / 社内サーバー経由印刷の場合、プリントサーバーの状態を確認。

---

### 4. Other common cases

| Type                            | Symptom                                                                   | Resolution Tips                                                                            |
| ------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| Monitor connection / モニター接続     | Screen not displaying / extended display not working /画面が映らない / 拡張表示にならない | Check cables → Reset display settings (`Win+P`) / ケーブル確認 → ディスプレイ設定リセット                    |
| Application issues /アプリ不具合      | Outlook / Teams won't start / 起動しない                                       | Clear cache, re-login, reinstall /キャッシュ削除・再ログイン・再インストール                                    |
| Account lock / アカウントロック         | Locked out after repeated failures /繰り返し失敗でロックアウト                         | Unlock in AD console, guide user to reset /AD管理コンソールで解除、ユーザーへ再設定案内                         |
| VPN connection error / VPN接続エラー | Authentication failure / Cannot establish tunnel /認証失敗 / トンネル確立不可         | Check certificate expiration / password change propagation delay /証明書期限切れ / パスワード変更反映遅延を確認 |


---
## Notes

- Document each case in a ticket system (Jira / Notion) to create a searchable knowledge base.  
- Record reproducible logs (date, error, device info) and analyze trends.  
- Regularly update FAQs and scripts to automate repetitive tasks.  
- 各ケースはチケットシステム（Jira / Notion）でナレッジベース化し、検索可能にする。
- トラブル発生時の再現ログ（日時・エラー内容・端末情報）を記録し、傾向を分析。
- 定期的にFAQ・スクリプト化を進め、繰り返し対応を自動化する。
