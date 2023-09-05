---
title: デフォルトのpipが旧バージョンのPythonに紐付いていた時の解決方法
tags:
  - Python
  - pip
  - Python3
private: false
updated_at: '2023-08-14T09:01:18+09:00'
id: fec097be5d6b19bfdb59
organization_url_name: null
slide: false
---
# Introduction

:::note info
__以前、同じ環境にPythonを同居させてしまった時の副作用が原因になっています。先に目を通していただくと理解しやすいかと存じます。__
:::

https://qiita.com/kagami_t/items/d606b83622f1efa80232

本記事では、Python自体は新バージョンをデフォルトにしたものの、pipは古いバージョンのがデフォルトになってしまった環境での解決方法を共有します。

__本記事が少しでも読者様の学びに繋がれば幸いです！__
__「いいね」をしていただけると今後の励みになるので、是非お願いします！__

## 環境
Ubuntu22.04

## 発生条件

FlaskとFastAPIとの比較を行っていたところ、個人PCにFlask-Loginをインストールしていないことに気が付き、インストールを行いました。

```bash
pip install Flask-Login
```

インストール後もVSCodeがimport文を認識しません。
pip listで確認してみます。

```bash
pip list | grep Flask
```

インストールした筈のFlask-Loginが見当たりません。
確認のためサーバーを立ち上げても、importエラーが起きています。

## 原因

違うパスにインストールされたかと思い再度インストールを行うと、インストール済のメッセージが出ます。一部抜粋します。

```bash
Requirement already satisfied: Flask>=1.0.4 in /home/hoge/.local/lib/python3.10/site-packages (from flask_login) (2.3.2)
```

Python3.10にインストールされています。私のPythonは3.11なので、使用していない旧バージョンにFlask-Loginがインストールされたため、importエラーが発生していました。

## 解決

pipのデフォルトをPython3.11にしてFlask-Loginをインストールすれば動きそうなので、まず旧バージョンを消してしまいます。
デフォルトのPythonを消すと二の舞なので、念の為Pythonバージョンを確認します。

```bash
python3 -V
```

Python3.11.1が返ってきたので、3.10は消して良さそうです。

```bash
Python 3.11.1
```

libのPython3.10は消しておきます。

```bash
rm -r /home/hoge/.local/lib/python3.10
```

Python3.11にpipが入っていなかったため、curlで`get-pip.py`を取ってきて実行します。

```bash
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py
```

pipバージョンを確認します。

```bash
python3 -m pip --version
```

Python3.11が確認できました。

```bash
pip 23.2.1 from /home/hoge/.local/lib/python3.11/site-packages/pip (python 3.11)
```

後は`pip install`して動作確認すれば完了です。

```bash
pip install Flask-Login
```

importエラーが消え、無事にFlaskサーバーが立ち上がりました。

### 最後に

Pythonの管理は仮想環境使った方が安心ですね。[^1]
[^1]: 私も以前はAnacondaで管理していましたが、個人環境ではメリットよりAnacondaのトラブルがボトルネックになったためpip+Dockerで管理しています。

本記事の問題が想定より早く解決してしまったため、Flask-Login固有の問題なのかは分かりませんでした。[^2]
[^2]: Python3.11にしてからインストールしたPytorch等は問題なく動作していたため、pipは入っていた筈です。

もし判明したら追記します。

最後まで閲覧頂きありがとうございました。
本記事がお役に立てば幸いです！


### 参考URL

https://pypi.org/project/Flask-Login/


