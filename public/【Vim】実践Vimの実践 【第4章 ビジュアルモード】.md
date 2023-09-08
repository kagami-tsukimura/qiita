---
title: 【Vim】実践Vimの実践 【第4章 ビジュアルモード】
tags:
  - Vim
  - Linux
  - Ubuntu
  - 新人プログラマ応援
private: false
updated_at: '2023-09-09T08:28:45+09:00'
id: 7b21948935b4da83b2b0
organization_url_name: null
slide: false
---

# Introduction

`実践Vim`をまとめるシリーズの第 4 章です。
第 1 章:`ドットコマンド`

https://qiita.com/kagami_t/items/623a56f52d10660f72ec

第 2 章:`ノーマルモード`

https://qiita.com/kagami_t/items/6914e0afc27cb9c995af

第 3 章:`挿入モード`

https://qiita.com/kagami_t/items/6c693f4e10450c8e9947

本記事のみで学べるよう努めますが、より詳しくかつ体系的に学びたい方は
`実践Vim`を読みながら進めることをおすすめします。

2023 年 6 月 12 日現在、`Kindle Unlimited`対象商品です。

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

## 第 4 章 ビジュアルモード

`Vim`の`ビジュアルモード`では、テキストを選択して操作を行う。
`ビジュアルモード`には以下の 3 種類がある。

- 文字を扱う。
- 行を扱う。
- 矩形領域を扱う。

繰り返しには`ドットコマンド`が有効。

## TIP20: ビジュアルモードとは

> ビジュアルモードではテキスト範囲を選択して、それらに操作を加えることができる。
> (実践 Vim「TIP20」より)

- `ノーマルモード`コマンドの大半は`ビジュアルモード`でも使用可能。
  - `ビジュアルモード` のカーソル移動で、テキスト選択領域を指定する。

## TIP21: ビジュアルな選択範囲の定義

> ビジュアルモードには 3 つのサブモードがあるが、これらはそれぞれ異なる種類のテキストを扱う。
> (実践 Vim「TIP21」より)

- 3 種類の`ビジュアルモード`
  - 文字指向　　 : 個々の単語やフレーズを操作する。
  - 行指向　　　 : 行全体を操作する。
  - ブロック指向 : ドキュメント中の矩形領域を操作する。

### `ビジュアルモード`の有効化。

- `ビジュアルモード`に切り替え。
  - `v`で`文字指向のビジュアルモード`に切り替え。
  - `V`で`行指向のビジュアルモード`に切り替え。
  - `Ctrl + v`で`ブロック指向のビジュアルモード`に切り替え。
  - `gv`で`直前の選択範囲を再選択。

## TIP22: 行指向のビジュアルモードコマンドを繰り返す。

> ビジュアルな選択範囲に対して行った変更をドットコマンドで繰り返すと、テキストの同じ範囲に対してその変更が繰り返される。
> (実践 Vim「TIP22」より)

- `ビジュアルモード`でインデントする。[^1]
  [^1]: 本書の`fibonacci-malformed.py`は Python2 系の記述だったため、Python3 系の記述に書き換えています。

  ```text: chapter_code/4_visual/visual_mode/fibonacci-malformed.py
  def fib(n):
      a, b = 0, 1
      while a < n:
  print(a)
  a, b = b, a+b
  fib(42)
  ```

  - `3j`で 4 行目に移動する。
  - `V`で`行指向のビジュアルモード`に切り替え。
  - `j`で 1 行下も選択する。
  - `>`でインデントする。
  - `.`でインデントを繰り返す。

  ```text: chapter_code/4_visual/visual_mode/fibonacci-malformed.py
  def fib(n):
      a, b = 0, 1
      while a < n:
          print(a)
          a, b = b, a+b
  fib(42)
  ```

## TIP23: 可能ならばビジュアルコマンドではなくオペレータを優先しよう

> Vim のノーマルモードでの操作と比べて、ビジュアルモードのほうが直感的かもしれないが、これには弱点がある。
> 常にドットコマンドとうまく連携できるとは限らないのだ。
> ノーマルモードのオペレータを適切に使えば、この弱点を回避できる。
> (実践 Vim「TIP23」より)

### ビジュアルオペレータを使う

- リンクを大文字に変換する。
  ```text: chapter_code/4_visual/visual_mode/list-of-links.html
  <a href="#">one</a>
  <a href="#">two</a>
  <a href="#">three</a>
  ```
  - `vit`で a タグの内容を選択する。
  - `U`で選択した文字列を大文字に変換する。
  ```text: chapter_code/4_visual/visual_mode/list-of-links.html
  <a href="#">ONE</a>
  <a href="#">two</a>
  <a href="#">three</a>
  ```
  - `j.j.`で残りの行も大文字変換を試みる。
    - `ビジュアルモード`のコマンドは同じ範囲しか繰り返されない。
    - 元々のコマンドが 3 文字に適用したため、3 文字しか繰り返されない。
  ```text: chapter_code/4_visual/visual_mode/list-of-links.html
  <a href="#">ONE</a>
  <a href="#">TWO</a>
  <a href="#">THRee</a>
  ```

### ノーマルオペレータを使う

- リンクを大文字に変換する。
  ```text: chapter_code/4_visual/visual_mode/list-of-links.html
  <a href="#">one</a>
  <a href="#">two</a>
  <a href="#">three</a>
  ```
  - `gUit`で a タグの内容を大文字に変換する。
    - `gU`: `ビジュアルモード`の`U`と同様のコマンド。
  ```text: chapter_code/4_visual/visual_mode/list-of-links.html
  <a href="#">ONE</a>
  <a href="#">two</a>
  <a href="#">three</a>
  ```
  - `j.j.`で残りの行も大文字変換する。
  ```text: chapter_code/4_visual/visual_mode/list-of-links.html
  <a href="#">ONE</a>
  <a href="#">TWO</a>
  <a href="#">THREE</a>
  ```

## TIP24: ブロック指向のビジュアルモードで表形式のデータを編集

> どんなエディタでもテキスト行は扱える。
> でも、テキスト列を操作するにはもっと専門的なツールが必要だ。
> Vim にはこの機能がある。
> (実践 Vim「TIP24」より)

- プレーンテキストを表形式に加工する。
  ```text: chapter_code/4_visual/visual_mode/chapter-table.txt
  Chapter            Page
  Normal mode          15
  Insert mode          31
  Visual mode          44
  ```
  - 列間に移動する。
  - `Ctrl + v`で`ブロック指向のビジュアルモード`に切り替え。
  - `3j`で 3 行下まで選択する。
  - `x...`で列間を削除する。
  - `gv`で直前の選択範囲(4 行分)を再選択。
  - `r|`で選択範囲をパイプ(|)に置換する。
  - `yyp`で先頭行をコピーする。
  - `Vr-`でコピーした行の文字すべてをハイフン(-)に置換する。
  ```text: chapter_code/4_visual/visual_mode/chapter-table.txt
  Chapter     |  Page
  -------------------
  Normal mode |    15
  Insert mode |    31
  Visual mode |    44
  ```

## TIP25: テキスト列の変更

> ブロック指向のビジュアルモードを使うと、テキストを一度に複数のテキスト行へ挿入できる。
> (実践 Vim「TIP25」より)

- `ブロック指向のビジュアルモード`でパスを変更する。
  ```text: chapter_code/4_visual/visual_mode/sprite.css
  li.one   a{ background-image: url('/images/sprite.png'); }
  li.two   a{ background-image: url('/images/sprite.png'); }
  li.three a{ background-image: url('/images/sprite.png'); }
  ```
  - `f/l`で`/`の後ろまで移動する。
  - `Ctrl + v`で`ブロック指向のビジュアルモード`に切り替え。
  - `jje`で 3 行分の`images`を選択する。
  - `c`で選択範囲を削除して`挿入モード`に切り替え。
  - `components`と入力する。
  - `Esc`でノーマルモードに切り替え。[^2]
    [^2]: `ノーマルモード`に戻るタイミングで、入力内容が他の行に反映される。
  ```text: chapter_code/4_visual/visual_mode/sprite.css
  li.one   a{ background-image: url('/components/sprite.png'); }
  li.two   a{ background-image: url('/components/sprite.png'); }
  li.three a{ background-image: url('/components/sprite.png'); }
  ```

## TIP26: 矩形状ではないビジュアルな選択範囲にテキストを追加

> ブロック指向のビジュアルモードは、コードを矩形状に選択して、そこに操作を加えるときにはすばらしく役に立つ。
> でも、矩形状のテキスト範囲でないと使えないというわけでもない。
> (実践 Vim「TIP26」より)

- 長さの異なる各行の末尾にセミコロン(;)を追加する。[^3]
  [^3]: ドットコマンド(.)で対処する場合は[第 1 章](https://qiita.com/kagami_t/items/623a56f52d10660f72ec '第1章')を参照。

  ```text: chapter_code/4_insert/insert_mode/2_foo_bar.js
  var foo = 1
  var bar = 'a'
  var foobar = foo + bar
  ```

  - `Ctrl + v`で`ブロック指向のビジュアルモード`に切り替え。
  - `jj$`で 3 行分の末尾まで選択する。
  - `A;`で各行の末尾にセミコロン(;)を入力する。
  - `Esc`でノーマルモードに切り替え。[^2]

  ```text: chapter_code/3_insert/insert_mode/replace.txt
  Typing in Insert mode extends the line, but in Replace mode
  the line length doesn't change.
  ```

### 最後に

`ビジュアルモード`は前回の[挿入モード](https://qiita.com/kagami_t/items/6c693f4e10450c8e9947 '挿入モード')、前々回の[ノーマルモード](https://qiita.com/kagami_t/items/6914e0afc27cb9c995af 'ノーマルモード')と比べて馴染みがなかったかもしれません。
知らなくてもエディタとして最低限の役割を果たせますし、`ビジュアル`でも 3 種類に分かれています。

もし難しく感じたら、ブロック指向(`Ctrl + v`)から試してみてください。
操作自体は`ノーマルモード`と大差ないため、覚える内容に対しての効果は大きいです。

また、本記事で紹介したコマンドを実際に試してみることを強くおすすめします。

最後まで閲覧頂きありがとうございました。
本記事がお役に立てば幸いです！

次回`コマンドラインモード`

https://qiita.com/kagami_t/items/4f9ae55aeabb9cec24a0

### 参考書籍

https://www.amazon.co.jp/%E5%AE%9F%E8%B7%B5Vim-%E6%80%9D%E8%80%83%E3%81%AE%E3%82%B9%E3%83%94%E3%83%BC%E3%83%89%E3%81%A7%E7%B7%A8%E9%9B%86%E3%81%97%E3%82%88%E3%81%86-Drew-Neil/dp/4048916599

### 参考 URL

https://github.com/vim-jp/vimdoc-ja

https://github.com/yochiyochirb/practical-vim/tree/master
