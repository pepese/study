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

<script src="http://gist-it.appspot.com/https://github.com/pepese/js-sample/blob/master/express-sample/app.js?footer=0"></script>

- 各種ライブラリのロード（ ```var xxx = require('xxx');``` ）
- ルーティング先コントローラ層ロジックのロード（ ```var xxx = require('./routes/xxx');``` ）
- Express本体オブジェクトの作成と設定（ ```var app = express();``` ）
  - View Template Engine（ ```Pug``` ） の設定
  - ロガー、パーサー、公開ディレクトリの設定
  - ルーティン部先ロジックの設定（ ```app.use('/xxx', xxx);``` ）
  - エラーハンドリングの設定


### index.js

<script src="http://gist-it.appspot.com/https://github.com/pepese/js-sample/blob/master/express-sample/routes/index.js?footer=0"></script>

### users.js

<script src="http://gist-it.appspot.com/https://github.com/pepese/js-sample/blob/master/express-sample/routes/users.js?footer=0"></script>

# ちょっとExpressの機能解説

## Expressアプリケーション

Expressアプリケーションの最小単位は以下のように作成する。

```javascript
const express = require('express');
const app = express();
```

## ミドルウェア関数

Expressは、単独では最小限の機能を備えたルーティングとミドルウェアのWebフレームワーク。  
Expressアプリケーションは様々な **ミドルウェア関数** 呼び出しを行うことで様々な機能を実現する。  
ミドルウェア関数はコールバック関数で以下の種類がある。

- アプリケーション・レベルのミドルウェア
- ルーター・レベルのミドルウェア
- エラー処理ミドルウェア
- 標準装備のミドルウェア
- サード・パーティー・ミドルウェア

### アプリケーション・レベルのミドルウェア

以下のように記述する。

```javascript
app.use([path,] callback [, callback...])
```

```path``` を記載しない場合は **全てのリクエスト** に対してミドルウェア関数（ ```callback``` :コールバック関数）が実行される。  
コールバック関数には ```(req, res, next)``` の3つを渡すことができる。
```next()``` を実行する次のミドルウェア関数に処理が移る。（ **```next()``` を実行しないと処理は終わる** ）

```javascript
app.use( (req, res, next) => {
  console.log('Time:', Date.now());
  next();
});
```

```path``` を記載すると **指定したパスへのリクエスト** に対してミドルウェア関数が実行される。  

```javascript
app.use('/user/:id', (req, res, next) => {
  console.log('Request Type:', req.method);
  next();
});
```

```javascript
app.get('/user/:id', (req, res, next) => {
  res.send('USER');
});
```

**```app.use``` は ```app.[all|get|post|put|delete|etc]``` よりも全て前に記述する必要がある** 。

### ルーター・レベルのミドルウェア

ルーター・レベルのミドルウェアは、express.Router() のインスタンスにバインドされる点を除き、アプリケーション・レベルのミドルウェアと同じ。

```javascript
const express = require('express');
const router = express.Router();
```

正直、 **アプリケーション・レベルとルーター・レベルの違いはよくわからない** 。  
以下の方針で使い分けることにする。

- アプリケーション・レベル
  - **アプリ全体** 、 **全てのHTTPメソッド** へ影響する処理
  - ```app.use()``` を使用する
- ルーター・レベル
  - **特定のパス** 、 **特定のHTTPメソッド** へのルーティング
  - ```router.[all|get|post|put|delete](path, callback [, callback...])``` を使用する

### エラー処理ミドルウェア

引数は ``` (err、req、res、next)``` となり ```err``` が追加される。

```javascript
app.use( (err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```

### 標準装備のミドルウェア

Expressの標準装備のミドルウェア関数は ```express.static``` のみ。  
過去はもったあったらしいが、個別モジュール化されたらしい。

```javascript
express.static(root, [options])
```

静的コンテツを提供する機能。  
```root``` 引数は、静的コンテンツを提供するルートディレクトリを指定する。  
```options``` オブジェクトにはプロパティを指定することができる。

```javascript
var options = {
  dotfiles: 'ignore',
  etag: false,
  extensions: ['htm', 'html'],
  index: false,
  maxAge: '1d',
  redirect: false,
  setHeaders: function (res, path, stat) {
    res.set('x-timestamp', Date.now());
  }
}

app.use(express.static('public', options));
```

### サード・パーティー・ミドルウェア

```npm``` でモジュールをインストールして、アプリケーション・レベルまたはルーター・レベルでロードして使用する。

```javascript
const express = require('express');
const bodyParser = require('body-parser'); // サード・パーティー・ミドルウェア

const app = express();

// Parsers for POST data
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
```

## モジュール呼び出し

```module.exports``` と ```require``` を使用すると、モジュールの外部参照が可能になる。  
以下のような ```index.js``` があったとすると

```javascript
router.post('/', (req, res) => {
  ...
}
module.exports = router; // 外部モジュールからrequireできるようになる
```

```app.js``` から以下のように ```index.js``` の機能を呼び出すことができる。

```javascript
const index = require('./index');
```

## Expressアプリケーションの設定

Expressでは、以下の記載で「key-value」形式でアプリケーションに対して様々な設定ができる。

```javascript
app.set(name, value);
app.get(name);
```

代表的な設定項目としては以下（[詳細](http://expressjs.com/ja/api.html#app.settings.table)）があるが、単にインメモリキーバリューストア（好きな「key-value」を設定できる）としても使用できる。

|Property|Type|Description|Default|
|:---:|:---:|:---|:---:|
|case sensitive routing|Boolean|ルーティング時、URLの大文字・小文字を区別する。|N/A (undefined)|
|env|String|環境名。productionとかdevelopmentとか。|process.env.NODE_ENV or “development”|
|etag|Varied| ```Etag``` レスポンスヘッダの設定を行う。|weak|
|jsonp callback name|String|JSONP callback名を設定する。|“callback”|
|json replacer|Varied|```JSON.stringify``` で利用される引数 ```replacer``` の設定。JSONの各メンバの値の変換の設定ができる。|N/A (undefined)|
|json spaces|Varied|```JSON.stringify``` で利用される引数 ```space``` の設定。JSONパース時に見やすいようにインデントのスペースを設定できる。|N/A (undefined)|
|query parser|Varied|URLクエリパラメータのパースの設定ができる。 ```simple``` 、 ```extended``` 、カスタマイズから選択できる。 ```simple``` はNodeJSが内包するNativeのクエリパーサ。 ```extended``` は サードパーティモジュール ```qs``` 。カスタマイズは関数を設定し、その関数はクエリパラメータの文字列を受け取り、キー・バリューのオブジェクトを返却するよう実装する。|"extended"|
|strict routing|Boolean|ルーティングにおいてURLの ```/foo``` と ```/foo/``` を区別するかどうか設定する。|N/A (undefined)|
|subdomain offset|Number|サブドメインにアクセスするために削除するホストのドット区切り部分の数。|2|
|trust proxy|Varied|信頼するWebプロキシの設定を行う。詳細は、[ここ](http://expressjs.com/ja/guide/behind-proxies.html)を参照。|false (disabled)|
|views|String or Array|Viewを配置するディレクトリを文字列もしくは配列で設定する。|process.cwd() + '/views'|
|view cache|Boolean|Viewテンプレートコンパイルキャッシュを有効にする。|true in production, otherwise undefined.|
|view engine|String|Viewエンジンの設定を行う。|N/A (undefined)|
|x-powered-by|Boolean|"X-Powered-By: Express" HTTPヘッダを有効にする。|true|

## モジュール間で変数をやりとりする方法

- http://memo.sugyan.com/entry/20120110/1326197416
- NodeJSでglobal変数を扱う方法
  - ```var global.hoge = 'hogehoge';``` みたいに、頭にglobalを付けるとglobalスコープで扱われる。
  - ただし、 ```require('./global.js');``` のようにグローバル変数を定義したモジュールを使用するモジュールでロードする必要がある。
