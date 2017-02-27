全文検索エンジン[ElasticSearch](https://www.elastic.co/jp/products/elasticsearch)についてまとめる。

# インストール

インストールから起動までは以下。

```bash
$ brew update
$ brew install elasticsearch
$ elasticsearch
```

起動確認。

```bash
$ curl http://localhost:9200/
```

## プラグインのインストール

```bash
$ elasticsearch-plugin install [plugin-name]
```

例えば日本語対応するなら以下。

```bash
$ elasticsearch-plugin install analysis-kuromoji
```

# 概要

ElasticSearchには**Indices**、**Types**、**Documents**、**Fields** があり、RDBと比較するとそれぞれ以下のような対応関係になる。

|RDB|ElasticSearch|
|:---|:---|
|Databases|Indices|
|Tables|Types|
|Rows|Documents|
|Columns|Fields|

上記はあくまで構造上の比較であって設計ではその限りではない。  
例えば、商品データを扱う場合、RDBではほぼ「商品テーブル」として作成するからElasticSearchでは「商品Type」かと思いきや「商品Index」とする場合もある。

## Cluster、Shards

ElasticSearchは複数ノードでクラスタ構成を作ることができ、ノード間は**Shard**という単位でレプリケーションをしている。  
Shardには**Primary Shard**と**Replica Shard**がある。  
IndexはN個のShardを持つ。デフォルトで1つのIndexは5つのShardに分割され、それぞれ1つのReplicaを持つ。（つまり、1つのIndexに10個のShard）  
イメージは[ここ](https://speakerdeck.com/johtani/elasticsearchru-men)で掴める。


# アクセス方法

ElasticSearchはRDBのようにドライバを使用してアクセスするのではなく、**REST API** でアクセスする。  
以下のようなURI構造になっている。

```bash
/{index}/{type}/{id}
```

上記のようなURI構造に対してGET、POST、PUT、DELETEメソッドでアクセスし、データのCRUDを行う。  
```{id}``` は一意である必要があり、POSTの際、指定してアクセスすればそれが、指定せずアクセスすれば自動生成される。  
また、ElasticSearchでは **楽観ロック** （更新開始時に排他処理は行なわず、更新完了時に他からの更新がないか確認。他からの更新があったらロールバックしてエラーを返却。）が採用されている。  

CRUDのAPIは[Single/Multi-document APIs](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs.html)など十分用意されている。  
Index・Type横断など含め**複数種類のデータを横断**で検索をかける場合は[Search APIs](https://www.elastic.co/guide/en/elasticsearch/reference/current/search.html)を用いる。  
その他にも様々なAPIがある。詳細は[ドキュメント](https://www.elastic.co/guide/index.html)を参照。

# 参考

- セミナー
  - https://info.elastic.co/japan-technical-workshop.html
- ハンズオン
  - http://kakakakakku.hatenablog.com/entry/2015/09/09/231956
  - https://github.com/Kakakakakku/elasticsearch-hands-on
- 記事
  - http://dev.classmethod.jp/server-side/elasticsearch-getting-started-08/
