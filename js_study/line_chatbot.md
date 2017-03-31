NodeJSとExpressでメッセージをオウム返しするLINEのチャットボットを作ってみた。  
Expressについては以下参照。

[http://blog.pepese.com/entry/2017/02/16/154804:embed:cite]

LINE部分は **LINE@** と **Messaging API** を使用した。

# 作成手順

```bash
$ express express-sample --view=pug --git
$ cd line-chatbot && npm install
$ npm install request crypto --save
$ mkdir config
$ touch config/config.json
```

Viewはなんでもいいが、デフォルトは嫌なのでとりあえす指定してみた。  
以下をそれぞれ後述の通り編集する。

- ```routes/index.js```
- ```config/config.json```

## ```routes/index.js```

<script src="http://gist-it.appspot.com/https://github.com/pepese/js-sample/blob/master/line-chatbot/routes/index.js?footer=0"></script>

## ```config/config.json```

<script src="http://gist-it.appspot.com/https://github.com/pepese/js-sample/blob/master/line-chatbot/config/config.json?footer=0"></script>

## 実行

```bash
$ npm start
```

```https``` でWebサーバか何かを立てて、 ```http://[FQDN]:3000/``` へプロキシさせる。  
その後、LINEアカウントの **Messaging API** へ上記のWebサーバのエンドポントを *Webhook URL* として登録する。

# ちょっと解説

LINEのMessage APIのAPI Referenceは[ここ](https://devdocs.line.me/ja/)。  
LINEに限らずWebhookに対応するアプリケーションを作成する際には、そのWebhookのサービスに合わせたインターフェースで開発する必要がある。  
ここでは以下について記載する。

- Webhook リクエスト
  - エンドユーザからメッセージを受信した際、LINE側のサーバから送信されるHTTPリクエスト
- Reply message リクエスト
  - Webhook リクエストを受け、メッセージを返却する場合にLINE側のサーバへ送信するHTTPリクエスト

## Webhook リクエスト

```javascript
{
  "events": [
      {
        "replyToken": "nHuyWiB7yP5Zw52FIkcQobQuGDXCTA",
        "type": "message",
        "timestamp": 1462629479859,
        "source": {
             "type": "user",
             "userId": "U206d25c2ea6bd87c17655609a1c37cb8"
         },
         "message": {
             "id": "325708",
             "type": "text",
             "text": "Hello, world"
          }
      }
  ]
}
```

- ```X-Line-Signature:{Signature}```
  - リクエストの送信元がLINEであることを確認するために署名検証するためのもの
  - ヘッダの値と、request body と Channel secret から計算した signature が同じものであることをリクエストごとに 必ず検証必要がある
    1. Channel secretを秘密鍵として、HMAC-SHA256アルゴリズムによりrequest bodyのダイジェスト値を得る。
    1. ダイジェスト値をBASE64エンコードした文字列が、request headerに付与されたsignatureと一致することを確認する。

## Reply message リクエスト

```javascript
{
    "replyToken":"nHuyWiB7yP5Zw52FIkcQobQuGDXCTA",
    "messages":[
        {
            "type":"text",
            "text":"Hello, user"
        },
        {
            "type":"text",
            "text":"May I help you?"
        }
    ]
}
```

```replyToken``` にWebhookで送られてきたボディの ```events[].replyToken``` を付与する必要がある。  
```messages``` の配列で複数のメッセージを返却することができる。  
また、ヘッダへ以下を付与する。

- ```Content-Type:application/json```
- ```Authorization: Bearer {ENTER_ACCESS_TOKEN}```
  - ```{ENTER_ACCESS_TOKEN}``` に Channel Access Token を付与する
