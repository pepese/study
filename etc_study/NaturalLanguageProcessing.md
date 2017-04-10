自然言語処理 （NLP : Natural Language Processing）
===

要素技術の全体像は[ここ](https://www.slideshare.net/pfi/ss-11474303#7)。  
一部表に整理したのが以下。

<table>
<tr><td rowspan="2">単語分析</td><td>形態素解析   </td><td>大量の辞書と文法知識を使って品詞に分解   </td><td>辞書・文法アプローチ</td></tr>
<tr>                            <td>N-gram解析   </td><td>機械的に文をN個に分類                  </td><td>数学的アプローチ  </td></tr>
<tr><td>構文解析            </td><td>形態素解析   </td><td>品詞情報から係り受け判別を行い文構造を解析</td><td>辞書・文法アプローチ</td></tr>
<tr><td rowspan="2">意味分析</td><td>シソーラス解析 </td><td>大量の類義語・反義語辞書（シソーラス）を使って出現語をグルーピング化</td><td>辞書・文法アプローチ</td></tr>
<tr>                            <td>ベクトル空間解析</td><td>統計解析的手法を使って語の近傍度を数学的に計算</td><td>数学的アプローチ  </td></tr>
</table>

N-gram解析のイメージは[ここ](https://www.slideshare.net/kazoo04/ss-56821735#31)。

# 基礎技術

- 形態素解析
  - 入力文を言語学的に意味を持つ最小単位である形態素（一般的単語よりやや細かい単位）に分割
  - 各形態素の品詞を決定
  - 活用などの語形変化をしている形態素に対しては原型を割り当て
- 構文解析
  - 文のタイプ（疑問文や命令文）を判別
  - 動詞句や名詞句といった句ごとのまとまりを見つける
  - 語と語の係り受け関係を調べる
- 重要語句の抽出
  - どのような内容が多いか少ないか、増えているか減っているか
  - どのような内容とどのような内容が統計的に関連性が高いか
  - 文章の内容として、どのような語句をどのような単位で抽出するかが分析結果の有効性に大きく影響
  - 抽出対象はキーワード
    - 出現頻度の高い語
    - 同じ頻度であれば、動詞より名詞
    - 係り受け関係をもつ体言と用言の組み合わせ
  - 問題点
    - 頻度が低いと、重要性が高くても、分析結果から外れてしまう
    - 頻度が高くても、重要性が低い語であれば分析結果における有用性が低くなる
    - そこで、抽出の単位をどうとるかで頻度の調整
      - 頻度が低すぎる　→　より短い単位に
      - 頻度が高すぎる　→　より長い単位に
      - 例:　「東京」　「基礎」　「研究所」 →　「基礎研究所」や「東京基礎研究所」に
- 語句内容の意味的役割の認識
  - 述語の分類　→　付属語情報を利用
  - 名詞概念　→　固有名詞や単位を伴う数値情報の抽出
  - 特定の目的に添った語句　→　テンプレートパターンとのマッチングによる５W1Hとして抽出
- 同義性の認識
  - テキストマイニングでは、表現の多様性を吸収するための技術が重要
  - 同義語辞書のような知識を参照
  - 各語がどのような語と係受けを結ぶかを構文解析により対象データから抽出
- データマイニング
  - 相関ルール（association rule）
    - 全文データベースシステムに蓄積された全テキスト集合の中から、XとYをともに含むテキストに成り立つ関係を扱う
  - 支持度（X⇒Y）
    - データベース全体の中でXとYをともに含むテキスト集合の割合
  - 確信度
    - データベース全体の中でXを含むテキスト集合のうち、XとYをともに含むテキスト集合の割合
  - 相関ルールX⇒Y
    - 単語集合Xと単語集合Yが、テキスト集合間で共起関係（cooccurrence)を示す
  - 検索式記述の修正に用いる
    - 全文データベースに格納された文書間の相関関係を示す

- [心理データ解析演習（心理デザインデータ解析演習）](http://cogpsy.educ.kyoto-u.ac.jp/personal/Kusumi/datasem.htm)
  - [心理データ解析演習：第５回
テキストマイニング入門](http://cogpsy.educ.kyoto-u.ac.jp/personal/Kusumi/datasem13/oka.pdf)
- [心理学のためのデータ解析法](http://cogpsy.educ.kyoto-u.ac.jp/personal/Kusumi/kaiseki.htm)


## 形態素解析

代表的なツールに **MeCab** 、 **JUMAN** 、茶筌( **ChaSen** )、 **KAKASI** がある。

<table>
<tr class="even">
<td align="center"></td>
<td align="center"><b>MeCab</b></td>
<td align="center"><a href=
"http://chasen.naist.jp/">ChaSen</a></td>
<td align="center"><a href="http://pine.kuee.kyoto-u.ac.jp/nl-resource/juman.html">JUMAN</a></td>
<td align="center"><a href="http://kakasi.namazu.org">KAKASI</a></td>
</tr>
<tr class="odd">
<td align="center">解析モデル</td>
<td align="center">bi-gram マルコフモデル</td>
<td align="center">可変長マルコフモデル</td>
<td align="center">bi-gram マルコフモデル</td>
<td align="center">最長一致</td>
</tr>
<tr class="even">
<td align="center">コスト推定</td>
<td align="center">コーパスから学習</td>
<td align="center">コーパスから学習</td>
<td align="center">人手</td>
<td align="center">コストという概念無し</td>
</tr>
<tr class="odd">
<td align="center">学習モデル</td>
<td align="center"><a href="http://www.cis.upenn.edu/~pereira/papers/crf.pdf">CRF</a> (識別モデル)</td>
<td align="center">HMM (生成モデル)</td>
<td align="center"></td>
<td align="center"></td>
</tr>
<tr class="even">
<td align="center">辞書引きアルゴリズム</td>
<td align="center">Double Array</td>
<td align="center">Double Array</td>
<td align="center">パトリシア木</td>
<td align="center">Hash?</td>
</tr>
<tr class="odd">
<td align="center">解探索アルゴリズム</td>
<td align="center">Viterbi</td>
<td align="center">Viterbi</td>
<td align="center">Viterbi</td>
<td align="center">決定的?</td>
</tr>
<tr class="even">
<td align="center">連接表の実装</td>
<td align="center">2次元 Table</td>
<td align="center">オートマトン</td>
<td align="center">2次元 Table?</td>
<td align="center">連接表無し?</td>
</tr>
<tr class="odd">
<td align="center">品詞の階層</td>
<td align="center">無制限多階層品詞</td>
<td align="center">無制限多階層品詞</td>
<td align="center">2段階固定</td>
<td align="center">品詞という概念無し?</td>
</tr>
<tr class="even">
<td align="center">未知語処理</td>
<td align="center">字種 (動作定義を変更可能)</td>
<td align="center">字種 (変更不可能)</td>
<td align="center">字種 (変更不可能)</td>
<td align="center"></td>
</tr>
<tr class="odd">
<td align="center">制約つき解析</td>
<td align="center">可能</td>
<td align="center">2.4.0で可能</td>
<td align="center">不可能</td>
<td align="center">不可能</td>
</tr>
<tr class="even">
<td align="center">N-best解</td>
<td align="center">可能</td>
<td align="center">不可能</td>
<td align="center">不可能</td>
<td align="center">不可能</td>
</tr>
</table>

### [Mecab](http://taku910.github.io/mecab/)

オープンソースの形態素解析エンジン。  
**条件付き確率場** と呼ばれる数理モデルを利用して、単語と単語間の繋がりの自然さを計算し、一番コスト（単語コスト、連結コスト）の低い、単語の列を選択するようにしている。  
単語ごとにコストを定義しておく必要があり、単語生成のコストが低いほど、その単語は選ばれやすくなる。

#### 辞書の種類

Mecabには、有志の方が固有名詞に対応したipadic-neologd辞書などいくつか種類がある。

- IPA辞書（IPADIC）
- [mecab-ipadic-NEologd](https://github.com/neologd/mecab-ipadic-neologd)
  - 有志によりIPA辞書が強化された辞書。新語・固有表現に強い。
- NAIST
  - IPAdic の後継。IPA辞書の固有名詞以外の全エントリをチェック（可能性に基づく品詞の整理）し、表記ゆれ情報を付与し、複合語の構造を付与する作業が行われている。
  - 固有名詞については不要な語、新規追加などの整理が随時行われている。
  - この作業により IPA辞書 のライセンスで問題となっていた ICOT 条項を削除し、広告条項無しの BSD ライセンスに変更された。
- UniDic
  - IPA 辞書よりも個々の単語を詳細に分類したもので、分割した形態素が文中で果たす役割をより精密に検出することができる。
  - UniDic現代語版
  - UniDic近代文語版
- JUMAN
- Yahoo形態素解析
- [IPA、NAIST、UniDic、JUMANの辞書実演比較](http://www.mwsoft.jp/programming/munou/mecab_dic_perform.html)

#### 辞書への単語の追加

- https://taku910.github.io/mecab/dic.html


## 構文解析

文章を構文木（Syntactic Tree）にして、その文章がどのような文構造を持つかを明らかにする。
文法規則によって、文の構造を句・文節を単位として解析する。  
句とは、2つ以上の語が集まって1つの品詞と同じような働きをしながら、文を構成する語の塊のことである。  
名詞の役割を果たす句を名詞句(NP)、動詞の役割を果たす句を動詞句(VP)とするように、形容詞句(ADJP)、副詞句(ADVP)などがある。  
英語では、句構造で構文解析を行うが、日本語の場合は、文節を単位に係り受け関係を用いて構文を解析するのが一般的である。  
係り受け関係を解析するフリーソフトとしては、JUMANをベースとした「KNP」、茶筌をベースとした「南瓜(CaboCha)」がある。

## 意味解析・意味理解

文中の単語は何らかの意味をもち、また文中で依存関係にある２語の間には何らかの意味関係がある。  
しかし、それらの意味はその単語だけ、あるいは依存関係を示す表現だけを見ていたのでは必ずしも一意に決まらない。  
これは、単語には複数の意味をもつものがあり、依存関係を示す表現、例えば名詞をつなぐ「AのB」という表現が示す意味関係にも複数ありえるからである。  
このような曖昧性の中には、文脈情報がなければ解釈できないもの、あるいは文脈によってもわからないものがある。

- 語義曖昧性解消 WSD (Word Sense Disambiguation)
- 述語項構造解析
- テキスト含意認識
- 格文法
- フレーム意味論
- 述語項構造解析ツール
  - 文章中に出現する述語とその格要素を同定するツール
    - SynCha 日本語の述語項構造解析器
    - ChaPAS Javaベースの日本語述語項構造解析器
    - YuCha 日本語述語項構造解析器「夕茶」
- 意味解析ツール
  - シソーラスを用いて、文章中に出現する述語とその格要素を分類するツール。フレーム意味論といった高次までは難しい。
    - 意味解析システムSAGE
    - 日本語 意味解析ツール AYA
- ベクトル空間解析による意味解析
  - 潜在意味解析（LSA: Latent Semantic Analysis）
  - ベクトル空間モデル
  - TF-IDF
  - word2vec
    - http://business.nikkeibp.co.jp/article/bigdata/20141110/273649/

# テキストマイニング

テキストマイニングはデータマイニングの派生語であり、1990年代の後半に用いられた新しい用語で、膨大に蓄積されたテキストデータを何らかの単位(文字、単語、フレーズ)に分解し、これらの関係を定量的に分析することである。  
しかし、テキストを構成する要素を定量的に分析する手法は、テキストマイニングという用語が使われる、はるか前から計量文体分析のような分野で用いられてきた。

- 計算的テキスト解析(computational text analysis)
- 統計的テキスト解析(statistical text analysis)
  - 計量文体学
    - 文章を構成する要素(文字、単語、文節、文、段落など)について統計的に集計分析を行い、文章のジャンルの特徴や個別作家の文体の特徴を定量的に解析する
  - 計量言語学とコーパス言語学
    - 言語現象を定量的に分析する分野
    - コーパス(corpus)とは、言語を分析のために集めた言語資料のこと

## テキストマイニングの分析の種類

- クラスター分析
  - 非階層的クラスター分析
    - 全対象の類似度（又は非類似度）を計算し、最も類似度の高いものから順次グルーピングする方法。
    - 最終的に1つのクラスターになるまで繰り返す。
    -　デンドログラムと呼ばれる樹形図で表現され、結びつきの階層構造を明確にできる。
  - 階層的クラスター分析
    - 分割の数などを与えて全体を一気に分割する方法。
    - 最適な分割数を決めるための仮定などを予め決める必要がある。
    - 結びつきの階層構造は分からない。
- ネットワーク分析
- 主成分分析
- 対応分析
- 潜在的意味解析
- etc…

### 階層的クラスター分析

手順

1. すべてのクラスターの組(初めは要素)に対して、クラスター間の距離(非類似性)を求める
2. クラスター間の距離(非類似性)を参照してクラスター間距離が最小のクラスターの組を結合し、新たなクラスターを作成する
3. 新たなクラスターとその他のクラスター間の距離(更新距離)を求める
4. クラスター数があらかじめ決められた数(通常は１)になるまで、2・3を繰り返す

文字データをどのように数値データとしてコードすればよいのか？  
それぞれの文章が持っている情報を、形態素解析の結果をもとに、２値データ、つまり、「ある文章に特定の単語が含まれているか(1)、いないか(0)」をデータにしたらどうか、と考える。  
テキストマイニングにおいてある文章(sn)は、全文中に含まれるすべての単語を要素(次元)とするベクトルとして表現できる。

|文章|高木（n_1）|田中（n_2）|尾崎（n_3）|ボール（n_4）|バット（n_5）|蹴る（v_1）|打つ（v_2）|
|---|---|---|---|---|---|---|---|
|高木がボールを蹴った（s_1）      |1|0|0|1|0|1|0|
|田中がバールを蹴った（s_2）      |0|1|0|1|0|1|0|
|尾崎がバットでボールを打った（s_3）|0|0|1|1|1|0|1|
|高木が田中を蹴った（s_4）        |1|1|0|0|0|1|0|
|田中が高木を蹴った（s_5）        |1|1|0|0|0|1|0|

★[これ](http://cogpsy.educ.kyoto-u.ac.jp/personal/Kusumi/datasem13/oka.pdf)整理してる途中★

クラスター分析では要素間の「非類似性」をもとにクラスタリングを行うことが分かっている。

- ２値データの場合の類似度： **Jaccard係数** （ **ジャッカール** ）
  - 集計したデータが２値データの場合や、間隔尺度のデータである場合は、それにあった非類似性の指標を用いる必要がある。
  - Jaccard係数は上記のようなデータを扱う際の「類似性」の指標。
- ２値データの場合の非類似度： **Jaccard距離**
  - Jaccard距離＝1 - Jaccard係数

- [KH Coderを用いたテキストマイニング入門](http://cogpsy.educ.kyoto-u.ac.jp/personal/Kusumi/datasem13/oka.pdf)

## ネットワーク分析

語の共起(共出現)パターン。
語の共起は、N-gramを含む広い意味での、語が文、あるいはテキストの中に同時用いられていることを指す。
N-gramモデルとは、「ある文字列の中で、n個の文字列または単語の組み合わせが、どの程度出現するか」を調査する言語モデルを意味します。
語のネットワークマップとは、基本的には、文、あるいはテキストの中で用いられた語をノードとし、同時に用いられた場合は、語と語を線(辺として)でリンクしたグラフである。


# 用語

- 分かち書き
  - 単に文が形態素で区切られた形
    - 「ＭｅＣａｂで形態素解析を行うとこうなる．」
    - => 「ＭｅＣａｂ　　で　　形態素　　解析　　を　　行う　　と　　こう　　なる」


# その他メモ

- 言語処理100本ノック 2015
  - http://www.cl.ecei.tohoku.ac.jp/nlp100/

- 同志社大学 R、R言語、R環境・・・・・・
  - https://www1.doshisha.ac.jp/~mjin/R/


分布一覧

|分布の総称|確率密度関数|乱数発生関数|
|:---|:---|:---|
|一様(Uniform)分布|dunif(x, min=0, max=1,･･･)|runif(n, min=0, max=1)|
|二項(Binomial)分布|dbinom(x, size, prob,･･･)|rbinom(n, size, prob)|
|ポアソン(Poisson)分布|dpois(x, lambda,･･･)|rpois(n, lambda)|
|正規(Normal)分布|dnorm(x, mean=0, sd=1,･･･)|rnorm(n, mean=0, sd=1)|
|カイ2乗(Chi-square )分布|dchisq(x, df, ncp=0,･･･)|rchisq(n, df, ncp=0)|
|t分布|dt(x, df,･･･)|rt(n, df)|
|F分布|df(x, df1, df2,･･･)|rf(n, df1, df2)|
|ガンマ(Gamma)分布|dgamma(x, shape,･･･)|rgamma(n, shape)|
|ベータ(Beta)分布|dbeta(x, shape1, shape2,･･･)|rbeta(n, shape1, shape2)|
|対数正規(Lognormal)分布|dlnorm(x, meanlog = 0, sdlog = 1,･･･)|rlnorm(n, meanlog = 0, sdlog = 1)|
|ロジスティック(Logistic)分布　|dlogis(x, ･･･)|rlogis(n)|
|指数(Exponential)分布|dexp(x, rate = 1, ･･･ )|rexp(n, rate = 1)|
|負二項(Negbinomail)分布|dnbinom(x, size, prob, mu,･･･ )|rnbinom(n, size, prob, mu)|
|多項(Multinomial)分布|dmultinom(x, prob, ･･･ )|rmultinom(n, size, prob)|
|幾何(Geometric)分布|dgeom(x, prob, ･･･ )|rgeom(n, prob)|
|超幾何(Hypergeometric)分布|dhyper(x, m, n, k, ･･･ )|rhyper(nn, m, n, k)|
|コーシー(Cauchy)分布|dcauchy(x,location=0,scale= 1,･･･ )|rcauchy(n,location=0,scale = 1)|
|ワイブル(Weibull)分布|dweibull(x,shape,scale=1,･･･ )|rweibull(n, shape, scale = 1)|
