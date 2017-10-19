Java屋のためのC++入門。  
と言っても完全に自分基準のメモ。

# 基本文法

## Hello, World !

sample.cc
```cpp
#include <iostream>

int main() {
  std::cout << "Hello, World !" << std::endl;
}
```

- 標準ライブラリのインクルード
    - `#include <iostream>`
- cout
    - 標準ライブラリ
    - `printf` と同じ機能であるが、関数ではなく **オブジェクト**
    - console out の略、たぶん
- endl
    - 標準ライブラリ
    - end line の略、たぶん
- C++ の標準ライブラリの名前空間を **標準名前空間** といい **std** と表記
- 名前空間の利用方法
    - `(名前空間名)::(変数名・クラス名など）`
- コンパイル
    - `$ g++ sample.cc`
- 実行
    - `$ ./a.out`

## 名前空間

sample.cc
```cpp
#include <iostream>
using namespace std;
int main() {
  cout << "Hello, World !" << endl;
  string in;
  cin >> in;
  cout << "input is " << in << endl;
}
```

- 名前空間の利用方法
    - `using namespace (名前空間名);`
    - `using namespace std;` と宣言すれば、標準ライブラリの `std::` を省略できる
- cin
    - 標準ライブラリ
    - `scanf` と同じ機能であるが、これも関数ではなくオブジェクト
    - console input の略、たぶん
    - コンソールからの入力待ちになる

## クラス

sample_class.h
```cpp
#ifndef _SAMPLE_CLASS_H_
#define _SAMPLE_CLASS_H_

class Sample {
public:
    Sample();  // コンストラクタ
    ~Sample(); // デストラクタ
    void setNum(int num);
    int  getNum();
    void countUp();
private:
    int num;
};

#endif //_SAMPLE_CLASS_H_
```

sample_class.cc
```cpp
#include "sample_class.h"
#include <iostream>

Sample::Sample() : num(0) {
    std::cout << "インスタンス生成" << std::endl;
}

Sample::~Sample() {
    std::cout << "インスタンス破棄" << std::endl;
}

void Sample::setNum(int num) {
    this->num = num;
}

int Sample::getNum() {
    return num;
}

void Sample::countUp() {
    num++;
}
```

sample.cc
```cpp
#include <iostream>
#include "sample_class.h"

int main() {
    Sample obj;
    obj.setNum(1);
    std::cout << obj.getNum() << std::endl;
    obj.countUp();
    std::cout << obj.getNum() << std::endl;
}
```

- コンストラクタ
    - クラス名と同定義
    - `:` より右はメンバ変数の初期化（無くてもいい）、カンマ区切りで複数
- デストラクタ
    - クラス名の最初に `~` をつけるとデストラクタ
- 擬似命令
    - `#ifndef` 、 `#define` 、 `#endif`
    - プリプロセッサによって処理される命令
    - 複数のファイルから1つのヘッダファイルをインクルードされた際の重複定義を回避する
- 自作ライブラリのインクルード
    - `#include "sample_class.h"`
- メンバへのアクセス
    - **ドット演算子** （ **.** ） を使う
    - インスタンスへのポインタを経由するときは **アロー演算子** （ **->** ）を使う
- コンパイル
    - `$ g++ sample_class.cc sample.cc`
    - ヘッダファイルは入れない

### 明示的なインスタンスの生成と破棄

sample.cc
```cpp
#include <iostream>
#include "sample_class.h"

int main() {
    Sample* obj = new Sample();
    obj->setNum(1);
    std::cout << obj->getNum() << std::endl;
    obj->countUp();
    std::cout << obj->getNum() << std::endl;
    delete obj;
}
```

- **new** 、 **delete** 演算子を使用する

## テンプレート

Javaでいうジェネリクス。

### テンプレート関数

template_sample.cc
```cpp
#include <iostream>
#include <string>

// テンプレート関数
template <typename T>
T add(T x, T y){
    return x + y;
}

int main(){
    std::cout << add<int>(1, 2) << std::endl;
    std::cout << add<std::string>("A", "B") << std::endl;
    return 0;
}
```

- テンプレートの定義
    - `template <typename T>`
- 複数のテンプレートの定義
    - `template<typename T, typename S>`

### テンプレートクラス

template_sample_class.h
```cpp
#ifndef _TEMPLATE_SAMPLE_CLASS_H_
#define _TEMPLATE_SAMPLE_CLASS_H_

template <typename T>
class TemplateSample {
private:
    T x;
    T y;
public:
    void set(T x, T y);
    T add();
};

#endif // _TEMPLATE_SAMPLE_CLASS_H_
```

template_sample_class.cc
```cpp
#include "template_sample_class.h"

#include <string>

template <typename T>
void TemplateSample<T>::set(T x, T y) {
    this->x = x;
    this->y = y;
}

template <typename T>
T TemplateSample<T>::add() {
    return this->x + this->y;
}

// 明示的テンプレートのインスタンス化
// 使用できる型を指定する
template class TemplateSample<int>;
template class TemplateSample<std::string>;
```

template_sample.cc
```cpp
#include <iostream>
#include <string>

#include "template_sample_class.h"

int main(){
    TemplateSample<int> int_obj;
    int_obj.set(1, 2);
    std::cout << int_obj.add() << std::endl;

    TemplateSample<std::string>* obj_string = new TemplateSample<std::string>();
    obj_string->set("A", "B");
    std::cout << obj_string->add() << std::endl;
    delete obj_string;

    return 0;
}
```


# ライブラリ

## ライブラリとリンク

- 静的ライブラリ（static library）
    - コンパイル時にリンクする
    - `ar` コマンドで `.o` をまとめて作った `.a` ファイル
        - Windowsの場合 `.lib` ファイル
    - `libhoge.a` （ `hoge` というライブラリ）がある時、 `gcc` に `-lhoge` オプションを与えるとリンクされる
    - `.a` ファイルの中身は `.o` ファイルの連結のようなものであり，連結時に与えた順番通りに読み込まれる
- 共有ライブラリ（shared library）
    - プログラム実行時にリンクする
    - `gcc` に `-shared` オプションを与えて得られる `.so` ファイル
        - Windowsの場合 `.dll` ファイル
    - `nm` コマンドで `.so` に関数が登録できたか確認できる
    - `libhoge.so` （ `hoge` というライブラリ）がある時，`gcc` に `-lhoge` オプションを与えるとリンクされる
- 共有ライブラリの動的リンク
    - 共有ライブラリは通常は動的リンクされる
    - `.so` ファイルの内容は実行ファイルに含まれず、 `.so` ファイルが必要であるということが記録される
    - 実行時に，動的リンカローダがメモリマップを弄って同じプロセスから使えるようにする
    - `ldd` コマンドで動的リンクされ依存している `.so` ファイルが見れる
- 共有ライブラリの静的リンク
    - 共有ライブラリも静的リンクできる
    - `gcc` に `-static` オプションを与えると共有ライブラリが静的リンクされる
        - この場合、 `.so` ファイルに依存しない
        - 他にも 例えば `-static-libstdc++` と与えると `libstdc++.so` への依存が消える
    - 吐かれるバイナリは大きくなる

`$ gcc access.c -L/usr/local/pgsql/lib  -I/usr/local/pgsql/include -lpq`
    - ライブラリへのパス指定（ `-L` ）
    - ヘッダファイルへのパス指定（ `-I` ）
    - 使用するライブラリ名の指定（ `-l` ）
        - 上記では `pq` というライブラリを指定している

## 外部ライブラリ Boost を使って見る

C++の準標準と呼ばれているライブラリ。

```sh
$ brew update
$ brew install boost
==> Downloading https://homebrew.bintray.com/bottles/boost-1.65.1.sierra.bottle.
######################################################################## 100.0%
==> Pouring boost-1.65.1.sierra.bottle.tar.gz
🍺  /usr/local/Cellar/boost/1.65.1: 12,679 files, 401.1MB
```

`/usr/local/Cellar/boost/1.65.1` へインストールされた模様。  
つまり、 `/usr/local/Cellar/boost/1.65.1/lib` にライブラリ、 `/usr/local/Cellar/boost/1.65.1/include` にヘッダファイルが配置されている。

boost_sample.cc
```cpp
#include <iostream>
#include <boost/foreach.hpp>
#include <vector>

#include <boost/range/algorithm.hpp>

using namespace std;

void dump(vector<int>& v) {
    BOOST_FOREACH(int x, v) {
        cout << x << endl;
    }
}

int main (int argc, char *argv[]) {
    vector<int> v;

    v.push_back ( 3 );
    v.push_back ( 4 );
    v.push_back ( 1 );
    v.push_back ( 2 );

    cout << "Before sort" << std::endl;
    dump (v);

    boost::sort(v);

    cout << "After sort" << std::endl;
    dump (v);
    return 0;
}
```

コンパイル、実行する。

```sh
$ g++ boost_sample.cc -L/usr/local/Cellar/boost/1.65.1/lib -I/usr/local/Cellar/boost/1.65.1/include
$ ./a.out
Before sort
3
4
1
2
After sort
1
2
3
4
```

## 外部ライブラリの入手

Macの場合は `brew` で入手できれば `/usr/local/Cellar` 配下に配備されるので、そちらにパスを通す。  
`brew` で入手できないものはコンパイル済みのものを入手、もしくはソースを入手してローカルでコンパイルする。

## 参考

- [C++の標準ライブラリを簡単に眺めてみよう](https://www.slideshare.net/maraigue/c-73294701)
- [外部ライブラリ](https://cpprefjp.github.io/third_party_library.html)

# make

http://qnighy.hatenablog.com/entry/20100117/1263729044
