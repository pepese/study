```sh
$ docker run -p 5000:5000 -d -v /var/registry:/tmp/registry registry:2
```

これでリポジトリが使える状態になりました。  
コンテナ上でイメージが格納される/tmp/registryと、ホストの/var/registryをマウントしています。  
Docker Registryはコンテナを止めると蓄積していたイメージが消えるため、基本的にホストとのマウントが必要です。

どのリポジトリにpushするかはタグに書かれているリポジトリ名で判断されますので、docker tagコマンドでタグを付与します。

```sh
$ docker tag docker.io/ubuntu localhost:5000/ubuntu
$ sudo docker images
```

HTTP(S)などのスキーマは不要。


タグを付けたらpushします。 localhost:5000を参照していることがログに出ています。

```sh
$ sudo docker push localhost:5000/ubuntu
```


取るとき

```sh
$ docker pull localhost:5000/test
```
