TensorFlow触ってみる。

- 環境構築
- 使い方

# 環境構築

Macにanyenvが導入されていること前提。

## python

```sh
$ anyenv install pyenv
$ exec $SHELL -l
$ which pyenv
/Users/xxxx/.anyenv/envs/pyenv/bin/pyenv
$ pyenv install --list
$ pyenv install 3.6.1
$ pyenv global 3.6.1
$ pyenv version
3.6.1 (set by /Users/xxxx/.anyenv/envs/pyenv/version)
$ pyenv versions
  system
* 3.6.1 (set by /Users/xxxx/.anyenv/envs/pyenv/version)
$ python --version
Python 3.6.1
$ which pip
/Users/xxxx/.anyenv/envs/pyenv/shims/pip
$ which pip3
/Users/xxxx/.anyenv/envs/pyenv/shims/pip3
$ pip --version
pip 9.0.1 from /Users/xxxx/.anyenv/envs/pyenv/versions/3.6.1/lib/python3.6/site-packages (python 3.6)
$ pip3 --version
pip 9.0.1 from /Users/xxxx/.anyenv/envs/pyenv/versions/3.6.1/lib/python3.6/site-packages (python 3.6)
```

## TensorFlow

pip/pip3 のバージョンが 8.1 以上である必要がある。

```sh
$ pip3 install --upgrade tensorflow
```

もしエラーがでたら以下を実行。

```sh
$ pip3 install --upgrade tfBinaryURL
```

正しくインストールできているか確認。

```sh
$ python
>>> import tensorflow as tf
```

[参考](https://www.tensorflow.org/install/install_mac)


# 使い方

## 用語

- テンソル（Tensor）
  - 線形的な量または線形的な幾何概念を一般化したもの
  - 規定を選べば、多次元の配列として表現できるようなもの
  - テンソル自身は、特定の座標系によらないで定まる対象
  - **多次元配列** でおk
  - テンソルの種類
    - 0階テンソル・・・スカラ、ただの数値
    - 1階テンソル・・・ベクトル、配列
    - 2階テンソル・・・行列、二次元配列
    - 3階テンソル・・・行列が複数個あるもの、三次元配列
    - 4階テンソル・・・行列が複数個あるものが複数個あるもの、四次元配列
- セッション（Session）
  - ニューラルネットワークを実行する単位のこと
  - プログラム上ではセッションを作成して、そこに実行するノードを指定することになる

## 書き方

### import

```python
import tensorflow as tf
```

### 定数

```python
# const1 と命名した値 3 の定数ノード
node1 = tf.constant(3, name="const1")
```

### 変数

```python
# val1 と命名した初期値 0 の変数ノード
node2 = tf.Variable(0, name="val1")
```

変数には初期化が必要なので注意が必要。

### 足し算

```python
# node1 と node2 を足し算するノード
node3 = tf.add(node1, node2)
```

### 変数の初期化

```python
init = tf.global_variables_initializer()
```

### セッション（Session）の作成

```python
sess = tf.Session()
```

> ```sh
> 2017-06-20 16:32:03.465042: W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use SSE4.2 instructions, but these are available on your machine and could speed up CPU computations.
> 2017-06-20 16:32:03.465069: W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use AVX instructions, but these are available on your machine and could speed up CPU computations.
> 2017-06-20 16:32:03.465078: W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use AVX2 instructions, but these are available on your machine and could speedup CPU computations.
> 2017-06-20 16:32:03.465086: W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use FMA instructions, but these are available on your machine and could speed up CPU computations.
> ```
>
> TensorFlow は CPU の拡張命令を使用可能なのだが、これが有効になっていない場合、上記のような警告がでる。

### セッションで変数を初期化

```python
sess.run(init)
```

### セッションの実行

```python
print(sess.run([node1, node2]))
# 出力：[3, 0]
```

ノードは作成しても上記のようにセッション上で実行しなければ値は出力されない。

```python
print(sess.run(node3))
# 出力：3
```

上記は node1 と node2 の出力を足し算する node3 を実行した結果となる。  
ここまでで一旦実行はできる。

### assign

あるノードのアウトプットを別のノードへインプットする。

```python
assign = tf.assign(from_node, to_node)
```

### プレースホルダ

入力値を色々変更できるノード。

```python
input = tf.placeholder(tf.int32, name="input")
```

以下の `feed_dict` でセッション実行時に入力値を指定できる。

```python
sess.run(run_node, feed_dict={input:3})
```

# TFLearn

**TFLearn** はTensorFlowをScikit-learnライク2に使えるライブラリのことで、Githubにサンプルコードが幾つか掲載されている。  
[参考](http://qiita.com/kenta1984/items/4452e91db806ee765a78)

# TensorBoard

TensorFlow と一緒にインストールされている。

```sh
$ which tensorboard
/Users/xxxx/.anyenv/envs/pyenv/shims/tensorboard
```

## 使い方

コード中に `writer = tf.summary.FileWriter('cnn', sess.graph)` のような感じで表示対象を出力しておき、下記のコマンドで TensorBoard を起動する。

```sh
$ tensorboard --logdir=./cnn
```

- [TensorFlowとTensorBoardでニューラルネットワークを可視化](http://qiita.com/sergeant-wizard/items/fdf4d64a0d221a81da34)

# 参考

- [深層学習とTensorFlow入門](https://www.slideshare.net/tak9029/tensorflow-67483532)
  - 超いいこれ
- [TensorFlow API Doc](https://www.tensorflow.org/api_docs/python/)

## MNISTチュートリアル

- [TensorFlow 日本語 MNIST](http://www.tensorflow-partner.jp/mnist-beginner)
- [TensorFlow 公式 MNIST](https://www.tensorflow.org/get_started/mnist/beginners)
- [MNISTチュートリアルの日本語解説](http://qiita.com/sergeant-wizard/items/55256ac6d5d8d7c53a5a)
- [MNISTチュートリアルの日本語解説](http://qiita.com/haminiku/items/36982ae65a770565458d)
- [for Beginners と for Experts の間を埋めたい](http://qiita.com/TomokIshii/items/92a266b805d7eee02b1d)
- [確率的勾配降下法](https://ja.wikipedia.org/wiki/%E7%A2%BA%E7%8E%87%E7%9A%84%E5%8B%BE%E9%85%8D%E9%99%8D%E4%B8%8B%E6%B3%95)
  - 学習時にトレーニングセットを **ミニバッチ** に分割してそれぞれのミニバッチでパラメータが収束するまで学習を繰り返す方法
