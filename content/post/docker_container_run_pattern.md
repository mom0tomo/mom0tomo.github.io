---
title: "docker container runコマンドとdocker container startコマンドを使い分ける"
date: 2023-01-19T13:00:39+09:00
draft: false
tags: ["Docker"]
images: ["images/articles/avatar.png"]
description: "Dockerで8888ポートをLISTENするサーバーを起動して、Dockerコンテナからcurlで動作確認する場合の想定。docker container runコマンドとdocker container startコマンドを使う方法がある。"
---

Dockerで888ポートをLISTENするサーバーを起動して、`$ curl localhost:8888` で叩いて動作確認したい。
Dockerfile内でサーバーの起動設定と、Exposeで8888ポートをLISTENする設定を行なっているものとする。

```bash
# イメージをビルドしておく
$ docker image build -t app_image .
```

## docker container runコマンドを使ってコンテナの作成と起動を同時に行うパターン

docker container runコマンドはコンテナの作成と起動を同時に行う。また、指定したイメージがローカルになければダウンロードも行う。

### シンプルにrunする

```bash
$ docker container run --name app_run -p 8888:8888 app_image

# 別セッションから動作チェックする
$ curl localhost:8888
Hello World!

# 事後処理としてコンテナを停止して削除する

## 停止と削除を行わずにもう一度runコマンドを打つと、同じ名前のコンテナを作成しようとして
## The container name "/app_run" is already in use by container xxxとエラーになる
## 起動中のコンテナを確認する
$ docker container ls
CONTAINER ID   IMAGE     COMMAND   CREATED       STATUS                   PORTS     NAMES
8ad5f2b083bb   app_image    "node"     5 minutes ago   Exited (0) 3 minutes ago             app_run

$ docker container stop app_run
$ docker container rm app_run
## コンテナが削除されたことを確認する
$ docker container ls -a
CONTAINER ID   IMAGE     COMMAND   CREATED       STATUS                   PORTS     NAMES
```

### 停止後に自動でコンテナを削除する

--rmオプションをつけて起動する。

```bash
# ーーrmオプションを付与する
$ docker container run --rm --name app_run -p 8888:8888 app_image

# 別セッションから動作チェックする
$ curl localhost:8888
Hello World!
```

## docker container createでコンテナを作成し、docker container startで起動するパターン

```bash
# コンテナを作成する
$ docker container create --name app_create -p 8888:8888 app_image
# コンテナを起動する
$ docker container start app_create
app_create

# 動作チェックする
$ curl localhost:8888
Hello World!

# 事後処理としてコンテナを停止し、削除する
## 起動中のコンテナを確認する
$ docker container ls
CONTAINER ID   IMAGE     COMMAND   CREATED       STATUS                   PORTS     NAMES
8ad5f2b083bb   app_image    "node"     5 minutes ago   Exited (0) 3 minutes ago             app_run
$ docker container stop app_run
$ docker container rm app_run
## コンテナが削除されたことを確認する
$ docker container ls -a
CONTAINER ID   IMAGE     COMMAND   CREATED       STATUS                   PORTS     NAMES
```

## おわりに

普段WebアプリケーションをDockerで起動する場合はdocker-composeを利用することが多いので、あまりこの辺りのコマンドについて調べたことがなかった。
Dockerを使ってワンライナーを叩くときは `$ docker container run --rm` を利用することが多い。

参考: [Docker コンテナで起動したシェルに接続する (docker container run/start/exec/attach)](https://maku77.github.io/p/y8cfimp/)
