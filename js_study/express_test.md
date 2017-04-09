NodeJS、Expressアプリケーションのテストをしてみる。  
使用するライブラリは以下。

- テスティングフレームワーク
  - Mocha
- アサート
  - Chai
- モック
  - Sinon.JS
- テストランナー
  - Karma
    - いらんかも。。。
- カバレッジ
  - istanbul

- MochaとChai
  - http://qiita.com/y_hokkey/items/f73ea6b3d5f6902396b6

以下の記事を読んだ前提で書く。

[http://blog.pepese.com/entry/2017/04/06/221119:embed:cite]

# インストール

## グローバル

```sh
$ npm install -g mocha chai mocha-sinon gulp
$ ndenv rehash
```

## ローカル

プロジェクトディレクトリで以下を実行。

```sh
$ npm install karma-mocha karma-chai --save-dev // これは違うかも。メモ。
$ npm install mocha should chai sinon gulp-istanbul gulp-mocha --save-dev
```

## ディレクトリ作成

先に紹介した記事の通り、Expressアプリケーションのソースディレクトリは ```app/``` だけで完結するようにする。  
以下のようにテストスクリプト用のディレクトリを作成する。

```sh
$ mkdir app/spec
$ mkdir app/spec/controllers
```

# テストスクリプト作成

先に紹介した記事のスクリプトをテストする。

## gulpfile.js

<script src="http://gist-it.appspot.com/https://github.com/pepese/js-sample/blob/master/express-sample/gulpfile.js?footer=0"></script>

## app/spec/controllers/get_index.spec.js

<script src="http://gist-it.appspot.com/https://github.com/pepese/js-sample/blob/master/express-sample/app/spec/controllers/get_index.spec.js?footer=0"></script>

## app/spec/controllers/get_users.spec.js

```javascript
```

## 実行

```sh
$ gulp test
```
