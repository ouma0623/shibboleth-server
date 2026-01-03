# Project Overview

このリポジトリは、AI（Cursor + MCP）を実務チームの一員として運用する前提で、
**仕様駆動 + PDCA + Issue駆動** により開発・検証・知見化を行うプロジェクトです。

- 仕様の唯一の正（SSOT）：`docs/`
- 実行状態（状態機械）：GitHub Issue（親Plan / 子Task）
- 最終成果・再現装置：Obsidian（ナレッジレビュー用）

---

## Quick Start（最短導線）

### 1) まず見る場所
- プロジェクトの館内マップ：`docs/00_INDEX.md`
- 要件：`docs/01_REQUIREMENTS/requirements.md`
- 運用・再現手順：`docs/03_OPERATIONS/runbook.md`

### 2) 基本フロー（PDCAコマンド）
このPJは以下のコマンドで回します。

#### Pre-Plan コマンド（Plan前段）
- `/review`：既存コードを分析し、改善点・リスクを洗い出す（Issue作成なし）
- `/doc`：既存コードから docs を再構築・整理する（Issue作成なし）

#### PDCA コマンド
- `/plan`：仕様理解 → docs更新 → 親Plan Issue作成 → 子Task Issue全作成
- `/do`：対象Taskを実装し、DoログをTask Issueへ追記（新規Task禁止）
- `/check`：テスト/動作確認し、Checkログ追記 → OKならTask Close → 親Plan表✅更新
- `/action`：知見/運用/変更履歴/Obsidian反映 → 親Plan更新→Close
- `/knowledge`：Obsidianプロジェクトディレクトリ内の全ファイル（作業ファイル・タスクファイル・総評・判断ログなど）をレビューし、AIナレッジ化できるものをどんどん知識として昇華していく（Obsidian側で実行）

#### 使用例

**新規機能開発の場合**
1. `/plan` で要件定義・設計・Task確定
2. `/do` で各Taskを実装
3. `/check` で各Taskを検証
4. `/action` で知見化・Obsidian反映

**既存コードの改善の場合**
1. `/review` で現状分析・改善点抽出
2. `/plan` で改善対象のスコープ定義・Task確定（/review結果を入力として使用）
3. `/do` で各Taskを実装
4. `/check` で各Taskを検証
5. `/action` で知見化・Notion反映

**ドキュメント整備の場合**
1. `/doc` で既存コードから docs を再構築
2. `/plan` で不足している docs の作成Taskを確定（必要に応じて）
3. `/do` で docs 作成
4. `/check` で docs の整合性確認
5. `/action` で最終整理

---

## Directory Layout（要点のみ）

- `docs/`：仕様・設計・運用・知見（SSOT）
- `.cursor/`：Cursorコマンド・ルール（AI実務OS）
- `.github/ISSUE_TEMPLATE/`：Issueテンプレ（親Plan/子Task）
- `scripts/operation/`：運用バッチ/スクリプト（追加時はrunbook更新必須）
- `scripts/test/`：テスト実行用スクリプト（追加時はrunbook更新必須）

詳細は `docs/00_INDEX.md` を参照。

---

## Rules（AI運用の前提）

- 不変原則：`.cursor/rules/00_general.mdc`
- 実行モデル：`.cursor/rules/10_execution_model.mdc`
- GitHub運用：`.cursor/rules/20_github.mdc`
- ディレクトリ：`.cursor/rules/30_directory.mdc`
- コマンド契約：`.cursor/rules/40_command_contracts.mdc`
- アプリコード：`.cursor/rules/50_app_code.mdc`
- インフラコード：`.cursor/rules/60_infra_code.mdc`
- AI Knowledge：`.cursor/rules/70_knowledge.mdc`
- Knowledge Promotion：`.cursor/rules/75_knowledge_promotion.mdc`
- 環境・ツール：`.cursor/rules/80_environment.mdc`

---

## Contribution（運用ルール）

- 仕様やタスクは `/plan` で確定し、Do/Checkで増やしません
- scripts を追加した場合は `docs/03_OPERATIONS/runbook.md` を必ず更新します
- 実行したことは Issue に証跡（Do/Checkログ）として残します

### よくある質問

**Q: Doフェーズで仕様変更が必要になった場合は？**
A: Planに差し戻してください。Do/Checkで仕様変更は禁止です。

**Q: scripts を作成したが runbook を更新し忘れた場合は？**
A: Doログに「runbook更新」を必須項目として追加しているため、更新し忘れを防げます。更新し忘れた場合は未完了扱いです。

**Q: /review や /doc で Issue を作成してもよいですか？**
A: いいえ。これらのコマンドは Issue を作成しません。必ず /plan を通してください。

**Q: Obsidian Vault のパスはどこですか？**
A: `/mnt/d/work/obsidian/cursor/` です。Actionフェーズで自動的にコピーされます。

**Q: /knowledge コマンドはどこで実行しますか？**
A: Obsidian側（`/mnt/d/work/obsidian/cursor/10_PROJECTS/PJ<XXX>-<YYYY-MM>/`）で実行します。プロジェクトディレクトリ内の全ファイルから知見を抽出し、Knowledge 化します。

**Q: Knowledge 昇格判断の基準は？**
A: `.cursor/rules/75_knowledge_promotion.mdc` の基準に従います。再利用性・判断価値・実務効果・表現可能性の4つの観点で評価し、合計3つ以上 Yes が付いたら Knowledge 化を推奨します。

---

## Links
- Hub（索引）：`docs/00_INDEX.md`
