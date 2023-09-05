---
title: React + TypeScriptで一意のIDを生成
tags:
  - JavaScript
  - math
  - TypeScript
  - UUID
  - React
private: false
updated_at: '2023-03-30T19:35:14+09:00'
id: f99f3499aa586ddcd02b
organization_url_name: null
slide: false
---
# 結論
__「`Math.random()`ではなく`uuid`を使用して一意のIDを生成しましょう」__

## Introduction
Reactでアプリケーションを開発する際に、一意のIDが必要になることがあります。
例えば`Math.random()`で生成した文字列をIDとして使用できそうです。
ただ、この文字列は疑似乱数のため重複する懸念があり、一意のIDとしては推奨されません。
業務で使っている`mlflow`では`uuid`が使われていることを思い出し、調べてみました。
Reactでも使えそうです。

__本記事が少しでも読者様の学びに繋がれば幸いです！__
__「いいね」をしていただけると今後の励みになるので、是非お願いします！__

## uuidライブラリを使用したIDの生成方法
一意のIDを生成する場合は`uuid`ライブラリが推奨されています。
こちらは`RFC4122`に従っており、衝突の可能性が限りなくゼロに近いです。
以下の手順に従って、`uuid`ライブラリを使用した一意のIDの生成方法を説明します。

1. インストール
    `uuid`ライブラリをインストールします。
    ```console: npm install
    $ npm install uuid
    ```

2. 実装
    `uuid`で一意のIDを生成します。
    ここでは以下の2パターンのコードで例示します。
    - 確認用にconsole出力
    ```tsx: App.tsx
    import { v4 as uuidv4 } from 'uuid';

    // Generate a unique ID
    const uniqueId = uuidv4();
    console.log('A unique ID:', uniqueId);
    ```    
    - 古典的なToDoアプリの抜粋
    ```tsx: App.tsx
    const todoAddHandler = (text: string) => {
      // NOTE: prevTodos is the current todos copy
      setTodos((prevTodos) => [
        ...prevTodos,
        // Generate a unique ID
        { id: uuidv4(), text: text },
      ]);
    ```
    `v4`は`uuid`ライブラリでランダムなUUIDを生成する関数の1つです。
    `uuidv4()`を呼び出すことで、一意のIDが生成されます。

3. React + TypeScriptで実装する場合
    TypeScriptでは、上記に加えて`uuid`ライブラリを使用するための型定義ファイル`@types/uuid`ライブラリが必要です。
    ```console: npm i
    $ npm i --save-dev @types/uuid
    ```
    これで`uuid`ライブラリをTypeScriptで使用できます。

4. 確認
    Console出力のコードで、一意のIDが生成されるかを確認します。
    ```console: npm start
    $ npm start
    ```
    ![Screenshot from 2023-03-26 17-33-56.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/a6574fcf-8f91-966f-f05d-389d5eec749a.png)

   `A unique ID: a016f360-9ffa-4fdf-9245-f9a3b9d06e6d`が出力されました。
    再読込してみましょう。

    ![Screenshot from 2023-03-26 17-37-43.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/21fd08e2-0edb-d171-ec94-52debec896b0.png)



   `A unique ID: 88550175-e7da-4414-b981-30bc7bc5630a`にIDが変更されました。
    一意のIDが正常に出力されていますね。

## まとめ
 - Reactでは、一意のIDを`uuid`ライブラリで生成できます。
 - React + Typescriptでuuidライブラリを使用する場合、以下の2コマンドが必要です。
    ```console: 
    $ npm install uuid
    $ npm i --save-dev @types/uuid
    ```
 - `uuid`をimportして、一意のIDを生成する。
    ```App.tsx
    import { v4 as uuidv4 } from 'uuid';

    // Generate a unique ID
    const uniqueId = uuidv4();
    console.log('A unique ID:', uniqueId);
    ```

## 最後に
閲覧頂きありがとうございました。
備忘録の側面もありますが、本記事がお役に立てば幸いです！

## 参考URL
[RFC4122](https://tex2e.github.io/rfc-translater/html/rfc4122.html "RFC4122")


