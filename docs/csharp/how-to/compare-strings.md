---
title: 文字列を比較する方法 - C# ガイド
description: 大文字小文字の区別およびカルチャ固有の順序を使用してまたは使用せずに文字列値を比較および並べ替える方法について説明します。
ms.date: 10/03/2018
helpviewer_keywords:
- strings [C#], comparison
- comparing strings [C#]
ms.openlocfilehash: dda3ec8cb6a0131867e6ea3bb0cf7199d86058ff
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73973319"
---
# <a name="how-to-compare-strings-in-c"></a>C\# で文字列を比較する方法

文字列を比較して、次の 2 つの質問のいずれかに回答します。"これら 2 つの文字列は等しいですか" または "これらを並べ替えるときにどのような順序でこれらの文字列を配置しますか"

これら 2 つの質問は、文字列比較に影響する要因によって複雑になります。

- 序数に基づく比較または言語的な比較を選択することができます。
- 大文字小文字が重要かどうかを選択できます。
- カルチャに固有の比較を選択できます。
- 言語的な比較は、カルチャおよびプラットフォームに依存します。

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

文字列を比較するときに、それらの間で順序を定義します。 比較は、文字列のシーケンスの並べ替えに使用されます。 シーケンスが既知の順序の場合、ソフトウェアと人間の両方が簡単に検索できます。 その他の比較では、文字列が等しいかどうかを確認する場合があります。 これらの類似性チェックは等値に似ていますが、大文字小文字の違いなど、いくつかの違いは無視される場合があります。

## <a name="default-ordinal-comparisons"></a>既定の序数の比較

既定では、最も一般的な操作は次のとおりです。

- <xref:System.String.CompareTo%2A?displayProperty=nameWithType>
- <xref:System.String.Equals%2A?displayProperty=nameWithType>
- <xref:System.String.op_Equality%2A?displayProperty=nameWithType> と <xref:System.String.op_Inequality%2A?displayProperty=nameWithType>。つまり、それぞれ[等価演算子 `==` と `!=`](../language-reference/operators/equality-operators.md#string-equality)。

大文字と小文字を区別する序数の比較を実行し、必要に応じて現在のカルチャを使用します。 次に例を示します。

[!code-csharp-interactive[Comparing strings using an ordinal comparison](../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs#1)]

既定の序数の比較では、文字列を比較するときに言語の規則を考慮しません。 2 つの文字列のそれぞれの <xref:System.Char> オブジェクトのバイナリ値を比較します。 その結果、既定の序数の比較でも大文字と小文字が区別されます。

<xref:System.String.Equals%2A?displayProperty=nameWithType> と `==` および `!=` 演算子を使用した等価性のテストは、<xref:System.String.CompareTo%2A?displayProperty=nameWithType> と <xref:System.String.Compare(System.String,System.String)?displayProperty=nameWithType)> のメソッドを使用した文字列の比較とは異なります。 等価性のテストでは大文字と小文字を区別する序数の比較が行われますが、比較メソッドでは大文字と小文字だけでなく、現在のカルチャを使用してカルチャを区別する比較が行われます。 既定の比較メソッドでは多くの場合、さまざまな種類の比較が実行されるため、実行する比較の種類を明示的に指定するオーバーロードを呼び出して、コードの意図を常に明確にすることをお勧めします。

## <a name="case-insensitive-ordinal-comparisons"></a>大文字と小文字を区別しない、序数に基づく比較

<xref:System.String.Equals(System.String,System.StringComparison)?displayProperty=nameWithType> メソッドでは、<xref:System.StringComparison> の <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> 値を指定して、
大文字と小文字を区別しない、序数に基づく比較ができるようにします。 <xref:System.String.Compare(System.String,System.String,System.StringComparison)?displayProperty=nameWithType> 引数に <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> の値を指定すると、大文字と小文字を区別しない序数に基づく比較が実行される、静的な <xref:System.StringComparison> メソッドもあります。 これを次のコードに示します。

[!code-csharp-interactive[Comparing strings ignoring case](../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs#2)]

大文字と小文字を区別しない、序数に基づく比較を実行すると、これらのメソッドでは[インバリアント カルチャ](xref:System.Globalization.CultureInfo.InvariantCulture)の大文字と小文字の区別が使用されます。

## <a name="linguistic-comparisons"></a>言語的な比較

現在のカルチャの言語の規則を使用して文字列の順序を指定することもできます。
これは、"単語の並べ替え順序" と呼ばれることもあります。 言語的な比較を実行するときには、一部の英数字以外の Unicode 文字に、特別な重みが割り当てられる場合があります。 たとえば、ハイフン ("-") に割り当てられる重みは非常に小さいため、並べ替え順序で "coop" と "co-op" の出現位置が隣接します。 さらに、いくつかの Unicode 文字が <xref:System.Char> インスタンスのシーケンスと等しくなる可能性があります。 次の例では、ドイツ語で "They dance in the street" という語句を 一方の文字列では "ss" (U+0073 U+0073)、もう一方の文字列では 'ß' (U+00DF) を使って表しています。 言語的に (Windows では)、"en-US" と "de-DE" の両方のカルチャで、"ss" はドイツ語の Essetz: 'ß' 文字と同じです。

[!code-csharp-interactive[Comparing strings using linguistic rules](../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs#3)]

このサンプルでは、言語的な比較に依存するオペレーティング システムの性質を示します。 対話型ウィンドウのホストは、Linux ホストです。 言語比較および序数比較は、同じ結果を生成します。 Windows ホストで同じこのサンプルを実行する場合、次の出力が表示されます。

```console
<coop> is less than <co-op> using invariant culture
<coop> is greater than <co-op> using ordinal comparison
<coop> is less than <cop> using invariant culture
<coop> is less than <cop> using ordinal comparison
<co-op> is less than <cop> using invariant culture
<co-op> is less than <cop> using ordinal comparison
```

Windows では、言語的な比較から序数に基づく比較に変更した場合に、"cop"、"coop"、および "co-op" の並べ替え順序は変更されます。 ドイツ語の 2 つの文も、異なる比較の種類を使用して異なる方法で比較されます。

## <a name="comparisons-using-specific-cultures"></a>特定のカルチャを使用した比較

このサンプルでは en-US と de-DE のカルチャの <xref:System.Globalization.CultureInfo> オブジェクトを格納しています。
比較は、カルチャ固有の比較を確保するために <xref:System.Globalization.CultureInfo> オブジェクトを使用して実行されます。

使用されるカルチャは言語的な比較に影響します。 次の例は、"en-US" カルチャと "de-DE" カルチャを使用する 2 つのドイツ語の文の比較の結果を示しています。

[!code-csharp-interactive[Comparing strings across cultures](../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs#4)]

カルチャに依存した比較は、一般的にユーザーによる文字列入力とユーザーによる他の文字列入力の比較および並べ替えに使用されます。 これらの文字列の文字および並べ替え規則は、ユーザーのコンピューターのロケールによって異なる場合があります。 同一の文字を含む文字列でも、現在のスレッドのカルチャに応じて、異なる方法で並べ替えられます。 さらに、Windows コンピューターでローカルでこのサンプル コードを試すと、次の結果が表示されます。

```console
<coop> is less than <co-op> using en-US culture
<coop> is greater than <co-op> using ordinal comparison
<coop> is less than <cop> using en-US culture
<coop> is less than <cop> using ordinal comparison
<co-op> is less than <cop> using en-US culture
<co-op> is less than <cop> using ordinal comparison
```

言語的な比較は、現在のカルチャに依存し、OS に依存します。 文字列の比較を使用する場合は、これを考慮する必要があります。

## <a name="linguistic-sorting-and-searching-strings-in-arrays"></a>配列の言語的な並べ替えと文字列の検索

次の例は、現在のカルチャに依存する言語的な比較を使用して、配列内の文字列を並べ替えおよび検索する方法を示しています。 <xref:System.Array> パラメーターを取得する静的な <xref:System.StringComparer?displayProperty=nameWithType> メソッドを使用します。

この例では、現在のカルチャを使用して文字列の配列を並べ替える方法を示します。

[!code-csharp-interactive[Sorting an array of strings](../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs#5)]

配列が並べ替えられた後は、バイナリ検索を使用してエントリを検索できます。 バイナリ検索は、コレクションの中央で開始され、要求された文字列がコレクションのどちらの半分に含まれているかを判断します。 各後続の比較は、半分にコレクションの残りの部分を半分に細分化します。  配列は <xref:System.StringComparer.CurrentCulture?displayProperty=nameWithType> を使用して並べ替えられます。 ローカル関数 `ShowWhere` は、文字列が見つかった場所に関する情報を表示します。 文字列が見つからなかった場合、返された値は、見つからなかった場合にどこにあるべきかを示します。

[!code-csharp-interactive[Searching in a sorted array](../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs#6)]

## <a name="ordinal-sorting-and-searching-in-collections"></a>コレクションの序数に基づく並べ替えと検索

次のコードでは、<xref:System.Collections.Generic.List%601?displayProperty=nameWithType> コレクション クラスを使用して文字列を格納します。 文字列は、<xref:System.Collections.Generic.List%601.Sort%2A?displayProperty=nameWithType> メソッドを使用して並べ替えられます。 このメソッドでは、2 つの文字列の順序を比較するデリゲートが必要です。 <xref:System.String.CompareTo%2A?displayProperty=nameWithType> メソッドは、その比較関数を提供します。 このサンプルを実行し、順序を確認します。 この並べ替え操作では、大文字小文字を区別する序数の並べ替えを使用します。 静的な <xref:System.String.Compare%2A?displayProperty=nameWithType> メソッドを使用し、別の比較規則を指定します。

[!code-csharp-interactive[Sorting a list of strings](../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs#7)]

並べ替えられたら、バイナリ検索を使用して文字列の一覧を検索できます。 次の例では、同じ比較関数を使用して、並べ替えられた一覧を検索する方法を示します。 ローカル関数 `ShowWhere` は、要求されたテキストの場所、またはあるべき場所を示します。

[!code-csharp-interactive[csProgGuideStrings#11](../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs#8)]

並べ替えと検索に常に同じ種類の比較を使用してください。 並べ替えと検索に異なる種類の比較を使用すると、予期しない結果になります。

<xref:System.Collections.Hashtable?displayProperty=nameWithType>、<xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType>、および <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> などのコレクション クラスには、要素またはキーの種類が <xref:System.StringComparer?displayProperty=nameWithType> の場合、`string` パラメーターを取るコンストラクターが用意されています。 通常は、これらのコンストラクターをできるだけ使用し、<xref:System.StringComparer.Ordinal?displayProperty=nameWithType> または <xref:System.StringComparer.OrdinalIgnoreCase?displayProperty=nameWithType> を指定する必要があります。

## <a name="reference-equality-and-string-interning"></a>参照の等価性と文字列インターン

どの例でも <xref:System.Object.ReferenceEquals%2A> を使用していません。 このメソッドは、2 つの文字列が同じオブジェクトかどうかを判断します。 これは、文字列比較で一貫性のない結果につながる可能性があります。 次の例は、C# の*文字列のインターン*機能を示しています。 プログラムで 2 つ以上の同じ文字列変数を宣言すると、コンパイラはそれらをすべて同じ場所に保管します。 <xref:System.Object.ReferenceEquals%2A> メソッドを呼び出すと、2 つの文字列がメモリ内の同じオブジェクトを実際に参照していることを確認できます。 インターン処理を回避するには、<xref:System.String.Copy%2A?displayProperty=nameWithType> メソッドを使用します。 コピーが行われた後、同じ値が含まれていても、2 つの文字列は別の記憶場所を使用します。 次の例を実行し、文字列 `a` と `b` が*インターン処理される*ことを示します。これは、同じ記憶域を共有することを意味します。 文字列 `a` と `c` は異なります。

[!code-csharp-interactive[Demonstrating string interning](../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs#9)]

> [!NOTE]
> 文字列の等価をテストする場合、実行する比較の種類を明示的に指定するメソッドを使用する必要があります。 コードを保守しやすく、読みやすくすることができます。 <xref:System.String?displayProperty=nameWithType> 列挙パラメーターを取る <xref:System.Array?displayProperty=nameWithType> および <xref:System.StringComparison> クラスのメソッドのオーバーロードを使用します。 実行する比較の種類を指定します。 等価をテストするときには、`==` および `!=` 演算子を使用しないでください。 <xref:System.String.CompareTo%2A?displayProperty=nameWithType> インスタンス メソッドは常に、序数に基づいた大文字小文字を区別する比較を実行します。 これらは、文字列をアルファベット順に並べ替える場合に適しています。

<xref:System.String.Intern%2A?displayProperty=nameWithType> メソッドを呼び出すことで、文字列をインターンしたり、既存のインターンされた文字列への参照を取得したりできます。 文字列がインターンされているかどうかを確認するには、<xref:System.String.IsInterned%2A?displayProperty=nameWithType> メソッドを呼び出します。

## <a name="see-also"></a>参照

- <xref:System.Globalization.CultureInfo?displayProperty=nameWithType>
- <xref:System.StringComparer?displayProperty=nameWithType>
- [文字列](../programming-guide/strings/index.md)
- [文字列の比較](../../standard/base-types/comparing.md)
- [アプリケーションのグローバライズとローカライズ](/visualstudio/ide/globalizing-and-localizing-applications)
