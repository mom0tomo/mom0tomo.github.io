---
title: "S3に.envを配置しているコンテナで、.envに破壊的変更を加えている最中に新しいコンテナが起動するとどうなるのか調べた"
date: 2023-01-15T21:45:39+09:00
draft: false
tags: ["AWS", "ECS", "S3"]
images: ["images/articles/avatar.png"]
description: "本番環境で稼働するサービスで、.envに破壊的変更を加えるケースはあまりないと思うが、下記の場合どのような挙動になるのか気になって調べた。結論として、.envに加えた変更は、新しいタスク定義を作成したタイミングで読み込まれる。このため古いタスク定義のままコンテナ（タスク）を起動しても挙動は変わらない。"
---

本番環境で稼働するサービスの `.env` に破壊的変更を加えるケースはあまりないと思うが、下記の場合どのような挙動になるのか気になって調べた。

## 想定するケース

- .envを利用し環境変数を読み込むRailsアプリケーションを想定する。
  - ECSタスク定義のenvironmentFilesオプションを利用し、S3のバケットに `.env` ファイルを配置する。
- S3バケット内の `.env` ファイル内の環境変数に破壊的変更を加える。
- 新しい環境変数に対応したアプリケーションのタスク定義をデプロイする前に、古いタスク定義のままコンテナ（タスク）を起動する。
  - 具体的には、変更を加えている最中にオートスケールした場合を想定した。

## 結論

.envに加えた変更は、新しいタスク定義を作成したタイミングで読み込まれる。
このため古いタスク定義のままコンテナ（タスク）を起動しても挙動は変わらない。

[Amazon ECS タスクに環境変数を渡す際の問題をトラブルシューティングするにはどうすればよいですか?｜環境変数が自動的に更新されない](https://aws.amazon.com/jp/premiumsupport/knowledge-center/ecs-task-environment-variables/#%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0%E3%81%8C%E8%87%AA%E5%8B%95%E7%9A%84%E3%81%AB%E6%9B%B4%E6%96%B0%E3%81%95%E3%82%8C%E3%81%AA%E3%81%84)

## 調査前に予想した挙動

1. 起動中のコンテナが停止する。
2. 古いタスク定義を元に、コンテナ（タスク）が起動する。
3. 起動時に破壊的変更が加えられた `.env` ファイルを読み込む。
4. Railsのエラーが発生してコンテナ（タスク）が停止する。
5. コンテナ（タスク）の起動と失敗を繰り返す。
6. コンテナを1つだけデプロイする設定にしていた場合、ECSコンテナ上にデプロイしているアプリケーションが完全に停止する。

## 実際の挙動

1. 起動中のコンテナが停止する。
2. 古いタスク定義を元に、コンテナ（タスク）が起動する。
3. `.env` ファイルは、タスク定義を更新した際に読み込まれる。
4. ECSコンテナ上にデプロイしているアプリケーションの挙動は、`.env` ファイルに破壊的変更を加える前と変わらない。

S3に配置した `.env` ファイル内の環境変数が読み込まれるタイミングについてよく理解していなかったが、調べて理解が深まった。
