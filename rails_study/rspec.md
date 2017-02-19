Ruby on RailsのデフォルトのテスティングフレームワークはMiniTestであるが、デファクトは**RSpec**。
RSpecはBDD（Behavior Driven Development）のアプローチ。
BDDではオブジェクトやメソッドの振る舞いを記述してテストを行う。

# RSpecのインストール

Gemfileに以下を追記する。

```ruby
group :test, :development do
  gem 'rspec-rails', '~> 3.0.0'
end
```

上記でdevelopment、test環境へRSpecが導入される。  
以下のコマンドを実行する。

```bash
$ bundle install
$ bundle exec rails g rspec:install
```

以下のファイルが作成される。

- .rspec
- spec/spec_helper.rb
- spec/rails_helper.rb

RSpecではtestディレクトリは使用しない。  
ここまで作成しておけば、```rails generate```コマンド実行時にRSpecのテストスクリプトも作成されるようになる。

# Modelのテスト



http://www.atmarkit.co.jp/ait/articles/1409/30/news037.html
https://relishapp.com/rspec
http://betterspecs.org/jp/
