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

本書ではパフォーマンステストを取り扱う。

実務ではパフォーマンステストに加えてストレステストも行い、メモリリークしていないか、データの増加に応じてパフォーマンスの問題が起きないかなどを観察する。

### k6を利用したシナリオテスト

```
# URLを生成する関数を作成
config.js
const BASE_URL = "http://localhost";

export function url(path) {
  return `${BASE_URL}${path}`;
}

# Webサービスの初期化シナリオ


```
