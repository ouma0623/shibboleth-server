# Changes（変更履歴 / 仕様変更ログ）

> このドキュメントは、本PJの変更履歴（仕様・挙動・運用影響）を記録する。  
> 実装中に仕様変更が発生した場合は `/plan` に差し戻し、requirements/design更新と合わせて本ログを追記する。  
> 「なぜ変えたか」「何が変わったか」「どこに影響するか」「どう確認したか」が追跡できることを目的とする。

---

## 記録ルール（運用）
- 追記タイミング：
  - 仕様変更確定時（Plan）
  - 最終整理（Action）
- 対象：
  - 仕様（要件/受け入れ基準/スコープ）の変更
  - 挙動が変わる実装変更
  - 運用に影響する変更（runbook更新が必要なもの）
- 例外：
  - 純粋なリファクタ（挙動不変）や微修正は原則不要  
    ※ただし将来調査に効く場合は記録してよい

---

## 変更履歴

### 2026-01-03（v1 / 変更ID: CHG-0001）
- 種別：初期実装
- 変更理由（Why）：
  - Phase 1: EC2構築・SSH接続の初期実装
- 変更概要（What）：
  - AWS CLIを使用してVPC、セキュリティグループ、SSHキーペア、EC2インスタンスを作成
  - SSH接続の疎通確認を実施
- 影響範囲（Impact）：
  - docs：requirements.md（要件定義）、runbook.md（運用手順）
  - コード：なし
  - データ：なし
  - インフラ：AWS VPC、EC2、セキュリティグループ、インターネットゲートウェイ、ルートテーブル、サブネットの作成
- 関連リンク：
  - Plan Issue：#1
  - Task Issue：#2, #3, #4, #5
  - PR：なし
  - Obsidian：`/mnt/d/work/obsidian/cursor/10_PROJECTS/PJ001-2026-01/`
- 検証（How to Verify）：
  - 観点：各リソースの存在確認、SSH接続の疎通確認
  - 手順：AWS CLIコマンドでリソースを確認、SSH接続を実行
  - 期待結果：全てのリソースが正常に作成され、SSH接続が成功する
- ロールバック方針（必要なら）：
  - リソース削除手順は `docs/03_OPERATIONS/runbook.md` に記載
- 備考：
  - テスト環境のため、セキュリティグループの送信元を0.0.0.0/0に設定

---

### YYYY-MM-DD（vX / 変更ID: CHG-0002）
- 種別：仕様変更 / 挙動変更 / 運用変更 / セキュリティ / パフォーマンス / リファクタ
- 変更理由（Why）：
- 変更概要（What）：
- 影響範囲（Impact）：
  - docs：requirements / design / runbook / knowledge（該当箇所）
  - コード：対象モジュール/ディレクトリ
  - データ：スキーマ/保存形式/移行有無
  - インフラ：リソース変更/破壊的変更有無
- 関連リンク：
  - Plan Issue：#XXXX
  - Task Issue：#YYYY, #ZZZZ
  - PR：#PPPP
  - Obsidian：`/mnt/d/work/obsidian/cursor/10_PROJECTS/PJ<XXX>-<YYYY-MM>/`
- 検証（How to Verify）：
  - 観点：
  - 手順：
  - 期待結果：
- ロールバック方針（必要なら）：
- 備考：

---

### YYYY-MM-DD（vX / 変更ID: CHG-0002）
- 種別：
- 変更理由（Why）：
- 変更概要（What）：
- 影響範囲（Impact）：
- 関連リンク：
- 検証（How to Verify）：
- ロールバック方針：
- 備考：

---


