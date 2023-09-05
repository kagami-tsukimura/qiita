---
title: 【Ubuntu】NVIDIAドライバでブラックアウトした時の対処
tags:
  - Python
  - Ubuntu
  - CUDA
  - NVIDIA
  - PyTorch
private: false
updated_at: '2023-07-29T06:54:54+09:00'
id: 33fcfed7769317f6a37a
organization_url_name: null
slide: false
---
# Introduction

先日メインで使っていたミニPCがクラッシュし、余ったパーツを流用して自作PCを組みました。
ショックが大きかった（現在返品対応中）ですが、GPUを導入してローカルでも機械学習を回せるようになったのでPytorchでGPU環境を構築しました。

まず`NVIDIAドライバ`をインストールしてreboot、ロック解除すると...

![pcsp015.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/af858af9-0969-6e83-73c2-953a3407b34a.png)

画面遷移が...

![pcsp015.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/af858af9-0969-6e83-73c2-953a3407b34a.png)

されない！
ブラックアウト！

似たような状況でお急ぎの方のために結論を書きますが、`CUDA Toolkit`だけインストールしてみてください！

※NVIDIAとGPU関連はトラブルをよく耳にするのであくまで私のケースの対処法です。

__本記事が少しでも読者様の学びに繋がれば幸いです！__
__「いいね」をしていただけると今後の励みになるので、是非お願いします！__


## 環境
Ubuntu22.04
GeForce RTX 3060

## 手順

ブラックアウトした場合、落ち着いて`Ctrl + Alt + F3(F6)`でテキストモードに入りましょう。
ユーザー名とパスワードを入力するとCLIが使用できます。

原因の`NVIDIAドライバ`は一旦消しちゃいましょう。
```console: 
sudo apt-get purge nvidia-*
```

reboot(私の場合はテキストモードに戻されました)するかロック画面に戻ります。
```console: 
sudo service display-manager restart
```

私はこれで画面遷移しましたが、ブラックアウトが戻らなければ、`nouveau`周りを疑いましょう。
幾つか記事を見ていると、`NVIDIAドライバ`インストールの流れでブラックリストに入れていました。
`blacklist.conf`で`nouveau`を消せば回復すると思います。
```console: 
sudo vim /etc/modprobe.d/blacklist.conf
sudo vim /etc/modules-load.d/blacklist.conf
```

画面が戻ったら下記リンクから`CUDA Toolkit`をインストールします。
自分の環境に合ったものをクリックしていくと、インストール用のコマンドが表示されるので上から順に進めればOKです。

https://developer.nvidia.com/cuda-downloads?target_os=Linux

私はUbuntu22.04なので下記のようになります。
Installer Typeはdeb (local)を選びましょう。(3GBくらいあります)

![Screenshot from 2023-07-28 21-08-26.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/104c5cb6-0bec-aff1-6074-4bcfe6b5cbed.png)

最新(12.2)で大丈夫でしたが、2023年7月現在のPytorch2.0のStable版ではCUDA11.7か11.8のため気になる方は合わせても良いと思います。

![Screenshot from 2023-07-28 21-23-02.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/e95f9055-c6cc-559b-76be-12f9e9351818.png)

インターネット状況によりますが、全体で20分程度かかります。

インストールし終わったらrebootして確認しましょう。

```console: 
reboot
```

```console: 
nvidia-smi
```

![Screenshot from 2023-07-28 22-08-34.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/d5a62cfc-25f0-84f5-9de3-9f20a659033d.png)

`NVIDIAドライバ`が正常にインストールされています。
`CUDA Version`でCUDAインストールも確認できます。

`~/.bashrc`にパスを通します。
以下2行を追記してください。

```console: 
sudo vim ~/.bashrc
```

```vim: 
export PATH=/usr/local/cuda:/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/lib:/usr/local/cuda/lib64:$LD_LIBRARY_PATH

```

再読込して確認します。

```console: 
source ~/.bashrc
```

```console: 
nvcc -V
```

![Screenshot from 2023-07-28 22-24-28.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/112794ca-b8fa-7399-f251-d8321ab8bbc5.png)

ここまで来れば一見落着です。
`tensorflow`でも`Pytorch`でもインストールして機械学習しちゃいましょう！


### 最後に

閲覧頂きありがとうございました。

試行錯誤していた限りでは`nvidia-persistenced.service`が悪さしていそうです。
手順をメモしておけば良かったのですが、物理的なクラッシュの次は論理的なクラッシュかと焦っていたため覚え書きで失礼しました。

今回のトラブルシューティングで感じましたが、NVIDIAドライバ関連の日本語記事が少なく感じました。
英語では割と盛んで私も参考にしましたが、NVIDIAとGPU関連は様々なトラブルがあり混乱しかねませんので、日本語の記事も充実して欲しく、その一助になればと思います。

Ubuntuにして約1年、公私問わずトラブル連発で耐性と低レイヤの知識がかなりついてきました！
ずっとWindowsユーザーでCLIに抵抗しかなかった中、下記の書籍で一通り勉強した知識が後から効いてきています。とてもおすすめです。

https://www.amazon.co.jp/%EF%BC%BB%E8%A9%A6%E3%81%97%E3%81%A6%E7%90%86%E8%A7%A3%EF%BC%BDLinux%E3%81%AE%E3%81%97%E3%81%8F%E3%81%BF-%E2%80%95%E5%AE%9F%E9%A8%93%E3%81%A8%E5%9B%B3%E8%A7%A3%E3%81%A7%E5%AD%A6%E3%81%B6OS%E3%80%81%E4%BB%AE%E6%83%B3%E3%83%9E%E3%82%B7%E3%83%B3%E3%80%81%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A%E3%81%AE%E5%9F%BA%E7%A4%8E%E7%9F%A5%E8%AD%98%E3%80%90%E5%A2%97%E8%A3%9C%E6%94%B9%E8%A8%82%E7%89%88%E3%80%91-%E6%AD%A6%E5%86%85-%E8%A6%9A/dp/429713148X

最初は疑問符ばかり浮かびましたが、知識を活かせると楽しくなってきますね。ええ...

...次回のトラブルは冬頃がいいなぁ。


本記事がお役に立てば幸いです！


### 参考URL

https://developer.nvidia.com/cuda-downloads?target_os=Linux

https://pytorch.org/get-started/locally/

