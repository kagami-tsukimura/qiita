---
title: 【Qiita CLI】 祝！正式版！ Qiita CLIを試してみた！
tags:
  - Qiita
  - Node.js
  - CLI
  - npm
  - QiitaCLI
private: false
updated_at: '2023-09-06T22:34:21+09:00'
id: 0760712d29003841a1fd
organization_url_name: null
slide: false
---

# Introduction

2023 年 8 月のアップデートで、[Qiita CLI](https://github.com/increments/qiita-cli)が正式リリースされました。

https://qiita.com/Qiita/items/563a6ea111709a03fbea

<https://qiita.com/Qiita/items/563a6ea111709a03fbea>[^1]
[^1]: リンクのところ<>で囲むと表示されなくなっちゃいますね。
あと若干リンクの反映も怪しいです。

個人的には GUI でもある程度快適に書けており、ベータ版で途中まで試した限りでは GUI の方が使い勝手良く感じました。

ですが GitHub 上で記事を管理できる点は魅力的なので、改めて正式版で試してみたいと思います。

なお、本記事も Qiita CLI で執筆しております。

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu22.04
Node.js 20.04

## 実行手順

手順については公式の記事や GitHub 上にわかりやすく記載されています。

https://github.com/increments/qiita-cli

https://qiita.com/Qiita/items/666e190490d0af90a92b#qiita-cli%E3%81%A8%E3%81%AF

## お試し（良かった点）

手順に従って Qiita CLI をセットアップしていきます。
Node.js が必要になるので、まだの方はインストールから始めましょう。

https://qiita.com/sefoo0104/items/0653c935ea4a4db9dc2b

Node.js は OS 問わず GUI から良い感じにインストールできます。[^2]
[^2]: CLI に慣れたせいか Ubuntu で GUI からインストールすると謎の不安感があります。

![Screenshot from 2023-09-05 20-15-33.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/99cafe7a-6fe8-0d0c-2d83-fbb94db54220.png)

`npx qiita init`で設定ファイルや GitHub Actions の準備がされるのですが、次のステップも紹介してくれて迷うことはなさそうです。

一応 tree を見ると以下のようになっています。

```bash
tree -a -L 1
```

![Screenshot from 2023-09-05 20-18-44.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/3ca1df31-1cb3-4af8-5706-ad664ccc679c.png)

公式記事を参考にトークンを発行してログインします。

```bash
npx qiita login
```

![Screenshot from 2023-09-05 20-20-27.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/3ba6de82-91b9-3c3a-492e-c45db81b5f1f.png)
この辺りまではストレスもなく良い感じです。

:::note info
少し仕様を確認してコマンドを覚えたら、とても快適です！
CLI で完結を意識してから認識が大きく変わりました。
慣れないうちは、下記の help コマンドを参照すると CLI で完結しやすいです。
:::

```bash
npx qiita help
```

## お試し（残念な点）

:::note warn
私の調査不足かも知れないためその際はご指摘ください。
実現有無はともかく、GUI と比較した際に気になった点です。
:::

投稿済みの記事が public/配下に Markdown 形式で取得できたので見てみると...
![Screenshot from 2023-09-05 20-25-18.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/721141d6-0364-d968-5d52-d25c387c8468.png)
ファイル名...
`id.md`で取れてくるようです。
`タイトル.md`で取れないのでしょうか？ とても参照がしにくいです。

---

Preview が保存しないと反映されない。

```bash
npx qiita preview
```

![Screenshot from 2023-09-05 21-40-15.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/27345cb9-0bcc-ea98-9222-f8d9760acfaf.png)

![Screenshot from 2023-09-05 21-40-55.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/5335ded8-fc05-ae33-8ee6-d9d86db95186.png)

書きながらデザインを確認できる点も気に入っていました。

---

テンプレートはありますが`.md`形式のため、GUI でタブ補完できていたタグ指定が面倒です。

```Markdown
tags:
  - 'Qiita'
  - 'CLI'
  - 'Node.js'
  - 'npm'
  - 'QiitaCLI'
```

これ重宝していました。
![Screenshot from 2023-09-05 21-36-23.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/e5dcc86c-5163-88fd-d983-f02a212e65c5.png)

---

画像の反映に 1 手間かかります。

GUI では画像をドラッグ&ドロップで反映させられました。
CLI では Preview から画像アイコンをクリックします。

![Screenshot from 2023-09-05 21-44-03.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/82be410a-f8d6-dd94-20f8-9bd590b6eb3f.png)

ファイルをアップロードする。

![Screenshot from 2023-09-05 21-45-10.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/6f6b64de-25d3-fe43-1d66-c14e681b5461.png)

URL をコピーして、貼り付ける。
といった流れで 2 手間くらいかかります。

## 投稿

以上が触ってみた感想です。

一旦コマンドから投稿します。

```bash
npx qiita publish 記事のファイルのベース名
```

### 最後に

最後まで閲覧頂きありがとうございました。

個人的にはとても便利な反面、痒いところに手が届かない印象でした。
投稿済みの md ファイルが`タイトル.md`で取得できれば他は慣れれば良いかな、といった印象です。

投稿自体は`タイトル.md`で可能なので、何かしらの手法で投稿済みのファイル名は直そうと思います。

今回はお試しでしたが、今後も Qiita CLI を使ってみて印象が変わりましたら共有します。[^3]
[^3]:書いていて思いましたが、Preview がダークモード対応していないの（目が）辛いです。

本記事がお役に立てば幸いです！

### 参考 URL

https://github.com/increments/qiita-cli

https://qiita.com/Qiita/items/666e190490d0af90a92b#qiita-cli%E3%81%A8%E3%81%AF
