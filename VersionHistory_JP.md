# Version History

## Version 1.2.1

+ macOSにて、`SFTextFormatter.cs`より発生していたコンパイルエラーの修正

## Version 1.2.0

このアップデートは、以下の通り、新たな機能と変更を含みます。

+ Localized SFText：ローカライズ（複数言語でストーリーを作る）のための新たなシナリオスクリプト
+ SFText Utilityの新たなコマンド
+ SFText Formatterの、自由にフォントを使用するための新たな設定
+ SFText構文の小さな変更：文法エラーがより厳しくチェックされる
+ バグ修正

また、Visual Studio Codeの拡張機能が次の通りにアップデートされます。

+ SFText Syntax: 1.2.0
+ SFText Formatter: 1.1.0
+ SFTExt Utility: 1.1.0

最新バージョンを使用していることを確認してください。

### Localized SFText

[詳細はここを確認](./LocalizedSFText_JP.md)

### SFText Utilityの新たなコマンド

以下の、新しい、もしくは機能修正されたコマンドがSFTextの編集で利用可能です。

| コマンド | ショートカットキー (windows) | 機能 |
| --- | --- | --- |
| Insert Scope Below | Ctrl+Enter | スコープの下に新しい行を挿入する |
| Insert Scope Above | Ctrl+Shift+Enter | スコープの上に新しい行を挿入する |
| Toggle Line Comment | Ctrl+/ | 選択範囲の各行をコメントアウトまたはアンコメントする |
| Toggle Scope Comment | Ctrl+Shift+/ | 選択範囲のスコープ開始行をコメントアウトまたはアンコメントする |

実際は、他にも新たに利用可能なコマンドがあります。ただし、それらはlocalized SFTextに関連するもののため、[localized SFTextのページ](./LocalizedSFText_JP.md)で説明されています。

### 好きなフォントを使用するためのSFTextフォーマッタの設定

新しい二つの設定項目、`Half Width Constant`（半角定数）と`Full Width Constant`（全角定数）に適切な値を設定することで、好きなフォントを使用することができます。これらの設定では、テキストを整列するために必要な半角文字と全角文字の数をそれぞれ指定します。

![](./Images/Formatter/FormatterSettings.png)

例えば、デフォルトのフォント("Consolas, 'Courier New', monospace")では、半角定数は20、全角定数は11とします。これは、**20の半角文字と、11の全角文字がちょうど整列する**ためです。

![](./Images/Formatter/DefaultFontAlignment.png)

半角・全角文字の幅はフォントによって異なるため、他のフォントを使用するときには、他の値を割り当てるかもしれません。例えば、"BIZ UD Gothic" フォントの場合、半角定数を2に、全角定数を1に設定すると、正しい整列ができます。

![](./Images/Formatter/BIZUD_Alignment.png)

指定された半角・全角定数が適切でない場合、SFTextフォーマッタは正しくSFTextをフォーマットできないということに注意してください。

> [!IMPORTANT]
> もし、適切な値が（上の画像に示される）三つの設定に設定されていることが確信できる一方、SFTextがフォーマッタによって適切に整列されない場合、その問題は使用しているフォントそれ自体によるものかもしれません。フォーマッタが、デフォルトのフォントではいくつかのケースでうまく働かず、その問題はフォント自体の問題であることが確認されています。
>
> そのようなケースでは、整列の問題を回避するために別のフォントを使用した方が良いです。

> [!TIP]
> 特定のフォントをSFTextを書く際に使用したいが、他のスクリプトの記述では異なるフォントを使用したい場合、以下のように設定ファイルを変更することで、SFTextを編集するときのみに有効なフォントを指定することができます。
> 
> ```json
> {
>     "[sftext]": {
>         "editor.fontFamily": "font-you-want-to-use",
>     }
> }
> ```

### より厳しい文法チェック

いくつかの変更が、SFTextの構文へなされました。ほとんどの場合、既存のSFTextを修正する必要はありませんが、SFTextの構文解析器は文法的なミスをより厳しくチェックします。

ここでは、2つの例を紹介します。

まず、前のバージョンでは、以下に示すよう、SFTextの各行に余分な縦線を置くことが許されていました。しかし、新しいバージョンでは、このケースにおいては文法エラーが通知されます。

```
$parallel | command name | comment  | | <-- Two redendant vertical bars here
```

二つ目の例として、前のバージョンでは、スコープは空行で終わらせることができ、その場合新たなコメントスコープが開始していました。新たなバージョンでは、このケースでは、スコープ宣言部を`//`で開始して、コメントスコープを明示的に開始する必要があります。

Second, in the previous version, a scope could be finished by a empty line and a new comment scope would start. But in the new version, in this case, we have to start a comment scope explicitly by starting the scope declaration part with `//`.

```
$parallel | command name         | 
          | {Arg1}               |
          | Not in comment scope | <-- A comment scope doesn't start from here
//        | In comment scope     | 
```

繰り返しになりますが、構文に劇的な変化はありません。ただし、より厳しい文法チェックによるいくつかの文法エラーに遭遇するかもしれません。

## Version 1.1.0

<details>

<summary>
詳細はここをクリック
</summary>

バージョン1.1.0での変更は、主にSFTextに関するものです。

### 改行シンボル

バージョン1.1.0では、会話スコープ中の複数のテキストを特定の文字とともに結合するために、他の文字と置き換えるためのシンボルを定義し、そのシンボルをスクリプトのいたるところに書く必要がありました。

例えば、空白でテキストを結合する場合：

シンボル"\<sp>"を定義し、string型用のデコーダーもしくはセリフを表示する用のコマンド中でそのシンボルを置き換えます。

```cs
// Symbols in a script have to be replaced with other characters in a string decoder or a method that displays a dialogue line

// String decoder
[DecoderMethod]
public string ConvertToString(string input)
{
    return input.Replace("<sp>", " ");
}

// Command that displays a dialogue line
[CommandMethod("display dialogue")]
public UniTask DisplayDialogueAsync(string name, string line, CancellationToken cancellationToken)
{
    line = line.Replace("<sp>", " ");

    // Display a dialogue line
    // ...
}
```

![](./Images/VersionHistory/v1.1.0/Space.v1.0.0.png)

バージョン1.1.0では、スクリプトのインポート時、"line break（改行）"を意味する新たなシンボル"\<bk>"が、会話スコープ中に記述された複数のテキストの間に挿入されます。そのため、シンボルを定義したりそれをスクリプト中に繰り返し入力する必要はなく、シンボルをどのように置き換えるのかを定義するだけで済みます。

シンボルの置き換えについては、SFTextクラスで定義された静的かつ読み込み専用の`SFText.LineBreakSymbol'を参照することができます。

```cs
// Symbols in a script have to be replaced with other characters in a string decoder or a method that displays a dialogue line
// You can use 'SFText.LineBreakSymbol', which is a static and read-only member of the 'SFText' class

// String decoder
[DecoderMethod]
public string ConvertToString(string input)
{
    return input.Replace(SFText.LineBreakSymbol, " ");
}

// Command that displays a dialogue line
[CommandMethod("display dialogue")]
public UniTask DisplayDialogueAsync(string name, string line, CancellationToken cancellationToken)
{
    line = line.Replace(SFText.LineBreakSymbol, " ");

    // Display a dialogue line
    // ...
}
```

![](./Images/VersionHistory/v1.1.0/Space.v1.1.0.png)

### コメントスコープ

バージョン1.0.0では、"//"で始まるコメントは他のスコープに影響を与えませんでした。

![](./Images/VersionHistory/v1.1.0/Comment.v1.0.0.png)


バージョン1.1.0では、"//"で始まるコメントはスコープを終了させ、その行からは「コメントスコープ」がスタートします。また、それは他のスコープ同様、他のスコープが開始するまで続きます。コメントスコープでは、コンテンツ記述部にコメントを書くことができます。

![](./Images/VersionHistory/v1.1.0/Comment.v1.1.0.png)

### スコープ終了のための空行

バージョン1.1.0では、一つのスコープは次のスコープが始まるまで持続しました。

![](./Images/VersionHistory/v1.1.0/Empty.v1.0.0.png)

バージョン1.1.0では、空行を書くと、他のスコープを開始するときと同様、一つのスコープが終了します。そして、次の行からはコメントスコープが開始します。SFTextでの空行とは、そのスコープ宣言部とコンテンツ記述部の両方が空もしくは空白の行を指します。

![](./Images/VersionHistory/v1.1.0/Empty.v1.1.0.png)

### その他

+ アプリケーションビルドエラーに関する不具合が修正されます
+ UnityエディターにおけるSFTextの外見が改善されます

</details>