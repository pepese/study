JSONベースのREST APIを簡単に作成する**jsonapi-resources**についてまとめる。

[GitHub](https://github.com/cerebris/jsonapi-resources)  
[HP](http://jsonapi-resources.com/v0.8/guide/)

# インストール

Gemfileを以下のように編集し、```bundle install```。

```ruby
gem 'jsonapi-resources'
```

```bash
$ bundle install
```

# 実装

以下を実装する。

- Model
    - 普通のActiveRecordなどのモデル
- ResourceController
    - リクエストを受け付けるコントローラ
    - ```app/controllers```配下に作成する
    - 処理はResourceが行う
    - ```JSONAPI::ResourceController```を継承したクラスを作成するか、application_controller.rbなどに```JSONAPI::ActsAsResourceController```をincludeする
- Resource
    - ResourceControllerが受け付けたリクエストを実際に処理する
    - ```app/resources```配下に作成する
    - Modelに似ており、実際に作成したModelに従って処理するデータを定義する
    - ```JSONAPI::Resource```を継承したクラスを作成する
- routes.rb
    - ```jsonapi-resources```などのメソッドでResourceControllerのルーティング設定を行う


## Model

Modelはいつも通りつくる。  
ここでは、ユーザ```User```が複数のコメント```Comment```をするモデルを考える。

```bash
$ bundle exec rails generate model user name:string
$ bundle exec rails generate comment user_id:integer text:text
```

```ruby
class User < ApplicationRecord
  has_many :comments
end
```

```ruby
class Comment < ApplicationRecord
  belongs_to :user
end
```

## ResourceController

ResourceControllerの機能を持つコントローラを1つ作る場合は下記のように```JSONAPI::ResourceController```を継承する。

```ruby
class UserController < JSONAPI::ResourceController
end
```

か、もしくは上記のようなベースとなるコントローラを作成し、ベースコントローラを継承する。ResourceController共通の処理を作成したい場合などにいい。  
すべてのコントローラをResourceControllerにしたい場合は、application_controller.rbに```JSONAPI::ActsAsResourceController```をincludeする方法でもよい。

```ruby
class ApplicationController < ActionController::Base
  include JSONAPI::ActsAsResourceController
end
```

ここでは、バージョニングも考慮してコマンドを利用して以下のように作成する。

```bash
$ bundle exec rails generate jsonapi:controller rapi/v1/user
$ bundle exec rails generate jsonapi:controller rapi/v1/comment
```




## Resource

すべてのResourceに共通な処理をまとめて作成したい場合は下記のようなリソースクラスを作成して、他のResourceはこのクラスを継承する形式などにするとよい。

```ruby
class ApplicationResource < JSONAPI::Resource
end
```

以下に代表的なメソッドを記載する。

```ruby
class Rapi::V1::UserResource < JSONAPI::Resource
  attributes :firstname, :lastname, :address
  has_many :comments
  filters :id, :firstname, :lastname
end
```

```ruby
class Rapi::V1::CommentResource < JSONAPI::Resource
  attributes :text
  has_one :users
  filter :id
end
```

- attributes
    - データを操作するカラムを定義する
- has_many
    - Active Recordのhas_manyに同じ
- has_one
    - Active Recordのbelongs_toに同じ
- filter / filters
    - クエリパラメータで検索条件を指定するカラムを設定する

ここでは、バージョニングも考慮してコマンドを利用して以下のように作成する。

```bash
$ bundle exec rails generate jsonapi:resource rapi/v1/user
$ bundle exec rails generate jsonapi:resource rapi/v1/comment
```



## routes.rb
