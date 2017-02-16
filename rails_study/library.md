# devise

deviseはRailsでユーザのログイン認証管理を行うメジャーなgem。  

[公式](https://github.com/plataformatec/devise)
参考：http://qiita.com/Salinger/items/873e3c667462746ae707

以下のモジュールで構成される。

- Database Authenticatable
    - パスワードをハッシュ化してDBへ保存し、ユーザがサインインしている間その信憑性を検証する
    - 認証はPOSTリクエストとBasic認証で行われる
- Onmiauthable
- Confirmable
- Recoverable
- Registerable
- Rememberable
- Trackable
- Timeoutable
- Validatable
- Lockable

# searchkick

ElasticSearchアクセスライブラリ

# kaminari

ページングライブラリ。

http://www.techscore.com/blog/2013/01/07/railsライブラリ紹介-ページングを行う「kaminari」/

# Nokogiri

 Webスクレイピング

# awesome_nested_set

モデルを階層構造に管理できるようにするgem。

http://ruby-rails.hatenadiary.com/entry/20150216/1424092796

# cancancan

ユーザ権限によるアクセス管理
※昔cancanってのがあったが、開発止まってるのでその代わりくさい

# annotate

Modelファイルに以下のようなコメントをつけてくれる

```ruby
# == Schema Information
#
# Table name: products
#
#  id                 :integer          not null, primary key
#  title              :string(255)   not null
#  price            :integer
#  created_at   :datetime        not null
#  updated_at  :datetime        not null
```
