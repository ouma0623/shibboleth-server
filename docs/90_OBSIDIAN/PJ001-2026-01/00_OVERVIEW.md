# 📌 Shibboleth IdPサーバー構築（Phase 1: EC2構築・SSH接続）

## 🧭 概要
- プロジェクト：Shibboleth IdPサーバー構築（Phase 1: EC2構築・SSH接続）
- PJ番号：PJ001-2026-01
- 種別：初期構築
- Plan Issue：#1
- リポジトリ：https://github.com/ouma0623/shibboleth-server
- 実施期間：2026-01-03 〜 2026-01-03

---

## 📂 公式ドキュメント（SSOT）
※ 正は Git / docs。Obsidianは閲覧用・ナレッジレビュー用。

- 📄 Docs Index：docs/00_INDEX.md
- 📄 Requirements：docs/01_REQUIREMENTS/requirements.md
- 📄 Design：
  - 該当なし（このフェーズでは設計書は作成しない）
- 📄 Operations：
  - runbook.md
  - runbook_test.md（該当なし）
- 📄 AI Knowledge：
  - `/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/INDEX.md`

---

## 🧩 実施内容サマリ
> 詳細は Issue / Docs / 各ファイルを参照

- **背景**：
  - Shibboleth認証を導入したIdPサーバーを構築する必要があるが、まずはテスト環境としてEC2インスタンスを構築し、SSH接続可能な状態にする必要がある
  - EC2インスタンスの構築とSSH接続の手順が不明確
  - CursorからEC2内で操作できる環境が整っていない
- **目的**：
  - AWS CLIを使用してVPCとEC2インスタンス（t2.micro）を構築し、SSH接続可能な状態にする
  - CursorのターミナルからSSH接続してEC2内でコマンドを実行できる環境を構築する
  - 次のフェーズでShibboleth IdPサーバーを構築・テストするための基盤を整備する
- **実施内容**：
  - AWS CLIを使用してVPC（CIDRブロック10.0.0.0/16）を作成
  - インターネットゲートウェイを作成し、VPCにアタッチ
  - パブリックサブネット（10.0.1.0/24）を作成
  - ルートテーブルを作成し、インターネットへのルートを追加
  - セキュリティグループ（shibboleth-idp-sg）を作成し、ポート22のインバウンドルールを追加
  - SSHキーペア（shibboleth-idp-key）を作成
  - EC2インスタンス（t2.micro、Amazon Linux 2023）を作成
  - SSH接続の疎通確認を実施
- **影響範囲**：
  - AWSアカウントにVPC、EC2、セキュリティグループなどのリソースが作成される
  - ローカル環境にSSHキーファイルが保存される
- **成果**：
  - VPC、セキュリティグループ、SSHキーペア、EC2インスタンスが正常に作成された
  - SSH接続が成功し、CursorターミナルからEC2インスタンス内でコマンドを実行できることを確認した
  - 次のフェーズでShibboleth IdPサーバーを構築する準備が整った

---

## 🗂 関連 Issue
- Plan：#1 - [Plan] 計画概要：Shibboleth IdPサーバー構築（Phase 1: EC2構築・SSH接続）
  - URL：https://github.com/ouma0623/shibboleth-server/issues/1
- Tasks：
  - #2 - [Task] T-01：VPCとセキュリティグループの作成
    - URL：https://github.com/ouma0623/shibboleth-server/issues/2
  - #3 - [Task] T-02：SSHキーペアの作成または確認
    - URL：https://github.com/ouma0623/shibboleth-server/issues/3
  - #4 - [Task] T-03：EC2インスタンスの作成
    - URL：https://github.com/ouma0623/shibboleth-server/issues/4
  - #5 - [Task] T-04：SSH接続の疎通確認
    - URL：https://github.com/ouma0623/shibboleth-server/issues/5

---

## 📌 レビュー / 注意点（抜粋）
- AWS CLIコマンドを順次実行することで、各リソースの作成プロセスを明確に理解できた
- runbook.mdにリソースIDと作成手順を記録することで、再現可能な状態を維持できた
- Checkフェーズで各リソースの存在と設定を確認することで、確実に動作することを検証できた
- リソース作成時のエラーハンドリングをより詳細に記録する余地があった

---

## 🔗 外部リンク
- GitHub Repo：https://github.com/ouma0623/shibboleth-server
- Pull Requests：なし
- その他：なし

---

## 📝 関連ファイル
- 判断ログ：01_DECISIONS.md
- 学び：02_LESSONS.md
- リンク集：03_LINKS.md
- タスクまとめ：04_TASKS.md
- 作業ファイル：99_work_files/

