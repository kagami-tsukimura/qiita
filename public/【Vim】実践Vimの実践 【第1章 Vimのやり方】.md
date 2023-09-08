---
title: 【Vim】実践Vimの実践 【第1章 Vimのやり方】
tags:
  - Vim
  - Linux
  - Ubuntu
  - 新人プログラマ応援
private: false
updated_at: '2023-09-09T08:16:14+09:00'
id: 623a56f52d10660f72ec
organization_url_name: null
slide: false
---

# Introduction

以下の記事で`Vim`の基礎をご紹介しました。

https://qiita.com/kagami_t/items/67f2183e8d1120ca9eff

https://qiita.com/kagami_t/items/bfa5964816ee85593f66

その中でも触れた`実践Vim`をまとめます。

対象者はチュートリアル完了後、実践的な操作に挑戦したい方です。
そのため`<CR>`は`Enter`で表現するなど、知識がなくとも読みやすくしています。

本記事のみで学べるよう努めますが、より詳しくかつ体系的に学びたい方は
`実践Vim`を読みながら進めることをおすすめします。

2023 年 5 月 14 日現在、`Kindle Unlimited`対象商品です。

https://www.amazon.co.jp/%E5%AE%9F%E8%B7%B5Vim-%E6%80%9D%E8%80%83%E3%81%AE%E3%82%B9%E3%83%94%E3%83%BC%E3%83%89%E3%81%A7%E7%B7%A8%E9%9B%86%E3%81%97%E3%82%88%E3%81%86-Drew-Neil/dp/4048916599

`実践Vim`のサンプルコードです。
記事更新に合わせて章ごとにディレクトリを分割し、参照しやすいようにします。
※公式のサンプルはリンク切れしています。

https://github.com/kagami-tsukimura/practical-vim-practical

実際に操作することで、コマンドを効率よく定着させましょう。

> Vim は永久に不滅のソフトウェア
> (`実践Vim`「謝辞」より)

ソフトウェアの世界は突き詰めていくとどこかで必ず`Vim`が待ち受けています。
苦手意識をなくして、より良い`開発者体験(DX)`を！

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu22.04
Vim9.0

## 第 1 章 Vim のやり方

## TIP1: ドットコマンドとは

> ドットコマンドを使うと、直前に行った変更を繰り返せる。
> これは Vim のコマンドの中で最も強力で、使い道も一番たくさんあるものだ。
> (実践 Vim「TIP1」より)

- `Vim`は同じことを繰り返すのがとても得意。
- `ドットコマンド（.）`：直前に行った変更を繰り返せる。
  - 変更のレベルは個々の文字、行全体、ファイル全体。
  - `Vim`コマンドの中で最も強力。

### インデントの繰り返し

- `>G`で現在の行からファイル末尾までのインデントを 1 段深くする。

```text:chapter_code/1_dots/the_vim_way/0_mechanics.txt
Line one
Line two
Line three
Line four
```

**2 行目で`>G`実行**

```text:chapter_code/1_dots/the_vim_way/0_mechanics.txt
Line one
        Line two
        Line three
        Line four
```

**\_`>G`後に`.`で再度インデント**

```text: chapter_code/1_dots/the_vim_way/0_mechanics.txt
Line one
                Line two
                Line three
                Line four
```

## TIP2: DRY（Don't Repeat Yourself）

> 何行にも渡って行末にセミコロンを追加するといったよくある状況について、Vim は 2 つの手順を一度で行える専用のコマンドを提供している。
> (実践 Vim「TIP2」より)

### 各行の末尾にセミコロン

- 行の末尾にセミコロンを追加する。

  ```javascript: chapter_code/1_dots/the_vim_way/2_foo_bar.js
  var foo = 1
  var bar = 'a'
  var foobar = foo + bar
  ```

  - `A;<Esc>`で変更。
    - `a`はカーソルの後ろに追加。
    - `A`は行末に追加（`$a`と同義）。

  ```javascript: chapter_code/1_dots/the_vim_way/2_foo_bar.js
  var foo = 1;
  var bar = 'a'
  var foobar = foo + bar
  ```

- 残りの行の末尾にもセミコロンを追加する。
  - `j`で 1 行下に移動。
  - `.`で変更。
  ```javascript: chapter_code/1_dots/the_vim_way/2_foo_bar.js
  var foo = 1;
  var bar = 'a';
  var foobar = foo + bar;
  ```

## TIP3: 一歩下がって、三歩進む

> Vim にはイディオム的な操作方法があるのだが、これを使うと、ある 1 文字の前後に空白文字を挿入できる。
> (実践 Vim「TIP3」より)

### 変更を繰り返す

- 文字列連結の`+`記号前後に空白文字を挿入。

  ```javascript: chapter_code/1_dots/the_vim_way/3_concat.js
  var foo = "method("+argument1+","+argument2+")";
  ```

  - `f+`で次の`+`に移動。
  - `s<Space>+<Space><Esc>`で空白挿入

  ```javascript: chapter_code/1_dots/the_vim_way/3_concat.js
  var foo = "method(" + argument1+","+argument2+")";
  ```

- 残りの`+`記号前後にも空白文字を挿入。
  - `;`で直前の`f`を繰り返す。
  - `.`で空白挿入
  ```javascript: chapter_code/1_dots/the_vim_way/3_concat.js
  var foo = "method(" + argument1 + "," + argument2 + ")";
  ```

## TIP4: 実行して、繰り返して、元に戻す

> 繰り返し作業にぶち当たったときには、モーションと変更の両者を繰り返し可能なものにすることで、最適な編集方針を打ち立てられる。
> Vim にはこれをうまくやるための天賦の才が備わっている。
> (実践 Vim「TIP4」より)

- `ドットコマンド`で繰り返していると、コマンドミスが簡単に発生する。
  - `u`（アンドゥ）で直前の変更を元に戻す。

## TIP5: 手作業での検索と置換

> Vim には検索置換処理を行う:substitute コマンドがある。
> (実践 Vim「TIP5」より)

- 一括置換はパターンマッチングで置換。

- パターンマッチングと置換は以下を参照。

https://kaworu.jpn.org/vim/vim%E3%81%AE%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E6%A4%9C%E7%B4%A2%E3%81%A8%E7%BD%AE%E6%8F%9B%E3%81%A7%E7%9F%A5%E3%81%A3%E3%81%A6%E3%81%8A%E3%81%8F%E3%81%B9%E3%81%8D%E3%81%93%E3%81%A8

- `content`を`copy`に一括置換。

  ```text: chapter_code/1_dots/the_vim_way/1_copy_content.txt
  ...We're waiting for content before the site can go live...
  ...If you are content with this, let's go ahead with it...
  ...We'll launch as soon as we have the content...
  ```

  - `:%s/content/copy/g`

  ```text: chapter_code/1_dots/the_vim_way/1_copy_content.txt
  ...We're waiting for copy before the site can go live...
  ...If you are copy with this, let's go ahead with it...
  ...We'll launch as soon as we have the copy...
  ```

- `content`を`copy`に 1 つずつ置換。
  - `content`にカーソルを合わせる。
  - `*`で単語検索。
  - `cwcopy<Esc>`で`content`を`copy`に置換。
  - `n`で次の`content`に移動。
  - `.`で`content`を`copy`に置換。
  ```text: chapter_code/1_dots/the_vim_way/1_copy_content.txt
  ...We're waiting for copy before the site can go live...
  ...If you are content with this, let's go ahead with it...
  ...We'll launch as soon as we have the copy...
  ```

## TIP6: ドットの公式

> 共通のパターンとなる最適な編集方針を見いだしてみよう。
> (実践 Vim「TIP6」より)

- `ドットの公式`: `ドットコマンド`の共通パターンとなる最適な編集方針。
  - キーストローク 1 回 + `ドットコマンド`が理想的な解放。
    - これより少ないキーストロークは無理なため。

### 最後に

`ドットコマンド(.)`を覚えておくと`Vim`の強力なコマンドを手軽に繰り返せます。

また、本記事で紹介したコマンドを実際に試してみることを強くおすすめします。

最後まで閲覧頂きありがとうございました。
本記事がお役に立てば幸いです！

次回`ノーマルモード`

https://qiita.com/kagami_t/items/6914e0afc27cb9c995af

### 参考書籍

https://www.amazon.co.jp/%E5%AE%9F%E8%B7%B5Vim-%E6%80%9D%E8%80%83%E3%81%AE%E3%82%B9%E3%83%94%E3%83%BC%E3%83%89%E3%81%A7%E7%B7%A8%E9%9B%86%E3%81%97%E3%82%88%E3%81%86-Drew-Neil/dp/4048916599

### 参考 URL

https://github.com/vim-jp/vimdoc-ja

https://github.com/yochiyochirb/practical-vim/tree/master
