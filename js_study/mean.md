Angular（所謂Angular2）でMEANスタックのアプリを作成してみる。  
と言いつつMongoDBについてはほぼ言及しない。

# プロジェクト作成手順

以下のアプリケーションを1つのプロジェクト内に同梱して作成する。

- フロントエンド
  - Angularベースのブラウザアプリケーション
  - サーバサイドのREST APIで通信する
  - **Angular CLI** を使用する
  - ソースコードディレクトリは ```src/```
- サーバサイド
  - NodeJS、ExpressベースのWebアプリケーション
  - ソースコードディレクトリは ```app/```

フロントエンド、サーバサイドの順で作成すること。

## フロントエンド

### Angular CLIでプロジェクト作成

SCSS、Bootstrap、jQueryベースで作成する。

```
$ ng new mean2-sample --style=scss
$ cd mean2-sample
$ npm install ng2-bootstrap bootstrap jquery --save
```

### angular-cli.json 修正

```javascript
// 省略
"styles": [
  "../node_modules/bootstrap/dist/css/bootstrap.min.css", // 追加行
  "styles.scss"
],
"scripts": [
  "../node_modules/jquery/dist/jquery.min.js",           // 追加行
  "../node_modules/bootstrap/dist/js/bootstrap.min.js"   // 追加行
],
// 省略
```

bootstrap、jqueryのスクリプト、スタイルシートを追加。

### src/app/app.module.ts 修正

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';
import { Ng2BootstrapModule } from 'ng2-bootstrap/ng2-bootstrap'; // 追加行

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    Ng2BootstrapModule // 追加行
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

[ng2-bootstrap](https://valor-software.com/ng2-bootstrap/#/)のモジュールを追加。  
その他Angularの詳細については以下参照。

[http://blog.pepese.com/entry/2017/03/16/152007:embed:cite]


## サーバサイド

Angular CLIで作成したプロジェクトにサーバサイド、つまりExpressを追加する。  
サーバサイドアプリケーションのソースディレクトリは ```app/``` だけで完結するようにする。  
フロントエンドとは異なりトランスパイル不要。  
フロントエンドトランスパイル後の ```dist``` をExpress側で公開ディレクトリに設定する。

サーバサイドアプリケーションを作成する方法は以下を参照。  
「フロントエンドアプリケーションをプロジェクトに同梱する場合」のほうの構成。

[http://blog.pepese.com/entry/2017/04/06/221119:embed:cite]

## 起動

```
$ ng build                                      // フロントエンドのビルド
$ NODE_ENV=production forever start app/app.js  // サーバサイドアプリ起動
```
