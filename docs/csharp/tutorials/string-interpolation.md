---
title: C# における文字列補間
description: C# の文字列補間を使用した結果文字列に書式設定された式の結果を含める方法について説明します。
author: pkulikov
ms.technology: csharp-fundamentals
ms.date: 09/02/2019
ms.openlocfilehash: b901ae661ebd4af625d9f3c999b0eb50dda1990d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73039207"
---
# <a name="string-interpolation-in-c"></a>C\# における文字列補間

このチュートリアルは、[文字列補間](../language-reference/tokens/interpolated.md)を使用して結果文字列に式の結果を書式設定したものを含める方法を示しています。 例では、基本的な C# の概念と .NET の型の書式設定について理解していることを前提としています。 文字列補間や .NET の型の書式設定の経験がない場合は、最初に[対話型の文字列補間に関するチュートリアル](exploration/interpolated-strings.yml)を参照してください。 .NET の型の書式設定の詳細については、「[.NET での型の書式設定](../../standard/base-types/formatting-types.md)」のトピックを参照してください。

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

## <a name="introduction"></a>はじめに

[文字列補間](../language-reference/tokens/interpolated.md)機能は、[複合書式設定](../../standard/base-types/composite-formatting.md)機能の上に構築されていて、結果文字列に書式設定された式の結果を含めるためのより読みやすく、便利な構文を提供します。

文字列リテラルを挿入文字列として識別するため、先頭に `$` の記号を追加してください。 挿入文字列の値を返す、有効な C# の式を埋め込むことができます。 次の例では、式が評価されるとすぐにその結果が文字列に変換され、結果文字列に含まれています。

[!code-csharp-interactive[string interpolation example](~/samples/snippets/csharp/tutorials/string-interpolation/Program.cs#1)]

例に示すように、式を中かっこで囲むことで挿入文字列に含めることができます。

```csharp
{<interpolationExpression>}
```

挿入文字列では、[文字列の複合書式設定](../../standard/base-types/composite-formatting.md)機能に含まれるすべての機能がサポートされます。 そのため、<xref:System.String.Format%2A?displayProperty=nameWithType> メソッドを使用するよりも読みやすい代替手法になります。

## <a name="how-to-specify-a-format-string-for-an-interpolation-expression"></a>補間式の書式設定文字列を指定する方法

コロン (":") と書式設定文字列を持つ補間式に従って、式の結果の型でサポートされる書式設定文字列を指定します。

```csharp
{<interpolationExpression>:<formatString>}
```

次の例は、日時や数値による結果を生成する式の標準とカスタムの書式設定文字列を指定する方法を示しています。

[!code-csharp-interactive[format string example](~/samples/snippets/csharp/tutorials/string-interpolation/Program.cs#2)]

詳細については、「[複合書式設定](../../standard/base-types/composite-formatting.md#format-string-component)」トピックの「[Format String コンポーネント](../../standard/base-types/composite-formatting.md)」のセクションを参照してください。 このセクションでは、.NET の基本データ型でサポートされる標準とカスタムの書式設定文字列について説明するトピックへのリンクを提供しています。

## <a name="how-to-control-the-field-width-and-alignment-of-the-formatted-interpolation-expression"></a>書式設定された補間式のフィールドの幅と配置を制御する方法

コンマ (",") と定数式を持つ補間式に従って、書式設定された式の結果の最小フィールド幅と配置を指定します。

```csharp
{<interpolationExpression>,<alignment>}
```

*alignment* の値が正の値である場合、書式設定された式の結果は右揃えになります。負の値である場合は、左揃えになります。

配置と書式設定文字列の両方を指定する必要がある場合は、alignment コンポーネントから開始します。

```csharp
{<interpolationExpression>,<alignment>:<formatString>}
```

次の例は、配置を指定する方法を示し、テキスト フィールドを区切るためにパイプ文字 ("|") を使用しています。

[!code-csharp-interactive[alignment example](~/samples/snippets/csharp/tutorials/string-interpolation/Program.cs#3)]

出力例が示すように、書式設定された式の結果の長さが指定したフィールドの幅を超える場合、*alignment* の値は無視されます。

詳細については、「[複合書式設定](../../standard/base-types/composite-formatting.md#alignment-component)」トピックの「[Alignment コンポーネント](../../standard/base-types/composite-formatting.md)」のセクションを参照してください。

## <a name="how-to-use-escape-sequences-in-an-interpolated-string"></a>挿入文字列でエスケープ シーケンスを使用する方法

挿入文字列は、通常の文字列リテラルで使用できるすべてのエスケープ シーケンスをサポートします。 詳細については、「[文字列のエスケープ シーケンス](../programming-guide/strings/index.md#string-escape-sequences)」を参照してください。

エスケープ シーケンスをリテラルで解釈するには、[verbatim](../language-reference/tokens/verbatim.md) 文字列リテラルを使用します。 verbatim 補間文字列は、`$` 文字で始まり、`@` 文字が続きます。 C# 8.0 以降では、`$` と `@` のトークンを任意の順序で使用できます。`$@"..."` と `@$"..."` はどちらも有効な verbatim 補間文字列です。

中かっこ "{" または "}" を結果文字列に含める場合は、2 つの中かっこ "{{" または "}}" を使用します。 詳細については、「[複合書式設定](../../standard/base-types/composite-formatting.md#escaping-braces)」トピックの「[エスケープ中かっこ ({})](../../standard/base-types/composite-formatting.md)」のセクションを参照してください。

次の例は、結果文字列に中かっこを含め、verbatim 挿入文字列を作成する方法を示しています。

[!code-csharp-interactive[escape sequence example](~/samples/snippets/csharp/tutorials/string-interpolation/Program.cs#4)]

## <a name="how-to-use-a-ternary-conditional-operator--in-an-interpolation-expression"></a>補間式で三項条件演算子 `?:` を使用する方法

コロン (":") が補間式の項目で特別な意味を持つときに、式で[条件演算子](../language-reference/operators/conditional-operator.md)を使用するには、次の例が示すようにその式をかっこで囲みます。

[!code-csharp-interactive[conditional operator example](~/samples/snippets/csharp/tutorials/string-interpolation/Program.cs#5)]

## <a name="how-to-create-a-culture-specific-result-string-with-string-interpolation"></a>文字列補間を使用してカルチャ固有の結果文字列を作成する方法

既定では、挿入文字列は、すべての書式設定操作に対して <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=nameWithType> プロパティで定義された現在のカルチャを使用します。 カルチャ固有の結果文字列を作成するには、<xref:System.FormattableString?displayProperty=nameWithType> インスタンスへの挿入文字列の暗黙的変換を使用し、その <xref:System.FormattableString.ToString(System.IFormatProvider)> メソッドを呼び出します。 その方法を次の例に示します。

[!code-csharp-interactive[specify different cultures](~/samples/snippets/csharp/tutorials/string-interpolation/Program.cs#6)]

例に示すように、<xref:System.FormattableString> インスタンスを 1 回使用して、さまざまなカルチャに対する複数の結果文字列を生成することができます。

## <a name="how-to-create-a-result-string-using-the-invariant-culture"></a>インバリアント カルチャを使用して結果文字列を作成する方法

<xref:System.FormattableString.ToString(System.IFormatProvider)?displayProperty=nameWithType> メソッドと共に静的な <xref:System.FormattableString.Invariant%2A?displayProperty=nameWithType> メソッドを使用して、<xref:System.Globalization.CultureInfo.InvariantCulture> の結果文字列に挿入文字列を解決することができます。 その方法を次の例に示します。

[!code-csharp-interactive[format with invariant culture](~/samples/snippets/csharp/tutorials/string-interpolation/Program.cs#7)]

## <a name="conclusion"></a>まとめ

このチュートリアルでは、文字列補間の使用に関する一般的なシナリオについて説明しています。 文字列補間の詳細については、[文字列補間](../language-reference/tokens/interpolated.md)に関するトピックを参照してください。 .NET の型の書式設定の詳細については、「[.NET での型の書式設定](../../standard/base-types/formatting-types.md)」および「[複合書式設定](../../standard/base-types/composite-formatting.md)」のトピックを参照してください。

## <a name="see-also"></a>参照

- <xref:System.String.Format%2A?displayProperty=nameWithType>
- <xref:System.FormattableString?displayProperty=nameWithType>
- <xref:System.IFormattable?displayProperty=nameWithType>
- [文字列](../programming-guide/strings/index.md)
