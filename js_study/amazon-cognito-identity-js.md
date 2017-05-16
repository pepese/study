Cognito を使って Angular アプリからユーザ認証する

Amazon Cognito を使ってユーザ認証ができる Angular アプリを作成してみる。  
以下の記事の知識を前提とする。

- あ
- あ

# Amazon Cognito の概要

Amazon Cognito はユーザ認証やソーシャル認証の機能を提供するAWSのマネージドサービス。  
大きく以下の機能がある。

- User Pools （ユーザプール）
  - ユーザーディレクトリを作成・管理し、アプリケーションのサインアップとサインイン機能を提供する
  - メールや電話番号の検証、MFA 認証などの拡張セキュリティ機能有り
  - カスタムアトリビュートを設定できる
- Federated Identities （フェデレーテッドアイデンティティ）
  - ユーザの ID を作成し、フェデレーテッド ID プロバイダー（Amazon、Facebook、Google、TwitterなどのOpenID Connect プロバイダおよびSAML ID プロバイダ)
  で認証できる
- Sync
  - アプリケーション関連のユーザーデータのオフラインでのアクセスとデバイス間の同期機能を提供する

ここでは１つ目の **User Pools** を使用する。

## Amazon Cognito User Pools の作成

以下の手順で User Pools を作成する。

1. 「https://ap-northeast-1.console.aws.amazon.com/cognito/」（Tokyoリージョンの場合）へアクセスする
2. 「Manage your User Pools」をクリック
3. 「Create a User Pool」をクリック
4. 任意の User Pool 名を入力して「Step through settings」をクリック
  - 「Review defaults」を設定すると各種設定画面をスキップしてデフォルト値が設定される
5. [Attribute 設定画面] ユーザプロファイルとして管理したい項目を選択して「Next step」をクリックする
  - ここでは「email」「phone number」を選択する
6. [Policies 設定画面] パスワードの設定ポリシー、ユーザの作成権限ポリシー、使用されないユーザアカウントのTTLを設定して「Next step」をクリックする
7. [Verifications 設定画面] MFAの使用如何、Verification Code の送信先（E-Mail or SMS）、SMS を使用する場合は Cognito へ Role の付与を設定して「Next step」をクリックする
  - ユーザがサインアップした際、 E-Mail か SMS へ数字6桁の Verification Code が送付され初回の本人認証に使用する
  - ここでは email を選択する
8. [Message Customization 設定画面] 送付するメッセージをカスタマイズして「Next step」をクリックする
9. [Tags 設定画面] タグを設定して「Next step」をクリックする
10. [Devices 設定画面] ユーザがアクセスしてきたデバイスを記憶するか否かを設定して「Next step」をクリックする
11. [App clients 設定画面] AWS-SDK を使用したクライアントアプリケーションから User Pool へアクセスする場合は「Add an app client」を実施して「Next step」をクリックする
  - Client の作成では、App client 名、 Client のシークレットキーを使用するか否か、認証用の HTTP ヘッダを使用するか否かを設定して」「Create app client」をクリックする
  - ここでは「Generate client secret」のチェックボックスを外し、シークレットキーを使用しないこととする
12. [Triggers 設定画面] 認証フローの各種チェックポイントで AWS Lambda を実行するか否かを設定して「Next step」をクリックする
13. [Review 画面] 最後にこれまでの設定を確認して「Create pool」をクリックする

以上の手順で User Pools が作成できる。  
Pool details 画面の **Pool Id** と、 App clients 画面の **App client id** は後ほど使用するのでひかえておく。

# クライアント側（SPA）の作成

```sh
$ npm upgrade -g @angular/cli
$ ndenv rehash
$ ng new cognito-js
$ cd cognito-js
$ npm install amazon-cognito-identity-js --save # 依存から aws-sdk も導入される
$ ng generate service services/cognito
```

`aws-sdk` には `@type/aws-sdk` が含まれている模様。（[参考](https://www.npmjs.com/package/@types/aws-sdk)）  
`src/tsconfig.app.json` を以下のように編集。

```javascript
{
  "extends": "../tsconfig.json",
  "compilerOptions": {
    "outDir": "../out-tsc/app",
    "module": "es2015",
    "baseUrl": "",
    "types": ["node"] # ここを加筆
  },
  "exclude": [
    "test.ts",
    "**/*.spec.ts"
  ]
}
```

`src/app/services/cognito.service.ts` を以下のように修正。

```typescript
import { Injectable } from '@angular/core';
import * as AWS from "aws-sdk";
import { CognitoUserPool, CognitoUserAttribute, CognitoUser, AuthenticationDetails } from 'amazon-cognito-identity-js';
import { environment } from '../../environments/environment';

@Injectable()
export class CognitoService {
  userPool = null;

  constructor() {
    AWS.config.region = environment.region;
    const data = { UserPoolId: environment.userPoolId, ClientId: environment.clientId};
    this.userPool = new CognitoUserPool(data);
  }

  signUp(username, password, email, phone) {
    const userData = {
      Username : username,
      Pool : this.userPool
    };
    let attributeList = [];
    const dataEmail = {
      Name : 'email',
      Value : email
    };
    const dataPhoneNumber = {
      Name : 'phone_number',
      Value : phone
    };
    let attributeEmail = new CognitoUserAttribute(dataEmail);
    let attributePhoneNumber = new CognitoUserAttribute(dataPhoneNumber);
    attributeList.push(attributeEmail);
    attributeList.push(attributePhoneNumber);
    this.userPool.signUp(username, password, attributeList, null, function(err, result){
      if (err) {
        console.log(err);
        return;
      }
      const cognitoUser = result.user;
      console.log('user name is ' + cognitoUser.getUsername());
    });
    return;
  }

  confirmRegistration(username, verification_code) {
    const userData = {
      Username : username,
      Pool : this.userPool
    };
    const cognitoUser = new CognitoUser(userData);
    cognitoUser.confirmRegistration(verification_code, true, function(err, result) {
      if (err) {
        alert(err);
        return;
      }
      console.log('call result: ' + result);
    });
    return;
  }

  signIn(username, password) {
    const userData = {
      Username : username,
      Pool : this.userPool
    };
    const cognitoUser = new CognitoUser(userData);
    const authenticationData = {
        Username : username,
        Password : password
    };
    const authenticationDetails = new AuthenticationDetails(authenticationData);
    cognitoUser.authenticateUser(authenticationDetails, {
      onSuccess: function (result) {
        console.log('access token + ' + result.getAccessToken().getJwtToken());
      },
      onFailure: function(err) {
        alert(err);
      }
    });
    return;
  }
}
```

`src/app/app.component.ts` を以下のように修正。

```typescript
import { Component } from '@angular/core';
import { CognitoService } from './services/cognito.service'; // 追加

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
  providers: [CognitoService] // 追加
})
export class AppComponent {
  title = 'app works!';

  constructor(
    private cognitoService: CognitoService # 追加
  ){
    // 好きなこと書く
    //cognitoService.signUp('username', 'password', 'xxxx@xxxx.com', '+819012345678');
    //cognitoService.confirmRegistration('username', '123456');
    //cognitoService.signIn('username', 'password');
  }
  // 好きなこと書く
}
```

`src/environments/environment.ts` を以下のように修正。

```typescript
export const environment = {
  production: false,
  region: 'ap-northeast-1',
  userPoolId: 'ap-northeast-1_xxxxxxxxx',
  clientId: 'xxxxxxxxxxxxxxxxxxxxxxxxxx'
};

```

`$ ng serve` するとエラー無く起動するところまで確認。


# その他参考

- チュートリアル: JavaScript アプリのユーザープールを統合する
  - http://docs.aws.amazon.com/ja_jp/cognito/latest/developerguide/tutorial-integrating-user-pools-javascript.html
- 例: JavaScript SDK の使用
  - http://docs.aws.amazon.com/ja_jp/cognito/latest/developerguide/using-amazon-cognito-user-identity-pools-javascript-examples.html
- プロキシ設定が必要な場合（いらんかった）
  - http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/node-configuring-proxies.html
- [JSON Web Token の効用](http://qiita.com/kaiinui/items/21ec7cc8a1130a1a103a)
