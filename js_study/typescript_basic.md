**TypeScript** について環境構築からトランスパイス、設定ファイルなどをまとめる。

# 環境構築

```sh
$ npm install -g typescript@latest
$ ndenv rehash$ tsc -v
Version 2.3.2
```

これで `tsc` コマンドを使って TypeScript の `.ts` ファイルをコンパイルできるようになる。

## 実行してみる

以下の `greeter.ts` ファイルを作成する。

```typescript
function greeter(person) {
    return "Hello, " + person;
}

var user = "pepese";

console.log(greeter(user));
```

以下のコマンドでコンパイル〜実行まで。

```sh
$ tsc greeter.ts
$ node greeter.js
Hello, pepese
```

# 設定ファイル

TypeScriptの設定ファイルは `tsconfig.json` 。  
`tsc` コマンドのオプションで様々設定できるが、設定ファイルがあれば省略できる。  


簡単な設定ファイルを作成してみて実行を確認にしてみる。  
カレントディレクトリに以下の **tsconfig.json** を作成して `$ tsc` を実行しみる。

```javascript
{
  "compilerOptions": {
    "baseUrl": "",
    "outDir": "dist",
    "sourceMap": true
  }
}
```

`dist` ディレクトリが作成され、その配下に `greeter.js` と `greeter.js.map` が作成されていることがわかる。

## **tsconfig.json** のパラメータ

```javascript
{
  "compilerOptions": {
  },
  "include": [
    "src/**/*"
  ],
  "exclude": [
    "node_modules",
    "**/*.spec.ts"
  ]
}
```

- compilerOptions
  - [コンパイラ・オプション](https://www.typescriptlang.org/docs/handbook/compiler-options.html)を指定する
  - 省略するとデフォルト値になる
- include / exclude
  - glob風のファイルパターンでコンパイル対象を指定できる
    - `*` は0文字以上の文字にマッチ(ディレクトリ区切りを除く)
    - `?` は1文字以上の文字にマッチ(ディレクトリ区切りを除く)
    - `**/` はサブディレクトリに再帰的にマッチ
  - デフォルトで、`.ts` 、 `.tsx` 、`.d.ts` ファイルが対象で、 `compilerOptions` の `alloJs` パラメータががtrueの場合は `.js` と `.jsx` も対象になる

## compilerOptions のパラメータ

`compilerOptions` 配下の代表的なパラメータは以下。

|パラメータ|説明|
|:---|:---|
|outDir|コンパイルしたコードの出力先。|
|baseUrl|コンパイル対象のファイルのベースディレクトリ。|
|sourceMap| `.map` ファイルを作成するか否か。|
|declaration| `.d.ts` ファイルを作成するか否か。|
|moduleResolution|　|
|emitDecoratorMetadata|　|
|experimentalDecorators|　|
|target|出力するECMAScriptのバージョンを指定。|

[参考](https://www.typescriptlang.org/docs/handbook/compiler-options.html)

### 型定義ファイルの指定

型定義ファイルの指定には、 `compilerOptions` 配下の `typeRoots` 、 `types` で設定する。  
デフォルトでは、 `node_modules/@types/` 配下の `.d.ts` ファイルが読み込まれる。  
`typesRoots` が指定されている場合は、それに含まれるパッケージのみが対象となる。（つまり、 `/node_modules/@types/` 配下は読まれない。）  
`types` が指定された場合、一覧に指定されたパッケージのみが含まれる。（ `@types` パッケージの自動インクルードが無効になる）  
以下の場合は、 `node_modules/@types/node` 、 `node_modules/@types/lodash` 、 `node_modules/@types/express` のみが含まれる。

```javascript
{
   "compilerOptions": {
       "types" : ["node", "lodash", "express"]
   }
}
```
