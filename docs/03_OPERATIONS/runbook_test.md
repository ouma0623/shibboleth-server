# Runbook Test（テスト実行手順）

本ドキュメントは、本プロジェクトにおける
**テスト実行用スクリプトの実行手順・再現手順**を記録するための Runbook Test である。

- `scripts/test/` 配下のスクリプトを追加・変更した場合は、**必ず本ドキュメントを更新する**
- `scripts/operation/` 配下のスクリプトは `docs/03_OPERATIONS/runbook.md` を更新する
- 実行できない手順は未完成とみなす
- チェックリストや判断基準は rules / Issue に従う

---

## 0. 前提情報（共通）

- 対象プロジェクト：
- 対象リポジトリ：
- 想定実行者：
  - 開発者
  - AI（Cursor + MCP）
- 対象環境：
  - local
  - dev
  - stg
  - prod
- 関連リンク：
  - docs hub：docs/00_INDEX.md
  - requirements：docs/01_REQUIREMENTS/requirements.md
  - runbook（運用）：docs/03_OPERATIONS/runbook.md
  - Plan Issue：#XXXX
  - Obsidian：`/mnt/d/work/obsidian/cursor/10_PROJECTS/PJ<XXX>-<YYYY-MM>/`

---

## 1. scripts/test（テスト実行用スクリプト）

手動確認・疎通確認・E2E 補助など、
**テスト目的で実行するスクリプト**を管理する。
新規作成・変更時は **必ず本章を更新する**。

### 1.1 一覧

| 名称 | パス | 対象 | 実行タイミング |
|---|---|---|---|
| | scripts/test/ | | |

---

### 1.2 実行手順

#### 対象スクリプト
- 名称：
- パス：scripts/test/<name>

#### 目的
- 何を確認するためのテストか：

#### 前提条件
- 実行環境：
- 必要な権限：
- 事前準備（データ / 状態）：

#### 実行方法

```bash
# 実行コマンド例
```

#### 期待結果
- 正常時の結果：
- 出力 / ログ：

#### NG 時の対応
- 想定される原因：
- 次のアクション：
  - Do（実装修正）
  - Plan（仕様見直し）

---

## 2. テスト実行フロー（概要）

- 単体テスト実行
  - 個別機能のテスト
  - モック・スタブの準備
- 統合テスト実行
  - 複数コンポーネントの連携テスト
  - 外部依存の扱い
- E2Eテスト実行
  - エンドツーエンドの動作確認
  - 環境構築手順

---

## 3. 変更履歴・関連

- 変更履歴：docs/01_REQUIREMENTS/changes.md
- 関連 Issue：
  - Plan：#XXXX
  - Task：#YYYY
- 関連 PR：#PPPP

---

## 4. メモ（任意）

- 環境依存事項：
- 将来の改善案：

