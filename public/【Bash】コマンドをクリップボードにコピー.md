---
title: 【Bash】コマンドをクリップボードにコピー
tags:
  - Bash
  - Mac
  - Windows
  - Linux
  - Ubuntu
private: false
updated_at: '2023-05-13T17:26:13+09:00'
id: 84f6ec3142a8b370d908
organization_url_name: null
slide: false
---

# Introduction

記事を投稿する際に、コードの中身をコピペするのが地味に面倒......
出力結果をコマンドでクリップボードにコピーするコマンドを紹介します。

エラーログや出力結果など、チームで共有する際に重宝します。

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Windows10
Mac
Ubuntu22.04

## Windows

`Windows`には`clip.exe`コマンドが標準機能として提供されています。
コマンドの後ろに`| clip`でコピー可能です。

```powershell:
cat .\test.txt | clip
```

## Mac

`Mac`には`pbcopy`コマンドが標準機能として提供されています。
コマンドの後ろに`| pbcopy`でコピー可能です。

```bash:
cat test.txt | pbcopy
```

## Ubuntu

`Ubuntu`には標準機能としての提供がないため、クリップボードへのコピーを提供する`xsel`コマンドをインストールします。

```bash:
sudo apt install xsel
```

コマンドの後ろに`| xsel --clipboard --input`でコピー可能です。

```bash:
cat test.txt | xsel --clipboard --input
```

`Ubuntu`だけ覚えるのが面倒なので`.bashrc`にエイリアス登録します。

```bash:
sudo vim ~/.bashrc
```

`clip`で`xsel`を呼び出せるようにします。

```vim: ~/.bashrc
alias clip='xsel --clipboard --input'
```

エイリアス登録することで、`Windows`同様に`| clip`でコピー可能になります。

```bash:
cat test.txt | clip
```

## 最後に

ターミナル上でのコピペが少しストレスだったので、もっと早く導入するべきでした......

最後まで閲覧頂きありがとうございました。
備忘録の側面もありますが、本記事がお役に立てば幸いです！

### 参考 URL

https://noauto-nolife.com/post/linux-commandline-clipboard/
