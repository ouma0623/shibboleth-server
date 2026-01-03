# Docs Index（Project Hub / Navigation）

> 本ドキュメントは **参照ハブ（索引・館内マップ）** です。  
> 「●●のときはどこを見るか」を最短で示します。  
> 手順やチェックリストは `docs/03_OPERATIONS/runbook.md` に集約します。

---

## 1. プロジェクト固有（下書きOK：/planで埋まっていく）

### 1.1 状態
- フェーズ：Action
- 最終更新日：2026-01-03

### 1.2 GitHub（状態機械）
- 親Plan Issue：#1 - [Plan] 計画概要：Shibboleth IdPサーバー構築（Phase 1: EC2構築・SSH接続）
  - URL：https://github.com/ouma0623/shibboleth-server/issues/1
- 子Task Issues：
  - #2 - [Task] T-01：VPCとセキュリティグループの作成（URL：https://github.com/ouma0623/shibboleth-server/issues/2）
  - #3 - [Task] T-02：SSHキーペアの作成または確認（URL：https://github.com/ouma0623/shibboleth-server/issues/3）
  - #4 - [Task] T-03：EC2インスタンスの作成（URL：https://github.com/ouma0623/shibboleth-server/issues/4）
  - #5 - [Task] T-04：SSH接続の疎通確認（URL：https://github.com/ouma0623/shibboleth-server/issues/5）
- 関連PR：なし

### 1.3 Obsidian（最終まとめ・ナレッジレビュー）
- Obsidian PJディレクトリ：`/mnt/d/work/obsidian/cursor/10_PROJECTS/PJ001-2026-01/`
- ローカル参照：`docs/90_OBSIDIAN/PJ001-2026-01/00_OVERVIEW.md`

---

## 2. 仕様（SSOT）
- 要件：`docs/01_REQUIREMENTS/requirements.md`
- 変更履歴：`docs/01_REQUIREMENTS/changes.md`

---

## 3. 設計
- 機能仕様：`docs/02_DESIGN/functional_spec.md`
- 構成図/方針：`docs/02_DESIGN/architecture.md`
- 基本設計：`docs/02_DESIGN/basic_design.md`

---

## 4. 運用・再現手順（Operations）
- Runbook（運用）：`docs/03_OPERATIONS/runbook.md`
- Runbook Test（テスト）：`docs/03_OPERATIONS/runbook_test.md`

### scripts（配置ルール：要約）
- 運用バッチ/スクリプト：`scripts/operation/`
  - 追加・変更時は `runbook.md` 更新必須
- テスト実行用スクリプト：`scripts/test/`
  - 追加・変更時は `runbook_test.md` 更新必須

---

## 5. AI Knowledge（再利用知見）
- `/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/`（Obsidian側のAIナレッジディレクトリ、SSOT）
- `/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/INDEX.md`（一覧）
- `/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/{decision,pitfall,pattern,prompt,mcp}/`
- Knowledge 昇格判断：`/mnt/d/work/obsidian/cursor/90_TEMPLATES/knowledge_review.md`
- Knowledge レビューコマンド：`/knowledge`
- **重要**：PJ固有のナレッジは作成しない（全て共通のナレッジとして管理）

---

## 6. AI運用ルール（Cursor）
- `.cursor/rules/00_general.mdc`
- `.cursor/rules/10_execution_model.mdc`
- `.cursor/rules/20_github.mdc`
- `.cursor/rules/30_directory.mdc`
- `.cursor/rules/40_command_contracts.mdc`
- `.cursor/rules/80_environment.mdc`
- `.cursor/rules/50_app_code.mdc`
- `.cursor/rules/60_infra_code.mdc`
- `.cursor/rules/70_knowledge.mdc`
- `.cursor/rules/75_knowledge_promotion.mdc`

---

## 7. 困ったときのナビ
- 仕様が分からない → 2（要件）
- 設計を確認したい → 3（設計）
- 実行方法/再現手順 → 4（Runbook）
- 判断理由/知見 → 5（AI Knowledge）
- ルールを確認 → 6（Cursor Rules）
- 今どこまで進んだ？ → 1.2（Issue）
