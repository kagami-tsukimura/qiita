---
title: React + TypeScriptで一意のIDを生成
tags:
  - JavaScript
  - math
  - TypeScript
  - UUID
  - React
private: false
updated_at: '2023-09-09T08:33:36+09:00'
id: f99f3499aa586ddcd02b
organization_url_name: null
slide: false
---

# 結論

**「`Math.random()`ではなく`uuid`を使用して一意の ID を生成しましょう」**

## Introduction

React でアプリケーションを開発する際に、一意の ID が必要になることがあります。
例えば`Math.random()`で生成した文字列を ID として使用できそうです。
ただ、この文字列は疑似乱数のため重複する懸念があり、一意の ID としては推奨されません。
業務で使っている`mlflow`では`uuid`が使われていることを思い出し、調べてみました。
React でも使えそうです。

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## uuid ライブラリを使用した ID の生成方法

一意の ID を生成する場合は`uuid`ライブラリが推奨されています。
こちらは`RFC4122`に従っており、衝突の可能性が限りなくゼロに近いです。
以下の手順に従って、`uuid`ライブラリを使用した一意の ID の生成方法を説明します。

1. インストール
   `uuid`ライブラリをインストールします。

   ```console: npm install
   $ npm install uuid
   ```

2. 実装
   `uuid`で一意の ID を生成します。
   ここでは以下の 2 パターンのコードで例示します。

   - 確認用に console 出力

   ```tsx: App.tsx
   import { v4 as uuidv4 } from 'uuid';

   // Generate a unique ID
   const uniqueId = uuidv4();
   console.log('A unique ID:', uniqueId);
   ```

   - 古典的な ToDo アプリの抜粋

   ```tsx: App.tsx
   const todoAddHandler = (text: string) => {
     // NOTE: prevTodos is the current todos copy
     setTodos((prevTodos) => [
       ...prevTodos,
       // Generate a unique ID
       { id: uuidv4(), text: text },
     ]);
   ```

   `v4`は`uuid`ライブラリでランダムな UUID を生成する関数の 1 つです。
   `uuidv4()`を呼び出すことで、一意の ID が生成されます。

3. React + TypeScript で実装する場合
   TypeScript では、上記に加えて`uuid`ライブラリを使用するための型定義ファイル`@types/uuid`ライブラリが必要です。

   ```console: npm i
   $ npm i --save-dev @types/uuid
   ```

   これで`uuid`ライブラリを TypeScript で使用できます。

4. 確認
   Console 出力のコードで、一意の ID が生成されるかを確認します。

   ```console: npm start
   $ npm start
   ```

   ![Screenshot from 2023-03-26 17-33-56.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/a6574fcf-8f91-966f-f05d-389d5eec749a.png)

   `A unique ID: a016f360-9ffa-4fdf-9245-f9a3b9d06e6d`が出力されました。
   再読込してみましょう。

   ![Screenshot from 2023-03-26 17-37-43.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/21fd08e2-0edb-d171-ec94-52debec896b0.png)

   `A unique ID: 88550175-e7da-4414-b981-30bc7bc5630a`に ID が変更されました。
   一意の ID が正常に出力されていますね。

## まとめ

- React では、一意の ID を`uuid`ライブラリで生成できます。
- React + Typescript で uuid ライブラリを使用する場合、以下の 2 コマンドが必要です。
  ```console:
  $ npm install uuid
  $ npm i --save-dev @types/uuid
  ```
- `uuid`を import して、一意の ID を生成する。

  ```App.tsx
  import { v4 as uuidv4 } from 'uuid';

  // Generate a unique ID
  const uniqueId = uuidv4();
  console.log('A unique ID:', uniqueId);
  ```

## 最後に

閲覧頂きありがとうございました。
備忘録の側面もありますが、本記事がお役に立てば幸いです！

## 参考 URL

[RFC4122](https://tex2e.github.io/rfc-translater/html/rfc4122.html 'RFC4122')
