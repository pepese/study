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
|Rows / Records|Documents|
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
`{id}` は一意である必要があり、POSTの際、指定してアクセスすればそれが、指定せずアクセスすれば自動生成される。  
また、ElasticSearchでは **楽観ロック** （更新開始時に排他処理は行なわず、更新完了時に他からの更新がないか確認。他からの更新があったらロールバックしてエラーを返却。）が採用されている。  

CRUDのAPIは[Single/Multi-document APIs](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs.html)など十分用意されている。  
Index・Type横断など含め**複数種類のデータを横断**で検索をかける場合は[Search APIs](https://www.elastic.co/guide/en/elasticsearch/reference/current/search.html)を用いる。  
その他にも様々なAPIがある。詳細は[ドキュメント](https://www.elastic.co/guide/index.html)を参照。

## インデックス作成

```sh
$ curl -X PUT http://<IPアドレス>:<port番号>/<インデックス名>
```

## インデックス一覧

```sh
$ curl http://<IPアドレス>:<port番号>/_aliases?pretty
```

## インデックス削除

```sh
$ curl -X DELETE http://<IPアドレス>:<port番号>/<インデックス名>
```

## タイプ作成

```sh
$ curl -X PUT http://<IPアドレス>:<port番号>/<インデックス名> -d '
{
    "mappings" : {
      "<タイプ名>" : {
        "properties" : {
          "author" : {
            "type" : "string"
          },
          "contents" : {
            "type" : "string",
            "analyzer": "japanese‎"
          },
          "enabled" : {
            "type" : "boolean"
          },
          "pub_date" : {
            "type" : "date",
            "format" : "dateOptionalTime"
          },
          "read_ratio" : {
            "type" : "double"
          },
          "reads" : {
            "type" : "long"
          },
          "subtitle" : {
            "type" : "string",
            "analyzer": "japanese‎"
          },
          "title" : {
            "type" : "string",
            "analyzer": "japanese‎"
          },
          "views" : {
            "type" : "long"
          }
        }
      }
    }
  }
}'
```

上記、型の定義は適当。  
タイプを作成する際は、上記の通り **マッピング** （ **Mapping** ） というテーブル定義的なものを作成する。  
マッピングをファイルで定義することもできる。

```sh
$ curl -X PUT http://<IPアドレス>:<port番号>/<インデックス名> --data @el_mapping.json
```

また、マッピングは後述する「データ登録」をいきなり行っても自動で作成される。

### マッピング / Mapping

マッピングで利用するデータタイプには以下のものがある。

- Core datatypes
  - 基本的なデータ型
- Complex datatypes
  - JSON をサポートするための特殊なデータ型
- Geo datatypes
  - ロケーション検索用のデータ型
  - 半径何メートル以内の情報を検索するなど、緯度・経度をベースにした検索に利用できる
- Specialised datatypes
  - Elasticsearch ならではのデータ型

#### Core datatypes

|種類|データ型|
|:---|:---|
|String datatype|string|
|Numeric datatype|long, integer, short, byte, double, float|
|Date datatype|date|
|Boolean datatype|boolean|
|Binary datatype|binary|

#### Complex datatypes

- Array datatype
  - Array support does not require a dedicated `type`
- Object datatype
  - `object` for single JSON objects
- Nested datatype
  - `nested` for arrays of JSON Objects

#### Geo datatypes

- Geo-point datatype
  - `geo_point` for lat/lon points
- Geo-Shape datatype
  - `geo_shape` for complex shapes like polygons

#### Specialised datatypes

- IPv4 datatype
  - `ip` for IPv4 addresses
- Completion datatype
  - `completion` to provide auto-complete suggestions
- Token count datatype
  - `token_count` to count the number of tokens in a string
- mapper-murmur3
  - `murmur3` to compute hashes of values at index-time and store them in the index
- Attachment datatype mapper-attachments plugin which supports indexing `attachments` like Microsoft Office formats, Open Document formats, ePub, HTML, etc. into an attachment datatype.

## データ登録

```sh
$ curl -X PUT http://<IPアドレス>:<port番号>/<インデックス名>/<タイプ名>/<id> -d '<json形式のデータ>'
```

タイプが作成されていない段階でデータ登録を行うと「自動マッピングルール」にしたがって、マッピングが作成される。

### 自動マッピングルール

```
１） 基本的なフィールド型
String:             string
Whole number:       byte, short, integer, long
Floating point:     float, double
Boolean:            boolean
Date:               date

２） 基本的な型マッピングルール
JSON type:                           | Field type:
-------------------------------------|---------------
Boolean: true or false               | “boolean”
Whole number: 123                    | “long”
Floating point: 123.45               | “double”
String, valid date: “2014-09-15"     | “date”
String: “foo bar”                    | “string”
```

# 参考

- セミナー
  - https://info.elastic.co/japan-technical-workshop.html
- ハンズオン
  - http://kakakakakku.hatenablog.com/entry/2015/09/09/231956
  - https://github.com/Kakakakakku/elasticsearch-hands-on
- 記事
  - http://dev.classmethod.jp/server-side/elasticsearch-getting-started-08/
