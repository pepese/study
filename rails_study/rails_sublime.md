Ruby on Rails開発で役立つSublime Text 3パッケージをまとめる。  
Sublime Text 3の導入については以下。

http://blog.asobicocoro.com/entry/2016/04/06/234256

# Ruby on Railsで役立つパッケージ

- SublimeCodeIntel
  - Tabによるコード補完補完
- CTags
  - メソッドの定義元にジャンプ
- SublimeLinter
- SublimeLinter-ruby
- RubyTest
  - 実装ファイルから ```Command + .``` でspecファイルを開ける
  - specファイルから ```Command + Shift + T``` でテストを実行できる  
- Ruby on Rails snippets
  - RubyとRailsのスニペット
- Rails Developer Snippets
  - ruby、rails、rspec、erbのスニペット

## CTagsの使い方

- ```brew install ctags```
- Sublime Text 3にPackage Control でCTagsをインストール
- Preferences -> Package Settings -> CTags -> Settings - Userを選択し、 ```CTags.sublime-settings``` ファイルを以下のように設定

```js:CTags.sublime-settings
{
  "command": "/usr/local/bin/ctags"
}
```

- ```Control + T``` -> ```Control + R``` で ```.tags``` ファイルを作成するディレクトリを選択する
- メソッドにカーソルを合わせ、```Control + T``` -> ```Control + T``` で定義候補一覧が表示される
- 一覧から定義元を選んでジャンプ

## SublimeLinterの使い方

- ```Command + Shift + p``` で ```SublimeLinter: Choose Gutter Theme``` と入力
- エラー表示に使われるマーク「Gutter Theme」の一覧が表示されるので好きなものを選ぶ
  - http://sublimelinter.readthedocs.io/en/latest/gutter_themes.html?highlight=choose%20gutter%20theme
- 新規ファイルを開きいて ```Command + Shift + p``` で ```Set Syntax: Ruby``` と入力
- ```arr = ['aaa', 'bbb']``` と記述し、行の左側に黄色い丸が表示されることを確認
- ```arr = ['aaa', 'bbb'``` とし、行の左側に赤い丸が表示されることを確認

# HTML

- Emmet
  - HTMLを簡易に記載できる（例えば、ul>li*3と記載し、Tabを押すとHTMLをつくってくれる）


# 参考

https://mattbrictson.com/sublime-text-3-recommendations  
http://uu4k.hatenablog.com/entry/2015/04/25/214332  
