Ruby on Railsの全体像について軽くまとめる。

# Ruby on Railsのライブラリ構造

- Action Pack
    - Action Controller
    - Action View
    - Action Dispatch
- Action Cable
- Active Job
- Active Model
- Active Record
- Active Mailer
- Active Support
- Railties

# Ruby on Railsのライブラリ機能

それぞれ以下のような機能を持つ。

|ライブラリ|機能概要|
|:-------|:------|
|Action Pack      |Action Conoroller、Action View、Action Dispatchの総称|
|Action Controller|コントローラ（リクエスト、状態、レスポンスの管理）|
|Action View      |ビュー（HTML、XML、JSONなどのフォーマット生成）|
|Action Dispatch  |ルーティング（リクエストをコントローラへ割り振る）|
|Action Cable     |WebSocket|
|Active Job       |非同期ジョブをキュー登録|
|Active Model     |モデル|
|Active Record    |ORM（モデルとRDBMSのテーブルをマッピング）|
|Active Mailer    |メール送信|
|Active Support   |Ruby標準ライブラリの拡張|
|Railties         |Railsの各種コンポーネントやプラグインを繋ぐ|

# Ruby on Railsアプリケーション作成コマンド

Ruby on Railsでは```rails```コマンドを使って設定ファイルやソースを半自動生成しながら開発する。  
アプリケーションの作成には```rails new```コマンドを使用する。

```bash
rails new [アプリ名] [options]
```

```rails new```コマンドのオプションは以下の通り。

|オプション                 |説明                                      |デフォルト|
|:--------------------------|:-----------------------------------------|:---------|
|-r, -ruby=PATH             |ruby実行ファイルまでのパス                ||
|-m, -template=TAMPLATE     |アプリケーションテンプレートのパス        ||
|-d, --database=DATABASE    |データベースの種類                        |sqlite3|
|-j, --javascript=JAVASCRIPT|組み込むJavaScriptライブラリーを指定。    |jquery|
|--skip-gemfile             |Grmfileを作成しない                       |作成する|
|ｰB, --skip-bundle          |bundle installを行わない                  |installする|
|-G, --skip-git             |.gitignoreを作成しない                    |作成する|
|--skip-keeps               |.keepを作成しない                         |作成する|
|-M, --skip-action-mailer   |Active Mailerを作成しない                 |作成する|
|-O, --skip-active-recode   |Active Recordを作成しない                 |作成する|
|-P, --skip-puma            |Pumaを関連付けない                        |する|
|-C, --skip-action-cable    |Action Cableを作成しない                  |作成する|
|-S, --skip-sprockets       |Sprocketsを作成しない                     |作成する|
|--skip-spring              |springアプリケーションをインストールしない||
|--skip-listen              |listen gemに依存した設定を作成しない      ||
|-J, --skip-javascript      |javascriptを組み込まない                  ||
|--skip-turbolinks          |turbolinks gemをスキップする              ||
|-T, --skip-test-unit       |test::unitを組み込まない                  ||
|--dev                      |github リポジトリ上の自分のコードから作成 ||
|--edge                     |github リポジトリ上の最新のコードから作成 ||
|--rc=RC                    |Railsコマンドのオプションファイルを指定   ||
|--no-rc                    |.railsrcファイルをロードしない            |する|
|--api                      |APIのみのアプリケーション設定             ||
|-f, --force                |ファイルが存在する場合に上書きする        ||
|-p, --pretend              |ドライランする                            |しない|
|-q, --quiet                |進捗情報を表示しない                      |する|
|-s, --skip                 |既に存在するファイルについてはスキップ    |しない|
|-h, --help                 |ヘルプ                                    ||
|-v, --version              |バージョンを表示                          ||

## APIモード

Ruby on Railsでは```rails new```コマンドで初回アプリケーションを作成する。  
その際、```--api```オプションをつけることで**APIモード**となり、assets、helper、view(erb)などの画面に関わるものが作成されない。  
さらにAPIモードでは**config/application.rb**に以下の設定が追加されている。  

```ruby
config.api_only = true
```

この設定があることにより、```rails```コマンドで作成される資材が通常モードに比べ不要なものが削減される。  
実際にこの設定を削除して```rails```コマンドを実行すると作成される資材が増える。  
また、APIモードではCORS対策として**config/initializers/cors.rb**が作成される。

## rails generateコマンド

Railsは大枠はMVC単位で開発していく。  
MVC以外にも作成単位があり、いずれも```rails generate```コマンドを使用してソースコードを半自動生成して開発を進めていく。  

```bash
rails generate GENERATOR [args] [options]
```

GENERATORには以下の種類がある。（rails generate --help）

```
Rails:
  assets
  channel
  controller
  generator
  helper
  integration_test
  jbuilder
  job
  mailer
  migration
  model
  resource
  scaffold
  scaffold_controller
  task

Coffee:
  coffee:assets

Js:
  js:assets

TestUnit:
  test_unit:generator
  test_unit:plugin
```

GENERATORの種類によって以下のファイルが生成される。（オプションで多少変化する）

|GENERATOR          |コントローラ|ビュー|モデル|マイグレーション|アセット|ルーティング|テスト|ヘルパー|
|:------------------|:----------:|:----:|:----:|:--------------:|:------:|:----------:|:----:|:------:|
|assets             |×          |×    |×    |×              |○      |×          |×    |×      |
|controller         |○          |○    |×    |×              |○      |○          |○    |○      |
|generator          |×          |×    |×    |×              |×      |×          |×    |×      |
|helper             |×          |×    |×    |×              |×      |×          |○    |○      |
|integration_test   |×          |×    |×    |×              |×      |×          |○    |×      |
|jbuilder           |×          |○    |×    |×              |×      |×          |×    |×      |
|mailer             |×          |×    |×    |×              |×      |×          |○    |×      |
|migration          |×          |×    |×    |○              |×      |×          |○    |×      |
|model              |×          |×    |○    |○              |×      |×          |○    |×      |
|resource           |○          |○    |○    |○              |○      |○          |○    |○      |
|scaffold           |○          |○    |○    |○              |○      |○          |○    |○      |
|scaffold_controller|○          |○    |×    |×              |×      |×          |○    |○      |
|task               |×          |×    |×    |×              |×      |×          |×    |×      |

ちなみに失敗したら```rails destroy GENERATOR```コマンドで削除できる。

実際のアプリケーションの作成は以下を参照。

⭐️ここにrails_basic.md⭐️
