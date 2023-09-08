---
title: デフォルトのpipが旧バージョンのPythonに紐付いていた時の解決方法
tags:
  - Python
  - pip
  - Python3
private: false
updated_at: '2023-09-09T08:33:36+09:00'
id: fec097be5d6b19bfdb59
organization_url_name: null
slide: false
---

# Introduction

:::note info
**以前、同じ環境に Python を同居させてしまった時の副作用が原因になっています。先に目を通していただくと理解しやすいかと存じます。**
:::

https://qiita.com/kagami_t/items/d606b83622f1efa80232

本記事では、Python 自体は新バージョンをデフォルトにしたものの、pip は古いバージョンのがデフォルトになってしまった環境での解決方法を共有します。

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu22.04

## 発生条件

Flask と FastAPI との比較を行っていたところ、個人 PC に Flask-Login をインストールしていないことに気が付き、インストールを行いました。

```bash
pip install Flask-Login
```

インストール後も VSCode が import 文を認識しません。
pip list で確認してみます。

```bash
pip list | grep Flask
```

インストールした筈の Flask-Login が見当たりません。
確認のためサーバーを立ち上げても、import エラーが起きています。

## 原因

違うパスにインストールされたかと思い再度インストールを行うと、インストール済のメッセージが出ます。一部抜粋します。

```bash
Requirement already satisfied: Flask>=1.0.4 in /home/hoge/.local/lib/python3.10/site-packages (from flask_login) (2.3.2)
```

Python3.10 にインストールされています。私の Python は 3.11 なので、使用していない旧バージョンに Flask-Login がインストールされたため、import エラーが発生していました。

## 解決

pip のデフォルトを Python3.11 にして Flask-Login をインストールすれば動きそうなので、まず旧バージョンを消してしまいます。
デフォルトの Python を消すと二の舞なので、念の為 Python バージョンを確認します。

```bash
python3 -V
```

Python3.11.1 が返ってきたので、3.10 は消して良さそうです。

```bash
Python 3.11.1
```

lib の Python3.10 は消しておきます。

```bash
rm -r /home/hoge/.local/lib/python3.10
```

Python3.11 に pip が入っていなかったため、curl で`get-pip.py`を取ってきて実行します。

```bash
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py
```

pip バージョンを確認します。

```bash
python3 -m pip --version
```

Python3.11 が確認できました。

```bash
pip 23.2.1 from /home/hoge/.local/lib/python3.11/site-packages/pip (python 3.11)
```

後は`pip install`して動作確認すれば完了です。

```bash
pip install Flask-Login
```

import エラーが消え、無事に Flask サーバーが立ち上がりました。

### 最後に

Python の管理は仮想環境使った方が安心ですね。[^1]
[^1]: 私も以前は Anaconda で管理していましたが、個人環境ではメリットより Anaconda のトラブルがボトルネックになったため pip+Docker で管理しています。

本記事の問題が想定より早く解決してしまったため、Flask-Login 固有の問題なのかは分かりませんでした。[^2]
[^2]: Python3.11 にしてからインストールした Pytorch 等は問題なく動作していたため、pip は入っていた筈です。

もし判明したら追記します。

最後まで閲覧頂きありがとうございました。
本記事がお役に立てば幸いです！

### 参考 URL

https://pypi.org/project/Flask-Login/
