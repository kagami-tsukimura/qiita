---
title: 【Redux】頭が痛くなるReduxの仕組みを完全に理解した（い）
tags:
  - Web
  - React
  - redux
private: false
updated_at: '2023-09-18T13:24:47+09:00'
id: afc20a25fcccf6aacfe4
organization_url_name: null
slide: false
---

# Introduction

本記事では Redux の概要を備忘録として残します。
Web 開発では React が好きなのですが、実務では使わず趣味程度のため、大規模開発向けと聞いていた Redux は敬遠していました。

ある程度 React には慣れたため props のバケツリレー解消目的で Redux を試すと、動くけど理解しきれていないものが出来上がりました。
意味を理解しながら実装できるように Redux の基礎を固めます。

:::note warn
警告
備忘録のため正確さよりも（私にとっての）理解しやすさを意識して記述しています。
:::

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu22.04

## Redux とは

状態管理のこと。
Redux で監視している現在の状態に応じて処理を変える。

### ex. 飲食店

<details><summary><b>=======================<br>&nbsp;&nbsp;&nbsp;飲食店でのRedux<br>&nbsp;========================</b></summary><div>

- 注文前
  - 客
    - 状態：メニューを悩んでいる。
  - 店員
    - 状態：何もしない。
- 注文
  - 客
    - 状態：店員を呼ぶ。
  - 店員
    - 状態：注文を聞きに行く。
- 注文後
  - 客
    - 状態：何もしない。
  - 店員
    - 状態：注文を伝える。

</div></details>

客の注文に応じて店員の処理を変える。
− 客が注文前か、注文するのか、注文後なのかを Redux で監視する。

### ex. X

<details><summary><b>=======================<br>&nbsp;&nbsp;&nbsp;XでのRedux<br>&nbsp;========================</b></summary><div>

- ログイン済
  - プロフィールを見せる。
- ログイン前
  - ログイン画面を見せる。

Redux でページの画面を変更する。

</div></details>

### React のみとの違い

- React：Component から子、孫、それ以下にバケツリレーして渡す。
  - 小規模でバケツリレーをあまりしなくて良い場合。
  - 保守性に欠ける。
- Redux：1 箇所で管理してどこの Component からでも呼び出せる。
  - 大規模でバケツリレーが必要な場合。
  - 保守性に優れる。

### Redux の仕組み

![Image](https://user-images.githubusercontent.com/113032853/268547801-bdb29500-9a8c-426d-a887-a6c3376fc13a.gif)

> Quoted from <https://redux.js.org/tutorials/essentials/part-1-overview-concepts>

<details><summary><b>=======================<br>&nbsp;&nbsp;&nbsp;Reduxによる処理の流れ<br>&nbsp;========================</b></summary><div>

1. State：状態

   - ex. $0

2. UI：Button

   - Deposit $10：ボタンを押して+$10 する。
     - Action 実行。

3. Dispatch：Action を Store に通知

   - Event Handler：Event(Deposit $10)を処理する。

4. Store：1 箇所で状態を管理し、どの Component でも使えるようにする。

   - Global に扱う。
   - Reducer：State（以前の状態） を Action（新しい状態） に更新。
     - Action と State を同時に取得する。
       - Action：Event(Deposit $10)）が Reducer に遷移する。
       - State：以前の状態（$0）が Reducer に遷移する。
     - R：Reducer 内のロジック
   - State：$0→$10 に更新。

</div></details>

### 最後に

最後まで閲覧頂きありがとうございました。

非常に基礎的な部分ではありますが、本記事にまとめた内容が実装する際の理解に役立ちました。
Redux に限らず少し複雑に感じる内容は以下の手順で進めることをおすすめします。

1. 軽く実装を見てみる。
2. 公式ページで言葉の定義を知る。
3. 実装してみる。
4. 理解できなかった箇所を公式ページで理解する。
5. 再度実装してみる。

成果に目を向けると精神的に苦しくなるので、成長に目を向けて 1 つずつ理解していきましょう！

本記事がお役に立てば幸いです！

### 参考 URL

https://redux.js.org/tutorials/essentials/part-1-overview-concepts
