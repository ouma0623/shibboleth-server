# Requirements（要件定義 / SSOT）

> このドキュメントは本PJの **要件の唯一の正（SSOT）** である。  
> 仕様・範囲・Done（完了条件）をここで確定し、実装や検証はこの内容に従う。  
> 変更が発生する場合は `/plan` に差し戻し、本ファイルを更新した上で進めること。

---

## 0. メタ情報（プロジェクト固有）
- プロジェクト名：Shibboleth IdPサーバー構築（Phase 1: EC2構築・SSH接続）
- 対象期間（任意）：2026-01-03 〜
- オーナー（任意）：
- 最終更新日：2026-01-03
- 関連（リンク）
  - docs hub：`docs/00_INDEX.md`
  - Plan Issue：#8 - https://github.com/ouma0623/template/issues/8
  - Obsidian：`/mnt/d/work/obsidian/cursor/10_PROJECTS/PJ001-2026-01/`
  - Github Repository：https://github.com/ouma0623/template

---

## 1. 背景（Background）
- 現状の課題：
  - Shibboleth認証を導入したIdPサーバーを構築する必要があるが、まずはテスト環境としてEC2インスタンスを構築し、SSH接続可能な状態にする必要がある
- 何が困っているか：
  - EC2インスタンスの構築とSSH接続の手順が不明確
  - CursorからEC2内で操作できる環境が整っていない
- なぜ今やるか（機会・リスク）：
  - 次のフェーズでShibboleth IdPサーバーの構築とテストを行うための基盤を整備する必要がある
  - テスト環境を早期に構築することで、後続の作業を効率的に進められる

---

## 2. 目的（Goal / Why）
- 達成したい状態：
  - AWS CLIを使用してVPCとEC2インスタンス（t2.micro）を構築し、SSH接続可能な状態にする
  - CursorのターミナルからSSH接続してEC2内でコマンドを実行できる環境を構築する
- 成果の使われ方（誰が/いつ/何のために）：
  - 開発者が次のフェーズでShibboleth IdPサーバーを構築・テストするための基盤として使用する
  - テスト用の環境として、安価に運用できる構成とする

---

## 3. スコープ（Scope）
> Planで確定し、Do/Checkで増やさない。

### 3.1 対象（In Scope）
- 対象機能：
  - AWS CLIを使用したVPCの作成（CIDRブロック、インターネットゲートウェイ、ルートテーブル、サブネットの設定）
  - AWS CLIを使用したEC2インスタンス（t2.micro）の作成
  - セキュリティグループの設定（SSH接続用ポート22の開放）
  - SSHキーペアの作成または既存キーペアの使用
  - SSH接続の疎通確認
  - CursorターミナルからのSSH接続確認
- 対象データ：
  - AWSリソースの設定情報（VPC ID、サブネット ID、インスタンス ID、パブリック IP など）
  - SSHキーペア情報（キーファイルのパス）
- 対象画面/API：
  - AWS CLIコマンド実行
  - SSH接続（ターミナル）
- 対象環境（dev/stg/prodなど）：
  - テスト環境（dev相当）
- 対象リポジトリ/ディレクトリ：
  - `/home/oumasan/work/shibboleth-server`
  - `scripts/operation/`（AWS CLI実行用スクリプト）
  - `docs/03_OPERATIONS/runbook.md`（運用手順）

### 3.2 対象外（Out of Scope）
- 今回やらないこと：
  - Shibboleth IdPサーバーのインストール・設定（次のフェーズで実施）
  - 本番環境の構築
  - CloudFormation/CDKなどのIaCツールの使用
  - 自動化スクリプトの作成（手動実行を前提）
  - 監視・アラートの設定
  - バックアップ・災害対策の設定
- 将来対応に回すこと：
  - Shibboleth IdPサーバーの構築・設定
  - IdPサーバーのテスト・検証
  - 本番環境への展開

---

## 4. ユースケース / 利用シナリオ（Use Cases）
> 「誰が」「何を」「どうしたい」を簡潔に列挙する。

- UC-01：VPCとEC2インスタンスの作成
  - 利用者：開発者
  - 操作/入力：AWS CLIコマンドを実行してVPCとEC2インスタンスを作成する
  - 期待結果：VPCとEC2インスタンスが正常に作成され、パブリックIPが割り当てられる

- UC-02：SSH接続の確認
  - 利用者：開発者
  - 操作/入力：CursorのターミナルからSSH接続コマンドを実行する
  - 期待結果：EC2インスタンスにSSH接続でき、EC2内でコマンドを実行できる

---

## 5. 要件（Requirements）

### 5.1 機能要件（Functional）
> 実装対象となる要件。曖昧さを残さない。

- FR-01：VPCの作成
  - 説明：AWS CLIを使用してVPCを作成する。CIDRブロックは10.0.0.0/16を使用する。インターネットゲートウェイを作成し、VPCにアタッチする。パブリックサブネット（10.0.1.0/24）を作成し、ルートテーブルを設定してインターネットへのルーティングを有効にする。
  - 入力：
    - AWS CLI認証情報（既に設定済み）
    - AWSリージョン（ap-northeast-1を想定、変更可能）
    - VPC CIDRブロック：10.0.0.0/16
    - サブネットCIDRブロック：10.0.1.0/24
  - 出力：
    - VPC ID（例：vpc-xxxxxxxxx）
    - インターネットゲートウェイ ID（例：igw-xxxxxxxxx）
    - サブネット ID（例：subnet-xxxxxxxxx）
    - ルートテーブル ID（例：rtb-xxxxxxxxx）
  - 例外/異常系：
    - AWS CLI認証情報が設定されていない場合：エラーメッセージを表示し、設定を促す
    - VPC作成に失敗した場合：エラーメッセージを確認し、リソースのクリーンアップを実施する
    - 既存のVPCとCIDRブロックが重複する場合：エラーメッセージを表示し、CIDRブロックを変更する
  - 受け入れ条件（Acceptance）：
    - `aws ec2 describe-vpcs --vpc-ids <VPC_ID>` コマンドでVPCが存在することを確認できる
    - `aws ec2 describe-internet-gateways --internet-gateway-ids <IGW_ID>` コマンドでインターネットゲートウェイがVPCにアタッチされていることを確認できる
    - `aws ec2 describe-subnets --subnet-ids <SUBNET_ID>` コマンドでサブネットがVPC内に作成されていることを確認できる

- FR-02：セキュリティグループの作成
  - 説明：SSH接続用のセキュリティグループを作成する。インバウンドルールでポート22（SSH）を開放する。送信元は0.0.0.0/0（任意のIPアドレス）を許可する（テスト環境のため）。
  - 入力：
    - VPC ID（FR-01で作成したVPC ID）
    - セキュリティグループ名：shibboleth-idp-sg
    - 説明：Shibboleth IdP server security group
    - インバウンドルール：ポート22、プロトコルTCP、送信元0.0.0.0/0
  - 出力：
    - セキュリティグループ ID（例：sg-xxxxxxxxx）
  - 例外/異常系：
    - VPC IDが存在しない場合：エラーメッセージを表示し、FR-01の実行を促す
    - セキュリティグループ作成に失敗した場合：エラーメッセージを確認し、リソースのクリーンアップを実施する
  - 受け入れ条件（Acceptance）：
    - `aws ec2 describe-security-groups --group-ids <SG_ID>` コマンドでセキュリティグループが存在し、ポート22のインバウンドルールが設定されていることを確認できる

- FR-03：SSHキーペアの作成または確認
  - 説明：SSH接続用のキーペアを作成する。既存のキーペアがある場合はそれを使用する。キーファイルは `~/.ssh/` ディレクトリに保存する。
  - 入力：
    - キーペア名：shibboleth-idp-key（既存の場合は既存のキーペア名を指定）
    - キーファイル保存先：~/.ssh/shibboleth-idp-key.pem
  - 出力：
    - キーペア名（例：shibboleth-idp-key）
    - キーファイルのパス（例：~/.ssh/shibboleth-idp-key.pem）
  - 例外/異常系：
    - キーペア作成に失敗した場合：エラーメッセージを確認し、権限を確認する
    - 既存のキーペアを使用する場合、キーファイルが存在しない場合：エラーメッセージを表示し、キーファイルのパスを確認する
  - 受け入れ条件（Acceptance）：
    - `aws ec2 describe-key-pairs --key-names <KEY_NAME>` コマンドでキーペアが存在することを確認できる
    - キーファイルが `~/.ssh/` ディレクトリに存在し、パーミッションが400（-r--------）に設定されていることを確認できる

- FR-04：EC2インスタンスの作成
  - 説明：t2.microインスタンスタイプでEC2インスタンスを作成する。OSはAmazon Linux 2023を使用する。作成したVPCのパブリックサブネットに配置し、作成したセキュリティグループを適用する。パブリックIPアドレスの自動割り当てを有効にする。
  - 入力：
    - インスタンスタイプ：t2.micro
    - AMI ID：Amazon Linux 2023の最新AMI ID（ap-northeast-1リージョン）
    - サブネット ID（FR-01で作成したサブネット ID）
    - セキュリティグループ ID（FR-02で作成したセキュリティグループ ID）
    - キーペア名（FR-03で作成または確認したキーペア名）
  - 出力：
    - インスタンス ID（例：i-xxxxxxxxx）
    - パブリックIPアドレス（例：54.xxx.xxx.xxx）
    - プライベートIPアドレス（例：10.0.1.xxx）
  - 例外/異常系：
    - AMI IDが取得できない場合：エラーメッセージを確認し、リージョンを確認する
    - インスタンス作成に失敗した場合：エラーメッセージを確認し、リソースのクリーンアップを実施する
    - インスタンスが起動しない場合：インスタンスの状態を確認し、ログを確認する
  - 受け入れ条件（Acceptance）：
    - `aws ec2 describe-instances --instance-ids <INSTANCE_ID>` コマンドでインスタンスが存在し、状態が「running」であることを確認できる
    - パブリックIPアドレスが割り当てられていることを確認できる
    - `aws ec2 get-console-output --instance-id <INSTANCE_ID>` コマンドでインスタンスの起動ログが正常であることを確認できる

- FR-05：SSH接続の疎通確認
  - 説明：CursorのターミナルからSSH接続を実行し、EC2インスタンス内でコマンドを実行できることを確認する。
  - 入力：
    - SSH接続コマンド：`ssh -i ~/.ssh/shibboleth-idp-key.pem ec2-user@<PUBLIC_IP>`
    - 実行するコマンド：`whoami`、`hostname`、`uname -a`
  - 出力：
    - SSH接続が成功し、EC2インスタンス内でコマンドが実行できる
    - `whoami` コマンドの結果：ec2-user
    - `hostname` コマンドの結果：インスタンスのホスト名
    - `uname -a` コマンドの結果：Linuxカーネル情報
  - 例外/異常系：
    - SSH接続がタイムアウトする場合：セキュリティグループの設定を確認する
    - 認証エラーが発生する場合：キーファイルのパーミッションを確認する（400である必要がある）
    - 接続拒否エラーが発生する場合：インスタンスの起動状態を確認する
  - 受け入れ条件（Acceptance）：
    - `ssh -i ~/.ssh/shibboleth-idp-key.pem ec2-user@<PUBLIC_IP>` コマンドでSSH接続が成功する
    - EC2インスタンス内で `whoami` コマンドを実行し、「ec2-user」が返される
    - EC2インスタンス内で `hostname` コマンドを実行し、ホスト名が返される
    - EC2インスタンス内で `uname -a` コマンドを実行し、Linuxカーネル情報が返される

---

### 5.2 非機能要件（Non-Functional）
> 実務で崩れやすいので必ず書く。分からなければ仮置きして明示する。

#### 性能（Performance）
- 期待性能：
  - EC2インスタンスの起動時間：5分以内（AWSの標準的な起動時間）
  - SSH接続の応答時間：3秒以内（ネットワーク状況による）
- 目標レスポンス：
  - SSH接続確立時間：3秒以内
- 許容バッチ時間（該当時）：
  - 該当なし（手動実行を前提）

#### 可用性（Availability）
- 目標稼働：
  - テスト環境のため、可用性要件は特に設定しない
  - インスタンスは手動で起動・停止する
- 障害時の許容：
  - インスタンスが起動しない場合、ログを確認して原因を特定する
  - 必要に応じてインスタンスを再作成する

#### セキュリティ（Security）
- 認証/認可：
  - AWS CLI認証情報を使用（既に設定済み）
  - SSH接続はキーペア認証を使用
- 取り扱う機密情報：
  - SSHキーファイル（~/.ssh/shibboleth-idp-key.pem）
  - AWS認証情報（環境変数または設定ファイル）
- ログに残してよい/悪い情報：
  - 残してよい：VPC ID、サブネット ID、インスタンス ID、パブリックIPアドレス（一時的なテスト環境のため）
  - 残してはいけない：SSHキーファイルの内容、AWS認証情報（アクセスキー、シークレットキー）

#### 運用（Operations）
- 監視/アラート（必要なら）：
  - テスト環境のため、監視・アラートは設定しない
  - 必要に応じて `aws ec2 describe-instance-status` コマンドでインスタンスの状態を確認する
- ロールバック方針：
  - インスタンス作成に失敗した場合、作成済みのリソース（VPC、セキュリティグループ、キーペア）を削除する
  - リソース削除手順は `docs/03_OPERATIONS/runbook.md` に記載する
- 運用手順の格納先：`docs/03_OPERATIONS/runbook.md`

#### 監査/証跡（Audit）
- Issueに残すべき証跡：
  - AWS CLIコマンドの実行結果（VPC ID、サブネット ID、インスタンス ID、パブリックIPアドレス）
  - SSH接続の確認結果（接続成功の証跡）
  - エラーが発生した場合のエラーメッセージと対処方法
- Obsidianに残すべきまとめ：
  - 構築手順のまとめ
  - 発生した問題と対処方法
  - 次フェーズへの引き継ぎ事項

---

## 6. データ要件（Data Requirements）
- データソース：
  - AWS CLIコマンドの実行結果（JSON形式）
  - SSH接続の確認結果（ターミナル出力）
- 正規化/変換（必要なら）：
  - AWS CLIコマンドの結果から必要な情報（VPC ID、サブネット ID、インスタンス ID、パブリックIPアドレス）を抽出する
- 保存先：
  - リソースIDは `docs/03_OPERATIONS/runbook.md` に記載する
  - SSHキーファイルは `~/.ssh/` ディレクトリに保存する
- データ品質（欠損/重複/フォーマット）：
  - AWS CLIコマンドの実行結果が正しい形式（JSON）であることを確認する
  - リソースIDが正しく取得できていることを確認する

---

## 7. 制約（Constraints）
- 技術的制約：
  - AWS CLIが既に設定済みであること（認証情報が利用可能）
  - CloudFormation/CDKなどのIaCツールは使用しない（AWS CLIのみ）
  - インスタンスタイプはt2.micro（テスト用で安価）
  - OSはAmazon Linux 2023を使用
- 期限/コスト制約：
  - テスト環境のため、コストを最小限に抑える（t2.microインスタンスを使用）
  - インスタンスは使用しない場合は停止または削除する
- 外部依存（API、権限、第三者）：
  - AWSアカウントへのアクセス権限が必要
  - VPC、EC2、セキュリティグループの作成権限が必要
  - インターネット接続が必要（SSH接続のため）

---

## 8. 受け入れ基準（Acceptance Criteria）
> Checkフェーズで「OK/NG」を判断するための基準。  
> "動いた気がする" を排除する。

- AC-01：VPCとEC2インスタンスが正常に作成されている
  - 判定方法：`aws ec2 describe-vpcs --vpc-ids <VPC_ID>` と `aws ec2 describe-instances --instance-ids <INSTANCE_ID>` コマンドでリソースが存在し、インスタンスの状態が「running」であることを確認する
  - 期待結果：VPC ID、サブネット ID、インスタンス ID、パブリックIPアドレスが取得できる

- AC-02：セキュリティグループが正常に設定されている
  - 判定方法：`aws ec2 describe-security-groups --group-ids <SG_ID>` コマンドでセキュリティグループが存在し、ポート22のインバウンドルールが設定されていることを確認する
  - 期待結果：セキュリティグループ IDが取得でき、ポート22のインバウンドルールが設定されている

- AC-03：SSHキーペアが正常に作成または確認されている
  - 判定方法：`aws ec2 describe-key-pairs --key-names <KEY_NAME>` コマンドでキーペアが存在することを確認し、キーファイルが `~/.ssh/` ディレクトリに存在し、パーミッションが400であることを確認する
  - 期待結果：キーペア名が取得でき、キーファイルが存在し、パーミッションが400である

- AC-04：EC2インスタンスにパブリックIPアドレスが割り当てられている
  - 判定方法：`aws ec2 describe-instances --instance-ids <INSTANCE_ID>` コマンドでパブリックIPアドレスが割り当てられていることを確認する
  - 期待結果：パブリックIPアドレスが取得できる（例：54.xxx.xxx.xxx）

- AC-05：CursorのターミナルからSSH接続が成功し、EC2インスタンス内でコマンドを実行できる
  - 判定方法：`ssh -i ~/.ssh/shibboleth-idp-key.pem ec2-user@<PUBLIC_IP>` コマンドでSSH接続を実行し、EC2インスタンス内で `whoami`、`hostname`、`uname -a` コマンドを実行する
  - 期待結果：SSH接続が成功し、`whoami` コマンドの結果が「ec2-user」、`hostname` と `uname -a` コマンドの結果が正常に返される

---

## 9. Done（完了条件：Planで確定）
> `/action` で Plan Issue をCloseできる条件。

- [ ] Plan Issue の子Taskが全て Close されている
- [ ] 受け入れ基準（Acceptance Criteria）を満たす
- [ ] 変更履歴（`docs/01_REQUIREMENTS/changes.md`）が更新されている
- [ ] 運用手順（`docs/03_OPERATIONS/runbook.md`）が更新されている
- [ ] AI Knowledge（該当時）が更新されている
- [ ] Obsidian プロジェクトディレクトリが作成されている

---

## 10. 変更方針（Change Policy）
- 仕様変更が必要になった場合：
  - `/plan` に差し戻して requirements を更新する
  - 変更内容は `docs/01_REQUIREMENTS/changes.md` に記録する
  - 影響がある Task は Plan Issue 側で再整理する（Task増殖はしない）

---

## 11. メモ（任意）
- 未確定事項：
  - AWSリージョンはap-northeast-1を想定しているが、変更可能
- 決定待ち：
  - なし
