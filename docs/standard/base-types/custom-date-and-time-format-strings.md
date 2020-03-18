---
title: カスタム日時形式文字列
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
ms.assetid: 98b374e3-0cc2-4c78-ab44-efb671d71984
ms.openlocfilehash: b33366922677b26f8fe99454206cacd5bb124f32
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78159274"
---
# <a name="custom-date-and-time-format-strings"></a>カスタム日時形式文字列

日時書式指定文字列は、<xref:System.DateTime> 値または <xref:System.DateTimeOffset> 値の書式設定操作によって生成されるテキスト表現を定義します。 また、文字列を日時に正常に変換するために解析操作で必要となる日時値の表現も定義します。 カスタム書式指定文字列は、1 つ以上のカスタム日時書式指定子で構成されます。 [標準の日時書式指定文字列](../../../docs/standard/base-types/standard-date-and-time-format-strings.md)以外の文字列は、すべてカスタム日時書式指定文字列として解釈されます。

> [!TIP]
> **書式指定ユーティリティ**である .NET Core Windows Forms をダウンロードできます。このアプリケーションを使用すると、書式指定文字列を数値または日付と時刻の値に適用して、結果の文字列を表示できます。 ソース コードは [C#](https://docs.microsoft.com/samples/dotnet/samples/winforms-formatting-utility-cs) と [Visual Basic](https://docs.microsoft.com/samples/dotnet/samples/winforms-formatting-utility-vb) で利用できます。

カスタム日時書式指定文字列は、<xref:System.DateTime> 値で使用することも、<xref:System.DateTimeOffset> 値で使用することもできます。

[!INCLUDE[C# interactive-note](~/includes/csharp-interactive-with-utc-partial-note.md)]

<a name="table"></a> 書式設定操作では、日時インスタンスの `ToString` メソッドまたは複合書式指定をサポートするメソッドで、カスタム日時書式指定文字列を使用できます。 両方の使用例を次に示します。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#17](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/custandformatting1.cs#17)]
[!code-vb[Formatting.DateAndTime.Custom#17](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/custandformatting1.vb#17)]

解析操作では、<xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType>、<xref:System.DateTime.TryParseExact%2A?displayProperty=nameWithType>、<xref:System.DateTimeOffset.ParseExact%2A?displayProperty=nameWithType>、および <xref:System.DateTimeOffset.TryParseExact%2A?displayProperty=nameWithType> の各メソッドで、カスタム日時書式指定文字列を使用できます。 これらのメソッドでは、解析操作が成功するための特定のパターンに入力文字列が完全に一致している必要があります。 日付に日、月、2 桁の年が含まれているかどうかを解析する <xref:System.DateTimeOffset.ParseExact%28System.String%2CSystem.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> メソッドを呼び出す例を次に示します。

[!code-csharp[Formatting.DateAndTime.Custom#18](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/custandparsing1.cs#18)]
[!code-vb[Formatting.DateAndTime.Custom#18](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/custandparsing1.vb#18)]

次の表に、カスタム日時書式指定子の説明および書式指定子ごとの書式設定後の文字列を示します。 既定では、結果の文字列は、en-US カルチャの書式指定規則を反映します。 特定の書式指定子でローカライズされた文字列が書式設定後に生成される場合、例には書式設定後の文字列が適用されるカルチャも示されます。 カスタム日時書式指定文字列の使用方法については、「[メモ](#notes)」セクションを参照してください。

| 書式指定子 | [説明] | 例 |
| ---------------------- | ----------------- | -------------- |
|"d"|月の日にち (1 ～ 31)。<br /><br /> 詳細については、「["d" カスタム書式指定子](#dSpecifier)」を参照してください。|2009-06-01T13:45:30 -> 1<br /><br /> 2009-06-15T13:45:30 -> 15|
|"dd"|月の日にち (01 ～ 31)。<br /><br /> 詳細については、「["dd" カスタム書式指定子](#ddSpecifier)」を参照してください。|2009-06-01T13:45:30 -> 01<br /><br /> 2009-06-15T13:45:30 -> 15|
|"ddd"|曜日の省略名。<br /><br /> 詳細については、「["ddd" カスタム書式指定子](#dddSpecifier)」を参照してください。|2009-06-15T13:45:30 -> Mon (en-US)<br /><br /> 2009-06-15T13:45:30 -> Пн (ru-RU)<br /><br /> 2009-06-15T13:45:30 -> lun. (fr-FR)|
|"dddd"|曜日の完全名。<br /><br /> 詳細については、「["dddd" カスタム書式指定子](#ddddSpecifier)」を参照してください。|2009-06-15T13:45:30 -> Monday (en-US)<br /><br /> 2009-06-15T13:45:30 -> понедельник (ru-RU)<br /><br /> 2009-06-15T13:45:30 -> lundi (fr-FR)|
|"f"|日時値の秒部分の 1/10。<br /><br /> 詳細については、「["f" カスタム書式指定子](#fSpecifier)」を参照してください。|2009-06-15T13:45:30.6170000 -> 6<br /><br /> 2009-06-15T13:45:30.05 -> 0|
|"ff"|日時値の秒部分の 1/100。<br /><br /> 詳細については、「["ff" カスタム書式指定子](#ffSpecifier)」を参照してください。|2009-06-15T13:45:30.6170000 -> 61<br /><br /> 2009-06-15T13:45:30.0050000 -> 00|
|"fff"|日時値の秒部分の 1/1000。<br /><br /> 詳細については、「["fff" カスタム書式指定子](#fffSpecifier)」を参照してください。|6/15/2009 13:45:30.617 -> 617<br /><br /> 6/15/2009 13:45:30.0005 -> 000|
|"ffff"|日時値の秒部分の 1/10000。<br /><br /> 詳細については、「["ffff" カスタム書式指定子](#ffffSpecifier)」を参照してください。|2009-06-15T13:45:30.6175000 -> 6175<br /><br /> 2009-06-15T13:45:30.0000500  -> 0000|
|"fffff"|日時値の秒部分の 1/100000。<br /><br /> 詳細については、「["fffff" カスタム書式指定子](#fffffSpecifier)」を参照してください。|2009-06-15T13:45:30.6175400 -> 61754<br /><br /> 6/15/2009 13:45:30.000005 -> 00000|
|"ffffff"|日時値の秒部分の 1/1000000。<br /><br /> 詳細については、「["ffffff" カスタム書式指定子](#ffffffSpecifier)」を参照してください。|2009-06-15T13:45:30.6175420 -> 617542<br /><br /> 2009-06-15T13:45:30.0000005 -> 000000|
|"fffffff"|日時値の秒部分の 1/10000000。<br /><br /> 詳細については、「["fffffff" カスタム書式指定子](#fffffffSpecifier)」を参照してください。|2009-06-15T13:45:30.6175425 -> 6175425<br /><br /> 2009-06-15T13:45:30.0001150 -> 0001150|
|"F"|日時値の秒部分の 1/10 (0 以外の場合)。<br /><br /> 詳細については、「["F" カスタム書式指定子](#F_Specifier)」を参照してください。|2009-06-15T13:45:30.6170000 -> 6<br /><br /> 2009-06-15T13:45:30.0500000 -> (no output)|
|"FF"|日時値の秒部分の 1/100 (0 以外の場合)。<br /><br /> 詳細については、「["FF" カスタム書式指定子](#FF_Specifier)」を参照してください。|2009-06-15T13:45:30.6170000 -> 61<br /><br /> 2009-06-15T13:45:30.0050000 -> (no output)|
|"FFF"|日時値の秒部分の 1/1000 (0 以外の場合)。<br /><br /> 詳細については、「["FFF" カスタム書式指定子](#FFF_Specifier)」を参照してください。|2009-06-15T13:45:30.6170000 -> 617<br /><br /> 2009-06-15T13:45:30.0005000 -> (no output)|
|"FFFF"|日時値の秒部分の 1/10000 (0 以外の場合)。<br /><br /> 詳細については、「["FFFF" カスタム書式指定子](#FFFF_Specifier)」を参照してください。|2009-06-15T13:45:30.5275000 -> 5275<br /><br /> 2009-06-15T13:45:30.0000500 -> (no output)|
|"FFFFF"|日時値の秒部分の 1/100000 (0 以外の場合)。<br /><br /> 詳細については、「["FFFFF" カスタム書式指定子](#FFFFF_Specifier)」を参照してください。|2009-06-15T13:45:30.6175400 -> 61754<br /><br /> 2009-06-15T13:45:30.0000050 -> (no output)|
|"FFFFFF"|日時値の秒部分の 1/1000000 (0 以外の場合)。<br /><br /> 詳細については、「["FFFFFF" カスタム書式指定子](#FFFFFF_Specifier)」を参照してください。|2009-06-15T13:45:30.6175420 -> 617542<br /><br /> 2009-06-15T13:45:30.0000005 -> (no output)|
|"FFFFFFF"|日時値の秒部分の 1/10000000 (0 以外の場合)。<br /><br /> 詳細については、「["FFFFFFF" カスタム書式指定子](#FFFFFFF_Specifier)」を参照してください。|2009-06-15T13:45:30.6175425 -> 6175425<br /><br /> 2009-06-15T13:45:30.0001150 -> 000115|
|"g"、"gg"|時期または時代 (年号)。<br /><br /> 詳細については、「["g" または "gg" カスタム書式指定子](#gSpecifier)」を参照してください。|2009-06-15T13:45:30.6170000 -> A.D.|
|"h"|12 時間形式の時間 (1 ～ 12)。<br /><br /> 詳細については、「["h" カスタム書式指定子](#hSpecifier)」を参照してください。|2009-06-15T01:45:30 -> 1<br /><br /> 2009-06-15T13:45:30 -> 1|
|"hh"|12 時間形式の時間 (01 ～ 12)。<br /><br /> 詳細については、「["hh" カスタム書式指定子](#hhSpecifier)」を参照してください。|2009-06-15T01:45:30 -> 01<br /><br /> 2009-06-15T13:45:30 -> 01|
|"H"|24 時間形式の時間 (0 ～ 23)。<br /><br /> 詳細については、「["H" カスタム書式指定子](#H_Specifier)」を参照してください。|2009-06-15T01:45:30 -> 1<br /><br /> 2009-06-15T13:45:30 -> 13|
|"HH"|24 時間形式の時間 (00 ～ 23)。<br /><br /> 詳細については、「["HH" カスタム書式指定子](#HH_Specifier)」を参照してください。|2009-06-15T01:45:30 -> 01<br /><br /> 2009-06-15T13:45:30 -> 13|
|"K"|タイム ゾーン情報。<br /><br /> 詳細については、「["K" カスタム書式指定子](#KSpecifier)」を参照してください。|<xref:System.DateTime> 値の場合:<br /><br /> 2009-06-15T13:45:30, Kind Unspecified -><br /><br /> 2009-06-15T13:45:30, Kind Utc -> Z<br /><br /> 2009-06-15T13:45:30, Kind Local -> -07:00 (depends on local computer settings)<br /><br /> <xref:System.DateTimeOffset> 値の場合:<br /><br /> 2009-06-15T01:45:30-07:00 --> -07:00<br /><br /> 2009-06-15T08:45:30+00:00 --> +00:00|
|"m"|分 (0 ～ 59)。<br /><br /> 詳細については、「["m" カスタム書式指定子](#mSpecifier)」を参照してください。|2009-06-15T01:09:30 -> 9<br /><br /> 2009-06-15T13:29:30 -> 29|
|"mm"|分 (00 ～ 59)。<br /><br /> 詳細については、「["mm" カスタム書式指定子](#mmSpecifier)」を参照してください。|2009-06-15T01:09:30 -> 09<br /><br /> 2009-06-15T01:45:30 -> 45|
|"M"|月 (1 ～ 12)。<br /><br /> 詳細については、「["M" カスタム書式指定子](#M_Specifier)」を参照してください。|2009-06-15T13:45:30 -> 6|
|"MM"|月 (01 ～ 12)。<br /><br /> 詳細については、「["MM" カスタム書式指定子](#MM_Specifier)」を参照してください。|2009-06-15T13:45:30 -> 06|
|"MMM"|月の省略名。<br /><br /> 詳細については、「["MMM" カスタム書式指定子](#MMM_Specifier)」を参照してください。|2009-06-15T13:45:30 -> Jun (en-US)<br /><br /> 2009-06-15T13:45:30 -> juin (fr-FR)<br /><br /> 2009-06-15T13:45:30 -> Jun (zu-ZA)|
|"MMMM"|月の完全名。<br /><br /> 詳細については、「["MMMM" カスタム書式指定子](#MMMM_Specifier)」を参照してください。|2009-06-15T13:45:30 -> June (en-US)<br /><br /> 2009-06-15T13:45:30 -> juni (da-DK)<br /><br /> 2009-06-15T13:45:30 -> uJuni (zu-ZA)|
|"s"|秒 (0 ～ 59)。<br /><br /> 詳細については、「["s" カスタム書式指定子](#sSpecifier)」を参照してください。|2009-06-15T13:45:09 -> 9|
|"ss"|秒 (00 ～ 59)。<br /><br /> 詳細については、「["ss" カスタム書式指定子](#ssSpecifier)」を参照してください。|2009-06-15T13:45:09 -> 09|
|"t"|AM/PM 指定子の最初の文字。<br /><br /> 詳細については、「["t" カスタム書式指定子](#tSpecifier)」を参照してください。|2009-06-15T13:45:30 -> P (en-US)<br /><br /> 2009-06-15T13:45:30 -> 午 (ja-JP)<br /><br /> 2009-06-15T13:45:30 ->  (fr-FR)|
|"tt"|AM/PM 指定子。<br /><br /> 詳細については、「["tt" カスタム書式指定子](#ttSpecifier)」を参照してください。|2009-06-15T13:45:30 -> PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> 午後 (ja-JP)<br /><br /> 2009-06-15T13:45:30 ->  (fr-FR)|
|"y"|年 (0 ～ 99)。<br /><br /> 詳細については、「["y" カスタム書式指定子](#ySpecifier)」を参照してください。|0001-01-01T00:00:00 -> 1<br /><br /> 0900-01-01T00:00:00 -> 0<br /><br /> 1900-01-01T00:00:00 -> 0<br /><br /> 2009-06-15T13:45:30 -> 9<br /><br /> 2019-06-15T13:45:30 -> 19|
|"yy"|年 (00 ～ 99)。<br /><br /> 詳細については、「["yy" カスタム書式指定子](#yySpecifier)」を参照してください。|0001-01-01T00:00:00 -> 01<br /><br /> 0900-01-01T00:00:00 -> 00<br /><br /> 1900-01-01T00:00:00 -> 00<br /><br /> 2019-06-15T13:45:30 -> 19|
|"yyy"|年 (3 桁以上)。<br /><br /> 詳細については、「["yyy" カスタム書式指定子](#yyySpecifier)」を参照してください。|0001-01-01T00:00:00 -> 001<br /><br /> 0900-01-01T00:00:00 -> 900<br /><br /> 1900-01-01T00:00:00 -> 1900<br /><br /> 2009-06-15T13:45:30 -> 2009|
|"yyyy"|年 (4 桁の数値)。<br /><br /> 詳細については、「["yyyy" カスタム書式指定子](#yyyySpecifier)」を参照してください。|0001-01-01T00:00:00 -> 0001<br /><br /> 0900-01-01T00:00:00 -> 0900<br /><br /> 1900-01-01T00:00:00 -> 1900<br /><br /> 2009-06-15T13:45:30 -> 2009|
|"yyyyy"|年 (5 桁の数値)。<br /><br /> 詳細については、「["yyyyy" カスタム書式指定子](#yyyyySpecifier)」を参照してください。|0001-01-01T00:00:00 -> 00001<br /><br /> 2009-06-15T13:45:30 -> 02009|
|"z"|UTC を基準とする時間単位のオフセット (先行ゼロなし)。<br /><br /> 詳細については、「["z" カスタム書式指定子](#zSpecifier)」を参照してください。|2009-06-15T13:45:30-07:00 -> -7|
|"zz"|UTC を基準とする時間単位のオフセット (先行ゼロ付きの 1 桁の値)。<br /><br /> 詳細については、「["zz" カスタム書式指定子](#zzSpecifier)」を参照してください。|2009-06-15T13:45:30-07:00 -> -07|
|"zzz"|UTC を基準とする時間および分単位のオフセット。<br /><br /> 詳細については、「["zzz" カスタム書式指定子](#zzzSpecifier)」を参照してください。|2009-06-15T13:45:30-07:00 -> -07:00|
|":"|時刻の区切り記号。<br /><br /> 詳細については、「[":" カスタム書式指定子](#timeSeparator)」を参照してください。|2009-06-15T13:45:30 -> : (en-US)<br /><br /> 2009-06-15T13:45:30 -> . (it-IT)<br /><br /> 2009-06-15T13:45:30 -> : (ja-JP)|
|"/"|日付の区切り記号。<br /><br /> 詳細については、「["/" カスタム書式指定子](#dateSeparator)」を参照してください。|2009-06-15T13:45:30 -> / (en-US)<br /><br /> 2009-06-15T13:45:30 -> - (ar-DZ)<br /><br /> 2009-06-15T13:45:30 -> . (tr-TR)|
|"*文字列*"<br /><br /> '*文字列*'|リテラル文字列の区切り記号。<br /><br /> 詳細については、「[文字リテラル](#Literals)」を参照してください。|2009-06-15T13:45:30 ("arr:" h:m t) -> arr: 1:45 P<br /><br /> 2009-06-15T13:45:30 ('arr:' h:m t) -> arr: 1:45 P|
|%|後続の文字をカスタム書式指定子として定義します。<br /><br /> 詳細については、「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。|2009-06-15T13:45:30 (%h) -> 1|
|&#92;|エスケープ文字。<br /><br /> 詳細については、「[文字リテラル](#Literals)」および「[エスケープ文字の使用](#escape)」を参照してください。|2009-06-15T13:45:30 (h \h) -> 1 h|
|その他の文字|文字が結果の文字列にそのままコピーされます。<br /><br /> 詳細については、「[文字リテラル](#Literals)」を参照してください。|2009-06-15T01:45:30 (arr hh:mm t) -> arr 01:45 A|

以降では、それぞれのカスタム日時書式指定子について詳しく説明します。 特に明記されない限り、各指定子は、<xref:System.DateTime> 値で使用しても、<xref:System.DateTimeOffset> 値で使用してもまったく同じ文字列形式を生成します。

## <a name="dSpecifier"></a> "d" カスタム書式指定子

"d" カスタム書式指定子は、月の日にちを 1 ～ 31 の数値として表します。 1 桁の日にちは、先行ゼロなしで書式設定されます。

"d" 書式指定子が単独で使用され、その他のカスタム書式指定子がない場合、"d" は標準の日時書式指定子として解釈されます。 単一の書式指定子を使用する方法の詳細については、この記事で後述する「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。

次の例では、複数の書式指定文字列の中に "d" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#1](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#1)]
[!code-vb[Formatting.DateAndTime.Custom#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#1)]

[表のトップへ](#table)

## <a name="ddSpecifier"></a> "dd" カスタム書式指定子

"dd" カスタム書式指定文字列は、月の日にちを 01 ～ 31 の数値として表します。 1 桁の日にちは、先行ゼロ付きで書式設定されます。

次の例では、カスタム書式指定文字列の中に "dd" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#2](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#2)]
[!code-vb[Formatting.DateAndTime.Custom#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#2)]

[表のトップへ](#table)

## <a name="dddSpecifier"></a> "ddd" カスタム書式指定子

"ddd" カスタム書式指定子は、曜日の省略名を表します。 曜日のローカライズされた省略名は、現在のカルチャまたは特定のカルチャの <xref:System.Globalization.DateTimeFormatInfo.AbbreviatedDayNames%2A?displayProperty=nameWithType> プロパティから取得されます。

次の例では、カスタム書式指定文字列の中に "ddd" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#3](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#3)]
[!code-vb[Formatting.DateAndTime.Custom#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#3)]

[表のトップへ](#table)

## <a name="ddddSpecifier"></a> "dddd" カスタム書式指定子

"dddd" カスタム書式指定子 (任意の数の "d" 指定子を追加可能) は、曜日の完全名を表します。 曜日のローカライズされた名前は、現在のカルチャまたは特定のカルチャの <xref:System.Globalization.DateTimeFormatInfo.DayNames%2A?displayProperty=nameWithType> プロパティから取得されます。

次の例では、カスタム書式指定文字列の中に "dddd" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#4](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#4)]
[!code-vb[Formatting.DateAndTime.Custom#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#4)]

[表のトップへ](#table)

## <a name="fSpecifier"></a> "f" カスタム書式指定子

"f" カスタム書式指定子は、秒の端数の最上位桁 (つまり、日時値の秒部分の 1/10) を表します。

"f" 書式指定子が単独で使用され、その他の書式指定子がない場合、"f" は標準の日時書式指定子として解釈されます。 単一の書式指定子を使用する方法の詳細については、この記事で後述する「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。

<xref:System.DateTime.ParseExact%2A>、<xref:System.DateTime.TryParseExact%2A>、<xref:System.DateTimeOffset.ParseExact%2A>、<xref:System.DateTimeOffset.TryParseExact%2A> のいずれかのメソッドに渡す書式指定文字列の一部として "f" 書式指定子を使用する場合、"f" 書式指定子の数は、文字列を正しく解析するために、秒の端数の最上位桁数が何桁存在している必要があるかを表します。

次の例では、カスタム書式指定文字列の中に "f" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[表のトップへ](#table)

## <a name="ffSpecifier"></a> "ff" カスタム書式指定子

"ff" カスタム書式指定子は、秒の端数の最上位 2 桁 (つまり、日時値の秒部分の 1/100) を表します。

次の例では、カスタム書式指定文字列の中に "ff" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[表のトップへ](#table)

## <a name="fffSpecifier"></a> "fff" カスタム書式指定子

"fff" カスタム書式指定子は、秒の端数の最上位 3 桁 (つまり、日時値の秒部分の 1/1000) を表します。

次の例では、カスタム書式指定文字列の中に "fff" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[表のトップへ](#table)

## <a name="ffffSpecifier"></a> "ffff" カスタム書式指定子

"ffff" カスタム書式指定子は、秒の端数の最上位 4 桁 (つまり、日時値の秒部分の 1/10000) を表します。

時刻値の 1/10000 秒要素を表示することもできますが、このような値は意味を持たない場合があります。 日付および時刻の値の精度は、システム クロックの分解能に依存します。 Windows NT Version 3.5 (およびそれ以降) および Windows Vista オペレーティング システムでは、システム時計の解像力は約 10 ～ 15 ミリ秒です。

[表のトップへ](#table)

## <a name="fffffSpecifier"></a> "fffff" カスタム書式指定子

"fffff" カスタム書式指定子は、秒の端数の最上位 5 桁 (つまり、日時値の秒部分の 1/100000) を表します。

時刻値の 1/100000 秒要素を表示することもできますが、このような値は意味を持たない場合があります。 日付および時刻の値の精度は、システム クロックの分解能に依存します。 Windows NT 3.5 (およびそれ以降) および Windows Vista オペレーティング システムでは、システム時計の解像力は約 10 ～ 15 ミリ秒です。

[表のトップへ](#table)

## <a name="ffffffSpecifier"></a> "ffffff" カスタム書式指定子

"ffffff" カスタム書式指定子は、秒の端数の最上位 6 桁 (つまり、日時値の秒部分の 1/1000000) を表します。

時刻値の 1/1000000 秒要素を表示することもできますが、このような値は意味を持たない場合があります。 日付および時刻の値の精度は、システム クロックの分解能に依存します。 Windows NT 3.5 (およびそれ以降) および Windows Vista オペレーティング システムでは、システム時計の解像力は約 10 ～ 15 ミリ秒です。

[表のトップへ](#table)

## <a name="fffffffSpecifier"></a> "fffffff" カスタム書式指定子

"fffffff" カスタム書式指定子は、秒の端数の最上位 7 桁 (つまり、日時値の秒部分の 1/10000000) を表します。

時刻値の 1/10000000 秒要素を表示することもできますが、このような値は意味を持たない場合があります。 日付および時刻の値の精度は、システム クロックの分解能に依存します。 Windows NT 3.5 (およびそれ以降) および Windows Vista オペレーティング システムでは、システム時計の解像力は約 10 ～ 15 ミリ秒です。

[表のトップへ](#table)

## <a name="F_Specifier"></a> "F" カスタム書式指定子

"F" カスタム書式指定子は、秒の端数の最上位桁 (つまり、日時値の秒部分の 1/10) を表します。 その桁がゼロの場合には、何も表示されません。

"F" 書式指定子が単独で使用され、その他の書式指定子がない場合、"F" は標準の日時書式指定子として解釈されます。 単一の書式指定子を使用する方法の詳細については、この記事で後述する「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。

<xref:System.DateTime.ParseExact%2A>、<xref:System.DateTime.TryParseExact%2A>、<xref:System.DateTimeOffset.ParseExact%2A>、<xref:System.DateTimeOffset.TryParseExact%2A> のいずれかのメソッドの引数として使用された場合、"F" 書式指定子の数は、文字列を正しく解析するために、秒の端数の最上位桁数が最大何桁存在している必要があるかを表します。

次の例では、カスタム書式指定文字列の中に "F" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[表のトップへ](#table)

## <a name="FF_Specifier"></a> "FF" カスタム書式指定子"ff" カスタム書式指定子。

"FF" カスタム書式指定子は、秒の端数の最上位 2 桁 (つまり、日時値の秒部分の 1/100) を表します。 ただし、後続のゼロは表示されません。また、2 桁のゼロも表示されません。

次の例では、カスタム書式指定文字列の中に "FF" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[表のトップへ](#table)

## <a name="FFF_Specifier"></a> "FFF" カスタム書式指定子

"FFF" カスタム書式指定子は、秒の端数の最上位 3 桁 (つまり、日時値の秒部分の 1/1000) を表します。 ただし、後続のゼロは表示されません。また、3 桁のゼロも表示されません。

次の例では、カスタム書式指定文字列の中に "FFF" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[表のトップへ](#table)

## <a name="FFFF_Specifier"></a> "FFFF" カスタム書式指定子

"FFFF" カスタム書式指定子は、秒の端数の最上位 4 桁 (つまり、日時値の秒部分の 1/10000) を表します。 ただし、後続のゼロは表示されません。また、4 桁のゼロも表示されません。

時刻値の 1/10000 秒要素を表示することもできますが、このような値は意味を持たない場合があります。 日付および時刻の値の精度は、システム クロックの分解能に依存します。 Windows NT 3.5 (およびそれ以降) および Windows Vista オペレーティング システムでは、システム時計の解像力は約 10 ～ 15 ミリ秒です。

[表のトップへ](#table)

## <a name="FFFFF_Specifier"></a> "FFFFF" カスタム書式指定子

"FFFFF" カスタム書式指定子は、秒の端数の最上位 5 桁 (つまり、日時値の秒部分の 1/100000) を表します。 ただし、後続のゼロは表示されません。また、5 桁のゼロも表示されません。

時刻値の 1/100000 秒要素を表示することもできますが、このような値は意味を持たない場合があります。 日付および時刻の値の精度は、システム クロックの分解能に依存します。 Windows NT 3.5 (およびそれ以降) および Windows Vista オペレーティング システムでは、システム時計の解像力は約 10 ～ 15 ミリ秒です。

[表のトップへ](#table)

## <a name="FFFFFF_Specifier"></a> "FFFFFF" カスタム書式指定子

"FFFFFF" カスタム書式指定子は、秒の端数の最上位 6 桁 (つまり、日時値の秒部分の 1/1000000) を表します。 ただし、後続のゼロは表示されません。また、6 桁のゼロも表示されません。

時刻値の 1/1000000 秒要素を表示することもできますが、このような値は意味を持たない場合があります。 日付および時刻の値の精度は、システム クロックの分解能に依存します。 Windows NT 3.5 (およびそれ以降) および Windows Vista オペレーティング システムでは、システム時計の解像力は約 10 ～ 15 ミリ秒です。

[表のトップへ](#table)

## <a name="FFFFFFF_Specifier"></a> "FFFFFFF" カスタム書式指定子

"FFFFFFF" カスタム書式指定子は、秒の端数の最上位 7 桁 (つまり、日時値の秒部分の 1/10000000) を表します。 ただし、後続のゼロは表示されません。また、7 桁のゼロも表示されません。

時刻値の 1/10000000 秒要素を表示することもできますが、このような値は意味を持たない場合があります。 日付および時刻の値の精度は、システム クロックの分解能に依存します。 Windows NT 3.5 (およびそれ以降) および Windows Vista オペレーティング システムでは、システム時計の解像力は約 10 ～ 15 ミリ秒です。

[表のトップへ](#table)

## <a name="gSpecifier"></a> "g" または "gg" カスタム書式指定子

"g" または "gg" カスタム書式指定子 (任意の数の "g" 指定子を追加可能) は、A.D. などの時期または時代 (年号) を表します。 書式設定される日付に時期または時代 (年号) の文字列が関連付けられていない場合、この指定子は無視されます。

"g" 書式指定子が単独で使用され、その他のカスタム書式指定子がない場合、"g" は標準の日時書式指定子として解釈されます。 単一の書式指定子を使用する方法の詳細については、この記事で後述する「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。

次の例では、カスタム書式指定文字列の中に "g" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#6](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#6)]
[!code-vb[Formatting.DateAndTime.Custom#6](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#6)]

[表のトップへ](#table)

## <a name="hSpecifier"></a> "h" カスタム書式指定子

"h" カスタム書式指定子は、時間を 1 ～ 12 の数値として表します。つまり、午前 0 時と午後 0 時から時間をカウントする 12 時間制で時間が表されます。 午前 0 時からの特定の時間を、午後 0 時からの同じ時間と区別できません。 時間は丸められず、1 桁の時間は先行ゼロなしで書式設定されます。 たとえば、午前と午後の 5:43 という時間は、このカスタム書式指定子によって "5" と表示されます。

"h" 書式指定子が単独で使用され、その他のカスタム書式指定子がない場合、"h" は標準の日時書式指定子として解釈され、<xref:System.FormatException> をスローします。 単一の書式指定子を使用する方法の詳細については、この記事で後述する「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。

次の例では、カスタム書式指定文字列の中に "h" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#7](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#7)]
[!code-vb[Formatting.DateAndTime.Custom#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#7)]

[表のトップへ](#table)

## <a name="hhSpecifier"></a> "hh" カスタム書式指定子

"hh" カスタム書式指定子 (任意の数の "h" 指定子を追加可能) は、時間を 1 ～ 12 の数値として表します。つまり、午前 0 時と午後 0 時から時間をカウントする 12 時間制で時間が表されます。 午前 0 時からの特定の時間を、午後 0 時からの同じ時間と区別できません。 時間は丸められず、1 桁の時間は先行ゼロ付きで書式設定されます。 たとえば、午前と午後の 5:43 という時間は、この書式指定子によって "05" と表示されます。

次の例では、カスタム書式指定文字列の中に "hh" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#8](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#8)]
[!code-vb[Formatting.DateAndTime.Custom#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#8)]

[表のトップへ](#table)

## <a name="H_Specifier"></a> "H" カスタム書式指定子

"H" カスタム書式指定子は、時間を 0 ～ 23 の数値として表します。つまり、午前 0 時から時間をカウントする 24 時間制で時間が表されます。 1 桁の時間は、先行ゼロなしで書式設定されます。

"H" 書式指定子が単独で使用され、その他のカスタム書式指定子がない場合、"H" は標準の日時書式指定子として解釈され、<xref:System.FormatException> をスローします。 単一の書式指定子を使用する方法の詳細については、この記事で後述する「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。

次の例では、カスタム書式指定文字列の中に "H" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#9](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#9)]
[!code-vb[Formatting.DateAndTime.Custom#9](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#9)]

[表のトップへ](#table)

## <a name="HH_Specifier"></a> "HH" カスタム書式指定子

"HH" カスタム書式指定子 (任意の数の "H" 指定子を追加可能) は、時間を 00 ～ 23 の数値として表します。つまり、午前 0 時から時間をカウントする 24 時間制で時間が表されます。 1 桁の時間は、先行ゼロ付きで書式設定されます。

次の例では、カスタム書式指定文字列の中に "HH" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#10](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#10)]
[!code-vb[Formatting.DateAndTime.Custom#10](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#10)]

[表のトップへ](#table)

## <a name="KSpecifier"></a> "K" カスタム書式指定子

"K" カスタム書式指定子は、日付と時刻の値のタイム ゾーン情報を表します。 この書式指定子を <xref:System.DateTime> 値で使用した場合、書式設定後の文字列は、<xref:System.DateTime.Kind%2A?displayProperty=nameWithType> プロパティの値によって定義されます。

- ローカル タイム ゾーンの場合 (<xref:System.DateTime.Kind%2A?displayProperty=nameWithType> プロパティ値 = <xref:System.DateTimeKind.Local?displayProperty=nameWithType>)、この指定子は "zzz" 指定子に相当し、書式設定後の文字列には世界協定時刻 (UTC: Coordinated Universal Time) からのローカル オフセットが含まれます ("-07:00" など)。

- UTC 時刻の場合 (<xref:System.DateTime.Kind%2A?displayProperty=nameWithType> プロパティ値 = <xref:System.DateTimeKind.Utc?displayProperty=nameWithType>)、書式設定後の文字列には UTC 日付を表す "Z" 文字が含まれます。

- タイム ゾーンが指定されていない時刻の場合 (<xref:System.DateTime.Kind%2A?displayProperty=nameWithType> プロパティ = <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType>)、<xref:System.String.Empty?displayProperty=nameWithType> と同じ結果になります。

"K" 書式指定子を <xref:System.DateTimeOffset> 値で使用した場合、この指定子は "zzz" 書式指定子に相当し、書式設定後の文字列には <xref:System.DateTimeOffset> 値の UTC を基準としたオフセットが含まれます。

"K" 書式指定子が単独で使用され、その他のカスタム書式指定子がない場合、"K" は標準の日時書式指定子として解釈され、<xref:System.FormatException> をスローします。 単一の書式指定子を使用する方法の詳細については、この記事で後述する「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。

次の例では、米国太平洋標準時タイム ゾーンのシステムで、"K" カスタム書式指定子と各種の <xref:System.DateTime> 値および <xref:System.DateTimeOffset> 値を組み合わせた結果の文字列を表示します。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#12](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#12)]
[!code-vb[Formatting.DateAndTime.Custom#12](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#12)]

[表のトップへ](#table)

## <a name="mSpecifier"></a> "m" カスタム書式指定子

"m" カスタム書式指定子は、分を 0 ～ 59 の数値として表します。 この分は、直前の時間から経過した分数です。 1 桁の分は、先行ゼロなしで書式設定されます。

"m" 書式指定子が単独で使用され、その他のカスタム書式指定子がない場合、"m" は標準の日時書式指定子として解釈されます。 単一の書式指定子を使用する方法の詳細については、この記事で後述する「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。

次の例では、カスタム書式指定文字列の中に "m" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#7](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#7)]
[!code-vb[Formatting.DateAndTime.Custom#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#7)]

[表のトップへ](#table)

## <a name="mmSpecifier"></a> "mm" カスタム書式指定子

"mm" カスタム書式指定子 (任意の数の "m" 指定子を追加可能) は、分を 00 ～ 59 の数値として表します。 この分は、直前の時間から経過した分数です。 1 桁の分は、先行ゼロ付きで書式設定されます。

次の例では、カスタム書式指定文字列の中に "mm" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#8](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#8)]
[!code-vb[Formatting.DateAndTime.Custom#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#8)]

[表のトップへ](#table)

## <a name="M_Specifier"></a> "M" カスタム書式指定子

"M" カスタム書式指定子は、月を 1 ～ 12 (13 の月がある暦の場合は 1 ～ 13) の数値として表します。 1 桁の月は、先行ゼロなしで書式設定されます。

"M" 書式指定子が単独で使用され、その他のカスタム書式指定子がない場合、"M" は標準の日時書式指定子として解釈されます。 単一の書式指定子を使用する方法の詳細については、この記事で後述する「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。

次の例では、カスタム書式指定文字列の中に "M" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#11](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#11)]
[!code-vb[Formatting.DateAndTime.Custom#11](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#11)]

[表のトップへ](#table)

## <a name="MM_Specifier"></a> "MM" カスタム書式指定子

"MM" カスタム書式指定子は、月を 01 ～ 12 (13 の月がある暦の場合は 1 ～ 13) の数値として表します。 1 桁の月は、先行ゼロ付きで書式設定されます。

次の例では、カスタム書式指定文字列の中に "MM" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#2](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#2)]
[!code-vb[Formatting.DateAndTime.Custom#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#2)]

[表のトップへ](#table)

## <a name="MMM_Specifier"></a> "MMM" カスタム書式指定子

"MMM" カスタム書式指定子は、月の省略名を表します。 月のローカライズされた省略名は、現在のカルチャまたは特定のカルチャの <xref:System.Globalization.DateTimeFormatInfo.AbbreviatedMonthNames%2A?displayProperty=nameWithType> プロパティから取得されます。

次の例では、カスタム書式指定文字列の中に "MMM" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#3](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#3)]
[!code-vb[Formatting.DateAndTime.Custom#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#3)]

[表のトップへ](#table)

## <a name="MMMM_Specifier"></a> "MMMM" カスタム書式指定子

"MMMM" カスタム書式指定子は、月の完全名を表します。 月のローカライズされた名前は、現在のカルチャまたは特定のカルチャの <xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A?displayProperty=nameWithType> プロパティから取得されます。

次の例では、カスタム書式指定文字列の中に "MMMM" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#4](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#4)]
[!code-vb[Formatting.DateAndTime.Custom#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#4)]

[表のトップへ](#table)

## <a name="sSpecifier"></a> "s" カスタム書式指定子

"s" カスタム書式指定子は、秒を 0 ～ 59 の数値として表します。 この秒は、直前の分から経過した秒数です。 1 桁の秒は、先行ゼロなしで書式設定されます。

"s" 書式指定子が単独で使用され、その他のカスタム書式指定子がない場合、"s" は標準の日時書式指定子として解釈されます。 単一の書式指定子を使用する方法の詳細については、この記事で後述する「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。

次の例では、カスタム書式指定文字列の中に "s" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#7](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#7)]
[!code-vb[Formatting.DateAndTime.Custom#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#7)]

[表のトップへ](#table)

## <a name="ssSpecifier"></a> "ss" カスタム書式指定子

"ss" カスタム書式指定子 (任意の数の "s" 指定子を追加可能) は、秒を 00 ～ 59 の数値として表します。 この秒は、直前の分から経過した秒数です。 1 桁の秒は、先行ゼロ付きで書式設定されます。

次の例では、カスタム書式指定文字列の中に "ss" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#8](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#8)]
[!code-vb[Formatting.DateAndTime.Custom#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#8)]

[表のトップへ](#table)

## <a name="tSpecifier"></a> "t" カスタム書式指定子

"t" カスタム書式指定子は、AM/PM 指定子の最初の文字を表します。 ローカライズされた適切な指定子は、現在のカルチャまたは特定のカルチャの <xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A?displayProperty=nameWithType> プロパティまたは <xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A?displayProperty=nameWithType> プロパティから取得されます。 AM 指定子は、0:00:00 (午前 0 時) から 11:59:59.999 までのすべての時刻に使用されます。 PM 指定子は、12:00:00 (正午) から 23:59:59.999 までのすべての時刻に使用されます。

"t" 書式指定子が単独で使用され、その他のカスタム書式指定子がない場合、"t" は標準の日時書式指定子として解釈されます。 単一の書式指定子を使用する方法の詳細については、この記事で後述する「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。

次の例では、カスタム書式指定文字列の中に "t" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#7](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#7)]
[!code-vb[Formatting.DateAndTime.Custom#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#7)]

[表のトップへ](#table)

## <a name="ttSpecifier"></a> "tt" カスタム書式指定子

"tt" カスタム書式指定子 (任意の数の "t" 指定子を追加可能) は、AM/PM 指定子全体を表します。 ローカライズされた適切な指定子は、現在のカルチャまたは特定のカルチャの <xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A?displayProperty=nameWithType> プロパティまたは <xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A?displayProperty=nameWithType> プロパティから取得されます。 AM 指定子は、0:00:00 (午前 0 時) から 11:59:59.999 までのすべての時刻に使用されます。 PM 指定子は、12:00:00 (正午) から 23:59:59.999 までのすべての時刻に使用されます。

AM と PM を区別する必要のある言語の場合、必ず "tt" 指定子を使用する必要があります。 たとえば、日本語の場合、AM/PM 指定子の 2 番目の文字は異なりますが、先頭文字は同じです。

次の例では、カスタム書式指定文字列の中に "tt" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#8](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#8)]
[!code-vb[Formatting.DateAndTime.Custom#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#8)]

[表のトップへ](#table)

## <a name="ySpecifier"></a> "y" カスタム書式指定子

"y" カスタム書式指定子は、年を 1 桁または 2 桁の数値として表します。 年が 2 桁を超える場合は、下 2 桁のみが結果に表示されます。 2 桁の年の 1 桁目がゼロで始まる場合 (2008 など)、先行ゼロが省略されます。

"y" 書式指定子が単独で使用され、その他のカスタム書式指定子がない場合、"y" は標準の日時書式指定子として解釈されます。 単一の書式指定子を使用する方法の詳細については、この記事で後述する「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。

次の例では、カスタム書式指定文字列の中に "y" カスタム書式指定子が含まれます。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#13)]
[!code-vb[Formatting.DateAndTime.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#13)]

[表のトップへ](#table)

## <a name="yySpecifier"></a> "yy" カスタム書式指定子

"yy" カスタム書式指定子は、年を 2 桁の数値として表します。 年が 2 桁を超える場合は、下 2 桁のみが結果に表示されます。 年が 2 桁に満たない場合は、2 桁になるまで数値が先行ゼロで埋められます。

解析操作では、"yy" カスタム形式指定子を使用した 2 桁の年は、書式プロバイダーの現在のカレンダーの <xref:System.Globalization.Calendar.TwoDigitYearMax%2A?displayProperty=nameWithType> プロパティに基づいて解釈されます。 次の例は、このケースでは現在のカルチャである en-US カルチャの既定のグレゴリオ暦を使用して、2 桁の年を持つ日付の文字列形式を解析します。 次に、<xref:System.Globalization.CultureInfo> プロパティが変更された <xref:System.Globalization.GregorianCalendar> オブジェクトが使用されるように、現在のカルチャの <xref:System.Globalization.GregorianCalendar.TwoDigitYearMax%2A> オブジェクトを変更します。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#19](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/parseexact2digityear1.cs#19)]
[!code-vb[Formatting.DateAndTime.Custom#19](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/parseexact2digityear1.vb#19)]

次の例では、カスタム書式指定文字列の中に "yy" カスタム書式指定子が含まれます。

[!code-csharp[Formatting.DateAndTime.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#13)]
[!code-vb[Formatting.DateAndTime.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#13)]

[表のトップへ](#table)

## <a name="yyySpecifier"></a> "yyy" カスタム書式指定子

"yyy" カスタム書式指定子は、年を 3 桁以上で表します。 年が 3 桁を超える場合は、それらの数字も結果に含まれます。 年が 3 桁に満たない場合は、3 桁になるまで数値が先行ゼロで埋められます。

> [!NOTE]
> 年が 5 桁になることがあるタイ仏暦については、この書式指定子ですべての有効桁が表示されます。

次の例では、カスタム書式指定文字列の中に "yyy" カスタム書式指定子が含まれます。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#13)]
[!code-vb[Formatting.DateAndTime.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#13)]

[表のトップへ](#table)

## <a name="yyyySpecifier"></a> "yyyy" カスタム書式指定子

"yyyy" カスタム書式指定子は、年を 4 桁以上で表します。 年が 4 桁を超える場合は、それらの数字も結果に含まれます。 年が 4 桁に満たない場合は、4 桁になるまで数値が先行ゼロで埋められます。

> [!NOTE]
> 年が 5 桁になることがあるタイ仏暦については、この書式指定子で 4 桁以上が表示されます。

次の例では、カスタム書式指定文字列の中に "yyyy" カスタム書式指定子が含まれます。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#13)]
[!code-vb[Formatting.DateAndTime.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#13)]

[表のトップへ](#table)

## <a name="yyyyySpecifier"></a> "yyyyy" カスタム書式指定子

"yyyyy" カスタム書式指定子 (任意の数の "y" 指定子を追加可能) は、年を 5 桁以上で表します。 年が 5 桁を超える場合は、それらの数字も結果に含まれます。 年が 5 桁に満たない場合は、5 桁になるまで数値が先行ゼロで埋められます。

"y" 指定子を追加すると、"y" 指定子の数と同じ桁数になるまで数値が先行ゼロで埋められます。

次の例では、カスタム書式指定文字列の中に "yyyyy" カスタム書式指定子が含まれます。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#13)]
[!code-vb[Formatting.DateAndTime.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#13)]

[表のトップへ](#table)

## <a name="zSpecifier"></a> "z" カスタム書式指定子

<xref:System.DateTime> 値で使用した場合、"z" カスタム書式指定子は、オペレーティング システムのローカル タイム ゾーンの、世界協定時刻 (UTC) を基準とした符号付きオフセット (時間単位) を表します。 インスタンスの <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> プロパティの値は反映されません。 そのため、<xref:System.DateTime> 値に対して "z" 書式指定子を使用することはお勧めできません。

<xref:System.DateTimeOffset> 値で使用した場合、この書式指定子は、<xref:System.DateTimeOffset> 値の、UTC を基準とするオフセット (時間単位) を表します。

このオフセットは、常に先頭の符号と共に表示されます。 正符号 (+) は UTC より時間が進んでいることを示し、負符号 (-) は UTC より時間が遅れていることを示します。 1 桁のオフセットは、先行ゼロなしで書式設定されます。

"z" 書式指定子が単独で使用され、その他のカスタム書式指定子がない場合、"z" は標準の日時書式指定子として解釈され、<xref:System.FormatException> をスローします。 単一の書式指定子を使用する方法の詳細については、この記事で後述する「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。

次の例では、カスタム書式指定文字列の中に "z" カスタム書式指定子が含まれます。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#14](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#14)]
[!code-vb[Formatting.DateAndTime.Custom#14](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#14)]

[表のトップへ](#table)

## <a name="zzSpecifier"></a> "zz" カスタム書式指定子

<xref:System.DateTime> 値で使用した場合、"zz" カスタム書式指定子は、オペレーティング システムのローカル タイム ゾーンの、UTC を基準とした符号付きオフセット (時間単位) を表します。 インスタンスの <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> プロパティの値は反映されません。 そのため、<xref:System.DateTime> 値に対して "zz" 書式指定子を使用することはお勧めできません。

<xref:System.DateTimeOffset> 値で使用した場合、この書式指定子は、<xref:System.DateTimeOffset> 値の、UTC を基準とするオフセット (時間単位) を表します。

このオフセットは、常に先頭の符号と共に表示されます。 正符号 (+) は UTC より時間が進んでいることを示し、負符号 (-) は UTC より時間が遅れていることを示します。 1 桁のオフセットは、先行ゼロ付きで書式設定されます。

次の例では、カスタム書式指定文字列の中に "zz" カスタム書式指定子が含まれます。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#14](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#14)]
[!code-vb[Formatting.DateAndTime.Custom#14](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#14)]

[表のトップへ](#table)

## <a name="zzzSpecifier"></a> "zzz" カスタム書式指定子

<xref:System.DateTime> 値で使用した場合、"zzz" カスタム書式指定子は、オペレーティング システムのローカル タイム ゾーンの、UTC を基準とした符号付きオフセット (時間および分単位) を表します。 インスタンスの <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> プロパティの値は反映されません。 そのため、<xref:System.DateTime> 値に対して "zzz" 書式指定子を使用することはお勧めできません。

<xref:System.DateTimeOffset> 値で使用した場合、この書式指定子は、<xref:System.DateTimeOffset> 値の、UTC を基準とするオフセット (時間および分単位) を表します。

このオフセットは、常に先頭の符号と共に表示されます。 正符号 (+) は UTC より時間が進んでいることを示し、負符号 (-) は UTC より時間が遅れていることを示します。 1 桁のオフセットは、先行ゼロ付きで書式設定されます。

次の例では、カスタム書式指定文字列の中に "zzz" カスタム書式指定子が含まれます。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#14](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#14)]
[!code-vb[Formatting.DateAndTime.Custom#14](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#14)]

[表のトップへ](#table)

## <a name="timeSeparator"></a> ":" カスタム書式指定子
":" カスタム書式指定子は、時、分、および秒を区別するための時刻の区切り記号を表します。 ローカライズされた適切な時刻の区切り記号は、現在のカルチャまたは特定のカルチャの <xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A?displayProperty=nameWithType> プロパティから取得されます。

> [!NOTE]
> 特定の日付と時刻の文字列について時刻の区切り記号を変更するには、リテラル文字列の区切り記号内に区切り記号を指定します。 たとえば、カスタム書式指定文字列 `hh'_'dd'_'ss` は、常に時刻の区切り記号として "\_" (アンダースコア) が使用される結果文字列を生成します。 カルチャのすべての日付について時刻の区切り記号を変更するには、現在のカルチャの <xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A?displayProperty=nameWithType> プロパティ値を変更するか、<xref:System.Globalization.DateTimeFormatInfo> オブジェクトのインスタンスを作成し、その <xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A> プロパティに文字を割り当てて、<xref:System.IFormatProvider> パラメーターを含む書式設定メソッドのオーバーロードを呼び出します。

":" 書式指定子が単独で使用され、その他のカスタム書式指定子がない場合、":" は標準の日時書式指定子として解釈され、<xref:System.FormatException> をスローします。 単一の書式指定子を使用する方法の詳細については、この記事で後述する「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。

[表のトップへ](#table)

## <a name="dateSeparator"></a> "/" カスタム書式指定子

"/" カスタム書式指定子は、年、月、および日を区別するための日付の区切り記号を表します。 ローカライズされた適切な日付の区切り記号は、現在のカルチャまたは特定のカルチャの <xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A?displayProperty=nameWithType> プロパティから取得されます。

> [!NOTE]
> 特定の日付と時刻の文字列について日付の区切り記号を変更するには、リテラル文字列の区切り記号内に区切り記号を指定します。 たとえば、カスタム書式指定文字列 `mm'/'dd'/'yyyy` は、常に日付の区切り記号として "/" が使用される結果文字列を生成します。 カルチャのすべての日付について日付の区切り記号を変更するには、現在のカルチャの <xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A?displayProperty=nameWithType> プロパティ値を変更するか、<xref:System.Globalization.DateTimeFormatInfo> オブジェクトのインスタンスを作成し、その <xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A> プロパティに文字を割り当てて、<xref:System.IFormatProvider> パラメーターを含む書式設定メソッドのオーバーロードを呼び出します。

"/" 書式指定子が単独で使用され、その他のカスタム書式指定子がない場合、"/" は標準の日時書式指定子として解釈され、<xref:System.FormatException> をスローします。 単一の書式指定子を使用する方法の詳細については、この記事で後述する「[単一のカスタム書式指定子の使用](#UsingSingleSpecifiers)」を参照してください。

[表のトップへ](#table)

## <a name="Literals"></a> 文字リテラル

カスタム日時書式指定文字列の次の文字は予約済みで、常に書式設定文字として解釈されます。または "、'、/、および \\ の場合は、特殊文字として解釈されます。

||||||
|-|-|-|-|-|
|F|H|K|M|d|
|f|G|h|m|s|
|t|y|z|%|:|
|/|"|'|&#92;||

その他の文字はすべて、文字リテラルとして常に解釈され、書式設定操作では、変更されずに結果の文字列に含まれます。  解析操作では、これらは入力文字列の文字と完全に一致している必要があります。比較では大文字小文字を区別します。

次の例には、書式指定文字列でローカル タイム ゾーンを表すため、リテラル文字 "PST" (太平洋標準時) と "PDT" (太平洋夏時間)が含まれています。 文字列は結果の文字列に含まれ、ローカル タイム ゾーン文字列を含む文字列も正常に解析することに注意してください。

[!code-csharp[Formatting.DateAndTime.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/LiteralsEx1.cs#20)]
[!code-vb[Formatting.DateAndTime.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/LiteralsEx1.vb#20)]

文字が結果の文字列に含まれるように、または入力文字列で正常に解析されるように、文字が予約文字としてではなく、リテラル文字として解釈されることを示すには、次の 2 つの方法があります。

- 予約済みの各文字をエスケープします。 詳細については、「[エスケープ文字の使用](#escape)」を参照してください。

次の例には、書式指定文字列でローカル タイム ゾーンを表すため、リテラル文字 "pst"(太平洋標準時間) が含まれています。 "s" と "t" はどちらもカスタム書式指定文字列であるため、どちらの文字も文字リテラルとして解釈されるにはエスケープする必要があります。

[!code-csharp[Formatting.DateAndTime.Custom#21](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/LiteralsEx2.cs#21)]
[!code-vb[Formatting.DateAndTime.Custom#21](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/LiteralsEx2.vb#21)]

- リテラル文字列全体を引用符またはアポストロフィで囲みます。 次の例は、前の例と似ていますが、区切られた文字列全体が文字リテラルとして解釈されることを示すため、"pst" が引用符で囲まれている点が異なります。

[!code-csharp[Formatting.DateAndTime.Custom#22](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/LiteralsEx3.cs#22)]
[!code-vb[Formatting.DateAndTime.Custom#22](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/LiteralsEx3.vb#22)]

## <a name="notes"></a>メモ

### <a name="UsingSingleSpecifiers"></a> 単一のカスタム書式指定子の使用

カスタム日時書式指定文字列は、複数の文字で構成されます。 日時書式指定メソッドでは、1 文字の文字列が標準の日時書式指定文字列として解釈されます。 文字が有効な書式指定子として認識されない場合は <xref:System.FormatException> がスローされます。 たとえば、"h" 指定子のみで構成される書式指定文字列は、標準の日時書式指定文字列として解釈されます。 ただし、この場合では、"h" という標準の日時書式指定子が存在しないため、例外がスローされます。

カスタム日時書式指定子のいずれかを書式指定文字列内の唯一の指定子として使用するには (つまり、"d"、"f"、"F"、"g"、"h"、"H"、"K"、"m"、"M"、"s"、"t"、"y"、"z"、":"、"/" のいずれかのカスタム書式指定子を単体で使用するには)、指定子の前または後に空白を挿入するか、または単一のカスタム日時指定子の前にパーセント ("%") 書式指定子を挿入します。

たとえば、"`%h"` は、現在の日時値が表す時要素を表示するカスタム日時書式指定文字列として解釈されます。 また、" h" または "h " の各書式指定文字列も使用できますが、書式設定後の文字列に時間と共に空白が挿入されます。 これらの 3 つの書式指定文字列の例を次に示します。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#16](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/literal1.cs#16)]
[!code-vb[Formatting.DateAndTime.Custom#16](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/literal1.vb#16)]

### <a name="escape"></a> エスケープ文字の使用

書式指定文字列内の "d"、"f"、"F"、"g"、"h"、"H"、"K"、"m"、"M"、"s"、"t"、"y"、"z"、":"、"/" の各文字は、リテラル文字ではなくカスタム書式指定子として解釈されます。 文字が書式指定子として解釈されないようにするには、その文字の前に、エスケープ文字の円記号 (\\) を付けます。 エスケープ文字は、その後に続く文字が、そのまま結果の文字列に含める必要がある文字リテラルであることを示します。

結果の文字列に円記号を含める場合は、円記号をもう 1 つ付加して、円記号自体をエスケープする必要があります (`\\`)。

> [!NOTE]
> C++ コンパイラや C# コンパイラなど、一部のコンパイラでは、同様に、1 つの円記号がエスケープ文字として解釈されることがあります。 書式設定時に文字列が正しく解釈されるようにするには、C# では、逐語的文字列リテラル文字 (@ 文字) を文字列の前に使用します。また、C# および C++ では、円記号の前にもう 1 つ円記号を付ける方法もあります。 両方の方法を次の C# の例に示します。

次の例では、エスケープ文字を使用して、書式設定操作で "h" と "m" の各文字が書式指定子として解釈されないようにします。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#15](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/escape1.cs#15)]
[!code-vb[Formatting.DateAndTime.Custom#15](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/escape1.vb#15)]

### <a name="control-panel-settings"></a>コントロール パネルの設定

コントロール パネルの **[地域と言語のオプション]** の設定は、各種のカスタム日時書式指定子を使った書式設定操作によって生成される結果の文字列に影響します。 これらの設定は、書式設定の制御に使用される値を提供する現在のスレッド カルチャに関連付けられた <xref:System.Globalization.DateTimeFormatInfo> オブジェクトを初期化するために使用されます。 コンピューターで使用する設定が異なる場合は、生成される文字列も異なります。

また、<xref:System.Globalization.CultureInfo.%23ctor%28System.String%29?displayProperty=nameWithType> コンストラクターを使用して、現在のシステム カルチャと同じカルチャを表す新しい <xref:System.Globalization.CultureInfo> オブジェクトをインスタンス化した場合、コントロール パネルの **[地域と言語のオプション]** 項目で設定されたカスタマイズが新しい <xref:System.Globalization.CultureInfo> オブジェクトに適用されます。 <xref:System.Globalization.CultureInfo.%23ctor%28System.String%2CSystem.Boolean%29?displayProperty=nameWithType> コンストラクターを使用すると、システムに対するカスタマイズが反映されない <xref:System.Globalization.CultureInfo> オブジェクトを作成できます。

### <a name="datetimeformatinfo-properties"></a>DateTimeFormatInfo のプロパティ

書式設定は、現在の <xref:System.Globalization.DateTimeFormatInfo> オブジェクトのプロパティの影響を受けます。このオブジェクトは、現在のスレッド カルチャによって暗黙的に指定されるか、または書式設定を実行するメソッドの <xref:System.IFormatProvider> パラメーターによって明示的に指定されます。 <xref:System.IFormatProvider> パラメーターには、カルチャを表す <xref:System.Globalization.CultureInfo> オブジェクト、または <xref:System.Globalization.DateTimeFormatInfo> オブジェクトを指定します。

各種のカスタム日時書式指定子によって生成される書式設定後の文字列も、現在の <xref:System.Globalization.DateTimeFormatInfo> オブジェクトのプロパティに依存します。 カスタム日時書式指定子によって生成される結果は、対応する <xref:System.Globalization.DateTimeFormatInfo> プロパティを変更することによって変えることができます。 たとえば、"ddd" 書式指定子を使用した場合、書式設定後の文字列には、<xref:System.Globalization.DateTimeFormatInfo.AbbreviatedDayNames%2A> 文字列配列に指定されている曜日の省略名が追加されます。 同様に、"MMMM" 書式指定子を使用した場合、書式設定後の文字列には、<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A> 文字列配列に指定されている月の正式名が追加されます。

## <a name="see-also"></a>参照

- <xref:System.DateTime?displayProperty=nameWithType>
- <xref:System.IFormatProvider?displayProperty=nameWithType>
- [型の書式設定](../../../docs/standard/base-types/formatting-types.md)
- [標準の日時形式文字列](../../../docs/standard/base-types/standard-date-and-time-format-strings.md)
- [サンプル: .NET Core WinForms 書式設定ユーティリティ (C#)](https://docs.microsoft.com/samples/dotnet/samples/winforms-formatting-utility-cs)
- [サンプル: .NET Core WinForms 書式設定ユーティリティ (Visual Basic)](https://docs.microsoft.com/samples/dotnet/samples/winforms-formatting-utility-vb)
