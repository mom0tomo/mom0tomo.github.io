---
title: "Oura Ringの歩数データをGitHub ActionsでPixelaに記録する"
date: 2022-10-10T18:47:39+09:00
draft: false
tags: ["Pixela", "GitHub Actions", "Oura Ring"]
images: ["images/articles/avatar.png"]
description: "「じぶんリリースノート」にPixelaで歩数を表示したくなったので、Oura Ringから歩数データを取得してGitHub Actionsで自動で記録するようにしました。"
---

在宅ワーク&山形在住になって始めた散歩が2年間続いています。最近a-knowさんに倣って「じぶんリリースノート」をつけ始めたので、そこに歩数の草を生やしてみたくなりました。
やってみた結果がこちらです。

![Pixela|歩数グラフ](https://pixe.la/v1/users/mom0tomo/graphs/pedometer)

### やったこと

[Mitsuyuki.ShiibaさんがCircleCIとFitbitでやっていたの]((https://bufferings.hatenablog.com/entry/2022/02/27/150342))とだいたい同じイメージです。

1. Oura RingのAPIで歩数を取得する
2. Pixelaに記録する
3. GitHub Actionsで定期的に実行する

前にこの記事を読んで歩数をPixelaにつけるのいいな〜と思っていたのでまねしました。仕事で普段GitHub Actionsを利用しているので、GitHub Actionsでやってみました。

## 1. Oura Ring APIを利用する

[Oura API v2](https://cloud.ouraring.com/v2/docs)を利用してデータを取得しました。
ちなみにgen 3からはサブスクリプションが導入され、サプスクリプションプランに入らないとAPIが利用できません。これがgen 3への乗り換えをためらっている理由の1つです。

まず、ログインして下記のURLにアクセスし、Personal Access Tokenを生成します。

[Personal Access Tokens](https://cloud.ouraring.com/personal-access-tokens)

あとはドキュメント通りに、このTokenをリクエストヘッダに付与してAPIを叩くだけです。今回は歩数のみ取り出したいのでjqで加工します。

```:bash
$ curl --location --request GET \
          "https://api.ouraring.com/v2/usercollection/daily_activity" \
          --header "Authorization: Bearer {Oura Personal Access Token}" | jq ".data[].steps"
```

[Oura API | Daily Activity](https://cloud.ouraring.com/v2/docs#tag/Daily-Activity)

GitHub Actionsを利用する場合、jqはプリインストールされているので何もしなくてもそのまま使えます。

## 2. piでPixelaに記録する

Pixelaに記録するのにはPixela公式のCLIツール[pi](https://github.com/a-know/pi)を使いました。

最初にユーザー登録とPIXELA_USER_TOKENの設定、グラフの作成を済ませます。piの使い方については[@a-knowさんによるとてもわかりやすい説明](https://blog.a-know.me/entry/2019/02/24/214142)があります。



PIXELA_USER_TOKEN、を先ほどと同じくGitHubのSettings > Secrets > Actionsに設定しておきます。

piを利用すると、↓のように簡単にPixelaへ記録できます。

```:bash
$ export PIXELA_USER_TOKEN={生成したトークン}
$ pi pixel post -u {ユーザー名} -g {登録したグラフの名前} -d {記録したい日付（yyyy-mm-dd形式）} -q {記録したいデータ（intかfloat）}
```

## 3. GitHub　Actionsで定期実行する

GitHub Actionsではこんな感じにcron形式で定期実行できます。

```:bash
on:
  schedule:
    - cron: 0 22 * * *
```

毎日1回23:55に、当日の歩数データを取得するようにしています。

GitHub Actionsのymlの全体はこちらです。

https://github.com/mom0tomo/mom0tomo.github.io/blob/main/.github/workflows/put_steps_to_pixela.yml

### 3のおまけ:Slackに結果を通知する

ついでに[rtCamp/action-slack-notify@v2](https://github.com/rtCamp/action-slack-notify)を利用して、ふだん使っているSlackへ通知するようにしました。

先ほどと同じく、あらかじめSlackのWebHook URLをGitHubのSettings > Secrets > Actionsに設定しておきます。

[Slack での Incoming Webhook の利用](https://slack.com/intl/ja-jp/help/articles/115005265063-Slack-%E3%81%A7%E3%81%AE-Incoming-Webhook-%E3%81%AE%E5%88%A9%E7%94%A8#incoming-webhook-u12398u35373u23450)

```:bash
- name: Notify Slack on Success
  uses: rtCamp/action-slack-notify@v2
  env:
    SLACK_CHANNEL: xxx
    SLACK_COLOR: ${{ job.status }}
    SLACK_MESSAGE: "Pixelaに歩数を記録しました :footprints:"
    SLACK_ICON_EMOJI: ":footprints:"
    SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK_URL }}
```

rtCamp/action-slack-notify@v2はデフォルトで成功した場合には通知するようになっています。そのままだと失敗した場合は通知が来ないので、失敗した場合も通知するように処理を追加しました。

```:bash
- name: Notify Slack on Failure
  if: failure()
  uses: rtCamp/action-slack-notify@v2.0.0
  env:
    SLACK_CHANNEL: xxx
    SLACK_COLOR: ${{ job.status }}
    SLACK_MESSAGE: "歩数の記録に失敗しました"
    SLACK_ICON_EMOJI: ":rotating_light:"
    SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK_URL }}
```

## おわりに

歩数に応じて草が生えるのはけっこううれしいです。Oura RingのAPIを使うといろいろな情報が取れるので、睡眠データなどでもっと遊んでみたいと思いました。
