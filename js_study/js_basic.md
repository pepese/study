# めも

ES5（2011年策定）  
ECMAScript 2015（ECMAScript 6）

- [ES2015で始めるJavaScript入門](http://qiita.com/ABCanG1015/items/824681cb88676da4f9a8)
- [JavaScript初級者のためのコーディングガイド](http://qiita.com/raccy/items/bf590d3c10c3f1a2846b)

過去のjsまとめは[これ](http://blog.pepese.com/entry/20130321/1363854485)。

ES6のブラウザ対応状況は、[compat-table](http://kangax.github.io/compat-table/es6/)がわかりやすい。  
NodeJSの対応状況は、[node.green](http://node.green)。

# 変数宣言

| |const|let|var|
|:---|:---|:---|:---|
|スコープ|ブロック|ブロック|関数|
|再代入|不可能|可能|可能|
|用途|定数|変数|使用しない|

# 関数

通常の関数定義は以下のように行う。

```js
function hoge(){...}
hoge();
```

定義した関数を変数に代入する場合は以下のように行う。

```js
var hoge = function hoge(){...};
hoge();
```

# 無名関数

無名関数は、通常 ```var hoge = function hoge(){...};``` のように書くところを冗長な ```hoge``` を省略して以下のように書くことをいう。

```js
var hoge = function (){...};
```

ちなみに以下のように書くと即時関数になる。

```js
var hoge = function (){...}();
```

# 即時関数

関数の定義と実行が同時になる関数。  
構文は以下。

```js
// 構文１
(function () {
    //関数の中身・・・
}());


//構文２
(function () {
    //関数の中身・・・
})();
```

ただし、 ```(``` や ```function``` で開始していなければ良いので下記のような描き方でも良い。

```js
!function(){}();
+function(){}();
-function(){}();
~function(){}();
0,function(){}();
a=function(){}();
[function(){}()];
1+function(){}();
0|function(){}();
0&function(){}();
0||function(){}();
1&&function(){}();
void function(){}();
typeof function(){}();
delete function(){}();
```

1つ目の ```!function(){}();``` は、即時関数と書き方としてよく見かける。  
また、引数は次のように解釈される。

```js
(function (a, b){...})('aaa', 'bbb');
```

上記の場合、

- 引数aに’aaa’、引数bに’bbb’が代入される

ことになる。

```js
(function (a, b, c, d){...})('aaa', 'bbb');
```

上記の場合、

- 引数aに’aaa’、引数bに’bbb’が代入」されるが
- 引数c、dには何も代入されず、関数内で使用できる単なる変数宣言と解釈される

ことになる。  
メリットは、 ```var c``` とか書かない分、文字数削減となること。


# ブラウザオブジェクト

ブラウザに組み込まれたオブジェクト。

- [参考](http://web-design-felica.hatenablog.com/entry/20160511/p1)
- [公式](https://developer.mozilla.org/ja/docs/Web/API/Window)
- [es6-features](https://codetower.github.io/es6-features/)
