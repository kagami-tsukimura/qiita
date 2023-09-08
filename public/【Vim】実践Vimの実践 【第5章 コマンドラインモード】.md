---
title: 【Vim】実践Vimの実践 【第5章 コマンドラインモード】
tags:
  - Vim
  - Linux
  - Ubuntu
  - 新人プログラマ応援
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
---

# Introduction

`実践Vim`をまとめるシリーズの第 5 章です。
第 1 章:`ドットコマンド`

https://qiita.com/kagami_t/items/623a56f52d10660f72ec

第 2 章:`ノーマルモード`

https://qiita.com/kagami_t/items/6914e0afc27cb9c995af

第 3 章:`挿入モード`

https://qiita.com/kagami_t/items/6c693f4e10450c8e9947

第 4 章:`ビジュアルモード`

https://qiita.com/kagami_t/items/7b21948935b4da83b2b0

本記事のみで学べるよう努めますが、より詳しくかつ体系的に学びたい方は
`実践Vim`を読みながら進めることをおすすめします。

2023 年 9 月 9 日現在、絶版商品のため `Amazon Kindle` での購入を推奨します。

https://www.amazon.co.jp/%E5%AE%9F%E8%B7%B5Vim-%E6%80%9D%E8%80%83%E3%81%AE%E3%82%B9%E3%83%94%E3%83%BC%E3%83%89%E3%81%A7%E7%B7%A8%E9%9B%86%E3%81%97%E3%82%88%E3%81%86-Drew-Neil/dp/4048916599

`実践Vim`のサンプルコードです。
記事更新に合わせて章ごとにディレクトリを分割し、参照しやすいようにします。
※公式のサンプルはリンク切れしています。

https://github.com/kagami-tsukimura/practical-vim-practical

実際に操作することで、コマンドを効率よく定着させましょう。

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu22.04
Vim9.0

## 第 5 章 コマンドラインモード

> 初めに ed ありき。
> ed から ex ができ、ex から vi ができた。
> そして。vi から Vim ができた。
> (実践 Vim「第 5 章 コマンドラインモード」より)

## TIP27: Vim のコマンドラインモード

- `コマンドラインモード`では Ex コマンドや検索パターン、あるいは式を入力するように求められる。

  - `:`で`コマンドラインモード`に切り替え。
  - `Esc`で`ノーマルモード`に切り替え。

- `Exコマンド`とは`コマンドラインモード`で実行するコマンド。
  - 本章では`Exコマンド`を紹介。
  - `EXコマンド`は広範囲を対象に、一度で多くの行を変更できる。

## TIP28: 連続する行に対してコマンドを実行する

> Ex コマンドの多くには、それを実行する行の[range]を指定できる。
> この範囲の始点と終点は行番号、マーク、パターンを使って指定可能だ。
> (実践 Vim「TIP28」より)

### 行番号をアドレスとして使用する

- `Exコマンド`の例。

  - `:print`で指定した行をエコーする。
  - 行番号をアドレスとして使用できる。

  ```html: chapter_code/5_commandline/ex_mode/practical-vim.html
  <!DOCTYPE html>
  <!--
  ! Excerpted from "Practical Vim",
  ! published by The Pragmatic Bookshelf.
  ! Copyrights apply to this code. It may not be used to create training material,
  ! courses, books, articles, and the like. Contact us if you are in doubt.
  ! We make no guarantees that this code is fit for any purpose.
  ! Visit http://www.pragmaticprogrammer.com/titles/dnvim for more book information.
  -->
  <html>
    <head><title>Practical Vim</title></head>
    <body><h1>Practical Vim</h1></body>
  </html>
  ```

  - `:1`で 1 行目に移動する。
  - `:p`で 1 行目の`<!DOCTYPE html>`をエコー。

  ```vim
  <!DOCTYPE html>
  ```

  - `:$`でファイル末尾に移動する。
  - `:p`で末尾の`</html>`をエコー。

  ```vim
  </html>
  ```

  - 移動とエコーは同時使用も可能。

    - `:11p`で 11 行目の`<head><title>Practical Vim</title></head>`をエコー。

    ```vim
    <head><title>Practical Vim</title></head>
    ```

### アドレスで行範囲を指定する

- アドレスは行範囲でも指定可能。

  - `:10,$p`で 10 行目から末尾までをエコー。

  ```vim
    <html>
    <head><title>Practical Vim</title></head>
    <body><h1>Practical Vim</h1></body>
    </html>
  ```

- 現在行は`.`で表せる。
  - `:.,$p`で 現在行から末尾までをエコー。
- すべての行は`%`で表せる。
  - `:%p`ですべての行をエコー。[^1]
    [^1]: `:%`は実務でも使用ケースが多いです。`:%d`で全行削除など。

## TIP29: [:t] / [:m]コマンドで行をコピー/移動

> :copy コマンド（とその短縮形の:t）を使うと、1 行以上の行を
> ドキュメント内のある場所から別の場所にコピーできる。
> 一方、:move コマンドでは、ドキュメント内の別の場所へ移動できる。
> (実践 Vim「TIP29」より)

### [:t]で行コピー

`:copy`コマンド(`:t`)で行を別の場所にコピーする。

```text: chapter_code/5_commandline/ex_mode/shopping-list.todo
Shopping list
    Hardware Store
        Buy new hammer
    Beauty Parlor
        Buy nail polish remover
        Buy nails
```

- `:2`で 2 行目に移動する。
- `:6copy.`(`:6t.`)で下の行に 6 行目の内容をコピーする。

```text: chapter_code/5_commandline/ex_mode/shopping-list.todo
Shopping list
    Hardware Store
        Buy nails
        Buy new hammer
    Beauty Parlor
        Buy nail polish remover
        Buy nails
```

`:t`コマンドの使用例

| コマンド |                    結果                    |
| :------: | :----------------------------------------: |
|   :6t.   |         6 行目を現在行の下にコピー         |
|   :t6    |        現在行を 6 行目の下にコピー         |
|   :t.    |         現在行をコピー(yyp と同様)         |
|   :t$    |        現在行をファイル末尾にコピー        |
| :'<,'>t0 | ビジュアルな選択範囲をファイル先頭にコピー |

### [:m]で行移動

`:move`コマンド(`:m`)で行を別の場所に移動する。

- `Hardware Store` セクションを `Beauty Parlor` の下に移動する。

```text: chapter_code/5_commandline/ex_mode/shopping-list.todo
Shopping list
    Hardware Store
        Buy nails
        Buy new hammer
    Beauty Parlor
        Buy nail polish remover
        Buy nails
```

- `:2`で 2 行目に移動する。
- `Vjj`で`Hardware Store` セクションをビジュアルモードで範囲選択する。
- `:'<,'>m$`で`Beauty Parlor` の下に移動する。[^2]
  [^2]:複雑に見えるが、`:`を押すと選択範囲である`'<,'>`は自動で記入される。
  是非試してほしいです。

```text: chapter_code/5_commandline/ex_mode/shopping-list.todo
Shopping list
    Beauty Parlor
        Buy nail polish remover
        Buy nails
    Hardware Store
        Buy nails
        Buy new hammer
```

- `@:`で直前の Ex コマンドを繰り返す。

## TIP30: 選択範囲に対してノーマルモードのコマンドを実行する

> 連続する行に対してノーマルモードコマンドを実行したいときには、
> :normal コマンドを使えばよい。
> ドットコマンドやマクロと組み合わせると、繰り返し作業をほんのわずかな手間で行える。
> (実践 Vim「TIP30」より)

- `:normal`でドットコマンドを繰り返す。
- 各業の末尾に`;`を挿入する。

```text: chapter_code/5_commandline/ex_mode/foobar.js
/***
 * Excerpted from "Practical Vim",
 * published by The Pragmatic Bookshelf.
 * Copyrights apply to this code. It may not be used to create training material,
 * courses, books, articles, and the like. Contact us if you are in doubt.
 * We make no guarantees that this code is fit for any purpose.
 * Visit http://www.pragmaticprogrammer.com/titles/dnvim for more book information.
***/
var foo = 1
var bar = 'a'
var baz = 'z'
var foobar = foo + bar
var foobarbaz = foo + bar + baz
```

- `:9`で`var foo = 1`(`;`追加したい行の最初)に移動する。
- `A;<Esc>`で`var foo = 1`の末尾に`;`を挿入してノーマルモードに戻る。

```text: chapter_code/5_commandline/ex_mode/foobar.js
/***
 * Excerpted from "Practical Vim",
 * published by The Pragmatic Bookshelf.
 * Copyrights apply to this code. It may not be used to create training material,
 * courses, books, articles, and the like. Contact us if you are in doubt.
 * We make no guarantees that this code is fit for any purpose.
 * Visit http://www.pragmaticprogrammer.com/titles/dnvim for more book information.
***/
var foo = 1;
var bar = 'a'
var baz = 'z'
var foobar = foo + bar
var foobarbaz = foo + bar + baz
```

- `jVG`で`var foo = 1`の下の行から行の末尾までを選択する。
- `:'<,'>normal .`で選択した行の末尾に`;`を挿入する。[^3]
  [^3]:ドットコマンドで試しましたが、ノーマルモードのコマンドであれば同じように使えます。

```text: chapter_code/5_commandline/ex_mode/foobar.js
/***
 * Excerpted from "Practical Vim",
 * published by The Pragmatic Bookshelf.
 * Copyrights apply to this code. It may not be used to create training material,
 * courses, books, articles, and the like. Contact us if you are in doubt.
 * We make no guarantees that this code is fit for any purpose.
 * Visit http://www.pragmaticprogrammer.com/titles/dnvim for more book information.
***/
var foo = 1;
var bar = 'a';
var baz = 'z';
var foobar = foo + bar;
var foobarbaz = foo + bar + baz;
```

## TIP31: 直前の Ex コマンドを繰り返す

> `.`コマンドは直前のノーマルモードコマンドを繰り返すのに使えるが、
> 直前の Ex コマンドを繰り返したいときには`.`コマンドではなく`@:`を使う必要がある。
> 直前の Ex コマンドを取り消す方法を知っておくのはいつだって役に立つ。
> ということで、これについて話をしていこう。
> (実践 Vim「TIP31」より)

📚 編集中...

### 最後に

📚 編集中...

また、本記事で紹介したコマンドを実際に試してみることを強くおすすめします。

最後まで閲覧頂きありがとうございました。
本記事がお役に立てば幸いです！

次回`コマンドラインモード`

[]

### 参考書籍

https://www.amazon.co.jp/%E5%AE%9F%E8%B7%B5Vim-%E6%80%9D%E8%80%83%E3%81%AE%E3%82%B9%E3%83%94%E3%83%BC%E3%83%89%E3%81%A7%E7%B7%A8%E9%9B%86%E3%81%97%E3%82%88%E3%81%86-Drew-Neil/dp/4048916599

### 参考 URL

https://github.com/vim-jp/vimdoc-ja

https://github.com/yochiyochirb/practical-vim/tree/master
