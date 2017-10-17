かつてMacでDockerを使用する場合はBoot2Dockerを使用していたが、DockerはMacに対応（[Docker for Mac](https://docs.docker.com/docker-for-mac/)）した。  
Docker for Macの使い方をまとめる。

インストールにあたり、下記のHomebrew環境を整えておくことが前提となる。

[http://blog.pepese.com/entry/2016/12/15/202723:embed:cite]

# Docker for Macのインストール

```bash
$ brew cask install virtualbox
$ brew install docker
$ brew install docker-machine
```

上記のように各ツールを個別にインストールする方法もあるが、ここでは**Docker ToolBox**を使用する。  
Docker ToolBoxを使用すれば以下が一式で入る。

- Docker Client
    - ```docker```コマンド
    - Dockerコンテナをコントロールするツール
- Docker Machine
    - ```docker-machine```コマンド
    - Docker VMを管理するツール
    - Dockerコンテナは、Docker VM上で作成・起動する
    - ここではVirtualBoxベースで作成する
- Docker Compose
    - ```docker-compose```コマンド
    - 複数のDockerコンテナで構成されるサービスをコントロールする際、yamlファイルに設定を記載することにより複数のdockerコマンドを定められた順序で実行してくれる
- Docker Kitematic
    - GUIツール
- VirtualBox
    - ご存知、Oracle製、仮想マシンツール

Docker ToolBoxのインストールは以下。

```bash
$ brew cask install docker-toolbox
```

インストール後の確認は以下。

```bash
$ docker -v
$ docker-machine -v
$ docker-compose -v
$ virtualbox --help
```

変更があるかもしれないので[公式](https://docs.docker.com/docker-for-mac/)参照。

# Docker VM

Docker Machineを使用してDocker VMの管理を行う。  
DockerコンテナはDocker VM上で起動する。

## Docker VMの作成

defaultという名前のDocker VMをVirtualBoxで作成する場合、以下のコマンドを実行する。

```bash
$ docker-machine create --driver virtualbox default
```

作成されたDocker VMは```~/.docker/machine/machines/default```ディレクトリに存在する。  
作成したDocker VMの一覧は以下のコマンドで確認できる。

```bash
$ docker-machine ls
NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER    ERRORS
default   -        virtualbox   Running   tcp://192.168.99.100:2376           v1.12.4  
```

## Docker VMの管理

Docker VMの起動・停止状態確認は以下のコマンドで確認できる。（Docker VM名がdefaultの場合）

```bash
$ docker-machine start default  # 起動
$ docker-machine stop default   # 停止
$ docker-machine status default # 状態確認
```

default（Docker VM）が起動している状態で以下のコマンドを実行すると環境設定を見ることができる。

```bash
docker-machine env default
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/tanakakns/.docker/machine/machines/default"
export DOCKER_MACHINE_NAME="default"
# Run this command to configure your shell:
# eval $(docker-machine env default)
```

上記にあるコマンドの出力結果の通り、```eval $(docker-machine env default)``` を実行することにより、default（Docker VM）上にdockerコマンドでコンテナをコントロールを行う環境が整う。  
以下のコマンドを実行して```Hello from Docker!```と表示されれば確認完了。

```bash
$ docker run hello-world
```

なお、Docker VM上に起動したDockerコンテナに接続する場合は、 ```docker-machine IP VM名``` でIPを確認してから接続する。

# MySQLを起動してみる

MySQLのイメージを取得して起動。

```sh
$ docker run --name mysql-57 -e MYSQL_ROOT_PASSWORD=password -d mysql/mysql-server:5.7
```

コンテナへ接続。

```sh
$ docker exec -it mysql-57 mysql -uroot -ppassword
mysql>
```

切断。停止。

```sh
mysql> exit;
$ docker stop mysql-57
```
