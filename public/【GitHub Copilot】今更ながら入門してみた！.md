---
title: 【GitHub Copilot】今更ながら入門してみた！
tags:
  - Python
  - GitHub
  - ChatGPT
  - 記事投稿キャンペーン_ChatGPT
  - 新人プログラマ応援_記事投稿キャンペーン
private: false
updated_at: '2023-09-05T22:34:39+09:00'
id: ba8c19bb34fd6a5a9642
organization_url_name: null
slide: false
ignorePublish: false
---

# GitHub Copilot とは

IT エンジニアは勿論、世間でも話題の`ChatGPT`と`GitHub`が共同開発した
AI 駆動のコーディングアシスタント(ざっくり書くと GPT ベースの AI ペアプロ)ツールです。

# Introduction

| 項番 | ページ内リンク                                    |
| :--: | :------------------------------------------------ |
|  1   | [導入](#導入)                                     |
|  2   | [機能](#機能)                                     |
|  3   | [お試し](#お試し)                                 |
|  4   | [自動化されたコード生成](#自動化されたコード生成) |
|  5   | [コード補完](#コード補完)                         |
|  6   | [Tabnine](#tabnine)                               |
|  7   | [注意](#注意)                                     |
|  8   | [最後に](#最後に)                                 |

`GitHub Copilot`は現在 30 日間フリートライアルを行っています。
(以前は 60 日だったような......)

ずっと気になっていたので、GW 中に trial で使い倒してみようと思い入門してみます。

結論を先に書くと、**素晴らしいツール！ だけど新人/駆け出しで扱う場合は注意が必要** といった感想でした。
まだお試ししていない方は、本記事で私と一緒に trial 登録していきましょう！

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu22.04
Python3.11.1

## 事前準備

- GitHub アカウントの作成。
  - まだの方は[<u>「GitHub に Sign Up」</u>](https://github.com/)

## 導入

以下の手順で`GitHub Copilot`に登録します。

1. [<u>「GitHub Copilot」</u>](https://github.com/features/copilot)にアクセスして`Start my free trial`をクリックします。
   ![1.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/098ae566-4530-b18f-27da-b51da63a7e22.png)
1. `Get access to GitHub Copilot`をクリックします。(アイコン可愛い......)
   ![2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/7fa2fac6-522d-75f7-a7ff-2ceb7453ef84.png)
1. 個人情報を入力していきます。trial 終了日付を確認しておきましょう。
   支払情報の登録も必要です。（クレジットカード/デビットカード or PayPal）
   ![3.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/87306e6d-84bb-018c-b558-5e9ff87aa4e0.png)
1. 素敵な演出と共に登録完了です。
   `GitHub`は 404 エラーのページなど可愛いものが多くて癒しになります。
   ![4.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/4eccf50d-c926-1b3b-0205-5609e91a151f.png)
1. `VSCode`拡張で連携できるので`GitHub Copilot`の拡張機能をインストールします。
   `GitHub Copilot`（安定版）と`GitHub Copilot Nightly`（最新版）とがありますが、今回は後者を試してみます。
   ![5.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/ac41d98d-a49b-d6ea-a738-1d4944781282.png)
   以上で`GitHub Copilot`の導入完了です。

## 機能

本記事では詳しく触れませんが、サービス一覧や導入後にやれることは以下の記事に詳しくまとまっていたので紹介します。

https://qiita.com/masakinihirota/items/0e58a6b921e4420a2882

## お試し

以下の 2 機能を試してみます。

- 自動化されたコード生成
  - コメントで機能の説明を入力し、適切なコードを生成する。
- コード補完
  - 入力したコードに基づいて次に書くべきコードを提案する。

コードは`Python`で書いていきます。

## 自動化されたコード生成

画像合成の処理を書きたいので、そのままコメント入力します。
![6.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/b114ecdf-bf18-a1da-c285-dca9ca1d7e6c.png)
`GitHub Copilot`の提案はこちら。こんないい加減なコメントでも書けちゃいました。
![7.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/60a39e6c-5d22-868b-ea86-ad5930a95e5a.png)
提案されたコードをパッと見た限り悪くはなさそうです。
ただ`Pillow`で書かれているので`OpenCV`にしたいです。
`OpenCV`の方が機械学習やっている私には馴染み深く、`ndarray`なので読込以外の速度が早いです。
![8.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/a42ac93a-08fb-bbd3-854e-3af51b08bed2.png)
引数で座標も指定したかったのでコメントに加えました。
`GitHub Copilot`の提案はこちら。
![9.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/31170867-d7a7-15b0-6e91-e62cc7818999.png)
好みに近いコードに変わってくれました！

座標指定(`x, y`)もありますね。
あと個人的には`docstring`書いてくれる点がとても助かります。
余裕ない時の`docstring`程、手間のかかるものはないですからね......

`arguments`も試してみました。
![12.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/6a770e3d-495a-f2bc-f281-07292726253f.png)
`GitHub Copilot`の提案はこちら。
![13.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/c9c12214-4679-10d3-1b2a-5922de33f658.png)
`args`の定義まで`Tab`キー押すだけで作ってくれます。
コメントが雑な影響で引数の命名が気に入りませんが、軽く直すだけで完結しそうです。

## コード補完

import で試してみました。
![10.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/dfa116be-c9cc-5d73-80b6-7bc920e596a4.png)
![11.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/57f81344-753f-3320-d9a6-76a15d1d11cc.png)
こちらは分かりやすく便利ですね。

`Python`ってライブラリ豊富なことが利点の一つですが、偶にある import 忘れや Typo を防げそうです。

## Tabnine

`GitHub Copilot`と、似たようなツールとして一応無料でも使える`Tabnine`があります。

https://www.tabnine.com/

`Tabnine`は`Tab`キーでコード補完してくれるツールです。
私も`VSCode`拡張で普段から使っており、以下 3 点が便利に感じます。

- 型定義面倒な時に便利。
- うろ覚えな内容を検索せずともサジェストしてくれて便利。
- Typo 防げて便利。

`TypeScript`書く時は割と助かっていますが、`Python`では Typo 対策くらいですかね。
練度なのか言語特性なのか定かではありませんが、少なくとも無料版では便利な拡張機能の一つ。といった印象です。

`Tabnine`は無料版を使っているため恐縮ですが、`GitHub Copilot`と比較すると精度が段違いで後者の方が良いですね。

trial 終了後は`Tabnine`に戻る想定でいましたが、使用感次第では有料で継続も見えてきそうです。

## 注意

使用感は文句無しですが良い点ばかりではありません。

本記事の冒頭で以下のように書きました。

> **素晴らしいツール！ だけど新人/駆け出しで扱う場合は注意が必要**

1. 提案されたコードの修正
   `GitHub Copilot`が提案したから必ず正しい！ ではないです。
   動作の検証や要件に合った修正、ハイパフォーマンスにするための改善等は必要です。
1. 著作権/ライセンス問題
   `GitHub Copilot`が提案するコードは、他のプロジェクトから取得されたものである可能性があります。
1. 開発力/問題解決力の低下
   `GitHub Copilot`は個人開発や簡単なスクリプトであれば作成できてしまうと思います。
   趣味で遊ぶには良いかも知れません。
   ただ、IT エンジニアとして必要なスキルは`GitHub Copilot`に依存することでも提案のためのコメントを適切に書くことでもありません。
   コード提案後に内部処理やパフォーマンスを理解し、より良いコーディングを学ぶことが必須になってきます。

簡単なコーディングにかける時間を`GitHub Copilot`で短縮し、空いた時間を以下のこと等に使えれば良いと思います。

- 公式ドキュメント等による具体的な処理の理解。
- より多くのコーディング。
- 趣味のお時間。

## 最後に

良くも悪くも`GitHub Copilot`は強力なツールであり、優秀なペアプログラマーでした。

想像以上に便利だったため、若干落ち着いてきていたモチベーションアップに繋がりました！
今後の継続を見極めるためにも、暫くはコーディングに励んでいきたいです。

最後まで閲覧頂きありがとうございました。

本記事がお役に立てば幸いです！
