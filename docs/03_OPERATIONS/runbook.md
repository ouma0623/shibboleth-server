# Runbook（運用・再現手順）

本ドキュメントは、本プロジェクトにおける
**運用手順・再現手順・障害時対応**を記録するための Runbook である。

- `scripts/operation/` 配下のスクリプトを追加・変更した場合は、**必ず本ドキュメントを更新する**
- `scripts/test/` 配下のスクリプトは `docs/03_OPERATIONS/runbook_test.md` を更新する
- 実行できない手順は未完成とみなす
- チェックリストや判断基準は rules / Issue に従う

---

## 0. 前提情報（共通）

- 対象プロジェクト：Shibboleth IdPサーバー構築（Phase 1: EC2構築・SSH接続）
- 対象リポジトリ：https://github.com/ouma0623/template
- 想定実行者：
  - 運用担当
  - 開発者
  - AI（Cursor + MCP）
- 対象環境：
  - テスト環境（dev相当）
- 関連リンク：
  - docs hub：docs/00_INDEX.md
  - requirements：docs/01_REQUIREMENTS/requirements.md
  - Plan Issue：#8
  - Obsidian：`/mnt/d/work/obsidian/cursor/10_PROJECTS/PJ001-2026-01/`

### 0.1 AWSリソース情報（2026-01-03作成）

- **VPC ID**：`vpc-0ab0d0b1cbef6e143`
  - CIDRブロック：10.0.0.0/16
  - リージョン：ap-northeast-1
- **インターネットゲートウェイ ID**：`igw-0c79b195fcd36988b`
  - VPCにアタッチ済み
- **サブネット ID**：`subnet-0fe72a919ba85c07f`
  - CIDRブロック：10.0.1.0/24
  - VPC：vpc-0ab0d0b1cbef6e143
- **ルートテーブル ID**：`rtb-0b41f74f4729ca834`
  - VPC：vpc-0ab0d0b1cbef6e143
  - 関連付けID：rtbassoc-01330067634427a20
- **セキュリティグループ ID**：`sg-055c544497e9dd59c`
  - グループ名：shibboleth-idp-sg
  - ポート22（SSH）のインバウンドルール設定済み（送信元：0.0.0.0/0）
- **SSHキーペア名**：`shibboleth-idp-key`
  - キーファイルパス：`~/.ssh/shibboleth-idp-key.pem`
  - キーファイルパーミッション：400（-r--------）
  - キーフィンガープリント：93:f4:a2:c8:12:93:a3:fc:1d:e0:a0:bd:3f:db:80:44:0f:0e:cf:56
- **EC2インスタンス ID**：`i-00b09d20ae84ede93`
  - インスタンスタイプ：t2.micro
  - AMI ID：ami-0a85e8e68d29c380f（Amazon Linux 2023）
  - パブリックIPアドレス：57.180.61.148
  - プライベートIPアドレス：10.0.1.159
  - サブネット：subnet-0fe72a919ba85c07f
  - セキュリティグループ：sg-055c544497e9dd59c
  - 状態：running

---

## 1. 通常運用フロー（概要）

- 通常時
  - 定常処理
  - バッチ実行
  - 状態確認
- 変更時
  - scripts 更新
  - runbook 更新
  - Issue に証跡を残す
- 障害時
  - 影響確認
  - 応急対応
  - ロールバックまたは恒久対応

---

## 2. AWSリソース作成手順（VPC・セキュリティグループ）

### 2.1 VPCとセキュリティグループの作成手順

#### 目的
- VPCとセキュリティグループを作成し、EC2インスタンスを配置するためのネットワーク基盤を構築する

#### 前提条件
- AWS CLIがインストールされ、認証情報が設定されている
- リージョン：ap-northeast-1

#### 実行方法

```bash
# 1. VPCを作成
VPC_ID=$(aws ec2 create-vpc --cidr-block 10.0.0.0/16 --region ap-northeast-1 --query 'Vpc.VpcId' --output text)
echo "VPC ID: $VPC_ID"

# 2. インターネットゲートウェイを作成
IGW_ID=$(aws ec2 create-internet-gateway --region ap-northeast-1 --query 'InternetGateway.InternetGatewayId' --output text)
echo "Internet Gateway ID: $IGW_ID"

# 3. インターネットゲートウェイをVPCにアタッチ
aws ec2 attach-internet-gateway --vpc-id $VPC_ID --internet-gateway-id $IGW_ID --region ap-northeast-1

# 4. パブリックサブネットを作成
SUBNET_ID=$(aws ec2 create-subnet --vpc-id $VPC_ID --cidr-block 10.0.1.0/24 --region ap-northeast-1 --query 'Subnet.SubnetId' --output text)
echo "Subnet ID: $SUBNET_ID"

# 5. ルートテーブルを作成
RT_ID=$(aws ec2 create-route-table --vpc-id $VPC_ID --region ap-northeast-1 --query 'RouteTable.RouteTableId' --output text)
echo "Route Table ID: $RT_ID"

# 6. インターネットへのルートを追加
aws ec2 create-route --route-table-id $RT_ID --destination-cidr-block 0.0.0.0/0 --gateway-id $IGW_ID --region ap-northeast-1

# 7. サブネットとルートテーブルを関連付け
aws ec2 associate-route-table --subnet-id $SUBNET_ID --route-table-id $RT_ID --region ap-northeast-1

# 8. セキュリティグループを作成
SG_ID=$(aws ec2 create-security-group --group-name shibboleth-idp-sg --description "Shibboleth IdP server security group" --vpc-id $VPC_ID --region ap-northeast-1 --query 'GroupId' --output text)
echo "Security Group ID: $SG_ID"

# 9. ポート22（SSH）のインバウンドルールを追加
aws ec2 authorize-security-group-ingress --group-id $SG_ID --protocol tcp --port 22 --cidr 0.0.0.0/0 --region ap-northeast-1
```

#### 期待結果
- VPC、インターネットゲートウェイ、サブネット、ルートテーブル、セキュリティグループが正常に作成される
- 各リソースIDが出力される

#### 確認方法

```bash
# VPCの確認
aws ec2 describe-vpcs --vpc-ids $VPC_ID --region ap-northeast-1

# インターネットゲートウェイの確認
aws ec2 describe-internet-gateways --internet-gateway-ids $IGW_ID --region ap-northeast-1

# サブネットの確認
aws ec2 describe-subnets --subnet-ids $SUBNET_ID --region ap-northeast-1

# セキュリティグループの確認
aws ec2 describe-security-groups --group-ids $SG_ID --region ap-northeast-1
```

#### 注意点
- 再実行可否：VPCは既に作成されているため、再実行するとエラーになる
- 二重実行リスク：同じCIDRブロックのVPCは作成できない
- 実行禁止時間帯：なし

---

## 3. scripts/operation（運用バッチ・スクリプト）

運用目的で実行するスクリプトを管理する。
新規作成・変更時は **必ず本章を更新する**。

### 3.1 一覧

| 名称 | パス | 目的 | 実行タイミング | 実行者 |
|---|---|---|---|---|
| | scripts/operation/ | | | |

---

### 3.2 実行手順

#### 対象スクリプト
- 名称：
- パス：scripts/operation/<name>

#### 目的
- 何のために実行するか：

#### 前提条件
- 実行環境：
- 必要な権限：
- 事前確認（データ / 状態）：

#### 実行方法

```bash
# 実行コマンド例
```

#### 期待結果
- 正常時の挙動：
- 出力 / ログ：

#### 注意点
- 再実行可否：
- 二重実行リスク：
- 実行禁止時間帯（あれば）：

---

### 3.3 ロールバック手順（必須）

#### ロールバックが必要になるケース
- VPCとセキュリティグループの作成に失敗した場合
- テスト完了後、リソースを削除する場合

#### 手順（VPCとセキュリティグループの削除）

```bash
# 注意：以下のコマンドは、VPC内の全てのリソース（EC2インスタンス等）を削除した後に実行すること

# 1. セキュリティグループのインバウンドルールを削除
aws ec2 revoke-security-group-ingress --group-id sg-055c544497e9dd59c --protocol tcp --port 22 --cidr 0.0.0.0/0 --region ap-northeast-1

# 2. セキュリティグループを削除
aws ec2 delete-security-group --group-id sg-055c544497e9dd59c --region ap-northeast-1

# 3. サブネットとルートテーブルの関連付けを解除
aws ec2 disassociate-route-table --association-id rtbassoc-01330067634427a20 --region ap-northeast-1

# 4. ルートテーブルを削除
aws ec2 delete-route-table --route-table-id rtb-0b41f74f4729ca834 --region ap-northeast-1

# 5. サブネットを削除
aws ec2 delete-subnet --subnet-id subnet-0fe72a919ba85c07f --region ap-northeast-1

# 6. インターネットゲートウェイをVPCからデタッチ
aws ec2 detach-internet-gateway --vpc-id vpc-0ab0d0b1cbef6e143 --internet-gateway-id igw-0c79b195fcd36988b --region ap-northeast-1

# 7. インターネットゲートウェイを削除
aws ec2 delete-internet-gateway --internet-gateway-id igw-0c79b195fcd36988b --region ap-northeast-1

# 8. VPCを削除
aws ec2 delete-vpc --vpc-id vpc-0ab0d0b1cbef6e143 --region ap-northeast-1
```

#### ロールバック後の確認方法
- `aws ec2 describe-vpcs --vpc-ids vpc-0ab0d0b1cbef6e143 --region ap-northeast-1` コマンドでVPCが存在しないことを確認する

---

### 2.2 SSHキーペアの作成手順

#### 目的
- SSH接続用のキーペアを作成し、EC2インスタンスへの接続に使用する

#### 前提条件
- AWS CLIがインストールされ、認証情報が設定されている
- リージョン：ap-northeast-1
- `~/.ssh/` ディレクトリが存在する（存在しない場合は自動作成される）

#### 実行方法

```bash
# 1. ~/.ssh/ ディレクトリを作成（存在しない場合）
mkdir -p ~/.ssh
chmod 700 ~/.ssh

# 2. 既存のキーペアを確認
aws ec2 describe-key-pairs --key-names shibboleth-idp-key --region ap-northeast-1

# 3. キーペアが存在しない場合、新規作成
aws ec2 create-key-pair --key-name shibboleth-idp-key --region ap-northeast-1 --query 'KeyMaterial' --output text > ~/.ssh/shibboleth-idp-key.pem

# 4. キーファイルのパーミッションを400に設定
chmod 400 ~/.ssh/shibboleth-idp-key.pem

# 5. キーペアの確認
aws ec2 describe-key-pairs --key-names shibboleth-idp-key --region ap-northeast-1

# 6. キーファイルのパーミッション確認
ls -l ~/.ssh/shibboleth-idp-key.pem
```

#### 期待結果
- キーペアが正常に作成される
- キーファイルが `~/.ssh/shibboleth-idp-key.pem` に保存される
- キーファイルのパーミッションが400（-r--------）に設定される

#### 確認方法

```bash
# キーペアの確認
aws ec2 describe-key-pairs --key-names shibboleth-idp-key --region ap-northeast-1

# キーファイルのパーミッション確認
ls -l ~/.ssh/shibboleth-idp-key.pem
```

#### 注意点
- 再実行可否：既存のキーペアがある場合はエラーになる（既存のキーペアを使用する場合は、キーファイルの存在とパーミッションを確認する）
- 二重実行リスク：同じ名前のキーペアは作成できない
- 実行禁止時間帯：なし
- **重要**：キーファイルは機密情報のため、リポジトリにコミットしないこと

---

### 2.3 EC2インスタンスの作成手順

#### 目的
- t2.microインスタンスタイプでEC2インスタンスを作成し、SSH接続可能な状態にする

#### 前提条件
- AWS CLIがインストールされ、認証情報が設定されている
- リージョン：ap-northeast-1
- VPC、サブネット、セキュリティグループ、SSHキーペアが作成済みであること

#### 実行方法

```bash
# 1. Amazon Linux 2023の最新AMI IDを取得
AMI_ID=$(aws ec2 describe-images --owners amazon --filters "Name=name,Values=al2023-ami-*" "Name=architecture,Values=x86_64" --query 'Images | sort_by(@, &CreationDate) | [-1].ImageId' --output text --region ap-northeast-1)
echo "AMI ID: $AMI_ID"

# 2. EC2インスタンスを作成
INSTANCE_ID=$(aws ec2 run-instances \
  --image-id $AMI_ID \
  --instance-type t2.micro \
  --key-name shibboleth-idp-key \
  --security-group-ids sg-055c544497e9dd59c \
  --subnet-id subnet-0fe72a919ba85c07f \
  --associate-public-ip-address \
  --region ap-northeast-1 \
  --query 'Instances[0].InstanceId' \
  --output text)
echo "Instance ID: $INSTANCE_ID"

# 3. インスタンスの起動を待つ（約30秒）
echo "Waiting for instance to start..."
sleep 30

# 4. インスタンスの状態とIPアドレスを確認
aws ec2 describe-instances --instance-ids $INSTANCE_ID --region ap-northeast-1 --query 'Reservations[0].Instances[0].{InstanceId:InstanceId,State:State.Name,PublicIpAddress:PublicIpAddress,PrivateIpAddress:PrivateIpAddress}' --output json

# 5. インスタンスの起動ログを確認（オプション）
aws ec2 get-console-output --instance-id $INSTANCE_ID --region ap-northeast-1 --query 'Output' --output text
```

#### 期待結果
- インスタンスIDが取得できる
- インスタンスの状態が「running」である
- パブリックIPアドレスが割り当てられている
- プライベートIPアドレスが割り当てられている

#### 確認方法

```bash
# インスタンスの状態確認
aws ec2 describe-instances --instance-ids $INSTANCE_ID --region ap-northeast-1

# パブリックIPアドレスの取得
aws ec2 describe-instances --instance-ids $INSTANCE_ID --region ap-northeast-1 --query 'Reservations[0].Instances[0].PublicIpAddress' --output text

# プライベートIPアドレスの取得
aws ec2 describe-instances --instance-ids $INSTANCE_ID --region ap-northeast-1 --query 'Reservations[0].Instances[0].PrivateIpAddress' --output text
```

#### 注意点
- 再実行可否：同じ設定で再実行すると、新しいインスタンスが作成される
- 二重実行リスク：複数のインスタンスが作成される可能性がある
- 実行禁止時間帯：なし
- **重要**：インスタンスは起動に時間がかかるため、状態が「running」になるまで待つこと

---

### 2.4 SSH接続の疎通確認手順

#### 目的
- CursorのターミナルからSSH接続を実行し、EC2インスタンス内でコマンドを実行できることを確認する

#### 前提条件
- EC2インスタンスが起動している（状態が「running」）
- パブリックIPアドレスが割り当てられている
- SSHキーファイル（~/.ssh/shibboleth-idp-key.pem）が存在し、パーミッションが400である

#### 実行方法

```bash
# 1. SSH接続を実行し、コマンドを実行
ssh -i ~/.ssh/shibboleth-idp-key.pem ec2-user@57.180.61.148 "whoami && hostname && uname -a"

# 2. インタラクティブなSSH接続（オプション）
ssh -i ~/.ssh/shibboleth-idp-key.pem ec2-user@57.180.61.148

# 3. SSH接続後、EC2インスタンス内でコマンドを実行
# whoami
# hostname
# uname -a
# exit
```

#### 期待結果
- SSH接続が成功する
- `whoami` コマンドの結果が「ec2-user」である
- `hostname` コマンドの結果がホスト名を返す
- `uname -a` コマンドの結果がLinuxカーネル情報を返す

#### 確認方法

```bash
# SSH接続の確認（コマンドをリモート実行）
ssh -i ~/.ssh/shibboleth-idp-key.pem ec2-user@57.180.61.148 "whoami"

# 期待結果：ec2-user
```

#### 注意点
- 再実行可否：何度でも実行可能
- 二重実行リスク：なし
- 実行禁止時間帯：なし
- **重要**：初回接続時はホストキーの確認が表示されるが、`-o StrictHostKeyChecking=no` オプションを使用することでスキップできる（テスト環境の場合）

---

## 4. scripts/test（テスト実行用スクリプト）

**注意**：`scripts/test/` 配下のテスト実行用スクリプトは、`docs/03_OPERATIONS/runbook_test.md` で管理します。  
本セクションではなく、`runbook_test.md` を参照してください。

---

## 5. 障害対応（トラブルシューティング）

実際に発生した事象をベースに追記していく。

### 5.1 既知の事象

| 発生条件 | 症状 | 原因 | 対応 |
|---|---|---|---|
| | | | |

---

### 5.2 調査手順

1. 影響範囲の確認
2. ログ確認箇所：
3. 一次対応：
4. 恒久対応（必要なら）：

---

## 6. 変更履歴・関連

- 変更履歴：docs/01_REQUIREMENTS/changes.md
- 関連 Issue：
  - Plan：#8
  - Task：#9（VPCとセキュリティグループの作成）
- 関連 PR：なし

---

## 7. メモ（任意）

- 環境依存事項：
- 将来の改善案：
