---
title: "【2023年12月】新しいMacのセットアップと入れたアプリ"
date: 2023-12-19T18:49:06+09:00
draft: false
tags: ["Mac"]
images: ["images/articles/avatar.png"]
description: ""
---

## 旧マシンから引き継ぐもの

会社から貸与されるPCが突然クラッシュしたことが複数回あったので、必要な設定のほとんどはクラウドで管理しています。Mustではないが古いPCから引き継げると便利なのは以下です。

### ~/.ssh/config

社内のサーバーにSSHするときの便利な設定などを記載しています。

### fish.config.local

fish.configはパブリックなdotfilesのリポジトリで管理しているので、秘密情報は別ファイルに分けています。

***

## dotfiles関係

gitで管理しており、install.shでシンボリックリンクを貼るだけ。

```
$ git clone https://github.com/mom0tomo/dotfiles.git
```

***

## Homebrew

なにはなくともHomebrew。

https://brew.sh/ja/

***

## Karabinerの設定

外付けの分割キーボードを使うようになってから、キーの変更は[Karabiner](https://karabiner-elements.pqrs.org/)で行っています。
設定ファイルはJsonでエクスポート・インポートできるので、dotilesリポジトリに入れてあります。

https://github.com/mom0tomo/dotfiles/tree/main/karabiner

## Screen Shotの場所を変更する

デスクトップがごちゃごちゃするのが嫌なので`~/sshot`という場所にスクリーンショットが収まるようにしています。

```
$ mkdir ~/sshot
```

Command + Shift + 5を推してスクリーンショットの保存先の設定を表示し、「オプション」から保存先を変更できます。

***

## Goのダウンロード

GOPATHの指定など難しいことはなくなったので、ダウンロードボタンをポチッとしてインストーラーを開くだけで完結するようになりました。

https://golang.org/dl/

***

## Homebrewで入れるツール

### fish shell

```
$ brew install fish
```

パッケージマネージャー[fisherman](https://github.com/fisherman/fisherman/wiki/%E6%97%A5%E6%9C%AC%E8%AA%9E)も併せてインストール。

### Git関係

git logの代わりに見やすい[tig](https://jonas.github.io/tig/)を使っています。

```
$ brew install tig
```

### ブログ用

このブログはGo製の静的サイトジェネレータ[Hugo](https://gohugo.io/)を使っています。

```
$ brew install hugo
```

## デスクトップアプリ類

### Slack

これがないと仕事も友達とのやりとりもできません。

[Slack](https://slack.com/intl/ja-jp/downloads/osx)

### Chrome

メインブラウザとして使っています。Safariはフロントエンドの検証時しか使いません。設定でもメインのブラウザをSafariから変更しておきます。

[Chrome](https://www.google.co.jp/chrome/browser/desktop/index.html?brand=CHBD&gclid=EAIaIQobChMI45X78s-m2AIVVR0rCh33TgUeEAAYASAAEgKUBfD_BwE)

### iterm2

慣れ親しんだターミナルです。

設定ファイルはdotfilesリポジトリで管理しています。

https://github.com/mom0tomo/dotfiles/tree/main/iterm2

### Notion

毎日の短い日記など、いろいろなメモを保存するのに使っています。仕事関連で打ったコマンドのメモ、大学の課題のメモや、プライベートで考えたことのメモなど。いまのところ無料枠で間に合っています。

https://www.notion.so/ja-jp/desktop

### Docker

コンテナ環境を手元でデバッグするときにはDocker Desktopを使っています。

https://www.docker.com/products/docker-desktop/

### Multipass

M1 MacでTerraformを実行できないので、Multipassを使ってUbuntuの仮想環境を作っています。

https://multipass.run/

### Kindle

最近、技術書は電子書籍で読むことが多いです。App Storeからインストールできます。

### Discord

WWGTの読書会や朝会はDiscordを使っています。

https://discord.com/download

### Around

1on1でAroundを使っています。Chromeなどの画面の上にフロート表示できるので便利です。

https://around.co/download

### Firefox

検証用ブラウザとして使っています。

## 仕事関係

以下はすべてApp Storeからインストールできます。

- Tailscale
  - VPN
- WireGuard
  - VPN
- Clockify
  - タイムトラッキングツール
- 1Password
  - パスワードマネージャー
- Postico
  - PostgreSQLクライアント
