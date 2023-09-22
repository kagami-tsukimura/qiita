---
title: 【imgsim】画像の類似度をPythonで手早く測りたい
tags:
  - Python
  - 機械学習
  - 画像認識
  - imgsim
private: false
updated_at: '2023-09-09T08:33:36+09:00'
id: a1cae07c9565ce501ced
organization_url_name: null
slide: false
ignorePublish: false
---

# Introduction

データセットを追加してリネームを繰り返していると、同一画像が複数枚混ざっていることに気が付きました。

効率的に取り除く方法として`imgsim`というライブラリを用いて画像の類似度を測定しました。

過学習の原因となる同一画像の削除、似たような画像の分類等に役立てられます。

**※詳細は下記 GitHub の方でご確認ください。**

https://github.com/chenmingxiang110/AugNet

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu22.04
Python3.11.1

## imgsim とは

異なる画像の特徴ベクトル間の距離や類似度の差を計算します。
`AugNet`というディープラーニング学習パラダイムを用います。
差が 0 なら同一画像、値が大きくなるほど特徴量の異なる画像です。

## AugNet とは

教師なし学習を使用して、画像の表現学習を行うための手法です。
data augmentation を用いて拡張した画像間の差を測定します。

## 実装

1. ライブラリをインストールします。

   ```bash:
   pip install imgsim
   ```

   `conda`にはなかったため、`pip`でインストールしてください。
   **※安易に混ぜるのは危険です。[^1]**

   [^1]:
       **混ぜるならせめて「Best Practices Checklist」は確認を。**
       [anaconda.com|Best Practices Checklist](https://www.anaconda.com/blog/using-pip-in-a-conda-environment 'Best Practices Checklist')

1. 画像を用意します。

   <img width="128" alt="penguin_lr" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/8001fdf8-84df-a588-5ab2-ad7357de4297.jpeg"><img width="128" alt="penguin" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/e39dadb5-fc18-b615-827d-453baa59c606.jpeg">
   <img width="128" alt="another_penguin" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/ca782e55-a3c6-39ab-8ae1-a19afcb8354e.jpeg"><img width="128" alt="hummingbird" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/6937560d-1637-a4c9-e468-fdebd033f913.png">

1. 類似度を測定します。
   [GitHub](https://github.com/chenmingxiang110/AugNet"GitHub")のサンプルコードを基にお試しします。

<details><summary><b>=======================<br>&nbsp;&nbsp;&nbsp;サンプルコード<br>&nbsp;========================</b></summary><div>

```python: measure_distance.py
import imgsim
import cv2


vtr = imgsim.Vectorizer()

penguin_img = cv2.imread("./input/king_penguin_00001.jpg")
penguin_lr_img = cv2.imread("./input/king_penguin_00001_flip_lr.jpg")
another_penguin_img = cv2.imread("./input/king_penguin_00019.jpg")
hummingbird_img = cv2.imread("./input/hummingbird_00010.png")


penguin_vec = vtr.vectorize(penguin_img)
penguin_lr_vec = vtr.vectorize(penguin_lr_img)
another_penguin_vec = vtr.vectorize(another_penguin_img)
hummingbird_vec = vtr.vectorize(hummingbird_img)

dist0 = imgsim.distance(penguin_vec, penguin_vec)
print("Same Distance =", round(dist0, 2))
dist1 = imgsim.distance(penguin_vec, penguin_lr_vec)
print("Reversal Distance =", round(dist1, 2))
dist2 = imgsim.distance(penguin_vec, another_penguin_vec)
print("Another Distance =", round(dist2, 2))
dist3 = imgsim.distance(penguin_vec, hummingbird_vec)
print("Other Distance =", round(dist3, 2))
```

</div></details>

以下のような実行結果になりました。

```bash:
Same Distance = 0.0
Reversal Distance = 11.53
Another Distance = 26.67
Other Distance = 32.2
```

結果を表にすると以下のようになります。

| Base                                                                                                                                                | Target                                                                                                                                                      | Distance  |
| --------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| <img width="128" alt="penguin" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/e39dadb5-fc18-b615-827d-453baa59c606.jpeg"> | <img width="128" alt="penguin" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/e39dadb5-fc18-b615-827d-453baa59c606.jpeg">         | **0.0**   |
| <img width="128" alt="penguin" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/e39dadb5-fc18-b615-827d-453baa59c606.jpeg"> | <img width="128" alt="penguin_lr" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/8001fdf8-84df-a588-5ab2-ad7357de4297.jpeg">      | **11.53** |
| <img width="128" alt="penguin" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/e39dadb5-fc18-b615-827d-453baa59c606.jpeg"> | <img width="128" alt="another_penguin" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/ca782e55-a3c6-39ab-8ae1-a19afcb8354e.jpeg"> | **26.67** |
| <img width="128" alt="penguin" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/e39dadb5-fc18-b615-827d-453baa59c606.jpeg"> | <img width="128" alt="hummingbird" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/6937560d-1637-a4c9-e468-fdebd033f913.png">      | **32.2**  |

結果をまとめます。

- 同一の画像は差が 0 である。
- 左右反転は差が小さい。
- 似たような画像より異なる画像の方が差が大きい。

当たり前の結果に思えますが、この 4 枚に対しては信頼できる結果でした。

## 最後に

閲覧頂きありがとうございました。

実はサンプルコードに少々難があったのですが、データセットの整理には何かしら役立てられそうでした。

類似度は他の手法でも測れるため、比較してみても面白いかも知れません。

本記事がお役に立てば幸いです！

## 参考 URL

https://github.com/chenmingxiang110/AugNet
