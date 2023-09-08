---
title: 【rembg】 背景除去の精度とコードのシンプルさが魅力
tags:
  - Python
  - 初心者
  - 機械学習
  - GoogleColaboratory
  - rembg
private: false
updated_at: '2023-09-09T08:33:36+09:00'
id: 1270ac49f6099b736c03
organization_url_name: null
slide: false
---

# Introduction

以前`Google`等のブラウザから簡単に画像をクロールするスクリプトを紹介しました。

https://qiita.com/kagami_t/items/22e8c5eb95ee17a2353c

ここでクローリングしてきた画像から背景除去してオブジェクトを取り出し、合成素材にする手法を試していきます。

使い道としては、`画像分類`や`物体検知`においてデータセットが不足している場合です。
現実で珍しいものやネットにないものは合成で無理矢理作ってデータセットを補強できます。
データ収集は機械学習パイプラインにおいて工数泥棒なので、特に機械学習のプロジェクトでは覚えておくと重宝します。

機械学習に限定せずとも、画像合成で Web サイトやヘッダー画像を作成したり汎用的に楽しめます。

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu22.04
Python3.11.1

## rembg とは

背景除去には幾つか手段が有りますが、`rembg`が汎用的で使用感も良好でした。

https://github.com/danielgatis/rembg

`rembg`では以下の処理を行います。

- `u2net`でオブジェクトのセグメンテーション。
- `pymatting`でセグメンテーションを用いて背景とオブジェクトを分離。

短いコードで背景除去でき、様々なジャンルや低画質でもある程度の精度を誇っています。

ただ欠点もあります。主に以下の 2 点です。

- `pip`限定で`conda install`ができないため、`conda`環境では使用が難しい。 - ※`conda`に`pip`を混ぜると壊れます。
  ~~- `python: >3.7, <3.11`とバージョン範囲が狭い。~~
  ~~- 私は`Python3.11.1`に上げてしまい、やむを得ず`Google Colab`で動かしました。~~ - 2023 年 6 月 7 日現在、`python: >3.7, <3.12`に対応されています。
  `Python3.11`にも無事インストールできました。

`conda`で環境管理、`Python`のバージョン指定はどちらも業務でありがちな内容のため注意が必要です。

上記の理由から`OpenCV`で自作したり`Image Matting`を紹介したかったのですが、工数や精度、汎用性で劣ってしまいます。

[Image Matting の参考](https://arxiv.org/pdf/1908.00672.pdf 'Image Matting')

## 実装

前述の通りバージョン問題で`Python`ファイルで実行できないため、`Google Colab`で作成した`rembg.ipynb`を`Python`ファイルに書き出しました。
そのままは使えませんが、コード自体は単調なので参考にしていただけると幸いです。

コードは`GitHub`にもあげてあります。

https://github.com/kagami-tsukimura/create-movie/tree/main/remove_bg

`icrawler`同様、こちらも 3 行で背景除去できますので抜粋します。

```python: rembg.py
  # 画像の読み込み
  input_img = cv2.imread(img)
  # 背景除去
  output_img = remove(input_img)
  # png変換して保存(透明度（アルファ値）は`png`にしないと変えられない)
  cv2.imwrite(img.replace(INPUT,OUTPUT).replace('jpg', 'png').replace('jpeg', 'png'), output_img)
```

元画像のペンギンさん。

![Screenshot from 2023-05-13 01-55-16.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/f79dc3b3-24dc-09df-cf1c-d5097469971e.png)

背景除去をしたペンギンさん。

![Screenshot from 2023-05-13 02-02-48.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/e6676202-8b27-a4aa-ea6a-632995eb17e5.png)

複数羽いても、綺麗に背景だけ除去されています。
これくらいはっきりした画像であれば自作や他モデルでも除去できそうですが、最低 3 行で背景除去できるのは大きな魅力です。

`Gaussian Blur`で画像をぼかして試してみます。
実際のデータセットは画質が良いとは限らないので、汎用性は重要です。

```python: rembg.py
# Gaussian Blur
imgs = glob(f'{IN_BLUR}/*')
for i, img in enumerate(imgs):
  input_img = cv2.imread(img)
  img_blur = cv2.GaussianBlur(input_img,     # 入力画像
                               (9,9),    # カーネルの縦/横
                                10,10        # 標準偏差
                            )
  cv2.imwrite(f'{IN_BLUR}/{i}.png', img_blur)
```

元画像のぼかしペンギンさん。

![Screenshot from 2023-05-13 02-08-06.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/5222048f-fac2-7c92-53c2-bb24d360ed0f.png)

背景除去をしたぼかしペンギンさん。

![Screenshot from 2023-05-13 02-12-11.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/bc1c0aec-8a32-e655-ef1e-05ba36725b04.png)

`Gaussian Blur`も関係なしに背景除去されています。
ぼかした際は自作するとチューニングが難しかったり、他モデルでは背景除去できなくなることがありますが、`rembg`は条件にあまり左右されず精度が良い印象です。

それ故に、前述した業務で使い辛い特性が勿体ないですね。

## 最後に

最後まで閲覧頂きありがとうございました。
備忘録の側面もありますが、本記事がお役に立てば幸いです！

### 参考 URL

https://github.com/danielgatis/rembg

[Image Matting の参考](https://arxiv.org/pdf/1908.00672.pdf 'Image Matting')

[Alpha Matting 技術「HAttMatting」](https://shiropen.com/2020/07/04/53618/ 'HAttMatting')
