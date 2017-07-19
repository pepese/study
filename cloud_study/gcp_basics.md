- AWS との対比
- GCP（Google Cloud Platform）のサービス概要

# AWS との対比

|カテゴリ|サービス|Amazon Web Services|Google Cloud Platform|
|:---|:---|:---|:---|
|compute|IaaS|Amazon EC2|Google Compute Engine|
||Containers|Amazon EC2 Container Service|Google Container Engine|
||Container Registry|Amazon EC2 Container Registry|Container Registry|
||PaaS|Amazon Lightsail|Google Cloud Launcher|
||PaaS|AWS Elastic Beanstalk|Google App Engine|
||Serverless|AWS Lambda|Google Cloud Functions|
||Batch|AWS Batch|-|
|Storage|Object Storage|Amazon S3|Google Cloud Storage|
||Block Storage|Amazon Elastic Block Store|Google Compute Engine Persistent Disks|
||Cold Storage|Amazon Glacier|Google Cloud Storage Nearline|
||File Storage|Amazon Elastic File System|Avere vFXT / GlusterFS|
||Gateway|AWS Storage Gateway|-|
|Database|RDBMS|Amazon RDS|Google Cloud SQL|
||Scaled RDBMS|-|Google Cloud Spanner|
||NoSQL: Key-value|Amazon DynamoDB|Google Cloud Bigtable|
||NoSQL: Indexed|Amazon SimpleDB|Google Cloud Datastore|
||NoSQL: Columnar Database|-|Google Cloud Bigtable|
|Network|VPC|Amazon VPC|Google Cloud Virtual Network|
||Load Balancer|Elastic Load Balancer|Google Cloud Load Balancing|
||Peering|Direct Connect|Google Cloud Interconnect|
||DNS|Amazon Route 53|Google Cloud DNS|
||CDN|Amazon CloudFront|Google Cloud CDN|
|Migration|Detect|AWS Application Discovery Service|-|
||Database|AWS Database Migration Service|-|
||Server|AWS Server Migration Service|-|
||Data|AWS Snowball|-|
|Dev Tools||AWS CodeStar||
|||AWS CodeCommit|Cloud Source Repositories|
|||AWS CodeBuild||
|||AWS CodeDeploy||
|||AWS CodePipeline||
||Trace|AWS X-Ray|Stackdriver Trace|
|Management Services|Monitoring|Amazon CloudWatch|Stackdriver Monitoring|
||Deployment|AWS CloudFormation|Google Cloud Deployment Manager|
|||AWS CloudTrail|API Manager|
|||AWS Config||
|||AWS OpsWorks||
|||AWS Service Catalog||
|||AWS Trusted Advisor||
|||AWS Managed Services||
|Security / ID|IAM|AWS Identity and Access Management|IAM|
|||Amazon Inspector||
||Certification|AWS Certificate Manager||
||LDAP|AWS Directory Service||
||WAF|AWS WAF||
|||AWS Shield||
|||AWS Artifact||
|Big Data & Analytics|Analytics|Amazon Athena|-|
||Search Engine|Amazon CloudSearch|-|
||Analytics: Full-text Search|Amazon Elasticsearch Service|-|
||Batch Data Processing|Amazon Elastic Map Reduce|Google Cloud Dataproc|
||Stream Data|Amazon Kinesis|-|
||Pub/Sub|-|Google Cloud Pub/Sub|
||Data Processing|AWS Data Pipeline|Google Cloud Dataflow|
||BI|Amazon QuickSight|-|
||Analytics: Query|Amazon Redshift|Google BigQuery|
|ML / DL||Amazon Lex||
|||Amazon Polly||
|||Amazon Rekognition||
|||Amazon Machine Learning||
|IoT|Platform|AWS IoT||
||Edge Computing|AWS Greengrass|-|
|Contact Center|Contact Center|Amazon Connect|-|
|Game|App Dev/Build|Amazon GameLift|-|
|Mobile|App Dev/Build|AWS Mobile Hub|-|
||Authentication|Amazon Cognito|Firebase|
||Device Management|AWS Device Farm|-|
||Analytics|Amazon Mobile Analytics|-|
||Push|Amazon Pinpoint|-|
||mBaaS|-|Firebase|
|Application Service||AWS Step Functions||
|||Amazon Simple Workflow Service||
|||Amazon API Gateway||
|||Amazon Elastic Transcoder||
|Messaging||Amazon Simple Queue Service||
|||Amazon Simple Notification Service|Google Cloud Pub/Sub|
|||Amazon SES||
|Enaterprise||Amazon WorkDocs||
|||Amazon WorkMail||
|||Amazon Chime||
|Desktop||Amazon WorkSpaces||
|||AppStream 2.0||

※ G Suite の機能は入れてません。

# GCP全体像

|カテゴリ|サービス|説明|
|:---|:---|:---|
|コンピュート|Compute Engine (GCE)|仮想マシンサービス。|
||Container Engine (GKE)|コンテナオーケストレーション。|
||App Engine (GAE)|運用が容易でスケーラブルなアプリケーションプラットフォーム。|
|ストレージ/データベース|Cloud Storage|オブジェクトストレージ。|
||Cloud SQL|RDBMSのマネージドサービス。|
||Cloud Datastore|NoSQLデータベース。|
|ビッグデータ|BigQuery|ペタバイト規模の大規模なデータ分析に対応したDWH。|
||Cloud Bigtable|低レイテンシ・高スループットのNoSQL。HBase互換API有り。|
||Cloud Spanner|ストロングコンシステンシーとスケーラビリティを備えた分散型RDB。|
||Cloud Dataflow|ストリーミング処理とバッチ処理に対応したフルマネージドデータ処理サービス。|
||Cloud Dataproc|Spark/Hadoopのフルマネージドサービス。|
||Cloud Pub/Sub|リアルタイム・高信頼性メッセージング・ストリーミングサービス。|
|ネットワーキング|Cloud Virtual Network|仮想ネットワーク。|
||Cloud Load Balancing|ロードバランサ。|
||Cloud DNS|DNSのマネージドサービス。|
|機械学習|Cloud Vision API|画像分析サービス。画像に含まれる情報を取得するだけでなく、不適切な画像を検出する機能や、文字認証(OCR)機能を提供。|
||Cloud Speechi API|音声のテキスト変換サービス。|
||Cloud Translation API|テキスト翻訳サービス。テキストをソース言語からターゲット言語に動的に翻訳できる。|
||Cloud Natural Language API|自然言語分析サービス。構文解析や感情解析が可能。|
||Cloud Machine Learning Engine|TensorFlowを用いた機械学習サービス。GPUを利用して大規模なモデルの構築／学習処理が可能。|
|運用管理ツール|Cloud Console|統合管理コンソール|
||Cloud Shell|ブラウザ上で利用可能なコマンドラインコンソール|
||Stackdriver|統合監視サービス|
||Cloud IAM|ユーザアクセス管理|

## GCE、GKE、GAEの違い

||GCE|GKE|GAE|
|:---|:---|:---|:---|
|サービス|仮想マシン|Kubernetesベースのコンテナクラスタ|アプリケーション実行環境|
|Appデプロイ方法|ゲストOS上にAppをインストール|Dockerイメージからコンテナをデプロイ|ソースコードのデプロイ|
|用途|従来型App実行環境|コンテナベースのマイクロサービス|スケーラビリティと開発効率重視のApp開発・実行環境|

## ユーザアカウントとプロジェクト

- GCPユーザは **Googleアカウント**
- **プロジェクト** を作成し、課金アカウントとの紐づけ
  - 個人でGCPを利用する場合は、プロジェクト作成アカウントが課金アカウント
  - **プロジェクトID** と呼ばれるUUIDで識別される
- プロジェクトに登録されたユーザのみがプロジェクト内リソースを操作可能（ **テナント** という考え方）

## GCPの開発・運用管理ツール

- Cloud SDK
  - `gcloud` コマンドを含めたGCPサービスをコントロールするツールやライブラリをまとめたもの
  - 開発者用のローカルマシンにインストールして使用する
- Cloud Shell
  - ブラウザ上のCloud Console内にあるコマンド端末
  - Cloud SDKの機能をインストール無しで利用できる
  - Cloud SDK以外にもLinux系コマンド、テキストエディタ、ビルドツール等様々利用できる
    - Linuxシェル：bash、sh
    - Linuxユーティリティ：Debian用の標準的なシステムユーティリティ
    - Google SDKとツール：Google App Engine SDK、Google Cloud SDK、gsutil for Cloud Storage
    - テキストエディタ：Emacs、Vim、Nano
    - ビルド・パッケージツール：Gradle、Make、Maven、npm、nvm、pip
    - バージョン管理ツール：Git、Mercurial
    - その他：Docker、iPython、MySQL Client、gRPCコンパイラ
  - GCE上の一時インスタンス上で実行される
    - f1-microサイズ、Debianベースの仮想マシン
    - ユーザまたはセッション毎にプロビジョニングされている
    - セッションが非アクティブになって1時間で停止する
    - なお、5GBの永続化ディスクがあり、インスタンスが停止しても蒸発しない

## デバック、トレース、分析ツール

- Stackdriver Debugger
  - GEAやGCEで実行されるJava Appをコードレベルで確認できる。
- Stackdriver Trace
  - GAEアプリケーションから呼び出されるリモートプロシージャコール(RPC)を表示して、所要時間を分析できる。
  - Appのリクエストに対するレイテンシを確認できる。

## ロギング、モニタリング

- Stackdriver Logging
  - GAE、GCEで実行さえるAppログの収集・保管・閲覧が可能。
  - GCP上の各種ストレージ・DBへのエクスポートが可能。
  - Cloud Loggingエージェントを用いて、サードパーティログの統合も可能。
- Stackdriver Monitoring
  - GCP上で稼働するApp用のダッシュボードとアラート。

## 自動デプロイ

- Cloud Launcher
  - 様々な種類のソフトウェアがデプロイされたGCEを簡単に起動できるサービス。
- Cloud Deployment Manager
  - テンプレートを作成して定義した構成のアプリケーションをデプロイできる。

## ストレージタイプ

Cloud Storageは勿論、各種DBのストレージとして利用できるストレージタイプは以下の通り。

|名前|説明|用途|
|Multi-Regional|地理的に独立した場所にデータを複製保存。可用性99.95%。||
|Regional|単一リージョン・複数ゾーンにデータを複製保存。可用性99.9%。||
|Nearline|月に1度程度の低アクセス頻度のデータを低価格で保存。可用性99%。||
|Coldline|年に1度程度の低アクセス頻度のデータを低価格で保存。可用性99%。||

## GCPの機械学習サービス

- Cloud Vision API
  - 画像のラベル付け
  - 不適切な画像の検出
  - 顔の位置、方向の検出
  - 顔を構成するパーツの検出
  - 顔の表情による感情分析
  - 画像に含まれる色の割合の検出
  - OCR処理（テキストの抜き出し）
  - Webから類似画像検索
- Cloud Translation API
  - テキストの翻訳
  - 言語（何語？）の検出
- Cloud Speech API
  - 音声ファイルのテキスト化
- Cloud Natural Language API
  - テキストの構文解析
  - カテゴリ別の単語の抽出
  - テキストの内容による感情分析
- Cloud Mchine Learning Engine
  - TensorFlowで記述した独自モデルの学習
  - 学習済みモデルのAPIサービス化
