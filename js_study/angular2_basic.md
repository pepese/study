フロントエンドMVCフレームワーク **Angular** （所謂Angular2）を触ってみた。  
下記の記事のように ```npm install``` しながらプロジェクト作ったらうまくいかなかったので [Angular CLI](https://github.com/angular/angular-cli) を使う。

[http://blog.pepese.com/entry/2017/02/16/141653:embed:cite]

なお、以下の記事で **ndenv** が入ってるテイで書く。

[http://blog.pepese.com/entry/2016/11/23/174208:embed:cite]


# Angular CLI の使い方

## インストール

```sh
$ npm install -g @angular/cli
$ ndenv rehash
$ ng version
                             _                           _  _
  __ _  _ __    __ _  _   _ | |  __ _  _ __         ___ | |(_)
 / _` || '_ \  / _` || | | || | / _` || '__|_____  / __|| || |
| (_| || | | || (_| || |_| || || (_| || |  |_____|| (__ | || |
 \__,_||_| |_| \__, | \__,_||_| \__,_||_|          \___||_||_|
               |___/
@angular/cli: 1.0.0-beta.32.3
node: 7.5.0
os: darwin x64
@angular/common: 2.4.8
@angular/compiler: 2.4.8
@angular/core: 2.4.8
@angular/forms: 2.4.8
@angular/http: 2.4.8
@angular/platform-browser: 2.4.8
@angular/platform-browser-dynamic: 2.4.8
@angular/router: 3.4.8
@angular/cli: 1.0.0-beta.32.3
@angular/compiler-cli: 2.4.8
```

以降、 ```ng``` コマンドでいろいろできるようになる。

## プロジェクト作成と起動

```sh
$ ng new angular-sample
$ cd angular-sample
$ ng serve
```

## プロジェクト構成

```
.
├── README.md
├── .angular-cli.json                * Angular CLI 設定ファイル
├── .editorconfig                    * エディタの設定ファイル
├── .gitignore
├── e2e                              * エンドツーエンドテストディレクトリ
│   ├── app.e2e-spec.ts
│   ├── app.po.ts
│   └── tsconfig.json
├── karma.conf.js                    * karma（テストランナー）の設定ファイル
├── package.json                     * npmの設定ファイル
├── protractor.conf.js               * e2eテストの設定ファイル
├── tslint.json                      * TypeScriptのLinter設定ファイル
└── src                              * ソースファイルディレクトリ
    ├── app
    │   ├── app.component.css
    │   ├── app.component.html
    │   ├── app.component.spec.ts
    │   ├── app.component.ts
    │   └── app.module.ts
    ├── assets
    ├── environments
    │   ├── environment.prod.ts
    │   └── environment.ts
    ├── favicon.ico
    ├── index.html
    ├── main.ts                       * ルートモジュールのロード（メイン）
    ├── polyfills.ts
    ├── styles.css
    ├── test.ts
    └── tsconfig.json                 * TypeScriptの設定ファイル
```

```.ditorconfig``` は[EditorConfig](http://editorconfig.org)というプロジェクトの設定ファイルで、エディタへプラグインなどを入れることにより様々なエディタの設定をプロジェクト単位で可能にする。  
テストは **Jasmine** （テスティングフレームワーク）と **Karma** （テストランナー）の組み合わせで実行される。

## 主要なコマンド

- ```ng new```
  - 新規にAngularプロジェクトを作成する
- ```ng build```
  - ```src``` 配下をビルドして ```dist``` へ出力する
- ```ng serve```
  - Webサーバを起動してアプリケーションを実行する
- ```ng generate```
  - blueprints から選択して新たにコードを生成する
- ```ng eject```
  - ```ng generate``` などで作成したコードを取り除き、webpackの設定やスクリプトを変更する
- ```ng get```
  - 設定（key-value）から値（value）を取得する
- ```ng set```
  - 設定（key-value）に値（value）を追加する
- ```ng lint```
  - Linterを実行する
- ```ng test```
  - テストを実行する
- ```ng e2e```
  - e2eテストを実行する
- ```ng xi18n```
  - コードからi18nメッセージを抜く

上記以外や詳細なオプションは ```ng help``` を参照。

### blueprints の種類

|blueprints|command|description|
|:---|:---|:---|
|component| ```ng generate component sample``` | コンポーネント（ ```sample.component.ts``` ）を作成する。|
|directive|a|a|
|pipe     |a|a|
|service  | ```ng generate service sample``` | コンポーネント（ ```sample.service.ts``` ）を作成する。|
|class    |a|a|
|interface|a|a|
|enum     |a|a|
|module   |a|a|

```ng generate component my-new-component``` みたいな感じで使える。  
ディレクトリを切りたい場合は ```/``` を加えて、 ```ng generate service sample/sample-service``` のようにする。

## Angular CLI のバージョンアップ方法

グローバルでは以下。

```sh
npm uninstall -g @angular/cli
npm cache clean
npm install -g @angular/cli@latest
```

ローカルプロジェクトでは以下。

```sh
rm -rf node_modules dist # use rmdir on Windows
npm install --save-dev @angular/cli@latest
npm install
```

# Angularのアーキテクチャ

Angularのアーキテクチャは以下から構成される。

<img src="https://angular.io/resources/images/devguide/architecture/overview2.png" />

- モジュール（@NgModule）
- コンポーネント（@Components）
- テンプレート（Templates）
- メタデータ（Metadata）
- データバインディング（Data binding）
- ディレクテイブ（Directives）
- サービス（Services）
- DI（Dependency injection）

## モジュール（@NgModule）

Angularのアプリケーションはモジュール単位で機能を管理する。  
モジュールは後述の **コンポーネント** や **テンプレート** を包含する。

Angularアプリケーションは少なくとも１つ **ルートモジュール** を持ち、```AppModule``` と命名する。  
ルートモジュールは、 ```src/main.ts``` からロードされる。  
通常のアプリケーションの場合、ルートモジュールは１つだが、巨大なアプリケーションの場合、複数のルートモジュールを持つこともある。

モジュールは以下のように作成する。

```typescript
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

@NgModule({
  imports:      [ BrowserModule ],
  providers:    [ Logger ],
  declarations: [ AppComponent ],
  exports:      [ AppComponent ],
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```

```AppModule``` クラスに ```@NgModule``` アノテーションを付与した形になっている。  
アノテーションにAngularに則した設定を行い、クラスには通常通り任意のフィールド、メソッドを定義する。  
アノテーションのプロパティは以下の通り。ちなみにアノテーションに設定するプロパティを **メタデータ** という。

- ```imports```
  - このモジュールに他のモジュールを取り込み、このモジュール内で定義されているコンポーネントやテンプレートが他のモジュールのクラスを使用できるようになる。
  - 他のモジュールの ```providers``` ・ ```exports``` に定義されたものを使用できるようになる。
- ```providers```
  - creators of services that this module contributes to the global collection of services; they become accessible in all parts of the app.
  - このモジュールおよび関係するコンポーネント・サービスへインジェクトするためのサービスを定義・インスタンス化する。
- ```declarations```
  - the view classes that belong to this module. Angular has three kinds of view classes: components, directives, and pipes.
  - このモジュールが所属させるビュークラスを指定する。Angularは、コンポーネント、ディレクティブ、パイプの３種類のビュークラスを持つ。
- ```exports```
  - the subset of declarations that should be visible and usable in the component templates of other modules.
  - 他のモジュールのコンポーネントやテンプレードで使用可能となるサブセットの定義。
  - このモジュール内のクラスを他のモジュールから使用可能にする。 ```imports``` の逆。
- ```bootstrap```
  - the main application view, called the root component, that hosts all other app views. Only the root module should set this bootstrap property.
  - **ルートコンポーネント** を定義する。ルートコンポーネントはアプリケーションのメインビュー。 **ルートモジュールにだけ** ```bootstarp``` プロパティを設定する。

以下が **ルートモジュール** を指定する方法（ ```src/main.ts``` ）。

```typescritp
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app/app.module';

platformBrowserDynamic().bootstrapModule(AppModule);
```

また、モジュール内では以下のようにライブラリから他のモジュールをロードすることもできる。

```typescript
import { Component } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
```

ロードしたライブラリモジュールは、 ```@NgModule``` の ```imports``` プロパティに指定することにより使用できるようになる。

```typescript
imports:      [ BrowserModule ],
```

## コンポーネント

コンポーネントはビューの役割を担う。  
コンポーネントクラス内で定義したフィールド変数やメソッドは、テンプレートで直接使用できる。

```typescript
@Component({
  moduleId: module.id,
  selector:    'hero-list',
  templateUrl: './hero-list.component.html',
  providers:  [ HeroService ]
})
export class HeroListComponent implements OnInit {
/* . . . */
}
```

## テンプレート

テンプレートはコンポーネントのビューとして使われるHTML。  
コンポーネントクラスの ```@Component``` アノテーションのメタデータ直接記載（ ```template``` ）することも、別ファイルとして作成して読み込む（ ```templateUrl``` ）ことも可能。  
テンプレート内では、HTMLの属性として **ディレクティブ** （ ```*ngFor``` など）を使用することができる。

- コンポーネントフィールドへのアクセス
  - ```*ngFor``` ： コレクションフィールドの値にアクセス
   - ```*ngFor="let hero of heroes"``` ： コレクションheroesの各要素をheroへ代入
- コンポーネントのメソッド呼び出し
  - ```(click)="onClickMe()"``` ： クリック時に ```onClickMe()``` メソッドを呼び出す
  - ```(keyup)="onKey($event)"``` ： キーアップ時にイベントを引数に ```onKey()``` メソッドを呼び出す

また、テンプレートにはスタイルシート（CSS、Sassなど）を指定することができ、```@Component``` アノテーションのメタデータ（ ```styles``` 、 ```styleUrls``` ）で指定することができる。

## メタデータ

メタデータはアノテーションに設定するプロパティ。  
Angularにクラスがどのように挙動するか知らせる役割。  
```@Component``` 内に以下のように設定できる。

```typescript
@Component({
  moduleId: module.id,
  selector:    'hero-list',
  templateUrl: './hero-list.component.html',
  providers:  [ HeroService ]
})
export class HeroListComponent implements OnInit {
/* . . . */
}
```

- moduleId
  - sets the source of the base address (module.id) for module-relative URLs such as the templateUrl.
- selector
  - CSS selector that tells Angular to create and insert an instance of this component where it finds a <hero-list> tag in parent HTML. For example, if an app's HTML contains <hero-list></hero-list>, then Angular inserts an instance of the HeroListComponent view between those tags.
- templateUrl
  - このコンポーネントで使用するHTMLの **テンプレート** のパスを指定する。
- providers
  - このコンポーネントがDI経由で使用するサービスクラスを指定する。
  - コンポーネントクラスの ```constructor``` の引数でも指定する必要がある。

```@Injectable``` 、 ```@Input``` 、 ```@Output``` などのアノテーションにもメタデータを設定できる。  
APIリファレンスにて使用できるメタデータを確認できる。  
例えば ```@Component``` のメタデータは[ここ](https://angular.io/docs/ts/latest/api/core/index/Component-decorator.html)で確認できる。


## データバインディング

DOMとコンポーネント間のデータ授受を行う機能。  
以下のように４種類ある。

<img src="https://angular.io/resources/images/devguide/architecture/databinding.png" />

- 単方向バインド（interpolation）
  - ```<li>{{hero.name}}</li>```
  - コンポーネントの ```hero.name``` プロパティの値を ```<li>``` 表示する
- プロパティバインド（property binding）
  - ```<hero-detail [hero]="selectedHero"></hero-detail>```
  - 親コンポーネントの ```selectedHero``` の値を、子コンポーネントの ```hero``` へ渡している
- イベントバインド（event binding）
  - ```<li (click)="selectHero(hero)"></li>```
  - ユーザのクリックにより ```selectHero``` が呼び出される。
- 双方向バインド（Two-way data binding）
  - ```<input [(ngModel)]="hero.name">```
  - ```ngModel``` を用いてproperty bindingとevent bindingを両方同時に実現する。
  - In two-way binding, a data property value flows to the input box from the component as with property binding. The user's changes also flow back to the component, resetting the property to the latest value, as with event binding.

```ngModel``` のようにHTMLタグの属性として記述できるAngularの機能を **ディレクティブ** という。

### イベント

```html
<button (click)="onClickButton()">ボタン1</button>
<button (click)="onClickButtonWithEvent($event)">ボタン2</button>
```

```(click)``` のように、イベント名をカッコで囲んだ属性に、イベントを処理するメソッドを記述する。  
引数なしの記述と、イベントオブジェクト ```$event``` を指定する記述ができる。  
コンポーネントクラス内では以下のようにメソッド定義する。

```typescript
onClickButtonWithEvent(event:any) {
   // targetプロパティでイベントを発生させたオブジェクトを取得
   var button = event.target;
   // ボタンのラベルをtextContentプロパティから取得して表示
   alert("ボタン「" + button.textContent + "」が押下されました");
 }
```

## ディレクティブ

```@Directive``` 。  
動的にテンプレート（というかDOM？）を作る機能。

```
<li *ngFor="let hero of heroes"></li>
<hero-detail *ngIf="selectedHero"></hero-detail>
```

## サービス

サービスは ```export``` された通常のtypescriptクラス。  
後述のインジェクタを使用してコンポーネントや他サービスのコンストラクタ経由でインジェクトする。

```
export class Logger {
  log(msg: any)   { console.log(msg); }
  error(msg: any) { console.error(msg); }
  warn(msg: any)  { console.warn(msg); }
}
```

```
export class HeroService {
  private heroes: Hero[] = [];

  constructor(
    private backend: BackendService,
    private logger: Logger) { }

  getHeroes() {
    this.backend.getAll(Hero).then( (heroes: Hero[]) => {
      this.logger.log(`Fetched ${heroes.length} heroes.`);
      this.heroes.push(...heroes); // fill cache
    });
    return this.heroes;
  }
}
```

### 作成手順

```sh
$ ng generate service sample/sample
installing service
  create src/app/sample/sample.service.spec.ts
  create src/app/sample/sample.service.ts
  WARNING Service is generated but not provided, it must be provided to be used
```

上記で作成した ```src/app/sample/sample.service.ts``` をロードするには、使用するモジュールとコンポーネントにロードする。

```typescript
import { SampleService } from './sample/sample.service' // インポート
...
@NgModule({
  ...
  providers: [SampleService], // providers に追加
  ...
})
export class AppModule { }
```

```typescript
...
import { SampleService } from './sample/sample.service' // インポート

@Component({
  ...
})
export class AppComponent {
  title: string;

  constructor(
    sampleService: SampleService // コンストラクタに設定
  ){
    this.title = sampleService.getTitle();
  }
}
```

## インジェクタ

サービスをコンポーネントのコンストラクタ経由でインジェクトする機能。  
以下のようにモジュールの ```providers``` プロパティで使用するインジェクト候補のサービスを定義する。

```
providers: [
  BackendService,
  HeroService,
  Logger
],
```

```@Injectable``` アノテーションがついたサービスクラスは多段Injectすることが可能になる。  
一段Injectはアノテーションが無しでもOK。

## フォームとバリデータ

http://codezine.jp/article/detail/9596?p=4

# APIリファレンス

APIリファレンスは[ここ](https://angular.io/docs/ts/latest/api/)。  
クラスの前に記号がついており、それぞれ以下の意味。

|記号|意味|
|:---|:---|
|D|Directive。ディレクティブ。|
|P|Pipe。|
|@|Decorator。所謂アノテーション。|
|C|Class。|
|I|Interface。|
|F|Function。|
|E|Enum。|
|T|Type Alias。|
|K|Const。|


# その他話題、メモ

## TypeScriptの型解決について

解決方法には以下がある。

- tsd
- typings
- @types

Angularは型定義が含まれているため、上記３つは不要。ただし、他のライブラリを用いる場合 ```@types``` を使用する。
