---
title: GitHubに、developブランチから新しくfeatureブランチを切り出してpushする。
tags:
  - Git
  - GitHub
private: false
updated_at: '2023-03-30T19:34:22+09:00'
id: f1e521b7e95a90376808
organization_url_name: null
slide: false
---
# Introduction
- `main`(master)を本番環境で運用。
- `develop`を開発環境で運用。
- 機能追加の際は`feature`ブランチを切り出して`develop`にプルリク→マージする。
（AI開発だと`develop`を本番環境で実験して、折を見て安定版を`main`(master)にマージすることもありますが...）
上記を想定して`feature`ブランチの切り出しを行っていきます。
幾つか方法はあると思いますが、私がプロジェクトで実際に行っている方法で今回は記載します。


__本記事が少しでも読者様の学びに繋がれば幸いです！__
__「いいね」をしていただけると今後の励みになるので、是非お願いします！__

## 環境
Ubuntu22.04

## 1. ブランチの最新化

デグレすると後々大きな問題になりかねないので、ローカルブランチを最新の状態にします。

```console
git fetch
git pull origin develop
```

## 2. ブランチの切り出し

`develop`ブランチから`feature`ブランチを作成します。
ここでは`feature/test`ブランチとします。
feature以下は、追加する機能の名称にするとわかりやすくて良いと思います。

```console
git checkout -b feature/test develop
```
`develop`ブランチをベースに、`feature/test`ブランチを作成してチェックアウトしました。

現在のブランチが`feature/test`ブランチであることを確認しておきましょう。

```console
git branch -a
```

ついでにlogも確認できると便利です。
もし`git tree`コマンドをエイリアス指定していない方は、以下の記事を参考に設定することを強くおすすめします。

https://qiita.com/takasianpride/items/842a785af610025a2030

設定が完了したらlogを確認しましょう。最新の`develop`ブランチと同様のlogが表示されます。
```console
git tree
```

## 3.GitHubにpush

既に`feature`ブランチに機能を追加済みであれば、通常通りpushすればリモートリポジトリに反映されます。
```console
git add .
git commit -m "prefix: any message"
git push origin feature/test
```

機能追加前にpushだけ済ませておくことも可能です。
```console
git push -u origin feature/test
```

ここまでの手順で、ローカルリポジトリの`feature/test`ブランチとリモートリポジトリの`origin/feature/test`ブランチが紐付けられました。

## おまけ： 誤って作成したブランチを削除したい

新しくブランチを切り出したけど、「機能追加が不要になった」「ブランチ名を間違えた」等の理由で切り出したばかりの`feature`ブランチを削除したくなることがあります。
では、リモートリポジトリから削除していきましょう。
GitHubのbranchesをクリックして、右側のゴミ箱マークからも削除できますが、今回はコマンドから削除します。
```console
git push -d origin feature/test
```
これでリモートリポジトリの`feature/test`は削除されました。
ローカルリポジトリも削除しておきましょう。
削除したい`feature`ブランチから別のブランチにcheckoutします。
```console
git checkout develop
```
`feature`ブランチを削除しましょう。
```console
git branch -D feature/test
```
このような結果であれば削除完了です。
```console: result
Deleted branch feature/test (was hogehoge).
```
このような結果であれば`git checkout`ができておりません。
`git branch -a`で現在のブランチを確認して、`git checkout`で別のブランチに移動しましょう。
そして再度、`git branch -D`です。
```console: result
error: Cannot delete branch 'feature/test' checked out at '/hogehoge'
```

## 最後に
閲覧頂きありがとうございました。
備忘録の側面もありますが、本記事がお役に立てば幸いです！
