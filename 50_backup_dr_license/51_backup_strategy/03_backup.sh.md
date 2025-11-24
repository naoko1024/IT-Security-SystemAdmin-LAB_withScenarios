# 03_backup.sh.md

## Objective / 目的

- This script automates regular backups of both the database and configuration files to ensure quick recovery in case of a system failure.  
- サーバー上のデータベースおよび設定ファイルを定期的にバックアップし、障害発生時に迅速な復旧を可能にします。

---
## Scenario / シナリオ

- The system administrator schedules this script via cron to run nightly. Each backup is saved with a timestamp, and older generations are automatically removed.  
- バックアップ運用担当者は、cronジョブを使用して毎晩自動バックアップを実行することとしています。バックアップは日付付きで保存され、古い世代は自動的に削除されるよう設計します。

---
## Expected Environment / 想定環境
- OS : Linux
- DB : PostgresSQL on Linux

---

## Procedure / 手順
### 1. Create a backup file / バックアップファイルを作成
```console
# example
sudo nano /etc/cron.daily/backup_job
sudo chmod +x /etc/cron.daily/backup_job
```
- Fill out with following scripts / 次からのスクリプトを書いていきます

---

### 2. Define Environmental Valuables / 環境変数設定

- Write a script in a backup file / バックアップファイルにスクリプトを書き込み
```console
#!/bin/bash
# ==========================================
# Backup Script for PostgreSQL and Config Files
# ==========================================

# Backup Destination / バックアップ保存先
BACKUP_DIR="/var/backups/company_app"

# PostgreSQL Settings / PostgreSQL設定
DB_NAME="companydb"
DB_USER="postgres"

# Source Config Directory / 設定ファイル保存元
CONFIG_DIR="/etc/company_app"

# Number of Backup Generations to Keep / 世代保持数（例：7日分）
RETENTION_DAYS=7

# Date Tag / 日付タグ
DATE=$(date +%F)

# Log File / ログ出力
LOG_FILE="/var/log/company_backup.log"
```

---

### 3. Run Backup / バックアップ実行

 - Write a script in a backup file / バックアップファイルにスクリプトを書き込み
```console
echo "[$(date '+%Y-%m-%d %H:%M:%S')] Backup started." >> $LOG_FILE

# Create Backup Directory / ディレクトリ作成
mkdir -p $BACKUP_DIR/$DATE

# Backup PostgreSQL Database / PostgreSQLデータベースのバックアップ
pg_dump -U $DB_USER $DB_NAME > $BACKUP_DIR/$DATE/pg_backup_${DATE}.sql
if [ $? -eq 0 ]; then
  echo "Database backup completed." >> $LOG_FILE
else
  echo "Database backup failed." >> $LOG_FILE
  exit 1
fi

# Backup Configuration Files / 設定ファイルのバックアップ
tar -czf $BACKUP_DIR/$DATE/config_backup_${DATE}.tar.gz -C $CONFIG_DIR .
if [ $? -eq 0 ]; then
  echo "Config file backup completed." >> $LOG_FILE
else
  echo "Config file backup failed." >> $LOG_FILE
  exit 1
fi
```

---

### 4. Remove Old Backups / 古いバックアップの削除

-  Write a script in a backup file / バックアップファイルにスクリプトを書き込み
```console
# Delete backups older than retention days / RETENTION_DAYSより古いバックアップを削除
find $BACKUP_DIR -type d -mtime +$RETENTION_DAYS -exec rm -rf {} \;
echo "Old backups older than $RETENTION_DAYS days have been removed." >> $LOG_FILE
```

---

### 5. Optional External Upload / 外部保存（任意）
 
 - Write a script in a backup file / バックアップファイルにスクリプトを書き込み
```console
# Example: Upload to S3 / 例: S3へアップロードする場合
# aws s3 sync $BACKUP_DIR s3://company-backup/ --delete
# echo "Uploaded backup to S3." >> $LOG_FILE
```

---

### 6. Completion Log / 完了ログ出力

 - Write a script in a backup file / バックアップファイルにスクリプトを書き込み
```console
echo "[$(date '+%Y-%m-%d %H:%M:%S')] Backup completed successfully." >> $LOG_FILE
exit 0
```

---

## Notes / 注意事項

- Register this script using cron, e.g. `0 2 * * * /path/to/backup.sh`.  
	cronでは `0 2 * * * /path/to/backup.sh` のように登録して実行します。

- Use `.pgpass` to avoid interactive password prompts.  
	PostgreSQLの接続には `.pgpass` を利用し、パスワード入力を不要にします。

- For higher redundancy, schedule regular uploads to an external storage such as S3.  
	S3などの外部ストレージに定期転送することで、より高い冗長性を確保できます。
