# IAM

- AWS操作をよりセキュアに行うための認証・認可の仕組み
- AWS操作のためのグループ・ユーザ・ロールの作成が可能
- グループ・ユーザ毎に実行できる操作を規定できる
- ユーザ毎に認証情報の設定が可能

- AWSのroot権限が必要な操作の例
 - AWSルートアカウントのメールアドレス・パスワードの変更
 - IAMユーザの課金情報へのアクセスに関するactivate/deactivate
 - 他のAWSアカウントへのRoute53のドメイン登録の移行
 - CloudFrontのキーペアの作成
 - AWSアカウントの停止
 - 脆弱性診断フォームの提出
 ‐ 逆引きDNS申請
 
**IAMグループ**

- IAMユーザをまとめるグループは1AWSアカウントで100グループまで作成可能
- グループに設定可能な情報
  - グループ名 / パス(組織) / パーミッション
  
- 強度の 強いパスワードポリシーの設定が可能
  - パスワードの長さ、有効期限、特殊文字の要求など
 
- アクセス条件の記述
  - Effect / Action(操作) / Resource / Condition(条件)
- Action
  - ec2:runInstances
  - ec2:AttachVolume
  - s3:CreateBucket
  - s3:DeleteObject
  - ec2:Describe"(ワイルドカード)
  - NotAction:iam:* (IAMの操作以外を許可する)
  
- Resource
  - EC2インスタンス
  - EBSボリューム
  - ARN(Amazon Resource Name)
    - arn:aws:で始まる文字列
    - arn:aws:service:region:account:resource
    -> arn:aws:s3:::mybucket
   
- Condition
  - Resourceに対するActionを許可、拒否するかの条件判定
  - ポリシー変数(条件キー)に対して、演算子を用いて条件を指定
  
- IAMロールによるクロスアカウントアクセス
  - あるアカウントのロールを別のアカウントのIAMロールに紐づける機能
 
