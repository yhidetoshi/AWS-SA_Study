# CloudFront

- Contents Delivery Network
 - 大規模なアクセスも世界中にあるエッジのキャパシティを活用して効率的かつ高速なコンテンツ配信が可能
  - ユーザからのアクセスを最も近いエッジサーバに誘導することでユーザへの配信を高速化
  - エッジサーバでは、コンテンツのキャッシングを行い、オリジンに負荷をかけずに効率的に配信
  
- Web Distribution
 - サポートプロトコル/ HTTPメソッド
  - HTTP / HTTPS対応
  - オリジンへのアクセス

- エッジでのGzip圧縮機能
 - CloudFrontエッジでコンテンツをGzip圧縮することでより高速にコンテンツを配信
 
- キャッシュコントロール機能
 - GET / HEAD / OPTIONのリクエストが対象
 - 単一ファイルサイズのキャッシングは最大20GBまで
 - URLパス毎にキャッシュ期間指定が可能
 - フォワードオプション機能による動的ページは新

- サポートするSSL証明書
 - デフォルト証明書
  - cloudfront.netドメインのSSL証明書は標準で利用可能
 - 独自SSL証明書
  - CloudFrontにて別途SSL証明書の利用課金がされる
  - AWS Certification Managerで発行された証明書
 - SNI(Server Name Indication)独自SSL証明書
  - CloudFrontの独自SSL証明書費用を負担せず、独自ドメインでのSSL通信が可能
- オリジン暗号化通信
 - CloudFrontエッジとオリジン間の通信方式を制御可能

- GEOリストリクション
 - 地域指定にゆるアクセス制御
  - 接続されるクライアントの地域情報を元にエッジでアクセス判定
  - BlacklistもしくはWhitelistで指定可能
  - Distribution全体に対して適用される
  - 制限されたアクセスには403を応答
  
- 証明付きURL/Cookie
 - Restricted Viewer Accessを有効にするだけで、署名のないアクセスを全てブロック
 - 標準(Canned Policy)
  - 有効期限 / 有効コンテンツパス
 - オプション
  - アクセス元IPアドレス制限
  - 有効開始時刻指定
  - 許可コンテンツのワイルドカード指定

- オリジンサーバの保護　
 - s3の場合、Origin Access Identity(OAI)を利用
 - オリジンカスタムヘッダーを利用し、CloudFrontで指定された任意のヘッダーをオリジン側でチェック
 - オリジン側のアドレスを公開しないとともにCloudFrontが利用するIPアドレスのみを許可させる
  - https://ip-ranges.amzonaws.com/ip-ranges.json
  - Jsonフォーマット

- Amazon WAF連携
 - Amazon WAFで定義したWeb ACLをCloudFornt Distributionに適用
 
- ストリーミング配信機能
 - 配信規模に応じて多くのネットワーク帯域が必要となるストリーミングを効率的に配信可能
 - 小規模から大規模配信まで柔軟に対応
- 対応可能な配信方式
 - Amazon s3をコンテンツストレージとしたオンデマンドストリーミング配信
 - ストリーミングサーバと連携したHTTPベースのストリーミング配信(オンデマンド・ライブ双方対応)