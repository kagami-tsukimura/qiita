---
title: 【lsof】ローカルのWebサーバーで起動したアプリが残っている時の対処法
tags:
  - Linux
  - Web
  - lsof
  - kill
private: false
updated_at: '2023-09-09T08:33:36+09:00'
id: a967c65ae584cdcd795c
organization_url_name: null
slide: false
---

# Introduction

以前に npm start で起動したアプリが残っていたので、プロセスの終了がてら共有します。
余談ですが、所謂ゾンビ化したプロセスには`ps aux|grep <process_name>`をよく使いますね。
Web サーバーでは本記事の手順になります。

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 前提

Terminal 上で現在起動中のプロセスが残っていれば、`Ctrl + C`で終了してください。
何らかの原因で`Ctrl + C`後もプロセスが残り続けた場合は、以下の方法をお試しください。

## 環境

Ubuntu22.04

## lsof コマンド

`lsof`コマンドで起動中のプロセスを確認することができます。

### 起動中のプロセスのポート番号が判明している場合

```console: 指定したポート番号のプロセスを表示
$ lsof -i :ポート番号
COMMAND    PID     USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
code    108547 hogehoge  114u  IPv4 1002172      0t0  TCP localhost:3000 (LISTEN)
```

### 起動中のプロセスのポート番号が判明していない場合

```console: 起動中のプロセスを表示
$ lsof -iTCP -sTCP:LISTEN
COMMAND      PID     USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
code      108547 hogehoge  114u  IPv4 1002172      0t0  TCP localhost:3000 (LISTEN)
code      108547 hogehoge  116u  IPv4 1002173      0t0  TCP localhost:3001 (LISTEN)
```

## kill コマンド

終了したいサービスのプロセス ID（PID）を確認し、`kill`コマンドを実行します。
誤った ID を入力して重要なプロセスを終了しないよう、PID はコピペすることを推奨します。

```console: プロセスを強制終了
$ kill -9 PID
```

## 最後に

閲覧頂きありがとうございました。
備忘録の側面もありますが、本記事がお役に立てば幸いです！

## 参考 URL

[lsof の使い方](https://hogem.hatenablog.com/entry/20070223/1172221315 'lsof')
