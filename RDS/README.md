# RDS

**RDSとは**

- 構築
  - APIでDBサーバを操作可能
  - 時間単位の従量課金
- 親和性
  - 6種類のエンジンをサポート
    - MySQL / PostgeSQL / SQL_server / Oracle / MariaDB / Aurora
- 運用
  - 可溶性向上のための機能
  - モニタリング、障害検知/復旧 バッチ、スケーリングが容易
- セキュリティ 
  - VPC、セキュリティグループ、暗号化に対応
 
- 必須機能が実装済み
  - バックアップ
    - 自動バックアップ
    - 手動バックアップ
  - 同期レプリケーション
  - 監視(CloudWatch)
  - 管理GUIやAPIで操作可能

- インスタンスクラスの選択
  - m4:標準インスタンス
  - r3はメモリを多めに搭載したインスタンス
  - t2:は小規模用インスタンス
  
**リードレプリカ(RR)**

- 読み取り専用のレプリカDB
  - 5台まで増設可能 (※ 上限緩和申請可能, Auroraは15台まで)
  - マルチAZとの組み合わせも可能
  - マスター昇格
  - RRのディスクタイプやインスタンスタイプをソースとは別のタイプに変更可能
 
- 想定ユースケース
  - 読み取りのスケーリング、BI等の解析処理の分散
  - マルチAZによる耐障害性の代替ではない
 
- 対応DBエンジン
  - MySQL
  - PostgreSQL
  - MariaDB

- MySQL / MariaDB / PostgreSQLの機能
  - クロスリージョンレプリケーション(リージョンをまたいだレプリケーション)
  - RRのカスケード
  - リードレプリカ側でのスナップショット実行
 
**スケールアップ機能** 

- マネージメントコンソールやAPIからスケールアップ可能
  - rebootを伴う
  - AWS-CLIからも可能
- スケールダウンも可能
  - 一時的に大きくして、その後戻す運用も可能
- インスタンスの変更でCPUとメモリだけでなく、ディスクIO帯域やネットワーク帯域が変更になる


**RDSで使用できるディスクボリュームタイプ**
 
- 標準(マグネティック) IOPS:無し IOリクエスト課金：あり
- General Purpose(GP2) SSD IOリクエスト課金：無し　　高性能 + バースト
- プロビジョンド IOPS(PIOPS) SSD IOリクエスト課金：無し　　高性能
 ※ 小さなインスタンスタイプではストレージと帯域不足で設定したIOPSに達しない場合がある
 
**自動スナップショットとリストア**

- RDS標準機能として自動的なバックアップを提供
  - 自動スナップショット + トランザクションログをs3に保存
- 自動スナップショット
  - 1日1回自動取得(時間指定可能)
  - 保存期間は最大35日文
  - 手動スナップショットは任意の時間に可能
  
- リストア方法
  - リストア: スナップショットを元にDBインスタンスを作成
  - Point in TIme Recovery
    - 指定した時刻(5分以前)の状態になるようにDNインスタンス作成

- スナップショットのユースケース
  - スナップショットのリージョン間コピー
    - 別リージョンにスナップショットをコピー可能
    - 別リージョンで、スナップショットからインスタンス起動可能
    
- 補足
  - 自動スナップショットは、利用インスタンスディスクサイズと同じサイズまでディスクコストが無料で利用切る
  - 自動スナップショットは、DBインスタンスを削除すると同時に削除される
  - スナップショット実行時に短時間IOが停止する
　　　　　　　　 - マルチ AZ構成であれば、スナップショットがスレーブから取得されるのでアプリケーションへの影響がない

**設定変更**

- RDSサーバには直接SSHログインできない
- 設定変更はパラメータグループ
- オプション機能の追加は、オプショングループ

**ログアクセス機能**

- 各種ログを直接参照する機能
  - API経由でダウンロード or マネジメントコンソールで表示
  
- PostgreSQL ログ種別無し　保存期：7日間
- MySQL / MariaDB  ログ種別：Error Slow_Query General  保存期間：24時間
- Oracle           ログ種別：Alr]ert Trace             保存期間：7日間　
- SQL_Server       ログ種別：Error Agent Trace         保存期間：7日間

 - ソフトウェアメンテナンス
   - メンテナンスウィンドウで指定した曜日、時間帯を自動実施
   - CloudWatch対応(監視)
     - ホスト層のメトリクス（CPU, Memory Usage）
     - ストレージ のメトリクス(IOPS, Queue Depth)
     - NWのメトリクス(受信スループット、送信スループット)
     - イベント通知機能(RDSで発生した40以上のイベントをSNS経由で通知) 
   - VPC対応 
     - VPC内部の任意のサブネットで起動可能
   - アクセス制御
     - Security Groupで実現
   - DBインスタンス暗号化機能
   　- ディスク 上に暗号化されたデータを保存
     - AES-256
     - AWS-KMSで鍵管理が可能
　 - 各種制限と緩和申請
     - RDSインスタンス数 40
     - 1マスターあたりのリードレプリカ数 5
     - 手動スナップショット数 50
     - ストレージ総量 100TB
     
**キャッシュウォーミング機能**

- InooDBバッファプールのダンプ/リストア
  - 終了時にバッファプールをファイルにダンプし起動時に読み込む
  - 起動直後のDBに対するアクセス性能劣化を防止
  - キャッシュを復元
  - フェイルオーバー、メンテナンス

**価格モデル**

- オンデマンドDBインスタンス
- Amazon RDSリザーブドインスタンス
