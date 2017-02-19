Ruby on Railsの開発環境として以下が考え得る。

- コードエディタ
  - Vim
  - Emacs
  - Sublime Text
  - Atom
- 統合開発環境(IDE)
  - Aptana Studio 3(Eclipseベース)
  - RubyMine(有償IDE)

Java出身の人はEclipseベースのaptana studioがオススメ。

# Aptana Stucioのインストール

```sh
$ brew cask search aptana
==> Partial matches
aptanastudio
$ brew cask install aptanastudio
```

brewコマンドについては以下を参照。

[]

2016/12/23時点で上記を実行するとaptana studio 3.6.1がインストールされる。
しかし、Mac with Java1.8の環境では起動することができない。（Java1.6なら起動する模様？）
筆者は3.5.0を手動でダウンロードしてきてインストールした。
が、動かなかった。
筆者は3.4.2を手動でダウンロードしてきてインストールした。
が、動かなかった。

普通にEclipseをインストールしてaptana studio pluginを入れてみることにする。

# EclipseにAptana Studioプラグインを導入

## Eclipseのインストール

```sh
$ brew cask install eclipse-ide
```

## aptana studio pluginのインストール
ヘルプ→Eclipseマーケットプレース
「aptana」で検索してインストール

## ruby開発ツールのインストール

ヘルプ→Install New Software
「All Available Sites」にして「ruby」でフィルタする
「Dynamic Language Toolkit - Ruby Development Tools」をインストール

## RadRailsのインストール

http://download.aptana.com/tools/radrails/plugin/install/radrails-bundle
RadRails

## Rubyのバージョンを設定

Eclipse→環境設定
Ruby→Interpreters
add→好きなのを設定する
※Finderから見れない場合は直接パスを入力する
```which ruby```で出たパスを貼り付ける

## 既存のプロジェクトをインポート

Importから、「General]
「Existing Folder as New Project」を選ぶ

Aptana Studio使ってみたけど、アカンわ。。。
