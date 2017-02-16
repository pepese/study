Ruby on Railsのコントローラについてまとめる。

Ruby on Railsの概要については以下を参照。

⭐️ここにrails_overview.md⭐️

ここではControllerを中心に記載する。

# Controller作成コマンド

```
rails generate controller [NAME] [action action] [options]
```

- NAME
    - コントローラ名
    - 「**[NameSpace]/**コントローラ名」と**スラッシュ**で区切ると**名前空間**を設定できる。
        - 名前空間を設定するとディレクトリが分割されたり、名前空間名のプレフィックスがついたりする
    - 「HogeFuge」のように**大文字区切り**にすると**アンダースコア区切り**の命名でController、Viewなどが作成される
        - 「HogeFuge」->「hoge_fuge_controller.rb」
- action
    - アクション名を指定
    - 作成されるコントローラクラスのメソッド（アクションメソッド）となる

```rails generate controller```コマンドのオプションは以下の通り。

- 「--skip-namespace」／「--no-skip-namespace」
    - 名前空間設定がスキップされる（デフォルト：スキップしない）
- 「-e [NAME]」「--template-engine=[NAME]」
    - 使用するテンプレートを指定（デフォルト：erb）
- 「-t [NAME]」「--test-framework=[NAME]」
    - 使用するテスティングフレームワークを指定（デフォルト：test_unit）
- 「--helper」
    - ヘルパーを作成するか（デフォルト：true）
- 「--assets」
    - アセットを作成するか（デフォルト：true）

# 規則・ルール

- 名前は複数形
- ApplicationControllerを継承
- publicメソッドはアクション
    - アクションはリクエストを受けつけるメソッド
    - アクションでないメソッドはprivateにする

```ruby
class ClientsController < ApplicationController
  def new
  end
end
```

newメソッドには何も定義しなくてもデフォルトでnew.html.erbを返却する。

# ルーティング

ルーティングの設定は以下を参照。

⭐️ここにroutes.md⭐️

# フィルタ

フィルタは、アクションの直前 (before)、直後 (after)、あるいはその両方 (around) に実行されるメソッド。

- before_action
- after_action
- around_action

以下はトランザクション境界を実現するaround_actionメソッドを使用したフィルタ。

```ruby
class ChangesController < ApplicationController
  around_action :wrap_in_transaction, only: :show

  private

  def wrap_in_transaction
    ActiveRecord::Base.transaction do
      begin
        yield
      ensure
        raise ActiveRecord::Rollback
      end
    end
  end
end
```

# リクエスト・レスポンスの情報

リクエスト・レスポンスの情報を保持するオブジェクトは以下の通り。

- request
    - リクエストの情報を保持する
- params
    - リクエストのURLパラメータやフォームの情報を保持する
- response
    - レスポンスの情報を保持する

# レスポンスの変更

- render
- respond_to

# 例外ハンドリング

デフォルトでは以下の挙動をする。

- URLが存在しない場合はpublicの404.htmlを返却
- 例外が発生した場合はpublicの500.htmlを返却

## rescue_from

特定の例外を指定し、ハンドリング時の命令を記載できる。

# ApplicationController

全てのコントローラの基底クラス。  
原則としてApplicationControllerを直接呼び出すアクションは定義できない。  
以下の用途で利用する。

- 個別のコントローラから呼び出せるヘルパーメソッド
- （ほとんど）全てのコントローラ共通のフィルタ
- 共通的な例外処理
- アプリケーション共通設定

# API
JSONの返却方法

```ruby
class Api::ProductsController < ApplicationController
  def index
    @products = Product.all
    respond_to do |format|
      format.html # => 通常のURLの場合、index.html.erb が返される
      format.json { render json: @products } # URLが.jsonの場合、@products.to_json が返される
    end
  end
end
```

返却するJSONのフォーマットを指定するにはViewでjbuilderを作成する。

## JSONのみを返却するAPIの作成

URLに「.json」をつけなくてもJSONを返却する。  
```namespace :api, { format: 'json' }```を使用して、以下のようにルーティングを設定する。

```ruby
# config/routes.rb
Rails.application.routes.draw do
  namespace :api, { format: 'json' } do
    resources :products
  end

  resources :products
  root to: 'products#index'
end



# ルーティング結果
$ rake routes
          Prefix Verb   URI Pattern                      Controller#Action
    api_products GET    /api/products(.:format)          api/products#index {:format=>"json"}
                 POST   /api/products(.:format)          api/products#create {:format=>"json"}
 new_api_product GET    /api/products/new(.:format)      api/products#new {:format=>"json"}
edit_api_product GET    /api/products/:id/edit(.:format) api/products#edit {:format=>"json"}
     api_product GET    /api/products/:id(.:format)      api/products#show {:format=>"json"}
                 PATCH  /api/products/:id(.:format)      api/products#update {:format=>"json"}
                 PUT    /api/products/:id(.:format)      api/products#update {:format=>"json"}
                 DELETE /api/products/:id(.:format)      api/products#destroy {:format=>"json"}
```

また、コントローラは以下のようにApiモジュール内にコントローラを定義する。

```ruby
# app/controllers/api/products_controller.rb
module Api
  class ProductsController < ApplicationController

    def index
      @products = Product.all
      render json: @products
    end
  end

  ...
end
```

# バージョニング

```namespace :v1```や```namespace :v2```を追加することでバージョニングを行える。

```ruby
# config/routes.rb
Rails.application.routes.draw do
  namespace :api, { format: 'json' } do
    namespace :v1 do
      resources :products
    end
    namespace :v2 do
      resources :products
      # v2のリソース宣言 ...
    end
  end

  resouces :products
  root to: 'products#index'
end



# ルーティング結果
$ rake routes
             Prefix Verb   URI Pattern                         Controller#Action
    api_v1_products GET    /api/v1/products(.:format)          api/v1/products#index {:format=>"json"}
    ....
    api_v2_products GET    /api/v2/products(.:format)          api/v2/products#index {:format=>"json"}
    ....
```

コントローラもroutesのnamespaceに合わせて```module V1```を追加する。

```ruby
# app/controllers/api/v1/products_controller.rb
module Api
  module V1
    class ProductsController < ApplicationController

      def index
        @products = Product.all
        render json: @products
      end
    end

    ...
  end
end
```

# 参考
http://railsguides.jp/action_controller_overview.html

http://edgeguides.rubyonrails.org/action_controller_overview.html
