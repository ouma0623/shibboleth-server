# /action
# PDCA: Action Phase
# Role: PMO / Knowledge Manager

## 【このコマンドの位置づけ】
Actionフェーズは「学習と制度化のフェーズ」である。
ここで何も残らないPJは"失敗"とみなす。

---

## 【ルール理解チェック（必須・省略不可）】
各コマンド実行時、最初に以下を短く宣言してから作業を進める。

- 私は以下を理解し遵守する：
  1) 全Task IssueがClose済みであることを確認してから実行する
  2) 振り返りを省略せず、学びをdocs/ルール/Obsidianに還元する
  3) ルール再定義を徹底的に行い、次回PJで活用できる状態にする
  4) ナレッジ化候補を記録し、判断ログ・学びを構造化して記録する
  5) Obsidianプロジェクトディレクトリを作成し、コピー後に00_OVERVIEW.md以外を削除して整理する

※ これを省略しない

## 【前提理解チェック（必須）】
- 全Taskが完了していること
- 親Plan Issue が残っていること
- docs/90_OBSIDIAN/TEMPLATE/ 配下のテンプレートファイルが存在すること
- **AIナレッジ参照**：Obsidian側のAIナレッジディレクトリ（`/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/`）が存在し、必ず参照して実施する

---

## 【責務】
- **学びを docs / ルール / Obsidian に還元する**
- **次回同じことを"考えなくてよい状態"を作る**
- **ルール再定義を徹底的に行う**
- **ナレッジ化候補を記録する**（詳細なレビューは `/knowledge` コマンドで実施）
- **判断ログ・学びを構造化して記録する**

---

## 【禁止事項】
- 振り返りを省略すること
- Plan Issue を更新せずに Close すること
- **ルール再定義を軽視すること**
- **ナレッジ化候補の記録を省略すること**
- **判断ログ・学びを曖昧に記録すること**

---

## 【Operation Plan】

### 1. 完了確認（Verify）
- 全 Task Issue が Close 済みであることを確認
- 各Task IssueのDoログ・Checkログが記録されていることを確認

### 2. 親Plan Issue 更新（Update）
- 実績ベースの設計方針
- 振り返り（良/悪/改善）
- Done条件の達成根拠

### 3. docs 更新（Update）
- DESIGN(設計・構成)　※必要に応じて
- docs/03_OPERATIONS/runbook.md（運用・手順）※必要に応じて
- docs/03_OPERATIONS/runbook_test.md（テスト実行手順）※必要に応じて
- docs/01_REQUIREMENTS/changes.md（履歴）
- docs/00_INDEX.md（最終リンク）

### 4. AI Knowledge 更新（Update）
- 再利用可能な知見を判断し、必要に応じて Knowledge 化する
- 保存先：`/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/`（Obsidian側のAIナレッジディレクトリ、SSOT）
- 1 Knowledge = 1 ファイル
- INDEX.md を必ず更新する
- **前提チェック**：Obsidian側のAIナレッジディレクトリ（`/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/`）を必ず参照して実施する
- **参照ルール**：`.cursor/rules/75_knowledge_promotion.mdc` を必ず参照する
- **詳細なナレッジレビュー**：`/knowledge` コマンドで実行する（Obsidian側で実施）

### 5. Obsidian プロジェクトディレクトリ作成（必須）

#### 5.1 PJ名の決定
1) 実行日（YYYY-MM形式）を取得する
   - 例：2025年12月 → `2025-12`
2) 既存のPJ番号を確認する
   - `docs/90_OBSIDIAN/` 配下のディレクトリを確認
   - 同じ月（YYYY-MM）内での最大PJ番号を特定する
3) 新しいPJ番号を決定する
   - 形式：`PJ<XXX>-<YYYY-MM>`
   - 既存のPJ番号がない場合：`PJ001-<YYYY-MM>`
   - 既存のPJ番号がある場合：最大番号+1（例：`PJ001-2025-12` が存在する場合、次は `PJ002-2025-12`）
   - 例：`PJ001-2025-12`, `PJ002-2025-12`
4) PJディレクトリ名を決定する
   - パス：`docs/90_OBSIDIAN/PJ<XXX>-<YYYY-MM>/`
   - 例：`docs/90_OBSIDIAN/PJ001-2025-12/`
   - **注意**：Planフェーズで既に作成されている場合は、そのディレクトリを使用する
   - **注意**：Planフェーズで既に作成されている場合は、そのディレクトリを使用する

#### 5.2 ディレクトリ構造の作成
1) PJディレクトリを作成する
   - `docs/90_OBSIDIAN/PJ<XXX>-<YYYY-MM>/`
2) `99_work_files/` ディレクトリを作成する
   - `docs/90_OBSIDIAN/PJ<XXX>-<YYYY-MM>/99_work_files/`
   - Plan/Do/Checkフェーズで作成された作業ファイルが既に存在する場合は、そのまま使用する

#### 5.3 テンプレートファイルから各ファイルを作成

##### 5.3.1 00_OVERVIEW.md の作成
1) `docs/90_OBSIDIAN/TEMPLATE/00_OVERVIEW.md` を読み込む
2) テンプレートの各項目を埋める：
   - `<案件名>`：親Plan Issueのタイトル
   - `<プロジェクト名>`：プロジェクト名
   - `<XXX>`：PJ番号（例：001）
   - `<YYYY-MM>`：年月（例：2025-12）
   - `<GitHub #XXXX>`：親Plan Issue番号
   - `<URL>`：GitHubリポジトリURL
   - その他の項目：親Plan Issue / Task Issue / docs の内容から埋める
3) 必須リンクを埋める：
   - docs/00_INDEX.md へのリンク
   - docs/01_REQUIREMENTS/requirements.md へのリンク
   - docs/00_INDEX.md に記載されている設計書へのリンク（存在する場合）
   - docs/03_OPERATIONS/runbook.md へのリンク
   - docs/03_OPERATIONS/runbook_test.md へのリンク（scripts/test が存在する場合）
   - `/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/INDEX.md` へのリンク
   - 親Plan Issue URL
   - 子Task Issue URL（すべて）
4) 作成した内容を `docs/90_OBSIDIAN/PJ<XXX>-<YYYY-MM>/00_OVERVIEW.md` として保存する

##### 5.3.2 01_DECISIONS.md の作成
1) `docs/90_OBSIDIAN/TEMPLATE/01_DECISIONS.md` を読み込む
2) 親Plan Issue / Task Issue / Doログ / Checkログから判断を抽出する
3) **判断抽出の観点**：
   - 技術選択（ライブラリ・フレームワーク・アーキテクチャ）
   - 実装方法の選択（複数の選択肢があった場合）
   - トレードオフの判断（性能 vs 保守性、速度 vs 品質など）
   - プロセス判断（開発フロー・運用方法）
   - リスク対応判断（障害対応・セキュリティ対策）
4) 各判断をD-001, D-002...の形式で記録する
5) 判断パターン・未解決の判断も記録する
6) 作成した内容を `docs/90_OBSIDIAN/PJ<XXX>-<YYYY-MM>/01_DECISIONS.md` として保存する

##### 5.3.3 02_LESSONS.md の作成
1) `docs/90_OBSIDIAN/TEMPLATE/02_LESSONS.md` を読み込む
2) 親Plan Issue / Task Issue / Doログ / Checkログから学びを抽出する
3) **学び抽出の観点**：
   - 技術的な学び（新しい技術・パターン・ベストプラクティス）
   - 運用上の学び（監視・アラート・ロールバック）
   - プロセス上の学び（開発フロー・コミュニケーション）
   - ハマりどころ（再発防止すべき失敗）
   - 成功パターン（次回も使える成功事例）
4) 各学びをL-001, L-002...の形式で記録する
5) カテゴリ別にまとめる（技術 / 運用 / プロセス）
6) ナレッジ化候補を特定する（`/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/` に移行すべき知見）
7) 次回への改善提案も記録する
8) 作成した内容を `docs/90_OBSIDIAN/PJ<XXX>-<YYYY-MM>/02_LESSONS.md` として保存する

##### 5.3.4 03_LINKS.md の作成
1) `docs/90_OBSIDIAN/TEMPLATE/03_LINKS.md` を読み込む
2) 全ての関連リンクを集約する：
   - ドキュメント（SSOT）
   - GitHub（Issue / PR）
   - 作業ファイル（99_work_files/ 配下）
   - 外部リソース
3) 作成した内容を `docs/90_OBSIDIAN/PJ<XXX>-<YYYY-MM>/03_LINKS.md` として保存する

##### 5.3.5 04_TASKS.md の作成
1) `docs/90_OBSIDIAN/TEMPLATE/04_TASKS.md` を読み込む
2) 各Task Issueの内容をまとめる：
   - Task番号・タイトル・状態
   - 実施内容（Doログの要約）
   - 検証結果（Checkログの要約）
   - 関連リンク
3) **証跡として記録すべき内容**：
   - 何を実施したか（具体的な作業内容）
   - どうやって実施したか（実装方法・手順）
   - 何が確認できたか（検証結果・判定）
   - なぜその方法を選んだか（判断理由）
   - 残課題・懸念事項
4) タスク統計も記録する
5) 重要なタスク・課題・改善点も記録する
6) 作成した内容を `docs/90_OBSIDIAN/PJ<XXX>-<YYYY-MM>/04_TASKS.md` として保存する

### 6. ルール再定義とナレッジ化候補の記録（必須・省略不可）
> **重要**：ルール再定義を徹底的に行い、ナレッジ化候補を記録し、次回のプロジェクトで活用できる状態にする。  
> **詳細なナレッジレビュー・昇格判断は `/knowledge` コマンドで実行する（Obsidian側で実施）**

#### 6.1 ナレッジ化候補の記録
01_DECISIONS.md と 02_LESSONS.md の内容を基に、ナレッジ化候補を記録する：

1) `02_LESSONS.md` の「ナレッジ化候補」セクションに記録する
   - 候補となる知見をリストアップ
   - 理由・移行先カテゴリを記録
2) `01_DECISIONS.md` の「判断パターン」セクションに記録する（該当時）
   - 繰り返し発生する判断パターンを記録
   - 判断基準・推奨選択を記録

**参照ルール**：`.cursor/rules/75_knowledge_promotion.mdc` の判断基準を参照する

#### 6.2 ルール再定義の判断（必須・省略不可）
> **重要**：ルール再定義を徹底的に行い、次回のプロジェクトで活用できる状態にする。

プロジェクトで発見した問題・改善点を基に、以下を判断する：

**ルール再定義が必要なケース**
- [ ] 既存ルールが不十分で、迷いが発生した
- [ ] 既存ルールに従ったが、問題が発生した
- [ ] 新しいパターンが発見され、ルール化すべき
- [ ] ルールの解釈が曖昧で、明確化が必要

**ルール再定義の手順**
1) 問題・改善点を特定する
2) どのルールファイル（.cursor/rules/*.mdc）を更新すべきか判断する
3) ルール更新案を検討する
4) ルールファイルを更新する
5) 01_DECISIONS.md の「判断パターン」セクションに記録する
6) 02_LESSONS.md の「次回への改善提案」セクションに記録する

**ルール再定義の観点**
- **実行モデル（10_execution_model.mdc）**
  - フェーズ遷移の改善
  - 必須項目の追加・変更
  - 禁止事項の追加・変更
- **GitHub運用（20_github.mdc）**
  - Issue管理の改善
  - テンプレートの改善
- **ディレクトリ（30_directory.mdc）**
  - SSOTの明確化
  - ディレクトリ構造の改善
- **コマンド契約（40_command_contracts.mdc）**
  - コマンド責務の明確化
  - 成果物の追加・変更
- **コードルール（50_app_code.mdc / 60_infra_code.mdc）**
  - コーディング規約の改善
  - 実装パターンの追加
- **ナレッジ管理（70_knowledge.mdc）**
  - ナレッジ化基準の明確化
  - テンプレートの改善
- **Knowledge Promotion（75_knowledge_promotion.mdc）**
  - 昇格判断基準の明確化
  - カテゴリ分類の改善
- **環境・ツール（80_environment.mdc）**
  - ツール設定の改善
  - MCP操作の改善

#### 6.3 ルール再定義の記録
- 01_DECISIONS.md に判断パターン・ルール再定義を記録する
- 02_LESSONS.md に改善提案を記録する
- 実際にルールファイルを更新した場合は、その旨を記録する

### 7. Obsidianへのコピー（必須）
1) Obsidian Vaultのパスを確認する
   - `/mnt/d/work/obsidian/cursor/`
2) コピー先ディレクトリを作成する
   - `/mnt/d/work/obsidian/cursor/10_PROJECTS/PJ<XXX>-<YYYY-MM>/`
   - 既に存在する場合はエラーとする（重複チェック）
3) `docs/90_OBSIDIAN/PJ<XXX>-<YYYY-MM>/` 配下の全ファイル・ディレクトリをコピーする
   - `00_OVERVIEW.md`
   - `01_DECISIONS.md`
   - `02_LESSONS.md`
   - `03_LINKS.md`
   - `04_TASKS.md`
   - `99_work_files/` ディレクトリ（中身も含む）
4) コピーが完了したことを確認する
5) コピー先のパスを報告する

### 8. ローカルファイルの整理（必須）
1) `docs/90_OBSIDIAN/PJ<XXX>-<YYYY-MM>/` 配下のファイルを確認する
2) `00_OVERVIEW.md` 以外のファイル・ディレクトリを削除する：
   - `01_DECISIONS.md` → 削除
   - `02_LESSONS.md` → 削除
   - `03_LINKS.md` → 削除
   - `04_TASKS.md` → 削除
   - `99_work_files/` → 削除
3) `00_OVERVIEW.md` のみを残す
4) 削除が完了したことを確認する

### 9. クローズ（Close）
- 親Plan Issue を Close

---

## 【フェーズ完了条件】
- 知見が再利用可能な形で残っている
- 次回PJで同じ判断を繰り返さずに済む
- **ルール再定義が徹底的に行われている**
  - 判断ログ（01_DECISIONS.md）が充実している
  - 学び（02_LESSONS.md）が構造化されている
  - ルール再定義が必要なケースが特定されている
  - ルールファイルが更新されている（該当時）
- **ナレッジ化候補が記録されている**
  - ナレッジ化候補が記録されている（詳細なレビューは `/knowledge` コマンドで実施）
- **Obsidianへのコピーが完了している**
- **ローカルファイルの整理が完了している**（00_OVERVIEW.mdのみ残存）

---

## 【出力】
- Close完了報告
- 更新docs一覧
- Obsidianプロジェクトディレクトリのパス（`/mnt/d/work/obsidian/cursor/10_PROJECTS/PJ<XXX>-<YYYY-MM>/`）
- 作成したObsidianファイル一覧（00_OVERVIEW.md, 01_DECISIONS.md, 02_LESSONS.md, 03_LINKS.md, 04_TASKS.md）
- ルール再定義の実施内容（判断パターン・改善提案・ルールファイル更新内容）
- ナレッジ化候補の記録内容（詳細なレビューは `/knowledge` コマンドで実施）
- 改善提案（ルール / 運用）
- **次のステップ**：`/knowledge` コマンドでナレッジレビューを実施する（Obsidian側で実施）

---

## 【フェーズ宣言】
Actionフェーズを開始する。
Actionフェーズを終了する。
