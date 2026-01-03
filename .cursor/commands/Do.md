# /do
# PDCA: Do Phase
# Role: Developer / Implementer

## 【このコマンドの位置づけ】
Doフェーズは「実行と記録のフェーズ」である。
判断・設計変更・タスク追加は禁止。

---
## 【ルール理解チェック（必須・省略不可）】
各コマンド実行時、最初に以下を短く宣言してから作業を進める。

- 私は以下を理解し遵守する：
  1) Planで確定したTaskのみを実行し、仕様変更・Task追加は禁止する
  2) Doログを必ず記録する（実施日・実施内容・変更ファイル・判断理由・残課題）
  3) scripts/operation または scripts/test を作成した場合、対応するrunbookを必ず更新する
  4) 作業ファイル（debug-log.txt、design-draft.md等）は docs/90_OBSIDIAN/PJ<XXX>-<YYYY-MM>/99_work_files/ に配置する
  5) Task定義（目的・完了条件）は変更せず、記録を残さずに実装しない

※ これを省略しない

## 【前提理解チェック（必須）】
- 対象は Plan で確定した Task Issue のみ
- Task定義（目的 / 完了条件）は変更しない
- 新規Taskは作成しない
- Doログは 10_execution_model.mdc の必須項目を満たさない限り完了扱いしない
- **AIナレッジ参照**：Obsidian側のAIナレッジディレクトリ（`/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/`）を必ず参照して実施する

---

## 【責務】
- Task に定義された作業を実行する
- 実行内容を“証跡として”残す

---

## 【禁止事項】
- 仕様を変えること
- Task を分割・追加すること
- 記録を残さずに実装すること

---

## 【Operation Plan】

### 1. Task Issue 読み取り（Read）
- 対象Task Issue
- タスク定義（Plan確定）

### 2. 実装（Update）
- コード実装
- テストコード作成（可能な範囲）

### 3. Task Issue 更新（Update）
Doフェーズ欄に以下を時系列で追記する：

- 実施日
- 実施内容
- 変更ファイル
- 判断理由
- 残課題 / 懸念

### 4. scripts 配置（該当時のみ）
- 運用目的のバッチ/スクリプトを作成した場合：
  - Create/Update: `scripts/operation/<name>.*`
  - Update: `docs/03_OPERATIONS/runbook.md`（実行手順・前提・ロールバック・注意点）

- テスト実行用スクリプトを作成した場合：
  - Create/Update: `scripts/test/<name>.*`
  - Update: `docs/03_OPERATIONS/runbook_test.md`（実行方法・期待結果・注意点）

※ scripts を作ったのに runbook を更新しない状態は「Do未完了」扱い。

### 5. 作業ファイル配置（該当時のみ）
> **注意**：案件固有の作業ファイル（debug-log.txt、design-draft.md、memo-YYYYMMDD.md等）を作成した場合、Obsidian PJディレクトリの `99_work_files/` に配置する。

1) 作業ファイルが作成された場合：
   - Planフェーズで作成されたPJディレクトリを確認する
     - `docs/90_OBSIDIAN/PJ<XXX>-<YYYY-MM>/99_work_files/`
   - 作業ファイルを `99_work_files/` に配置する
   - ファイル名は意味のある名前を付ける（例：`debug-log-YYYYMMDD.txt`, `design-draft.md`）

### 6. docs 補足（必要時のみ）
- runbook.md（運用影響が出た場合）
- changes.md（仕様に影響しない変更記録）

---

## 【フェーズ完了条件】
- Task定義に沿った実装が完了している
- Doログが客観的に再現可能
- scripts/operation を作成した場合、docs/03_OPERATIONS/runbook.md が更新されている
- scripts/test を作成した場合、docs/03_OPERATIONS/runbook_test.md が更新されている

---

## 【出力】
- 実装差分要約
- Task Issue 更新要約
- 次アクション（Check or Plan差戻）

---

## 【フェーズ宣言】
Doフェーズを開始する。
Doフェーズを終了する。
