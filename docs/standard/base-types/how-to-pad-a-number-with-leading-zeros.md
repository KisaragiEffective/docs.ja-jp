---
title: '方法: 数値に先行するゼロを埋め込む'
ms.date: 02/25/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- numeric format strings [.NET Framework]
- formatting [.NET Framework], numbers
- number formatting [.NET Framework]
- numbers [.NET Framework], format strings
ms.assetid: 0b2c2cb5-c580-4891-8d81-cb632f5ec384
ms.openlocfilehash: bc3c4b75c484274c214141d8fbfcf8ac592b0b99
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73131975"
---
# <a name="how-to-pad-a-number-with-leading-zeros"></a>方法: 数値に先行するゼロを埋め込む

整数値に先行ゼロを追加するには、精度指定子と[標準の数値書式指定文字列](../../../docs/standard/base-types/standard-numeric-format-strings.md) "D" を使用します。 整数値と浮動小数点数値の両方に先行ゼロを追加するには、[カスタム数値書式指定文字列](../../../docs/standard/base-types/custom-numeric-format-strings.md)を使用します。 この記事では、この両方の方法を使用して数値に先行ゼロを埋め込む方法を示します。

## <a name="to-pad-an-integer-with-leading-zeros-to-a-specific-length"></a>特定の長さになるまで整数値に先行ゼロを埋め込むには

1. 整数値を表示する際の最小桁数を決定します。 この数には先行桁数も含みます。

1. 整数値を 10 進数値と 16 進数値のどちらで表示するかを決定します。

    - 整数値を 10 進数値として表示するには、`ToString(String)` メソッドを呼び出し、 *パラメーターの値として文字列 "D*n`format`" を渡します。この *n* は、文字列の最小長を表します。

    - 整数値を 16 進数値として表示するには、`ToString(String)` メソッドを呼び出し、format パラメーターの値として文字列 "X*n*" を渡します。この *n* は、文字列の最小長を表します。

また、[C#](../../csharp/language-reference/tokens/interpolated.md) と [Visual Basic](../../visual-basic/programming-guide/language-features/strings/interpolated-strings.md) の両方で挿入文字列に書式指定文字列を使用することも、<xref:System.String.Format%2A?displayProperty=nameWithType>複合書式指定<xref:System.Console.WriteLine%2A?displayProperty=nameWithType>を使用するメソッド ([ や ](../../../docs/standard/base-types/composite-formatting.md) など) を呼び出すこともできます。

次の例は、書式指定された数値全体の長さが 8 文字以上になるように、先行ゼロを使用してさまざまな数値を書式指定します。

[!code-csharp[Formatting.HowTo.PadNumber#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.PadNumber/cs/Pad1.cs#1)]
[!code-vb[Formatting.HowTo.PadNumber#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.PadNumber/vb/Pad1.vb#1)]

## <a name="to-pad-an-integer-with-a-specific-number-of-leading-zeros"></a>特定の数の先行ゼロを整数値に埋め込むには

1. 整数値に表示する先行ゼロの数を決定します。

1. 整数値を 10 進数値と 16 進数値のどちらで表示するかを決定します。

    - それを 10 進数値として書式指定すると、"D" 標準書式指定子を使用する必要があります。

    - それを 16 進数値として書式指定すると、"X" 標準書式指定子を使用する必要があります。

1. 整数値の `ToString("D").Length` メソッドまたは `ToString("X").Length` メソッドを使用して、先行ゼロが埋め込まれていない数値文字列の長さを決定します。

1. 書式指定した文字列に埋め込む先行ゼロの数を、埋め込まれていない数値の文字列の長さに加算します。 先行ゼロの数を加算すると、埋め込み文字列の全体の長さが定義されます。

1. 整数値の `ToString(String)` メソッドを呼び出し、10 進数値文字列の場合は "D*n*"、16 進数値の場合は "X*n*" を渡します。*n* は埋め込み文字列全体の長さを表します。 書式指定文字列 "D*n*" または "X*n*"は、複合書式指定をサポートするメソッドでも使用できます。

次の例は、整数値に 5 つの先行ゼロを埋め込みます。

[!code-csharp[Formatting.HowTo.PadNumber#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.PadNumber/cs/Pad1.cs#2)]
[!code-vb[Formatting.HowTo.PadNumber#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.PadNumber/vb/Pad1.vb#2)]

## <a name="to-pad-a-numeric-value-with-leading-zeros-to-a-specific-length"></a>特定の長さになるまで数値に先行ゼロを埋め込むには

1. 文字列形式の数値の整数部分の桁数を決定します。 合計桁数には先行ゼロも含みます。

1. ゼロ プレースホルダー "0" を使用してゼロの最小数を表すカスタム数値書式指定文字列を定義します。

1. 数値の `ToString(String)` メソッドを呼び出し、カスタム書式指定文字列を渡します。 カスタム書式指定文字列はまた、文字列補間や、複合書式指定をサポートするメソッドでも使用できます。

次の例では、いくつかの数値を先行ゼロを使用して書式指定しています。 その結果、書式指定された数値の全体の長さは整数部が少なくとも 8 桁になります。

[!code-csharp[Formatting.HowTo.PadNumber#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.PadNumber/cs/Pad1.cs#3)]
[!code-vb[Formatting.HowTo.PadNumber#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.PadNumber/vb/Pad1.vb#3)]

## <a name="to-pad-a-numeric-value-with-a-specific-number-of-leading-zeros"></a>特定の数の先行ゼロを数値に埋め込むには

1. 数値に埋め込む先行ゼロの数を決定します。

1. 先行ゼロが埋め込まれていない数値文字列での整数部の桁数を決定します。

    1. 文字列形式の数値に小数点が含まれるかどうかを決定します。

    1. 小数点記号を含める場合は、整数部分の文字数を決定します。

         または

         小数点記号を含めない場合は、文字列の長さを決定します。

1. 次のものを使用するカスタム書式指定文字列を作成します。

    - 文字列に表示する各先行ゼロに対するゼロ プレースホルダー "0"。

    - 既定の文字列内の各桁を表すゼロ プレースホルダーまたは桁プレースホルダー "#" のどちらか。

1. 数値の `ToString(String)` メソッド、または複合書式指定をサポートするメソッドのパラメーターとして、カスタム書式指定文字列を指定します。

次の例は、2 つの <xref:System.Double> 値を 5 つの先行ゼロで埋め込みます。

[!code-csharp[Formatting.HowTo.PadNumber#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.PadNumber/cs/Pad1.cs#4)]
[!code-vb[Formatting.HowTo.PadNumber#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.PadNumber/vb/Pad1.vb#4)]

## <a name="see-also"></a>参照

- [カスタム数値書式指定文字列](../../../docs/standard/base-types/custom-numeric-format-strings.md)
- [標準の数値書式指定文字列](../../../docs/standard/base-types/standard-numeric-format-strings.md)
- [複合書式指定](../../../docs/standard/base-types/composite-formatting.md)
