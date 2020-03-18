---
title: タプル型 - C# ガイド
description: C# の名前のないタプルと名前付きタプルについて
ms.date: 05/15/2018
ms.technology: csharp-fundamentals
ms.assetid: ee8bf7c3-aa3e-4c9e-a5c6-e05cc6138baa
ms.openlocfilehash: 9ce9e1d4395d1a75f36004384ec215c615cd9802
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79156910"
---
# <a name="c-tuple-types"></a>C# のタプル型

C# のタプルは、軽量構文を使用して定義する型で、 構文がシンプルである、変換の規則が要素の数 ("カーディナリティ" と呼ばれます) と種類に基づく、コピー、等値テスト、および割り当ての規則が一貫している、などのメリットがあります。 そのトレードオフとして、タプルでは、継承に関連するオブジェクト指向の表現形式の一部がサポートされていません。 概要については、[C# 7.0 の新機能のタプル](whats-new/csharp-7.md#tuples)に関する記事のセクションをご覧ください。

この記事では、C# 7.0 以降のバージョンでタプルに適用される言語の規則、タプルの使用方法、およびタプルを操作するための入門的ガイダンスについて説明します。

> [!NOTE]
> 新しいタプル機能を使用するには、<xref:System.ValueTuple> 型が必要です。
> 型が含まれていないプラットフォームで使用する場合は、NuGet パッケージ [`System.ValueTuple`](https://www.nuget.org/packages/System.ValueTuple/) を追加する必要があります。
>
> これは、フレームワークで提供される型に依存するその他の言語機能に似ています。 たとえば、`async` インターフェイスに依存する `await` や `INotifyCompletion`、`IEnumerable<T>` に依存する LINQ などがあります。 ただし、.NET がプラットフォームにさらに依存しなくなりつつあるため、配信メカニズムもそれに応じて変わりつつあります。 .NET Framework が、言語コンパイラと同じ周期で配布されるとは限りません。 新しい言語機能が新しい型に依存する場合、それらの型は、言語機能の配布時に NuGet パッケージとして入手できます。 これらの新しい型は .NET 標準 API に追加され、フレームワークの一部として配信されるため、NuGet パッケージは必要なくなります。

詳しく見ていく前に、新しいタプルのサポートを追加した理由について説明します。 メソッドが返すのは 1 つのオブジェクトです。 タプルを使用すると、その 1 つのオブジェクトに複数の値を簡単にパッケージできます。

.NET Framework には `Tuple` ジェネリック クラスが既にありますが、 このクラスには 2 つの大きな制限がありました。 1 つは、`Tuple` クラスのプロパティに `Item1`、`Item2` という名前が付けられるというものです。 その名前にはセマンティック情報が保持されていません。 つまり、この `Tuple` 型を使用しても、各プロパティの情報を伝達することはできません。 新しい言語機能を使用すると、タプルの要素に意味的にわかりやすい名前を宣言して使用できます。

`Tuple` クラスは参照型であるため、パフォーマンスの問題が発生しやすくなります。 `Tuple` 型を使用するには、オブジェクトを割り当てる必要があります。 ホット パスでは、多数の小さいオブジェクトの割り当てがアプリケーションのパフォーマンスに大きな影響を及ぼすことがあります。 そのため、タプルの言語サポートでは、新しい `ValueTuple` 構造体を活用します。

`class` や `struct` を作成して複数の要素を伝達すれば、この弱点を回避できますが、 その分、手間が増え、設計の意図がわかりにくくなります。 `struct` や `class` を作成すると、データと動作の両方で型を定義しなければなりませんが、 1 つのオブジェクトに複数の値を格納したいだけという状況もよくあります。

これらの機能と `ValueTuple` ジェネリック構造体では、タプル型に動作 (メソッド) を追加できないという規則が適用されます。
すべての `ValueTuple` 型が "*mutable 構造体*" で、 各メンバー フィールドがパブリック フィールドであるため、 非常に軽量です。 ただし、不変性が重要な場合は、タプルを使用しないでください。

タプルは、`class` 型や `struct` 型に比べて、シンプルで柔軟なデータ コンテナーです。 両者の違いを見てみましょう。

## <a name="named-and-unnamed-tuples"></a>名前付きのタプルと名前がないタプル

既存の `ValueTuple` 型で定義されたプロパティと同様、`Item1` 構造体のフィールドには `Item2`、`Item3`、`Tuple` といった名前が付いています。
"*名前のないタプル*" には、この名前しか使用できません。 タプルに代替フィールド名を付けなかった場合は、名前のないタプルが作成されます。

[!code-csharp[UnnamedTuple](../../samples/snippets/csharp/tuples/program.cs#01_UnNamedTuple "Unnamed tuple")]

前の例のタプルは、リテラル定数を使って初期化されており、C# 7.1 の*タプル フィールド名プロジェクション*を使って作成された要素名はありません。

タプルを初期化する際に、新しい機能を使用して、各フィールドにわかりやすい名前を付けることができます。 これによって、"*名前付きタプル*" が作成されます。
名前付きタプルには `Item1`、`Item2`、`Item3` という名前の要素がまだ存在しますが、
名前を付けた要素に対してシノニムも設定されます。
名前付きタプルを作成するには、各要素の名前を指定します。 たとえば、タプル初期化の一環として名前を指定できます。

[!code-csharp[NamedTuple](../../samples/snippets/csharp/tuples/program.cs#02_NamedTuple "Named tuple")]

コンパイラと言語によってシノニムが処理されるため、名前付きタプルを効果的に使用できるようになります。 IDE やエディターは Roslyn API を使用して、セマンティック名を読み取ります。 これにより、同じアセンブリ内の任意の場所で、セマンティック名によって名前付きタプルの要素を参照できます。 定義した名前は、コンパイル済み出力が生成されるときに、対応する `Item*` に置き換えられます。 これらの要素に設定した名前は、コンパイルされた Microsoft Intermediate Language (MSIL) には含まれません。

C# 7.1 以降、タプルのフィールド名は、タプルの初期化に使用した変数によって指定される場合があります。 これは、 **[タプル プロジェクション初期化子](#tuple-projection-initializers)** と呼ばれます。 次のコードでは、要素 `accumulation` (整数)、および `count` (倍精度浮動小数点型) で `sum` という名前のタプルを作成します。

[!code-csharp[ProjectedTuple](../../samples/snippets/csharp/tuples/program.cs#ProjectedTupleNames "Named tuple")]

コンパイラは、パブリック メソッドまたはプロパティから返されたタプルに指定されている名前を伝える必要があります。 このような場合、コンパイラはメソッドに <xref:System.Runtime.CompilerServices.TupleElementNamesAttribute> 属性を追加します。 この属性には、タプルの各要素に付けられた名前が含まれた <xref:System.Runtime.CompilerServices.TupleElementNamesAttribute.TransformNames> リスト プロパティが含まれています。

> [!NOTE]
> Visual Studio などの開発ツールも、そのメタデータを読み取り、メタデータ フィールド名を使用する IntelliSense などの機能を提供します。

名前付きタプルを相互に割り当てるための規則を理解するには、新しいタプルと `ValueTuple` 型の基本を理解しておくことが重要です。

## <a name="tuple-projection-initializers"></a>タプル プロジェクション初期化子

一般に、タプル プロジェクション初期化子は、タプルの初期化ステートメントの右側にある変数またはフィールド名を使用して機能します。
明示的な名前が指定された場合は、射影された名前より優先されます。 たとえば、次の初期化子では、要素は `explicitFieldOne` や `explicitFieldTwo` ではなく、`localVariableOne` と `localVariableTwo` になります。

[!code-csharp[ExplicitNamedTuple](../../samples/snippets/csharp/tuples/program.cs#ProjectionExample_Explicit "Explicitly named tuple")]

明示的な名前が指定されていないフィールドの場合、適用可能な暗黙的な名前が射影されます。 明示的または暗黙的のいずれかで、セマンティック名を指定するための要件はありません。 次の初期化子には、フィールド名 `Item1` があり、その値は `42` と `stringContent` で、その値は "The answer to everything" です。

[!code-csharp[MixedTuple](../../samples/snippets/csharp/tuples/program.cs#MixedTuple "mixed tuple")]

候補フィールド名がタプル フィールドに射影されない場合の条件は 2 つあります。

1. 候補名が予約されているタプル名の場合。 例としては、`Item3`、`ToString` または `Rest` です。
1. 候補名が、別のタプル フィールド名 (明示的または暗黙的のいずれか) の複製である場合。

これらの条件によってあいまいさを回避します。 この名前がタプルのフィールドのフィールド名として使用される場合、あいまいさの原因となります。 この条件はどちらも、コンパイル時エラーを発生させることはありません。 代わりに、射影された名前のない要素には、射影されたセマンティック名がありません。  これらの条件の例を以下に示します。

[!code-csharp-interactive[Ambiguity](../../samples/snippets/csharp/tuples/program.cs#ProjectionAmbiguities "tuples where projections are not performed")]

これらの条件は、タプル フィールド名プロジェクションが利用できなかった場合、C# 7.0 で記述されたコードに対する重大な変更になるため、コンパイラ エラーが発生することはありません。

## <a name="equality-and-tuples"></a>等値とタプル

C# 7.3 以降では、タプル型で `==` および `!=` 演算子がサポートされます。 これらの演算子は、左の引数の各メンバーと右の引数の各メンバーを順番に比較することによって機能します。 これらの比較はショートさせます。 これらは、ペアが等値でなくなるとすぐにメンバーの評価を停止します。 次のコード例では `==` を使用しますが、比較規則がすべて `!=` に適用されます。 次のコード例は、整数の 2 つのペアの等値比較を示しています。

[!code-csharp-interactive[TupleEquality](../../samples/snippets/csharp/tuples/program.cs#Equality "Testing tuples for equality")]

タプルの等値テストをより簡単にするルールがいくつかあります。 次のコードに示すように、いずれかのタプルが null 許容タプルの場合、タプルの等値性によって[リフト変換](~/_csharplang/spec/conversions.md#lifted-conversion-operators)が実行されます。

[!code-csharp-interactive[NullableTupleEquality](../../samples/snippets/csharp/tuples/program.cs#NullableEquality "Comparing Tuples and nullable tuples")]

タプルの等値性では、両方のタプルの各メンバーに対して暗黙の変換も実行されます。 これらには、リフト変換、拡大変換などの暗黙の型変換も含まれます。 次の例は、整数から long 型への暗黙の型変換によって、整数の 2 つのタプルを long 型の 2 つのタプルと比較できることを示しています。

[!code-csharp-interactive[SnippetMemberConversions](../../samples/snippets/csharp/tuples/program.cs#SnippetMemberConversions "converting tuples for equality tests")]

タプルのメンバーの名前は、等値性のテストに参加しません。 ただし、いずれかのオペランドが明示的な名前を持つタプル リテラルの場合、コンパイラは、この名前が他のオペランドの名前と一致しない場合、警告 CS8383 を生成します。
両方のオペランドがタプル リテラルである場合、警告は次の例に示すように右オペランドに含まれます。

[!code-csharp-interactive[MemberNames](../../samples/snippets/csharp/tuples/program.cs#SnippetMemberNames "Tuple member names do not participate in equality tests")]

最後に、タプルに入れ子になったタプルが含まれることがあります。 タプルの等値性によって、次の例に示すように、入れ子になったタプルを通じて各オペランドの "シェイプ" が比較されます。

[!code-csharp-interactive[NestedTuples](../../samples/snippets/csharp/tuples/program.cs#SnippetNestedTuples "Tuples may contain nested tuples that participate in tuple equality.")]

シェイプが異なる 2 つのタプルの等値性 (または非等値性) を比較すると、コンパイル時エラーになります。 コンパイラは、入れ子になったタプルを比較するためにその分解を試行することはありません。

## <a name="assignment-and-tuples"></a>割り当てとタプル

この言語では、要素の数が同じタプル型間での割り当てがサポートされています。ここでは、右側の各要素をそれに対応する左側の要素に暗黙的に変換できます。 他の変換は、割り当てでは考慮されません。 あるタプルを、シェイプが異なる別のタプルに割り当てると、コンパイル時エラーになります。 コンパイラは、入れ子になったタプルを割り当てるためにその分解を試行することはありません。
タプル型間で許可されている割り当ての種類を見てみましょう。

以降の例で使用されている変数について考えます。

[!code-csharp[VariableCreation](../../samples/snippets/csharp/tuples/program.cs#03_VariableCreation "Variable creation")]

最初の 2 つの変数 `unnamed` および `anonymous` では、要素にセマンティック名が割り当てられていません。 フィールド名は `Item1` と `Item2` になります。
最後の 2 つの変数 `named` および `differentName` では、要素にセマンティック名が付けられています。 この 2 つのタプルでは、要素名が異なっています。

この 4 つのタプルに含まれている要素の数 ("カーディナリティ" と呼ばれます) と要素の型は同じです。 このため、これらの割り当てはすべて機能します。

[!code-csharp[VariableAssignment](../../samples/snippets/csharp/tuples/program.cs#04_VariableAssignment "Variable assignment")]

タプルの名前が割り当てられていないことに注意してください。 要素の値は、タプルの要素の順序に従って割り当てられます。

要素の型または数が異なるタプルを割り当てることはできません。

```csharp
// Does not compile.
// CS0029: Cannot assign Tuple(int,int,int) to Tuple(int, string)
var differentShape = (1, 2, 3);
named = differentShape;
```

## <a name="tuples-as-method-return-values"></a>メソッドの戻り値としてのタプル

タプルはメソッドの戻り値として使用できます。これはタプルの一般的な使用方法の 1 つです。 その例を見てみましょう。 数値シーケンスの標準偏差を計算する次のメソッドについて考えます。

[!code-csharp[StandardDeviation](../../samples/snippets/csharp/tuples/statistics.cs#05_StandardDeviation "Compute Standard Deviation")]

> [!NOTE]
> この例では、未修正のサンプル標準偏差を計算します。
> 修正後のサンプル標準偏差式は、`Average` 拡張メソッドで行われるのと同様に、平均値との差の二乗和を、N ではなく (N-1) で除算します。 標準偏差のこうした数式の間に生じる差の詳細については、統計値のテキストを参照してください。

上のコードは、標準偏差の教科書どおりの数式に従っています。 正しい答えが生成されますが、非効率的な実装です。 このメソッドは、シーケンスを 2 回列挙します。1 回は平均値を生成するため、もう 1 回は平均値との差を 2 乗して、その平均値を生成するためです
(前述のとおり、LINQ クエリは遅延評価されるため、平均値との差と、その差の平均値の計算で生成される列挙は 1 つだけです)。

シーケンスの列挙を 1 つだけ使用して標準偏差を計算する、別の数式があります。  この計算では、シーケンスを列挙しながら、2 つの値が生成されます。1 つはシーケンス内のすべての項目の合計、もう 1 つは各値の二乗和です。

[!code-csharp[SumOfSquaresFormula](../../samples/snippets/csharp/tuples/statistics.cs#06_SumOfSquaresFormula "Compute Standard Deviation using the sum of squares")]

このバージョンでは、シーケンスを 1 回だけ列挙しますが、 再利用可能なコードとは言えません。 操作を続けていくと、さまざまな統計計算処理の多くが、シーケンス内の項目数、シーケンスの合計、およびシーケンスの二乗和を使用していることがわかります。 このメソッドをリファクタリングし、その 3 つの値すべてを生成するユーティリティ メソッドを作成しましょう。 3 つすべての値をタプルとして戻すことができます。

このメソッドを更新して、列挙中に計算された 3 つの値をタプルに格納しましょう。 そうすると、次のバージョンが作成されます。

[!code-csharp[TupleVersion](../../samples/snippets/csharp/tuples/statistics.cs#07_TupleVersion "Refactor to use tuples")]

Visual Studio のリファクタリング サポートにより、主要な統計情報の機能をプライベート メソッドに抽出できます。 これにより、3 つの値 `private static`、`Sum`、`SumOfSquares` を含むタプル型を返す `Count` メソッドが作成されます。

[!code-csharp[TupleMethodVersion](../../samples/snippets/csharp/tuples/statistics.cs#08_TupleMethodVersion "After extracting utility method")]

編集を手動ですばやく行う必要がある場合は、使用できるオプションが他にもいくつかあります。 まず、`var` 宣言を使用することで、`ComputeSumAndSumOfSquares` メソッド呼び出しのタプルの結果を初期化できます。 `ComputeSumAndSumOfSquares` メソッド内に異なる 3 つの変数を作成することもできます。 最終的なバージョンを次のコードに示します。

[!code-csharp[CleanedTupleVersion](../../samples/snippets/csharp/tuples/statistics.cs#09_CleanedTupleVersion "After final cleanup")]

この最終バージョンは、この 3 つの値を必要とするすべてのメソッド、またはそのサブセットで使用できます。

このようなタプルを返すメソッドで要素の名前を管理するオプションが他にもサポートされています。

戻り値の宣言からフィールド名を削除して、名前のないタプルを返すことができます。

```csharp
private static (double, double, int) ComputeSumAndSumOfSquares(IEnumerable<double> sequence)
{
    double sum = 0;
    double sumOfSquares = 0;
    int count = 0;

    foreach (var item in sequence)
    {
        count++;
        sum += item;
        sumOfSquares += item * item;
    }

    return (sum, sumOfSquares, count);
}
```

このタプルのフィールドは、`Item1`、`Item2`、および `Item3` という名前が付けられます。
メソッドから返されたタプルの要素には、セマンティック名を指定することをお勧めします。

LINQ クエリを作成するときも、タプルが便利です。 最終的な予測される結果では、選択されているオブジェクトのプロパティのすべてではなく、一部が含まれる場合が一般的です。

従来、クエリの結果は、匿名型のオブジェクトのシーケンスに射影していましたが、 この方法には多くの制限が伴いました。メソッドの戻り値の型では、匿名型に名前を付けるのが簡単ではなかったからです。 代替手段として `object` や `dynamic` を結果の型に使用すると、パフォーマンス コストは大きくなります。

タプル型のシーケンスを返すのは簡単です。要素の名前と型は、コンパイル時に IDE ツールで使用することができます。
たとえば、次の ToDo アプリケーションを考えてみます。 ToDo リストの 1 つのエントリを表すために、次のようなクラスを定義します。

[!code-csharp[ToDoItem](../../samples/snippets/csharp/tuples/projectionsample.cs#14_ToDoItem "To Do Item")]

モバイル アプリケーションでサポートされるのは、タイトルしか表示されないコンパクト形式の現在の ToDo 項目です。 その LINQ クエリでは、ID とタイトルのみが含まれるプロジェクションが作成されます。 タプルのシーケンスを返すメソッドは、その設計を適切に表現しています。

[!code-csharp[QueryReturningTuple](../../samples/snippets/csharp/tuples/projectionsample.cs#15_QueryReturningTuple "Query returning a tuple")]

> [!NOTE]
> C# 7.1 では、タプル プロジェクションを使用して、匿名型で名前が付けられたプロパティと同様の方法で、要素を使用する名前付きタプルを作成できます。 上記のコードでは、クエリ プロジェクションの `select` ステートメントで、要素 `ID` と `Title` を含むタプルを作成します。

名前付きタプルは、署名に含めることができます。 これによって、コンパイラと IDE ツールは静的チェックを行い、結果が正しく使用されていることを確認できます。 名前付きタプルには静的情報も含まれているため、リフレクション、動的バインドなど、コストのかかるランタイム機能を使用して結果を操作する必要がありません。

## <a name="deconstruction"></a>分解

タプル内のすべての項目を展開するには、メソッドによって返されるタプルを*分解*します。 タプルは 3 とおりの方法で分解できます。  まず、かっこの中で各フィールドの型を明示的に宣言して、タプルの要素ごとに個別の変数を作成することができます。

[!code-csharp[Deconstruct](../../samples/snippets/csharp/tuples/statistics.cs#10_Deconstruct "Deconstruct")]

また、かっこの外に `var` キーワードを使用して、タプルの各フィールドに対して暗黙的に型指定された変数を宣言することもできます。

[!code-csharp[DeconstructToVar](../../samples/snippets/csharp/tuples/statistics.cs#11_DeconstructToVar "Deconstruct to Var")]

`var` キーワードは、かっこ内のいずれか 1 つの変数宣言に使用することも、すべての変数宣言に使用することもできます。

```csharp
(double sum, var sumOfSquares, var count) = ComputeSumAndSumOfSquares(sequence);
```

タプル内のフィールドすべての型が同じでも、かっこ外では使用できない型があります。

既存の宣言を使用してタプルを分解することもできます。

```csharp
public class Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) => (X, Y) = (x, y);
}
```

> [!WARNING]
> 既存の宣言をかっこ内の宣言と混在させることはできません。 たとえば、`(var x, y) = MyMethod();` は許可されません。 *x* はかっこ内で宣言されており、*y* は他の場所で以前に宣言されているため、これによりエラー CS8184 が生成されます。

### <a name="deconstructing-user-defined-types"></a>ユーザー定義型の分解

上に示したように、すべてのタプル型を分解できます。 また、ユーザー定義型 (クラス、構造体、またはインターフェイス) も簡単に分解できます。

型の作成者は、型を構成するデータ要素を表す任意の数の `Deconstruct` 変数に対して値を割り当てる `out` メソッドを 1 つ以上定義できます。 たとえば、次の `Person` 型は、person オブジェクトを、名と姓を表す要素に分解する `Deconstruct` メソッドを定義しています。

[!code-csharp[TypeWithDeconstructMethod](../../samples/snippets/csharp/tuples/person.cs#12_TypeWithDeconstructMethod "Type with a deconstruct method")]

deconstruct メソッドを使用すると、`Person` から、`FirstName` プロパティと `LastName` プロパティを表す 2 つの文字列を割り当てることができます。

[!code-csharp[Deconstruct Type](../../samples/snippets/csharp/tuples/program.cs#12A_DeconstructType "Deconstruct a class type")]

自分で作成していない型を分解することもできます。
`Deconstruct` メソッドは、オブジェクトのアクセス可能なデータ メンバーを展開する拡張メソッドとして使用できます。 次の例は、`Student` から派生した `Person` 型と、`Student` を 3 つの変数 `FirstName`、`LastName`、`GPA` に分解する拡張メソッドを示しています。

[!code-csharp[ExtensionDeconstructMethod](../../samples/snippets/csharp/tuples/person.cs#13_ExtensionDeconstructMethod "Type with a deconstruct extension method")]

`Student` オブジェクトには、アクセス可能な `Deconstruct` メソッドが 2 つあります。`Student` 型に対して宣言された拡張メソッドと、`Person` 型のメンバーです。 両方ともスコープ内にあり、`Student` を 2 つまたは 3 つの変数に分解できます。
student を 3 つの変数に割り当てると、名、姓、GPA のすべてが返されます。 student を 2 つの変数に割り当てると、名と姓のみが返されます。

[!code-csharp[Deconstruct extension method](../../samples/snippets/csharp/tuples/program.cs#13A_DeconstructExtension "Deconstruct a class type using an extension method")]

クラスまたはクラス階層で複数の `Deconstruct` メソッドを定義するときには注意が必要です。 `Deconstruct` パラメーターの数が同じ `out` メソッドが複数あると、あいまいさが生じ、 呼び出し元が、必要な `Deconstruct` メソッドを簡単には呼び出せなくなる場合があります。

この例では、出力パラメーターが `Deconstruct` の `Person` メソッドには 2 つ、`Deconstruct` の `Student` メソッドには 3 つ含まれるため、呼び出しが不明確になる可能性は最小限に抑えられています。

分解演算子は、等値性のテストには参加しません。 次の例ではコンパイラ エラー CS0019 が生成されます。

```csharp
Person p = new Person("Althea", "Goodwin");
if (("Althea", "Goodwin") == p)
    Console.WriteLine(p);
```

`Deconstruct` メソッドは `Person` オブジェクト `p` を 2 つの文字列を含むタプルに変換できますが、これを等値テストのコンテキストで適用することはできません。

## <a name="tuples-as-out-parameters"></a>out パラメーターとしてのタプル

タプルは、"*それ自体*" を out パラメーターとして使用できます。 前述の「[分解](#deconstruction)」セクションで説明したあいまいさと混同しないようにしてください。 メソッド呼び出しでは、タプルのシェイプについてのみ記述する必要があります。

[!code-csharp[TuplesAsOutParameters](~/samples/snippets/csharp/tuples/program.cs#01_TupleAsOutVariable "Tuples as out parameters")]

または、[_unnamed_](#named-and-unnamed-tuples) タプルを使用し、そのフィールドを `Item1` および `Item2` として参照することもできます。

```csharp
dict.TryGetValue(2, out (int, string) pair);
// ...
Console.WriteLine($"{pair.Item1}: {pair.Item2}");
```

## <a name="conclusion"></a>まとめ

クラスや構造体では動作の定義が必要であるため、新しい言語とライブラリで名前付きタプルがサポートされたことで、動作を定義せずに複数の要素を格納するデータ構造設計が格段に扱いやすくなりました。 こうした型に対してタプルを使用するのは簡単です。 詳細な `class` または `struct` 構文を使用して型を作成しなくても、静的な型チェックのすべてのメリットを利用できます。 とは言っても、class や struct は、`private` や `internal` のユーティリティ メソッドにとっては非常に便利です。 複数の要素を含む値がパブリック メソッドによって返される場合は、ユーザー定義型、`class` または`struct` を作成します。
