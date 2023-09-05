---
title: 【npm】ブラウザ内で高精度に背景除去できる background-removal-js を試す
tags:
  - JavaScript
  - GitHub
  - npm
  - TypeScript
private: false
updated_at: '2023-07-01T17:55:45+09:00'
id: d7ca7bd9337209df7edf
organization_url_name: null
slide: false
---
# Introduction

`background-removal-js`というブラウザで簡単に使える背景除去ライブラリがお手軽で良かったので紹介します。

デモ画面が用意されているため、こちらを用いて精度を試していきます。

https://img.ly/showcases/cesdk/web/background-removal/web

私は普段[rembg](https://qiita.com/kagami_t/items/1270ac49f6099b736c03)で画像背景を除去しているので、以下の記事で試した画像を用いて簡単に比較します。

https://qiita.com/kagami_t/items/1270ac49f6099b736c03

__本記事が少しでも読者様の学びに繋がれば幸いです！__
__「いいね」をしていただけると今後の励みになるので、是非お願いします！__

## 環境
Ubuntu22.04

## imgly/background-removal-js

https://github.com/imgly/background-removal-js

`background-removal-js`はブラウザで簡単に背景除去ができるライブラリです。
サーバー不要で外部とデータをやり取りしないためセキュアに使えます。
`npm`で提供されており、READMEも整備されてサンプルコードもあるためwebアプリへの組み込みは容易そうです。

中身としては`WASM`で`ONIX`形式のモデルを扱っているようです。

>Download Size vs Quality
The onnx model is shipped in various sizes and needs.
small (~40 MB) is the smallest model and is in most cases working fine but sometimes shows some artifacts. It's a quantized model.
medium (~80MB) is the default model.

品質とのトレードオフになりますが、より軽量なモデルも選択できるようです。

## 実験

[rembg](https://qiita.com/kagami_t/items/1270ac49f6099b736c03)で用いた以下の画像で精度を確認していきます。
    <img width="320" alt="penguin_1" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/24dfb079-9f78-adfc-869d-01f2c5b5b3a1.jpeg"><img width="320" alt="penguin_2" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/9981078e-d2ac-477e-70c8-9b248d120a20.jpeg">

- デモ画面にアクセスします。

https://img.ly/showcases/cesdk/web/background-removal/web

- 画面下部のサンプル画像を選択すればすぐにお試しできます。
今回は画像を指定したいため、`Upload Image`で画像を選択します。
背景除去が開始されます。

![Screenshot from 2023-07-01 17-07-42.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/fbb0a020-2372-6a1c-9097-fe340d3f9a28.png)

10秒ほどで結果が出力されました。

![Screenshot from 2023-07-01 17-34-49.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/2abc5810-ba02-5f6a-d0ce-6a28356d8f37.png)


右のペンギンや足元など、完璧ではありませんが綺麗に背景除去されています。

`Edit in CE.SDK`をクリックすると`Creative Editor SDK`画面に遷移されます。
文字や図形、Blurなどで画像を編集した上で画像ファイルやPDFとして保存できます。[^1]
[^1]:初めて`Creative Editor SDK`使いましたが、下手な編集ソフトより癖がなく好みでした。

![Screenshot from 2023-07-01 17-38-11.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/bd59a5c7-d36f-1ab6-ed8e-43d96a7d17d4.png)

最後に`rembg`との比較結果です。

|original|
|---|
|<img alt="penguin_1_original" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/24dfb079-9f78-adfc-869d-01f2c5b5b3a1.jpeg">|
|rembg|
|<img alt="penguin_1_rembg" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/615f4a32-bed2-7345-2d08-27a0f258e34b.png">|<img width="128" alt="penguin" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/e39dadb5-fc18-b615-827d-453baa59c606.jpeg">
|background-removal-js|
|<img alt="penguin_1_background-removal" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/ae3fd332-bd68-1a61-89da-9bddb1df8b17.png">|<img width="128" alt="penguin_lr" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/ae3fd332-bd68-1a61-89da-9bddb1df8b17.png">
|original|
|<img alt="penguin_1_original" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/9981078e-d2ac-477e-70c8-9b248d120a20.jpeg">|
|rembg|
|<img alt="penguin_1_rembg" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/a0081fda-797e-210b-8170-9ef4290f05a8.png">|
|background-removal-js|
|<img alt="penguin_1_background-removal" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/5e698615-64ef-9c2c-0f37-4794e46e5219.png">|

比較すると`rembg`は境界線が輪郭のように残っていて、`background-removal-js`は境界線も綺麗です。
私の調べた限り、従来では`rembg`が使いやすさと精度で圧倒的に優れていましたが、`background-removal-js`もかなり高精度で好印象でした。
少なくとも`OpenCV`や`Semantic Segmentation`やら`Image Matting`よりはずっと良いですね。パラメータ調整や、人以外はうまく除去できなくて悩んでいた時代は過去になりました。

### 最後に



最後まで閲覧頂きありがとうございました。
本記事がお役に立てば幸いです！


### 参考URL

https://github.com/imgly/background-removal-js

https://onnx.ai/

https://gigazine.net/news/20230630-background-removal-js/

