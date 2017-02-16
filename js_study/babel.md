[webpack](https://webpack.github.io) + [Babel](https://babeljs.io) で ES6（ECMAScript 2015）をトランスパイルしてブラウザで動くまでをまとめる。  
以降を記載する。

- サンプルが動くまで
  - サンプルが動くまでの手順
- 説明
  - 上記の手順の説明

# サンプルが動くまで

ブラウザに「Hello World !」とポップアップが出るサンプルを作成する。

## 前提

```sh
$ npm -v
4.0.2
```

## プロジェクトの作成

```sh
$ mkdir es6-sample
$ cd es6-sample
$ npm init -y
$ npm install --save-dev \
      webpack \
      babel-cli \
      babel-core \
      babel-preset-env \
      babel-preset-es2015 \
      babel-polyfill \
      babel-loader \
      webpack-dev-server
$ mkdir src; touch src/app.js
$ mkdir dist; touch dist/index.html
$ touch webpack.config.js
$ touch .babelrc
```

トランスパイル後のディレクトリ構造は以下のようになる。

```
$ tree -a -I 'node_modules'
.
├── .babelrc
├── dist
│   ├── app.bundle.js
│   ├── app.bundle.js.map
│   └── index.html
├── package.json
├── src
│   └── app.js
└── webpack.config.js
```

以降、各ファイルの内容を記載する。

## package.json

<script src="http://gist-it.appspot.com/https://github.com/pepese/es6-sample/blob/master/package.json?footer=0"></script>

```npm run build``` でES6のトランスパイル、```npm run start``` で簡易Webサーバが起動する。

## .babelrc

<script src="http://gist-it.appspot.com/https://github.com/pepese/es6-sample/blob/master/.babelrc?footer=0"></script>

ここでトランスパイル対象の種類を指定する。

## webpack.config.js

<script src="http://gist-it.appspot.com/https://github.com/pepese/es6-sample/blob/master/webpack.config.js?footer=0"></script>

webpackの設定ファイル。  
```npm run build``` でES6のトランスパイル、```npm run start``` で簡易Webサーバが起動するように ```package.json``` にスクリプトを設定している。

## index.html

<script src="http://gist-it.appspot.com/https://github.com/pepese/es6-sample/blob/master/dist/index.html?footer=0"></script>

トランスパイル後のJSコードをロードしている。

## app.js

<script src="http://gist-it.appspot.com/https://github.com/pepese/es6-sample/blob/master/src/app.js?footer=0"></script>

## 実行方法

1. ```npm run build``` でトランスパイル
1. ```npm run start``` で簡易Webサーバの起動
1. ```http://localhost:8080/webpack-dev-server/``` へブラウザでアクセス
1. 「Hello World !」とポップアップが出れば成功


# 説明

## Babelのインストール

```sh
npm install --save-dev babel babel-preset-es2015
```

```babel``` はトランスパイラだが、これだけだとまだ何をトランスパイルするのか分からない。  
```babel-preset-es2015``` を入れるとES6をトランスパイルできるようになる。  
Babelは ```.babelrc``` というファイルがあると、その設定を読み込んでくれる。

### Babel6での変更？

Babel5からBabel6で ```babel``` が ```babel-core``` と ```babel-cli``` に分割された模様。  
CLIを使用しないのであれば、 ```babel-core``` だけでいい。

## .babelrc

トラインスパイルする内容を指定する場合、以下のように **プリセット（presets）** を指定する。

```js
{
  "presets": [es2015", "react"]
}
```

```babel-preset-2015``` 、 ```babel-preset-react``` を使用する場合は上記のように指定する。  

## Polyfill

```
$ npm install --save-dev babel-polyfill
```

Babelでトランスパイルすると古いブラウザでも動作する Javascript（ES5） を出力するが、古いブラウザのブラウザオブジェクトの中には標準オブジェクト（ES6で記述できるブラウザオブジェクトの扱い方）に対応していないものがある。  
```babel-polyfill``` を利用すると、古いブラウザでも動くように標準オブジェクトを拡張してくれる。  
ちなみに、古いブラウザーに欠けている部分、新しいブラウザーでも足りない機能の穴を埋めることを、 **ポリフィル (polyfill)**  という。  

標準オブジェクトを拡張すると、既存の環境と互換性を失うことになるため、別ライブラリ　```babel-polyfill``` に分かれている。

```babel-polyfill``` の他に ```babel-runtime``` というのがある。  
```babel-polyfill``` が標準オブジェクトを拡張するのに対して、 ```babel-runtime``` は標準オブジェクトの対応していない機能を、その場で別のコードにトランスパイルして解決する。  
```babel-polyfill``` を導入しているほうが多そうな肌感。

## その他のBabelプラグイン

- babel-preset-env
  - [compat-table](http://kangax.github.io/compat-table/es6/)を用いて、サポートされている環境に基づいて必要なBabelプラグインを自動で決定するライブラリ
- babel-loader
  - webpackからbabelを使用できるようにするやつ
- babel-preset-react
  - Reactをトランスパイルするプリセット
- babel-preset-stage-0
  - ECMAScriptのステージを指定できる
  - ステージについては後述
- bable-register
  - 通常のwebpackの設定ファイルは ```webpack.config.js``` というファイルだが、 ```webpack.config.babel.js``` というファイル名でES6で書けるようになる

## ステージ（stage）

BabelはECMAScript2015 (ES6), ECMAScript7をECMAScript5に変換するが、ECMAScript2015 (ES6), ECMAScript7の仕様でもまだ議論中のものもある。  
そこで、ECMAScriptの中でもまだ議論しきれていない仕様に関しては、stageという概念を使って安定度を示している。  
初期値は2が指定されており、Draft版のものが使用できる。  
基本的には初期値で問題無いが、それ以上のものを使いたい場合はリスクを加味して使用すること。  
ステージは以下がある。

|stage|description|
|:---|:---|
|0|Strawman - アイデア|
|1|Proposal - 提案|
|2|Draft - ドラフト|
|3|Candidate- 仕様書と同じ形式|
|4|Finished - 策定完了|

## sourceMaps

トラインスパイル前後のコードをマッピングする。  
デバッグなどする際、エラー箇所がトランスパイル前のコードのどこに対応するのかがわかるようになる。

## webpack

webpackは、複数のファイルの依存関係を顧慮してビルドを行うツール。  
設定ファイルは ```webpack.config.js``` 。

- ```entry``` でトランスパイル対象を指定して、```output``` で出力先を指定する。
- Loaderという仕組みがあり、これでES6やReactをコンパイルできる。
- Pluginという仕組みがあり、これでUglifyなど圧縮処理などができる。PluginはLoaderの前後で実行される。
- WebpackDevServerという確認用簡易Webサーバがある。
