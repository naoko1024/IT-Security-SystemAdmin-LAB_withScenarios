# 02_restore_procedure.md

## Objective / 目的

- Defines the restoration procedure from backup data to enable the fastest possible recovery in case of system failures or data corruption.
- This document provides an example procedure assuming the use of NAS and AWS S3, demonstrating how to automatically retrieve backup data from servers or containers using the AWS CLI.
- システム障害やデータ破損時に、最短時間で復旧できるよう、バックアップデータからのリストア手順を定義します。
- このファイルでは、NASおよびAWS S3を使用していると想定して、AWS CLIを使用しサーバーやコンテナ上から自動で取得する手順の例を示しています。

---
## Scenario / シナリオ

- This scenario assumes a server failure resulting in the loss of database and configuration files. 
- Recovery will be performed using the latest nightly backup.  
- サーバー障害により、アプリケーションデータベースと設定ファイルが消失したケースを想定します。
- バックアップは定期的に取得されており、前日深夜のデータを使用して復旧を行います。

---

## Steps / 手順

### 1. Verify Environment / 環境の確認

- AWS CLI is installed and configured. / AWS CLIがインストールされていると想定しています。
- Confirm that the new server or container instance is running.  / 新しいサーバーまたはコンテナが起動済みであることを確認します。
- Ensure network configurations (IP, DNS, firewall rules) match the previous environment.  / ネットワーク設定（IP・DNS・FWルール）が旧環境と同等であることを確認します。

---

### 2. Retrieve Backup Data / バックアップデータの取得

- Download the relevant backup data from S3 or the NAS storage.  /  S3またはNAS上のバックアップストレージから対象データを取得します。

#### Linux Host

```console
aws s3 cp s3://company-backup/db/pg_backup_$(date -d "yesterday" +%F).sql /tmp/
aws s3 cp s3://company-backup/config/files_$(date -d "yesterday" +%F).tar.gz /tmp/
```

#### Docker Container

```console
docker exec -it pg-test sh -c "aws s3 cp s3://company-backup/db/pg_backup_$(date -d 'yesterday' +%F).sql /tmp/"
```

- Verify integrity using hash or size comparison. / ダウンロードしたデータの整合性を確認します（ハッシュまたはサイズ比較）。
```console
sha256sum /tmp/*.sql /tmp/*.tar.gz
```

---

### 3. Restore Database / データベースの復元

- Prepare a clean PostgreSQL instance.  /PostgreSQLの空インスタンスを準備します。
- Clear existing data and restore from the backup file.  / 既存データをクリアし、バックアップファイルをリストアします。

#### Linux Host
```console
sudo -u postgres psql -c "DROP DATABASE IF EXISTS companydb;"
sudo -u postgres psql -c "CREATE DATABASE companydb;"
sudo -u postgres psql companydb < /tmp/pg_backup_$(date -d "yesterday" +%F).sql
```

#### Docker Container
```console
docker exec -i pg-test psql -U postgres -c "DROP DATABASE IF EXISTS companydb;"
docker exec -i pg-test psql -U postgres -c "CREATE DATABASE companydb;"
docker exec -i pg-test psql -U postgres companydb < /tmp/pg_backup_$(date -d "yesterday" +%F).sql
```

- Verify that data has been successfully restored.  
- 正常にデータが復元されたことを確認します。
```console
sudo -u postgres psql -d companydb -c "\dt"
```

---

### 4. Restore Configuration Files / 設定ファイルの復旧

- Remove existing configuration files and extract the backup archive.  / 既存設定を削除し、バックアップアーカイブを展開します。
```console
sudo rm -rf /etc/company_app/*
sudo tar -xzvf /tmp/files_$(date -d "yesterday" +%F).tar.gz -C /etc/company_app/
```

- Adjust file permissions and ownerships.  / パーミッションと所有権を修正します。
```console
sudo chown -R root:appgroup /etc/company_app/
sudo chmod -R 640 /etc/company_app/
```

---

### 5. Validation / 動作確認

- Restart the application service.  /アプリケーションを再起動します。
```console
sudo systemctl restart company_app.service
```

- Check connectivity via Web or API.  / WebまたはAPIアクセスで疎通確認を行います。
```console
curl -I http://localhost:8080/health
```

- Verify that logs contain no errors.   /ログにエラーがないことを確認します。
```console
journalctl -u company_app.service | tail -n 20
```

---

### 6. Report Recovery / 復旧報告

- After recovery, report the following:  
	復旧完了後、以下を報告します。

- Time of recovery / 復旧時刻
- Backup generation used / 使用したバックアップ世代
- Result (Success/Failure) / 復旧の成否
- Additional actions / 追加対応項目

---

### Notes / 備考

- Perform regular restore tests to validate the backup. 
	定期的にテストリストアを実施し、バックアップの有効性を検証すること。
- Measure restore duration to define the RTO baseline.
	リストアに要する時間を計測し、RTOの目安とする。
- Use Linux host as primary execution environment; Docker container commands can be applied if needed. 
	Linux ホスト上での手順を基本とし、必要に応じて Docker コンテナ内でも同様のコマンドで実施可能。
