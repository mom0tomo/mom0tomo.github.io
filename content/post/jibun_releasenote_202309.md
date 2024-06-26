---
title: "じぶんリリースノート(202309)"
date: 2023-10-01T13:36:39+09:00
draft: false
tags: ["じぶんリリースノート"]
images: ["images/articles/avatar.png"]
description: "じぶんリリースノート(202309)"
---

更新内容は以下のとおりです。

## 本

### 読みおわった

今月読み終わった本はありませんでした。

### 途中

- [Web API: The Good Parts](https://bookmeter.com/books/8983711)
- [達人が教えるWebパフォーマンスチューニング 〜ISUCONから学ぶ高速化の実践](https://bookmeter.com/books/19792437)
- [Go言語プログラミングエッセンス](https://bookmeter.com/books/20712629)

## ブログ

9月はブログ記事を書きませんでした。

## イベント

- 3連休に東京に行きました
  - 中央線沿いの先輩たちを誘って飲みに行きました
  - 銀座のヴァニラ画廊で[井上佐藤原画展「10DANCE」](https://www.vanilla-gallery.com/archives/2023/20230907ab.html)を見てきました
  - 友達と銀座松屋のシュウ・ウエムラのブースに行き、はじめてタッチアップしてもらいました
  - オフライン開催のカンファレンスや技術イベントも増えてきたので、東京に行く頻度を増やそうと思います

## 今月の歩数

東京で歩いたときに靴が痛んでいることに気づいたので、ランニングもできる新しいシューズを買いました。
いいソールの靴は足に負担がかからないので走りやすく、最近は散歩に出る日に1-3kmほど走っています。

- 平均: 6325歩/日

![Pixela](https://pixe.la/v1/users/mom0tomo/graphs/pedometer)

- 散歩中に出会ったねこの数

![Pixela](https://pixe.la/v1/users/mom0tomo/graphs/neko)

## この期間のふりかえり（KPT形式）

### Keep

#### 専門スキル

- Sad Serversの前半問題を解いている（2周目）。はじめて解いたときよりも理解が進んでいる
- SRE NEXTに参加し刺激を受けた
- Datadogの1o1ワークショップに参加した

#### 仕事

- 半年前から取り組んでいるシステムのリリースに向けて動いている
  - 検証も手慣れてきて、元々の実装者である上司と一緒に作業したりアドバイスをもらえた
- SRE NEXTやDatadog 101ワークショップでの知見を同僚に展開できた
- たくさんの問い合わせや相談を受けており、頼りにされている実感がある

#### 個人プロジェクト/コミュニティ活動

- 読書会でスタッフがファシリテーターをできない日があったが、参加者の方がファシリテーターをやってくれた
  - とても助かった。参加者の方がファシリテーターをやってくれるのははじめてだった。緊張感もありいつもよりも活発に議論ができた
- WWGT向けにGoのブートキャンプ（3日間）を開催してもらう話が動いている
  - 平日に休みを取って参加する予定。みっちり叩き込みたい

#### 交友

- 結婚式を挙げる友達にプレゼントを渡して喜んでもらえた
- 仙台で友達に会った。出産以来2人だけで会うことがなかったので、話が弾んだ

#### 健康

- ランニング用に新しい靴を買って、走り始めた
- 月初に酷く肩を痛めたので肩の筋トレを始めた
- 肌の調子を良くするためのスキンケア用品にお金をかけはじめた
  - パックしたり、PolaのBAミルクフォームを購入して使ったりしている
- 飲みすぎそうな飲み会の前後にミラグレーンを飲んだら効果絶大だった

#### 生活

- 3連休に思い立って弾丸で東京に行った
  - もともとかなり慎重なタイプなのだが、最近は「今やりたいと思ったことはやろう」という精神になってきている
- 「[世界で一番美しい少年](https://gaga.ne.jp/most-beautiful-boy/)」を見た
  - 「ベニスに死す」での役は完璧だったと思っているが、1人の人の人生が大人によってめちゃくちゃにされたのを見るのは辛かった
  - 本人がユーモアのある人であったのが救い
  - 日本に長期滞在しアイドル活動をしたことも大きく取り上げられていて、時勢的にもいろいろ思うところがあった
  - 毎日会社に行くのに通った目黒川、桜並木が大きく出ていたので驚いた
- 同僚からキューバのバンド「Buena Vista Social Club」を教えてもらった
- 友達に勧められてタランティーノの「Death Proof」を見た
- 随分前から全部見たいと思っていた「Sex and the City」を最新のエピソードまですべて見終わった
- [小田切ヒロさんのメイク動画](https://www.youtube.com/@hirobeautychannel)にハマり、メイクの練習をしている
- めったにYouTubeを見ないのに「[Kevin's English Room](https://www.youtube.com/@KevinsEnglishRoom)」にハマって毎日のように見ていた

### Problem

#### 専門スキル

- まったく違うアーキテクチャのチームの担当になりそうだが、アーキテクチャなど何も把握できていない
  - ものにできれば大きなステップアップになるが、今は不安しかない
- 本を読む時間も、ブログを書く時間も取れなかったが、何が原因で1か月があっという間だったのかがわからない
- 問い合わせやインシデント対応など、その都度出てくる問題に対応するのは楽しいが、自分から何かを考えて動くことができているかというと疑問がある
  - SRE的にいうとトイルに足を取られている状態だと感じる
  - トイルの総量も価値ある仕事にかけられた時間の総量も数値的にふりかえることができていない
  
#### 仕事

- SREとして、所属するチームの方向性がわからない
  - 個人レベルではうまく動けているが、チームのレベルで大きな目標や目指すものが感じられていない
  - 目標設定や評価のタイミングで悩むことが増え、時間がかかっている
- 【前回のProblem】複業先でやりたい・やるべき仕事が山積みになっていて手が回らなくなってきた
- 【前回のProblem】9月中のリリースを目指していたPJTについて、さらに遅延することになった
  - 自分が主因ではないので仕方がないが、後ろ倒しになると他の作業にも影響が出るので工夫して他の仕事も進めたい

#### 個人プロジェクト/コミュニティ活動

- 問題なし

#### 健康

- 月初に酷く肩を痛め、整形外科でレントゲンを撮った
  - かなりのなで肩でストレートネックだということがわかった。元々首が長い方だが、このせいでさらに首が長く見えるらしい
- 家族がコロナに感染した
  - 判明後3日間は乗り切ったが、自分も感染した
  - 抵抗力があるのか熱は1日だけ38度出た程度だったが、喉の痛みが酷い
  - 他の体調不良も重なり、土日が潰れた
- 隔離のため仕事部屋で寝ており、体に負荷がかかっている

#### 交友

- 東京でのイベント開催が増えてきたので、もっとフットワーク軽く東京に行きたい
- コロナに感染したため親族の結婚式を欠席した

#### 生活

- 諸事情で1か月くらい実質1人暮らしをしており、会話の筋力が落ちている
- 迷う時間が無駄だと思い、必要だと思うものをバンバンネットで購入している。大した金額ではないが総額にすると消費が増えている
- 普段は「これを見たい」というものをじっくり見るスタイルだが、今月はなんとなくいろいろな作品を見たり聞いたりしている
  - これはこれで悪くないが、ここに書くほどの印象に残るものはあまりなかった
- 全体的に「消費」に傾いた月だと感じた。大量消費的なスタイルは好きではないので、来月はもう少しクリエイティブにしたい

### Try

#### 短期

- 【前回のTry】Web API the Good Partsを読み終える
  - できればWeb配信の技術も読む
- 【前回のTry】Sad Serversを解き終える
- 【前回のTry】肩の筋トレを続ける

#### 長期

- 監視に強くなる
- 目標を数値で具体的につくる
- 肩こりと腰痛を防ぐために筋肉をつける
- Kubernetesを運用できると言えるレベルになる（Want）

年末まで残り実質2.5か月くらいになってきたので、スピードアップして仕事を進めたいです。また、できるだけ早く完全な形で感染症から回復したいです。
