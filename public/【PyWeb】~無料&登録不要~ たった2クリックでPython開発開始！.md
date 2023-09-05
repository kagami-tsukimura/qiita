---
title: 【PyWeb】~無料&登録不要~ たった2クリックでPython開発開始！
tags:
  - Python
  - Web
  - Cloud
  - 初心者
  - 機械学習
private: false
updated_at: '2023-06-18T21:11:06+09:00'
id: 6c2dba79906fe85d79e2
organization_url_name: null
slide: false
---
# お忙しい方へ
1.__⬇⬇⬇クリック⬇⬇⬇__

https://pyweb.ayax.jp/PyWeb.html

2.__`利用規約に同意し開始`クリック！__
![Screenshot from 2023-06-16 22-41-33.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/c95a5371-0962-09e8-7dd0-4cfd2876817d.png)

3.__Hello, World!__
![Screenshot from 2023-06-16 22-49-56.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/2e3e7b19-b2ec-10e7-5fc5-b82f9e3de574.png)
![Screenshot from 2023-06-16 22-49-57.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/95b24dda-5d6f-7099-d96c-bbd0afbdeb61.png)

(利用規約に同意......下から1クリックでいけます笑)

https://pyweb.ayax.jp/index.php

# Introduction

6/15(木)に`PyWeb`がリリースされました。
![Screenshot from 2023-06-16 22-55-16.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/76ac20b0-9f92-279f-3f28-255f1f2975b8.png)
実は2月にβ版が公開されており、満を持してVer1.0正式版公開になりました。
`PyWeb`は会員登録、インストール、費用不要でPython実行環境を提供するサービスです。
類似のサービスとして最も有名なのはNotebook形式でGPUも使える`Google Colaboratory`でしょうか。
日本語UIですぐ使えるものは`paiza.io`が提供しています。

https://paiza.io/ja/projects/new

他にも英語UIのものや公式が提供しているものなどが幾つかあります。

上記と比べて`PyWeb`は子どもや初学者に大変優しい作りになっており、初学者向けの勉強会やGIGAスクール構想等で授業に用いるのに適していると感じました。

※本記事ではWindowsもしくはChromebook(Linux制限)で、Python実行に環境構築が必要な視点で紹介します。
理由はGIGAスクール端末について以下のような記事があったためです。
大変参考になりました。

https://qiita.com/harryp0tterK/items/8ff14cdb1218cd2c97a6

__本記事が少しでも読者様の学びに繋がれば幸いです！__
__「いいね」をしていただけると今後の励みになるので、是非お願いします！__


## PyWebとは

`PyWeb`はayaxが提供する、Web上でPythonを実行できるサービスです。[^1]
[^1]: ayaxについては下記リンクを参照ください。
https://pyweb.ayax.jp/developer.html

対象は小中学生や初学者で、左記を対象にした学習テキストも提供されています。
プログラミング初学者にとって入口に立つ前の挫折ポイントである、環境構築の障壁を取り払ったサービスで、思い立った時にすぐプログラミングをスタートできます。

機能は以下の通りです。
![Screenshot from 2023-06-16 23-29-38.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/8631ed25-e979-3ad9-200f-f93689a803fd.png)
ハードルが高そうなエラー発生に対するフォローがなされていてコンセプトへの一貫性を感じられます。
クラウドとの連携ができるのも好印象です。類似のものでもクラウド対応不可のサービスが多い印象です。

一部制限もあります。
![Screenshot from 2023-06-16 23-29-38 (コピー).png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/86e7fc14-b1b9-24f2-12f4-7ec56c7d9dd1.png)
気になるのは2時間の利用制限とGUIに制限があることですかね。
個人的には上記が気になるレベルになれば環境構築して本格的に学んだ方が良いと思います。


## PyWebをお試し
`PyWeb`の構成は左・中央・右の3ペインになっています。
左にファイル、中央にコード入力、右に出力です。
![Screenshot from 2023-06-16 22-49-56.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/2e3e7b19-b2ec-10e7-5fc5-b82f9e3de574.png)

左ペインにチュートリアルとサンプルのファイルがあり、何を書いたら良いかわからない人も安心です。

チュートリアルではカメさんが荒ぶってくれます。
ブラウザによっては以下のようなエラーが出るかもしれません。
Chromeでは問題なく動作したのでChrome使いましょう。[^2]
[^2]: ホームページに`※一部機能はブラウザ(Firefox,Safari等）によって動作しません。`とあります。FirefoxとBraveではエラーが発生してカメが動きませんでした。
```bash:
Python ModuleNotFoundError: No module named 'pykame'
```


![output.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/8856c8ca-7123-3b5f-6979-e5987f8b9354.gif)

Pythonの基礎も学べます。
図を使って視覚的に理解でき分かりやすいです。
![Screenshot from 2023-06-17 00-00-54.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/107022f0-04cf-6bb2-b92b-51d9e87b1955.png)
初学者は下手な書籍やサイトから入るよりハードル低いのではないでしょうか。
Pythonは公式チュートリアルも不親切なので今後初学者には`PyWeb`を勧め用途思っています。

画像検出やWebスクレイピング、機械学習もありました。
充実度合いは半端ではないです。
![Screenshot from 2023-06-17 00-11-57.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/5ac8d328-8f2f-47c7-f149-7b52906123a7.png)
画像検出はOpenCVで行うので問題ないですが、機械学習は少し重たかったのでGIGAスクール端末でどこまで動くかは怪しいです。
scikit-learnで手書数字分類するだけですが、読み込み中の表示がないと壊れたのかと思われるかもしれませんね。

### 最後に

Pythonの基礎の基礎、ひいてはプログラミングの入口は`PyWeb`で良い気がしました。
`PyWeb`で一通りプログラミングの概念を学んで、ゲームっぽいのが良ければ[Scratch](https://scratch.mit.edu/studios/1168062 "Scratch")がおすすめです。
物足りなくなったらPythonの環境構築をしてVSCodeやPyCharmを入れるか ~~(勿論Vimでも)~~ Google Colaboratryに進むのも良いかと思います。
特に[Scratch](https://scratch.mit.edu/studios/1168062 "Scratch")はビジュアルプログラミングでとっつきやすく、アカウント不要で作品の共有も可能なため`PyWeb`との親和性も強いです。

`PyWeb`は総じてプログラミングの楽しい部分を全面に押し出したサービスでした。
挫折しそうな環境構築やエラーの解析を省いた上で、ソースコードには触れるためプログラムへの抵抗感を上手いことなくしてくれるのではないかと思います。

仕事で使っていると忘れがちですが、元来プログラミングは楽しいものです。 ~~(多分)~~

個人的にはPython初学者向けに勉強会を開く際、お試しで使ってみる予定です。
今まではGoogle Colaboratoryを使っていましたが、慣れなくて上手く扱えない方もいました。
PCに慣れない方でも`PyWeb`ならすぐに扱えそうなので、上記が改善できるか楽しみです。

簡単な紹介になりましたが、皆様も是非お試しください。
特に駆け出しの方や、その指導をされる方は一度触れておいて損はないサービスだと思います。

### 参考URL

https://pyweb.ayax.jp/PyWeb.html



