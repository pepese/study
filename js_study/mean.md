Angular（所謂Angular2）でMEANスタックのアプリを作成してみる。  
と言いつつMongoDBについてはほぼ言及しない。

# プロジェクト作成手順

以下のアプリケーションを1つのプロジェクト内に作成する。

- フロントエンド
  - Angularベースのブラウザアプリケーション
  - サーバサイドのREST APIで通信する
- サーバサイド
  - NodeJS、ExpressベースのWebアプリケーション

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

### Expressの追加

```sh
$ npm npm install -g forever
$ npm install express@5.0.0-alpha.5 body-parser cookie-parser debug morgan pug serve-favicon request fs --save
$ mkdir app                     // サーバサイドExpressアプリ用のソースディレクトリ作成
$ touch app/app.js              // Expressアプリケーション起動ポイントの作成
$ mkdir app/api                 // REST API用のコントローラディレクトリ作成
$ touch app/api/index.js
$ mkdir app/controllers         // VIEW用のコントローラディレクトリ作成
$ touch app/controllers/.gitkeep
$ mkdir app/models              // Mongoose（MongoDB）用のモデルディレクトリ作成
$ touch app/models/.gitkeep
$ mkdir app/repositories        // DAO/Repository用のディレクトリ作成
$ touch app/repositories/.gitkeep
$ mkdir app/views               // 画面用のディレクトリ作成
$ touch app/views/.gitkeep
$ mkdir app/config              // 設定ファイル用ディレクトリ作成
$ touch app/config/config.json  // 環境差分ファイル作成
$ mkdir app/log                 // ログ出力用ディレクトリ作成
```

### app/app.js

```
```

### app/api/index.js

```
```

### app/config/config.json

```
```


## 起動

```
$ ng build // フロントエンドのビルド
$ NODE_ENV=production forever start app/app.js  // サーバサイドアプリ起動
```

```NODE_ENV``` で環境を指定することができる。デフォルトは ```development``` 。
