---
title: S3とCloudFrontで静的コンテンツをホスティングして、独自ドメイン＋HTTPSで配信する（with Terraform）
date: 2023-07-09T16:41:20+09:00
draft: false
tags: ["AWS", "Terraform", "S3", "CloudFront"]
images: ["images/articles/avatar.png"]
description: "S3とCloudFrontで静的コンテンツをホスティングして、HTTPSで配信したい。。HTTPSで配信したいときにいくつか詰まったポイントがあったので、その解決方法をまとめる。"
---

## やりたいこと

S3とCloudFrontで静的コンテンツをホスティングして、HTTPSで配信したい。

基本は下記のブログ記事の手順をTerraformに起こしたもの。

[CloudFront+S3に独自ドメインと証明書を添えて | DevelopersIO](https://dev.classmethod.jp/articles/cloudfront-s3-customdomain/)

記事との違いは、S3のURLへの直接アクセスを拒否するために、block public accessを有効化する点。また、CloudFrontのOrigin Access Control（OAC）を利用する点が記事とは異なる。上の記事ではまだOAC機能がリリースされていないので、レガシーな権限管理方法を利用している（「S3バケットへ直接アクセスは禁止する設定を入れる」のあたり）。

HTTPで配信する場合は、S3のbucketをパプリックアクセス可能にして「静的ウェブサイトホスティング」機能を有効化するだけで配信できる。
独自ドメインを利用する場合も、Route53にAレコードを追加し、S3側で発行されたエンドポイントを指定するだけで設定が完了する。

ただ、HTTPSで配信したいときにいくつか詰まったポイントがあったので、その解決方法をまとめる。

## 事前準備

- 独自ドメインを取得しておく。今回はexample.comとする。Route53でドメインを管理している前提で進める。
- S3 bucket名は使いたいドメインと同じ名前で作成しておく。今回の場合はexample.comというbucketを作る。
  - bucket名はグローバルでユニークでなければいけないので、他の人が使っていないか確認しておく（すでに同名のbucketが存在する場合、作成時にエラーになる）。

## 詰まったポイント

### HTTPSで配信したい場合、S3の静的ウェブサイトホスティングだけでは実現できない

S3 bucketのweb配信設定はHTTPにしか対応していない。HTTPSにしたい場合、CloudFront等を使う必要がある。

ACMで証明書を発行してRoute53で管理する独自ドメインに証明書を当てても、S3の静的ウェブサイトホスティングだけではHTTPSで配信できない。この理由は、S3の静的ウェブサイトホスティングはS3のエンドポイントを利用しているため。S3のエンドポイントはAWSが発行するドメインであるため、独自ドメインとの関連性に証明書を当ててもHTTPSで配信できない。

### CloudFrontからS3へのアクセスを制御する際はOrigin Access Control（OAC）を利用する

昔はレガシーな権限管理方法しかなかったが、その後Origin Access  Identity(OAI)という機能がリリースされ推奨されていた。
しかし、今はさらに時代が変わりOrigin Access Control（OAC）が推奨されている。

- [[NEW] CloudFrontからS3への新たなアクセス制御方法としてOrigin Access Control (OAC)が発表されました！](https://dev.classmethod.jp/articles/amazon-cloudfront-origin-access-control/)

OACは下記の記事の通り設定する。

- [【Terraform】CloudFront から S3 へのアクセス制御に Origin Access Control を利用する](https://zenn.dev/kou_pg_0131/articles/tf-cloudfront-oac)

### block public access有効・ACL無効の設定はデフォルトで指定されているのでいじらない

2023/04のアップデートで、S3の新規作成時にblock public accessがデフォルトで有効化されるようになった。また、ACLはデフォルトで有効化される様になった。
[今S3のIaCで「AccessControlListNotSupported: The bucket does not allow ACLs」というエラーが出たならそれは2023年4月に行われたS3の仕様変更が原因かもしれない](https://dev.classmethod.jp/articles/s3-acl-error-from-202304/)

Terraformで下記の様に書いている部分は指定が不要になった。とくにACLはデフォルトで有効化される様になったので、指定するとエラーになる。デフォルトのまま使えばいいので、触らない。

```terraform
// エラーになるので指定しない
resource "aws_s3_bucket_acl" "example" {
  bucket = aws_s3_bucket.example.bucket
  acl    = "private"
}

// エラーにはならないがデフォルトで有効化されているので指定する必要がない
resource "aws_s3_bucket_public_access_block" "example" {
  bucket                  = aws_s3_bucket.example.id
  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}
```

先に静的ウェブサイトホスティングを有効化して検証したときは注意が必要。
静的ウェブサイトホスティングをつかうときはblock public accessを無効化する必要がある。先に一度静的Webサイトホスティングを試した場合は、S3 bucketのblock public accessの設定を元に戻し、bucketを非公開状態にしておくことを忘れない。何か意図がある場合は公開のままにしてもよいが、基本的にセキュリティ上はCloudFrontを経由しない通信を拒否するようにした方がいい。

ちなみに一番最初の記事にあるレガシーな権限管理方法には「S3バケットへ直接アクセスは禁止する設定を入れる」というオプションがあるが、OACやOAIを利用する場合このオプションはない。S3のblock public accessを無効化したままにすると、S3のエンドポイントに直接アクセスできてしまう。このため、block public accessを有効化しておく。

### 独自ドメインを使う場合、ClopudFrontデフォルトの証明書は使えない。ACMで発行する必要がある

CloudFrontのデフォルトの証明書はTerraformで下記のように設定できる。

```terraform
    viewer_certificate {
        cloudfront_default_certificate = true # これは独自ドメインでは使えない
    }
```

CloudFrontのデフォルトの証明書は、*.cloudfront.netというドメイン向けに発行される。つまり、このデフォルトの証明書が使えるのはCloudFrontのドメインを利用する場合のみ。考えれば当たり前なのだが少し迷った。

今回の様に独自ドメインを使う場合は、ACMで証明書を発行しておく必要がある。

```terraform
    viewer_certificate {
        acm_certificate_arn = aws_acm_certificate.example.arn # これをあらかじめ発行しておく
        ssl_support_method  = "sni-only"
    }
```

### CloudFrontで使う証明書はus-east-1で発行する

CloudFrontで使う証明書は、us-east-1で発行する必要がある。他のリージョンで発行した証明書は使えない。
Terrraformでproviderでap-northeast-1を指定していると、ACMの証明書を発行したときもap-northeast-1で発行されてしまう。

下記のような方法で回避する。

- [terrafromでaws acm作成 cloudfrontの場合バージニアで作成しないといけないんだけどどうやるの？](https://qiita.com/tos-miyake/items/f0e5f28f2a69e4d39422)
- [TerraformでACMをバージニアで取得する](https://wiki.adachin.me/archives/830)

### CloudFrontの代替ドメインはオプショナルだが、独自ドメインを使う場合は設定が必要

CloudFrontの代替ドメインはオプショナルなので設定しなくてもディストリビューションを構築できる。代替ドメインとは、CloudFrontのドメインを使わずに独自ドメインでアクセスするためのドメインのこと。独自ドメインを使う場合に代替ドメインを設定せずにRoute53でエイリアスレコードを発行し、example.comのAレコードに*.cloudfront.netのドメインを指定すると、`ERR_SSL_PROTOCOL_ERROR`になる。これはよくあるエラー事例にも載っている。

[CloudFront の「ERR_SSL_PROTOCOL_ERROR」および「リクエストを満たせませんでした」というエラーを解決するにはどうすればよいですか？](https://repost.aws/ja/knowledge-center/cloudfront-ssl-connection-errors)

```terraform
    aliases = [
        "example.com"
    ]
```

## Terraform全体像

<script src="https://gist.github.com/mom0tomo/55ef18e34c36574e3d9f604d5af09b84.js"></script>
