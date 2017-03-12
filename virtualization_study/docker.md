dockerをちょっと整理してみる。

```sh
$ docker pull centos:6.8
$ docker run -it centos:6.8 /bin/bash
(detach)
```

# 使い方・概要

```sh
$ docker [OPTIONS] COMMAND [arg...]
```

|COMMAND|description|
|:---|:---|
|attach    |Attach to a running container|
|build     |DockerfileからDockerイメージを作成|
|commit    |Create a new image from a container's changes|
|cp        |ホストOSとDockerコンテナ間でファイルのコピー|
|create    |Create a new container|
|diff      |Inspect changes on a container's filesystem|
|events    |Get real time events from the server|
|exec      |Run a command in a running container|
|export    |Export a container's filesystem as a tar archive|
|history   |Show the history of an image|
|images    |Dockerイメージの一覧|
|import    |Import the contents from a tarball to create a filesystem image|
|info      |Display system-wide information|
|inspect   |Return low-level information on a container, image or task|
|kill      |Kill one or more running containers|
|load      |Load an image from a tar archive or STDIN|
|login     |Log in to a Docker registry.|
|logout    |Log out from a Docker registry.|
|logs      |Fetch the logs of a container|
|network   |Manage Docker networks|
|node      |Manage Docker Swarm nodes|
|pause     |Pause all processes within one or more containers|
|port      |List port mappings or a specific mapping for the container|
|ps        |Dockerコンテナの一覧|
|pull      |Pull an image or a repository from a registry|
|push      |Push an image or a repository to a registry|
|rename    |Rename a container|
|restart   |Restart a container|
|rm        |Dockerコンテナの削除|
|rmi       |Dockerイメージの削除|
|run       |Run a command in a new container|
|save      |Save one or more images to a tar archive (streamed to STDOUT by default)|
|search    |Search the Docker Hub for images|
|service   |Manage Docker services|
|start     |Start one or more stopped containers|
|stats     |Display a live stream of container(s) resource usage statistics|
|stop      |Dockerコンテナを停止する|
|swarm     |Manage Docker Swarm|
|tag       |Tag an image into a repository|
|top       |Display the running processes of a container|
|unpause   |Unpause all processes within one or more containers|
|update    |Update configuration of one or more containers|
|version   |Show the Docker version information|
|volume    |Manage Docker volumes|
|wait      |Block until a container stops, then print its exit code|

上記のようにいろいろコマンドはあるが、最低限使うものだけ後述する。  
なお、コマンド・オプションがわからないのときは ```docker COMMAND --help``` でヘルプが見れる。

## docker run

```sh
$ docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

以下、代表的なオプションメモ。

|OPTIONS|description|
|:---|:---|
|-c, --cpu-shares int|CPU shares (relative weight)|
|-d, --detach|Dockerコンテナをバックグラウンドで起動し、コンテナIDを標準出力する。|
|-e, --env value|環境変数を設定する。 (default [])|
|-h, --hostname string|Dockerコンテナのホスト名。|
|-i, --interactive|インタラクティブモード。標準入力を開いたままにする。-tとセットで使用。|
|-m, --memory string|Memory limit|
|-p, --publish value|Dockerコンテナ側のポートとホストOS側のポートを設定する。 (default [])|
|-t, --tty|疑似端末に接続する。Dockerコンテナの出力を擬似ディスプレイに出力・表示するイメージ。-iとセットで使用。|
|-v, --volume value|Bind mount a volume (default [])|
|-w, --workdir string|Working directory inside the container|

下記で、CentOSのイメージをインタラクティブモードで起動させてbashコマンドを起動（CentOSにログインした状態に同じ）。  
```PID=1``` の ```/bin/bash``` プロセスで接続しているので ```exit``` すると ```/bin/bash``` プロセスが終了し、コンテナも停止する。

```sh
$ sudo docker run -i -t centos /bin/bash
```

下記で、Swaggerをバックグラウンドで起動する。（ログインした状態にならない）

```sh
$ docker run -d -p 8001:8080 swaggerapi/swagger-editor
```

Swaggerのコンテナを起動する例にしたのは気まぐれ。

### 起動中のDockerコンテナにbash接続する方法

1. docker attach CONTAINER
  - Dockerコンテナで既に起動している ```/bin/bash``` プロセス（PID=1）に接続する
  - exitするとDockerコンテナも停止する
  - detachすればDockerコンテナは停止しない
1. docker exec -it CONTAINER /bin/bash
  - Dockerコンテナ内に新規で ```/bin/bash``` プロセスを起動し、接続する
  - exitしてもDockerコンテナは停止しない

**detach** の方法は ```Ctrl + P -> Ctrl + Q``` （ctrlを押しながらP -> Qと押す）。

## docker stop

Dockerコンテナを停止する。

```sh
$ docker stop CONTAINER
```

```CONTAINER``` の部分は **CONTAINER ID** だが、起動時に ```--name``` オプションで名前を指定しておけばそれでもよい。

## docker ps

Dockerのプロセス一覧を見ることができる。  
など、Dockerコンテナは停止しても、停止した状態でイメージが残る。  
停止したコンテナも含めて見たい場合は以下。

```sh
$ docker ps -a
```

コンテナの削除は以下。

```sh
$ docker rm CONTAINER
```

## ホストOS、Dockerコンテナ間でのファイルのやり取り

### コンテナからホストへのコピー

```sh
$ docker cp <コンテナID>:/etc/my.cnf my.cnf
```

### ホストからコンテナへのコピー
```sh
$ docker cp my.cnf <コンテナID>:/etc/my.cnf
```

## Docker commit

Dockerのイメージを作成する。

```sh
$ docker commit <コンテナID> <イメージ名>
```

### Dockerイメージ・コンテナの受け渡し

```save``` か ```export``` コマンドでtarファイルを作成できる。

```sh
$ docker save <イメージ名> > <イメージ名>.tar
$ docker export <コンテナ名> > <コンテナ名>.tar
```

違いは以下。

- [save](https://docs.docker.com/engine/reference/commandline/save/)
  - 上記のレイヤーやタグといったメタ情報含めてコンテナをtarでまとめる
- [export](https://docs.docker.com/engine/reference/commandline/export/)
  - ファイルシステムを愚直にtarでまとめ、メタ情報は無視される

ローカルでコンテナを別のDocker環境へ引き渡したい場合は、```save``` したものを渡し ```load``` してもらう。  
[Docker Hub](https://hub.docker.com)や[Docker Registry](https://docs.docker.com/registry/)といったリポジトリを経由して渡すことも可能。

```sh
$ docker load < イメージ名>.tar
```

## その他 Docker コマンド

http://qiita.com/curseoff/items/a9e64ad01d673abb6866

# ボリュームプラグイン/ストレージドライバ/データ保存用コンテナ

## ボリュームプラグイン

コンテナ外のストレージをコンテナ内に「ボリューム」としてマウントするためのプラグイン機構。  
Dockerはコンテナを削除するとコンテナ内のデータも消えてしまうが、コンテナ外のボリュームを利用することでデータを永続化できる。  
以下のように使用する。

```sh
$ docker run -v <ホスト上ディレクトリ>:<コンテナ内のマウント先> <イメージ>
$ docker run -v <ホスト上ディレクトリ>:<コンテナ内のマウント先>:ro <イメージ>
```

```:ro``` をつけることでホスト上のディスクへのアクセスにリードオンリー指定が可能。  
コンテナにマウントされているボリュームは ```docker inspect``` コマンドで確認できる。

## ストレージドライバ

Dockerがイメージやコンテナを管理する際に使用するもの。

[ボリュームプラグイン/ストレージドライバ参考](http://knowledge.sakura.ad.jp/knowledge/5021/)

## データ保存用コンテナ

ホストのストレージをコンテナにマウントするのではなく、データ保存用のコンテナを作成・利用することも可能。  
busyboxというのを使うのがあるあるのよう。  
http://qiita.com/TakashiOshikawa/items/11316ffd2146b36b0d7d

# ロギング

## Logging Driver

コンテナを削除するとコンテナ内のファイルシステムに書き込まれたデータも消えてしまう。ログもまた然り。  
Dockerではコンテナ外にログを出力することができる。  
[参考](http://knowledge.sakura.ad.jp/knowledge/6752/)

# イメージ置き場

[Dockerイメージ置き場の件](https://speakerdeck.com/ozzozz/dockerimezizhi-kichang-falsejian)

# 各種ミドルウェア搭載済みDockerコンテナの起動

## Redis

```sh
$ docker run --name redis -d -p 6379:6379 redis redis-server --appendonly yes
```

なお、 **Docker for Mac** などを使用してVM上にDockerコンテナを起動している場合は、localhost(127.0.0.1)で接続できないことに注意。  
```docker-machine``` を使用している場合は ```docker-machine ip default``` （defaultはVM名）でIPを確認して接続する。

# 参考

- [Dockerについて基本から最近追加された機能までまとめ](http://qiita.com/tigberd/items/b94ae2bf7d78685cd6f5)
  - entrypointの話が味噌
- [Dockerクラスタ swarm モード](http://qiita.com/zembutsu/items/8da9398d0be61e17aa4f)
- [Dockerドキュメント日本語化プロジェクト](http://docs.docker.jp/)
- [Docker入門](http://docker.yuichi.com/)
- [Dockerの基礎・使い方がよくわかる記事・スライド11選](https://career.levtech.jp/guide/knowhow/article/68/)
- [Dockerのライフサイクルを理解するハンズオン資料](http://qiita.com/zembutsu/items/d146295cfcf69c205c1e)
