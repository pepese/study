Ruby on Railsのモデルについてまとめる。

Ruby on Railsでは標準で、O/Rマッパーである**Active Recored**が使用できる。  

# Model作成コマンド

```
rails generate model NAME [field[:type][:index] field[:type][:index]] [options]
```

- NAEM
    - モデル名
    - 「**[NameSpace]/**モデル名」と**スラッシュ**で区切ると名前空間を設定できる。
        - 名前空間を設定するとディレクトリが分割されたり、名前空間名のプレフィックスがついたりする
    - 「HogeFuge」のように**大文字区切り**にすると**アンダースコア区切り**の命名でModel、テーブルなどが作成される
        - 「HogeFuge」->「models/hoge_fuge.rb」
- field[:type][:index]
    - フィールド名、データ型、インデックスを指定
    - type（データ型）には以下が指定できる
        - integer
        - primary_key
        - decimal
        - float
        - boolean
        - binary
        - string
        - text
        - date
        - time
        - datetime
    - index（インデックス）には以下が指定できる
        - uniq
        - index

```rails generate model```コマンドのオプションは以下の通り。

- 「--skip-namespace」／「--no-skip-namespace」
    - 名前空間設定がスキップされる（デフォルト：スキップしない）
- 「--force-plural」／「--no-force-plural」
    - モデル名を強制的に適用する（デフォルト：しない）
- 「-o [NAME]」「--orm=[NAME]」
    - 使用するO/Rマッパーを指定（デフォルト：active_record）
- 「--migration」／「--no-migration」
    - マイグレーションファイルを作成するか否か（デフォ：する）
- 「--timestamps」／「--no-timestamps」
    - タイムスタンプ列（created_at、updated_at）を作成するか否か（デフォ：する）
- 「--parent=[PARENT]」
    - モデルの親クラスを指定
- 「--indexes」／「--no-indexes」
    - 外部キーにインデックスを付与するか否か（デフォ：する）
- 「--primary-key-type=PRIMARY_KEY_TYPE」
    - 主キーのタイプを指定
- 「-t [NAME]」「--test-framework=[NAME]」
    - 使用するテスティングフレームワークを指定（デフォ：test_unit）
- 「--fixture」／「--no-fixture」
    - フィクスチャを生成するか否か（デフォ：する）
- 「-r [NAME]」「--fixture-replacement=[NAME]」
    - フィクスチァを変更する

# 規則・ルール

- モデル/クラスは単数形、テーブル/スキーマは複数形
- ApplicationRecordを継承
- 外部キーは「**テーブル名の単数形_id**」

Model作成時、特に指定しなければ以下のカラムが自動的に追加される。

- id
    - integer型の主キー
- created_at
    - datetime型のレコード作成時の日付時刻
- updated_at
    - datetime型のレコード更新時の日付時刻

# Active Modelのメソッド

Modelは以下のメソッドをデフォルトで持っている。

- new/build
    - オブジェクトを生成する
    - 保存はされていないため**save**メソッドなどで保存する
    - buildはnewのエイリアス
    - オブジェクト生成と保存を同時に行う場合は**create**を使用する
- find
    - データベースからデータを検索する
- find_by_colum
    - カラムを指定して最初の１件を取得する
- find_by_sql
    - SQLを直接指定
- where
    - 条件を指定
- order/reorder
    - 取得した値を並び替え
- select
    - 取得するカラムを指定
- limit
    - レコード件数を指定
- group
    - 取得した値をグループ化
- having
    - 取得したデータを絞り込む
- scope
    - 利用する検索条件をあらかじめ準備
- default_scope
    - デフォルトのスコープを定義
- count
    - 検索結果の行数を取得
- maximun/average/minimum/sum
    - カラムの最大値/平均値/最小値/合計値を求める
- save
    - newの後、データベースに保存する
- update/update_all/update_attributes/update_attribute
    - データベースを更新する
- destroy/delete/destroy_all/delete_all
    - データベースから削除
- calculate
    - 集計やテーブル結合など
-

# Active Recordのメソッド

Active Modelは抽象化レイヤであり、デフォルトの実装レイヤはActive Record。  
以下はActive Recordのメソッド。

- to_xml
    - モデルオブジェクトをXMLへ変換する
- increment
    - 特定のレコードをインクリメントする
- all
    - すべてのレコードを取得する
- find_all_by
    - カラム名を指定してすべてのレコードを取得する
- first/last
    - 先頭/末尾のレコードを取得する
- except
    - 条件を除外する
- reload
    - モデルを再取得する
- column_for_attribute
    - データベースから取得したレコードのカラム名を取得する
- attribute_names
    - テーブルすべてのカラム名を取得する
- new_record_?/persisted?/changed?/destroyed?
    - レコードの状態をチェックする
- includes
    - 関連するテーブルをまとめて取得する
- reverse_order
    - 取得した値を逆順にする
- find_each
    - データの大きなモデルに対して、分割してレコードを取得する
- composed_of
    - 複数カラムを擬似的にまとめる
- any?
    - モデルがからであるか確認する
- create
    - モデルを生成して保存する
- concat
    - 複数のレコードを追加する
- attribute_for_inspect
    - テーブルの属性を取得
- attribute_present?
    - モデルに任意のカラムが存在するかチェックする
- attribute
    - モデルのすべてのカラムと属性を取得する
- has_attribute?
    - モデルに任意のカラムが存在するかチェックする
- respond_to?
    - モデルに対して呼び出せるかチェックする

メチャいっぱいある。書くの諦めた。
http://railsdoc.com/model

# その他

最新のDB定義状況はdb/schema.rbにあり。
Railsではデータの投入にSeed（db/seeds.rb）とFixtureがある。
SeedはマスタデータにFixtureはテストデータにしようすると良い。
また、gem annotateを使用するとModelにSchema情報のコメントを挿入してくれる。
