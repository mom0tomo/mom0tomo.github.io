---
title: Hugoのブログテーマを更新した（cactus-plusをminiに更新）
date: 2023-05-28T16:59:20+09:00
draft: false
tags: ["Hugo", "Blog"]
images: ["images/articles/avatar.png"]
description: "Hugoでgenerateしている、このブログのテーマを更新しました。今まで使っていたcactus-plusがminiというテーマに変わったので、対応しました。"
---

Hugoでgenerateしている、このブログのテーマを更新しました。今まで使っていたcactus-plusがminiというテーマに変わったので、対応しました。

利用しているテーマはこちらです。

<div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://github.com/nodejh/hugo-theme-mini" data-iframely-url="//cdn.iframe.ly/api/iframe?card=small&url=https%3A%2F%2Fgithub.com%2Fnodejh%2Fhugo-theme-mini&key=6da8f492348dcf72ef42ec6631ea70ef"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>

数年前にGitHubのリポジトリごとrenameされたことは知っていたのですが、自分でテーマのデザインを上書きして使っていたので、腰を入れて対応する必要があると思いずっと先延ばしにしていました。

Hugoを触り始めてすぐの頃だったので、よくわからないままにテーマの上書きをしたところがあり、修正すべき点が複雑になっていました。

しかし、何かのきっかけでネストした箇条書きがうまく表現されなくなったことと、コードブロックの表示でシンタックスハイライトが効かなくなったことがずっと気になっており、今回一念発起して移行しました。移行にかかった時間は、合計して1日ほどでした。

対応した内容は下記です。

- 旧テーマを上書きしてデザインを変更している部分を洗い出す
  - ナビゲーションバー、footerなどのデザイン変更
  - ソーシャルシェア用のリンクを配置したパーツ、前後の記事を表示するパーツを追加
  - Mono Social Iconsフォントの利用をやめて、必要なものだけSVGファイルにする
- 旧テーマを使っていた際にcommitしていたファイルのうち、結局上書きに使っていなかったファイルを削除する
- 新テーマの設定項目に合わせてconfig.tomlを整理する
- 新テーマの気に入っていないデザインを上書きする

## 見た目のbefore/after

同じ作者の方の作成したテーマなので、パッとみた時の印象はあまり変わりませんが、比較してみるとけっこう違います。

| before                                                             | after                                                            |
| ------------------------------------------------------------------ | ---------------------------------------------------------------- |
| ![before top](/images/articles/update_hugo_theme_before_top.png)   | ![after top](/images/articles/update_hugo_theme_after_top.png)   |
| ![before post](/images/articles/update_hugo_theme_before_post.png) | ![after post](/images/articles/update_hugo_theme_after_post.png) |

数年間、ビルドする際に自分のリポジトリにフォークしたテーマを使ってしのいでいたので、懸案事項が解決してうれしいです。

また、全体に細すぎる時や小さすぎる文字が減り、ネストした箇条書きやコードのシンタックスハイライトも正常に動作するようになったので、読みやすくなったと思います。
