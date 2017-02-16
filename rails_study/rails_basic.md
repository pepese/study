サンプルアプリを作りながらRuby on Railsの全体像を把握する。  
実行環境構築は以下を参照。  

[http://blog.pepese.com/entry/20140526/1401075656:embed:cite]

また、ここでの実行環境は以下。  

```
> ruby -v
ruby 2.3.1p112 (2016-04-26 revision 54768) [x64-mingw32]

> rails -v
Rails 5.0.0.1
```

# Ruby on Railsアプリケーションの作成

Ruby on Railsでは**rails**コマンドを使って設定ファイルやソースを半自動生成しながら開発する。  
アプリケーションの作成には「**rails new**」コマンドを使用する。  

Ruby on Railsの概要と```rails new```コマンド、```rails generate```コマンドについては以下を参照。

⭐️ここにrails_overview.md⭐️

以降の手順でアプリケーションを作成していく。  

**１．「C:\workspace\rails」を作成してcdする。**  
**２．「rails new sample」をうつ。**   
sampleというアプリケーションのスケルトンが作成される。  
バージョンを指定したい場合は「rails \_4.2.7\_ new sample」のように打つ。  
ディレクトリツールはこんな感じ。（一部省略）
```
> tree /f .
C:.
│  config.ru       # Rack設定ファイル（Ruby製Webサーバ）
│  Gemfile         # Gemの定義ファイル
│  Rakefile        # Rakeの定義ファイル
├─app              # アプリのクラス、設定ファイル
│  ├─assets        # アセット（image/js/css）
│  ├─channels      #  WebSocketクラス
│  ├─controllers   # コントローラクラス
│  ├─helpers       # ビューヘルパー
│  ├─jobs          # Active Jobクラス
│  ├─mailers       # Active Mailerクラス
│  ├─models        # モデルクラス
│  └─views         # ビュー（ERBなど）
├─bin              # 各種スクリプトを配置
├─config           # 各種設定ファイルを配置
│  ├─environments  # 環境別設定ファイル
│  ├─initializers  # 初期化用設定ファイル
│  └─locales       # 国際化用設定ファイル
├─db               # データベース定義、設定ファイル
├─lib              # 自作ライブラリ置き場
├─log              # ログ出力先
├─public           # 公開ファイル置き場
├─test             # テストスクリプト
├─tmp              # テンポラリ
└─vendor           # サードパーティJS/CSS置き場
```

**３．「rails server」をうつ。**  
WEBrickというRails組み込みのHTTPサーバが起動する。  
**４．ブラウザで「http://localhost:3000」へアクセス。**  
「Yay! You’re on Rails!」って出る！
HTTPサーバを停止するときはDOSでCtr+cでとまる。

## コントローラ（C）の作成

Controllerの基本は以下を参照。

⭐️ここにrails_basic_overview.md⭐️

ここでは以下の手順でControllerを作成していく。

**１．「C:\rubyWorkspace\sample」へcd**  
**２．「rails generate controller hello index」をたたく**   
ディレクトリツリーはこんな感じ。（新規作成された箇所のみ）
```
create  app/controllers/hello_controller.rb
 route  get 'hello/index'
invoke  erb
create    app/views/hello
create    app/views/hello/index.html.erb
invoke  test_unit
create    test/controllers/hello_controller_test.rb
invoke  helper
create    app/helpers/hello_helper.rb
invoke    test_unit
invoke  assets
invoke    coffee
create      app/assets/javascripts/hello.coffee
invoke    scss
create      app/assets/stylesheets/hello.scss
```

**３．コントローラクラス（hello_controller.rb）を編集**
```ruby
class HelloController < ApplicationController
  def index
    render text: 'Hello World !' # この行を追加！
  end
end
```

**４．ルーティングの設定（/config/routes.rb）**  
アクセスURIとコントローラクラス＋アクションメソッドとの紐付を「**routes.rb**」で行う。  
今回はrails generateコマンドで自動生成されている部分の確認。  
```ruby
Rails.application.routes.draw do
  get 'hello/index' # この行が自動で追加されている
end
```
HTTPメソッド**Get**で「http://localhost:3000/<strong>hello/index</strong>」へアクセスすると**helloクラスのindexメソッド**が呼び出される。  
また、「**rake routes**」コマンドで現在有効なルートをリスト表示してくれる。  

**５．「rails server」をたたく**  
とめていなければ実行する必要はない。  

**６．ブラウザで「http://localhost:3000/hello/index」へアクセス**  
「Hello World !」と表示される。  
これはhelloコントローラクラスのindexアクションで「**render text: 'Hello World !'**」の出力がそのまま表示されているだけで、ビュー機能は使用していない。  

## ビュー（V）の作成
Viewは```rails generate controller```コマンドで既に作成されており、「**app/views/hello/index.html.erb**」にある。 （Controller毎にディレクトリが作成される） 
rails generateコマンドを実行する際、**action**を指定しておくとアクションメソッドと同じファイル名（＋.html.erb）で作成される。  
**ERB**（拡張子erb）はEmbedded Rubyの略でRubyで使用されるテンプレートエンジン。  
ここでは以下の手順でビューを作成していく。

**１．コントローラクラス（hello_controller.rb）を編集**
```ruby
class HelloController < ApplicationController
  def index
    # render text: 'Hello World !' # コメントアウト
    @msg = 'Hello World !'         # 追加行
  end
end
```
アットマーク（**@**）を付けた変数は**テンプレート変数**といい、ビューへ値を引き渡す役目になる。  

**２．ビュー（views/hello/index.html.erb）を編集**  
```html
<h1>Hello#index</h1>
<p>Find me in app/views/hello/index.html.erb</p>
<%= @msg %> <!-- この行を追加-->
```
「@msg」はテンプレート変数でコントローラのアクションメソッドから値が渡される。  

**３．「rails server」をたたく**  
とめていなければ実行する必要はない。  

**４．ブラウザで「http://localhost:3000/hello/index」へアクセス**  
テンプレートで記述されている「Hello#index」「Find me in app/views/hello/index.html.erb」と
テンプレート変数で引き渡した「Hello World !」が表示される。  

## ビュー（V）の変更

**helloコントローラクラスのindex**でテンプレート変数を使用すると「**views/hello/index.html.erb**」のERBテンプレートが自動的に選択される。
コントロールクラスで
```ruby
render '[PATH]'
```
を記述するとテンプレートを指定することができる。  
PATHは「**/app/views**」ディレクトリからの相対パスで記載し、**.html.erbは省略**する。  

**１．ビュー用のテンプレートファイル（views/hello/sampleView.html.erb）を作成**  
```html
<h1>Sample View Page</h1>
<%= @msg %>
```
★ERBテンプレートは文字コードを**UTF-8**で保存する。  

**２．コントローラクラス（hello_controller.rb）を編集**
```ruby
class HelloController < ApplicationController
  def index
    # render text: 'Hello World !'
    @msg = 'Hello World !'
    render 'hello/sampleView' # この行を追加
  end
end
```

**３．「rails server」をたたく**  
とめていなければ実行する必要はない。  

**４．ブラウザで「http://localhost:3000/hello/index」へアクセス**  
ブラウザで表示されたページのソースを見ると「Sample View Page」と表示されて、先ほど自作したテンプレートが使用されていることがわかる。  
表示されたページをソースで見ると、なにやらその他にたくさんhtmlタグが追加されている。  
これは<strong>レイアウトテンプレート</strong>というものが使用されている。  
「**app/views/layouts/application.html.erb**」を見ると「**<%= yield %>**」という部分があり、ここに作成したテンプレートが適用されるようになっている。  
されにstylesheetやjavascriptはコントローラ単位でディレクトリがわけられて作成されたものが自動反映されるようになっている。  
レイアウトの自動反映がイヤな場合は「application.html.erb」を削除もしくはリネームする。  


## モデル（M）の作成

Rails標準のO/Rマッパーである**Active Recored**が使用できる。  
Modelの基本は以下を参照。

⭐️ここにmodel_basic.md⭐️

ここでは以下の手順でModelを作成していく。

**１．「C:\rubyWorkspace\sample」へcd**  
**２．「rails generate model user no:string name:string age:integer birthday:date active:boolean」をたたく**  
以下のようにファイルが更新される。
```
invoke  active_record
create    db/migrate/20161103065150_create_users.rb
create    app/models/user.rb
invoke    test_unit
create      test/models/user_test.rb
create      test/fixtures/users.yml
```

**３．「rake db:migrate」をたたく**  
Railsには<strong>マイグレーション</strong>というテーブルの作成・変更を行う機能がある。  
マイグレーションを実行するためのファイルは「**db/migrate/**20161103065150_create_users.rb」に自動生成されている。
```ruby
class CreateUsers < ActiveRecord::Migration[5.0]
  def change
    create_table :users do |t|
      t.string :no
      t.string :name
      t.integer :age
      t.date :birthday
      t.boolean :active

      t.timestamps
    end
  end
end
```

**４．「rake db:fixtures:load FIXTURES=users」をたたく**  
Railsには**フィクスチャ**という機能があり、DBへテスト用データを下敷きする機能がある。  
フィクスチャを実行するためのファイルは「**test/fixtures/**user.yml」に自動生成されている。
```ruby
one:
  'no': MyString
  name: MyString
  age: 1
  birthday: 2016-11-03
  active: false

two:
  'no': MyString
  name: MyString
  age: 1
  birthday: 2016-11-03
  active: false
```
また、Railsでは「**rails dbconsole**」というコマンドでDBクライアントを起動することができる。
```
> rails dbconsole
SQLite version 3.15.0 2016-10-14 10:20:30
Enter ".help" for usage hints.
sqlite> .tables
ar_internal_metadata  schema_migrations     users
sqlite> .schema users
CREATE TABLE "users" (
  "id"         INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
  "no"         varchar,
  "name"       varchar,
  "age"        integer,
  "birthday"   date,
  "active"     boolean,
  "created_at" datetime NOT NULL,
  "updated_at" datetime NOT NULL
);
sqlite> select * from users;
298486374|MyString|MyString|1|2016-11-03|f|2016-11-03 06:55:13.749144|2016-11-03 06:55:13.749144
980190962|MyString|MyString|1|2016-11-03|f|2016-11-03 06:55:13.749144|2016-11-03 06:55:13.749144
sqlite> .quit
```
※見やすくちょっとフォーマット変更している。  

上記を確認するとわかるが、以下のフィールドが自動付与されている。
- id
    - 自動で連番になる主キー
- created_at
    - レコードの作成日時
- updated_at
    - レコードの更新日時

**５．コントローラクラス（hello_controller.rb）を編集**  
DBから全レコード取得するロジックを書く。
```ruby
class HelloController < ApplicationController
  def index
    # render text: 'Hello World !'
    @msg = 'Hello World !'
    render 'hello/sampleView'
  end

  # 以下、追加メソッド
  def users
    @users = User.all
  end
end
```

**６．ビュー用のテンプレートファイル（views/hello/users.html.erb）を手動で作成**  
コントローラで取得したデータをテーブルで表示する。  
```html
<table border='1'>
  <tr>
    <th>No</th>
    <th>名前</th>
    <th>年齢</th>
    <th>アクティブユーザ？</th>
  </tr>

<% @users.each do |user| %>
  <tr>
    <td><%= user.no %></td>
    <td><%= user.name %></td>
    <td><%= user.age %></td>
    <td><%= user.active %></td>
  <tr>
<% end %>

</table>
```

**７．ルーティングの設定（/config/routes.rb）**  
```ruby
Rails.application.routes.draw do
  get 'hello/index'
  get 'hello/users' # この行を追加
end
```

**８．「rails server」をたたく**  
とめていなければ実行する必要はない。  

**９．ブラウザで「http://localhost:3000/hello/users」へアクセス**

■補足
データベースの設定は「**config/database.yml**」にある。

# 用語メモ
- gem
    - Rubyライブラリのパッケージ管理システム
        - Lunuxでいうaptやyumみたいなもの
- bundler
    - gemを管理する仕組み
    - Gemfileでgemパッケージのバージョン指定などが行える
- Rake
    - Rubyで記述された様々なタスクをコマンド実行できるツール
- RVM(Ruby Version Manager)
    - 複数バージョンのRuby実行環境を共存させる仕組み

# 参考
- [Railsドキュメント](http://railsdoc.com/)
- [Ruby on Rails チュートリアル（日本語）](http://railstutorial.jp/)


<table>
<tr>
<td>
<iframe src="https://rcm-fe.amazon-adsystem.com/e/cm?t=tanakakns-22&o=9&p=8&l=as1&asins=4797386290&ref=qf_sp_asin_til&fc1=000000&IS2=1&lt1=_blank&m=amazon&lc1=0000FF&bc1=000000&bg1=FFFFFF&f=ifr" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
</td>
<td>
<iframe src="https://rcm-fe.amazon-adsystem.com/e/cm?t=tanakakns-22&o=9&p=8&l=as1&asins=4774164100&ref=qf_sp_asin_til&fc1=000000&IS2=1&lt1=_blank&m=amazon&lc1=0000FF&bc1=000000&bg1=FFFFFF&f=ifr" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
</td>
<td>
<iframe src="https://rcm-fe.amazon-adsystem.com/e/cm?t=tanakakns-22&o=9&p=8&l=as1&asins=4774165166&ref=qf_sp_asin_til&fc1=000000&IS2=1&lt1=_blank&m=amazon&lc1=0000FF&bc1=000000&bg1=FFFFFF&f=ifr" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
</td>
</tr>
</table>
