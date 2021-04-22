---
title: 最上位レベルのステートメント - C# チュートリアル
description: このチュートリアルでは、最上位レベルのステートメントを使用して、アイデアを探索しながら概念を試して証明する方法を示します
ms.date: 10/28/2020
ms.openlocfilehash: 60044b58fb52302cd559d7e8802657b602f1b6eb
ms.sourcegitcommit: 985c603cb21a085f8a8105f34ff5b87a44b76ab4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2021
ms.locfileid: "107564832"
---
# <a name="tutorial-explore-ideas-using-top-level-statements-to-build-code-as-you-learn"></a>チュートリアル: 学習しながらコードをビルドするために最上位レベルのステートメントを使用してアイデアを探索する

このチュートリアルで学習する内容は次のとおりです。

> [!div class="checklist"]
>
> - 最上位レベルのステートメントの使用を制御するルールについて学習する。
> - 最上位レベルのステートメントを使用してアルゴリズムを探索する。
> - 探索を再利用可能なコンポーネントにリファクタリングする。

## <a name="prerequisites"></a>必須コンポーネント

お使いのマシンを、.NET 5 が実行されるように設定する必要があります。これには C# 9 コンパイラが含まれます。 C# 9 コンパイラは [Visual Studio 2019 バージョン 16.9 プレビュー 1](https://visualstudio.microsoft.com/vs/preview/) または [.NET 5.0 SDK](https://dot.net/get-dotnet5) 以降で使用できます。

このチュートリアルでは、.NET と、C# と Visual Studio または .NET Core CLI のいずれかに精通していることを前提としています。

## <a name="start-exploring"></a>実際に使ってみる

最上位レベルのステートメントを使用すると、プログラムのエントリ ポイントをクラスの静的メソッドに配置することによって必要とされる余分な手続きを避けることができます。 新しいコンソール アプリケーションの一般的な開始点は、次のコードのようになります。

```csharp
using System;

namespace Application
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

上記のコードは、`dotnet new console` コマンドを実行し、新しいコンソール アプリケーションを作成した結果です。 これらの 11 行には、実行可能コードが 1 行しか含まれていません。 そのプログラムは、新しい最上位レベルのステートメント機能を使用して簡素化できます。 これにより、このプログラム内の 2 つを除くすべての行を削除できます。

```csharp
using System;

Console.WriteLine("Hello World!");
```

この機能により、新しいアイデアの探索を開始するために必要なものが簡素化されます。 最上位レベルのステートメントをスクリプト作成シナリオで、あるいは探索するために使用できます。 基本的な作業が完了したら、コードのリファクタリングを開始し、ビルドした再利用可能なコンポーネントに対してメソッド、クラス、またはその他のアセンブリを作成できます。 最上位レベルのステートメントを使用すれば、迅速な実験と初心者向けチュートリアルが可能になります。 また、実験から完全なプログラムへのスムーズなパスが得られます。

最上位レベルのステートメントは、ファイルに示される順序で実行されます。 最上位レベルのステートメントは、アプリケーション内の 1 つのソース ファイルのみで使用できます。 複数のファイルで使用すると、コンパイラでエラーが発生します。

## <a name="build-a-magic-net-answer-machine"></a>マジック .NET 回答マシンをビルドする

このチュートリアルでは、"はい" か "いいえ" の質問にランダムな回答で答えるコンソール アプリケーションをビルドします。 機能は少しずつビルドしていきます。 一般的なプログラムの構造に必要な手続きではなく、タスクに専念できます。 その後、機能に問題がなければ、必要に応じてアプリケーションをリファクタリングすることができます。

まず、質問をコンソールに書き戻すことをお勧めします。 まず、次のコードを記述します。

```csharp
using System;

Console.WriteLine(args);
```

`args` 変数は宣言しません。 最上位レベルのステートメントを含む単一のソース ファイルの場合、コンパイラでコマンドライン引数を意味する `args` が認識されます。 すべての C# プログラムの場合と同じように、引数の型は `string[]` となります。

次の `dotnet run` コマンドを実行して、コードをテストすることができます。

```dotnetcli
dotnet run -- Should I use top level statements in all my programs?
```

コマンド ラインの `--` の後の引数は、プログラムに渡されます。 `args` 変数の型を確認できます。それが、コンソールに出力された内容であるためです。

```console
System.String[]
```

コンソールに質問を書き込むには、引数を列挙し、それらをスペースで区切る必要があります。 `WriteLine` 呼び出しを次のコードに置き換えます。

:::code language="csharp" source="snippets/top-level-statements/ProgramSnippets.cs" ID="EchoInput":::

これで、プログラムを実行すると、質問が引数の文字列として正しく表示されるようになります。

## <a name="respond-with-a-random-answer"></a>ランダムな回答で答える

質問を問い返した後、ランダムな回答を生成するコードを追加できます。 まず、考えられる答えの配列を追加します。

:::code language="csharp" source="snippets/top-level-statements/ProgramSnippets.cs" ID="Answers":::

この配列に含まれる答えは、肯定的なものが 10 個、あいまいなものが 5 個、否定的なものが 5 個です。 次に、配列からランダムな回答を生成して表示する以下のコードを追加します。

:::code language="csharp" source="snippets/top-level-statements/ProgramSnippets.cs" ID="GenerateAnswer":::

アプリケーションをもう一度実行して、結果を確認することができます。 次の出力のようになるはずです。

```dotnetcli
dotnet run -- Should I use top level statements in all my programs?

Should I use top level statements in all my programs?
Better not tell you now.
```

このコードで質問に回答しますが、もう 1 つ機能を追加してみましょう。 たとえば、質問アプリを使用して、答えについての考え方をシミュレートしたいとします。 これは、ASCII アニメーションを少し追加し、作業を一時中断して行うことができます。  質問を問い返す行の後に、次のコードを追加します。

:::code language="csharp" source="snippets/top-level-statements/UtilitiesPassOne.cs" ID="AnimationFirstPass":::

`using` ステートメントをソース ファイルの先頭に追加する必要もあります。

```csharp
using System.Threading.Tasks;
```

`using` ステートメントは、ファイル内の他のどのステートメントよりも前にある必要があります。 それ以外の場合、コンパイラ エラーになります。 プログラムをもう一度実行して、アニメーションを表示することができます。 これにより、エクスペリエンスが向上します。 ご自分の好みに合わせて遅延の長さを試してください。

上のコードにより、スペースで区切られた一連の回転する行が作成されます。 `await` キーワードを追加すると、`async` 修飾子を持ち、<xref:System.Threading.Tasks.Task?displayProperty=nameWithType> を返すメソッドとしてプログラムのエントリ ポイントを生成するようにコンパイラに指示されます。 このプログラムからは値が返されないため、プログラムのエントリ ポイントにより `Task` が返されます。 プログラムから整数値が返された場合は、最上位レベルのステートメントの末尾に return ステートメントを追加します。 その return ステートメントにより、返す整数値が指定されます。 最上位レベルのステートメントに `await` 式が含まれている場合は、戻り値の型が <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> になります。

## <a name="refactoring-for-the-future"></a>今後のリファクタリング

プログラムのコードは次のようになるはずです。

```csharp
using System;
using System.Threading.Tasks;

Console.WriteLine();
foreach(var s in args)
{
    Console.Write(s);
    Console.Write(' ');
}
Console.WriteLine();

for (int i = 0; i < 20; i++)
{
    Console.Write("| -");
    await Task.Delay(50);
    Console.Write("\b\b\b");
    Console.Write("/ \\");
    await Task.Delay(50);
    Console.Write("\b\b\b");
    Console.Write("- |");
    await Task.Delay(50);
    Console.Write("\b\b\b");
    Console.Write("\\ /");
    await Task.Delay(50);
    Console.Write("\b\b\b");
}
Console.WriteLine();

string[] answers =
{
    "It is certain.",       "Reply hazy, try again.",     "Don't count on it.",
    "It is decidedly so.",  "Ask again later.",           "My reply is no.",
    "Without a doubt.",     "Better not tell you now.",   "My sources say no.",
    "Yes – definitely.",    "Cannot predict now.",        "Outlook not so good.",
    "You may rely on it.",  "Concentrate and ask again.", "Very doubtful.",
    "As I see it, yes.",
    "Most likely.",
    "Outlook good.",
    "Yes.",
    "Signs point to yes.",
};

var index = new Random().Next(answers.Length - 1);
Console.WriteLine(answers[index]);
```

上のコードは適切です。 正常に動作する。 しかし、再利用することはできません。 これでアプリケーションが動作するようになったので、次は再利用可能な部分を取得します。

候補の 1 つとして、待機中のアニメーションを表示するコードがあります。 このスニペットはメソッドになることがあります。

まず、ファイルにローカル関数を作成します。 現在のアニメーションを次のコードに置き換えます。

```csharp
await ShowConsoleAnimation();

static async Task ShowConsoleAnimation()
{
    for (int i = 0; i < 20; i++)
    {
        Console.Write("| -");
        await Task.Delay(50);
        Console.Write("\b\b\b");
        Console.Write("/ \\");
        await Task.Delay(50);
        Console.Write("\b\b\b");
        Console.Write("- |");
        await Task.Delay(50);
        Console.Write("\b\b\b");
        Console.Write("\\ /");
        await Task.Delay(50);
        Console.Write("\b\b\b");
    }
    Console.WriteLine();
}
```

前のコードで、main メソッド内にローカル関数が作成されています。 それでも再利用することはできません。 そのため、そのコードをクラスに抽出します。 *utilities.cs* という名前の新しいファイルを作成し、次のコードを追加します。

:::code language="csharp" source="snippets/top-level-statements/UtilitiesPassOne.cs" ID="SnippetUtilities":::

最上位レベルのステートメントが含まれているファイルには、そのファイルの最後の最上位レベルのステートメントの後に、名前空間と型を含めることもできます。 ただし、このチュートリアルでは別のファイルにアニメーション メソッドを配置して、より簡単に再利用できるようにします。

最後に、アニメーション コードをクリーンアップして、いくつか重複しているものを削除することができます。

:::code language="csharp" source="snippets/top-level-statements/Utilities.cs" ID="Animation":::

これでアプリケーションが完成し、後で使用するために再利用可能な部分がリファクタリングされました。 以下のメイン プログラムの完成版で示されているように、最上位レベルのステートメントから新しいユーティリティ メソッドを呼び出すことができます。

:::code language="csharp" source="snippets/top-level-statements/Program.cs":::

これにより、`Utilities.ShowConsoleAnimation` の呼び出しが追加され、別の `using` ステートメントが追加されます。

## <a name="summary"></a>まとめ

最上位レベルのステートメントを利用すると、新しいアルゴリズムを探索するのに使用するシンプルなプログラムをより簡単に作成できます。 異なるコード スニペットを試すことで、アルゴリズムを試してみることができます。 動作について学習したら、コードをより保守しやすいようにリファクタリングできます。

最上位レベルのステートメントを使用すると、コンソール アプリケーションに基づくプログラムが簡素化されます。 これには、Azure Functions、GitHub Actions、およびその他の小規模なユーティリティが含まれます。 詳細については、「[最上位レベルのステートメント (C# プログラミング ガイド)](../../programming-guide/main-and-command-args/top-level-statements.md)」を参照してください。
