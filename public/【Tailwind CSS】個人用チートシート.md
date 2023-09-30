---
title: 【Tailwind CSS】個人用チートシート
tags:
  - 'tailwindcss'
  - 'Web'
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

# Introduction

個人開発をする際にTailwind CSSをよく使うのですが、簡単な実装でも調査に時間を費やすことが多くありました。
本業がMLエンジニアのためWebに疎く、時間が経つと記憶から抜け落ちてしまいます。。。
本記事ではTailwind CSSで個人的に頻繁に使う or 忘れがちな実装をメモしていきます。

慣れており、様々な機能を使いこなせる方は公式のチートシートが充実していて大変素晴らしいです。

https://tailwindcomponents.com/cheatsheet/

また、Tailwind CSSはテンプレートも充実しています。
特に私はPreline UIをよく使うので紹介しておきます。
[![Preline UI](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/106716/7b2a93f2-5bfe-90dc-963c-7df5f0c54a88.png)](https://preline.co/)

:::note info
Information:
Tailwind CSS v3の内容を記載します。
:::

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu22.04
tailwindcss@3.3.3

## チートシートメモ

| 期待事項               | 実装                           |
| :--------------------- | :----------------------------- |
| 横並びにしたい         | flex items-center              |
| 改行したくない         | whitespace-nowrap              |
| レスポンシブ対応したい | sm @media (min-width: 640px)   |
|                        | md @media (min-width: 768px)   |
|                        | lg @media (min-width: 1024px)  |
|                        | xl @media (min-width: 1280px)  |
|                        | 2xl @media (min-width: 1536px) |

:::note info
Information:
随時追加予定。
:::

### 最後に

最後まで閲覧頂きありがとうございました。
本記事がお役に立てば幸いです！

### 参考URL

https://tailwindcomponents.com/cheatsheet/

https://preline.co/
