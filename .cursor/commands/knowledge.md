# /knowledge
# Knowledge Review & Promotion Command
# Role: Knowledge Manager / Librarian

## 【このコマンドの位置づけ】
Knowledge Review & Promotion コマンドは、**Obsidianプロジェクトディレクトリ内の全ファイル**をレビューし、`/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/` に昇格させるべき知見を抽出・昇格させるためのコマンドである。

本コマンドは **Obsidian側で実行**し、プロジェクトディレクトリ内の作業ファイル・タスクファイル・総評・判断ログなど全てから、AIナレッジ化できるものをどんどん知識として昇華していく。

---

## 【ルール理解チェック（必須・省略不可）】
各コマンド実行時、最初に以下を短く宣言してから作業を進める。

- 私は以下を理解し遵守する：
  1) docs は仕様の唯一の正
  2) Knowledge の正は `/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/`
  3) Obsidian はナレッジレビュー用・閲覧用
  4) Knowledge 化は `.cursor/rules/75_knowledge_promotion.mdc` の基準に従う（**必ず参照して理解して実施する**）
  5) `.cursor/rules/70_knowledge.mdc` の運用・管理ルールに従う（**必ず参照して理解して実施する**）
  6) 合計3つ以上 Yes が付いたら Knowledge 化を推奨する
  7) ユースケース・適用前提・状況判断のための前提を必ず記入する

※ これを省略しない

---

## 【前提理解チェック（必須）】
- Obsidianプロジェクトディレクトリ（`/mnt/d/work/obsidian/cursor/10_PROJECTS/PJ<XXX>-<YYYY-MM>/`）が存在すること
- プロジェクトディレクトリ内にファイル（00_OVERVIEW.md, 01_DECISIONS.md, 02_LESSONS.md, 04_TASKS.md, 99_work_files/等）が存在すること
- **Obsidian側のAIナレッジディレクトリ（`/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/`）が存在すること**
- **`.cursor/rules/75_knowledge_promotion.mdc` を必ず参照して理解していること**
- **`.cursor/rules/70_knowledge.mdc` を必ず参照して理解していること**

---

## 【責務】
- Obsidianプロジェクトディレクトリ内の**全ファイル**をレビューする
- `.cursor/rules/75_knowledge_promotion.mdc` の基準に従って評価する（**必ず参照して理解して実施する**）
- `.cursor/rules/70_knowledge.mdc` の運用・管理ルールに従う（**必ず参照して理解して実施する**）
- Knowledge 化すべき知見を特定し、`/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/` に昇格させる
- ユースケース・適用前提・状況判断のための前提を必ず記入する
- INDEX.md を更新する

---

## 【禁止事項】
- プロジェクト固有の情報を Knowledge 化すること
- 一時的なトレンドに依存する情報を Knowledge 化すること
- 抽象度が低すぎる具体例を Knowledge 化すること
- `.cursor/rules/75_knowledge_promotion.mdc` の基準を考慮せずに Knowledge 化すること
- **重複チェックをせずに Knowledge 化すること**
- **既存Knowledgeと重複する内容を、前提条件の違いを明確にせずに Knowledge 化すること**

---

## 【Operation Plan】

### 1. ルールの読み込み（Read）（必須・省略不可）
1) `.cursor/rules/75_knowledge_promotion.mdc` を読み込む（**必ず参照して理解する**）
   - Knowledge 昇格判断基準を理解する
   - 4つの観点（再利用性・判断価値・実務効果・表現可能性）を理解する
   - カテゴリ分類の基準を理解する
   - ナレッジ化の対象ファイルと抽出観点を理解する
2) `.cursor/rules/70_knowledge.mdc` を読み込む（**必ず参照して理解する**）
   - AI Knowledge の運用・管理ルールを理解する
   - 保存場所・保存単位・分類・必須構造を理解する
   - テンプレートの使い方を理解する

### 2. Obsidianプロジェクトディレクトリの読み取り（Read）
1) 対象プロジェクトディレクトリを特定する
   - `/mnt/d/work/obsidian/cursor/10_PROJECTS/PJ<XXX>-<YYYY-MM>/`
2) プロジェクトディレクトリ内の**全ファイル・ディレクトリ**を確認する：
   - `00_OVERVIEW.md`：PJ総評
   - `01_DECISIONS.md`：判断ログ
   - `02_LESSONS.md`：学び
   - `03_LINKS.md`：リンク集
   - `04_TASKS.md`：タスクまとめ
   - `99_work_files/`：作業ファイルディレクトリ
     - debug-log.txt
     - design-draft.md
     - memo-YYYYMMDD.md
     - その他の作業ファイル

### 3. 知見の抽出（Extract）
各ファイルから知見を抽出する。`.cursor/rules/75_knowledge_promotion.mdc` の「4. ナレッジ化の対象」に従って抽出する。

#### 3.1 00_OVERVIEW.md からの抽出
- 実施内容サマリから学びを抽出
- レビュー・注意点から知見を抽出

#### 3.2 01_DECISIONS.md からの抽出
- 各判断（D-XXX）から判断パターンを抽出
- 判断パターンセクションから知見を抽出
- 未解決の判断から将来の知見候補を抽出

#### 3.3 02_LESSONS.md からの抽出
- 各学び（L-XXX）から知見を抽出
- ナレッジ化候補セクションを確認
- 次回への改善提案から知見を抽出

#### 3.4 04_TASKS.md からの抽出
- 各タスクの実施内容から実装パターンを抽出
- 検証結果から検証パターンを抽出
- 残課題・懸念からハマりどころを抽出
- 重要なタスクから判断パターンを抽出

#### 3.5 99_work_files/ からの抽出
- debug-log.txt：デバッグ時の知見・ハマりどころ
- design-draft.md：設計時の判断・選択・トレードオフ
- memo-YYYYMMDD.md：作業メモから知見・判断・パターンを抽出
- その他の作業ファイル：ファイル内容に応じて知見を抽出

#### 3.6 抽出の観点
各ファイルから以下の観点で知見を抽出する（`.cursor/rules/75_knowledge_promotion.mdc` の「4.2 抽出の観点」参照）：
- **判断・選択**：なぜそうしたか、判断基準は何か
- **失敗・ハマりどころ**：何が問題だったか、どう回避するか
- **成功パターン**：何がうまくいったか、どう再現するか
- **実装パターン**：どう実装したか、なぜその方法を選んだか
- **検証パターン**：どう検証したか、何を確認したか
- **プロセス改善**：プロセス上の学び、改善提案

### 4. Knowledge 昇格判断（Evaluate）
抽出した各知見について、`.cursor/rules/75_knowledge_promotion.mdc` の「2. Knowledge 昇格判断基準」に従って評価する。

#### 4.1 判断の実施
1) 各知見について、4つの観点で評価する：
   - **再利用性**（4項目）
   - **判断価値**（5項目）
   - **実務効果**（6項目）
   - **表現可能性**（6項目）
2) Yes数をカウントする
3) **合計3つ以上 Yes** の場合、Knowledge 化を推奨する
4) カテゴリを決定する（`.cursor/rules/75_knowledge_promotion.mdc` の「3. Knowledge カテゴリ分類」参照）

#### 4.2 カテゴリの決定
Knowledge 化する場合、適切なカテゴリを決定する：
- **decision/**：判断・選択に関する知見
- **pitfall/**：ハマりどころ・失敗事例
- **pattern/**：実装・運用の定石
- **prompt/**：AIプロンプトの知見
- **mcp/**：MCP操作の定型

#### 4.3 ファイル名の決定と重複チェック（必須・省略不可）
- カテゴリに応じた命名規則に従う（`.cursor/rules/70_knowledge.mdc` 参照）
- **重複チェック**：`/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/<category>/` 配下の既存ファイルを確認する
- **重複判定基準**：
  - 同じ内容・同じ前提条件のKnowledgeが既に存在する場合は重複とみなす
  - 前提条件や環境が異なる場合は重複とみなさない（別のKnowledgeとして作成可能）
  - 既存Knowledgeの「適用前提・ユースケース」セクションを確認し、適用条件が異なるか判断する
- 重複がない場合のみ、新しいKnowledgeファイルを作成する

### 5. Knowledge ファイル作成（Create）
Knowledge 化する場合、以下の手順でファイルを作成する。

#### 5.1 テンプレートの読み込み
1) 適切なカテゴリのテンプレートを参照する
   - `/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/TEMPLATES/decision_entry.md`
   - `/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/TEMPLATES/pitfall_entry.md`
   - `/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/TEMPLATES/pattern_entry.md`
   - `/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/TEMPLATES/prompt_entry.md`
   - `/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/TEMPLATES/mcp_entry.md`
2) テンプレートの「適用前提・ユースケース」セクションを必ず記入する
   - 適用すべき状況・適用すべきでない状況を明確に定義する
   - 前提条件・制約・注意事項を記入する
   - 複数のPJで矛盾が起きないように、適用条件を具体的に記述する

#### 5.2 内容の記入
1) **適用前提・ユースケース**を必ず記入する（最重要）
   - 適用すべき状況・適用すべきでない状況を明確に定義する
   - 前提条件・制約・注意事項を記入する
   - 複数のPJで矛盾が起きないように、適用条件を具体的に記述する
2) 基本情報を記入する
3) What / Why / How を記入する
4) 効果・影響を記入する
5) 適用例・パターンを記入する
6) 関連Knowledgeを記入する
7) 検証・確認方法を記入する

#### 5.3 ファイルの保存と重複チェック（必須・省略不可）
1) **重複チェック**：`/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/<category>/` 配下の既存ファイルを確認する
   - 既存Knowledgeファイルの「適用前提・ユースケース」セクションを読み込む
   - 同じ内容・同じ前提条件のKnowledgeが既に存在する場合は重複とみなす
   - 前提条件や環境が異なる場合は重複とみなさない（別のKnowledgeとして作成可能）
2) 重複がない場合のみ、適切なカテゴリディレクトリに保存する
   - `/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/<category>/<filename>.md`
3) ファイル名は意味のある名前を付ける（`.cursor/rules/70_knowledge.mdc` の命名規則に従う）
4) 重複がある場合は、既存Knowledgeファイルを更新するか、適用前提が異なることを明確にした上で別ファイルとして作成する

### 6. INDEX.md 更新（Update）
1) `/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/INDEX.md` を読み込む
2) **既存Knowledgeファイルの確認**（必須・省略不可）
   - `/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/<category>/` 配下の全ファイルを確認する
   - 各カテゴリ（decision, pitfall, pattern, prompt, mcp）の既存ファイルを列挙する
   - INDEX.mdに記載されていない既存Knowledgeファイルを特定する
3) **既存KnowledgeファイルをINDEX.mdに反映する**（該当時）
   - INDEX.mdに記載されていない既存Knowledgeファイルを、適切なカテゴリセクションに追加する
   - ファイル名とリンクを追加する
   - ファイル名の形式（K001, K002など）を確認し、適切に記載する
4) 新しいKnowledgeファイルを追加する
5) カテゴリ別に整理する
6) 関連リンクを追加する
7) 更新履歴を記録する

### 7. Obsidianプロジェクトディレクトリの更新（Update）
1) `/mnt/d/work/obsidian/cursor/10_PROJECTS/PJ<XXX>-<YYYY-MM>/02_LESSONS.md` の「ナレッジ化候補」セクションを更新する
   - Knowledge 化した知見を「昇格済み」として記録
   - Knowledge ファイルへのリンクを追加
2) `/mnt/d/work/obsidian/cursor/10_PROJECTS/PJ<XXX>-<YYYY-MM>/01_DECISIONS.md` の「判断パターン」セクションを更新する（該当時）
   - Knowledge 化した判断パターンを「昇格済み」として記録
   - Knowledge ファイルへのリンクを追加

### 8. レビュー結果の記録（Record）
1) Knowledge 昇格判断の結果を記録する
   - Knowledge 化した知見の一覧（ファイルパス・カテゴリ・タイトル）
   - Knowledge 化しなかった知見とその理由
   - 今後のレビューで検討すべき知見

---

## 【フェーズ完了条件】
- プロジェクトディレクトリ内の全ファイルがレビューされている
- 全ての知見について Knowledge 昇格判断が完了している
- Knowledge 化すべき知見が `/mnt/d/work/obsidian/cursor/20_KNOWLEDGE/` に昇格している
- 各Knowledgeファイルに「適用前提・ユースケース」が記入されている
- **重複チェックが完了し、既存Knowledgeとの重複がない（または前提条件の違いが明確化されている）**
- INDEX.md が更新されている
- Obsidianプロジェクトディレクトリが更新されている

---

## 【出力】
- レビュー対象ファイル一覧
- 抽出した知見の一覧
- Knowledge 化した知見の一覧（ファイルパス・カテゴリ・タイトル）
- Knowledge 化しなかった知見とその理由
- INDEX.md の更新内容
- 今後のレビューで検討すべき知見

---

## 【フェーズ宣言】
Knowledge Review & Promotion フェーズを開始する。
Knowledge Review & Promotion フェーズを終了する。
