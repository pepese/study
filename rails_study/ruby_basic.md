# 標準出力

- print
  - 標準出力
- puts
  - 文末改行付き標準出力
- p
  - オブジェクトの型も分かる出力

# 変数

- 型宣言なし
- 変数
  - 小文字もしくは_から開始
- 定数
  - 大文字から開始、慣習的に全部大文字
  - 再代入してもエラーにならないが、警告が出る

## オブジェクトの調べ方

Rubyでは全ての値はオブジェクトとなっており、 ```.class``` や ```.methods``` でオブジェクトのクラスやメソッドを調べることができる。

```ruby
p obj.class
p obj.methods
```

# 文字列

ダブルクオートかシングルクオートでくくる。違いは以下。

- ダブルクオート
  - 特殊文字（改行とかタブ）の使用や式展開（ ```#{式}``` ）が行われる
- シングルクオート
  - 特殊文字が式もそのまま表示される

# ```!``` や ```?``` がついたメソッド

- ```!``` がついたメソッド
  - 例） ```upcase!```
  - オブジェクトの値自体もメソッドの処理に応じて変更してしまう
  - **破壊的メソッド**
- ```?``` がついたメソッド
　- 例） ```empty?```
  - 真偽値（ ```true``` or ```false``` ）を返すメソッド

# ハッシュ

- key - value ペアの集合

```ruby
hoge = {"key1" => "value1", "key2" => "value2"}
hoge = {:key1 => "value1", :key2 => "value2"} // シンボルオブジェクト使用
hoge = {key1: "value1", key2: "value2"} // シンボルオブジェクト略記
```

# %記法

以下は全て同じ意味になる。

```ruby
"hello"
%Q(hello)
%(hello)
```

ダブルクオートの代わりに ```%Q``` を使用している。  
文字列ないにダブルクオートを入れたい際などにエスケープの必要性が無いので可読性が上がる。  
その他にも%記法には色々ある。

```ruby
["red", "blue"]
%W(red blue)

['red', 'blue']
%w(red blue)
```

# lambda

```ruby
->(a,b){ p [a,b] }
```
上記は下に同じ。
```ruby
lambda{|a, b| p [a, b] }
```
