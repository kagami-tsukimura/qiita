---
title: 【Vim】実践Vimの実践 【第3章 挿入モード】
tags:
  - Vim
  - Linux
  - Ubuntu
  - 新人プログラマ応援
private: false
updated_at: '2023-09-09T08:37:15+09:00'
id: 6c693f4e10450c8e9947
organization_url_name: null
slide: false
ignorePublish: false
---

# Introduction

`実践Vim`をまとめるシリーズの第 3 章です。
前々回:`ドットコマンド`

https://qiita.com/kagami_t/items/623a56f52d10660f72ec

前回:`ノーマルモード`

https://qiita.com/kagami_t/items/6914e0afc27cb9c995af

本記事のみで学べるよう努めますが、より詳しくかつ体系的に学びたい方は
`実践Vim`を読みながら進めることをおすすめします。

2023 年 6 月 11 日現在、`Kindle Unlimited`対象商品です。

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

## 第 3 章 挿入モード

## TIP13: 挿入モードで簡単修正

> 挿入モードでテキストを記述しているときに間違えたら、そこですぐに修正できる。
> モードを切り替える必要はない。
> (実践 Vim「TIP13」より)

- `i`キーで`挿入モード`に移動。

  - 左下には`-- 挿入 --`と表示。

- `Backspace`キー以外で手早く修正するコマンドがある。[^1]
  [^1]: `Vim`に限らず`bash`のコマンドラインでも使用可能。
  - `Ctrl + h`で直前の 1 文字を削除。(`Backspace`と同じ処理。)
  - `Ctrl + w`で直前の 1 単語を削除。
  - `Ctrl + u`で行全体を削除。

## TIP14: ノーマルモードへの復帰

> 挿入モードは、テキスト入力というただ 1 つの作業に特化したモードだ。
> 一方、Vim ユーザーが 1 日の大半を過ごすのがノーマルモードだ。
> (実践 Vim「TIP14」より)

- モードの切り替えの煩雑さを削減する。

  - `Esc`で`ノーマルモード`に切り替え。
  - `Ctrl + [`で`ノーマルモード`に切り替え。

- `挿入ノーマルモード`は 1 コマンドのみ`ノーマルモード`で行う。
  - `Ctrl + o`で`挿入ノーマルモード`に切り替え。
  - 左下には`-- (挿入) --`と表示。
  - 1 コマンド後は`挿入モード`に戻る。
  - 画面操作をしてすぐに`挿入モード`に戻りたい時に重宝。
    - `zz`コマンドでスクロールなど。

## TIP15: 挿入モードから抜けないでレジスタから貼り付け

> Vim のヤンク操作とプット操作は通常、ノーマルモードから実行されるが、挿入モードから抜けないでドキュメントにテキストを貼り付けたいときがある。
> (実践 Vim「TIP15」より)

### ヤンクして貼り付け

- カンマ(,)より前のテキストをレジスタにヤンクして、次の行の末尾に貼り付ける。

  ```text: chapter_code/3_insert/insert_mode/practical-vim.txt
  Practical Vim, by Drew Neil
  Read Drew Neil's
  ```

  - `yt,`でカンマ(,)より前のテキストをレジスタにヤンク。
  - `jA<Space>`で次の行の末尾で`挿入モード`に切り替え、スペースを入れる。
  - `<Ctrl + r>0`でヤンクしたテキストを貼り付け。
  - `.<Esc>`でドット(.)を入力、`挿入モード`に切り替え。

  ```text: chapter_code/3_insert/insert_mode/practical-vim.txt
  Practical Vim, by Drew Neil
  Read Drew Neil's Practical Vim.
  ```

## TIP16: 簡単な計算をその場で実行

> Expression レジスタを使うと、計算を実行して、その結果を直接ドキュメントに挿入できる。
> (実践 Vim「TIP16」より)

- `Expressionレジスタ`
  - `<Ctrl + r>=`で実行。
  - 式を入力し`Enter`で計算結果をドキュメントに挿入。
    - `Vim`のスクリプトを電卓のように扱える。

### 式の計算

- `Expressionレジスタ`で式の計算を行う。

  ```text: chapter_code/3_insert/insert_mode/back-of-envelope.txt
  6 chairs, each costing $35, totals $
  ```

  - `A`で行末に移動し`挿入モード`に切り替え。
  - `cW`で「.blog」を「.news」に変更。
  - `6*35<Enter>`で 6\*35 の結果(210)を挿入する。

  ```text: chapter_code/3_insert/insert_mode/back-of-envelope.txt
  6 chairs, each costing $35, totals $210
  ```

## TIP17: 文字コードを使って特殊文字を入力

> Vim はその文字コードを指定することでどんな文字でも入力できる。
> キーボードに対応するキーがない記号を入力する時に、これが便利だ。
> (実践 Vim「TIP17」より)

- 特殊文字の挿入。
  - `<Ctrl + v>{3桁の数値コード}`で 10 進コードで表現される文字を挿入。
    - `<Ctrl + v>{065}`で`A`
  - `<Ctrl + v>u{4桁の数値コード}`で 16 進コードで表現される文字を挿入。
    - `<Ctrl + v>{00bf}`で`¿`
  - `<Ctrl + v>{nondigit(非数値)}`で数値以外の文字をそのまま挿入。
  - `<Ctrl + k>{char1}{char2}`で{char1}{char2}のダイグラフで表現される文字を挿入。

## TIP18: ダイグラフによって特殊文字を挿入

> Vim はその数値コードを指定することでどんな文字でも入力できるが、その一方で数値コードを覚えるのは難しいし、入力もめんどうだ。
> 特殊文字はダイグラフとしても入力が可能だ。
> (実践 Vim「TIP18」より)

ダイグラフの組み合わせ自体がどんな文字かを表現しており、覚えやすく推測が容易。

- ダイグラフによる特殊文字の挿入。
  - `<Ctrl + k>?I`で`¿`
  - `<Ctrl + k>12`で`½`
    - `14,34`でそれぞれ`¼,¾`

## TIP19: 置換モードで既存のテキストを上書き

> 置換モードは、これがドキュメントにすでに入力されているテキストを上書きすることを除けば、挿入モードと同じだ。
> (実践 Vim「TIP19」より)

- `置換モード`でピリオド(.)をカンマ(,)に、大文字を小文字に変更する。

  ```text: chapter_code/3_insert/insert_mode/replace.txt
  Typing in Insert mode extends the line. But in Replace mode
  the line length doesn't change.
  ```

  - `f.`で直近のピリオド(.)に移動。
  - `R`で`置換モード`に切り替え。
    - `, b<Esc>`で`. B`を`, b`に置換。

  ```text: chapter_code/3_insert/insert_mode/replace.txt
  Typing in Insert mode extends the line, but in Replace mode
  the line length doesn't change.
  ```

### 最後に

前回の`ノーマルモード`に比べ、`挿入モード`は一般的なエディタに近く馴染み深かったと思います。
前回も記載しましたが、イメージとしては、日々のコーディングが`挿入モード`です。
コーディングのための準備が`ノーマルモード`です。

モードの切り替えは`Vim`を扱う上でストレスになる要因のため、`挿入モード`以外も積極的に使用して慣れることをおすすめします。
モードの特性を理解することで、`Vim`が更に強力なエディタとなります。

最後に紹介した`置換モード`ですが、`r`コマンドで 1 文字だけ置換できます。
使用シーンによりますが、ちょっとした置換が多い場合は是非お試しください。

また、本記事で紹介したコマンドを実際に試してみることを強くおすすめします。

最後まで閲覧頂きありがとうございました。
本記事がお役に立てば幸いです！

次回`ビジュアルモード`

https://qiita.com/kagami_t/items/7b21948935b4da83b2b0

### 参考書籍

https://www.amazon.co.jp/%E5%AE%9F%E8%B7%B5Vim-%E6%80%9D%E8%80%83%E3%81%AE%E3%82%B9%E3%83%94%E3%83%BC%E3%83%89%E3%81%A7%E7%B7%A8%E9%9B%86%E3%81%97%E3%82%88%E3%81%86-Drew-Neil/dp/4048916599

### 参考 URL

https://github.com/vim-jp/vimdoc-ja

https://github.com/yochiyochirb/practical-vim/tree/master
