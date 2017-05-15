
# Amazon Cognito 側の準備

- User Pool の作成
  - User Pool ID が取得できる
- App Clients の作成
  - App Client ID が取得できる

# クライアント側（SPA）の準備

- [amazon-cognito-identity-js](https://github.com/aws/amazon-cognito-identity-js/)
  - typescript typings も含まれている模様

```sh
$ npm install --save-dev webpack json-loader
$ npm install --save amazon-cognito-identity-js
```

- [AWS SDK for JavaScript / Usage with TypeScript](https://www.npmjs.com/package/aws-sdk#usage-with-typescript)
- [AWS SDK for JavaScriptを使ってブラウザーからCognito User Poolsへサインアップしてみた](http://dev.classmethod.jp/cloud/aws/singup-to-cognito-userpools-using-javascript/)
- [Serverless + Webpack + TypeScript + tslint メモ](http://qiita.com/y13i/items/3ab22f8f8f1c41c5402f)

# 参考

- [CognitoUserPoolsを使うAngular2 SPAのサンプルを動かす](http://dev.classmethod.jp/cloud/aws/aws-cognito-angular2-quickstart/)
- [JSON Web Token の効用](http://qiita.com/kaiinui/items/21ec7cc8a1130a1a103a)
- [Amazon Cognito 認証のフロー](http://docs.aws.amazon.com/ja_jp/cognito/latest/developerguide/amazon-cognito-user-pools-authentication-flow.html)
- [Amazon Cognito トークンを使用する](http://docs.aws.amazon.com/ja_jp/cognito/latest/developerguide/amazon-cognito-user-pools-using-tokens-with-identity-providers.html)


# やったことメモ

```sh
$ npm upgrade -g @angular/cli
$ ndenv rehash
$ ng new cognito-js
$ cd cognito-js
$ npm install amazon-cognito-identity-js --save # 依存から aws-sdk も導入される
```

aws-sdkには@type/aws-sdkが含まれている模様。（[参考](https://www.npmjs.com/package/@types/aws-sdk)）  
src/tsconfig.app.jsonを以下のように編集。

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

src/app/app.component.ts を以下のように修正

```typescript
import { Component } from '@angular/core';
import * as AWS from "aws-sdk";
import { CognitoUserPool, CognitoUserAttribute, CognitoUser, AuthenticationDetails } from 'amazon-cognito-identity-js';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app works!';

  constructor(){
    AWS.config.region = 'ap-northeast-1';

    const data = { UserPoolId: 'ap-northeast-1_xxxxxxxx', ClientId: 'xxxxxxxx'};
    const userPool = new CognitoUserPool(data);
    console.log(userPool);
    //const cognitoUser = userPool.getCurrentUser();
    //console.log(cognitoUser);

    const userData = {
      Username : 'xxxx', // your username here
      Pool : userPool
    };


    // サインアップ（ユーザ作成）
    //let attributeList = [];
    //const dataEmail = {
    //  Name : 'email',
    //  Value : 'xxxxxxxx'
    //};
    //const dataPhoneNumber = {
    //  Name : 'phone_number',
    //  Value : 'xxxxxxxx'
    //};
    //let attributeEmail = new CognitoUserAttribute(dataEmail);
    //let attributePhoneNumber = new CognitoUserAttribute(dataPhoneNumber);
    //attributeList.push(attributeEmail);
    //attributeList.push(attributePhoneNumber);
    //let cognitoUser;
    //userPool.signUp('user name', 'password', attributeList, null, function(err, result){
    //  if (err) {
    //    console.log(err);
    //    return;
    //  }
    //  cognitoUser = result.user;
    //  console.log('user name is ' + cognitoUser.getUsername());
    //});

    // この時点でSMSでユーザに verification code が送られる

    // 送られてきた verification codeを確認する
    //const cognitoUser = new CognitoUser(userData);
    //cognitoUser.confirmRegistration('xxxxxx', true, function(err, result) {
    //  if (err) {
    //    alert(err);
    //    return;
    //  }
    //  console.log('call result: ' + result);
    //});


    // サインイン（ログイン）
    const cognitoUser = new CognitoUser(userData);
    const authenticationData = {
        Username : 'xxxx', // your username here
        Password : 'xxxx', // your password here
    };
    const authenticationDetails = new AuthenticationDetails(authenticationData);
    cognitoUser.authenticateUser(authenticationDetails, {
      onSuccess: function (result) {
        console.log('access token + ' + result.getAccessToken().getJwtToken());
      },
      onFailure: function(err) {
        alert(err);
      }//,
      //mfaRequired: function(codeDeliveryDetails) {
      //    const verificationCode = prompt('Please input verification code' ,'');
      //    cognitoUser.sendMFACode(verificationCode, this);
      //}
    });
  }
}
```

`$ ng serve` するとエラー無く起動するところまで確認。

- チュートリアル: JavaScript アプリのユーザープールを統合する
  - http://docs.aws.amazon.com/ja_jp/cognito/latest/developerguide/tutorial-integrating-user-pools-javascript.html
- 例: JavaScript SDK の使用
  - http://docs.aws.amazon.com/ja_jp/cognito/latest/developerguide/using-amazon-cognito-user-identity-pools-javascript-examples.html
- プロキシ設定が必要な場合（いらんかった）
  - http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/node-configuring-proxies.html
