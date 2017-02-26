JavaScriptについて軽くまとめる。

# 変数宣言

| |const|let|var|
|:---|:---|:---|:---|
|スコープ|ブロック|ブロック|関数|
|再代入|不可能|可能|可能|
|用途|定数|変数|使用しない|

# データ型

- プリミティブ型（基本型）
  - 真偽値（Boolean）: trueまたはfalseのデータ型
  - 数値（Number）: 42 や 3.14159 などの数値のデータ型
  - 文字列（String）: "JavaScript" などの文字列のデータ型
  - undefined: 値が未定義であることを意味するデータ型
  - null: 値が存在しないnull値を意味するデータ型
  - シンボル（Symbol）: ES2015から追加された一意で不変な値のデータ型
- オブジェクト（複合型)
  - プリミティブ型以外のデータ
  - オブジェクト、配列、関数、正規表現、Dateなど

型変換は ```String([1, 2, 3])``` のように行う。  
また、数値の詳細な変換は以下。

```javascript
// "1"をパースして10進数として取り出す
Number.parseInt("1", 10); // => 1;
// 余計な文字はパース時に無視して取り出す
Number.parseInt("42px", 10); // => 42
Number.parseInt("10.0", 10); // => 10
Number.parseFloat("1"); // => 1
Number.parseInt("42.0px"); // => 42.0
Number.parseFloat("10.0"); // => 10.0
```

型の確認は以下。

```javascript
typeof true;// => "boolean"
typeof 42; // => "number"
typeof "JavaScript"; // => "string"
typeof Symbol("シンボル");// => "symbol"
typeof undefined; // => "undefined"
typeof null; // => "object"
typeof ["配列"]; // => "object"
typeof { "key": "value" }; // => "object"
typeof function() {}; // => "function"
```

# ループ

## Array#forEach

```javascript
var array = [1, 2, 3, 4, 5];
array.forEach((currentValue, index, array) => {
    // 実行する文
});

[1, 2, 3].forEach(currentValue => {
    console.log(num);
});
// 1
// 2
// 3
// と順番に出力される
```

## Array#some

配列をループし、検査を１つでもパスする要素があれば ```ture``` を返却する。

```javascript
var isPassed = array.some((currentValue, index, array) => {
  // 検査を行う処理を記載する。
  // 検査をパスするならtrue、そうでないならfalseを返す
});
```

## Array#filter

配列を全ループし、検査をパスする要素のみ返却する。

```javascript
var filterdArray = array.filter((currentValue, index, array) => {
  // 検査を行う処理を記載する。
  // 検査をパスするならtrue、そうでないならfalseを返す
});
```

## for...in

オブジェクトのループに使用する。  
key-valueの **key** が取り出される。

```javascript
for (variable in object)
    実行する文;
```

## for...of [ES6]

オブジェクトのループに使用する。  
key-valueの **value** が取り出される。

```javascript
for (variable of iterable)
    実行する文;
```

# オブジェクト

所謂 **JSON** 。  
**Symbol** を利用する場合は以下。

```javascript
var object = {
    key1: "value1"
};
// ドット記法で参照
console.log(object.key1); // => "value"
// ブラケット記法で参照
console.log(object["key1"]); // => "value"
// `key2`プロパティを追加し値を代入
object.key2 = "value2";
console.log(object.key); // => "value2"
```

**Symbol** を利用しない場合は以下。

```javascript
var object = {
    "ja": "日本語",
    "en": "英語"
};
var myLang = "ja";
console.log(object[myLang]); // => "日本語"
var key = "key-string";
// `key`の評価結果 "key-string" をプロパティ名に利用
object[key] = "value of key";
// 取り出すときも同じく`key`変数を利用
console.log(object[key]); // => "value of key"
```

プロパティにアクセスしても存在しない場合は ```undefined``` が返却される。

# 関数

## 通常の関数

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

## 無名関数

無名関数は、通常 ```var hoge = function hoge(){...};``` のように書くところを冗長な ```hoge``` を省略して以下のように書くことをいう。

```js
var hoge = function (){...};
```

ちなみに以下のように書くと即時関数になる。

```js
var hoge = function (){...}();
```

## 即時関数

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

## Arrow Function [ES6]

```=>``` を使うことで匿名関数を定義できるArrow Functionがある。

```js
const 関数名 = () => {
    // 関数を呼び出した時の処理
    // ...
    return 関数の返す値;
};
```

# ブラウザオブジェクト

ブラウザに組み込まれたオブジェクト。

- [参考](http://web-design-felica.hatenablog.com/entry/20160511/p1)
- [公式](https://developer.mozilla.org/ja/docs/Web/API/Window)
- [es6-features](https://codetower.github.io/es6-features/)

# 参考

ES5（2011年策定）  
ECMAScript 2015（ECMAScript 6）

- [[WIP] JavaScriptの入門書](https://asciidwango.github.io/js-primer/)
- [ES2015で始めるJavaScript入門](http://qiita.com/ABCanG1015/items/824681cb88676da4f9a8)
- [JavaScriptオブジェクトの作成パターン ES6時代のベストプラクティス](https://getpocket.com/a/read/1437030710)
- [JavaScript初級者のためのコーディングガイド](http://qiita.com/raccy/items/bf590d3c10c3f1a2846b)
- [「JavaScript初級者のためのコーディングガイド」に補足を試みる](http://qiita.com/ms2sato/items/94ed459640a1d89cb4de?utm_source=Qiitaニュース&utm_campaign=383650bdcf-Qiita_newsletter_242_11_1_2017&utm_medium=email&utm_term=0_e44feaa081-383650bdcf-32789017)
- [Javascriptを理解したい時にみるとはかどる記事まとめ](http://qiita.com/okmttdhr/items/3b35ebe6017fe4057ddc)

過去のjsまとめは[これ](http://blog.pepese.com/entry/20130321/1363854485)。

ES6のブラウザ対応状況は、[compat-table](http://kangax.github.io/compat-table/es6/)がわかりやすい。  
NodeJSの対応状況は、[node.green](http://node.green)。
