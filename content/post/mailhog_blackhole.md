---
title: MailHogでメールを/dev/nullに捨てる方法
date: 2023-06-09T15:29:20+09:00
draft: false
tags: ["Mail"]
images: ["images/articles/avatar.png"]
description: "MailHogでメールを/dev/nullに捨てる（black hole mail serverにする）方法を調べました。-maildir-pathオプションに/dev/nullを渡すことで実現できました。"
---

## mailhogでメールを/dev/nullに捨てたい

MailHogは、Go言語で書かれたSMTPサーバーです。

<div class="iframely-embed"><div class="iframely-responsive" style="padding-bottom: 50%; padding-top: 120px;"><a href="https://github.com/mailhog/MailHog" data-iframely-url="//cdn.iframe.ly/api/iframe?url=https%3A%2F%2Fgithub.com%2Fmailhog%2FMailHog&key=6da8f492348dcf72ef42ec6631ea70ef"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>

開発環境やステージング環境でメール送信をシミュレートする際に便利です。送信されたメールをキャプチャして、ブラウザベースのユーザーインターフェイスで確認ができます。

しかし検証したい内容によっては、メールをキャプチャする必要がない場合もあります。
この場合メールを/dev/nullに捨てることで、必要以上にメールサーバーのメモリを消費することを防ぐことができます。

AWSなどクラウドインフラを利用している場合、メモリをたくさん積むと料金も嵩みます。アプリケーション側でのメール送信処理のテストを実行したいが、ブラウザ上でのメール本文の確認が不要という場合、メールをキャプチャせずに捨てたいです。

MailHogのREADMEを見ても設定方法が書いていなかったので、オプションで指定できるのか調べました。

[CONFIG.md](https://github.com/mailhog/MailHog/blob/master/docs/CONFIG.md)を見たところ、良さそうなオプションがあったので使ってみました。

```bash
-maildir-path
```

「maildir storage backend」を指定するオプションだそうなので、このオプションでストレージに `/dev/null` を指定してあげるとよさそうです。

CMDの上書きを利用して、コンテナで動かしてみました。ベースになるimageは[公式のDocker image](https://hub.docker.com/layers/mailhog/mailhog/v1.0.1/images/sha256-8d76a3d4ffa32a3661311944007a415332c4bb855657f4f6c57996405c009bea)を利用しています。

タスク定義はこんな感じです。 `command` のところでコマンドの上書きを利用し、オプションを指定しています。

```bash
[
    {
        "name": "${var.env_name}-mailhog-blackhole",
        "image": "mailhog/mailhog:v1.0.1",
          # ...（省略）
        "command": ["-maildir-path", "/dev/null"],
          # ...（省略）
        }
    }
]
```

この設定でコンテナを起動すると、期待通りメールをキャプチャせずに捨てることができました。

## 余談

MailHogのDockerfileを確認すると、デフォルトでSMTPのポート（1025）とHTTPのポート（8025）が開いていました。

```bash
CMD ["/bin/sh"]
/bin/sh -c apk add --no-cache
/bin/sh -c [ ! -e
ENV GOLANG_VERSION=1.14.7
/bin/sh -c set -eux; apk
ENV GOPATH=/go
ENV PATH=/go/bin:/usr/local/go/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
/bin/sh -c mkdir -p "$GOPATH/src"
WORKDIR /go
/bin/sh -c apk --no-cache add
/bin/sh -c adduser -D -u
USER mailhog
WORKDIR /home/mailhog
ENTRYPOINT ["MailHog"]
EXPOSE 1025 8025
```

[CONFIG.md](https://github.com/mailhog/MailHog/blob/master/docs/CONFIG.md)には、このポートを上書きした場合に使えるオプションもありました。

```txt
MH_API_BIND_ADDR  -api-bind-addr  0.0.0.0:8025 Interface and port for HTTP API server to bind to
MH_UI_BIND_ADDR   -ui-bind-addr   0.0.0.0:8025 Interface and port for HTTP UI server to bind to
MH_SMTP_BIND_ADDR -smtp-bind-addr 0.0.0.0:1025 Interface and port for SMTP server to bind to
```

なお、今回の場合Web UIからのアクセスはないので8025のポートでLISTENしている必要はありません。ただ、公開用のポートとEXPOSEで指定したポートを紐づけない限り外からアクセスはできないので、そのままにしておきました。
