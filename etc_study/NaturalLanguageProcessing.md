
# [Mecab](http://taku910.github.io/mecab/)

オープンソースの形態素解析エンジン。  
**条件付き確率場** と呼ばれる数理モデルを利用して、単語と単語間の繋がりの自然さを計算し、一番コスト（単語コスト、連結コスト）の低い、単語の列を選択するようにしている。  
単語ごとにコストを定義しておく必要があり、単語生成のコストが低いほど、その単語は選ばれやすくなる。

## 辞書の種類

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

## 辞書への単語の追加

- https://taku910.github.io/mecab/dic.html

## 用語

- 分かち書き
  - 単に文が形態素で区切られた形
    - 「ＭｅＣａｂで形態素解析を行うとこうなる．」
    - => 「ＭｅＣａｂ　　で　　形態素　　解析　　を　　行う　　と　　こう　　なる」
