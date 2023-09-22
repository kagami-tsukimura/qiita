---
title: GitHubの新規リポジトリを作成して、developブランチによる開発準備
tags:
  - Git
  - GitHub
  - 初心者向け
  - 個人開発
  - 新人プログラマ応援_記事投稿キャンペーン
private: false
updated_at: '2023-09-09T08:33:36+09:00'
id: ade25ff6e8bf8faf2ab2
organization_url_name: null
slide: false
ignorePublish: false
---

# Introduction

個人開発で GitHub を扱う際に行っている準備についてまとめました。
作業を定型化するための覚書です。

| No. | 作業内容リンク                                        |
| :-: | :---------------------------------------------------- |
|  1  | [ローカルリポジトリの設定](#ローカルリポジトリの設定) |
|  2  | [GitHub の設定](#githubの設定)                        |
|  3  | [開発用ブランチの作成](#開発用ブランチの作成)         |
|  4  | [おまけ](#おまけ)                                     |
|  5  | [参考 URL](#参考url)                                  |

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 事前準備

GitHub アカウントの作成

## 環境

Ubuntu22.04

## 新規リポジトリのセットアップ

GitHub で管理できるように新規リポジトリを作成します。

### ローカルリポジトリの設定

Terminal を起動し、作成したいアプリケーションのルートディレクトリに移動します。
ここでは仮に、`develop/test/`とします。
Terminal は`Ctrl+Alt+T`で起動して、`cd`コマンドで移動します。

```console
cd develop/test/
```

`git config`コマンドで、デフォルトのブランチ名(ここでは main)を設定します。

```console
git config --global init.defaultBranch main
```

`git init`コマンドでリポジトリを初期化します。

```console
git init
```

`git config`をしないと警告が出ます。
メッセージは記載しますが、詳細は割愛します。

```console: warning
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint: 	git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint: 	git branch -m <name>
Initialized empty Git repository in /home/hogehoge/develop/test/.git/
```

`git add`コマンドでディレクトリ配下の全ファイルをステージングに登録します。

```console
git add .
```

`git commit -m`で変更内容(`git add`した内容)をコメント付きでローカルリポジトリにコミットします。

```console
git commit -m "Initialize repository"
```

このような結果であれば正常に動作しています。

```console: result
[main (root-commit) 44037f6] Initialize repository
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test.txt
```

1 ファイルもない状態でコミットを行うと以下のメッセージが表示されます。
この場合は何かしらファイルを作成して、`git add`コマンドからやり直してください。

```console: result
nothing to commit (create/copy files and use "git add" to track)
```

これでローカルリポジトリへのコミットが完了しました。
GitHub 側でリポジトリ作成後にコマンド入力が続きます。
Terminal は閉じずに残しておきましょう。

### GitHub の設定

ローカルリポジトリでコミットした変更内容を、リモートリポジトリにも反映させましょう。
GitHub の画面右上の`+▼`をクリックするとプルダウンメニューが表示されます。
![Screenshot from 2023-03-25 11-47-16.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/6188a2fb-8c52-05cc-cb85-fbde9c4fa9f8.jpeg)
一番上の`New repository`をクリックします。
![Screenshot from 2023-03-25 12-09-11.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/c2b17c8d-3177-f3ac-e54b-cc8e70bef729.jpeg)
新規リポジトリ作成画面で`Repository name`を入力後、`Create repository`をクリックしましょう。
これで GitHub 側のリポジトリが作成されます。
![Screenshot from 2023-03-25 12-33-31.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/d7315137-d629-dd7a-c25e-bcabbcb0dd52.png)

### ローカルリポジトリにリモートリポジトリ情報を追加します。

GitHub の画面からリモートリポジトリの URL をコピーします。
![Screenshot from 2023-03-25 12-38-39.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/14b14797-52b0-a191-39b5-48f0adabfb4f.jpeg)
Terminal に戻り、`git remote`コマンドでリモートリポジトリ情報を追加します。

```console
git remote add origin [リモートリポジトリのURL]
```

`git push`コマンドでローカルリポジトリの内容をリモートリポジトリに push します。

```console:
git push origin main
```

GitHub のページに戻ってリロードすると、push した内容が反映されていることが確認できます。

![Screenshot from 2023-03-25 13-13-02.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/711f7677-4f72-ff14-98ad-27cce1baea93.png)
このまま GitHub を活用することもできますが、main のブランチで開発することは推奨されません。
個人開発でも履歴管理の観点から開発用にブランチを切って、開発や検証を行うことを推奨します。
では実際に、開発用のブランチを作成していきます。

### 開発用ブランチの作成

新規ブランチ作成に取りかかる前に、`git branch`コマンドで現在のブランチを確認します。

```console
git branch -a
```

作成した`main`ブランチが表示されます。

```console: result
* main
  remotes/origin/main
```

ここに開発用の`develop`ブランチを追加するために、`git checkout`コマンドを使用します。

```console
git checkout -b develop
```

`git branch`コマンドで`develop`ブランチの追加を確認します。

```console
git branch -a
```

ローカルリポジトリに`develop`ブランチが作成されチェックアウトされています。

```console: result
* develop
  main
  remotes/origin/main
```

`git push`コマンドでローカルリポジトリの`develop`ブランチをリモートリポジトリに push します。
また、`-u`オプションによりローカルリポジトリとリモートリポジトリが紐付けられます。

```console
git push -u origin develop
```

このような結果であれば正常に動作しています。

```console: result
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote:
remote: Create a pull request for 'develop' on GitHub by visiting:
remote:      https://github.com/kagami-tsukimura/test/pull/new/develop
remote:
To https://github.com/kagami-tsukimura/test.git
 * [new branch]      develop -> develop
Branch 'develop' set up to track remote branch 'develop' from 'origin'.
```

GitHub のページに戻ってリロードすると、push 内容が反映されていることを確認できます。
![Screenshot from 2023-03-25 14-22-50 (1).jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/c1e739d9-6b68-9601-daab-e7f949c63313.jpeg)
以上で私が行っている GitHub での開発準備完了です。
余談ですが、業務など共同開発を行う場合は`develop`ブランチから`feature`ブランチを切り出して開発することが多いと思います。
あくまで今回の手順は、個人開発で簡単なアプリケーション開発を行う準備となっております。

### おまけ

これまでの手順で`develop`ブランチで開発、`main`ブランチへの PullRequest 及び Merge ができるようになりました。
ただ、このままでは`main`ブランチへ直接 push することも可能になってしまっています。
main ブランチが保護されていない旨のメッセージも表示されています。
![Screenshot from 2023-03-25 14-27-54.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/abcb3175-7b8a-2f87-9bc9-270eadc1be6f.jpeg)
念の為、勝手に`main`ブランチを書き換えられてしまわないように、
`Protect this branch`をクリックして`main`ブランチを保護しておきましょう。
以下の設定にチェックを入れて、画面下部の`Save changes`をクリックして保護は完了です。

- Require a pull request before merging
  - Require approvals
- Require status checks to pass before merging
  ![Screenshot from 2023-03-25 14-29-12.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/bb89e4f1-f1b2-4961-b45f-814c43bd4999.png)

`develop`ブランチから更に`feature`ブランチを切り出して開発したい場合は、こちらの記事を参考にしてください。

https://qiita.com/kagami_t/items/f1e521b7e95a90376808

### 最後に

閲覧頂きありがとうございました。
備忘録の側面もありますが、本記事がお役に立てば幸いです！

### 参考 URL

[Git checkout|Atlassian Git Tutorial](https://www.atlassian.com/ja/git/tutorials/using-branches/git-checkout 'Git checkout')
