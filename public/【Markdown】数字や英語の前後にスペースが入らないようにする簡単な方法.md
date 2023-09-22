---
title: 【Markdown】数字や英語の前後にスペースが入らないようにする簡単な方法
tags:
  - Markdown
  - VSCode
  - prettier
  - QiitaCLI
private: false
updated_at: '2023-09-22T23:15:26+09:00'
id: 96b2b642ef7e253fddba
organization_url_name: null
slide: false
ignorePublish: false
---

# Introduction

Qiita CLIがリリースされ、従来のGUIからVSCodeでMarkdown形式で記事を書くようになりました。

https://qiita.com/kagami_t/items/0760712d29003841a1fd

しかしMarkdownで書くことで、VSCodeに入れていたPrettierという拡張機能が影響し、数字と英語の前後に半角スペースが入り込んでいました。

本記事ではUbuntu22.04環境で上記の事象を解決します。

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu22.04

## 実施内容

まずは事象の確認を行います。

![Screenshot from 2023-09-22 22-31-37.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/451afcd0-885a-54cc-c93d-30ee07776d84.png)

> 2023 年 8 月
> 個人的には GUI でも

以前からMarkdownでメモを取っているので気にはなりましたが、Prettierのようなコードフォーマッターに余計な設定を入れたくはなかったため放置していました。
メモのような個人用途はまだしも、人様に読んで頂く上で見辛い記事を提供するわけにはいかないため、スペースを取り除く設定を加えました。

最初は以下の記事を参考に設定しましたが、適用されずスペースは残りました。
設定からPrettierのResolve Global ModulesをTrueにする方法です。

https://qiita.com/LifescrewDesign/items/4c5b8f076bdf59538f96

Win10ではResolve Global Modulesでフォーマットできない旨が書かれており、私の環境（Ubuntu22.04）でも同様に動作しませんでした。

別の方法で解決することにし、以下の記事を参考に設定しました。
対処法2であればUbuntu22.04環境でも適用できました。

https://qiita.com/kumapo0313/items/92d1597da5f3752f6584

記事の内容に沿ってPrettierのディレクトリを開きます。
Ubuntuでは以下に格納されています。

```bash
cd ~/.vscode/extensions/esbenp.prettier-vscode-10.1.0/node_modules/prettier/
```

記事が若干古いためprettierのバージョンなど異なりますので、必要に応じて`esbenp.prettier-vscode-10.1.0/`は`esbenp`辺りでタブ補完をするとスムーズです。

index.jsを修正していきます。
私のバージョンでは33413行目が該当箇所なため、vimで行を指定して開きます。

```bash
vim +33143 index.js
```

普通にvimで開いてから、`/function appendNode`で引っ掛けても良いです。

![Screenshot from 2023-09-22 23-00-33.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/7679ff04-3c1e-ba3b-84e5-bdfa72f4145e.png)

修正前は`value: " "`となっているため、画像の通り`value: ""`としてスペースを消しましょう。

スペースが消せるか保存して確認します。

![Screenshot from 2023-09-22 23-06-40.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/1f599584-847b-59c6-9a35-354047d2de21.png)

無事消せました！

### 最後に

最後まで閲覧頂きありがとうございました。
OS問題やバージョン問題がありそうなので、特にUbuntu環境の方は参考にしてみてください！
Qiita CLIの登場もあり、QiitaユーザーでMarkdownを書く機会が増える方もいると思います。
スペースが気になっていた方は、是非本記事や紹介した他の方の記事を見ながらスペース消して快適な執筆生活を送りましょう！

本記事がお役に立てば幸いです！

### 参考URL

https://github.com/prettier/prettier/issues/5938

https://qiita.com/LifescrewDesign/items/4c5b8f076bdf59538f96

https://qiita.com/kumapo0313/items/92d1597da5f3752f6584
