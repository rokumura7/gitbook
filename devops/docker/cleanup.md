# Cleanup

## 停止中のコンテナ、使われていないネットワーク、使われていないイメージ、使われていないキャッシュの削除

```text
$ docker system prune
```

## イメージの削除

### タグのついていないイメージの削除

```text
$ docker images -qf dangling=true | xargs docker rmi
```

### タグの有無に関わらずまとめて削除

```text
$ docker images -q | xargs docker rmi -f
```

## コンテナの削除

### 紐付いたデータボリュームも削除

```text
$ docker ps -aqf status=exited | xargs docker rm -v
```

## ボリュームの削除

### 浮いたボリュームの削除

```text
$ docker volume ls -qf dangling=true | xargs docker volume rm
```

## Docker Composeコンテナの削除

```text
$ docker-compose rm -v
```

## 参考

* [Dockerのお掃除](https://qiita.com/MasanoriIwakura/items/53cb7968e44f7a25e083)
* [Dockerのイメージを削除ができない時は「-f」オプションを使う](https://qiita.com/jungissei/items/5907819063a177ac7c81)
* [Docker for Macを使っていたら50GB位ディスク容量を圧迫していたのでいろんなものを削除する](https://qiita.com/shinespark/items/526b70b5f0b1ac643ba0)

