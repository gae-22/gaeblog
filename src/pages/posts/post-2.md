---
layout: ../../layouts/MarkdownPostLayout.astro
title: 'entersys作成記'
pubDate: Jun 23 2023
description: 'This is a record of the creation of "entersys"'
author: 'gae'
---

# entersys

[`entersys`](https://gitlab.mma.club.uec.ac.jp/gae/entersys)(部員専用)とは MMA の部室入退室管理システムのことである．

今回はその大まかな流れを記す．詳細は [gitlab](https://gitlab.mma.club.uec.ac.jp/gae/entersys) を参照．

![実験段階の画面](./../../../public/post-2/post-2.png '実験段階の様子')

## 作成の経緯

MMA では部室の入退室管理を Slack で行っている．コロナ禍になり，部室に誰がいたのかを把握しようという流れになったそう．入学前なので知らない．

`entersys`作成以前は，Slack に自分で`入室` `退室`を投稿する必要があった．でもわざわざ Slack を開いて投稿するのは面倒だし，忘れることもある．

そこで，学生証のバーコードを利用して楽に入退室の管理ができないかと考えた結果，思いついたのが`entersys`だった．

## 作成の流れ

### 1. 計画

そういったアルゴリズム(?)を考えるのは初めてだったので，まずは計画を立てた．

-   どのような機能を実装するか
-   言語は何にしようか
-   どこで動かそうか

など，初心者としては初めての部で運用するシステムだったため，考えることが多かった．

### 2. 実装

実装は以下の通り`python`にて行った．大まかに分けると以下のようになる

-   読み取った学籍番号をデータと照会して MMAid を特定する
-   特定した MMAid と現時点の入室者を照らし合わせ，入室か退室かを判断する
-   入室か退室かを判断した結果を Slack に投稿する

特に大変だったのは MMAid を特定する部分だった．学籍番号を読み取った後，学籍番号と MMAid の対応表を作成しておく必要があった．その対応表も適宜更新する必要があり，初心者には大変だった．`python`は多くのライブラリが用意されているためどうにか作成することができた．

ただ，終わってみると最終的にはかなりシンプルにできたのではないだろうか．毎日対応表を自動更新したり，Slack の表示をいじったりと，使いやすく見やすい管理システムができたのではないかと思っている．単純な動作部分だけで言えばかなり簡単にできたので初心者の学習にはちょうどいいかもしれない．自動更新や Slack の表示をいじる部分は初心者には難しいかもしれない．

今回はこれで終わりにする．書き慣れておらず，文章が下手なので読みにくいと思うが，読んでくれてありがとうございました．

また次回．