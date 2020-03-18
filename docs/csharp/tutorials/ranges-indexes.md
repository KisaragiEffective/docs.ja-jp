---
title: インデックスと範囲を使用してデータの範囲を調べる
description: この高度なチュートリアルでは、インデックスと範囲を使用してデータを調べ、シーケンシャル データ セットのスライスを調べる方法について説明します。
ms.date: 09/20/2019
ms.technology: csharp-fundamentals
ms.custom: mvc
ms.openlocfilehash: 3d4c022ff8d6e7f260632e34d6f28277014c85c8
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75345629"
---
# <a name="indices-and-ranges"></a>インデックスと範囲

範囲とインデックスには、シーケンス内の 1 つの要素または範囲にアクセスできる簡潔な構文が用意されています。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> - シーケンス内の範囲に構文を使用します。
> - 各シーケンスの開始と終了に関する設計上の決定について説明します。
> - <xref:System.Index> 型と <xref:System.Range> 型のシナリオについて説明します。

## <a name="language-support-for-indices-and-ranges"></a>インデックスと範囲の言語サポート

この言語のサポートでは、次の 2 つの新しい型と 2 つの新しい演算子が使用されています。

- <xref:System.Index?displayProperty=nameWithType> はシーケンスとしてインデックスを表します。
- index from end 演算子の `^`。シーケンスの末尾から相対的なインデックスを指定します。
- <xref:System.Range?displayProperty=nameWithType> はシーケンスのサブ範囲を表します。
- 範囲演算子の `..`。範囲の先頭と末尾をそのオペランドとして指定します。

インデックスのルールから始めましょう。 配列 `sequence` を考えます。 `0` インデックスは `sequence[0]` と同じです。 `^0` インデックスは `sequence[sequence.Length]` と同じです。 `sequence[sequence.Length]` と同様に、`sequence[^0]` は例外をスローすることに注意してください。 任意の数値 `n` の場合、インデックス `^n` は `sequence[sequence.Length - n]` と同じです。

```csharp
string[] words = new string[]
{
                // index from start    index from end
    "The",      // 0                   ^9
    "quick",    // 1                   ^8
    "brown",    // 2                   ^7
    "fox",      // 3                   ^6
    "jumped",   // 4                   ^5
    "over",     // 5                   ^4
    "the",      // 6                   ^3
    "lazy",     // 7                   ^2
    "dog"       // 8                   ^1
};              // 9 (or words.Length) ^0
```

末尾の単語は `^1` インデックスで取得できます。 初期化の下に次のコードを追加します。

[!code-csharp[LastIndex](~/samples/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_LastIndex)]

範囲は、範囲の*先頭*と*末尾*を指定します。 範囲は排他的です。つまり、*末尾*は範囲に含まれません。 範囲 `[0..^0]` は、`[0..sequence.Length]` が範囲全体を表すのと同じように、範囲全体を表します。 

次のコードでは、単語 "quick"、"brown"、"fox" から成る部分範囲が作成されます。 それには、`words[1]` から `words[3]` までが含まれます。 要素 `words[4]` が範囲内にありません。 同じメソッドに次のコードを追加します。 それをコピーして、対話型ウィンドウの下部に貼り付けます。

[!code-csharp[Range](~/samples/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_Range)]

次のコードでは、"lazy" と "dog" の部分範囲が作成されます。 それには、`words[^2]` と `words[^1]` が含まれます。 末尾インデックス `words[^0]` は含まれません。 次のコードも追加します。

[!code-csharp[LastRange](~/samples/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_LastRange)]

次の例では、先頭と末尾の一方または両方が開いている範囲が作成されます。

[!code-csharp[PartialRange](~/samples/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_PartialRanges)]

範囲やインデックスを変数として宣言することもできます。 この変数は、文字 `[` と `]` の内側で使用できます。

[!code-csharp[IndexRangeTypes](~/samples/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_RangeIndexTypes)]

次のサンプルは、こうした選択肢の理由の多くを示しています。 `x`、`y`、`z` を変更してさまざまな組み合わせを試してください。 実験するときには、有効な組み合わせになるように `x` が `y` 未満の値、`y` が `z` 未満の値を使用します。 新しいメソッドに次のコードを追加します。 さまざまな組み合わせを試してください。

[!code-csharp[SemanticsExamples](~/samples/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_Semantics)]

## <a name="type-support-for-indices-and-ranges"></a>インデックスと範囲の型のサポート

インデックスと範囲は明確かつ簡潔な構文を提供することで、シーケンス内で 1 つの要素または要素のサブ範囲にアクセスします。 インデックス式は、一般的に、シーケンスの要素の型を返します。 範囲式は、一般的に、ソース シーケンスと同じシーケンス型を返します。

型に <xref:System.Index> または <xref:System.Range> パラメーターを持つ[インデクサー](../programming-guide/indexers/index.md)が用意されている場合、それぞれ明示的にインデックスまたは範囲をサポートします。 型に用意されているのが 1 つの <xref:System.Range> パラメーターをとるインデクサーの場合、その型は、<xref:System.Span%601?displayProperty=nameWithType> などの別のシーケンス型を返すように選択できます。

アクセス可能なゲッターと戻り値の型 `int` を持つ `Length` または `Count` という名前のプロパティがある場合、型は**可算**です。 インデックスまたは範囲を明示的にサポートしていない可算型は、それらを暗黙的にサポートしている可能性があります。 詳細については、[機能の提案に関する注記](~/_csharplang/proposals/csharp-8.0/ranges.md)の「[暗黙的なインデックスのサポート](~/_csharplang/proposals/csharp-8.0/ranges.md#implicit-index-support)」と「[暗黙的な範囲のサポート](~/_csharplang/proposals/csharp-8.0/ranges.md#implicit-range-support)」のセクションを参照してください。 暗黙的な範囲のサポートを使用している範囲によって返されるのは、ソース シーケンスと同じシーケンス型です。

たとえば、次の .NET 型は、<xref:System.Array>、<xref:System.String>、<xref:System.Span%601>、および <xref:System.ReadOnlySpan%601> のインデックスと範囲の両方をサポートしています。 <xref:System.Collections.Generic.List%601> はインデックスをサポートしていますが、範囲はサポートしていません。

## <a name="scenarios-for-indices-and-ranges"></a>インデックスと範囲のシナリオ

シーケンス全体の一部の範囲について何らかの分析を行う場合は、範囲とインデックスを使用することがよくあります。 どの部分の範囲が関係しているかを正確に読み取る場合に、この新しい構文の方が明確です。 ローカル関数 `MovingAverage` は、引数として <xref:System.Range> を受け取ります。 このメソッドでは、最小値、最大値、および平均値を計算するときに、その範囲のみが列挙されます。 プロジェクトで次のコードを試してみてください。

[!code-csharp[MovingAverages](~/samples/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_MovingAverage)]

完成したコードは [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/RangesIndexes) リポジトリからダウンロードできます。
