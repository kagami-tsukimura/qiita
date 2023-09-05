---
title: 【OpenCV】画像の差分を取得するとサンリオ間違い探しが高速クリアできる！
tags:
  - Python
  - 画像処理
  - OpenCV
  - Python3
private: false
updated_at: '2023-08-02T21:42:34+09:00'
id: 2b4db4e2464439a48fb4
organization_url_name: null
slide: false
---
# Introduction

画像の類似度を測る[imgsim](https://qiita.com/kagami_t/items/a1cae07c9565ce501ced)の記事を投稿したところ、画面の差分を確認できないかとリクエストいただきました。

https://qiita.com/kagami_t/items/a1cae07c9565ce501ced

`imgsim`は特徴ベクトルから画像間の距離を測るもので、差分検出とは異なります。
画像の差分は`OpenCV`による検出が分かりやすいため、`imgsim`の記事と同様に手早く差分を取得して比較するスクリプトを紹介します。[^1]
[^1]: ノイズ等も考慮して正確に取得するのであれば`SIFT`, `AKAZE`等の特徴点抽出や、機械学習を用いる必要があります。

本記事では基本編で、簡単な画像差分を検出して比較します。
勿論アプリやweb画面にも応用可能です。

そして応用編で、難しいと話題になっているサンリオの間違い探しを差分検出で高速クリアしてみます。
(間違い探しクリアって誰得? と思ったのですが、非エンジニアの知人にこの話をしたところCNNよりずっと食いつきが良かったです...)

https://twitter.com/hapidanbui/status/1669223213199503360

© 2023 SANRIO CO., LTD. / Via Twitter: @hapidanbui

[基本編](#基本編)
[応用編](#応用編)

__本記事が少しでも読者様の学びに繋がれば幸いです！__
__「いいね」をしていただけると今後の励みになるので、是非お願いします！__

## 環境
Ubuntu22.04
Python3.11

## 実装

先に結論として出力に使用したソースコードを紹介します。
解説はコメントアウトで詳細に書きました。
`OpenCV`に馴染みのない方は参考にしてください。

<details><summary><b>=======================<br>&nbsp;&nbsp;&nbsp;サンプルコード<br>&nbsp;========================</b></summary><div>

```python: 
import cv2
import matplotlib.pyplot as plt
import numpy as np


def compare_images(image1_path, image2_path):
    """
    2つの画像を比較し、差分を表示する関数。

    Args:
        image1_path (str): 1つ目の画像のパス
        image2_path (str): 2つ目の画像のパス
    """

    # 画像を読み込む
    image1 = cv2.imread(image1_path)
    image2 = cv2.imread(image2_path)

    # 差分画像を計算
    diff = cv2.absdiff(image1, image2)

    # グレースケールに変換
    gray2 = cv2.cvtColor(image2, cv2.COLOR_BGR2GRAY)
    gray_diff = cv2.cvtColor(diff, cv2.COLOR_BGR2GRAY)

    # BGRからRGBに変換
    image1_rgb = cv2.cvtColor(image1, cv2.COLOR_BGR2RGB)
    image2_rgb = cv2.cvtColor(image2, cv2.COLOR_BGR2RGB)

    # カラーマップを適用するために差分画像を正規化
    norm_diff = gray_diff / np.max(gray_diff)

    # 差分画像に重みをかけて2枚目の画像の色に反映
    diff_img = cv2.addWeighted(gray2, 0.1, gray_diff, 2, 100)

    diff_colored = np.zeros_like(image2_rgb)
    diff_colored[..., 0] = image2_rgb[..., 0] * norm_diff
    diff_colored[..., 1] = image2_rgb[..., 1] * norm_diff
    diff_colored[..., 2] = image2_rgb[..., 2] * norm_diff

    # 結果をMatplotlibで表示
    fig, axes = plt.subplots(2, 2, figsize=(10, 10))

    # 1枚目の画像を表示
    axes[0, 0].imshow(image1_rgb)
    axes[0, 0].set_title("Image 1")
    axes[0, 0].axis("off")

    # 2枚目の画像を表示
    axes[0, 1].imshow(image2_rgb)
    axes[0, 1].set_title("Image 2")
    axes[0, 1].axis("off")

    # 差分画像（グレースケール）を表示
    axes[1, 0].imshow(diff_img, cmap="gray")
    axes[1, 0].set_title("Difference (Grayscale)")
    axes[1, 0].axis("off")

    # 差分画像（カラー）を表示
    axes[1, 1].imshow(diff_colored)
    axes[1, 1].set_title("Difference (Colored)")
    axes[1, 1].axis("off")

    plt.tight_layout()
    plt.show()


if __name__ == "__main__":
    # 2つの画像を比較して違いを検出
    image1_path = "./Pictures/p.png"
    image2_path = "./Pictures/output.png"
    compare_images(image1_path, image2_path)

```
</div></details>

実行すると、2☓2マスに以下の画像を出力します。
- 1行1列目: 変数`image1_path`で指定した1枚目の画像を出力。
- 1行2列目: 変数`image2_path`で指定した2枚目の画像を出力。
- 2行1列目: 1枚目と2枚目の差分を白、それ以外を白黒で出力。
- 2行2列目: 1枚目と2枚目の差分をカラーで出力。

それでは出力内容を見ていきましょう。

## 基本編

[実装したスクリプト](#実装)で私のペンギンアイコンから画像差分を検出します。

1. 線の有無を検出
1枚目に私のアイコン、2枚目に横線を一本引いた画像を用意します。
線は`OpenCV`を使うと簡単に引けます。
![2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/348cdfc2-67b6-229e-7f48-e8de571f5839.png)
横線が浮かび上がりました。
差分が視覚的で良い感じです。

1. 色違いを検出
2枚目と同じ画像を用いて、片方だけ帽子の色を赤くしました。
急にサンタさん感が増します。
![Screenshot from 2023-06-23 21-49-32.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/ce4e1d5d-078b-3efd-d4d1-23a566149386.png)
色の変化した帽子が浮かび上がりました。
線だけでなく、色の差分もしっかり検出できています。

## 応用編

今回の間違い探しで用いる画像は以下の2枚です。
間違いは10ヶ所あります。

![Screenshot from 2023-06-22 22-48-57.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/48f0e654-e674-f586-73f7-416fd4bc5bf5.png)
© 2023 SANRIO CO., LTD. / Via Twitter: @hapidanbui

10ヶ所全部見つかりましたか？
試しに目視でやりましたが8ヶ所で目が疲れてきました...

[実装したスクリプト](#実装)で実行しちゃいましょう。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/18e17536-bbff-8497-09ec-1a00424ecadf.png)
© 2023 SANRIO CO., LTD. / Via Twitter: @hapidanbui

何とか10ヶ所すべて出力できました！
右下の木目が若干見辛いですが、これ以上はノイズが乗ってくるので検出できただけ良しとします。

簡単な間違い探しやWeb画面の差分であればもっと分かりやすいため十分機能を果たせています。

## おまけ

[実装したスクリプト](#実装)に至るまでの過程を記載します。

### 差分検出

まずは簡単に差分を検出しました。
1枚目と2枚目の差分を3枚目で出力します。

<details><summary><b>=======================<br>&nbsp;&nbsp;&nbsp;差分検出サンプル<br>&nbsp;========================</b></summary><div>

```python: 
import cv2
import matplotlib.pyplot as plt


def compare_images(image1_path, image2_path):

    image1 = cv2.imread(image1_path)
    image2 = cv2.imread(image2_path)

    diff = cv2.absdiff(image1, image2)
    gray_diff = cv2.cvtColor(diff, cv2.COLOR_BGR2GRAY)

    image1_rgb = cv2.cvtColor(image1, cv2.COLOR_BGR2RGB)
    image2_rgb = cv2.cvtColor(image2, cv2.COLOR_BGR2RGB)

    fig, axes = plt.subplots(2, 2, figsize=(10, 10))

    axes[0, 0].imshow(image1_rgb)
    axes[0, 0].set_title("Image 1")
    axes[0, 0].axis("off")

    axes[0, 1].imshow(image2_rgb)
    axes[0, 1].set_title("Image 2")
    axes[0, 1].axis("off")

    axes[1, 0].imshow(gray_diff, cmap="gray")
    axes[1, 0].set_title("Difference")
    axes[1, 0].axis("off")

    axes[1, 1].axis("off")

    plt.tight_layout()
    plt.show()


image1_path = "./Pictures/1.png"  
image2_path = "./Pictures/2.png"  
compare_images(image1_path, image2_path)

```
</div></details>

![Screenshot from 2023-06-22 22-15-12.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/94955df9-357d-7d9f-f321-e9a1bc68d2df.png)
© 2023 SANRIO CO., LTD. / Via Twitter: @hapidanbui

記事作成前の完成イメージはこれでしたが、目視で追うのが少々面倒です。

### 矩形出力

<details><summary><b>=======================<br>&nbsp;&nbsp;&nbsp;矩形サンプル<br>&nbsp;========================</b></summary><div>

```python: 
import cv2
import matplotlib.pyplot as plt
import numpy as np


def compare_images(image1_path, image2_path):

    image1 = cv2.imread(image1_path)
    image2 = cv2.imread(image2_path)

    diff = cv2.absdiff(image1, image2)
    gray_diff = cv2.cvtColor(diff, cv2.COLOR_BGR2GRAY)

    image1_rgb = cv2.cvtColor(image1, cv2.COLOR_BGR2RGB)
    image2_rgb = cv2.cvtColor(image2, cv2.COLOR_BGR2RGB)

    norm_diff = gray_diff / np.max(gray_diff)

    image2_diff = image2_rgb.copy()
    contours, _ = cv2.findContours(
        gray_diff, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE
    )
    for contour in contours:
        x, y, w, h = cv2.boundingRect(contour)
        cv2.rectangle(image2_diff, (x, y), (x + w, y + h), (255, 0, 0), 2)

    fig, axes = plt.subplots(2, 2, figsize=(10, 10))

    axes[0, 0].imshow(image1_rgb)
    axes[0, 0].set_title("Image 1")
    axes[0, 0].axis("off")

    axes[0, 1].imshow(image2_rgb)
    axes[0, 1].set_title("Image 2")
    axes[0, 1].axis("off")

    axes[1, 0].imshow(gray_diff, cmap="gray")
    axes[1, 0].set_title("Difference (Grayscale)")
    axes[1, 0].axis("off")

    axes[1, 1].imshow(image2_diff)
    axes[1, 1].set_title("Difference with Rectangles")
    axes[1, 1].axis("off")

    plt.tight_layout()
    plt.show()


image1_path = "./Pictures/1.png"
image2_path = "./Pictures/2.png"
compare_images(image1_path, image2_path)

```
</div></details>

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/cbeb1e4c-7ab9-8958-8bb1-8b5c6119891f.png)

何か思っていたのと違う...
小さすぎる差分はまとめるようにします。

### 矩形出力（差分のみ）

```python: 
    for contour in contours:
        area = cv2.contourArea(contour)
        # 面積が一定以上の場合にのみ矩形を描画
        if area > 100:
            x, y, w, h = cv2.boundingRect(contour)
            cv2.rectangle(image2_diff, (x, y), (x + w, y + h), (255, 0, 0), 2)

```

- 出力結果
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/7eec37f5-8192-6427-ccb6-377ceb8996da.png)
© 2023 SANRIO CO., LTD. / Via Twitter: @hapidanbui

簡単な箇所は検出できています。
基本編の差分であれば十分な気もしますが、悔しいので続けました。
とはいえ単純な手法で矩形出力は厳しことが分かり、[実装したスクリプト](#実装)のように差分を浮かび上がらせて完成させました。


### 最後に


`OpenCV×Python`は良い教材が少ないので、私は`Udemy`で勉強しました。

https://www.udemy.com/course/pythonopencv/

[Introduction](#introduction)にも記載した`SIFT`, `AKAZE`等の特徴点抽出についても触れており、大変勉強になりました。
`OpenCV`の諸機能を淡々と進められるので、`Python`を抵抗なく書ける程度の知識がある方におすすめです。

最後まで閲覧頂きありがとうございました。
本記事がお役に立てば幸いです！


### 参考URL

https://peaceandhilightandpython.hatenablog.com/entry/2016/01/16/002028

