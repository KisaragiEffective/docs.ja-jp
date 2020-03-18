---
title: 文字列補間 - C# チュートリアル
description: このチュートリアルでは、C# で文字列補間機能を使用して、大きい文字列で書式設定された計算式の結果を含める方法を示します。
ms.date: 10/23/2018
ms.openlocfilehash: 593f3a77370da73dfd5f090be98112327b86b1f7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75346779"
---
# <a name="use-string-interpolation-to-construct-formatted-strings"></a>文字列補間を使用し、書式設定された文字列を作成する

このチュートリアルでは、C# で[文字列補間](../../language-reference/tokens/interpolated.md)を使用して、単一の結果の文字列に値を挿入する方法を説明します。 C# コードを記述し、コードをコンパイルおよび実行して結果を確認します。 チュートリアルには、値を文字列に挿入し、それらの値の書式をさまざまな方法で設定する方法を示す、一連のレッスンが含まれています。

このチュートリアルでは、開発用に使用できるマシンがあることを想定しています。 Windows、Linux、または macOS 上でローカルの開発環境を設定する手順については、.NET チュートリアル [Hello World in 10 minutes](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/intro) (10 分で Hello World) に記載されています。 また、使用しているブラウザーでこのチュートリアルの[対話型バージョン](interpolated-strings.yml)を完了することもできます。

## <a name="create-an-interpolated-string"></a>挿入文字列を作成する

*interpolated* という名前のディレクトリを作成します。 それを現在のディレクトリにして、コンソール ウィンドウから次のコマンドを実行します。

```dotnetcli
dotnet new console
```

このコマンドによって、現在のディレクトリに新しい .NET Core コンソール アプリケーションが作成されます。

お好みのエディターで *Program.cs* を開き、`Console.WriteLine("Hello World!");` の行を次のコードで置き換えます。`<name>` は自分の名前に置き換えてください。

```csharp
var name = "<name>";
Console.WriteLine($"Hello, {name}. It's a pleasure to meet you!");
```

コンソール ウィンドウで「`dotnet run`」と入力し、このコードを試します。 プログラムを実行すると、あいさつ文に自分の名前を含む単一の文字列が表示されます。 <xref:System.Console.WriteLine%2A> メソッド呼び出しに含まれる文字列は、*挿入文字列式*です。 これは、埋め込みコードを含む文字列から (*結果文字列*という) 単一の文字列を構築できる一種のテンプレートです。 挿入文字列は、文字列に値を挿入したり、文字列を (結合) 連結したりする場合に特に便利です。

この簡単な例には、すべての挿入文字列に含める必要がある次の 2 つの要素が含まれています。

- 始まりの引用符文字の前の `$` で始まる文字列リテラル。 `$` シンボルと引用符文字の間にスペースを挿入することはできません (スペースが含まれている場合の動作を確認したい場合は、`$` 文字の後にスペースを挿入し、ファイルを保存してから、コンソール ウィンドウで「`dotnet run`」と入力してプログラムを再実行します。 C# コンパイラには、"エラー CS1056:予期しない文字 '$' です" というエラー メッセージが表示されます。

- 1 つ以上の*補間式*。 補間式は、始めかっこと終わりかっこ (`{` と `}`) で示されます。 かっこ内に (`null` を含む) 値を返す C# 式を配置できます。

その他のいくつかのデータ型を持つ文字列補間の例をさらにいくつか試してみましょう。

## <a name="include-different-data-types"></a>さまざまなデータ型を含める

前のセクションでは、文字列補間を使用して、1 つの文字列内に別の文字列を挿入しましたが、 補間式の結果を任意のデータ型にすることもできます。 挿入文字列にさまざまなデータ型の値を含めてみましょう。

次の例では、最初に、`Name` [プロパティ](../../properties.md)と `ToString` [メソッド](../../methods.md)を持つ[クラス](../../programming-guide/classes-and-structs/classes.md) データ型 `Vegetable` を定義します。このメソッドは、<xref:System.Object.ToString?displayProperty=nameWithType> メソッドのビヘイビアーを[オーバーライド](../../language-reference/keywords/override.md)します。 [`public` アクセス修飾子](../../language-reference/keywords/public.md)により、そのメソッドは、すべてのクライアント コードで `Vegetable` インスタンスの文字列表現を取得するために使用できるようになります。 この例の `Vegetable.ToString` メソッドでは、`Vegetable` [コンストラクター](../../programming-guide/classes-and-structs/constructors.md)で初期化される `Name` プロパティの値を返します。

```csharp
public Vegetable(string name) => Name = name;
```

次に、[`new` 演算子](../../language-reference/operators/new-operator.md)を使用し、コンストラクター `Vegetable` に名前を指定することで `item` という名前の `Vegetable` クラスのインスタンスを作成します。

```csharp
var item = new Vegetable("eggplant");
```

最後に、`item` 変数を挿入文字列に含めます。ここには、<xref:System.DateTime> 値、<xref:System.Decimal> 値、`Unit` [列挙](../../language-reference/builtin-types/enum.md)値も含まれます。 エディターのすべての C# コードを以下のコードに置き換えてから、`dotnet run` コマンドを使用して実行します。

```csharp
using System;

public class Vegetable
{
   public Vegetable(string name) => Name = name;

   public string Name { get; }

   public override string ToString() => Name;
}

public class Program
{
   public enum Unit { item, kilogram, gram, dozen };

   public static void Main()
   {
      var item = new Vegetable("eggplant");
      var date = DateTime.Now;
      var price = 1.99m;
      var unit = Unit.item;
      Console.WriteLine($"On {date}, the price of {item} was {price} per {unit}.");
   }
}
```

補間文字列の挿入式 `item` は、結果の文字列のテキスト "eggplant" に解決されることに注意してください。 これは、式の結果の型が文字列でない場合に、結果が次の方法で文字列に解決されるためです。

- 補間式が `null` の場合、空の文字列 (""、または <xref:System.String.Empty?displayProperty=nameWithType>) が使用されます。

- 補間式が `null` でない場合、通常、結果の型の `ToString` メソッドが呼び出されます。 `Vegetable.ToString` メソッドの実装を更新して、これをテストすることができます。 すべての型にこのメソッドの実装が含まれるため、`ToString` メソッドを実装する必要なない場合があります。 これをテストするには、例の `Vegetable.ToString` メソッドの定義をコメント アウトします (この操作を行うには、コメント シンボル `//` を前に配置します)。 出力では、"eggplant" という文字列が完全修飾型名 (この例では "Vegetable") に置き換えられます。これは、<xref:System.Object.ToString?displayProperty=nameWithType> メソッドの既定の動作です。 列挙値の `ToString` メソッドの既定の動作は、値の文字列表現を返すことです。

この例の出力では、日付の精度が高すぎ ("eggplant" の価格は毎秒変更されることはありません)、価格の値は通貨の単位を示していません。 次のセクションでは、式の結果における文字列表現の書式を制御することで、こうした問題を修正する方法について説明します。

## <a name="control-the-formatting-of-interpolation-expressions"></a>補間式の書式設定を制御する

前のセクションでは、適切に書式設定されていない 2 つの文字列が結果文字列に挿入されました。 1 つは、日付のみが適切な日時の値でした。 もう 1 つは、通貨単位を示さない価格でした。 両方の問題には簡単に対処することができます。 文字列補間では、特定の型の書式設定を制御する*書式指定文字列*を指定することができます。 前の例の `Console.WriteLine` 呼び出しを変更し、次の行に示すように、日付と価格の式の書式指定文字列を含めます。

```csharp
Console.WriteLine($"On {date:d}, the price of {item} was {price:C2} per {unit}.");
```

コロン (":") と書式指定文字列を持つ補間式に従って、書式指定文字列を指定します。 "d" は、短い日付形式を表す[標準の日時書式設定文字列](../../../standard/base-types/standard-date-and-time-format-strings.md#the-short-date-d-format-specifier)です。 "C2" は、小数点以下が 2 桁の通貨値として数値を表す[標準の数値書式指定文字列](../../../standard/base-types/standard-numeric-format-strings.md#the-currency-c-format-specifier)です。

.NET ライブラリの多くの型で、定義済みの書式指定文字列セットがサポートされています。 これらには、数値型と日時型がすべて含まれます。 書式指定文字列をサポートする型の完全なリストについては、「[.Net 型の書式設定](../../../standard/base-types/formatting-types.md)」記事の「[.NET クラス ライブラリの型および書式指定文字列](../../../standard/base-types/formatting-types.md#format-strings-and-net-types)」を参照してください。

テキスト エディターで書式指定文字列を変更してみて、変更するたびにプログラムを再実行し、変更が日時と数値の書式設定にどのように影響するかを確認します。 `{date:d}` の "d" を "t" (短い時刻形式を表示する)、"y" (年と月を表示する)、"yyyy" (4 桁の数字として年を表示する) に変更します。 `{price:C2}` "C2" を "e" (指数表記の場合) と "F3" (小数点以下が 3 桁の数値の場合) に変更します。

書式設定を制御するだけでなく、結果の文字列に含まれる書式指定された文字列のフィールドの幅と配置を制御することもできます。 次のセクションでは、この方法を説明します。

## <a name="control-the-field-width-and-alignment-of-interpolation-expressions"></a>補間式のフィールドの幅と配置を制御する

通常、補間式の結果が文字列に書式設定される場合、文字列は先頭スペースおよび末尾スペースなしで結果文字列に含まれます。 通常、データのセットを操作する場合、フィールドの幅とテキストの配置を制御できることで、読みやすい出力を生成できます。 そのためには、テキスト エディターのすべてのコードを次のコードに置き換えてから、「`dotnet run`」と入力してプログラムを実行します。

```csharp
using System;
using System.Collections.Generic;

public class Example
{
   public static void Main()
   {
      var titles = new Dictionary<string, string>()
      {
          ["Doyle, Arthur Conan"] = "Hound of the Baskervilles, The",
          ["London, Jack"] = "Call of the Wild, The",
          ["Shakespeare, William"] = "Tempest, The"
      };

      Console.WriteLine("Author and Title List");
      Console.WriteLine();
      Console.WriteLine($"|{"Author",-25}|{"Title",30}|");
      foreach (var title in titles)
         Console.WriteLine($"|{title.Key,-25}|{title.Value,30}|");
   }
}
```

作成者の名前は左揃えになり、書き込まれたタイトルは右揃えになります。 補間式の後にコンマ (",") を追加し、*最小*のフィールド幅を指定して、配置を指定します。 指定された値が正数の場合、フィールドは右揃えになります。 負数の場合、フィールドは左揃えになります。

`{"Author",-25}` と `{title.Key,-25}` のコードから負号を削除してみて、次のコードのように、例を再実行します。

```csharp
Console.WriteLine($"|{"Author",25}|{"Title",30}|");
foreach (var title in titles)
   Console.WriteLine($"|{title.Key,25}|{title.Value,30}|");
```

この場合、作成者情報は右揃えになります。

単一の補間式にアラインメント指定子と書式指定文字列を組み合わせることができます。 この操作を行うには、最初に配置を指定して、その後にコロンと書式指定文字列を続けます。 `Main` メソッド内のすべてのコードを次のコードに置き換えると、フィールド幅が定義された 3 つの書式指定された文字列が表示されます。 次に、`dotnet run` コマンドを入力してプログラムを実行します。

```csharp
Console.WriteLine($"[{DateTime.Now,-20:d}] Hour [{DateTime.Now,-10:HH}] [{1063.342,15:N2}] feet");
```

出力は次のようになります。

```console
[04/14/2018          ] Hour [16        ] [       1,063.34] feet
```

文字列補間のチュートリアルはこれで終了です。

詳細については、[文字列補間](../../language-reference/tokens/interpolated.md)に関するトピックと「[C# における文字列補間](../../tutorials/string-interpolation.md)」チュートリアルを参照してください。
