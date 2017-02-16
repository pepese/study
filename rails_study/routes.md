Ruby on Railsのルーティングについてまとめる。  

コントローラ・アクションへのリクエストのルーティングは「**config/routes.rb**」で設定する。

```ruby
Rails.application.routes.draw do
# ここに設定を記載
end
```

## resourcesメソッド

```ruby
Rails.application.routes.draw do
  resources :users
end
```

例えば、上記の設定で以下のルーティングが設定される。  
※「:users」はシンボル。「:users, products, ...」のように複数も設定できる。

|URL|アクション|HTTPメソッド|挙動|
|:---|:---|:---|:---|
|/users         |index  |GET      |ユーザ一覧画面      |
|/users/:id     |show   |GET      |単一ユーザ画面      |
|/users/new     |new    |GET      |新規ユーザ登録画面   |
|/users         |create |POST     |新規ユーザ登録処理   |
|/users/:id/edit|edit   |GET      |既存単一ユーザ編集画面|
|/users/:id/edit|update |PATCH/PUT|既存単一ユーザ更新処理|
|/users/:id     |destroy|DELETE   |既存単一ユーザ削除処理|

基本、GETは画面を返却し、その他のHTTPメソッドは処理を行う。  
また、resourcesはビューヘルパ等で利用できるURLヘルパも作成する。

|ヘルパ名（_path）|ヘルパ名（_url）|戻り値|
|:---|:---|:---|
|users_path        |users_url        |/users         |
|user_path(id)     |user_url(id)     |/users/:id     |
|new_user_path     |new_user_url     |/users/new     |
|edit_user_path(id)|edit_user_url(id)|/users/:id/edit|

以下のオプションがある。

|オプション|説明|
|:-------|:---|
| :as	           |ルート名に利用する別名|
| :controller    |コントローラを指定|
| :path_names    |指定したアクションのみ名前の変更|
| :path          |URLを書き換える|
| :only          |生成されるURLを絞り込む|
| :except        |指定したURLは生成しない|
| :shallow       |resource(s)のネストを作成した際、浅いルートを作成|
| :shallow_path  |浅いネストでURLを書き換える|
| :shallow_prefix|浅いネストでプレックスを指定|
| :format	       |フォーマット・拡張子を指定|

## resourceメソッド

複数形であることに注意。resourcesメソッドの一覧系を作成しないバージョン。

### resources/resourceメソッドのネスト

以下のようにネスト可能。

```ruby
Rails.application.routes.draw do
  resources :users do
    resources :products
  end
end
```

## matchメソッド

ルーティングを指定する。

```
match URLパターン [, オプション]
```

```ruby
Rails.application.routes.draw do
  match 'users/new' # -> 「/users/new」のアクセスを「users#new」へ
  match 'users', controller: 'users', action: 'new' # -> 「/users」のアクセスを「users#new」へ
  match 'users', to: 'users#new', via: :get, as: 'siginup'
end
```

オプションは以下の通り。

|オプション|説明|
|:---|:---|
| :controller, :action|コントローラとアクションを指定、必ずセットで指定|
| :to                 |	:controller, :actionの短縮形|
| :via                |HTTPメソッドを指定|
| :as	                |ルート名を指定|

## rootメソッド

ルート（/）へのアクセスを設定する。

```
root [オプション]
```

```ruby
Rails.application.routes.draw do
  root to: 'pages#index' # -> 「/」のアクセスを「pages#index」へ
end
```

## ネームスペースの変更（scope、namespaceメソッド）

URLにネームスペースを設定できる。

```
scope モジュール名 [, オプション] do
  ルート定義
end
```

```
namespace モジュール名 [, オプション] do
  ルート定義
end
```

namespaceは、URLもcontroller格納フォルダも、指定のパスになる。


```ruby
namespace :admin do
  resources :users
end
```

```

# rake routes
        Prefix Verb   URI Pattern                     Controller#Action
   admin_users GET    /admin/users(.:format)          admin/users#index
               POST   /admin/users(.:format)          admin/users#create
new_admin_user GET    /admin/users/new(.:format)      admin/users#new
dit_admin_user GET    /admin/users/:id/edit(.:format) admin/users#edit
    admin_user GET    /admin/users/:id(.:format)      admin/users#show
               PATCH  /admin/users/:id(.:format)      admin/users#update
               PUT    /admin/users/:id(.:format)      admin/users#update
               DELETE /admin/users/:id(.:format)      admin/users#destroy
```

scopeメソッドとmoduleオプションは、controllerの格納フォルダだけ、指定パスになる。

```ruby
scope module: :admin do
  resources :users
end
```

```
# rake routes
   Prefix Verb   URI Pattern               Controller#Action
    users GET    /users(.:format)          admin/users#index
          POST   /users(.:format)          admin/users#create
 new_user GET    /users/new(.:format)      admin/users#new
edit_user GET    /users/:id/edit(.:format) admin/users#edit
     user GET    /users/:id(.:format)      admin/users#show
          PATCH  /users/:id(.:format)      admin/users#update
          PUT    /users/:id(.:format)      admin/users#update
          DELETE /users/:id(.:format)      admin/users#destroy
```

scopeメソッドのみの場合は、URLだけ、指定のパスになる。

```ruby
scope '/admin' do
  resources :users
end
```

```
# rake routes
   Prefix Verb   URI Pattern                     Controller#Action
    users GET    /admin/users(.:format)          users#index
          POST   /admin/users(.:format)          users#create
 new_user GET    /admin/users/new(.:format)      users#new
edit_user GET    /admin/users/:id/edit(.:format) users#edit
     user GET    /admin/users/:id(.:format)      users#show
          PATCH  /admin/users/:id(.:format)      users#update
          PUT    /admin/users/:id(.:format)      users#update
          DELETE /admin/users/:id(.:format)      users#destroy
```

オプションは以下の通り。

|オプション|説明|
|:---|:---|
| :path        |ルートのパスを指定|
| :module      |namespaceを指定|
| :as	         |ルート名を指定|
| :shallow_path|浅いネストでURLを書き換える|

## get、post、patch、put、deleteメソッド

特定のHTTPメソッドでルーティングを設定する。

```
get URLパターン [, オプション]
```

```ruby
Rails.application.routes.draw do
  get 'users/new', to: 'users#new2'
end
```

上記はGETの例だが、post、patch、put、deleteも同様に記載する。


_oksky-chat_session=ZjMwekE4aHJmWlhTRHd6dmxyclZrNkZyUkVFdVFZOFVuWU5aMEo4WjVrKzlSc3loUlI1dDRDa09pRko0dGdXTldPa0RyaTJCM2oraHptd1lVWmZNUkMvNUlhQnJsR1Rpc28zTUFMRDFnYkxBUnp1Nk9uZ0lpR2RhQTR0UmJkazNad3Q3MkNabTk3bHJWanR3N1VRbkhnVjlpUjNERGRzbzVLdmE0QUF5Z05iTW13Vm5YSVlyUTZoMXE3OHR5Ym4zbEVKcFRwV1gvdkxNbk8vL2hzYkdIWmhVZ2NMeGtEMVhwYjhHNEJvV0xpVTZWS0xaditYNHRCbHJsR1FpcXNkRmRXUmdwdnBOUlVCUGtmaklJbTZFUWZQb1hMamtLVUVCaFk2NXYrNmlkZWluclY1MDJTeEZWUnZDbVlWaXVvb1YwdjNuM1Vpc0h3czV6cmNNcmdVSzBBM01yKy9kYWlFQUhnSXFIeUlieHdFPS0tTHRKZmJ3NE5zUThxeU9hUWRtbWpGUT09--cab3ff44102d2b11b0583013fba56d89886f1eba; Domain=localhost; Path=/; Expires=2016/12/30 15:42:06;	581 B	✓	
