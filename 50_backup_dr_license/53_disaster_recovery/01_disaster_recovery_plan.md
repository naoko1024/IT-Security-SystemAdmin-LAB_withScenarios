# 01_disaster_recovery_plan.md

## Objective / 目的

- Defines a sample of the basic Disaster Recovery (DR) plan and Business Continuity framework for internal IT systems, including databases, file servers, and critical network infrastructure.  
- Minimize downtime, data loss, and operational disruption in the event of system failure, natural disaster, or human error.
- 社内情報システム（データベース、ファイルサーバー、ネットワーク機器など）に対する災害復旧（DR）および事業継続計画（BCP）の基本設計のサンプルを示します。  
- システム障害・自然災害・人的ミスなどの発生時に、業務停止・データ損失・復旧遅延を最小限に抑えることを目的とします。

---

## Scenario / シナリオ

- The company operates internal services hosted on-premise and partially on cloud (e.g., AWS).  
- Daily backups are already automated and verified.  
- One day, a hardware failure or regional power outage occurs, resulting in total loss of the primary database server.  
- The following plan describes how to restore system availability and data integrity within defined recovery objectives.
- 社内サービスはオンプレミスおよび一部クラウド（例：AWS）上で稼働しています。  
- データベースはすでに自動バックアップおよび検証が行われています。  
- ある日、ハードウェア障害や地域停電によりメインDBサーバーが完全に停止・消失した場合を想定します。  
- 以下では、定義された復旧目標内で、可用性とデータ整合性を回復する方法を示します。

---

## Recovery Objectives / 復旧目標

| Metric / 指標                             | Definition /定義                                                      | Goal / 目標値                                               |
| --------------------------------------- | ------------------------------------------------------------------- | -------------------------------------------------------- |
| RTO (Recovery Time Objective / 復旧時間目標)  | Maximum allowable downtime to restore services /業務システムを復旧させるまでの許容時間 | Within 4 hours / 4時間以内                                   |
| RPO (Recovery Point Objective / 復旧地点目標) | Maximum acceptable data loss / データ損失を許容できる最大時間幅                     | Within 24 hours (up to last backup) / 24時間以内（前日バックアップまで） |

---

## Step 1: Establish Recovery Environment / 復旧環境を整備

- Prepare a standby server or cloud instance with compatible OS and PostgreSQL version.  
- Verify that access credentials, SSH keys, and firewall rules are available.  
- Ensure network connectivity and DNS settings can be temporarily rerouted to the recovery host.
- 同一バージョンのOSおよびPostgreSQLを搭載したスタンバイサーバーまたはクラウドインスタンスを用意します。  
- アクセス資格情報、SSH鍵、ファイアウォール設定を確認します。  
- ネットワーク経路およびDNSの切替が一時的に行えることを確認します。

---

## Step 2: Restore Data from Backup / バックアップからデータを復旧

- Retrieve the latest `.sql.gz` backup file from `/var/backups/postgres/`.  
	 `/var/backups/postgres/` にある最新の `.sql.gz` バックアップを取得します。  
- Transfer it to the recovery host securely (e.g., via `scp` or `rsync`).  
	`scp` や `rsync` などで安全に復旧サーバーへ転送します。  
- Decompress and restore using `psql`:  
	解凍後、以下のコマンドでデータを復元します：
```console
# example
gunzip pg_backup_2025-11-01_00-00-00.sql.gz
psql -U postgres -f pg_backup_2025-11-01_00-00-00.sql
```

---

## Step 3: Validate System Integrity / システムの整合性を確認

- Check database tables, user accounts, and stored data.  
- Run key service functions to ensure applications connect properly.  
- Compare record counts and checksum hashes with pre-disaster reports if available.
- データベースのテーブル構造、ユーザーアカウント、データ内容を確認します。  
- アプリケーションが正常に接続し、主要機能が動作するかテストします。  
- 可能であれば、災害前のデータ件数・ハッシュ値と照合し、整合性を確認します。

---

## Step 4: Switch Service and Notify Stakeholders

- Update DNS or load balancer to direct traffic to the recovery system.  
- Notify users and management that the recovery environment is active.  
- Keep the recovery state running until the original system is repaired and validated.
- DNSまたはロードバランサーを更新し、トラフィックを復旧環境へ切り替えます。  
- 復旧環境が稼働中である旨をユーザーおよび管理部門に周知します。  
- 元のシステムが修復・検証されるまで、復旧環境を運用し続けます。

---

## Step 5: Post-Recovery Review

- Document the recovery duration, encountered issues, and resolutions.  
- Update backup frequency or RTO/RPO if gaps were identified.  
- Review incident response communication and escalation flow.
- 復旧に要した時間、発生した問題とその対処内容を記録します。  
- 課題があればバックアップ頻度やRTO/RPOの見直しを行います。  
- インシデント対応時の報告・連絡・エスカレーション体制を再確認します。

---

## Summary of Disaster Recovery Design

| Item / 項目                   | Details / 内容                                                  |
| --------------------------- | ------------------------------------------------------------- |
| Backup /バックアップ              | Automated (`pg_backup_cron.sh`) / 自動化済み (`pg_backup_cron.sh`) |
| Recovery Environment /復旧環境  | Cloud or standby server /クラウドまたはスタンバイサーバー                     |
| Recovery Targets /復旧目標      | RTO 4h / RPO 24h                                              |
| Integrity Validation /整合性検証 | Data comparison & functional test /データ比較・動作テスト                |
| Improvement Cycle /改善サイクル   | Post-recovery review updates / 事後レビューによる更新                    |

