---
title: 標準 TimeSpan 書式指定文字列
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- format specifiers, standard time interval
- format strings
- standard time interval format strings
- standard format strings, time intervals
- format specifiers, time intervals
- time intervals [.NET Framework], formatting
- time [.NET Framework], formatting
- formatting [.NET Framework], time
- standard TimeSpan format strings
- formatting [.NET Framework], time intervals
ms.assetid: 9f6c95eb-63ae-4dcc-9c32-f81985c75794
ms.openlocfilehash: ec06edc16829c6d4caf8c760922aac1471e365c2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75346628"
---
# <a name="standard-timespan-format-strings"></a>標準 TimeSpan 書式指定文字列

標準の <xref:System.TimeSpan> 書式指定文字列では、単一の書式指定子の使用により、書式設定操作によって生成される <xref:System.TimeSpan> 値のテキスト表現が定義されます。 空白を含む複数の文字で構成される書式指定文字列は、カスタムの <xref:System.TimeSpan> 書式指定文字列として解釈されます。 詳細については、「[カスタム TimeSpan 書式指定文字列](../../../docs/standard/base-types/custom-timespan-format-strings.md)」を参照してください。  
  
 <xref:System.TimeSpan> 値の文字列形式は、<xref:System.TimeSpan.ToString%2A?displayProperty=nameWithType> メソッドのオーバーロードの呼び出しと、<xref:System.String.Format%2A?displayProperty=nameWithType> などの複合書式指定をサポートするメソッドによって生成されます。 詳細については、「[型の書式設定](../../../docs/standard/base-types/formatting-types.md)」と「[複合書式指定](../../../docs/standard/base-types/composite-formatting.md)」をご覧ください。 次の例では、書式設定操作で標準書式指定文字列を使用する方法を示しています。  
  
 [!code-csharp[Conceptual.TimeSpan.Standard#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.standard/cs/formatexample1.cs#2)]
 [!code-vb[Conceptual.TimeSpan.Standard#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.standard/vb/formatexample1.vb#2)]  
  
 標準の <xref:System.TimeSpan> 書式指定文字列は、解析操作に必要な入力文字列の書式を定義するために <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> メソッドと <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> メソッドでも使用されます (解析では、特定の値の文字列形式が、その値に変換されます)。次のコード例では、解析操作で標準書式指定文字列を使用する方法を示しています。  
  
 [!code-csharp[Conceptual.TimeSpan.Standard#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.standard/cs/parseexample1.cs#3)]
 [!code-vb[Conceptual.TimeSpan.Standard#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.standard/vb/parseexample1.vb#3)]  
  
標準の時間間隔書式指定子を次の表に示します。  
  
|書式指定子|名前|説明|使用例|  
|----------------------|----------|-----------------|--------------|  
|"c"|固定 (不変) 書式|この指定子はカルチャに依存しません。 `[-][d'.']hh':'mm':'ss['.'fffffff]` の書式を使用します。<br /><br /> ("t" と "T" の各書式指定文字列によって生成される結果は同じになります。)<br /><br /> 詳細情報:[固定の ("c") 書式指定子](#the-constant-c-format-specifier)。|`TimeSpan.Zero` -> 00:00:00<br /><br /> `New TimeSpan(0, 0, 30, 0)` -> 00:30:00<br /><br /> `New TimeSpan(3, 17, 25, 30, 500)` -> 3.17:25:30.5000000|  
|"g"|一般の短い書式|この指定子は必要なものだけを出力します。 カルチャに依存し、`[-][d':']h':'mm':'ss[.FFFFFFF]` の書式になります。<br /><br /> 詳細情報:[一般の短い ("g") 書式指定子](#the-general-short-g-format-specifier)。|`New TimeSpan(1, 3, 16, 50, 500)` -> 1:3:16:50.5 (en-US)<br /><br /> `New TimeSpan(1, 3, 16, 50, 500)` -> 1:3:16:50,5 (fr-FR)<br /><br /> `New TimeSpan(1, 3, 16, 50, 599)` -> 1:3:16:50.599 (en-US)<br /><br /> `New TimeSpan(1, 3, 16, 50, 599)` -> 1:3:16:50,599 (fr-FR)|  
|"G"|一般の長い書式|この指定子は、常に日数と 7 桁の小数部を出力します。 カルチャに依存し、`[-]d':'hh':'mm':'ss.fffffff` の書式になります。<br /><br /> 詳細情報:[一般の長い ("G") 書式指定子](#the-general-long-g-format-specifier)。|`New TimeSpan(18, 30, 0)` -> 0:18:30:00.0000000 (en-US)<br /><br /> `New TimeSpan(18, 30, 0)` -> 0:18:30:00,0000000 (fr-FR)|  

## <a name="the-constant-c-format-specifier"></a>固定の ("c") 書式指定子  
 "c" 書式指定子は、次の書式で <xref:System.TimeSpan> 値の文字列形式を返します。  
  
 [-][*d*.]*hh*:*mm*:*ss*[.*fffffff*]  
  
 角かっこ ([ および ]) 内の要素は省略可能です。 コロン (:) とピリオド (.) は、リテラル文字です。 残りの要素について次の表で説明します。  
  
|要素|説明|  
|-------------|-----------------|  
|*-*|負の時間間隔を示す、省略可能なマイナス記号。|  
|*d*|先行ゼロを付けない、省略可能な日数。|  
|*hh*|"00" ～ "23" の範囲の時間数。|  
|*mm*|"00" ～ "59" の範囲の分数。|  
|*ss*|"0" ～ "59" の範囲の秒数。|  
|*fffffff*|省略可能な秒の小数部。  "0000001" (1 ティック、つまり 1,000 万分の 1 秒) ～ "9999999" (1,000 万分の 9,999,999 秒、つまり 1 秒より 1 ティック少ない) までの範囲の値が可能です。|  
  
 "g" および "G" 書式指定子とは異なり、"c" 書式指定子はカルチャに依存しません。 不変で、かつ .NET Framework 4 より前のすべての .NET Framework バージョンに共通する <xref:System.TimeSpan> 値の文字列表現が生成されます。 "c" は、既定の <xref:System.TimeSpan> 書式文字列です。<xref:System.TimeSpan.ToString?displayProperty=nameWithType> メソッドは、"c" 書式指定文字列を使用して時間間隔値の書式を設定します。  
  
> [!NOTE]
> <xref:System.TimeSpan> では、動作が "c" 標準書式指定文字列と同じである "t" と "T" の標準書式指定文字列もサポートされます。  
  
 次の例では、2 つの <xref:System.TimeSpan> オブジェクトのインスタンスを作成し、これらを使用して算術演算を実行し、結果を表示します。 いずれの場合も、複合書式指定を使用し、"c" 書式指定子を使用して <xref:System.TimeSpan> 値を表示します。  
  
 [!code-csharp[Conceptual.TimeSpan.Standard#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.standard/cs/standardc1.cs#1)]
 [!code-vb[Conceptual.TimeSpan.Standard#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.standard/vb/standardc1.vb#1)]  

## <a name="the-general-short-g-format-specifier"></a>一般の短い ("g") 書式指定子  
 "g" <xref:System.TimeSpan> 書式指定子は、必要な要素のみを含めることによって、コンパクトな形式で <xref:System.TimeSpan> 値の文字列形式を返します。 次の書式を使用します。  
  
 [-][*d*:]*h*:*mm*:*ss*[.*FFFFFFF*]  
  
 角かっこ ([ および ]) 内の要素は省略可能です。 コロン (:) は、リテラル文字です。 残りの要素について次の表で説明します。  
  
|要素|説明|  
|-------------|-----------------|  
|*-*|負の時間間隔を示す、省略可能なマイナス記号。|  
|*d*|先行ゼロを付けない、省略可能な日数。|  
|*h*|先行ゼロを付けない、"0" ～ "23" の範囲の時間数。|  
|*mm*|"00" ～ "59" の範囲の分数。|  
|*ss*|"00" ～ "59" の範囲の秒数。|  
|*.*|秒の小数部の区切り記号。 ユーザー オーバーライドなしで指定されているカルチャの <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A> プロパティと同等です。|  
|*FFFFFFF*|秒の小数部。 可能な限り少ない桁数が表示されます。|  
  
 "G" 書式指定子と同様に、"g" 書式指定子はローカライズされます。 その秒の小数部の区切り記号は、現在のカルチャまたは指定されているカルチャの <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A> プロパティに基づきます。  
  
 次の例では、2 つの <xref:System.TimeSpan> オブジェクトのインスタンスを作成し、これらを使用して算術演算を実行し、結果を表示します。 いずれの場合も、複合書式指定を使用し、"g" 書式指定子を使用して <xref:System.TimeSpan> 値を表示します。 また、現在のシステム カルチャ (この場合は、英語 - 米国 (en-US)) とフランス語 - フランス (fr-FR) カルチャの書式指定規則を使用して、<xref:System.TimeSpan> 値の書式を設定します。  
  
 [!code-csharp[Conceptual.TimeSpan.Standard#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.standard/cs/standardshort1.cs#4)]
 [!code-vb[Conceptual.TimeSpan.Standard#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.standard/vb/standardshort1.vb#4)]  

## <a name="the-general-long-g-format-specifier"></a>一般の長い ("G") 書式指定子  
 "G" <xref:System.TimeSpan> 書式指定子は、常に日数と秒の小数部の両方を含む長い形式で、<xref:System.TimeSpan> 値の文字列形式を返します。 "G" 標準書式指定子によって生成される文字列は、次の書式になります。  
  
 [-]*d*:*hh*:*mm*:*ss*.*fffffff*  
  
 角かっこ ([ および ]) 内の要素は省略可能です。 コロン (:) は、リテラル文字です。 残りの要素について次の表で説明します。  
  
|要素|説明|  
|-------------|-----------------|  
|*-*|負の時間間隔を示す、省略可能なマイナス記号。|  
|*d*|先行ゼロを付けない日数。|  
|*hh*|"00" ～ "23" の範囲の時間数。|  
|*mm*|"00" ～ "59" の範囲の分数。|  
|*ss*|"00" ～ "59" の範囲の秒数。|  
|*.*|秒の小数部の区切り記号。 ユーザー オーバーライドなしで指定されているカルチャの <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A> プロパティと同等です。|  
|*fffffff*|秒の小数部。|  
  
 "G" 書式指定子と同様に、"g" 書式指定子はローカライズされます。 その秒の小数部の区切り記号は、現在のカルチャまたは指定されているカルチャの <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A> プロパティに基づきます。  
  
 次の例では、2 つの <xref:System.TimeSpan> オブジェクトのインスタンスを作成し、これらを使用して算術演算を実行し、結果を表示します。 いずれの場合も、複合書式指定を使用し、"G" 書式指定子を使用して <xref:System.TimeSpan> 値を表示します。 また、現在のシステム カルチャ (この場合は、英語 - 米国 (en-US)) とフランス語 - フランス (fr-FR) カルチャの書式指定規則を使用して、<xref:System.TimeSpan> 値の書式を設定します。  
  
 [!code-csharp[Conceptual.TimeSpan.Standard#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.standard/cs/standardlong1.cs#5)]
 [!code-vb[Conceptual.TimeSpan.Standard#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.standard/vb/standardlong1.vb#5)]
  
## <a name="see-also"></a>関連項目

- [型の書式設定](../../../docs/standard/base-types/formatting-types.md)
- [カスタム時間間隔書式指定文字列](../../../docs/standard/base-types/custom-timespan-format-strings.md)
- [文字列の解析](../../../docs/standard/base-types/parsing-strings.md)
