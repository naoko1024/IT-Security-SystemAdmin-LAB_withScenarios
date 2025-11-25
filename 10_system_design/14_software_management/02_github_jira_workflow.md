# 02_github_jira_workflow.md
## Objective / 目的

- Provide practical guidance for managing internal IT scripts and configuration files using GitHub, and for tracking IT support tasks via Jira, suitable for IT operations teams.  
- 社内で使用するITスクリプトや設定ファイルの管理をGitHubで行い、JiraでITサポート業務の依頼やタスクを追跡する運用方法を学ぶ。

---

## Scenario / シナリオ

The IT team needs to maintain internal scripts for system monitoring and configuration updates.  
- Changes must be tracked, and requests from other departments should be logged and monitored using Jira.  
- IT staff are expected to make updates to scripts, push changes to GitHub, and link Jira tickets to track task status.
- 社内の監視スクリプトや設定更新スクリプトを管理する必要があり、変更履歴をGitHubで追跡します。  
- 他部署からの依頼はJiraに登録し、進捗状況を管理します。  
- IT担当者はスクリプトを更新したらGitHubに反映し、Jiraチケットと紐付けてタスク状況を確認します。
---
## Prerequisite / 前提条件
- Already connecting GitHub andGit / GitHubとGitの連携済み 

- You can also check ->
	- 【初心者向け】GithubとローカルPCとの連携 by @tariki-code https://qiita.com/tariki-code/items/1c1d720bf389d1c47d85#%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB
	- Connecting to GitHub by GitHub Docs  https://docs.github.com/en/get-started/using-github/connecting-to-github

---
## Step 1: GitHub Repository Setup / GitHub リポジトリ設定

1. Create a repository for IT scripts / IT用スクリプトのリポジトリ作成
```console
mkdir it-scripts
cd it-scripts
git init
```

2. Add a README and initial script / READMEと初期スクリプト追加
```console
echo "# IT Scripts Repository" > README.md
echo "echo 'System check script'" > monitor.git.sh
git add .
git commit -m "Initial commit"

# Check status within root directry
git log
or
git log --oneline
or
git status
```

3. Set repository permissions (Example) / アクセス権の設定例

- Only IT team members can push changes / ITチームのみ push 可能
- General employees have read-only access / 一般社員は閲覧のみ

- You can also check ->【Git入門解説】Gitの基本的な使い方をマスターしよう https://codezine.jp/article/detail/16559

---

## Step 2: Script Update Workflow / スクリプト更新手順

#### 1. Pull the latest changes / 最新状態の取得
```console
git pull origin main
```

#### 2. Update or add a script / スクリプトを更新・追加
```console
# Using text eiditor
nano monitor.sh

# Adding a line using shell
echo "echo 'Logging system check at $(date)'" >> monitor.sh
```

#### 3. Commit and push changes / 変更をコミット＆プッシュ
```console
git add monitor.sh
git commit -m "Add logging to monitor script"
git push origin main
```

4. Periodic review / 定期レビュー

- Review scripts weekly / スクリプトを週次で確認
- Backup critical scripts / 重要スクリプトをバックアップ
    

---

## Step 3: Jira Task Tracking / Jira タスク管理

#### 1. Create a Jira ticket for each task / タスクごとにJiraチケットを作成

- Ticket example: “Update monitor script for logging” / チケット例:  “ログ監視スクリプトを更新する”
- Assign owner and due date / 担当者と期限を設定

#### 2. Link GitHub commits to Jira tickets / GitHubコミットをJiraチケットに紐付け

- Include ticket number in the commit message / コミットメッセージにチケット番号を含める
```console
git add monitor.sh

# Commit locally with ticket number / チケット番号を含めてローカルでコミット
git commit -m "ABC-123: Add logging to monitor script"

git push origin main
```
- Jira detects ticket number and automatically link it within Jira system when you push a commit to Github with ticket number / コミットを GitHub に push すると、Jira がその番号を認識して自動で紐付ける
- Write a commit message following your project policy/ コミットメッセージの書き方は Jira のプロジェクト設定に従う
- You need to register the GitHub repository and configure the integration in Jira, for example through “DVCS Connections.” / Jira の「DVCS コネクション」などで、GitHub リポジトリを登録し連携を設定しておく必要があります

- You can also check ->
	- Ultimate Jira Tutorial for Beginners (2025)  by Stewart Gauld https://www.youtube.com/watch?v=8jWKwiIcWPI
	- Integrate Jira with GitHub https://support.atlassian.com/jira-cloud-administration/docs/integrate-jira-software-with-github/
	- GitHub との連携 https://support.atlassian.com/ja/jira-service-management-cloud/docs/integrate-with-github/

### 3. Monitor task status / チケット進捗を管理

- Use In Progress / Review / Done
- Add comments or attachments as needed / コメントや添付資料を追加

---

## Step 4: Periodic Review / 定期レビュー

- Review scripts weekly / スクリプトの週次確認
- Check Jira ticket status / Jiraチケット状況の確認
- Backup critical scripts / 重要スクリプトのバックアップ
