---
title: 【Python】大量ファイルを一括リネーム
tags:
  - Python
  - ShellScript
  - 初心者
  - ワンライナー
private: false
updated_at: '2023-09-09T08:33:36+09:00'
id: 8256cb84663969603cc8
organization_url_name: null
slide: false
---

# Introduction

頻繁にデータセットに大量のファイルを追加することがあり、ファイル名に統一感がなくないことが気になっていました。

使用する端末が異なることもあり、何かある度に使い捨てでコードを書くのが手間なため備忘録とします。

ファイル名が被るとリネームする手間が増えるのと、雑多なデータセットを見る際の抵抗感を減らすことが目的です。

処理自体は単純ですが他の記事を見ていると、バックアップせずに使って上書かれ、泣きを見そうなスクリプトが多いです。
コピペしても比較的安全に使用できるコードを紹介します。

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu22.04
Python3.11.1

## 実装

先にソースコード全体を紹介します。

```python: rename.py
import os
import argparse
from glob import glob
import shutil

parser = argparse.ArgumentParser()
parser.add_argument("-i", "--input", type=str, default="input")
parser.add_argument("-o", "--output", type=str, default="output")
parser.add_argument("-n", "--name", type=str, required=True)
parser.add_argument("-e", "--extension", type=str, required=True)
args = parser.parse_args()

input_dir = args.input
output_dir = args.output
backup_dir = "backup"

os.makedirs(output_dir, exist_ok=True)
os.makedirs(backup_dir, exist_ok=True)

files = glob(f"{input_dir}/*")

for i, file in enumerate(files):
    rename_file = f"{output_dir}/{args.name}_{i}.{args.extension}"
    backup_file = f"{backup_dir}/{os.path.basename(file)}"
    while os.path.exists(rename_file):
        i += 1
        rename_file = f"{output_dir}/{args.name}_{i}.{args.extension}"
    shutil.copy2(file, backup_file)
    shutil.move(file, rename_file)
```

1. `./input`(又は`-i`で指定するディレクトリ)にファイルを格納。
1. コピペして以下のコマンドを実行。
   ```bash:
   python3 rename.py -n <ファイル名> -e <拡張子>
   ```
1. `./backup`にリネーム前のファイル、`./output`にリネーム後のファイルが格納されていることを確認。

## お試し

`./input`に画像を格納します。
フェアリーペンギンのデータセットを作る予定で追加を重ねた結果、統一感のないネーミングになってしまいました。(という設定です)

![Screenshot from 2023-05-27 12-15-16.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/e7dcb684-a8b2-10a1-3523-366efca85ea5.png)

リネームのスクリプトを実行します。
ファイル名は`Fairy_<連番>.png`にします。
拡張子を指定するコードにした理由は統一感と、`jpg`では透過(アルファチャンネル)対応していないので`png`に変える必要性が出るためです。

```bash:
python3 rename.py -n Fairy -o png
```

`./output`を確認すると、`Fairy_<連番>.png`にリネームされています。
統一感が出ました。

![Screenshot from 2023-05-27 12-23-44.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/2e7a7a52-54cb-97a6-c244-3e6c87946ea3.png)

## おまけ ワンライナーでリネーム

お試しで行ったような簡単なリネームならワンライナーでも可能です。

```bash:
ls *.png | awk 'BEGIN{ a=1 }{ printf "mv %s Fairy_%d.png\n", $0, a++ }' | sh
```

ただしワンライナーで`|`を挟んだ箇所は別個の処理となるため、他の処理が割り込む可能性を考慮してバックアップだけは取っておくことをおすすめします。

余談ですが、`awk`には`NR`という処理中の行番号を表すビルトイン変数があります。
連番に使えそうですが、あくまで`NR`の値は`ls`コマンドの出力行数なため行番号の重複でファイルが一部消えます。

**※シェルは下記書籍 1 冊読んだだけの門外漢のため、あくまで参考程度に。**

https://www.amazon.co.jp/%E3%83%9E%E3%82%B9%E3%82%BF%E3%83%AA%E3%83%B3%E3%82%B0Linux%E3%82%B7%E3%82%A7%E3%83%AB%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%97%E3%83%88-%E7%AC%AC2%E7%89%88-%E2%80%95Linux%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%80%81bash%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%97%E3%83%88%E3%80%81%E3%82%B7%E3%82%A7%E3%83%AB%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0%E5%AE%9F%E8%B7%B5%E5%85%A5%E9%96%80-Mokhtar-Ebrahim/dp/481440011X

## 最後に

最後まで閲覧頂きありがとうございました。
備忘録の側面もありますが、本記事がお役に立てば幸いです！

### 参考 URL

https://note.nkmk.me/python-os-rename-glob-format-basename-splitext/
