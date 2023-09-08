---
title: 【pip】 Python3をアップデートしたら詰まった話
tags:
  - Python
  - pip
  - 初心者
  - Python3
private: false
updated_at: '2023-09-09T08:33:36+09:00'
id: ccc89a9a403c196b69d6
organization_url_name: null
slide: false
---

# Introduction

GW に突入しましたが、皆様はいかがお過ごしでしょうか？
私は無事 9 連休を勝ち取り、業務で使い込んでいるにもかかわらず意図的に避けてきた`Python`の記事を書こうかなと準備しておりました。
折角なので`Python`もアップデートして臨もうと。
なお業務では`anaconda`、自宅では`pip`で管理しており、業務中は`pip`=腫れ物です。
にもかかわらず大して調べず深夜のノリでやって更新に失敗......解決に苦労しました。
調べても`pip`下での`Python`アップデートはあまり出てこなかったので、手順と対処法を共有します。

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu22.04
Python3.10(アップデート前)
Python3.11(アップデート後)

## 設定ファイルのエクスポート

アップデート後の`Python`にパッケージを一括インストールするために、現在の設定をエクスポートします。
`anaconda`では`環境名.yaml`に書き出すのが一般的という認識ですが、`pip`では`requirements.txt`に書き出します。

```bash:
pip freeze > requirements.txt
```

## Python のアップデート

最新版の`Python`は以下のサイトからダウンロードしました。
非公式とありますが、提供元は`Python Japan`様で安心です。

https://pythonlinks.python.jp/ja/index.html

インストール方法は以下の記事が分かりやすかったです。

https://self-development.info/ubuntu%e3%81%ab%e6%9c%80%e6%96%b0%e3%83%90%e3%83%bc%e3%82%b8%e3%83%a7%e3%83%b3%e3%81%aepython%e3%82%92%e3%82%a4%e3%83%b3%e3%82%b9%e3%83%88%e3%83%bc%e3%83%ab%e3%81%99%e3%82%8b/

記事の通り、アップデート後に`Python`のバージョンが上がっていることを確認します。

```bash:
python3 -V
```

## 設定ファイルのインポート

`requirements.txt`をアップデート後の`Python`にインポートします。

```bash
pip install -r requirements.txt
```

## トラブル発生

確認のため`numpy`等を import してみると、お馴染みのエラーが発生。

```python
ModuleNotFoundError: No module named 'numpy'
```

`Python`のディレクトリがどこにあるかわからなかったので、`pip`インストールしたパッケージの格納先を調べました。
`Location`に出力されます。覚えておくと何かと便利です。

```bash
pip show numpy
```

`Python`のディレクトリを確認。消した筈の`Python3.10`が共存しています。
![Screenshot from 2023-04-30 01-48-50.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/8a55e4f3-e029-403c-59e3-0e3c7122ac64.png)
ちなみに、`pip install`した package は`Python`配下の`site-packages`に格納されますので各ディレクトリ確認しました。
するとインポート結果はすべて`Python3.10`の方に入っていました。
`Python3.10`を消して再度インポートを試すと、消した筈の`Python3.10`が復活します。

## 対処法

`pip`がインストールされている`Python`のバージョンを確認します。

```bash:
pip -V
```

`Python3.10`の方が返ってきました。
ここまでで頓珍漢な試行錯誤を繰り返していたこともあり、パスが`Python3.10`に通っていることは予測できました。
パスを`Python3.11`に通せば正常にインポートできそうなので、`.bashrc`に 1 行追記します。

```bash:~/.bashrc
export PATH=/home/<ユーザーネーム>/.local/lib/python3.11/site-packages:$PATH
```

[設定ファイルのインポート](#設定ファイルのインポート)を再実行すると、無事`Python3.11`にパッケージがインポートされました。
`site-packages`や`Python`ファイルの実行も試し、正常に使用できるようになりました。

## おまけ

余談ですが、最近画像編集フリーソフトの`GIMP`を使う機会が多くありました。
除去しきれなかった背景の除去や加工、合成、座標確認など行えることと操作性のバランスが良く、特に検証レベルでは何かと役に立つのでおすすめします。
(アプリケーションが豊富なので他の選択肢もありそうですが)`Windows`も対応しています。

https://www.gimp.org/

## 最後に

紆余曲折ありながらも無事`Python3.11`にアップデート完了しました。
速度改善など個人的に試したいことが盛り沢山です。
ただアップデートにより、実装に使う予定だった背景除去の`Rembg`がバージョン問題で現状使えなさそうで少し困っています......
業務では`OpenCV`で背景除去スクリプトを書いていますが、工数の割に精度は難ありだったので代替案を検討中です。
元も子もないですが、部分的には`Python`で書いて、スクリプト全体は`Google Colab`で流せるようにするのもありかなと考えています......
最後まで閲覧頂きありがとうございました。
備忘録の側面もありますが、本記事がお役に立てば幸いです！
