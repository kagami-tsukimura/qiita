---
title: 【Python】Streamlit Sharingで簡単・爆速でWebページをデプロイする！
tags:
  - Python
  - Web
  - Webアプリケーション
  - Streamlit
private: false
updated_at: '2023-10-16T19:40:01+09:00'
id: c54702e271d729948e24
organization_url_name: null
slide: false
ignorePublish: false
---

# Introduction

皆様は`Streamlit`をご存じでしょうか？
完結に説明すると、WebシステムをPythonのみで作成できるフレームワークです。

<details><summary><b>=======================<br>&nbsp;&nbsp;&nbsp;Streamlitについて<br>&nbsp;========================</b></summary><div>

Pythonで Web開発となるとDjangoやFlask、FastAPI等が有名ですが、
`Streamlit`ではデータの可視化や機械学習モデルを用いたデモに用いられます。

一般的な Web 開発に必要なHTMLやCSS、JavaScriptを用いずとも作成できるため、Web開発に明るくない方や社内で結果を共有する際に優位性を発揮します。

本記事で内容には触れませんが、公式チュートリアル及びドキュメントが充実しているため興味のある方は覗いてみてください。

https://docs.streamlit.io/knowledge-base/tutorials

英語ですがコードは簡易で画像も多いため、やりたいことはほぼチュートリアルで解決します。
学習コストもかなり低いため、Pythonで次何しよう？ という方は是非一度お試しください！

</div></details>

手軽かつ非常に簡単に作成でき、デプロイも簡単でした。

ただしUIの変更があって、古い記事やUdemy等では少し躓く可能性があるように感じました。
本記事では公式が提供している`Streamlit Sharing`を用いたデプロイ方法を共有します。

:::note warn
以前はStreamlit Cloudという名称で、現在とはUIが異なっています。
:::

本記事でデプロイしたページは、Udemyの下記講座で作成したものです。

https://www.udemy.com/course/data-analysis_and_dashboard/

[ソースコードとデモページ](#参考URL)も公開しております。

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu22.04

## デプロイ準備

デプロイしたい`Streamlit`のプロジェクトをGitHubにコミットしておきます。
コミット手順は手前味噌ですが下記を参考にどうぞ。

https://qiita.com/kagami_t/items/ade25ff6e8bf8faf2ab2

## デプロイ

`Streamlit`の公式ページにアクセスします。

https://streamlit.io/

画面右上の`Sign up`をクリックします。

![st0.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/7c98b2a5-006a-e55a-12fd-cee2e24fd672.png)

GitHubとGoogle連携で登録できます。
私は`Continue with GitHub`から連携しました。

![Screenshot from 2023-08-25 21-10-34.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/01380420-fd3a-e484-7d8c-9f47842f9a82.png)

登録完了したら`New app`からサクッとデプロイしていきましょう。

![Screenshot from 2023-08-25 21-22-16.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/a817ba42-3c0d-ad0e-ba08-c2571096d7c9.png)

|        項目        | 説明                                         |
| :----------------: | :------------------------------------------- |
|     Repository     | GitHubのリポジトリをリストから選択します。   |
|       Branch       | デプロイするブランチをリストから選択します。 |
|   Main file path   | デプロイするファイルを記入します。           |
| App URL (Optional) | ドメインを記入します。                       |

![Screenshot from 2023-08-25 21-24-00.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/35d25b90-f884-0181-06c8-d3da3eab5d32.png)

Deploy! には少し時間がかかります。

![Screenshot from 2023-08-23 21-33-59.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/a810cb4e-f9e1-2f89-7bab-97453a84e4f4.png)

完了すると、先程指定したドメインにWebページがデプロイされます。

![Screenshot from 2023-08-23 21-35-25.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/67f757b0-da2b-2252-6f42-af6db9294071.png)

インタラクティブなページがサクッと & 無料でデプロイできちゃいました！

![Screenshot from 2023-08-23 21-35-40.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/de470e8d-7885-1d25-e47d-40ac56112be5.png)

### プラン制限

:::note warn
Freeプランでは以下の制約があります。

- メモリ: 1GBまで
- Privateのアプリ: 一つまで
- Publicのアプリ: 無制限

:::

![Screenshot from 2023-08-25 21-15-29.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/5186c997-52ed-e065-1b83-9df84c7c39f1.png)

### 最後に

最後まで閲覧頂きありがとうございました。

私の参画しているプロジェクトでは物体検知や分類結果を直接見せることが多いですが、お客様や社内外向けのデモを`Streamlit`で作成し、手元で見てもらえるため理解が深まりやすいと思います。

特に機械学習やデータ分析周りは学ぶことも多いため、工数削減にも大変役立ちます。
UIもリッチなので、工数以上に努力の跡が見せられると思います...!

本記事がお役に立てば幸いです！

### 参考 URL

https://github.com/kagami-tsukimura/streamlit-wage-analysis

https://app-wage-analysis-kagami-t.streamlit.app/

https://streamlit.io/
