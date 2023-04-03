---
title: "Cloudflare Imagesとソーシングキットを試してみる"
date: 2023-04-02T13:47:39+09:00
draft: false
tags: ["Cloudflare"]
images: ["images/articles/avatar.png"]
description: "CDNを使って画像を配信したいという場合、S3からCloudflare Imagesへの移行が簡単そうだったので試してみました。料金体系がシンプルで、ソーシングキットでS3からまとめて画像をCloudflare Imagesにインポートすることも可能であるため使いやすい印象です。ソーシングキットを使うとIAMユーザーのアクセスキーとシークレットを使ってS3から画像をインポートすることができます。また画像はAPI経由で追加・削除が可能でした。"
---

## はじめに
CDNを使って画像を配信したいという場合、S3から[Cloudflare Images](https://developers.cloudflare.com/images/cloudflare-images/)への移行が簡単そうだったので試してみました。

## 保管とコスト

料金体系はシンプルです。

- 画像の保管は10万点まで$5/月
- 画像の読み出しは10万点まで$1/月

画像の保管について、20種類の画像処理ができますがオリジナルの一枚のみが課金対象です。

## Cloudflare imagesのソーシングキットを使ってみる

ソース（S3など）からまとめて画像をCloudflare Imagesにインポートすることが可能です。この機能はSourcing kit（ソーシングキット）という名前が付いています。

ソーシングキットを使うにあたっては、AWS IAMユーザーを作成し、アクセスキーとシークレットを発行する必要があります。
権限的にはIAMロールでもできそうですが、ソーシングキットはIAMユーザーのアクセスキーとシークレットを必要とするのでIAMユーザーを作成します。

S3 bucketのリスト権限とバケットの中身のGet権限を付与します。

参考: https://developers.cloudflare.com/images/cloudflare-images/sourcing-kit/credentials/

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:Get*",
                "s3:List*"
            ],
            "Resource": [
                "arn:aws:s3:::<BUCKET_NAME>",
                "arn:aws:s3:::<BUCKET_NAME>/*"
            ]
        }
    ]
}
```

ソース名を決め、bucket名とアクセスキー、シークレットを画面に入力してボタンを押していくとimportが開始します。
画面上では資格情報の「アクセスキーID」となっている部分にアクセスキーを、「アクセスキー」となっている部分にシークレットを入力します。

![ソーシングキットの画面](/images/articles/sourcing_kit.png)

Cloudflare imagesには、S3 bucketのようなフォルダ構造がありません。
S3 bucket内のフォルダのパスは、そのままimage IDになります。

例: S3 bucketのフォルダ構造
![S3 bucketのフォルダ構造](/images/articles/s3_path.png)

例: Cloudflare imagesの管理画面とimage ID
![Cloudflare imagesのimage ID](/images/articles/image_id.png)

S3をバックエンドとしてリアルタイムに同期する機能などはありません。手動で1回限りのインポートができるだけです。
S3等からストレージを完全にCloudflare Imagesに移行するユーザーをターゲットにしている機能でした。

これを踏まえ、S3側でファイル名そのまま画像を書き換えるとどうなるか試しました。

S3側でファイルを上書きしても、リアルタイムに同期しているわけではないのでCloudflare Images側ではとくに変化はありませんでした。

手動で再度ソーシングキットからインポートすると、「Overwrite images?」のチェックボックスにチェックを入れた場合は画像が上書きされました。
キャッシュされているため、画像のプレビューはうまく機能しませんでしたが（変更前の画像のサムネイルが表示される）、ダウンロードしてみたところ画像が書き変わっていました。

### ソースの削除について

ソースを削除する機能があったので、どのような動作をするのか試しました。

結論として、ソースを削除してもイメージはCloudflare Imagesに保管されたまま残りました。
「ソースの削除は永久的であり、元に戻すことはできません。また、Cloudflareと外部ソースとの接続も削除されます。」という警告の通り、S3との接続を解除する機能のようです。ソースを削除すると、先ほどソーシングキットから設定を行ったS3 bucketがソースの一覧から消えました。

画像自体を消したい場合は、画面から1つずつ消すから、シェルスクリプトなどを書いてAPI経由で消す必要があります。

## 画像のURLについて

Cloudflare Imagesで発行される画像のURLは `https://imagedelivery.net/<アカウントハッシュ>/<image_id>/<variant_name>`　の形式になります。

アカウントハッシュは管理画面から確認できます。
ソーシングキットを使ってS3から画像をインポートした場合、S3 bucket内のディレクトリのパスがimage IDになります。

### Variantについて

Variantオリジナル画像から作成された異なるサイズ、解像度、形式などの派生画像と、その指定方法を定義したものの両方を指します。
URLのVariantは、指定方法定義したもののことです。Variantには任意の名前をつけることが可能です。

publicというvariantがデフォルトで用意されていました。

![Public Variant](/images/articles/public_variant.png)

また「フレキシブルバリアント」の機能を有効化すると、任意のVariantを指定したURLにアクセスすることで動的に画像を最適化することができました。

![フレキシブルバリアント](/images/articles/variant.png)

## 画像をAPI経由でアップロードする

API経由で画像をアップロードすることもできます。

```
curl -X POST -F file=@<ファイルのパス> \
 -H "Authorization: Bearer <api_token>" \
	https://api.cloudflare.com/client/v4/accounts/<アカウントID>/images/v1 \
-F 'id=<image_id(画像のパス）>
```

アカウントIDはアカウントハッシュと同じく管理画面から確認できます。

'id=<image_id(画像のパス）>' のオプションは省略できますが、S3 bucketからインポートした画像とimage IDの形式を揃えたい場合は利用することになります。

たとえば下記のように指定することでimage IDを任意のものに設定できます。

```
curl -X POST -F file=@test.jpg \
 -H "Authorization: Bearer <api_token>" \
	https://api.cloudflare.com/client/v4/accounts/<アカウントID>/images/v1 \
-F 'id=room/images/test.jpeg'
```

## おわりに

思ったより簡単にS3からの移行ができそうでした。

ただ、ディレクトリのような概念がないので、1つのCloudflareアカウントで複数のS3 bucketの画像を管理する際はコツが要りそうです。
たとえばdevelopment/staging/productionのS3 bucketの画像を管理する場合、image IDを `development/room/images/test.jpeg` と `staging/room/images/test.jpeg` のように分けて、それぞれのパスごとにアクセス権を設定する形になるのかなと思います。

このあたりは改めて試してみたいです。
