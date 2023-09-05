---
title: 【Vim】 エラーに素早く対処する！ 行数指定でファイル起動
tags:
  - Vim
  - Linux
  - Ubuntu
private: false
updated_at: '2023-05-12T22:44:54+09:00'
id: f2f938ec27216a6cc04d
organization_url_name: null
slide: false
---
# Introduction

仕事でもプライベートでもメインで`VSCode`サブで`Vim`を使っているのですが、日々の業務をこなす中で、`VSCode`より`Vim`の方が便利と思うケースが幾つかありました。

コマンドだけ丸暗記は苦痛なので、実際に業務で起きたケースに基づいて紹介していきます。

忙しい方向けで先にコマンド、後に活用例を記載します。

1記事1テーマ予定ですが、単発にするかシリーズ化するかは不明です。

モチベ次第で……

__本記事が少しでも読者様の学びに繋がれば幸いです！__
__「いいね」をしていただけると今後の励みになるので、是非お願いします！__

## 環境
Ubuntu22.04

## 行番号を指定して開く。

`Vim`起動時に`+`又は`-c`で指定した行数を開けます。

`web_crawler.py`の110行目を指定して開く例。

```bash
vim +110 web_crawler.py
```
```bash
vim -c110 web_crawler.py
```
```bash
vim -c 110 web_crawler.py
```

行数指定はファイル名の後でも可。

```bash
vim web_crawler.py +110
```
```bash
vim web_crawler.py -c110
```
```bash
vim web_crawler.py -c 110
```

`+`はブランクがあると行数指定できません。`+<行数>`
`-c`の行数指定はブランク有無問わず指定可能です。`-c<行数>`,`-c　<行数>`

個人的に`-c`は実行ファイルの引数と混同しやすいので`+`の方が覚えやすいです。

## 実際の使用ケース

「ターミナルでPythonファイルを実行！」
```bash:
python3 web_crawler.py
```
「あ、エラー出た。」

```console:
Start crawling images...
Traceback (most recent call last):
  File "/home/_/create_movie/crawler/ml/web_crawler.py", line 135, in <module>
    main(args)
  File "/home/_/create_movie/crawler/ml/web_crawler.py", line 110, in main
    with open(SEARCH_FILE) as f:
         ^^^^^^^^^^^^^^^^^
FileNotFoundError: [Errno 2] No such file or directory: 'search.yaml'
```

「110行目でエラーか……`yaml`ファイルの配置を直下から変えたんだっけ。」

「`VSCode`起動して……ファイル開いて……110行目に移動して……パス修正。」

「修正したからターミナルに戻って……マルチタスクで大量に窓開いているから戻り辛い。」

「もう一度Pythonファイルを実行！」

......手間ですね。遅いですね。ストレッサーになりかねませんね。
こんなケースで先程ご紹介した行数指定のコマンドが役に立ちます。

```bash
vim +110 web_crawler.py
```

これでエラー箇所をいち早く修正してそのまま再実行可能です。

忘れても普通に`Vim`でファイルを開いた後に`行数G`で移動できます。
今回のように110行目であれば`Ctrl + F`や、最悪`jkhl`で移動も一応出来ます。

ただ`Vim`で開いてすぐお目当てのコードと向き合えることは想像以上に心地良いので、皆様にも是非体験していただきたいです。

軽いエラーの度にアプリケーションを遷移していると集中力も切れやすいので、楽していきましょう！

## 最後に

エラーが複数あったり、根が深いものであれば使い慣れたエディタを使うことをおすすめします。
ただ今回のように、エラーを見てすぐ修正案が浮かんだ場合はどうでしょうか？
いち早く直して再実行したいですよね。

今回ご紹介したコマンドを覚えておけばコマンド入力と移動を同時に行えます。
素早くエラーを修正して、ストレスフリーで業務や個人開発に勤しみましょう。

最後まで閲覧頂きありがとうございました。
備忘録の側面もありますが、本記事がお役に立てば幸いです！
