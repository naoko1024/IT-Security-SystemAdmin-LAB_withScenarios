# antivirus_mdm_policy.md

## Objective / 目的
- To ensure all corporate devices are protected against malware, ransomware, and other endpoint threats through standardized antivirus and endpoint detection systems, with intro level policy and configuration samples.
- note:
- Microsoft Defender and CrowdStrike has not been installed for demonstration purposes. However, their behavior is being confirmed via videos and other materials.
- Operational policies and configuration procedures are build as sample, and intended for introductory-level.
- 全社デバイスを対象に、マルウェア・ランサムウェアなどの脅威から保護するため、標準化されたアンチウイルスおよびエンドポイント検知システムを運用することを目的とする。
- Microsoft Defender と CrowdStrikeはデモ用にインストールしていません。動画等で挙動を確認しています。
- 運用ポリシーや設定手順はサンプルで、イントロレベルを想定しています。

---
## 1. Microsoft Defender for Endpoint (Windows Environment) / Microsoft Defender for Endpoint（Windows環境）

### What is Microsoft Defender?
- Microsoft Defender provides integrated antivirus, firewall, and threat detection features built into Windows.  
- Policy distribution via Intune allows centralized management of all corporate endpoints.  
- Microsoft Defender は Windows に標準搭載されており、ウイルス対策・ファイアウォール・脅威検知機能を統合的に提供する。
- Intune を通じたポリシー配信により、社内全端末の状態を一元管理できる。
- Youtube -> https://www.youtube.com/watch?v=YFPyTPDfZeY by Microsoft Security, Microsoft Defender for Endpoint Overview
### Basic Configuration Example / 基本設定例

- **Real-time protection / リアルタイム保護**: Enabled (default) / 有効（デフォルト設定のまま）
- **Automatic scanning / 自動スキャン**: Set daily at 03:00 / 毎日 03:00 に設定
- **Cloud-delivered protection / クラウド配信保護**: Enabled / 有効
- **Sample submission / サンプル送信**: Automatic (controlled by corporate policy) / 自動送信（社内ポリシーで制御可）
powershell
### Status check command / 状態確認コマンド（PowerShell）
Get-MpComputerStatus
### Manual scan / 手動スキャン実行
Start-MpScan -ScanType QuickScan
- Mp stands for Microsoft Protection / コマンドの中のmpはMicrosoft Protectionの略

**MDM Integration (Intune) / デバイス構成プロファイル**

- Go to (設定パス): Endpoint security > Antivirus > Microsoft Defender 
- Choose below from Central management:
	- Real-time protection / リアルタイム保護 ... 常時ウイルス検出を有効化
	- Cloud-delivered protection / クラウド配信保護 ... Defender がクラウドから最新脅威情報を取得して検出
	- Exclusion folders / 除外フォルダ ... スキャン対象から除外するフォルダを指定

**Reports / レポート**
- Monitor detection status from Defender Security Center or Intune portal  
    Defender セキュリティセンターまたは Intune 管理ポータルから検出状況を監視可能
---
## 2. CrowdStrike Falcon (macOS / Some Windows Environments) / CrowdStrike Falcon（macOS / 一部Windows環境）

### What is CrowdStrike Falcon?
- CrowdStrike Falcon is a cloud-based EDR (Endpoint Detection & Response) platform that prevents unknown threats via AI detection and behavioral analysis.  
- Install a lightweight agent and visualize endpoint threats via management console.  
- CrowdStrike Falcon はクラウドベースの EDR（Endpoint Detection & Response）であり、AI検知・振る舞い分析によって未知の脅威も防止する。
- 軽量エージェントをインストールし、管理コンソール上で全端末の脅威状況を可視化します。
- Youtube -> https://www.youtube.com/watch?v=GjR4p7ajR74 by CrowdStrike, See Falcon Endpoint Security in Action
### Basic Setup Steps / 基本セットアップ手順

1. Download Falcon Sensor from CrowdStrike management portal  
    Falcon Sensor をダウンロード（CrowdStrike 管理ポータルから）
2. Install via command (macOS example) / 以下コマンドでインストール（macOS例）:
`sudo installer -pkg FalconSensorMac.pkg -target /`
3. Register company-specific Customer ID (CID) / 企業固有の「Customer ID」(CID) を登録:
`sudo /Library/CS/falconctl license <your-CID-here>`
4. Check status / ステータス確認:
`sudo /Library/CS/falconctl stats`
- Monitor real-time detection and quarantine in management console  
    管理コンソールでリアルタイム検知・隔離状況を確認
**MDM Integration (Jamf / Intune) / Jamf / Intune 経由の更新管理**

- Falcon Sensor can be auto-deployed via Jamf Pro  
    Jamf Pro経由で Falcon Sensor を自動配信可能
- Example: Policies > Scripts > Install Falcon Agent 
    設定例：Policies > Scripts > Install Falcon Agent
- Intune can also manage deployment centrally via script-based distribution  
    Intune を利用する場合も、スクリプトベースの配布で一元管理が可能
---

## 3. Reporting & Incident Handling / レポート & インシデント対応

|Item / 項目|Action / 対応内容|
|---|---|
|Threat detection / 脅威検出|Auto-quarantine, user notification, report to SOC / 自動隔離・ユーザー通知・SOCへの報告|
|False positives / 誤検知|Security team verifies and adds to exclusion list / セキュリティチームによる確認後、除外リストへ追加|
|Regular review / 定期レビュー|Analyze Defender/CrowdStrike logs monthly to evaluate detection trends / 月次でDefender/CrowdStrikeの検知ログを分析し、検出傾向を評価|

---

## Implementation Notes / 導入メモ

- Running Defender and CrowdStrike simultaneously is **not recommended** to avoid false positives.  
    Defender と CrowdStrike の 二重稼働は推奨されない。重複検知による誤動作を防ぐため、導入環境ごとに明確に分けること。
- CrowdStrike relies on cloud analysis; network instability may delay reporting.  
    CrowdStrike はクラウド分析が中心のため、ネットワークが不安定な環境ではレポート遅延が発生する可能性あり。
- Design with future EDR/XDR integration in mind (e.g., Microsoft Sentinel, CrowdStrike Falcon Insight) for scalability.  
    将来的に EDR/XDR 統合（例：Microsoft Sentinel, CrowdStrike Falcon Insight）への発展を想定して設計しておくと拡張性が高い。
- Check license/subscription annually to prevent protection lapse.  
    ライセンス・サブスクリプションの更新状況を年1回確認し、期限切れによる保護停止を防止する。
