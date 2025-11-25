# 03_software_version_control.md

## Objective / 目的

- Provide practical guidance for tracking changes to internal scripts, configuration files, and small in-house applications using version control systems.  
- 社内スクリプト、設定ファイル、簡易アプリケーションの変更履歴をバージョン管理システムで追跡する運用方法を学ぶ。
---
## Scenario / シナリオ

- The IT team maintains automation scripts and configuration files used across departments. Each change must be logged, reviewed, and recoverable in case of errors or misconfigurations.  
- ITチームは各部署で使用される自動化スクリプトや設定ファイルを管理し、変更履歴を記録・レビュー・復元可能します。

---
## Environment / 環境
- This example assumes a Linux environment, because most internal automation scripts (bash, Python, etc.) and configuration files run on Linux servers.
- However, Windows users can use WSL (Windows Subsystem for Linux) or Git for Windows to follow almost the same procedure.
- 今回の例は Linux環境を前提 にしています。理由として情シスで扱う社内スクリプト（bash、Pythonなど）や設定ファイルの多くは Linux サーバー上で動作するためです。
- しかしWindows環境でも WSL や Git for Windows を使えば、ほぼ同じ操作が可能です。
---

## Steps / 手順
### Step 1: Initialize Repository / リポジトリ初期化
- Repository stores scripts, config files, and templates / スクリプト、設定ファイル、テンプレートを格納
```console
mkdir it-scripts
cd it-scripts
git init
echo "# IT Scripts Repository" > README.md
git add .
git commit -m "Initial commit"
```

### Step 2: Branching for Updates / 更新用ブランチ作成

- Branches allow testing and review before merging / ブランチでテスト・レビュー後に統合
```console
git checkout -b feature/update-monitoring
nano monitor.sh
```

### Step 3: Commit Changes / 変更コミット

- Each commit should describe changes clearly / コミットメッセージで変更内容を明確化
```console
git add monitor.sh
git commit -m "Enhance logging functionality"
```



### Step 4: Merge and Review / マージとレビュー

- Review by IT lead or peer / ITリードまたは同僚によるレビュー
- Resolve conflicts if any / 競合があれば解消
```console 
git checkout main
git merge feature/update-monitoring
```



### Step 5: Tagging Versions / バージョンタグ

- Use tags to mark stable releases / 安定版をタグで管理
- Helps rollback if issues occur / 問題発生時にロールバック可能
```console
git tag -a v1.1 -m "Updated monitoring script with logging enhancements"
git push origin main --tags
```

- You can also check ->
	- git tagの使い方まとめ https://qiita.com/growsic/items/ed67e03fda5ab7ef9d08
	- 2.6 Git の基本 - タグ by git https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E5%9F%BA%E6%9C%AC-%E3%82%BF%E3%82%B0
---

### Step 6: Periodic Backup and Audit / 定期バックアップと監査

- Push changes to central Git server regularly / 変更を定期的に中央Gitサーバーにプッシュ
- Audit commit history for compliance / コミット履歴を監査して遵守確認
- Ensure sensitive information is not committed / 機密情報が含まれないよう確認
