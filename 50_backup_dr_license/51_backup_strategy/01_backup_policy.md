# 01_backup_policy.md

## Objective / 目的

- Defines example of the company’s backup policy for internal systems, servers, and databases.
- The goal is to ensure rapid recovery after data loss or system failure by clarifying backup targets, retention rules, storage destinations, and verification methods.
- 社内システム・サーバー・データベースのバックアップ方針のサンプルを定義します。
- データ損失や障害発生時に迅速に復旧できるよう、バックアップ対象・世代管理・保存先・検証方法を明確にします。

## Scenario / シナリオ

- File server usage has been increasing, and older backup generations are no longer traceable.
- The IT team adopts a “three-generation backup” model and stores data in both cloud storage and an on-premises NAS for redundancy.
- 社内ファイルサーバーの容量増加に伴い、過去のバックアップ世代が不明確になっていることが判明しました。
- ITチームは「3世代管理方式」を採用し、バックアップデータをクラウドとローカルNASの両方に保存する方針に変更します。
    

---

## Backup Policy Overview / バックアップ方針概要

|項目|内容|Description|
|---|---|---|
|**対象**|ファイルサーバー、PostgreSQL、構成ファイル|File servers, PostgreSQL, configuration files|
|**頻度**|毎日1回（深夜2時）|Daily at 2:00 AM|
|**保存期間**|最新3世代＋週次スナップショット4週|3 latest daily versions + 4 weekly snapshots|
|**保存先**|NAS / AWS S3|NAS / AWS S3|
|**暗号化**|AES256（転送時・保存時）|AES256 during transfer and storage|
|**検証方法**|月次ランダム復元テスト|Monthly random restore test|
|**通知**|成功／失敗をメール通知|Email notification on success/failure|
## Scenario Environment / 想定環境
- OS : Linux
- DB : PostgresSQL on Linux or on Docker/Linux

---

## Step 1: Define Backup Targets / バックアップ対象を定義する

- List up all directories and databases requiring backup and assign schedules based on importance and update frequency. / バックアップ対象ディレクトリ・データベースを一覧化し、重要度と更新頻度に応じてスケジュールを決定します。
```console
# Internal File Server
/srv/fileshare/  

# OS Configuration 
/etc/  

# PostgreSQL DB on backup
/var/lib/postgresql/15/main/  

# Confirm folders
ls -ld /srv/fileshare/ /etc /var/lib/postgresql/15/main
```

- When including Docker services
```console
# Docker Volume stored on Linux
/var/lib/docker/volumes/myapp_data/_data/

# Confirm the folder
docker volume ls
```

- Create a directory that cron.daily stores the backup data / バックアップ用ディレクトリを作成
```console
#  Example
sudo mkdir -p /backup/daily
sudo chown root:root /backup/daily
sudo chmod 700 /backup/daily
```

-  Confirm current backup settings
```console
# Linux
ls -l /etc/cron.daily/
```

- You can also check ->
	- 【Linux】cron.dailyに登録したタスクが毎日2回実行されてしまう https://qiita.com/wsigma21/items/bcbf24b5f089063ae2f6
---
## Step 2: Configure Backup Rotation / 世代管理の設定

- Configure automated deletion of outdated backups and retain the latest three generations. / 古いバックアップを自動削除し、最新3世代のみ保持するよう設定します。

- Create a file and place  a cleanup command / ファイルを作り、その中に削除コマンドを配置
```console
# Example
sudo touch /etc/cron.daily/backup_cleanup
sudo chmod +x /etc/cron.daily/backup_cleanup
```

```console
find /backup/daily/ -type f -mtime +3 -delete
```
or
```console
find /backup/daily/ -type f -mtime +3 -exec rm -f {} \;
```
- `-exec` → 見つかったファイルに対して指定したコマンドを実行する。
- `rm -f` → 強制削除（存在しないファイルでもエラーを出さない）。
- `{}` → `find` が見つけたファイル名をここに展開する。
- `\;` → `-exec` の終了を示す。バックスラッシュはシェルに解釈されないようにするため。

- You can also check -->
	- cron 式と rate 式を使用して Amazon EventBridge でルールをスケジュールする https://docs.aws.amazon.com/ja_jp/eventbridge/latest/userguide/eb-scheduled-rule-pattern.html
---

## Step 3: Define Storage and Security / 保存先とセキュリティの定義

- Store backups in both NAS and AWS S3, ensuring encrypted transfer using `aws s3 cp`. / バックアップはNASとAWS S3の2系統に保存し、`aws s3 cp` により暗号化通信で転送します。
```console
# Example / コマンド例

# 1) Sync to NAS (best-effort incremental copy)
rsync -av --delete /backup/daily/ backupuser@backup01:/backup/daily/

# 2) Upload to S3 (recursive upload, with server-side AES256 encryption)
aws s3 sync /backup/daily/ s3://company-backup-bucket/daily/ --sse AES256

# 3) Verify S3 upload (list objects)
aws s3 ls s3://company-backup-bucket/daily/ --recursive
```

- When back up the settings of Linux itself / Linux 自身をバックアップする例
```console
#!/bin/bash
DATE=$(date +%Y%m%d)

# Daily backup directory
TARGET=/backup/daily/$DATE
mkdir -p $TARGET

# Filesystem backup
rsync -av /srv/fileshare/ $TARGET/fileshare/
rsync -av /etc/ $TARGET/etc/

# PostgreSQL backup
pg_dumpall > $TARGET/postgresql.sql
```

- Backup inside Docker / Docker 上でのバックアップ例
```console
# Archive dokcer volume using tar / Docker volume を tar アーカイブ化（一般的）

docker run --rm \
  -v myapp_data:/data \
  -v /backup/daily:/backup \
  alpine sh -c "tar czf /backup/myapp_data_$(date +%Y%m%d).tar.gz /data"
  
# Back up from PosgreSWL container / PostgreSQL コンテナから直接バックアップ
# （例: container 名が postgres の場合）

docker exec postgres pg_dumpall -U postgres > /backup/daily/postgres_$(date +%Y%m%d).sql

# Send to AWS S3 with AWS CLI/ AWS CLIを使ったAWS S3 への送信 
aws s3 sync /backup/daily s3://company-backup-bucket/daily/ --sse AES256

# Backup within Docker
docker run --rm \
  -v myapp_data:/data \
  -v /backup/daily:/backup \
  alpine sh -c "tar czf /backup/myapp_data_$(date +%Y%m%d).tar.gz /data"

```
- Linux 上に AWS CLI がインストールされていれば、コンソールからS3に送れます。
- You can also check ->
	- Installing or updating to the latest version of the AWS CLI https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
	- How to Backup a Linux PC to a Synology NAS using Rsync! https://www.youtube.com/watch?app=desktop&v=wSGaG3AQV7o

## Step 4: Setup Verification and Alert / 検証と通知設定

- Validate log files after backup completion, send email reports, and retry automatically on failure. / バックアップ完了後はログを検証し、メール報告を送信します。失敗時は自動再試行し、それでも失敗した場合はSlack通知を実施します。

- Add lines to output a log at the end of backup file from Step 3, as needed / 必要ならログを出力するスクリプトを、Step3のログファイルに追加
```console
# Store log to back.up file
>> /var/log/backup.log 2>&1
```

```console
if grep -q "error" /var/log/backup.log; then
  /usr/bin/mail -s "Backup Failed" it-team@company.local < /var/log/backup.log
else
  /usr/bin/mail -s "Backup Completed" it-team@company.local < /var/log/backup.log
fi
```

- When Docker, and using a script works for both docker and Linux. / DockerとLinux 共通で動くlog チェックスクリプト
```console
LOG=/var/log/backup.log

if grep -qi "error" $LOG; then
  echo "Backup Failed: $(date)" | mail -s "Backup Failed" it-team@company.local
else
  echo "Backup Completed: $(date)" | mail -s "Backup Completed" it-team@company.local
fi

```

---

## Step 5: Review and Documentation / 定期見直しとドキュメント化

- Review this policy annually or whenever major system changes occur and keep documentation updated. / バックアップポリシーは年1回、またはシステム変更時に見直し、ドキュメントを最新化して全メンバーが参照できるようにします。
    

---

## Expected Outcome / 期待される成果

- Establish a secure, automated backup mechanism for critical data.
- Improve reliability through clear retention rules and periodic restore testing.
- Reduce operational risk and ensure long-term data continuity.
- 重要データを安全かつ自動でバックアップする仕組みを確立できる。
- 世代管理の明確化とテスト運用により信頼性を向上できる。
- 運用リスクを低減し、事業継続性を長期的に確保できる。
