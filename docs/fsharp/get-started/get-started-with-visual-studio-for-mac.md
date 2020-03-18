---
title: Visual Studio for Mac でのF#使用を開始する
description: を Visual Studio for Mac と共F#に使用する方法について説明します。
ms.date: 07/03/2018
ms.openlocfilehash: cd45ab9c59cef76e4bf85a93f39d8e2ee063d200
ms.sourcegitcommit: 93762e1a0dae1b5f64d82eebb7b705a6d566d839
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/27/2019
ms.locfileid: "74552369"
---
# <a name="get-started-with-f-in-visual-studio-for-mac"></a>Visual Studio for Mac でのF#使用を開始する

F#また、ビジュアルF#ツールは Visual Studio for Mac IDE でもサポートされています。 [Visual Studio for Mac がインストールされ](install-fsharp.md#install-f-with-visual-studio-for-mac)ていることを確認します。

## <a name="creating-a-console-application"></a>コンソールアプリケーションの作成

Visual Studio for Mac の最も基本的なプロジェクトの1つは、コンソールアプリケーションです。  これを行う方法を次に示します。  Visual Studio for Mac が開いたら、次のようになります。

1. **[ファイル]** メニューの **[新しいソリューション]** をポイントします。

2. [新しいプロジェクト] ダイアログには、コンソールアプリケーションに2つの異なるテンプレートがあります。  .NET Framework をターゲットとする > .NET の下に1つあります。  その他のテンプレートは .net core > アプリの下にあり、.NET Core を対象としています。  どちらのテンプレートも、この記事の目的で機能します。

3. [コンソールアプリ] でC# 、 F#必要に応じてをに変更します。  **[次へ]** ボタンをクリックして先に進みます。  

4. プロジェクトに名前を付け、アプリに必要なオプションを選択します。  プレビューウィンドウには、選択したオプションに基づいて作成されるディレクトリ構造が表示されます。  

5. **[作成]** をクリックします。  ソリューションエクスプローラーにF#プロジェクトが表示されます。

## <a name="writing-your-code"></a>コードの記述

まず、コードをいくつか記述してみましょう。  `Program.fs` ファイルが開いていることを確認し、その内容を次の内容に置き換えます。

[!code-fsharp[HelloSquare](~/samples/snippets/fsharp/getting-started/hello-square.fs)]

前のコードサンプルでは、`x` という名前の入力を受け取り、それを単独で乗算する関数 `square` が定義されています。  でF#は[型の推定](../language-reference/type-inference.md)が使用されるため、`x` の型を指定する必要はありません。  コンパイラF#は、乗算が有効な型を認識し、`square` の呼び出し方法に基づいて `x` に型を割り当てます。  `square`にマウスポインターを合わせると、次のように表示されます。

```console
val square: x:int -> int
```

これは、関数の型シグネチャとして知られています。  これは、次のように読み取ることができます。 "Square は、x という整数を受け取り、整数を生成する関数です。"  コンパイラによって現在の `int` 型が `square` されていることに注意してください。これは、乗算が*すべて*の型のジェネリックではなく、閉じられた型のセット全体でジェネリックであるためです。  コンパイラF#はこの時点で `int` を選択しましたが、`float`などの別の入力型を使用して `square` を呼び出すと、型シグネチャが調整されます。

別の関数 `main`が定義されています。これは、プログラムの実行F#を開始する必要があることをコンパイラに通知するために、`EntryPoint` 属性で修飾されています。  これは、コマンドライン引数をこの関数に渡すことができる他の[C スタイルのプログラミング言語](https://en.wikipedia.org/wiki/Entry_point#C_and_C.2B.2B)と同じ規則に従い、整数のコードが返されます (通常は `0`)。

この関数は、`12`の引数を使用して `square` 関数を呼び出します。  次F#に、コンパイラは `square` の型を `int -> int` に割り当てます。これは、`int` を受け取り、`int`を生成する関数です。  `printfn` の呼び出しは、C スタイルのプログラミング言語に似た書式指定文字列を使用する書式設定された印刷関数で、書式指定文字列で指定されたパラメーターに対応するパラメーターで、結果と改行を出力します。

## <a name="running-your-code"></a>コードの実行

上部のメニューから **[実行]** をクリックし、 **[デバッグなしで開始]** をクリックして、コードを実行し、結果を確認できます。  これにより、デバッグなしでプログラムが実行され、結果を確認できるようになります。

コンソールウィンドウに次の出力が表示され、Visual Studio for Mac ポップアップされます。

```console
12 squared is 144!
```

おめでとうございます!  Visual Studio for Mac で最初F#のプロジェクトを作成し、その関数F#を呼び出した結果を出力する関数を記述し、プロジェクトを実行して結果を確認しました。

## <a name="using-f-interactive"></a>対話型F#の使用

Visual Studio for Mac のビジュアルF#ツールの最適な機能の1つは、 F#対話型ウィンドウです。  このメソッドを使用すると、コードを呼び出して結果を対話形式で確認できるプロセスにコードを送信できます。

使用を開始するには、コードで定義されている `square` 関数を強調表示します。  次に、トップレベルメニューの **[編集]** をクリックします。  次**に、[選択F#範囲を対話型に送信**] を選択します。  これにより、 F#対話型ウィンドウのコードが実行されます。  または、選択範囲を右クリックし、 **[選択項目をF#対話形式で送信]** を選択します。  F#対話型ウィンドウが表示され、次のようなウィンドウが表示されます。

```console
>

val square : x:int -> int

>
```

これは、関数の上にマウスポインターを置いたときに表示される、`square` 関数の関数シグネチャと同じです。  `square` がF#対話型プロセスで定義されるようになったため、異なる値を使用して呼び出すことができます。

```console
> square 12;;
val it : int = 144
>square 13;;
val it : int = 169
```

これにより、関数が実行され、結果が `it`新しい名前にバインドされ、`it`の型と値が表示されます。  各行は `;;`で終了する必要があることに注意してください。  これは、 F#関数呼び出しが終了したことを対話形式で認識する方法です。  対話型でF#新しい関数を定義することもできます。

```console
> let isOdd x = x % 2 <> 0;;

val isOdd : x:int -> bool

> isOdd 12;;
val it : bool = false
```

上の例では、`isOdd`という新しい関数が定義されています。この関数は、`int` を受け取り、それが奇数かどうかを確認します。  この関数を呼び出すと、異なる入力で何が返されるかを確認できます。  関数呼び出しの中で関数を呼び出すことができます。

```console
> isOdd (square 15);;
val it : bool = true
```

[パイプ転送演算子](../language-reference/symbol-and-operator-reference/index.md)を使用して、値を次の2つの関数にパイプライン処理することもできます。

```console
> 15 |> square |> isOdd;;
val it : bool = true
```

パイプ転送演算子などの詳細については、以降のチュートリアルで説明します。

これは、対話型で実行できることを確認するF#ことだけを目的としています。  詳細については、を[使用しF#た対話型プログラミングに関する](../tutorials/fsharp-interactive/index.md)ページをご覧ください。

## <a name="next-steps"></a>次のステップ:

このF#言語の主要機能の一部については、「」 [ F#のツアー](../tour.md)をご覧ください。  ここでは、のF#一部の機能の概要について説明し、Visual Studio for Mac にコピーして実行できるコードサンプルをいくつか紹介します。  この[ F#ガイド](../index.yml)で紹介している、使用できる優れた外部リソースもあります。

## <a name="see-also"></a>参照

- [F#ルビ](../index.yml)
- [F# のツアー](../tour.md)
- [F#言語リファレンス](../language-reference/index.md)
- [型の推論](../language-reference/type-inference.md)
- [シンボルと演算子のリファレンス](../language-reference/symbol-and-operator-reference/index.md)
