---
title: '方法: 文字列を DateTime に変換する'
description: 日付と時刻を表す文字列を解析して、その文字列から DateTime を作成する手法について説明します。
ms.date: 02/15/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parsing strings, date and time strings
- date and time strings
- ParseExact method
- enumerations [.NET Framework], parsing strings
- base types, parsing strings
- DateTime object
- time strings
ms.openlocfilehash: 9555304e570226b2ed3b040735cf099b5a018f93
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78156544"
---
# <a name="parsing-date-and-time-strings-in-net"></a>.NET での日付と時刻文字列の解析

文字列を解析して <xref:System.DateTime> オブジェクトに変換するには、日付と時刻がテキストとしてどのように表現されるのかを指定する必要があります。 異なるカルチャでは、日、月、年の並び順が異なります。 24 時間制を使用する時間表記があり、また "AM" および "PM" を指定する時間表記もあります。 一部のアプリケーションは日付のみを必要とします。 時刻のみを必要とするアプリケーションもあります。 日付と時刻の両方を指定する必要があるアプリケーションもあります。 文字列を <xref:System.DateTime> オブジェクトに変換するメソッドを使用すると、想定する書式、およびアプリケーションに必要な日付と時刻の要素について、詳しく指定できます。 テキストを正しく <xref:System.DateTime> に変換するには、3 つのサブタスクが必要です。

1. 日付と時刻を表すテキストについて、どのような書式を想定するのか指定する必要があります。
1. 日時の書式に対してカルチャを指定する場合があります。
1. テキストの表現で不足している構成要素を、日付と時刻でどのように設定するのか指定する場合があります。

<xref:System.DateTime.Parse%2A> メソッドと <xref:System.DateTime.TryParse%2A> メソッドでは、多くの一般的な日付と時刻の表現を変換します。 <xref:System.DateTime.ParseExact%2A> メソッドと <xref:System.DateTime.TryParseExact%2A> メソッドでは、日付と時刻の書式指定文字列で指定されたパターンに適合する文字列形式を変換します。 (詳細については、「[標準の日時書式指定文字列](standard-date-and-time-format-strings.md)」および「[カスタム日時書式指定文字列](custom-date-and-time-format-strings.md)」の記事をご覧ください。)

現在の <xref:System.Globalization.DateTimeFormatInfo> オブジェクトでは、テキストを日付と時刻として解釈する方法について、より細かく制御できます。 <xref:System.Globalization.DateTimeFormatInfo> のプロパティでは、日付と時刻の区切り記号、月、日、時代 (年号) の名前、"AM" および "PM" を指定する書式について記述します。 現在のスレッド カルチャでは、現在のカルチャを表す <xref:System.Globalization.DateTimeFormatInfo> が提供されます。 特定のカルチャまたはカスタム設定が必要な場合は、解析メソッドの <xref:System.IFormatProvider> パラメーターを指定します。 <xref:System.IFormatProvider> パラメーターには、カルチャを表す <xref:System.Globalization.CultureInfo> オブジェクト、または <xref:System.Globalization.DateTimeFormatInfo> オブジェクトを指定します。

日付や時刻を表すテキストでは、一部の情報が不足している場合があります。 たとえば、ほとんどの人は、「3 月 12 日」という日付が今年の日付を表していると仮定するでしょう。 同様に、"2018年 3 月" は 2018 年の 3 月を表します。 多くの場合、時刻を表すテキストでは、時間、分、AM/PM の指定のみが含まれます。  解析メソッドでは、適切な既定値を使用して、この情報の不足が処理されます。

- 時刻のみが存在する場合、日付には現在の日付が使用されます。
- 日付のみが存在する場合、時刻には午前 0 時が使用されます。
- 日付で年が指定されていない場合、現在の年が使用されます。
- 月の日付が指定されていない場合、月の最初の日付が使用されます。

文字列内に日付が存在する場合、それには月、および日または年のいずれかが含まれている必要があります。 時刻が存在する場合、それには時間、および分または AM/PM 指定子のいずれかが含まれている必要があります。

<xref:System.Globalization.DateTimeStyles.NoCurrentDateDefault> 定数を指定して、各既定値をオーバーライドできます。 この定数を使用する場合、不足しているすべての年、月、または日のプロパティは値 `1` に設定されます。 [ を使用する](#styles-example)最後の例<xref:System.DateTime.Parse%2A>では、この動作について説明しています。

日付と時刻のコンポーネントだけでなく、日付と時刻の文字列形式には、世界協定時刻 (UTC) と何時間異なるかを示すオフセットを含めることができます。 たとえば、文字列 "2/14/2007 5:32:00 -7:00" は、UTC より 7 時間早い時刻を定義します。 オフセットが時刻の文字列形式から省略される場合、解析では、その <xref:System.DateTime> プロパティに <xref:System.DateTime.Kind%2A> を設定して、<xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType> オブジェクトを返します。 オフセットが指定されている場合、解析では、その <xref:System.DateTime> プロパティに <xref:System.DateTime.Kind%2A> を設定した <xref:System.DateTimeKind.Local?displayProperty=nameWithType> オブジェクトと、使用しているマシンのローカル タイム ゾーンに調整された値を返します。 解析メソッドで <xref:System.Globalization.DateTimeStyles> 値を使用することで、この動作を変更できます。
  
書式プロバイダーは、あいまいな数値の日付を解釈するためにも使用されます。 文字列 "02/03/04" で表される日付は、どの構成要素が月、日、年なのかはっきりしません。 構成要素は、書式プロバイダーにある類似した日付書式と同じ順序で解釈されます。

## <a name="parse"></a>Parse

<xref:System.DateTime.Parse%2A?displayProperty=nameWithType> メソッドを使用して `string` を <xref:System.DateTime> に変換する方法の例を次に示します。 この例では、現在のスレッドに関連付けられているカルチャを使用します。 現在のカルチャに関連付けられている <xref:System.Globalization.CultureInfo> で入力文字列を解析できない場合、<xref:System.FormatException> がスローされます。

> [!TIP]
> この記事にあるすべての C# サンプルは、ブラウザーで実行できます。 出力を確認するには、 **[実行]** ボタンを押します。 また、サンプルを編集して自分で実験することもできます。

> [!NOTE]
> これらの例は、[C#](https://github.com/dotnet/samples/tree/master/snippets/csharp/how-to/conversions) と [Visual Basic](https://github.com/dotnet/samples/tree/master/snippets/visualbasic/how-to/conversions) 両方の GitHub ドキュメント リポジトリで使用可能です。 または、[C#](https://github.com/dotnet/samples/raw/master/snippets/csharp/how-to/conversions.zip) または [Visual Basic](https://github.com/dotnet/samples/raw/master/snippets/visualbasic/how-to/conversions.zip) の Zip ファイルとしてプロジェクトをダウンロードできます。

[!code-csharp-interactive[Parsing.DateAndTime#1](../../../samples/snippets/csharp/how-to/conversions/StringToDateTime.cs#1)]
[!code-vb[Parsing.DateAndTime#1](../../../samples/snippets/visualbasic/how-to/conversions/Program.vb#1)]

文字列を解析するときに使用される書式規則を含むカルチャを、明示的に定義することもできます。 <xref:System.Globalization.DateTimeFormatInfo> プロパティによって返される標準 <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType> オブジェクトのいずれかを指定します。 次の例では、書式プロバイダーを使用して、ドイツ語の文字列を <xref:System.DateTime> に解析します。 これにより、<xref:System.Globalization.CultureInfo> カルチャを表す `de-DE` が作成されます。 この `CultureInfo` オブジェクトによって、この特定の文字列が正常に解析されます。 これによって、<xref:System.Threading.Thread.CurrentCulture> の <xref:System.Threading.Thread.CurrentThread> がどのように設定されていても除外されます。  
  
[!code-csharp[Parsing.DateAndTime#2](../../../samples/snippets/csharp/how-to/conversions/StringToDateTime.cs#2)]
[!code-vb[Parsing.DateAndTime#2](../../../samples/snippets/visualbasic/how-to/conversions/Program.vb#2)]

ただし、<xref:System.DateTime.Parse%2A> メソッドのオーバーロードを使用してカスタム書式プロバイダーを指定することはできますが、このメソッドでは非標準の書式の解析はサポートされません。 非標準の書式設定で示された日付と時刻を解析するには、代わりに <xref:System.DateTime.ParseExact%2A> メソッドを使用します。  

<a name="styles-example"></a>次の例では、<xref:System.Globalization.DateTimeStyles> 列挙体を使用して、未指定のフィールドの <xref:System.DateTime> に現在の日付と時刻の情報を追加しないことを指定します。  

[!code-csharp[Parsing.DateAndTime#3](../../../samples/snippets/csharp/how-to/conversions/StringToDateTime.cs#3)]
[!code-vb[Parsing.DateAndTime#3](../../../samples/snippets/visualbasic/how-to/conversions/Program.vb#3)]

## <a name="parseexact"></a>ParseExact

<xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType> メソッドでは、文字列が指定された文字列パターンのいずれかに準拠する場合、文字列を <xref:System.DateTime> オブジェクトに変換します。 指定された形式のいずれでもない文字列がこのメソッドに渡された場合、<xref:System.FormatException> がスローされます。 標準の日付と時刻の書式指定子のいずれか、またはカスタムの書式指定子の組み合わせを指定することができます。 カスタムの書式指定子を使用すると、カスタムの認識文字列を構成することができます。 (指定子の詳細については、[標準の日付と時刻の書式指定文字列](standard-date-and-time-format-strings.md)と[カスタムの日付と時刻の書式指定文字列](custom-date-and-time-format-strings.md)に関するトピックをご覧ください)。  

次の例では、<xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType> メソッドに、解析する文字列オブジェクト、書式指定子、<xref:System.Globalization.CultureInfo> オブジェクトが順に渡されます。 この <xref:System.DateTime.ParseExact%2A> メソッドでは、`en-US` カルチャの長い日付パターンに従う文字列のみを解析できます。  

[!code-csharp[Parsing.DateAndTime#4](../../../samples/snippets/csharp/how-to/conversions/StringToDateTime.cs#4)]
[!code-vb[Parsing.DateAndTime#4](../../../samples/snippets/visualbasic/how-to/conversions/Program.vb#4)]

<xref:System.DateTime.Parse%2A> メソッドと <xref:System.DateTime.ParseExact%2A> メソッドの各オーバーロードには、<xref:System.IFormatProvider> パラメーターもあります。このパラメーターでは、文字列の書式設定に関するカルチャ固有の情報が指定されます。 この <xref:System.IFormatProvider> オブジェクトは、標準的なカルチャを表す <xref:System.Globalization.CultureInfo> オブジェクトか、<xref:System.Globalization.DateTimeFormatInfo> プロパティによって返される <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType> オブジェクトです。  <xref:System.DateTime.ParseExact%2A> では、日付と時刻のカスタム書式を 1 つ以上定義する、文字列または文字列配列の追加の引数も使用されます。  

## <a name="see-also"></a>参照

- [文字列の解析](parsing-strings.md)
- [型の書式設定](formatting-types.md)
- [.NET での型変換](type-conversion.md)
- [標準の日付と時刻の書式](standard-date-and-time-format-strings.md)
- [カスタムの日時と書式指定文字列](custom-date-and-time-format-strings.md)
