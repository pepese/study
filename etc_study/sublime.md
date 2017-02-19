# インストール

[公式](https://www.sublimetext.com/3)から「portable version」を入手して展開するだけ。  
（Windowsではportable versionでないとプロキシ環境下で何故か動かなかった）  
MacユーザはHomebrewを使用して以下のようにインストール。

```sh
$ brew cask install sublime-text
```

Homebrewについては以下を参照。

[http://blog.pepese.com/entry/2016/12/15/202723:embed:cite]

# 起動方法

アイコンクリックでもいいがコマンドラインで起動できる。

```sh
$ subl file
```

プロジェクト（カレントディレクトリ）で起動したい場合は以下。

```sh
$ subl .
```

# Package Controlのインストール

パッケージの検索・インストールを簡単にしてくれる。  
「View」→「Show Console」からコンソールを起動。  
下記を実行。

```
import urllib.request,os,hashlib; h = 'df21e130d211cfc94d9b0905775a7c0f' + '1e3d39e33b79698005270310898eea76'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

プロキシ環境の場合は下記を実行。

```
import urllib.request,os,hashlib; h = 'df21e130d211cfc94d9b0905775a7c0f' + '1e3d39e33b79698005270310898eea76'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler({"http": "http://[username]:[password]@[proxy_server]:[port]", "https": "https://[username]:[password]@[proxy_server]:[port]"})) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

「[username]:[password]@[proxy_server]:[port]」の箇所はプロキシ環境に合わせて書き換える。  
また、最新のインストールコマンドは[公式](https://packagecontrol.io/installation)を参照。  
　  
その他、以降のオペレーションなどでプロキシ関係でうまくいかないときは、

- 「Preferences」→「Package Settings」→「Package Controll」→「Settings-User」

を選択して、 ```Package Control.sublime-settings``` ファイルを開き、編集する。

```js:PackageControl.sublime-settings
{
  "http_proxy": "プロキシサーバのアドレス:ポート番号",
  "https_proxy": "プロキシサーバのアドレス:ポート番号",
  "proxy_username": "プロキシのユーザ名",
  "proxy_password": "プロキスのパスワード"
}
```

その他設定は[ここ](https://packagecontrol.io/docs/settings)を参照。（※プロキシの設定とか）

# 日本語化

**Japanize** というパッケージを導入する。

- 「Preferences」→「Package Control」を選択（ ```Command + Shift + P``` ）
- 「Package Control: Install Package」を選択
- 「Japanize」を選択
- 「Package Control Message」の内容に従う

Macユーザは以下の手順で。

1. ```~/Library/Application Support/Sublime Text 3/Packages/Japanize``` にインストールされている*.jpファイルを、以下にコピー。
  - ```~/Library/Application Support/Sublime Text 3/Packages/Default```
  - Defaultフォルダがない場合は自分で作成
1. コピーしたファイルをオリジナルのファイル（.jpが付かないファイル）と置き換える。
1. ```~/Library/Application Support/Sublime Text 3/Packages/Japanize/Main.sublime-menu```（.jpが付かない方）を、以下にコピー。
  - ```~/Library/Application Support/Sublime Text 3/Packages/User```
  - すると、他のプラグインで上書きされてしまっているトップメニューも日本語化される。

# カスタマイズ

## テーマの変更

- 「Package Control: Install Package」で使用したいテーマを検索してインストール
- 「Preferences」→「Settings」を選択して ```Preferences.sublime-settingsーUser``` ファイルを開く
- 設定ファイルに各テーマ用のコードを記述・保存

ググって好きなテーマを探す。  
筆者は**Material Theme**をインストールして以下の設定を行った。

```js:Preferences.sublime-settingsーUser
{
  "theme": "Material-Theme-Darker.sublime-theme",
  "color_scheme": "Packages/Material Theme/schemes/Material-Theme-Darker.tmTheme"
}
```

## インデントの変更
- 「Preferences」→「Settings - User」を選択して設定ファイルを開く
- 「"tab_size": 2」（タブサイズが２）を追記
- 「"translate_tabs_to_spaces": true」（タブをスペースに変換）を追記して保存

# その他

基本には

- 「Package Control: Install Package」（ ```Command + Shift + P``` してInstall Package）
- 好きなパッケージを入力・インストール

でパッケージをインストールできる。  
プログラマな人は下記のパッケージ辺りを入れておくといいかも。

- AdvancedNewFile
  - ```Option + Command + N``` で新規ファイルを作成できる
- All Autocomplete
  - コード補完を補強するパッケージ
- SublimeLinter
  - 下記の「SublimeLinter-XX」を使うためのフレームワーク
- SublimeLinter-XX
  - リアルタイムに構文チェック
  - XXの部分に任意の文字が入っていろんな種類がある
    - SublimeLinter-javac
    - SublimeLinter-ruby
    - その他「https://github.com/SublimeLinter」
- BracketHighlighter
  - []、()、{}、””、”、などの開始、終了をハイライトしてくれるパッケージ
- GitGutter
  - Git管理してるファイルを編集すると「+ -」で行番号のとこに表示
- SideBarEnhancements
  - サイドバーのファイル/フォルダ操作を拡張
- ConvertToUTF8
  - UTF-8以外の文字コードのファイルをUTF-8にしてくれる
- SublimeCodeIntel
  - 補完機能を強化、Tabによるコード補完補完
- BoundKeys
  - 複雑になったショートカットキーを確認できるパッケージ

# 便利機能

- [ショートカット](http://qiita.com/elly/items/1ae275d1b2432cc98d68)
- [スニペット](http://nelog.jp/how-to-set-sublime-text-snippet)
  - 少しのキーボード入力をトリガーとして、よく利用するコードなどを手軽に書き込むことができる機能
- [キーバインド](http://qiita.com/shibainurou/items/dc18f2dfc91e36adb208)
- [画面分割]
  - ```Alt + Shift + N``` で好きな画面の個数に分割できる。（Nは好きな数）
  - 変則的に分割したい場合は[ここ](http://daredemopc.blog51.fc2.com/blog-entry-1091.html)
