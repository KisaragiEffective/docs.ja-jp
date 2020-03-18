---
title: 標準の日時書式指定文字列
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- formatting [.NET Framework], dates
- custom DateTime format string
- format specifiers, custom date and time
- format strings
- custom date and time format strings
- formatting [.NET Framework], time
- date and time strings
ms.assetid: bb79761a-ca08-44ee-b142-b06b3e2fc22b
ms.openlocfilehash: 883902142a91e275ab64ad5d12c197c665bd9b36
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79398473"
---
# <a name="standard-date-and-time-format-strings"></a>標準の日時書式指定文字列

標準の日時書式指定文字列は、単一の書式指定子を使用して日付と時刻の値のテキスト表現を定義します。 空白を含む複数の文字で構成される日時書式指定文字列は、カスタム日時書式指定文字列として解釈されます。詳細については、「[カスタム日時書式指定文字列](../../../docs/standard/base-types/custom-date-and-time-format-strings.md)」をご覧ください。 標準またはカスタムの書式指定文字列には、次の 2 とおりの使用方法があります。

- 書式設定操作によって生成される文字列を定義する。

- 解析操作によって <xref:System.DateTime> 値または <xref:System.DateTimeOffset> 値に変換できる日付と時刻の値のテキスト表現を定義する。

> [!TIP]
> **書式指定ユーティリティ**である .NET Core Windows Forms をダウンロードできます。このアプリケーションを使用すると、書式指定文字列を数値または日付と時刻の値に適用して、結果の文字列を表示できます。 ソース コードは [C#](https://docs.microsoft.com/samples/dotnet/samples/winforms-formatting-utility-cs) と [Visual Basic](https://docs.microsoft.com/samples/dotnet/samples/winforms-formatting-utility-vb) で利用できます。

標準の日時書式指定文字列は、<xref:System.DateTime> 値で使用することも、<xref:System.DateTimeOffset> 値で使用することもできます。

[!INCLUDE[C# interactive-note](~/includes/csharp-interactive-with-utc-partial-note.md)]

<a name="table"></a>標準日時書式指定子を次の表に示します。 特に明記されない限り、特定の標準日時書式指定子は、<xref:System.DateTime> 値で使用しても、<xref:System.DateTimeOffset> 値で使用してもまったく同じ文字列形式を生成します。 標準の日時書式指定文字列の使用方法については、「[メモ](#Notes)」をご覧ください。

|書式指定子|説明|使用例|
|----------------------|-----------------|--------------|
|"d"|短い形式の日付パターン。<br /><br /> 詳細については、「[短い形式の日付 ("d") 書式指定子](#ShortDate)」を参照してください。|2009-06-15T13:45:30 -> 6/15/2009 (en-US)<br /><br /> 2009-06-15T13:45:30 -> 15/06/2009 (fr-FR)<br /><br /> 2009-06-15T13:45:30 -> 2009/06/15 (ja-JP)|
|"D"|長い形式の日付パターン。<br /><br /> 詳細については、「[長い形式の日付 ("D") 書式指定子](#LongDate)」を参照してください。|2009-06-15T13:45:30 -> Monday, June 15, 2009 (en-US)<br /><br /> 2009-06-15T13:45:30 -> 15 июня 2009 г. (ru-RU)<br /><br /> 2009-06-15T13:45:30 -> Montag, 15. Juni 2009 (de-DE)|
|"f"|完全な日付と時刻のパターン (短い形式の時刻)。<br /><br /> 詳細情報:[完全な日付と短い形式の時刻 ("f") 書式指定子](#FullDateShortTime)。|2009-06-15T13:45:30 -> Monday, June 15, 2009 1:45 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> den 15 juni 2009 13:45 (sv-SE)<br /><br /> 2009-06-15T13:45:30 -> Δευτέρα, 15 Ιουνίου 2009 1:45 μμ (el-GR)|
|"F"|完全な日付と時刻のパターン (長い形式の時刻)。<br /><br /> 詳細情報:[完全な日付と長い形式の時刻 ("F") 書式指定子](#FullDateLongTime)。|2009-06-15T13:45:30 -> Monday, June 15, 2009 1:45:30 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> den 15 juni 2009 13:45:30 (sv-SE)<br /><br /> 2009-06-15T13:45:30 -> Δευτέρα, 15 Ιουνίου 2009 1:45:30 μμ (el-GR)|
|"g"|一般の日付と時刻のパターン (短い形式の時刻)。<br /><br /> 詳細情報:[一般の日付と短い形式の時刻 ("g") 書式指定子](#GeneralDateShortTime)。|2009-06-15T13:45:30 -> 6/15/2009 1:45 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> 15/06/2009 13:45 (es-ES)<br /><br /> 2009-06-15T13:45:30 -> 2009/6/15 13:45 (zh-CN)|
|"G"|一般の日付と時刻のパターン (長い形式の時刻)。<br /><br /> 詳細情報:[一般の日付と長い形式の時刻 ("G") 書式指定子](#GeneralDateLongTime)。|2009-06-15T13:45:30 -> 6/15/2009 1:45:30 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> 15/06/2009 13:45:30 (es-ES)<br /><br /> 2009-06-15T13:45:30 -> 2009/6/15 13:45:30 (zh-CN)|
|"M"、"m"|月日パターン。<br /><br /> 詳細情報:[月 ("M"、"m") 書式指定子](#MonthDay)。|2009-06-15T13:45:30 -> June 15 (en-US)<br /><br /> 2009-06-15T13:45:30 -> 15. juni (da-DK)<br /><br /> 2009-06-15T13:45:30 -> 15 Juni (id-ID)|
|"O"、"o"|ラウンドトリップする日付と時刻のパターン。<br /><br /> 詳細情報:[ラウンドトリップ ("O"、"o") 書式指定子](#Roundtrip)。|<xref:System.DateTime> の値:<br /><br /> 2009-06-15T13:45:30 (DateTimeKind.Local) --> 2009-06-15T13:45:30.0000000-07:00<br /><br /> 2009-06-15T13:45:30 (DateTimeKind.Utc) --> 2009-06-15T13:45:30.0000000Z<br /><br /> 2009-06-15T13:45:30 (DateTimeKind.Unspecified) --> 2009-06-15T13:45:30.0000000<br /><br /> <xref:System.DateTimeOffset> の値:<br /><br /> 2009-06-15T13:45:30-07:00 --> 2009-06-15T13:45:30.0000000-07:00|
|"R"、"r"|RFC1123 パターン。<br /><br /> 詳細情報:[RFC1123 ("R"、"r") 書式指定子](#RFC1123)。|2009-06-15T13:45:30 -> Mon, 15 Jun 2009 20:45:30 GMT|
|"s"|並べ替え可能な日付と時刻のパターン。<br /><br /> 詳細情報:[並べ替え可能な日付と時刻 ("s") 書式指定子](#Sortable)。|2009-06-15T13:45:30 (DateTimeKind.Local) -> 2009-06-15T13:45:30<br /><br /> 2009-06-15T13:45:30 (DateTimeKind.Utc) -> 2009-06-15T13:45:30|
|"t"|短い形式の時刻パターン。<br /><br /> 詳細情報:[短い形式の時刻 ("t") 書式指定子](#ShortTime)。|2009-06-15T13:45:30 -> 1:45 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> 13:45 (hr-HR)<br /><br /> 2009-06-15T13:45:30 -> 01:45 م (ar-EG)|
|"T"|長い形式の時刻パターン。<br /><br /> 詳細情報:[長い形式の時刻 ("T") 書式指定子](#LongTime)。|2009-06-15T13:45:30 -> 1:45:30 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> 13:45:30 (hr-HR)<br /><br /> 2009-06-15T13:45:30 -> 01:45:30 م (ar-EG)|
|"u"|並べ替え可能な日付と時刻のパターン (世界時刻)。<br /><br /> 詳細情報:[世界共通の並べ替え可能な日付と時刻 ("u") 書式指定子](#UniversalSortable)。|<xref:System.DateTime> 値の場合: 2009-06-15T13:45:30 -> 2009-06-15 13:45:30Z<br /><br /> <xref:System.DateTimeOffset> 値の場合: 2009-06-15T13:45:30 -> 2009-06-15 20:45:30Z|
|"U"|完全な日付と時刻のパターン (世界時刻)。<br /><br /> 詳細情報:[世界共通の完全な日付と時刻 ("U") 書式指定子](#UniversalFull)。|2009-06-15T13:45:30 -> Monday, June 15, 2009 8:45:30 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> den 15 juni 2009 20:45:30 (sv-SE)<br /><br /> 2009-06-15T13:45:30 -> Δευτέρα, 15 Ιουνίου 2009 8:45:30 μμ (el-GR)|
|"Y"、"y"|年月パターン。<br /><br /> 詳細情報:[年月 ("Y") 書式指定子](#YearMonth)。|2009-06-15T13:45:30 -> June, 2009 (en-US)<br /><br /> 2009-06-15T13:45:30 -> juni 2009 (da-DK)<br /><br /> 2009-06-15T13:45:30 -> Juni 2009 (id-ID)|
|その他の 1 文字|未定義の指定子。|ランタイム <xref:System.FormatException> をスローします。|

## <a name="how-standard-format-strings-work"></a>標準書式指定文字列の動作

書式設定操作では、標準書式指定文字列はカスタム書式指定文字列のエイリアスにすぎません。 エイリアスを使ってカスタム書式指定文字列を表すことの利点は、カスタム書式指定文字列はそれ自体が変化する場合があるのに対し、エイリアスは不変であるという点です。 通常、日付と時刻の値の文字列形式はカルチャによって変わるため、この点は重要です。 たとえば、"d" 標準書式指定文字列は、日付と時刻の値を短い日付形式のパターンで表示することを意味します。 インバリアント カルチャでは、このパターンは "MM/dd/yyyy" になります。 fr-FR カルチャでは、これが "dd/MM/yyyy" になります。 ja-JP カルチャでは、これが "yyyy/MM/dd" になります。

書式設定操作の標準書式指定文字列が特定のカルチャのカスタム書式指定文字列に対応付けられている場合は、使用するカスタム書式指定文字列に該当するカルチャを次のいずれかの方法で定義できます。

- 既定の (現在の) カルチャを使用できます。 次の例では、現在のカルチャの短い日付形式を使って日付を表示します。 この場合、現在のカルチャは en-US です。

  [!code-csharp-interactive[System.DateTime.Conceptual.Formatting#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/cs/StandardFormats1.cs#1)]
  [!code-vb[System.DateTime.Conceptual.Formatting#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/vb/StandardFormats1.vb#1)]

- 使用する書式のカルチャを表す <xref:System.Globalization.CultureInfo> オブジェクトを、<xref:System.IFormatProvider> パラメーターを持つメソッドに渡すことができます。 次の例では、pt-BR カルチャの短い日付形式を使って日付を表示します。

  [!code-csharp[System.DateTime.Conceptual.Formatting#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/cs/StandardFormats1.cs#2)]
  [!code-vb[System.DateTime.Conceptual.Formatting#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/vb/StandardFormats1.vb#2)]

- 書式情報を提供する <xref:System.Globalization.DateTimeFormatInfo> オブジェクトを、<xref:System.IFormatProvider> パラメーターを持つメソッドに渡すことができます。 次の例では、hr-HR カルチャの <xref:System.Globalization.DateTimeFormatInfo> オブジェクトに基づく短い日付形式で日付を表示します。

  [!code-csharp[System.DateTime.Conceptual.Formatting#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/cs/StandardFormats1.cs#3)]
  [!code-vb[System.DateTime.Conceptual.Formatting#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/vb/StandardFormats1.vb#3)]

> [!NOTE]
> 日付と時刻の値の書式設定に使用するパターンまたは文字列のカスタマイズの詳細については、<xref:System.Globalization.NumberFormatInfo> クラスに関するトピックを参照してください。

標準書式指定文字列には、長いカスタム書式指定文字列に代わる、不変の省略形としての利用価値もあります。 このカテゴリに該当する標準書式指定文字列は、"O" (または "o")、"R" (または "r")、"s"、"u" の 4 つです。 これらの文字列は、インバリアント カルチャで定義されたカスタム書式指定文字列に対応します。 これらの書式指定文字列によって生成される日時値の文字列形式は、カルチャに関係なく一定です。 次の表で、この 4 つの標準日時書式指定文字列について説明します。

|標準書式指定文字列|DateTimeFormatInfo.InvariantInfo プロパティによる定義|カスタム書式指定文字列|
|----------------------------|----------------------------------------------------------|--------------------------|
|"O" または "o"|None|yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'fffffffzz|
|"R" または "r"|<xref:System.Globalization.DateTimeFormatInfo.RFC1123Pattern%2A>|ddd, dd MMM yyyy HH':'mm':'ss 'GMT'|
|"s"|<xref:System.Globalization.DateTimeFormatInfo.SortableDateTimePattern%2A>|yyyy'-'MM'-'dd'T'HH':'mm':'ss|
|"u"|<xref:System.Globalization.DateTimeFormatInfo.UniversalSortableDateTimePattern%2A>|yyyy'-'MM'-'dd HH':'mm':'ss'Z'|

標準書式指定文字列は、解析操作の <xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType> メソッドまたは <xref:System.DateTimeOffset.ParseExact%2A?displayProperty=nameWithType> メソッドで使用することもできます。これらのメソッドでは、解析操作が成功するための特定のパターンに入力文字列が完全に一致している必要があります。 標準書式指定文字列の多くは複数のカスタム書式指定文字列に対応付けられるため、日付と時刻の値はさまざまな書式で表されることがありますが、解析操作は成功します。 <xref:System.Globalization.DateTimeFormatInfo.GetAllDateTimePatterns%28System.Char%29?displayProperty=nameWithType> メソッドを呼び出すことにより、標準書式指定文字列に対応する 1 つまたは複数のカスタム書式指定文字列を確認できます。 次の例では、"d" (短い日付形式のパターン) 標準書式指定文字列に対応付けられているカスタム書式指定文字列を表示します。

[!code-csharp[Formatting.DateAndTime.Standard#17](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/stdandparsing1.cs#17)]
[!code-vb[Formatting.DateAndTime.Standard#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/stdandparsing1.vb#17)]

以降では、<xref:System.DateTime> 値および <xref:System.DateTimeOffset> 値の標準書式指定子について説明します。

<a name="ShortDate"></a>

## <a name="the-short-date-d-format-specifier"></a>短い形式の日付 ("d") 書式指定子

"d" 標準書式指定子は、特定のカルチャの <xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A?displayProperty=nameWithType> プロパティで定義されるカスタム日時書式指定文字列を表します。 たとえば、インバリアント カルチャの <xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A> プロパティによって返されるカスタム書式指定文字列は "MM/dd/yyyy" です。

返される文字列の書式を制御する <xref:System.Globalization.DateTimeFormatInfo> オブジェクト プロパティの一覧を次の表に示します。

|プロパティ|説明|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A>|結果文字列の全体的な書式を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A>|日付の構成要素、つまり年、月、および日を区切る文字列を定義します。|

次の例では、"d" 書式指定子を使用して、日付と時刻の値を表示します。

[!code-csharp[Formatting.DateAndTime.Standard#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#1)]
[!code-vb[Formatting.DateAndTime.Standard#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#1)]

[表のトップへ](#table)

<a name="LongDate"></a>

## <a name="the-long-date-d-format-specifier"></a>長い形式の日付 ("D") 書式指定子

"D" 標準書式指定子は、現在の <xref:System.Globalization.DateTimeFormatInfo.LongDatePattern%2A?displayProperty=nameWithType> プロパティで定義されるカスタム日時書式指定文字列を表します。 たとえば、インバリアント カルチャのカスタム書式指定文字列は "dddd, dd MMMM yyyy" です。

返される文字列の書式を制御する <xref:System.Globalization.DateTimeFormatInfo> オブジェクト プロパティの一覧を次の表に示します。

|プロパティ|説明|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.LongDatePattern%2A>|結果文字列の全体的な書式を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.DayNames%2A>|結果文字列に含まれるローカライズされた日付の名前を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|結果文字列に含まれるローカライズされた月の名前を定義します。|

次の例では、"D" 書式指定子を使用して、日付と時刻の値を表示します。

[!code-csharp[Formatting.DateAndTime.Standard#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#2)]
[!code-vb[Formatting.DateAndTime.Standard#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#2)]

[表のトップへ](#table)

<a name="FullDateShortTime"></a>

## <a name="the-full-date-short-time-f-format-specifier"></a>完全な日付と短い形式の時刻 ("f") 書式指定子

"f" 標準書式指定子は、長い形式の日付 ("D") パターンと短い形式の時刻 ("t") パターンを空白で区切って組み合わせて表します。

結果文字列は、特定の <xref:System.Globalization.DateTimeFormatInfo> オブジェクトの書式情報に影響されます。 返される文字列の書式を制御できる <xref:System.Globalization.DateTimeFormatInfo> オブジェクト プロパティの一覧を次の表に示します。 一部のカルチャの <xref:System.Globalization.DateTimeFormatInfo.LongDatePattern%2A?displayProperty=nameWithType> プロパティおよび <xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A?displayProperty=nameWithType> プロパティによって返されるカスタム書式指定子では、一部のプロパティが使用されない場合があります。

|プロパティ|説明|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.LongDatePattern%2A>|結果文字列の日付要素の書式を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A>|結果文字列の時刻要素の書式を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.DayNames%2A>|結果文字列に含まれるローカライズされた日付の名前を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|結果文字列に含まれるローカライズされた月の名前を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|時刻の構成要素、つまり時間、分、および秒を区切る文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|午前 0 時から正午前までの時刻を 12 時間形式で示す文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|正午から午前 0 時前までの時刻を 12 時間形式で示す文字列を定義します。|

次の例では、"f" 書式指定子を使用して、日付と時刻の値を表示します。

[!code-csharp[Formatting.DateAndTime.Standard#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#3)]
[!code-vb[Formatting.DateAndTime.Standard#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#3)]

[表のトップへ](#table)

<a name="FullDateLongTime"></a>

## <a name="the-full-date-long-time-f-format-specifier"></a>完全な日付と長い形式の時刻 ("F") 書式指定子

"F" 標準書式指定子は、現在の <xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A?displayProperty=nameWithType> プロパティで定義されるカスタム日時書式指定文字列を表します。 たとえば、インバリアント カルチャのカスタム書式指定文字列は "dddd, dd MMMM yyyy HH:mm:ss" です。

返される文字列の書式を制御できる <xref:System.Globalization.DateTimeFormatInfo> オブジェクト プロパティの一覧を次の表に示します。 一部のカルチャの <xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A> プロパティによって返されるカスタム書式指定子では、一部のプロパティが使用されない場合があります。

|プロパティ|説明|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A>|結果文字列の全体的な書式を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.DayNames%2A>|結果文字列に含まれるローカライズされた日付の名前を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|結果文字列に含まれるローカライズされた月の名前を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|時刻の構成要素、つまり時間、分、および秒を区切る文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|午前 0 時から正午前までの時刻を 12 時間形式で示す文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|正午から午前 0 時前までの時刻を 12 時間形式で示す文字列を定義します。|

次の例では、"F" 書式指定子を使用して、日付と時刻の値を表示します。

[!code-csharp[Formatting.DateAndTime.Standard#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#4)]
[!code-vb[Formatting.DateAndTime.Standard#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#4)]

[表のトップへ](#table)

<a name="GeneralDateShortTime"></a>

## <a name="the-general-date-short-time-g-format-specifier"></a>一般の日付と短い形式の時刻 ("g") 書式指定子

"g" 標準書式指定子は、短い形式の日付 ("d") パターンと短い形式の時刻 ("t") パターンを空白で区切って組み合わせて表します。

結果文字列は、特定の <xref:System.Globalization.DateTimeFormatInfo> オブジェクトの書式情報に影響されます。 返される文字列の書式を制御できる <xref:System.Globalization.DateTimeFormatInfo> オブジェクト プロパティの一覧を次の表に示します。 一部のカルチャの <xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A?displayProperty=nameWithType> プロパティおよび <xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A?displayProperty=nameWithType> プロパティによって返されるカスタム書式指定子では、一部のプロパティが使用されない場合があります。

|プロパティ|説明|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A>|結果文字列の日付要素の書式を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A>|結果文字列の時刻要素の書式を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A>|日付の構成要素、つまり年、月、および日を区切る文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|時刻の構成要素、つまり時間、分、および秒を区切る文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|午前 0 時から正午前までの時刻を 12 時間形式で示す文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|正午から午前 0 時前までの時刻を 12 時間形式で示す文字列を定義します。|

次の例では、"g" 書式指定子を使用して、日付と時刻の値を表示します。

[!code-csharp[Formatting.DateAndTime.Standard#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#5)]
[!code-vb[Formatting.DateAndTime.Standard#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#5)]

[表のトップへ](#table)

<a name="GeneralDateLongTime"></a>

## <a name="the-general-date-long-time-g-format-specifier"></a>一般の日付と長い形式の時刻 ("G") 書式指定子

"G" 標準書式指定子は、短い形式の日付 ("d") パターンと長い形式の時刻 ("T") パターンを空白で区切って組み合わせて表します。

結果文字列は、特定の <xref:System.Globalization.DateTimeFormatInfo> オブジェクトの書式情報に影響されます。 返される文字列の書式を制御できる <xref:System.Globalization.DateTimeFormatInfo> オブジェクト プロパティの一覧を次の表に示します。 一部のカルチャの <xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A?displayProperty=nameWithType> プロパティおよび <xref:System.Globalization.DateTimeFormatInfo.LongTimePattern%2A?displayProperty=nameWithType> プロパティによって返されるカスタム書式指定子では、一部のプロパティが使用されない場合があります。

|プロパティ|説明|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A>|結果文字列の日付要素の書式を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.LongTimePattern%2A>|結果文字列の時刻要素の書式を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A>|日付の構成要素、つまり年、月、および日を区切る文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|時刻の構成要素、つまり時間、分、および秒を区切る文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|午前 0 時から正午前までの時刻を 12 時間形式で示す文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|正午から午前 0 時前までの時刻を 12 時間形式で示す文字列を定義します。|

次の例では、"G" 書式指定子を使用して、日付と時刻の値を表示します。

[!code-csharp[Formatting.DateAndTime.Standard#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#6)]
[!code-vb[Formatting.DateAndTime.Standard#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#6)]

[表のトップへ](#table)

<a name="MonthDay"></a>

## <a name="the-month-m-m-format-specifier"></a>月 ("M"、"m") 書式指定子

"M" または "m" 標準書式指定子は、現在の <xref:System.Globalization.DateTimeFormatInfo.MonthDayPattern%2A?displayProperty=nameWithType> プロパティで定義されるカスタム日時書式指定文字列を表します。 たとえば、インバリアント カルチャのカスタム書式指定文字列は "MMMM dd" です。

返される文字列の書式を制御する <xref:System.Globalization.DateTimeFormatInfo> オブジェクト プロパティの一覧を次の表に示します。

|プロパティ|説明|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.MonthDayPattern%2A>|結果文字列の全体的な書式を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|結果文字列に含まれるローカライズされた月の名前を定義します。|

次の例では、"m" 書式指定子を使用して、日付と時刻の値を表示します。

[!code-csharp[Formatting.DateAndTime.Standard#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#7)]
[!code-vb[Formatting.DateAndTime.Standard#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#7)]

[表のトップへ](#table)

<a name="Roundtrip"></a>

## <a name="the-round-trip-o-o-format-specifier"></a>ラウンドトリップ ("O"、"o") 書式指定子

"O" または "o" 標準書式指定子は、タイム ゾーン情報を保持するパターンを使用するカスタム日時書式指定文字列を表し、ISO 8601 に準拠する結果文字列を生成します。 この書式指定子は、<xref:System.DateTime> 値の日付と時刻の値を、<xref:System.DateTime.Kind%2A?displayProperty=nameWithType> プロパティと共にテキストとして保持できるように設計されています。 <xref:System.DateTime.Parse%28System.String%2CSystem.IFormatProvider%2CSystem.Globalization.DateTimeStyles%29?displayProperty=nameWithType> パラメーターが <xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType> に設定されている場合は、`styles` メソッドまたは <xref:System.Globalization.DateTimeStyles.RoundtripKind?displayProperty=nameWithType> メソッドを使用して、書式設定された文字列を変換前の文字列に戻すことができます。

"O" または "o" 標準書式指定子は、<xref:System.DateTime> 値の "yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'fffffffK" カスタム書式指定文字列と <xref:System.DateTimeOffset> 値の "yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'fffffffzzz" カスタム書式指定文字列に対応します。 この文字列の中で、個々の文字 (ハイフン、コロン、アルファベットの "T" など) を区切る一対の単一引用符は、各文字がリテラルであって変更できないことを示します。 アポストロフィは、出力された文字列には現れません。

"O" または "o" 標準書式指定子 (および "yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'fffffffK" カスタム書式指定文字列) は、<xref:System.DateTime.Kind%2A> 値の <xref:System.DateTime> プロパティを保持するため、ISO 8601 の 3 種類のタイム ゾーン情報表記形式を利用します。

- <xref:System.DateTimeKind.Local?displayProperty=nameWithType> 日時値のタイム ゾーン コンポーネントは、UTC からのオフセットです (例:+01:00、-07:00)。 <xref:System.DateTimeOffset> のすべての値もこの形式で表記されます。

- <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> 日時値のタイム ゾーン コンポーネントは、"Z" (ゼロ オフセットを示す) を使用して UTC を表記します。

- <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType> 日時値にはタイム ゾーン情報はありません。

"O" または "o" 標準書式指定子は国際基準に準拠しているため、この指定子を使用する書式設定操作または解析操作では常に、インバリアント カルチャとグレゴリオ暦が使用されます。

`Parse` と `TryParse` の `ParseExact`、`TryParseExact`、<xref:System.DateTime>、および <xref:System.DateTimeOffset> メソッドに渡される文字列が、これらの書式で表記される場合には、書式指定子 "O" または "o" を使用して解析できます。 <xref:System.DateTime> オブジェクトの場合、呼び出す解析オーバーロードの `styles` パラメーターに値 <xref:System.Globalization.DateTimeStyles.RoundtripKind?displayProperty=nameWithType> が指定されている必要があります。 "O" または "o" 書式指定子に対応するカスタム書式文字列を使用して解析メソッドを呼び出した結果は、"O" または "o" を使用した場合の結果と同じではないことに注意してください。 これは、カスタム書式文字列を使用する解析メソッドでは、タイム ゾーン コンポーネントがない日時値または "Z" を使用して UTC を示している日時値の文字列形式を解析できないためです。

次の例では、米国のシステム上で "o" 書式指定子を使用して、一連の <xref:System.DateTime> 値および <xref:System.DateTimeOffset> 値を表示しています。表示します。

[!code-csharp[Formatting.DateAndTime.Standard#8](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/o1.cs#8)]
[!code-vb[Formatting.DateAndTime.Standard#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/o1.vb#8)]

次の例では、"o" 書式指定子を使用して書式設定された文字列を作成し、日付および時刻の `Parse` メソッドを呼び出して元の日時値を復元します。

[!code-csharp[Formatting.DateandTime.Standard#16](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Roundtrip1.cs#16)]
[!code-vb[Formatting.DateandTime.Standard#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/RoundTrip1.vb#16)]

[表のトップへ](#table)

<a name="RFC1123"></a>

## <a name="the-rfc1123-r-r-format-specifier"></a>RFC1123 ("R"、"r") 書式指定子

"R" または "r" 標準書式指定子は、<xref:System.Globalization.DateTimeFormatInfo.RFC1123Pattern%2A?displayProperty=nameWithType> プロパティで定義されるカスタム日時書式指定文字列を表します。 このパターンは定義済みの標準を反映し、プロパティは読み取り専用です。 したがって、使用されるカルチャまたは指定された書式プロバイダーに関係なく、常に同じです。 カスタム書式指定文字列は、"ddd, dd MMM yyyy HH':'mm':'ss 'GMT'" です。 この標準書式指定子を使用した場合、書式設定操作または解析操作で常にインバリアント カルチャが使用されます。

結果文字列は、インバリアント カルチャを表す <xref:System.Globalization.DateTimeFormatInfo> プロパティによって返される <xref:System.Globalization.DateTimeFormatInfo.InvariantInfo%2A?displayProperty=nameWithType> オブジェクトの次のプロパティに影響されます。

|プロパティ|説明|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.RFC1123Pattern%2A>|結果文字列の書式を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.AbbreviatedDayNames%2A>|結果文字列に含まれる日付の省略名を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.AbbreviatedMonthNames%2A>|結果文字列に含まれる月の省略名を定義します。|

RFC 1123 標準では、時刻は世界協定時刻 (UTC: Coordinated Universal Time) で表されますが、書式設定操作では、書式設定される <xref:System.DateTime> オブジェクトの値は変更されません。 そのため、書式設定操作を行う前に <xref:System.DateTime> メソッドを呼び出して、<xref:System.DateTime.ToUniversalTime%2A?displayProperty=nameWithType> 値を UTC に変換する必要があります。 一方、<xref:System.DateTimeOffset> 値では、この変換が自動的に行われます。書式設定操作の前に <xref:System.DateTimeOffset.ToUniversalTime%2A?displayProperty=nameWithType> メソッドを呼び出す必要はありません。

次の例では、米国のシステム上で "r" 書式指定子を使用して、<xref:System.DateTime> 値および <xref:System.DateTimeOffset> 値を表示しています。表示します。

[!code-csharp-interactive[Formatting.DateAndTime.Standard#9](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#9)]
[!code-vb[Formatting.DateAndTime.Standard#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#9)]

[表のトップへ](#table)

<a name="Sortable"></a>

## <a name="the-sortable-s-format-specifier"></a>並べ替え可能な日付と時刻 ("s") 書式指定子

"s" 標準書式指定子は、<xref:System.Globalization.DateTimeFormatInfo.SortableDateTimePattern%2A?displayProperty=nameWithType> プロパティで定義されるカスタム日時書式指定文字列を表します。 このパターンは定義済みの標準 (ISO 8601) を反映し、プロパティは読み取り専用です。 したがって、使用されるカルチャまたは指定された書式プロバイダーに関係なく、常に同じです。 カスタム書式指定文字列は、"yyyy'-'MM'-'dd'T'HH':'mm':'ss" です。

"s" 書式指定子の目的は、日付と時刻の値に基づいて、一貫して昇順または降順で並べ替える結果の文字列を作成することです。 そのため、"s" 標準書式指定子は一貫性のある形式での日付と時刻の値を表していますが、書式設定操作によって、書式設定の対象となる日付と時刻のオブジェクトの値が <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> プロパティやその <xref:System.DateTimeOffset.Offset%2A?displayProperty=nameWithType> 値を反映するように変更されることはありません。 たとえば、日付と時刻値の 2014-11-15T18:32:17+00:00 と 2014-11-15T18:32:17+08:00 を書式設定することで生成される結果文字列は同じになります。

この標準書式指定子を使用した場合、書式設定操作または解析操作で常にインバリアント カルチャが使用されます。

次の例では、米国のシステム上で "s" 書式指定子を使用して、<xref:System.DateTime> 値および <xref:System.DateTimeOffset> 値を表示しています。表示します。

[!code-csharp-interactive[Formatting.DateAndTime.Standard#10](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#10)]
[!code-vb[Formatting.DateAndTime.Standard#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#10)]

[表のトップへ](#table)

<a name="ShortTime"></a>

## <a name="the-short-time-t-format-specifier"></a>短い形式の時刻 ("t") 書式指定子

"t" 標準書式指定子は、現在の <xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A?displayProperty=nameWithType> プロパティで定義されるカスタム日時書式指定文字列を表します。 たとえば、インバリアント カルチャのカスタム書式指定文字列は "HH:mm" です。

結果文字列は、特定の <xref:System.Globalization.DateTimeFormatInfo> オブジェクトの書式情報に影響されます。 返される文字列の書式を制御できる <xref:System.Globalization.DateTimeFormatInfo> オブジェクト プロパティの一覧を次の表に示します。 一部のカルチャの <xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A?displayProperty=nameWithType> プロパティによって返されるカスタム書式指定子では、一部のプロパティが使用されない場合があります。

|プロパティ|説明|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A>|結果文字列の時刻要素の書式を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|時刻の構成要素、つまり時間、分、および秒を区切る文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|午前 0 時から正午前までの時刻を 12 時間形式で示す文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|正午から午前 0 時前までの時刻を 12 時間形式で示す文字列を定義します。|

次の例では、"t" 書式指定子を使用して、日付と時刻の値を表示します。

[!code-csharp[Formatting.DateAndTime.Standard#11](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#11)]
[!code-vb[Formatting.DateAndTime.Standard#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#11)]

[表のトップへ](#table)

<a name="LongTime"></a>

## <a name="the-long-time-t-format-specifier"></a>長い形式の時刻 ("T") 書式指定子

"T" 標準書式指定子は、特定のカルチャの <xref:System.Globalization.DateTimeFormatInfo.LongTimePattern%2A?displayProperty=nameWithType> プロパティで定義されるカスタム日時書式指定文字列を表します。 たとえば、インバリアント カルチャのカスタム書式指定文字列は "HH:mm:ss" です。

返される文字列の書式を制御できる <xref:System.Globalization.DateTimeFormatInfo> オブジェクト プロパティの一覧を次の表に示します。 一部のカルチャの <xref:System.Globalization.DateTimeFormatInfo.LongTimePattern%2A?displayProperty=nameWithType> プロパティによって返されるカスタム書式指定子では、一部のプロパティが使用されない場合があります。

|プロパティ|説明|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.LongTimePattern%2A>|結果文字列の時刻要素の書式を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|時刻の構成要素、つまり時間、分、および秒を区切る文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|午前 0 時から正午前までの時刻を 12 時間形式で示す文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|正午から午前 0 時前までの時刻を 12 時間形式で示す文字列を定義します。|

次の例では、"T" 書式指定子を使用して、日付と時刻の値を表示します。

[!code-csharp[Formatting.DateAndTime.Standard#12](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#12)]
[!code-vb[Formatting.DateAndTime.Standard#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#12)]

[表のトップへ](#table)

<a name="UniversalSortable"></a>

## <a name="the-universal-sortable-u-format-specifier"></a>世界共通の並べ替え可能な日付と時刻 ("u") 書式指定子

"u" 標準書式指定子は、<xref:System.Globalization.DateTimeFormatInfo.UniversalSortableDateTimePattern%2A?displayProperty=nameWithType> プロパティで定義されるカスタム日時書式指定文字列を表します。 このパターンは定義済みの標準を反映し、プロパティは読み取り専用です。 したがって、使用されるカルチャまたは指定された書式プロバイダーに関係なく、常に同じです。 カスタム書式指定文字列は、"yyyy'-'MM'-'dd HH':'mm':'ss'Z'" です。 この標準書式指定子を使用した場合、書式設定操作または解析操作で常にインバリアント カルチャが使用されます。

結果文字列では、時刻は世界協定時刻 (UTC) で表される必要がありますが、書式設定操作では、元の <xref:System.DateTime> 値の変換は実行されません。 そのため、書式設定する前に <xref:System.DateTime> メソッドを呼び出して、<xref:System.DateTime.ToUniversalTime%2A?displayProperty=nameWithType> 値を UTC に変換する必要があります。  一方、<xref:System.DateTimeOffset> 値では、この変換が自動的に行われます。書式設定操作の前に <xref:System.DateTimeOffset.ToUniversalTime%2A?displayProperty=nameWithType> メソッドを呼び出す必要はありません。

次の例では、"u" 書式指定子を使用して、日付と時刻の値を表示します。

[!code-csharp-interactive[Formatting.DateAndTime.Standard#13](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#13)]
[!code-vb[Formatting.DateAndTime.Standard#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#13)]

[表のトップへ](#table)

<a name="UniversalFull"></a>

## <a name="the-universal-full-u-format-specifier"></a>世界共通の完全な日付と時刻 ("U") 書式指定子

"U" 標準書式指定子は、特定のカルチャの <xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A?displayProperty=nameWithType> プロパティで定義されるカスタム日時書式指定文字列を表します。 このパターンは、"F" パターンと同じです。 ただし、<xref:System.DateTime> 値は、書式設定される前に、自動的に UTC に変換されます。

返される文字列の書式を制御できる <xref:System.Globalization.DateTimeFormatInfo> オブジェクト プロパティの一覧を次の表に示します。 一部のカルチャの <xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A> プロパティによって返されるカスタム書式指定子では、一部のプロパティが使用されない場合があります。

|プロパティ|説明|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A>|結果文字列の全体的な書式を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.DayNames%2A>|結果文字列に含まれるローカライズされた日付の名前を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|結果文字列に含まれるローカライズされた月の名前を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|時刻の構成要素、つまり時間、分、および秒を区切る文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|午前 0 時から正午前までの時刻を 12 時間形式で示す文字列を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|正午から午前 0 時前までの時刻を 12 時間形式で示す文字列を定義します。|

"U" 書式指定子は、<xref:System.DateTimeOffset> 型ではサポートされません。この書式指定子を使って <xref:System.FormatException> 値の書式を設定しようとすると、<xref:System.DateTimeOffset> がスローされます。

次の例では、"U" 書式指定子を使用して、日付と時刻の値を表示します。

[!code-csharp[Formatting.DateAndTime.Standard#14](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#14)]
[!code-vb[Formatting.DateAndTime.Standard#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#14)]

[表のトップへ](#table)

<a name="YearMonth"></a>

## <a name="the-year-month-y-y-format-specifier"></a>年月 ("Y"、"y") 書式指定子

"Y" または "y" 標準書式指定子は、特定のカルチャの <xref:System.Globalization.DateTimeFormatInfo.YearMonthPattern%2A?displayProperty=nameWithType> プロパティで定義されるカスタム日時書式指定文字列を表します。 たとえば、インバリアント カルチャのカスタム書式指定文字列は "yyyy MMMM" です。

返される文字列の書式を制御する <xref:System.Globalization.DateTimeFormatInfo> オブジェクト プロパティの一覧を次の表に示します。

|プロパティ|説明|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.YearMonthPattern%2A>|結果文字列の全体的な書式を定義します。|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|結果文字列に含まれるローカライズされた月の名前を定義します。|

次の例では、"y" 書式指定子を使用して、日付と時刻の値を表示します。

[!code-csharp[Formatting.DateAndTime.Standard#15](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#15)]
[!code-vb[Formatting.DateAndTime.Standard#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#15)]

[表のトップへ](#table)

<a name="Notes"></a>

## <a name="notes"></a>メモ

### <a name="control-panel-settings"></a>コントロール パネルの設定

コントロール パネルの **[地域と言語のオプション]** での設定は、書式設定操作によって生成される結果の文字列に影響します。 これらの設定は、書式設定の制御に使用される値を提供する現在のスレッド カルチャに関連付けられた <xref:System.Globalization.DateTimeFormatInfo> オブジェクトを初期化するために使用されます。 コンピューターで使用する設定が異なる場合は、生成される文字列も異なります。

また、 <xref:System.Globalization.CultureInfo.%23ctor%28System.String%29?displayProperty=nameWithType> コンストラクターを使用して、現在のシステム カルチャと同じカルチャを表す新しい <xref:System.Globalization.CultureInfo> オブジェクトをインスタンス化した場合、コントロール パネルの **[地域と言語のオプション]** 項目で設定されたカスタマイズが新しい <xref:System.Globalization.CultureInfo> オブジェクトに適用されます。 <xref:System.Globalization.CultureInfo.%23ctor%28System.String%2CSystem.Boolean%29?displayProperty=nameWithType> コンストラクターを使用すると、システムに対するカスタマイズが反映されない <xref:System.Globalization.CultureInfo> オブジェクトを作成できます。

### <a name="datetimeformatinfo-properties"></a>DateTimeFormatInfo のプロパティ

書式設定は、現在の <xref:System.Globalization.DateTimeFormatInfo> オブジェクトのプロパティの影響を受けます。このオブジェクトは、現在のスレッド カルチャによって暗黙的に指定されるか、または書式設定を実行するメソッドの <xref:System.IFormatProvider> パラメーターによって明示的に指定されます。 <xref:System.IFormatProvider> パラメーターには、カルチャを表す <xref:System.Globalization.CultureInfo> オブジェクトを指定するか、特定のカルチャの日時書式設定規則を表す <xref:System.Globalization.DateTimeFormatInfo> オブジェクトを指定する必要があります。 標準日時書式指定子の多くは、現在の <xref:System.Globalization.DateTimeFormatInfo> オブジェクトのプロパティによって定義されている書式設定パターンのエイリアスです。 標準日時書式指定子によって生成される結果は、対応する <xref:System.Globalization.DateTimeFormatInfo> プロパティの、対応する日時形式パターンを変更することによって変えることができます。

## <a name="see-also"></a>関連項目

- <xref:System.DateTime?displayProperty=nameWithType>
- <xref:System.DateTimeOffset?displayProperty=nameWithType>
- [型の書式設定](../../../docs/standard/base-types/formatting-types.md)
- [カスタム日時形式文字列](../../../docs/standard/base-types/custom-date-and-time-format-strings.md)
- [サンプル: .NET Core WinForms 書式設定ユーティリティ (C#)](https://docs.microsoft.com/samples/dotnet/samples/winforms-formatting-utility-cs)
- [サンプル: .NET Core WinForms 書式設定ユーティリティ (Visual Basic)](https://docs.microsoft.com/samples/dotnet/samples/winforms-formatting-utility-vb)
