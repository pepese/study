Node.jsのWebフレームワーク ```Express``` 触ってみた。  
下記の記事を読んだテイで書く。

[http://blog.pepese.com/entry/2017/02/16/141653:embed:cite]


# インストール

## Expressのインストール

```sh
$ npm install express --save
```

Express単体を入れる時はこれ。  
以降の説明は ```express-generator``` を使うので上記はやらなくていい。

## express-generatorのインストール

```express-generator``` は Expressベースのアプリ雛形を作成するツール。  
RailsのScaffoldのようなもの。

```sh
$ npm install -g express-generator
$ ndenv rehash # ndenvを使っている人は
```

# サンプルアプリの作成

以下の順で記載する。

- express-generatorの使い方
- express-generatorで雛形作成
- ディレクトリ構造
- 実行と確認
- コードを見てみる

## express-generatorの使い方

```sh
$ express --help

  Usage: express [options] [dir]

  Options:

    -h, --help           output usage information
        --version        output the version number
    -e, --ejs            add ejs engine support
        --pug            add pug engine support
        --hbs            add handlebars engine support
    -H, --hogan          add hogan.js engine support
    -v, --view <engine>  add view <engine> support (ejs|hbs|hjs|jade|pug|twig|vash) (defaults to jade)
    -c, --css <engine>   add stylesheet <engine> support (less|stylus|compass|sass) (defaults to plain css)
        --git            add .gitignore
    -f, --force          force on non-empty directory
```

## express-generatorで雛形作成

ExpressのデフォルトのView Template Engine は ```Jade``` だが、 Jadeは ```Pug``` にリネームされた。  
今後、ExpressのデフォルトのView Template EngineはPugに置き換えられるため、ここでは ```Pug``` を使用する。

```sh
$ express express-sample --view=pug --git

   create : express-sample
   create : express-sample/package.json
   create : express-sample/app.js
   create : express-sample/.gitignore
   create : express-sample/public
   create : express-sample/public/javascripts
   create : express-sample/public/stylesheets
   create : express-sample/public/stylesheets/style.css
   create : express-sample/public/images
   create : express-sample/routes
   create : express-sample/routes/index.js
   create : express-sample/routes/users.js
   create : express-sample/views
   create : express-sample/views/index.pug
   create : express-sample/views/layout.pug
   create : express-sample/views/error.pug
   create : express-sample/bin
   create : express-sample/bin/www

   install dependencies:
     $ cd express-sample && npm install

   run the app:
     $ DEBUG=express-sample:* npm start

$ cd express-sample/
$ npm install
```

```express <プロジェクト名>``` コマンドでExpressの雛形プロジェクトが作成される。  
npmのpackage.jsonが作成されているので「npm install」でモジュールをインストールする。  
```npm start``` コマンド（package.jsonにて定義）で ```node ./bin/www``` が実行され、サービスが起動する。  
「./bin/www」には、Express.jsでHTTPサービス（3000番ポート）が起動するJavaScriptがコーディングされており、「app.js」に記述されたサービスが実行される。

## ディレクトリ構造

```sh
$ tree
.
├── app.js           # メインJavaScript
├── bin/             # サービス起動JavaScript（www）置き場
├── node_modules/    # npmモジュール置き場
├── package.json
├── public/          # 公開ディレクトリ
│   ├── images/
│   ├── javascripts/
│   └── stylesheets/
├── routes/          # ルーティング先のスクリプト置き場（MVCのCとロジックに相当）
│   ├── index.js
│   └── users.js
└── views/           # View用のコード置き場（MVCのVに相当）
    ├── error.pug
    ├── index.pug
    └── layout.pug
```

## 実行と確認

```sh
# 実行
$ npm start
```

```sh
# 確認
$ curl localhost:3000
<!DOCTYPE html><html><head><title>Express</title><link rel="stylesheet" href="/stylesheets/style.css"></head><body><h1>Express</h1><p>Welcome to Express</p></body></html>
$ curl localhost:3000/users
respond with a resource
```

## コードを見てみる

### app.js

<script src="http://gist-it.appspot.com/https://github.com/pepese/express-sample/blob/master/app.js?footer=0"></script>

- 各種ライブラリのロード（ ```var xxx = require('xxx');``` ）
- ルーティング先コントローラ層ロジックのロード（ ```var xxx = require('./routes/xxx');``` ）
- Express本体オブジェクトの作成と設定（ ```var app = express();``` ）
  - View Template Engine（ ```Pug``` ） の設定
  - ロガー、パーサー、公開ディレクトリの設定
  - ルーティン部先ロジックの設定（ ```app.use('/xxx', xxx);``` ）
  - エラーハンドリングの設定


### index.js

<script src="http://gist-it.appspot.com/https://github.com/pepese/express-sample/blob/master/routes/index.js?footer=0"></script>

### users.js

<script src="http://gist-it.appspot.com/https://github.com/pepese/express-sample/blob/master/routes/users.js?footer=0"></script>
