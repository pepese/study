STSはSpringFrameworkをサポートしたEclipse。  
STS（Spring Tool Suite）のインストールと設定についてまとめる。  
普通のEclipseでも同様なものがほとんど。ただし、Java系に偏る。

# インストール

[公式](http://www.springsource.org/downloads/sts-ggts)から入手。  
筆者は「STS-3.8.3.RELEASE」を使用した。（EclipseはNeon）  

もしくはMacユーザは下記のようにHomebrewで導入可能。

```sh
$ brew cask install sts
```

Homebrewについては以下を参照。

[http://blog.pepese.com/entry/2016/12/15/202723:embed:cite]

# 各種設定

以下について書く。

- 日本語化
- プロキシ設定
  - プロキシ環境で開発する際、ID・パスワードを設定する必要がある
- プラグインの導入
- フォーマッタの設定
  - ```Ctrl + Shift + F``` （Win）、 ```Command + Shift + F``` （Mac）でコードを整形してくれる際の設定
- Emacs風のキーバイドに変える
- Lombokの導入

## 日本語化

日本語化には「pleiades」を使用する。  
[ここ](http://mergedoc.sourceforge.jp/)から「 Pleiades プラグイン本体」のみをダウンロードする。

1. ```pleiades.zip``` を解凍。
1. ```features``` 、 ```plugins``` をSTSの```features``` 、 ```plugins``` に上書きする。
1. pleiadesのREADMEの通り、 ```STS.ini``` に下記を追記する。
  - READMEには ```STS.ini``` ではなく ```eclipse.ini``` と書いている

```
-Xverify:none
-javaagent:plugins/jp.sourceforge.mergedoc.pleiades/pleiades.jar
```

## プロキシ設定

1. 「ウィンドウ」→「設定」
1. 「一般」→「ネットワーク接続」
1. 「アクティブ・プロバイダー」を「マニュアル」に設定
1. 「プロキシー・エントリー」のHTTP(S)に対してプロキシサーバのホスト・ポート・認証ユーザ・パスワードを設定
1. 「適用」ボタン

## プラグインの導入

1. 「ヘルプ」→「Eclipseマーケットプレース」
1. 望みのプラグインを検索し、「インストール」ボタン

## フォーマッタの設定

フォーマッタは、CheckStyleなどのコードインスペクションツールを使用する場合はルールを合わせることが望ましい。  
CheckStyleの設定ファイルからフォーマッタのルールを生成する方法については後述する。  
以下は手動でフォーマッタを設定する場合。

1. 「ウィンドウ」→「設定」
1. 「Java」→「コード・スタイル」→「フォーマッタ―」

## Emacs風のキーバイドに変える

- [Spring Tool Suite]→[環境設定]→[General]→[Keys]の「Scheme」欄を「Emacs」にするだけ（Mac）

以下が未対応なので追加設定する。

- ```Ctrl + H```
    - 1つ前の文字を消すショートカット
    - デフォルトではショートカット自体設定されていない
    - 「Open Search Dialog」が衝突しているのでまずこちらを```Ctrl + Option + S```に設定
    - その後「Delete Previous」（前を削除）に```Ctrl + H```を設定

よく使うショートカットは以下。

- ```Command + /```
    - タイピング中に候補を表示


Mark set: ctrl + space

## Lombokの導入

[Lombok](https://projectlombok.org)を使用するとSetter/Getterなど冗長なコードを削減できる。  
しかし、STS/Eclipseがその省略を理解できずエラー表示されてしまう。  
以降の手順を行うと、エラーが出なくなる。

- lombok.jarを下記のフォルダにコピー
  - ```/Applications/STS.app/Contents/MacOS``` （Mac + Homebrewの場合）

```sh
$ cp ~/.m2/repository/org/projectlombok/lombok/1.16.10/lombok-1.16.10.jar /Applications/STS.app/Contents/MacOS/lombok.jar
```

- STS.iniの編集
  - ```/Applications/STS.app/Contents/Eclipse/STS.ini``` （Mac + Homebrewの場合）
  - 以下を追記

```
-javaagent:lombok.jar
-Xbootclasspath/a:lombok.jar
```

- STSの再起動
- エラーが消えない場合はプロジェクトのクリーン

# いつも使うプラグインと使い方概要

インストールは先に記載した「プラグインの導入」を参照。

- CheckStyle
- FindBugs

## CheckStyle

1. プロジェクトを右クリック→「プロパティ―」

<table border="0">
<tr>
<td>
<iframe src="http://rcm-jp.amazon.co.jp/e/cm?t=tanakakns-22&o=9&p=8&l=as1&asins=4797367474&ref=qf_sp_asin_til&fc1=000000&IS2=1&lt1=_blank&m=amazon&lc1=0000FF&bc1=000000&bg1=FFFFFF&f=ifr" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
</td>
</tr>
</table>
