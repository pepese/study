# LINEのビジネスアカウントの種類

以下の３種類がある。

- LINE公式アカウント
- ビジネスコネクトアカウント
- LINE@

## LINE公式アカウント

企業、ブランド、アーティストなどが開設でき、同一メッセージを友だち登録したユーザへ一斉に送信するなど、広範囲・単一方向のコミュニケーションに特化したサービス。  
LINEビジネスコネクトを利用する場合は、API型公式アカウントと呼ばれる。  
（LINE公式アカウント＋LINEビジネスコネクト＝API型アカウント）

## ビジネスコネクトアカウント

ビジネスコネクトのためのアカウントです。取得するためには審査がある。  
LINEビジネスコネクトは、APIの利用と、外部データとの接続によって、1to1や双方向のコミュニケーションを可能にする。  
APIはLINE側のメッセージングサーバによって公開されている。  
LINEパートナー企業などからサポートが受けれる分、LINE@アカウントでMessaging APIなどを使用して開発するのと違う。

## LINE@

店舗や施設、個人などが利用できるアカウントで、友だち登録したユーザーへの一斉配信、ユーザーとの双方向のやり取りも可能。  
LINEビジネスコネクトには対応していない。




# [LINE BUSINESS CENTER](https://business.line.me/ja/)

LINE Business Centerは、LINEが提供するサービス（LINE@⋅LINE Login⋅Messaging APIなど）のアカウントを一括で管理したり、各種サービスの申込みをしたりできるツール。  
LINEアカウント情報（メールアドレス⋅パスワード）で登録・ログインが可能。

## [LINE@](https://business.line.me/ja/services/lineat)

企業やお店の情報発信に役立つビジネス向けLINEアカウント。

- メッセージ一斉配信
- 1:1トーク
- タイムライン機能

開発者向けにMessaging APIも使用できる。
LINEビジネスコネクトの利用はできない。

## [LINE Login](https://business.line.me/ja/services/login)

あなたのサービスをLINEユーザと繋げるログイン機能。
スマホネイティブアプリやWebサイトにLINEアカウントでのログイン機能を提供する。
要はGoogleやFaceBookのソーシャルログイン（OpenIDやOAuth）のLINE版。

## [Messaging API](https://business.line.me/ja/services/bot)

あなたのサービスとLINEユーザの双方向コミュニケーションを可能にする。
1:1、グループでトーク可能。
通常のメッセージだけでなく、画像や選択ボタン付のメッセージなどを送信可能。

- Push API
    - Botから任意のタイミングでユーザーに対してメッセージ送信を行うAPI
- Reply API
    - ユーザーからのメッセージに対して応答メッセージを送信するAPI

その他も含めたAPIリファレンスは[ここ](https://devdocs.line.me/ja/#messaging-api)。
