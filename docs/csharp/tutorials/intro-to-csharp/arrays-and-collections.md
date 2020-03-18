---
title: コレクションでの作業 - C# チュートリアルの概要
description: このチュートリアルでは、リスト コレクションについて確認して C# を学習します。
ms.date: 10/13/2017
ms.custom: mvc
ms.openlocfilehash: 25d20de2eae8ad1f544fa17553c173a6141ae464
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79156690"
---
# <a name="learn-to-manage-data-collections-using-the-generic-list-type"></a>リスト型を使用したデータ コレクションの管理について説明します

このチュートリアルでは、C# 言語の概要と <xref:System.Collections.Generic.List%601> クラスの基本を説明します。

このチュートリアルでは、開発用に使用できるマシンがあることを想定しています。 Windows、Linux、または macOS 上でローカルの開発環境を設定する手順については、.NET チュートリアル [Hello World in 10 minutes](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/intro) (10 分で Hello World) に記載されています。 使用するコマンドの概要については、詳細な情報へのリンクが掲載されている[開発ツールに対する理解を深める](local-environment.md)方法に関するページをご覧ください。

## <a name="a-basic-list-example"></a>基本のリストの例

*list-tutorial* という名前のディレクトリを作成します。 それを現在のディレクトリとし、`dotnet new console` を実行します。

好みのエディターで *Program.cs* を開き、既存のコードを次のコードで置き換えます。

```csharp
using System;
using System.Collections.Generic;

namespace list_tutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            var names = new List<string> { "<name>", "Ana", "Felipe" };
            foreach (var name in names)
            {
                Console.WriteLine($"Hello {name.ToUpper()}!");
            }
        }
    }
}
```

`<name>` をユーザー名で置き換えます。 *Program.cs* を保存します。 コンソール ウィンドウに「`dotnet run`」と入力して試します。

文字列のリストを作成し、そのリストに 3 つの名前を追加し、それらの名前をすべて大文字で出力しました。 先のチュートリアルで学習した概念を使用して、リストをループしています。

名前を表示するコードは、[文字列補間](../../language-reference/tokens/interpolated.md)機能を使用します。  `string` の前に文字 `$` を配置すると、文字列宣言に C# コードを埋め込むことができます。 実際の文字列は、生成する値でその C# コードを置き換えます。 この例では、`{name.ToUpper()}` メソッドを呼び出したため、文字列は <xref:System.String.ToUpper%2A> をそれぞれの名前に置き換え、文字を大文字に変換しています。

続けて確認していきましょう。

## <a name="modify-list-contents"></a>リスト コンテンツを変更する

作成したコレクションは <xref:System.Collections.Generic.List%601> 型を使用します。 この型は、要素のシーケンスを格納します。 要素の型を山かっこの内側で指定します。

この <xref:System.Collections.Generic.List%601> 型の重要な点は増減が可能で、要素を追加したり削除したりできることです。 `}` メソッドの閉じかっこ `Main` の前に、次のコードを追加します。

```csharp
Console.WriteLine();
names.Add("Maria");
names.Add("Bill");
names.Remove("Ana");
foreach (var name in names)
{
    Console.WriteLine($"Hello {name.ToUpper()}!");
}
```

さらに 2 つの名前をリストの末尾に追加しました。 また、1 つを削除しました。 ファイルを保存し、「`dotnet run`」と入力して試します。

<xref:System.Collections.Generic.List%601> を使用すると、**インデックス**でも個々の項目を参照できます。 リスト名に続く `[` と `]` のトークンの間にインデックスを記述します。 C# では、初めのインデックスには 0 を使用します。 追加したコードのすぐ下に次のコードを追加して試します。

```csharp
Console.WriteLine($"My name is {names[0]}");
Console.WriteLine($"I've added {names[2]} and {names[3]} to the list");
```

リストの末尾を越えてインデックスにアクセスすることはできません。 インデックスは 0 から始まるため、有効なインデックスの最大値はリスト内の項目の数より 1 小さくなることに注意してください。 <xref:System.Collections.Generic.List%601.Count%2A> プロパティを使用すれば、リストの長さを確認できます。 次のコードを Main メソッドの末尾に追加します。

```csharp
Console.WriteLine($"The list has {names.Count} people in it");
 ```

ファイルを保存し、もう一度「`dotnet run`」と入力して結果を確認します。

## <a name="search-and-sort-lists"></a>リストを検索して並び替える

サンプルでは比較的小さいリストを使用していますが、ご利用のアプリケーションでは、より多くの (場合によっては何千もの) 要素が含まれるリストを作成することもよくあるかもしれません。 そうした大規模なコレクションの中から要素を見つけるには、別々の項目をリストで検索する必要があります。 <xref:System.Collections.Generic.List%601.IndexOf%2A> メソッドは項目を検索し、その項目のインデックスを返します。 `Main` メソッドの下部に次のコードを追加します。

```csharp
var index = names.IndexOf("Felipe");
if (index == -1)
{
    Console.WriteLine($"When an item is not found, IndexOf returns {index}");
}
else
{
    Console.WriteLine($"The name {names[index]} is at index {index}");
}

index = names.IndexOf("Not Found");
if (index == -1)
{
    Console.WriteLine($"When an item is not found, IndexOf returns {index}");
}
else
{
    Console.WriteLine($"The name {names[index]} is at index {index}");

}
```

同じように、リスト内の項目を並び替えできます。 <xref:System.Collections.Generic.List%601.Sort%2A> メソッドは、リスト内のすべての項目を正規順序 (文字列の場合はアルファベット順) で並び替えます。 `Main` メソッドの下部に次のコードを追加します。

```csharp
names.Sort();
foreach (var name in names)
{
    Console.WriteLine($"Hello {name.ToUpper()}!");
}
```

ファイルを保存し、「`dotnet run`」と入力してこの最新バージョンを試します。

次のセクションを開始する前に、現在のコードを別のメソッドに移動してみましょう。 移動しておくと、新しい例で作業を開始するときに楽になります。 `Main` メソッドの名前を `WorkingWithStrings` に変更し、`Main` を呼び出す新しい `WorkingWithStrings` メソッドを記述します。 完成したコードは次のようになります。

```csharp
using System;
using System.Collections.Generic;

namespace list_tutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            WorkingWithStrings();
        }

        static void WorkingWithStrings()
        {
            var names = new List<string> { "<name>", "Ana", "Felipe" };
            foreach (var name in names)
            {
                Console.WriteLine($"Hello {name.ToUpper()}!");
            }

            Console.WriteLine();
            names.Add("Maria");
            names.Add("Bill");
            names.Remove("Ana");
            foreach (var name in names)
            {
                Console.WriteLine($"Hello {name.ToUpper()}!");
            }

            Console.WriteLine($"My name is {names[0]}");
            Console.WriteLine($"I've added {names[2]} and {names[3]} to the list");

            Console.WriteLine($"The list has {names.Count} people in it");

            var index = names.IndexOf("Felipe");
            if (index == -1)
            {
                Console.WriteLine($"When an item is not found, IndexOf returns {index}");
            }
            else
            {
                Console.WriteLine($"The name {names[index]} is at index {index}");
            }

            index = names.IndexOf("Not Found");
            if (index == -1)
            {
                Console.WriteLine($"When an item is not found, IndexOf returns {index}");
            }
            else
            {
                Console.WriteLine($"The name {names[index]} is at index {index}");

            }

            names.Sort();
            foreach (var name in names)
            {
                Console.WriteLine($"Hello {name.ToUpper()}!");
            }
        }
    }
}
```

## <a name="lists-of-other-types"></a>その他の型のリスト

ここまでは、リスト内で `string` 型を使用してきました。 別の型を使用して <xref:System.Collections.Generic.List%601> を作成してみましょう。 数値のセットを作成します。

新しい `Main` メソッドの下部に次のコードを追加します。

```csharp
var fibonacciNumbers = new List<int> {1, 1};
```

これにより整数のリストが作成され、最初の 2 つの整数が値 1 に設定されます。 これらは、数列の 1 つである*フィボナッチ数列*の最初の 2 つの値です。 次のフィボナッチ数はそれぞれ、その直前の 2 つの数値の合計を取得することによって得られます。 このコードを追加します。

```csharp
var previous = fibonacciNumbers[fibonacciNumbers.Count - 1];
var previous2 = fibonacciNumbers[fibonacciNumbers.Count - 2];

fibonacciNumbers.Add(previous + previous2);

foreach (var item in fibonacciNumbers)
    Console.WriteLine(item);
```

ファイルを保存し、「`dotnet run`」と入力して結果を確認します。

> [!TIP]
> このセクションにだけ集中したいときは、`WorkingWithStrings();` を呼び出すコードはコメント アウトしてかまいません。 `/` のように、呼び出しの前に `// WorkingWithStrings();` 文字を 2 つ記述します。

## <a name="challenge"></a>課題

このレッスンと以前のレッスンの中から、いくつかの概念を理解できているかどうかを確認してみましょう。 ここまでフィボナッチ数を使用して作成してきたコードを使ってください。 シーケンスの最初の 20 個の数を生成するコードを記述してみましょう。 (ヒント: フィボナッチ数の 20 番目の数は 6765 です。)

## <a name="complete-challenge"></a>課題完了

[GitHub にある完成したサンプル コードを表示すること](https://github.com/dotnet/samples/tree/master/csharp/list-quickstart/Program.cs#L13-L23)で、ソリューションの例を確認できます。

ループの繰り返しごとに、リストの最後の 2 つの整数を取得して合計し、その値をリストに追加しています。 このループは、20 個の項目がリストに追加されるまで繰り返されます。

おつかれさまでした。リストについてのチュートリアルはこれで終了です。 続けて独自の開発環境で[クラスの概要](introduction-to-classes.md)のチュートリアルに進むことができます。

`List` 型の使用方法の詳細については、[.NET ガイド](../../../standard/index.md)の[コレクション](../../../standard/collections/index.md)に関するトピックで学習できます。 その他の多くのコレクション型についても学習できます。
