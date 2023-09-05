---
title: React Tutorial備忘録
tags:
  - JavaScript
  - React
private: false
updated_at: '2023-04-02T10:38:21+09:00'
id: 730312f959032ec00428
organization_url_name: null
slide: false
---
# Introduction
`React`未経験の私が学んだことを書き連ねます。
公式チュートリアルをベースに学習しています。

__本記事が少しでも読者様の学びに繋がれば幸いです！__
__「いいね」をしていただけると今後の励みになるので、是非お願いします！__

## renderとは?
- コンポーネントの出力（UIの表示）を行うためのメソッド。
- 各コンポーネントはプロパティ(`props`)および状態(`state`)を受け取り、UIをレンダリングします。

## propsとは?
- 親コンポーネントから子コンポーネントに渡される値のこと。
- 読み取り専用。
- コンポーネント内では変更不可。
- コンポーネント間の情報の受け渡しに使用されます。

## stateとは？
- `Reactコンポーネント`内で管理される変数のこと。
- コンポーネントの状態を表し、ユーザーの操作やイベントに応じて動的に変更可能。
- コンポーネントが再レンダリングされる際に更新され、UIに反映されます。
- コンポーネントの`constructor`内で初期化され、`setState`メソッドで変更されます。

```console
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }
  render() {
    return (
      <button
        className='square'
        onClick={() => {
          this.setState({ value: 'X' });
        }}
      >
        {this.state.value}
      </button>
    );
  }
}
```

## UIをレンダリングとは？
- `Reactコンポーネント`を基に、ブラウザ上にHTML要素を生成してWebページを構築すること。
- `render`メソッドからJSXを通じて、ブラウザ上に表示される要素が生成されます。
- これにより、`Reactコンポーネント`を使用してWebページのUIを構築することができます。

## イベントハンドラの仕様
- `React`では、イベントハンドラ内の`this`は自動的にバインドされません。
- 以下のようなコードでは、`this.props.value`が参照できずにエラーになります。
```console
<button
  className='square'
  onClick={function () {
    console.log(this.props.value);
  }}
>
```
- アロー関数を用いて、イベントハンドラの定義が必要です。
- 以下のようなコードであれば、クリックイベント内の`this`が正しくバインドされ、`this.props.value`の参照が可能になります。
```console
<button
  className='square'
  onClick={() => {
    console.log(this.props.value);
  }}
>

```

## 参考URL
[React tutorial](https://ja.reactjs.org/tutorial/tutorial.html "React Tutorial")
