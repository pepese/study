[公式](http://rails-erd.rubyforge.org)  

ER図を作成できるGem。

# インストール

```bash
$ brew install graphviz
```

```ruby
group :development do
  gem "rails-erd"
end
```

```bash
$ bundle install
```

# ER図作成

```bash
$ bundle exec rake erd
```

# カスタマイズ

カスタマイズしたいときは[ここ](http://rails-erd.rubyforge.org/customise.html)参照。
