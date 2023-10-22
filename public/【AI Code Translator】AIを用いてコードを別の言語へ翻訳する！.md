---
title: 【AI Code Translator】AIを用いてコードを別の言語へ翻訳する！
tags:
  - Java
  - Python
  - AI
  - ChatGPT
private: false
updated_at: '2023-10-22T15:44:23+09:00'
id: 741a056c5624dfd6adc1
organization_url_name: null
slide: false
ignorePublish: false
---

# AI Code Translator

`ChatGPT`が話題となり、ITエンジニア界隈は勿論の事メディアでもその名を聞くようになりました。
その中でも個人的に面白いと思った`AI Code Translator`を紹介します。

## 忙しい人へ

- [AI Code Translator](https://ai-code-translator.vercel.app/ 'AI Code Translator')とは？
  入力したコードを指定した言語へと翻訳するサービス。
  Python→Java等、現状39もの言語に対応。
- 使い方
  OpenAI（ChatGPT）のAPIキーを入力。
  `Input`プルダウンで翻訳元の言語を選択、コードをペースト。
  `Output`プルダウンで翻訳先の言語を選択。
  GPT バージョンを指定して`Translate`を実行。
- 感想
  視覚的なUIでレスポンスも早く快適。
  翻訳精度は概ね良好。
  Google翻訳やDeepL同様、参考にして書き換える用途には有用。
  新しく言語を学ぶ際の比較や書き換えに活躍しそう。

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## AI Code Translatorとは？

AIを使用して、入力したコードをある言語から別の言語に翻訳するサービスです。
例えばPythonで記述したコードをJavaに翻訳するといった機能になります。
以前から似たようなサービスは存在しており、個人的な経験としてはVB.NETを書いていた 2020 年当時、C#しか情報がなくてC#→VB.NETのサービスを何度か試しました(残念な結果に終わりましたが...)
なので上手く使えるサービスであれば、情報が不足している言語での解決策や、レガシーコードのリプレイスが加速しそうです！

それでは、[AI Code Translator](https://ai-code-translator.vercel.app/ 'AI Code Translator')の画面を見ていきましょう。視覚的でわかりやすいですね。
![Screenshot from 2023-04-01 11-15-29.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/40dd1f7b-b6b2-67f2-4eb9-a2bf127c0fad.png)
リリースされて間もないサービスで情報も少ないため、手順を紹介していきます。

## 実行手順

1.  `OpenAI（ChatGPT）`のAPIキーを入力します。
    APIキーは以下のリンクから取得できます。OpenAIのアカウント登録がまだの方は、事前に登録が必要です。
    [OpenAI APIkey](https://platform.openai.com/account/api-keys 'OpenAI API')
    ![Screenshot from 2023-04-01 11-15-29.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/e9fef7aa-499e-8359-666e-fdd067f734c7.jpeg)

1.  `Input`に翻訳元、`Output`に翻訳先の言語をプルダウン指定します。
    ここでは翻訳元をPython、翻訳先をVisual Basic.NETとします。
    ![Screenshot from 2023-04-01 11-36-15.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/79e2c3c7-83f0-7a2a-6494-70f91cd2be84.png)
    なお、AI Code Translatorでは現状39もの言語に対応しています。
    対応言語表は[こちら](#対応言語表)

1.  `Input`内に翻訳したいコードをペーストしましょう。
    ここではPythonでFizzBuzzをペーストします。
    ![Screenshot from 2023-04-01 10-27-32.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/e46380d2-56a4-dc2d-d249-cb555b1c4510.png)
    以下にコードを記載しますが、一般的なFizzBuzzから少しだけ複雑化しました。
    オプションで動的に開始と終了を設定できるようにしました。
    `-s`で開始ナンバー(デフォルトは 1)、`-e`で終了ナンバー(デフォルトは 15)の設定です。

        ```python:fizzbuzz.py
        import argparse


        def fizzbuzz(start, end):
            for num in range(start, end + 1):
                if num % 3 == 0 and num % 5 == 0:
                    print("FizzBuzz")
                elif num % 3 == 0:
                    print("Fizz")
                elif num % 5 == 0:
                    print("Buzz")
                else:
                    print(num)


        if __name__ == "__main__":
            parser = argparse.ArgumentParser(description="FizzBuzz")
            parser.add_argument(
                "--start",
                "-s",
                type=int,
                default=1,
                help="Starting number(default:1)",
            )
            parser.add_argument(
                "--end",
                "-e",
                type=int,
                default=15,
                help="Ending number(default:15)",
            )
            args = parser.parse_args()

            fizzbuzz(args.start, args.end)
        ```
        `AI Code Translator`のお手並み拝見ですね。

1.  GPTバージョンを指定して`Translate`を実行します。
    私は無課金ユーザーでGPT-4は試せないため、GPT-3.5で実行します。
    ![Screenshot from 2023-04-01 10-29-52.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/0ff0c897-a543-2dda-b3c2-4a2ab636bba9.png)
    実行結果は以下になります。

    ````visualbasic: fizzbuzz.vb
    Imports System

        Module Module1
            Sub Main()
                Dim start As Integer = 1
                Dim [end] As Integer = 15

                For num As Integer = start To [end]
                    If num Mod 3 = 0 AndAlso num Mod 5 = 0 Then
                        Console.WriteLine("FizzBuzz")
                    ElseIf num Mod 3 = 0 Then
                        Console.WriteLine("Fizz")
                    ElseIf num Mod 5 = 0 Then
                        Console.WriteLine("Buzz")
                    Else
                        Console.WriteLine(num)
                    End If
                Next
            End Sub
        End Module
        ```
        概ね良さそうですね！
        上記のPythonコードで少し意地悪をしたオプションが可変でなくなってしまっていますが、デフォルトの値を用いてくれています。

    ````

1.  動作確認をします。
    [OneCompiler](https://onecompiler.com/vb/3z4bkaqrq 'OneCompiler')で動作確認をしました。
    ![Screenshot from 2023-04-01 12-04-07.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/0458a4e9-aaff-46e0-93a0-bf9cf846f2d7.png)
    正常に動作していますね！

## 他の言語でも試してみる(Java)

気になったのでPython→Javaも試してみました。
![Screenshot from 2023-04-01 12-16-23.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/49461f1f-12de-74ac-622e-ce90878421f6.png)

```java: fizzbuzz.java
import java.util.Scanner;

public class FizzBuzz {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter starting number: ");
        int start = scanner.nextInt();
        System.out.print("Enter ending number: ");
        int end = scanner.nextInt();
        fizzbuzz(start, end);
    }

    public static void fizzbuzz(int start, int end) {
        for (int num = start; num <= end; num++) {
            if (num % 3 == 0 && num % 5 == 0) {
                System.out.println("FizzBuzz");
            } else if (num % 3 == 0) {
                System.out.println("Fizz");
            } else if (num % 5 == 0) {
                System.out.println("Buzz");
            } else {
                System.out.println(num);
            }
        }
    }
}
```

おお！おお？ 動作確認してみましょう。
![Screenshot from 2023-04-01 12-21-31.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/3b263204-e20b-9e8d-8eb7-515626fda963.png)

Scannerで標準入力から値を受け取っていますね！
一応動作は問題ないです。
外部ライブラリを用いるのは難しくとも、固定値かInteger.parseIntを使ったコードになるのではと考えていたので面白い結果でした。
個人開発や新しい言語の学習には役立ちそうに思います！
業務ではファイル数も多岐に渡るため使い方は限られますし、慣れていない言語へのリプレイスで参考程度になりそうですかね。
とはいえGoogle翻訳やDeepLも完璧ではないですし、使い方次第で化けそうな予感もします。
引き続き試していきたいと思います！

## まとめ

- [AI Code Translator](https://ai-code-translator.vercel.app/ 'AI Code Translator')は入力したコードを別の言語に翻訳するサービス。
- 英語ですがUIはわかりやすく、レスポンスも良く快適な使用感。
- 肝心の翻訳は概ね良好。ただし完璧とはいかない。
- 何らかの言語を覚えた人が、新しい言語の学習をするのに重宝しそう。

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## URL

[AI Code Translator](https://ai-code-translator.vercel.app/ 'AI Code Translator')
[AI Code Translator(GitHub)](https://github.com/mckaywrigley/ai-code-translator 'AI Code Translator(GitHub)')

### 対応言語表

| No. | 言語              |
| :-: | :---------------- |
|  1  | JavaScript        |
|  2  | TypeScript        |
|  3  | Python            |
|  4  | TSX               |
|  5  | JSX               |
|  6  | Go                |
|  7  | C                 |
|  8  | C++               |
|  9  | Java              |
| 10  | C#                |
| 11  | Visual Basic .NET |
| 12  | SQL               |
| 13  | Assembly Language |
| 14  | PHP               |
| 15  | Ruby              |
| 16  | Swift             |
| 17  | Kotlin            |
| 18  | R                 |
| 19  | Objective-C       |
| 20  | Perl              |
| 21  | Scala             |
| 22  | Dart              |
| 23  | Rust              |
| 24  | Haskell           |
| 25  | Lua               |
| 26  | Groovy            |
| 27  | Elixir            |
| 28  | Clojure           |
| 29  | Lisp              |
| 30  | Julia             |
| 31  | Matlab            |
| 32  | Fortran           |
| 33  | COBOL             |
| 34  | Bash              |
| 35  | Powershell        |
| 36  | PL/SQL            |
| 37  | CSS               |
| 38  | HTML              |
| 39  | NoSQL             |
