---
title: 『達人が教えるWebパフォーマンスチューニング』を読んだ（②シナリオを持った負荷試験）
date: 2023-06-10T09:29:20+09:00
draft: false
tags: ["Book", "SRE"]
images: ["images/articles/avatar.png"]
description: ""
---

## Chapter 4 シナリオを持った負荷試験　読書メモ

### パフォーマンステストとストレステスト

パフォーマンステストは短期間の負荷をかけて、システムの性能を測定すること。
ストレステストは長時間の負荷をかけて、システムの耐久性を測定すること。

本書ではパフォーマンステスト（性能テスト）を取り扱う。

この章については、著者の藤原さんのスライドが詳しくわかりやすい。

<div style="left: 0; width: 100%; height: 0; position: relative; padding-bottom: 56.1972%;"><iframe src="https://speakerdeck.com/player/c6db5916aeda4148a3b891afd4e5ec17" style="top: 0; left: 0; width: 100%; height: 100%; position: absolute; border: 0;" allowfullscreen scrolling="no"></iframe></div>

なお、実務ではパフォーマンステストに加えてストレステストも行い、メモリリークしていないか、データの増加に応じてパフォーマンスの問題が起きないかなどを観察する必要がある。

### 負荷試験で大切なこと

- ゴールを決める
  - レイテンシ/スループット/特定のサーバーリソースのxxの最大化 etc.
- 測りたいものを言語化する
  - レイテンシ/スループット/サーバーリソースの利用率・利用料/コスト etc.
- 負荷をかける対象のサーバーリソースは固定する
  - Auto Scalingなどは止めておく
  - できるだけ変数を減らす

事前準備
- サーバー側のメトリクスを本番同様に取得してダッシュボードを作る
  - CPU使用率
  - スループット/レイテンシ（HTTP statusコード別）
  - データベースへのクエリ数
  - アプリケーションサーバー固有の情報
  - ジョブキューのメトリクス
  - ネットワークトラフィック etc.

### k6を利用したシナリオテスト

k6はGo製の軽量なシナリオテスト用ツールで、Java Scriptでテストを記述できるのが特徴。同様のツールで有名なものにvegetaがあるが、より高度なシナリオテストができる。

シナリオテストとは

こんな感じで書いて実行できる。

initialize.js

```js
// k6のhttp処理のためのmoduleをimport
import http from "k6/http";

// k6のsleep関数をimport
import { sleep } from "k6";

// 独自に定義したurl関数をimport
import { url } from "./config.js";

// k6が実行する関数
// /initializeに１0sのタイムアウトを指定してGETリクエストし、完了後1s待機する
export default function () {
  http.get(url("/initialize"), {
    timeout: "10s",
  });
  sleep(1);
}
```

```bash
# --vusオプション: Virtual Users（実行の並列度）を指定する
$ k6 run --vus 1 comment.js
```
