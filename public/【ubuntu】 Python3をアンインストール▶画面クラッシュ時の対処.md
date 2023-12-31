---
title: 【ubuntu】 Python3をアンインストール▶画面クラッシュ時の対処
tags:
  - Python
  - Linux
  - Ubuntu
private: false
updated_at: '2023-09-09T08:33:36+09:00'
id: d606b83622f1efa80232
organization_url_name: null
slide: false
ignorePublish: false
---

# Introduction

**下手に`Python`アンインストールしない！**
**`ubuntu-desktop`を再インストールしても直らない場合に！**

別バージョンの`Python`を試す予定で`Python3.10`と`Python3.11`をインストールしていました。
不要になったため後から入れた`Python3.10`をアンインストールする予定だったのですが、何を思ったのか(タブ補完で typo して)デフォルトで入れた`Python3.11`の方をアンインストールしてしまいました。

途中で気付いて`Ctrl+C`したのですが、画面が落ちて無事クラッシュ......

`ubuntu-desktop`を再インストールすれば直るとのことですが、それでは解決しませんでした。
似たような現象の方に届くことを願って共有します。

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu22.04

## 前提

- `ubuntu`で`Python3`をアンインストールして画面クラッシュ。
- `ubuntu-desktop`を再インストールしても変化なし。
  ```bash:
  sudo apt-get install --reinstall ubuntu-desktop
  ```

## 結論

リカバリーモードで CUI ログイン画面に入り、`gdm3`を再インストールしてサービスを再起動したら回復しました。
手順を以下に記載します。

1. 電源ボタンからマシンを起動する。
1. `Shift`を連打で`GNU GRUB`を起動する。(普段は`Esc`でも起動できるのですが、何故か効きませんでした)
1. 上から 2 番目の「`Advanced options for Ubuntu`」を選択する。
1. カーネル選択で、語尾が`recovery mode`のものを選択する。
1. 下から 2 番目の`root`を選択し、root ユーザーでコマンド入力できるようにする。
1. `gdm3`を再インストールする。
   ```bash:
   sudo apt purge gdm3
   sudo apt install gdm3
   ```
1. `gdm3`の設定ファイルを確認する。
   ```bash:
   sudo vim /etc/gdm3/custom.conf
   ```
1. `Vim`で以下の行からコメントアウトを解除する。
   ```vim:
   WaylandEnable=false
   ```
1. サービスを再起動する。しないと画面暗転に逆戻りしました。
   ```bash:
   sudo service gdm3 restart
   ```
1. マシンを再起動し、GUI ログイン画面が起動することを確認する。
   ```bash:
   reboot
   ```

## おまけ：`grub>`が表示される方

`grub`のログイン画面に入ったり、`Ctrl+Alt+F2`でコマンドモードに入ってコマンド入力等の手段もありました。

https://kajindowsxp.com/grub2/

1. Ubuntu の/ (ルートパス)は`ls`や`ls /boot`コマンドで表示される`vmlinuz-`の更新日時が一番新しいものをルートパスとして指定してください。
   私の場合は(hd0,3)でした。
   `bash: 
    set root=(hd0,3)
    `
1. カーネルイメージのパスは、`vmlinuz-`のカーネルバージョンが一番新しいものを指定してください。ドライブ名は 1. と同じです。
   ```bash:
   linuxefi [カーネルイメージのパス] root=(hd0,3)
   ```
1. 初期 RAM ディスクのパスは`initrd.img-`で、2. と同じカーネルバージョンを指定してください。
   ```bash:
   initrdefi initrd.img-[カーネルイメージのパス]
   ```
1. 再起動する。
   ```bash:
   reboot
   ```

そもそもクラッシュしているため、すべてに当てはまる事象ではないと思います。
上記紹介記事の手順も参考にしてみてください。

## 最後に

最後まで閲覧頂きありがとうございました。

`Python`をアンインストールしなければ良いだけの話ですが、`Linux`にはトラブルがつきものなのでお守り代わりに本記事を覚えておいていただければ幸いです。

正直結論に至るまで数時間は格闘して試行錯誤したため、カーネル周りについてかなり良い勉強になりました。怪我の功名でした。

実践も良いですが心臓に悪いため、私が`Linux`の勉強に使っている書籍を紹介しておきます。
安易にクラッシュさせずに書籍で勉強しましょう......

https://www.amazon.co.jp/%EF%BC%BB%E8%A9%A6%E3%81%97%E3%81%A6%E7%90%86%E8%A7%A3%EF%BC%BDLinux%E3%81%AE%E3%81%97%E3%81%8F%E3%81%BF-%E2%80%95%E5%AE%9F%E9%A8%93%E3%81%A8%E5%9B%B3%E8%A7%A3%E3%81%A7%E5%AD%A6%E3%81%B6OS%E3%80%81%E4%BB%AE%E6%83%B3%E3%83%9E%E3%82%B7%E3%83%B3%E3%80%81%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A%E3%81%AE%E5%9F%BA%E7%A4%8E%E7%9F%A5%E8%AD%98%E3%80%90%E5%A2%97%E8%A3%9C%E6%94%B9%E8%A8%82%E7%89%88%E3%80%91-%E6%AD%A6%E5%86%85-%E8%A6%9A/dp/429713148X

備忘録の側面もありますが、本記事がお役に立てば幸いです！

### 参考 URL

https://qiita.com/R1ngNebula/items/a35af01868b590ac7e89
