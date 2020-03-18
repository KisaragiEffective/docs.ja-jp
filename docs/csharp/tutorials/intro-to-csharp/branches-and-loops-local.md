---
title: 分岐とループ - C# の概要に関するチュートリアル
description: 分岐とループに関するこのチュートリアルでは、C# のコードを記述して、この言語における、ステートメントを繰り返し実行するための条件付き分岐とループに対応している構文について学習します。
ms.date: 10/31/2017
ms.custom: mvc
ms.openlocfilehash: 44b634e3c2120116ee7fd66770398a6b66c8ed8c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73739130"
---
# <a name="learn-conditional-logic-with-branch-and-loop-statements"></a>分岐およびループ ステートメントを使用した条件付きロジックについて説明します

このチュートリアルでは、変数を調べ、その変数に基づいて実行パスを変更するコードを記述する方法について説明します。 C# コードを記述し、コードをコンパイルおよび実行して結果を確認します。 チュートリアルには、C# における分岐構造とループ構造を確認する一連のレッスンが含まれています。 これらのレッスンでは、C# 言語の基本を説明します。

このチュートリアルでは、開発用に使用できるマシンがあることを想定しています。 Windows、Linux、または macOS 上でローカルの開発環境を設定する手順については、.NET チュートリアル [Hello World in 10 minutes](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/intro) (10 分で Hello World) に記載されています。 使用するコマンドの概要については、[開発ツールの概要](local-environment.md)のページと詳細へのリンクをご覧ください。

## <a name="make-decisions-using-the-if-statement"></a>`if` ステートメントを使用した条件判定

「*branches-tutorial*」という名前のディレクトリを作成します。 それを現在のディレクトリにして、次のコマンドを実行します。

```dotnetcli
dotnet new console -n BranchesAndLoops -o .
```

このコマンドによって、現在のディレクトリに新しい .NET Core コンソール アプリケーションが作成されます。

お好みのエディターで *Program.cs* を開き、`Console.WriteLine("Hello World!");` の行を次のコードで置き換えます。

```csharp
int a = 5;
int b = 6;
if (a + b > 10)
    Console.WriteLine("The answer is greater than 10.");
```

コンソール ウィンドウで「`dotnet run`」と入力し、このコードを試します。 "答えは 10 を超えています" というメッセージが コンソールに出力されるはずです。

合計が 10 未満になるように `b` の宣言を変更します。

```csharp
int b = 3;
```

もう一度「`dotnet run`」を入力します。 計算結果が 10 未満であるため、何も出力されません。 判定中の**条件**が false になっています。 `if` ステートメントの考えられる分岐の 1 つである true 分岐のみを記述したため、実行すべきコードがありません。

> [!TIP]
> C# (または何らかのプログラミング言語) について詳しく学習するに従い、コードを記述する際にミスをすることもあるでしょう。 コンパイラは、エラーを発見して報告します。 エラーの出力とそのエラーを生成したコードをよく確認してください。 通常、コンパイラ エラーは問題を発見するのに役立ちます。

この最初のサンプルは、`if` とブール型の機能を示しています。 *ブール値*は、`true` または `false` という 2 つの値のいずれかを持つことができる変数です。 C# ではブール変数の専用の型として `bool` を定義しています。 `if` ステートメントは、`bool` の値を確認します。 値が `true` の場合、`if` に続くステートメントを実行します。 それ以外の場合はスキップします。

条件を確認してその条件に基づいてステートメントを実行するというこのプロセスは非常に機能的です。

## <a name="make-if-and-else-work-together"></a>if と else を組み合わせた使用

true 分岐と false 分岐で別々のコードを実行するには、条件が false のときに実行する `else` 分岐を作成します。 これを試してみます。 次のコードの最後の 2 行を `Main` メソッドに追加します (最初の 4 行は既にあるはずです)。

```csharp
int a = 5;
int b = 3;
if (a + b > 10)
    Console.WriteLine("The answer is greater than 10");
else
    Console.WriteLine("The answer is not greater than 10");
```

`else` キーワードに続くステートメントは、判定中の条件が `false` のときにのみ実行されます。 `if` と `else` をブール条件と組み合わせれば、`true` と `false` の条件を両方処理するのに必要な機能がすべて整います。

> [!IMPORTANT]
> `if` と `else` のステートメント内にあるインデントは、人が読みやすいようにするためのものです。
> C# 言語はインデントや空白文字を重要なものとして扱いません。
> `if` や `else` のキーワードに続くステートメントは、条件に基づいて実行されます。 このチュートリアルのすべてのサンプルでは、一般的な記述方法に従い、ステートメントの制御フローに基づいて行にインデントを挿入しています。

インデントは重要ではないため、条件に基づいて実行するブロック内に 1 つ以上のステートメントがある場合には、`{` と `}` を使用して示します。 通常、C# プログラマーは `if` と `else` の句にはこの中かっこを使用します。 次の例は、さきほど作成したものと同じ内容です。 上のコードを、次のコードに一致するように変更します。

```csharp
int a = 5;
int b = 3;
if (a + b > 10)
{
    Console.WriteLine("The answer is greater than 10");
}
else
{
    Console.WriteLine("The answer is not greater than 10");
}
```

> [!TIP]
> このチュートリアルの残りの箇所では、一般に認められている記述方法に従って、すべてのコード サンプルで中かっこを使用しています。

さらに複雑な条件を判定できます。 `Main` メソッド内でこれまでに記述したコードの下に、次のコードを追加します。

```csharp
int c = 4;
if ((a + b + c > 10) && (a == b))
{
    Console.WriteLine("The answer is greater than 10");
    Console.WriteLine("And the first number is equal to the second");
}
else
{
    Console.WriteLine("The answer is not greater than 10");
    Console.WriteLine("Or the first number is not equal to the second");
}
```

`==` シンボルは、"*同等性*" をテストします。 `==` を使用すると、同等性のテストが `a = 5` で確認した割り当てと区別されます。

`&&` は "and" (および) を表します。 これは、true 分岐でステートメントを実行するには、両方の条件が true になる必要があることを意味しています。  また、これらの例では、ステートメントが `{` と `}` で囲まれていれば、各条件分岐に複数のステートメントを持つことができることを示しています。

`||` を使用して "or" (または) を表すこともできます。 これまでに記述したコードの下に、次のコードを追加します。

```csharp
if ((a + b + c > 10) || (a == b))
{
    Console.WriteLine("The answer is greater than 10");
    Console.WriteLine("Or the first number is equal to the second");
}
else
{
    Console.WriteLine("The answer is not greater than 10");
    Console.WriteLine("And the first number is not equal to the second");
}
```

`a`、`b`、および `c`の値を変更し、探索する `&&` と `||` を切り替えます。 `&&` 演算子と `||` 演算子がどのように機能するかをより深く理解できます。

最初の手順が完了しました。 次のセクションを開始する前に、現在のコードを別のメソッドに移動してみましょう。 移動しておくと、新しい例で作業を開始するときに楽になります。 `Main` メソッドの名前を `ExploreIf` に変更し、`Main` を呼び出す新しい `ExploreIf` メソッドを記述します。 完成したコードは次のようになります。

```csharp
using System;

namespace BranchesAndLoops
{
    class Program
    {
        static void ExploreIf()
        {
            int a = 5;
            int b = 3;
            if (a + b > 10)
            {
                Console.WriteLine("The answer is greater than 10");
            }
            else
            {
                Console.WriteLine("The answer is not greater than 10");
            }

            int c = 4;
            if ((a + b + c > 10) && (a > b))
            {
                Console.WriteLine("The answer is greater than 10");
                Console.WriteLine("And the first number is greater than the second");
            }
            else
            {
                Console.WriteLine("The answer is not greater than 10");
                Console.WriteLine("Or the first number is not greater than the second");
            }

            if ((a + b + c > 10) || (a > b))
            {
                Console.WriteLine("The answer is greater than 10");
                Console.WriteLine("Or the first number is greater than the second");
            }
            else
            {
                Console.WriteLine("The answer is not greater than 10");
                Console.WriteLine("And the first number is not greater than the second");
            }
        }

        static void Main(string[] args)
        {
            ExploreIf();
        }
    }
}
```

`ExploreIf()` の呼び出しをコメント アウトします。 こうしておくと、このセクションの作業を進めるにあたって出力がすっきりします。

```csharp
//ExploreIf();
```

C# では、`//` の後ろが**コメント**になります。 コードとしては実行せずにソース コード内に残したい任意のテキストを、コメントにすることができます。 コンパイラは、コメントから実行可能コードを生成しません。

## <a name="use-loops-to-repeat-operations"></a>ループを使用した処理の繰り返し

このセクションでは**ループ**を使用してステートメントを繰り返し実行します。 `Main` メソッド内で次のコードを試してください。

```csharp
int counter = 0;
while (counter < 10)
{
    Console.WriteLine($"Hello World! The counter is {counter}");
    counter++;
}
```

`while` ステートメントは、条件を確認して `while` に続くステートメントまたはステートメント ブロックを実行します。 条件が false になるまで、条件の確認とステートメントの実行を繰り返します。

この例では、もう 1 つ新しい演算子が使用されています。 `++` 変数のあとにある `counter` は、**インクリメント**演算子です。 `counter` の値に 1 を足し、その値を `counter` 変数に格納します。

> [!IMPORTANT]
> コードを実行したときに `while` のループ条件が false に切り替わることを確認してください。 それ以外の場合は、プログラムが終了することのない**無限ループ**を作成します。 **CTRL-C** またはその他の方法でプログラムを強制的に終了する必要があるため、このサンプルでは実践しません。

`while` ループは、条件を判定してから `while` に続くコードを実行します。 `do` ... `while` ループは、最初にコードを実行してからその条件を確認します。 do while ループを次のコードで示します。

```csharp
int counter = 0;
do
{
    Console.WriteLine($"Hello World! The counter is {counter}");
    counter++;
} while (counter < 10);
```

この `do` ループと先述の `while` ループの出力は同じ結果になります。

## <a name="work-with-the-for-loop"></a>for ループの処理

C# では **for** ループがよく使用されます。 Main() メソッドで次のコードを試してみてください。

```csharp
for (int index = 0; index < 10; index++)
{
    Console.WriteLine($"Hello World! The index is {index}");
}
```

このループは、既に使用した `while` ループや `do` ループと同じ機能を持っています。 `for` ステートメントは 3 つの部分に分かれてその機能を制御します。

最初の部分は、**for 初期化子**です。`int index = 0;` は、`index` がループ変数であることを宣言し、その初期値を `0` に設定しています。

2 つ目の部分は、**for 条件**です。`index < 10` は、counter の値が 10 未満である間は `for` ループが実行され続けることを宣言しています。

最後の部分は、**for 反復子**です。`index++` は、`for` ステートメントに続くブロックを実行したあとにループ変数を変更する方法を指定しています。 ここでは、ブロックが実行されるごとに `index` が 1 ずつ増加するよう指定しています。

ご自身でこれを実行してみてください。 以下をそれぞれ試してみます。

- 初期化子を変更して別の値で開始する。
- 条件を変更して別の値で停止する。

完了したら次に進み、学習したことを使用して自分でいくつかのコードを記述してみましょう。

## <a name="combine-branches-and-loops"></a>分岐とループの組み合わせ

C# 言語における `if` ステートメントとループ構造を見てきました。では、1 から 20 の整数のうち 3 で割り切れる数の合計を求める C# コードを記述できるか確認してみましょう。  次にいくつかヒントを示します。

- `%` 演算子は、除算演算の剰余を算出します。
- `if` ステートメントは、数を合計に入れるべきかどうか確認する条件を作ります。
- `for` ループは、1 から 20 までのすべての数を 1 つずつ確認する一連の手順を繰り返すのに役立ちます。

ご自身で試してみてください。 そして自分がとった方法を確認してください。 答えは 63 になるはずです。 [GitHub で完成版のコードを表示する](https://github.com/dotnet/samples/tree/master/csharp/branches-quickstart/Program.cs#L46-L54)と、考えられる答えの 1 つを確認できます。

これで "分岐とループ" に関するチュートリアルは終了です。

続けて独自の開発環境で[配列とコレクション](arrays-and-collections.md)のチュートリアルに進むことができます。

次のトピックでこれらの概念の詳細を学習できます。

- [if と else ステートメント](../../language-reference/keywords/if-else.md)
- [while ステートメント](../../language-reference/keywords/while.md)
- [do ステートメント](../../language-reference/keywords/do.md)
- [for ステートメント](../../language-reference/keywords/for.md)
