---
title: 【Vim】複数行に一括でコメントアウト
tags:
  - Vim
private: false
updated_at: '2023-04-25T20:56:42+09:00'
id: c8e81ff2bf3adb102cda
organization_url_name: null
slide: false
---

# Introduction

`Vim`で複数行をコメントアウトする際に、挿入モードで 1 行ずつ行っていた方がいたので共有します。

以前作成した fizzbuzz のコードを例にしていきます。

```python: fizzbuzz.py
import argparse


def fizzbuzz(start, end):
    for num in range(start, end + 1):
        if num % 3 == 0 and num % 5 == 0:
            print("FizzBuzz")
        elif num % 3 == 0:
            print("Fizz")
        elif num % 5 == 0:
            print("Buzz")
        else:
            print(num)


if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="FizzBuzz")
    parser.add_argument(
        "--start",
        "-s",
        type=int,
        default=1,
        help="Starting number(default:1)",
    )
    parser.add_argument(
        "--end",
        "-e",
        type=int,
        default=15,
        help="Ending number(default:15)",
    )
    args = parser.parse_args()

    fizzbuzz(args.start, args.end)
```

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu22.04

## お忙しい方へ

1. `Ctrl` + `v`。
1. `k`キーでコメントアウト範囲を選択。
1. `Shift` + `i`
1. `#`等を入力。
1. `Esc`キーで抜ける。

## 1 行のコメントアウト

1 行の場合は`i`キーで挿入モードにし、コメントアウトしたい行の頭で`#`等を入力します。
削除する場合は通常モードで`x`を入力します。
ちなみに`Vim`では`0`キーで行の頭に移動できます。

![Screenshot from 2023-04-25 20-25-20.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/84545765-c46a-8c46-8a23-5c578697f3b0.png)

## 複数行のコメントアウト

複数行のコメントアウトは以下の手順を行います。

1. `0`キーで行の頭に移動します。
1. `Ctrl` + `v`で矩形ビジュアルモードにします。
   ![Screenshot from 2023-04-25 20-47-38.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/c460ea2f-c79e-069d-e014-eda06bddfd50.png)
1. `k`キーで下に移動し、コメントアウトしたい行の範囲まで選択します。
   ![Screenshot from 2023-04-25 20-49-26.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/b3a246cb-1413-eee2-18d5-f241fe96e025.png)
1. `Shift` + `i`で挿入モードにすると選択範囲の 1 行目に遷移するので、`#`等を入力します。
   入力は 1 行だけで大丈夫です。
   ![Screenshot from 2023-04-25 20-50-27.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/61a7a60f-bd3b-1b52-1ce6-3bac10a3853f.png)
1. `Esc`キーで抜けると、選択範囲すべてに`#`等の入力した文字が反映されます。
   ちなみに`Ctrl` + `[`キーでも`Esc`同様の処理になります。
   `Esc`キーで小指を痛めている方は試してみてください。
   ![Screenshot from 2023-04-25 20-51-27.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/8fd92148-a52c-528e-5ea0-4239b93876b4.png)
   以上で一括コメントアウトは完了です。

## 複数行のコメントアウトを削除

一括削除する場合、複数行のコメントアウトの 3. の範囲選択までは同じです。

1. 範囲選択をしたら、`d`キーで一括削除できます。
   ![Screenshot from 2023-04-25 20-44-53.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/bfbe8b9d-65bb-fc9c-7421-03bc75ffad70.png)

## 最後に

閲覧頂きありがとうございました。
今回は頻度が多いであろう一括コメントアウトとして記事にしましたが、`#`を別の文字列にすれば複数行に同じ文字を入力する手法として応用可能です。
是非参考にしてみてください。
本記事がお役に立てば幸いです！
