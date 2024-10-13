---
title: "TinyGo Keeb Tour 2024(仙台)に行ってきた"
date: 2024-10-13T19:23:12+09:00
draft: false
tags: ["EVENT", "WOMEN WHO GO", "電子工作"]
images: ["images/articles/avatar.png"]
description: TinyGo Keeb Tour 2024 in 仙台 with Sendai.goに行ってきました。かなり久しぶりの電子工作＆少人数でのオフラインハンズオンイベントでとても楽しかったです。作ったキーボードは家に持ち帰り、プログラムを少し改良して自由に表示を変えて遊んでみました。
---

TinyGo Keeb Tour 2024 in 仙台 with Sendai.goに行ってきた。

<div class="iframely-embed"><div class="iframely-responsive" style="padding-bottom: 56.1881%; padding-top: 120px;"><a href="https://sendaigo.connpass.com/event/327533/" data-iframely-url="//cdn.iframe.ly/api/iframe?url=https%3A%2F%2Fsendaigo.connpass.com%2Fevent%2F327533%2F&key=6da8f492348dcf72ef42ec6631ea70ef"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>

会場は[enspaceさん](https://www.enspace.work/meeting-event/meeting/5b1/)。前回は[Go Conference mini 2022 Autumn IN SENDAI](https://sendaigo.connpass.com/event/256463/)でお世話になった。奇しくもGo Conference miniと開催時期もほぼ同じ。仙台には毎月行っているけれども、enspaceさんのある肴町公園あたりの路地はあまり歩くことがない。このエリアは老舗の喫茶店や服・雑貨屋などがある雰囲気のいい場所。

<div class="iframely-embed"><div class="iframely-responsive" style="height: 170px; padding-bottom: 0;"><a href="https://iko-yo.net/facilities/142181" data-iframely-url="//cdn.iframe.ly/api/iframe?url=https%3A%2F%2Fiko-yo.net%2Ffacilities%2F142181&key=6da8f492348dcf72ef42ec6631ea70ef"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>

肴町公園の角に学生時代にも行ったことがなかったコーナーハウスというすてきな喫茶店も見つけたので、今度時間がある時に立ち寄ってみたいと思った。

---

[最後に電子工作系のポストをしたのが2018年](http://localhost:1313/post/20180510/)なので、はんだづけは2018年ぶりかもしれない。今回のイベントはPC以外は手ぶらでOKで、スタッフの方が会場ですべて準備して貸してくれるので本当にありがたい。

会場に入って挨拶をしたところ、さっそく@sago35さんがTinyGoでぐるぐる光るピアスをプレゼントしてくれた。東京で開かれた前回のGo Conferenceで、わたしが基盤とスイッチのピアスをしていたところ、大層褒めてもらったのがきっかけ。派手に光ってとてもかっこいいピアスなので、いろいろな技術イベントにつけていきたい。

{{< tweet user="ysaito8015" id="1844945524228513919" >}}

お昼には@senoueさんによる山形風芋煮が振る舞われた。山形芋煮になくてはならない醤油、味マルジュウを使った本格派。enspaceさんの5FにはBBQができそうなくらい広いベランダがあり、みんなそこのすてきなチェアやベンチに座って芋煮を食べた。里芋もこんにゃくも牛肉も自分が家でつくるものよりずっといいもので、大鍋で作るので味も染みていて美味しく、とても楽しい時間だった。

{{< tweet user="mom0tomo" id="1844944511551910147" >}}

{{< tweet user="micchiebear" id="1844937065437462537" >}}

午前中はもくもくとはんだづけをがんばり（初っ端から基盤の面裏を間違え、@taknb2nchさんに苦労して吸い取ってもらった...）、午後も引き続きハードウェアの組み立てをして、15:00頃からようやくソフトウェアのビルドと書き込みをはじめた。10:00-18:00なので時間がたっぷりあると思っていたが、あっという間に時間が経った。ちなみにキーボードのキーはもともと透明な軸のものをもらっていたが、@micchiebearさんが青軸も持ってきてくれていたので青軸にしてみた。せっかくの自作キーボードなので、派手にペチペチ言わせてみたかったため。

{{< tweet user="mom0tomo" id="1844953869861913071" >}}

---

ハンズオンの終了後は、@ken5owataさん作のGopherくん基盤を巡るジャンケン大会が開かれ、争奪戦が繰り広げられた。

{{< tweet user="mom0tomo" id="1845019595901394996" >}}

イベントは知らない人にたくさん挨拶するので緊張して夜にはくたくたになっていることが多いが、Sendai.goのみなさんは顔見知りだったし、スタッフのみなさんにも友人知人がたくさんいたので、懇親会まで楽しめた。

またオンラインでつながりのあった@1019HiroyaさんやSendai.goのみなさん、@uji_rbさんとオフラインで交流できたのがとてもうれしかった。

@micchiebearさんと「今度はWomen Who Go TokyoでもGopherくん基盤を使った組み込みイベントをやりたいね」と話し、さっそく来年の計画が始まろうとしている。

{{< tweet user="micchiebear" id="1845018762350563375" >}}

---

翌日サンプルプログラムをもとに少し改良して、こんなものを作った。

{{< tweet user="mom0tomo" id="1845359705595789650" >}}

上の液晶のGopherくんのアニメーションと、下のキーボードのキーを押下したときに表示を変えるプログラムを同時に動かすのが地味に難しい。そして昨日@sago35さんにわかりやすく説明してもらたのだが、やっぱり「キーボードのキーを押下したときに何かする」というのは組んでみると難しかった。

かなり試行錯誤したが、思った通りのものがつくれたので満足した。

ただ、左端のキー3つがどうしても光らないので次回のイベントなどでどこがまずそうか聞いてみたい。

今回自分が実装したものはこちら。

<div class="iframely-embed"><div class="iframely-responsive" style="padding-bottom: 50%; padding-top: 120px;"><a href="https://github.com/mom0tomo/tinygo_keeb_workshop_2024/pull/1" data-iframely-url="//cdn.iframe.ly/api/iframe?url=https%3A%2F%2Fgithub.com%2Fmom0tomo%2Ftinygo_keeb_workshop_2024%2Fpull%2F1&key=6da8f492348dcf72ef42ec6631ea70ef"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>
