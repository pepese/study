Ruby on Railsのビューについてまとめる。

Ruby on Railsの概要については以下を参照。

⭐️ここにrails_overview.md⭐️

ここではViewを中心に記載する。

# レイアウトテンプレート

## views/layouts/application.html.erb

レイアウトテンプレート内の ```<%= yield %>``` の部分にコントローラ個別のViewが適用される。  
```application.html.erb``` は基本的には全てのコントローラに適用される。  
レイアウトテンプレートの変更や無効化については後述する。

## コントローラ単位でレイアウトを変更

特に指定しなければ「**views/layouts/application.html.erb**」ファイルがアプリケーションに含まれる全てのテンプレートとなるが、特定のコントローラに含まれるアクションから呼び出されるテンプレートには指定したレイアウトを設定したい場合は「**views/layouts/コントローラ名.html.erb**」というファイルを作成する。

## レイアウトを無効にする

レイアウトテンプレートを無効にするには、コントローラに設定を追加する。  
**layout**メソッドを利用した以下の２つの方法がある。

- 全てのアクションのテンプレートを無効にする

```ruby
class UsersController < ApplicationController
  layout false

  def show
  end
end
```

- 特定のアクションのテンプレートを無効にする

```ruby
class UsersController < ApplicationController
  def show
    render :layout => false
  end
end
```

# メソッド

- link_to
    - コントローラへのリンク
- render
    - 部分テンプレートの適用

その他のメソッドは[ここ](http://railsdoc.com/view)。
