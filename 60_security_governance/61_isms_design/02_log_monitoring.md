# 02_log_monitoring.md
## Objective / 目的

- Centralize and visualize system/network events to detect anomalies early.
- Set up basic Syslog collection and monitoring.
- Prepare foundation for future SIEM integration.
- For hands-on using Syslog and SIEM softwares, I used pre-configured VMs from TryHackMe. Their walk-through rooms related to this task is below.
- システム・ネットワークイベントを一元管理し、異常を早期検知します。
- 基本的なSyslog収集・監視環境を構築します。
- 将来的なSIEM統合の基礎を整えます。
- Syslogを使用するハンズオンについては、TryHackMeの構築済みVMを使用しました。参考URLは以下です。

## Reference
- You can also check (VM access may require subscriptions.) ->
	- Linux Forensics https://tryhackme.com/room/linuxforensics

---

## Scenario / シナリオ

- An internal network contains multiple Linux servers and network devices that must be regularly monitored.
- Logs from each host are forwarded to a central Syslog server for aggregation.
- IT team periodically reviews logs to detect potential incidents early.
- 社内ネットワークには複数のLinuxサーバーとネットワーク機器が存在し、定期的に監視する必要があります。
- 各ホストのログは中央のSyslogサーバーに送信され、集約されます。
- ITチームは、ログを定期的に確認し、潜在的なインシデントを早期に検知できるようにします。


---

## Lab Steps / 演習ステップ

### 1. Syslog Server Setup / サーバー側の設定

- Install rsyslog / rsyslogのインストール**

```console
sudo apt update
sudo apt install rsyslog -y
```

- Enables the Syslog server (rsyslog) to receive remote logs over UDP and TCP on port 514.  
  / rsyslog サービスを有効化し、UDP/TCP 514番ポートでリモートログの受信を可能にする。
	- `module(load="imudp")` → UDPでsyslogを受信するためのモジュール（imudp）を読み込む
	- `input(type="imudp" port="514")` → UDPのポート514番でログを受信するように設定
```console
nano /etc/rsyslog.conf

# Provides UDP syslog reception module
input(type="imudp" port="514")

# Provides TCP syslog reception module
module
input(type="imtcp" port="514")
```

- Restart service:
```console
sudo systemctl restart rsyslog
sudo systemctl enable rsyslog
```
- Enables Syslog server to receive remote logs over UDP and TCP on port 514. /サーバーはUDP/TCP 514番ポートでリモートログを受信可能に。

- You can also check ->
	- 23.5. ロギングサーバーでの rsyslog の設定 by Red Hat Enterprise Linux https://docs.redhat.com/ja/documentation/red_hat_enterprise_linux/7/html/system_administrators_guide/s1-configuring_rsyslog_on_a_logging_server

---

### 2. Client Configuration / クライアント側の設定

- Edit `/etc/rsyslog.conf` or `/etc/rsyslog.d/50-default.conf`:
```console
*.* @192.168.1.10:514   # UDP
*.* @@192.168.1.10:514  # TCP
```

- Restart rsyslog:
```console
sudo systemctl restart rsyslog
```
- It starts to forward all log messages to the central Syslog server. /クライアントから中央Syslogサーバーへ全ログを転送を開始
- Files already being stored is not forwarded. /すでに保存されている過去ログファイル は自動では転送されません

---

### 3. Log Storage and Review / ログ保管と確認

- **Directory Structure Example / ディレクトリ構成例**

```text
/var/log/central/
├── server1/
│   ├── auth.log
│   ├── syslog
│   └── kern.log
├── firewall/
│   └── fw.log
└── router/
    └── network.log
```

- Add template for host-specific logs:
```console
$template RemoteLogs,"/var/log/central/%HOSTNAME%/%PROGRAMNAME%.log"
*.* ?RemoteLogs
& stop
```
- Organize logs by host using Syslog templates. /Syslogテンプレートを利用し、ホストごとにフォルダ分け。
---

### 4. Log Review and Detection / ログ確認と異常検知

- 4-1. Manual Review / 手動確認
```console
grep "Failed password" /var/log/central/*/auth.log
```

- 4-2. Automated Monitoring / 自動監視例

```console
nano /usr/local/bin/log_watch.sh
```

```bash
#!/bin/bash
LOGDIR="/var/log/central"
if grep -q "Failed password" $LOGDIR/*/auth.log; then
    echo "Alert: Failed SSH login detected at $(date)" | mail -s "Security Alert" admin@example.com
fi
```

- Add to cron for periodic execution:
```txt
*/15 * * * * /usr/local/bin/log_watch.sh
```
- Checks failed SSH logins every 15 minutes and sends alerts./15分毎にSSHログイン失敗を確認し、メール通知。

- You can also check -> What are the common options used with grep command? by LabEx https://labex.io/questions/what-are-the-common-options-used-with-grep-command-271291
---

### 5. Future Integration / 今後の拡張

|Tool|Description|
|---|---|
|Graylog|Web GUIベースのSyslog分析ツール|
|Wazuh|SIEM + EDR機能を統合したOSS|
|Elasticsearch + Kibana|可視化・検索に優れるログ分析基盤|
- Later integration enables advanced correlation and visualization. / 将来的にはSIEM相当の分析・可視化が可能。
  
---

## Verification Points / 確認事項

- Syslog server receives logs from all configured clients.
- Logs are correctly organized by host and program.
- Manual and automated review scripts function as intended.
- Alert emails are delivered timely upon detecting anomalies.
- Directory and file permissions prevent unauthorized access.
- 設定された全クライアントからログが受信されていること。
- ホスト・プログラム別にログが正しく整理されていること。
- 手動および自動レビューのスクリプトが正常に動作すること。
- 異常検知時にメール通知が確実に届くこと。
- ディレクトリ・ファイルの権限が適切に管理されていること。

---

## Expected Outcome / 期待される成果

- Centralized log collection and initial anomaly detection.
- Manual and automated monitoring processes operational.
- Foundation prepared for future SIEM integration and correlation analysis.
- ログの集中管理と初期異常検知体制を確立。
- 手動・自動の監視体制が整備される。
- 将来的なSIEM統合と相関分析の基盤を準備。
