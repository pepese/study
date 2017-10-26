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
- 擬似命令（特にここでは、 **インクルードガード** という）
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

## 例外

標準ライブラリ `<stdexcept>` の中に例外が定義されている。（ [参考](https://cpprefjp.github.io/reference/stdexcept.html) ）  
例えば、配列で確保したメモリ領域外にアクセスした場合の処理は **try-catch** で実現できる。

```cpp
#include <stdexcept>

void sampleFunction(Vector& v) {
    try {
        v[v.size()] = 1;
    } catch(out_of_range) {
        // 例外処理を記述
    }
}
```

## その他キーワード

- **コピー** と **ムーブ**
- **ラムダ式**
- **特殊化**
- **具体化**


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

## 標準ライブラリ

代表的な標準ライブラリは以下。

|#include|代表的なオブジェクト、関数|説明|
|:---|:---|:---|
|algorithm|copy(),find(),sort()|コンテナに対して適用するアルゴリスム|
|array|array|固定長オブジェクトを保持するシーケンスコンテナ|
|chrono|duration,time_point|時間に関するユーティリティ|
|cmath|sqrt(),pow()|算術演算ユーティリティ|
|complex|complex,sqrt(),pow()|複素数|
|fstream|fstream,ifstream,ofstream|ファイル入出力ストリーム|
|future|future,promise|非同期処理|
|iostream|istream,ostream,cin,cout|入出力ストリーム|
|map|map,nultimap|指定された型の要素の探索木機能を提供するコンテナ|
|memory|unique_ptr,shared_ptr,allocator|メモリアロケータ、スマートポインタ、GCなどのメモリに関するユーティリティ機能|
|random|default_random_engine,normal_distribution|乱数|
|regex|regex,smatch|正規表現|
|string|string,basic_string|文字列リテラル、文字列処理|
|set|set,multiset|指定された型の要素の集合を提供するコンテナ|
|sstream|istrstream,ostrstream|文字列ストリーム|
|thread|thread|スレッド機能|
|unordered_map|unordered_map,unordered_multimap|指定された型の要素の連想配列を提供するコンテナ|
|utility|move(),swap(),pair|各種ユーティリティ機能|
|vector|vector|指定された型の要素が並んだシケーンスであるコンテナ|

オブジェクトを内部に保持することを目的としたクラスを **コンテナ** と呼ぶ。

## ヘッダファイル

インターフェースを定義し、 **ヘッダファイル** `.h` にまとめて `#include` する。  
ヘッダファイルには以下のようなものが含まれる。

|項目|記述例|
|:---|:---|
|名前付き名前空間|namespace N {}|
|inline 名前空間|inline namespace N {}|
|型の定義|struct Point {int x, y;}|
|テンプレートの宣言|template<typename T> class Z|
|テンプレートの定義|template<typename T> class V {}|
|関数の宣言|extern int strlen(const char*);|
|inline 関数の定義|inline char get(char* p) {}|
|constexpr 関数の定義|constexpr int fac(int n) { return (n<2) ? 1 : n*fac(n-1);}|
|データの宣言|extern int a;|
|const の定義|const float pi = 3.141593|
|constexpr の定義|const float pi2 = pi*pi|
|列挙体|enum class Light {red, yellow, green};|
|名前の宣言|class Matrix;|
|型別名|using value_type = long;|
|コンパイル時のアサーション|static_assert(4<=sizeof(int), "small ints");|
|インクルード|#include <algorithm>|
|マクロ定義|#define VERSION 12.03|
|条件コンパイル|#ifdef __cplusplus|

以下のようなものは含むべきではない。

|項目|記述例|
|:---|:---|
|通常の関数定義|char get(char* p) {}|
|データの定義|int a;|
|集成体の定義|short tbl[] = {};|
|名前無し名前空間|namespace {}|
|using|using namespace Foo;|

## 参考

- [C++の標準ライブラリを簡単に眺めてみよう](https://www.slideshare.net/maraigue/c-73294701)
- [外部ライブラリ](https://cpprefjp.github.io/third_party_library.html)

# ビルドツール

ビルドツールには **GNU Make** 、 **CMake** 、 **GYP** などいくつかあるが、ここでは GNU Make を扱う。  
所謂 make と言われるものは GNU Make を指す。

## GNU Make

http://qnighy.hatenablog.com/entry/20100117/1263729044
[参考](https://www.ecoop.net/coop/translated/GNUMake3.77/make_toc.jp.html)

- コメント： `#`
- 改行： バックスラッシュ `\`
- コマンドでエコーバックさせない： 文頭に `@`

### **ルール** と **ターゲット**

```
[ターゲット]:
	[コマンド]
```

インデントは **タブ** 。  
上記の塊を **ルール** という。  
ターゲットには **ファイル** か **タスク** を指定できる。  
ターゲットにファイルを指定した際、すでにそのファイルが存在すると、 *コマンドは実行されない* 。  
**タスク** の場合は以下のように指定する。

```
.PHONY: [タスク名]

[タスク名]:
	[コマンド]
```

`.PHONY` でタスク用のターゲットを定義する。  
ターゲットを複数定義した場合、 `$ make [ターゲット]` で指定のターゲットを実行できる。  
ターゲットを指定しなかった場合は、最初のターゲットのみが実行される。  
複数ターゲットをスペース区切りで並べると、複数ターゲット実行される。（ `$ make target1 target2 ...` ）

`targetA` の事前条件として `targetB` の実行が必要な場合、以下のように書く。

```
targetA: targetB
	[targetAのコマンド]
```

例えば以下。

```
task1: task2
	@echo 1
task2:
	@echo 2
```

上記を実行すると以下のようになる。

```sh
$ make
2
1
```

さらに以下。

```
task1: task2 task3
	@echo 1
task2:
	@echo 2
task3:
	@echo 3
```

上記を実行すると以下のようになる。  
（ `3 -> 2 -> 1` の順ではないことに注意）

```sh
$ make
2
3
1
```

また、コマンド行が無いルールを作成することもできる。  
この場合、そのターゲットは **複数ターゲットをまとめる** 機能になる。

```
tasks: task1 task2 task3
```

### 変数

```
objects = program.o foo.o utils.o
program: $(objects)
        cc -o program $(objects)

$(objects): defs.h
```

変数を `$()` で展開して、ターゲットとして利用できる。  
変数への値の代入方法は以下もある。

- 単純展開変数 `:=`
    - その時点での値が展開されて代入される
    - 右辺の変数の値がのちの処理で変更されても、左辺まで反映されることはない
- 条件分岐変数割り当てオペレータ `?=`
    - その変数が定義されていない場合にのみ、変数が定義され、値が代入される

### 関数

関数の呼び出しは以下。

```
$(関数 引数)
${関数 引数}
```

どのような関数があるかは [ここ](https://qiita.com/chibi929/items/b8c5f36434d5d3fbfa4a) を参照。

### 条件分岐

```
[条件分岐ディレクティヴ]
  真の場合の内容
else
  偽の場合の内容
endif
```

`else` 句は省略可能。  
条件分岐のディレクティブには以下がある。

|ディレクティブ|説明|使い方|
|:---|:---|:---|
|ifeq|値が等しいか|ifeq (arg1, arg2)|
|ifneq|値が等しくないか|ifneq (arg1, arg2)|
|ifdef|変数は定義されているか|ifdef 変数|
|ifndef|変数は定義されていないか|ifndef 変数|
