JS初心者だが、AngularでHTTPリクエストを発行する処理を書いてみた。  
Angularについては以下参照。

[http://blog.pepese.com/entry/2017/03/16/152007:embed:cite]

# サンプル

HTTP GETリクエストを発行して画面に結果を出すサンプルを書いてみる。  
モジュールにて以下を記載する。

```typescript
...
import { HttpModule } from '@angular/http';  // インポート
...
@NgModule({
...
  imports: [
    ...
    HttpModule, // インポート
    ...
  ],
...
})
```

コンポーネント（サービスでもいい）にて以下を記載。

```typescript
...
import { Http, Response } from '@angular/http';  // インポート
...
@Component({
  ...
})
export class AppComponent {

  constructor(
    ...
    private http: Http,  // コンストラクタにて設定
    ...
  ){
    ...
  }
  ...
  getSample(event:any) {
    return this.http.get("https://httpbin.org/ip")
                    .map(res => res.json())
                    .subscribe(t => this.resText = t.origin);
  }
  ...
}  
```

テンプレートに以下を記載する。

```html
<button (click)="getSample()">テスト</button>
<br />
<p>{{resText}}</p>
```

以上で「テスト」ボタンを押下するとIPが表示されるサンプル。

# @angular/http

[Httpクラス](https://angular.io/docs/ts/latest/api/http/index/Http-class.html)では以下のメソッドが利用できる。

- request(url: string|Request, options?: RequestOptionsArgs) : Observable<Response>
- get(url: string, options?: RequestOptionsArgs) : Observable<Response>
- post(url: string, body: any, options?: RequestOptionsArgs) : Observable<Response>
- put(url: string, body: any, options?: RequestOptionsArgs) : Observable<Response>
- delete(url: string, options?: RequestOptionsArgs) : Observable<Response>
- patch(url: string, body: any, options?: RequestOptionsArgs) : Observable<Response>
- head(url: string, options?: RequestOptionsArgs) : Observable<Response>
- options(url: string, options?: RequestOptionsArgs) : Observable<Response>

返却は全て ```Observable<Response>``` となっている。  
[Responseクラス](https://angular.io/docs/ts/latest/api/http/index/Response-class.html)のフィールド・メソッドは以下の通り。

- type : ResponseType
- ok : boolean
- url : string
- status : number
- statusText : string
- bytesLoaded : number
- totalBytes : number
- headers : Headers
- toString() : string

また、Responseは[Bodyクラス](https://github.com/angular/http-builds/blob/master/%40angular/http.js)を継承しており、メソッドは以下の通り。

- json()
  - ボディをJSON形式で返却する
- text()
  - ボディを文字列で返却する
- arrayBuffer()
  - ボディをArrayBuffer形式で返却する
- blob()
  - ボディをBlob形式で返却する

[Observableクラス](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html)は以下のようなメソッドを提供しており、レスポンスに対して様々な処理を挟むことができる。

- map
  - レスポンスに対してmap処理、オブジェクトを変換する処理を挟む
- subscribe
  - 任意の処理を記載することができる
  - レスポンスを受けてからの処理は全てここに記載する必要がある
- catch
  - エラー発生時のハンドリング処理を行う

なお、Observableクラスは [RxJS](https://github.com/Reactive-Extensions/RxJS) パッケージ提供となっている。  
Rx(Reactive Extensions)、関数型リアクティブプログラミングの機能を提供する。

# rxjs/Observable

RxJS（Reactive Extensions for JavaScript）は、Observables というアーキテクチャを用いたリアクティブ・プログラミング用のライブラリで非同期処理を簡易に実現する。

# 参考

- http://blog.yuhiisk.com/archive/2016/05/24/angular2-http-client.html
