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
 
