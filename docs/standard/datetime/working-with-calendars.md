---
title: カレンダーの使用
ms.date: 04/01/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- globalization [.NET], calendars
- calendars, global applications
- global applications, calendars
- world-ready applications, calendars
- international applications [.NET], calendars
- culture, calendars
ms.assetid: 0c1534e5-979b-4c8a-a588-1c24301aefb3
ms.openlocfilehash: de8e5a03c769a22f3320c7785706555898bf8c1c
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74802740"
---
# <a name="work-with-calendars"></a>カレンダーを操作する

日時の値は特定の時点を表しますが、その文字列形式はカルチャに依存し、特定のカルチャで日時の値の表示に使用される規則と、そのカルチャで使用される暦の両方に応じて決まります。 このトピックでは、.NET でのカレンダーのサポートについて説明し、日付値を操作するときに calendar クラスを使用する方法について説明します。

## <a name="calendars-in-net"></a>.NET のカレンダー

.NET のすべてのカレンダーは、基本カレンダーの実装を提供する <xref:System.Globalization.Calendar?displayProperty=nameWithType> クラスから派生します。 <xref:System.Globalization.Calendar> クラスを継承するクラスの 1 つに <xref:System.Globalization.EastAsianLunisolarCalendar> クラスがあります。これは、すべての太陰太陽暦の基本クラスです。 .NET には、次のカレンダー実装が含まれています。

- <xref:System.Globalization.ChineseLunisolarCalendar>。中国の太陰太陽暦を表します。

- <xref:System.Globalization.GregorianCalendar>。グレゴリオ暦を表します。 この暦は、<xref:System.Globalization.GregorianCalendarTypes?displayProperty=nameWithType> 列挙体で定義されるサブタイプ (アラビア語や中東フランス語など) にさらに分けられます。 <xref:System.Globalization.GregorianCalendar.CalendarType%2A?displayProperty=nameWithType> プロパティは、グレゴリオ暦のサブタイプを指定します。

- <xref:System.Globalization.HebrewCalendar>。ヘブライ暦を表します。

- <xref:System.Globalization.HijriCalendar>。回教暦を表します。

- <xref:System.Globalization.JapaneseCalendar>。和暦を表します。

- <xref:System.Globalization.JapaneseLunisolarCalendar>。日本の太陰太陽暦を表します。

- <xref:System.Globalization.JulianCalendar>。ユリウス暦を表します。

- <xref:System.Globalization.KoreanCalendar>。韓国暦を表します。

- <xref:System.Globalization.KoreanLunisolarCalendar>。韓国の太陰太陽暦を表します。

- <xref:System.Globalization.PersianCalendar>。ペルシャ暦を表します。

- <xref:System.Globalization.TaiwanCalendar>。台湾暦を表します。

- <xref:System.Globalization.TaiwanLunisolarCalendar>。台湾の太陰太陽暦を表します。

- <xref:System.Globalization.ThaiBuddhistCalendar>。タイ仏暦を表します。

- <xref:System.Globalization.UmAlQuraCalendar>。ウムアルクラ暦を表します。

暦は、次の 2 とおりの方法で使用できます。

- 特定のカルチャで使用される暦として使用する。 <xref:System.Globalization.CultureInfo> オブジェクトにはそれぞれ、現在の暦 (オブジェクトで現在使用している暦) があります。 あらゆる日付と時刻の値の文字列形式には、現在のカルチャとその現在の暦が自動的に反映されます。 通常、現在の暦は、カルチャの既定の暦になります。 <xref:System.Globalization.CultureInfo> オブジェクトには、カルチャで使用できる追加のカレンダーを含む、オプションのカレンダーもあります。

- 特定のカルチャに依存しないスタンドアロンの暦として使用する。 この場合、暦が反映された値として日付を表すには、<xref:System.Globalization.Calendar> のメソッドを使用します。

<xref:System.Globalization.ChineseLunisolarCalendar>、<xref:System.Globalization.JapaneseLunisolarCalendar>、<xref:System.Globalization.JulianCalendar>、<xref:System.Globalization.KoreanLunisolarCalendar>、<xref:System.Globalization.PersianCalendar>、および <xref:System.Globalization.TaiwanLunisolarCalendar> の 6 つの Calendar クラスは、スタンドアロンの暦としてのみ使用できます。 これらは、どのカルチャでも、既定の暦またはオプションの暦としては使用されません。

## <a name="calendars-and-cultures"></a>カレンダーとカルチャ

各カルチャには既定の暦があり、<xref:System.Globalization.CultureInfo.Calendar%2A?displayProperty=nameWithType> プロパティで定義されます。 <xref:System.Globalization.CultureInfo.OptionalCalendars%2A?displayProperty=nameWithType> プロパティは、特定のカルチャでサポートされるすべての暦を指定する <xref:System.Globalization.Calendar> オブジェクトの配列を返します。これには、そのカルチャの既定の暦も含まれます。

<xref:System.Globalization.CultureInfo.Calendar%2A?displayProperty=nameWithType> プロパティと <xref:System.Globalization.CultureInfo.OptionalCalendars%2A?displayProperty=nameWithType> プロパティの例を次に示します。 この例では、タイ語 (タイ) と日本語 (日本) のカルチャの `CultureInfo` オブジェクトをそれぞれ作成し、それらの既定の暦とオプションの暦を表示します。 どちらについても、カルチャの既定の暦は <xref:System.Globalization.CultureInfo.OptionalCalendars%2A?displayProperty=nameWithType> コレクションにも含まれます。

[!code-csharp[Conceptual.Calendars#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/calendarinfo1.cs#1)]
[!code-vb[Conceptual.Calendars#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/calendarinfo1.vb#1)]

特定の <xref:System.Globalization.CultureInfo> オブジェクトで現在使用されている暦は、カルチャの <xref:System.Globalization.DateTimeFormatInfo.Calendar%2A?displayProperty=nameWithType> プロパティで定義されます。 カルチャの <xref:System.Globalization.DateTimeFormatInfo> オブジェクトは、<xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType> プロパティから返されます。 カルチャの作成時の既定値は <xref:System.Globalization.CultureInfo.Calendar%2A?displayProperty=nameWithType> プロパティの値と同じですが、 カルチャの現在の暦は、<xref:System.Globalization.CultureInfo.OptionalCalendars%2A?displayProperty=nameWithType> プロパティから返される配列に含まれる任意の暦に変更することができます。 現在の暦を <xref:System.Globalization.CultureInfo.OptionalCalendars%2A?displayProperty=nameWithType> プロパティの値に含まれない暦に設定しようとすると、<xref:System.ArgumentException> がスローされます。

アラビア語 (サウジアラビア) のカルチャで使用する暦を変更する例を次に示します。 まず、<xref:System.DateTime> 値をインスタンス化し、現在のカルチャ (この例では英語 (米国)) と現在のカルチャの暦 (この例ではグレゴリオ暦) を使用して日付を表示します。 次に、現在のカルチャをアラビア語 (サウジアラビア) に変更し、そのカルチャの既定のウムアルクラ暦を使用して日付を表示します。 さらに、`CalendarExists` メソッドを呼び出して、アラビア語 (サウジアラビア) のカルチャで回教暦がサポートされているかどうかを判別します。 この暦はサポートされているため、現在の暦を回教暦に変更し、日付をもう一度表示します。 いずれの場合も、日付は、現在のカルチャの現在の暦を使用して表示されます。

[!code-csharp[Conceptual.Calendars#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/changecalendar2.cs#2)]
[!code-vb[Conceptual.Calendars#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/changecalendar2.vb#2)]

## <a name="dates-and-calendars"></a>日付とカレンダー

<xref:System.Globalization.Calendar> 型のパラメーターを含み、指定した暦の値を日付の要素 (月、日、および年) に反映できるコンストラクターは例外として、<xref:System.DateTime> と <xref:System.DateTimeOffset> の値は、どちらも常にグレゴリオ暦に基づきます。 つまり、たとえば <xref:System.DateTime.Year%2A?displayProperty=nameWithType> プロパティはグレゴリオ暦の年を返し、<xref:System.DateTime.Day%2A?displayProperty=nameWithType> プロパティはグレゴリオ暦の月の日付を返します。

> [!IMPORTANT]
> 日付の値とその文字列形式で処理が異なることに注意してください。 日付の値はグレゴリオ暦に基づき、文字列形式は特定のカルチャの現在の暦に基づきます。

次の例は、<xref:System.DateTime> のプロパティと、それに対応する <xref:System.Globalization.Calendar> のメソッドの違いを示しています。 この例では、現在のカルチャはアラビア語 (エジプト)、現在の暦はウムアルクラ暦です。 <xref:System.DateTime> 値は、2011 年の 7 番目の月の 15 番目の日に設定されています。 この値は、明らかにグレゴリオ暦の日付と解釈されます。<xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> メソッドでインバリアント カルチャの規則を使用した場合に、これらと同じ値が返されるためです。 現在のカルチャの規則を使用して書式指定された日付の文字列形式は 14/08/32 です。これは、この日付に相当するウムアルクラ暦の日付です。 次に、`DateTime` と `Calendar` のメンバーを使用して、<xref:System.DateTime> 値の日、月、および年が返されます。 いずれの場合も、<xref:System.DateTime> のメンバーから返される値にはグレゴリオ暦の値が反映され、<xref:System.Globalization.UmAlQuraCalendar> のメンバーから返される値にはウムアルクラ暦の値が反映されます。

[!code-csharp[Conceptual.Calendars#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/datesandcalendars2.cs#3)]
[!code-vb[Conceptual.Calendars#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/datesandcalendars2.vb#3)]

### <a name="instantiate-dates-based-on-a-calendar"></a>カレンダーに基づいて日付をインスタンス化する

<xref:System.DateTime> と <xref:System.DateTimeOffset> の値はグレゴリオ暦に基づくため、別の暦の日、月、または年の値を使用する場合は、<xref:System.Globalization.Calendar> 型のパラメーターを含むオーバーロードされたコンストラクターを呼び出して日付の値をインスタンス化する必要があります。 また、特定の暦の <xref:System.Globalization.Calendar.ToDateTime%2A?displayProperty=nameWithType> メソッドのいずれかのオーバーロードを呼び出して、特定の暦の値に基づいて <xref:System.DateTime> オブジェクトをインスタンス化することもできます。

次の例では、<xref:System.DateTime> オブジェクトを <xref:System.Globalization.HebrewCalendar> コンストラクターに渡して 1 つの <xref:System.DateTime> 値をインスタンス化し、<xref:System.DateTime> メソッドを呼び出してもう 1 つの <xref:System.Globalization.HebrewCalendar.ToDateTime%28System.Int32%2CSystem.Int32%2CSystem.Int32%2CSystem.Int32%2CSystem.Int32%2CSystem.Int32%2CSystem.Int32%2CSystem.Int32%29?displayProperty=nameWithType> 値をインスタンス化しています。 2 つの値はヘブライ暦の同一の値を使用して作成されるため、<xref:System.DateTime.Equals%2A?displayProperty=nameWithType> メソッドを呼び出すと、2 つの <xref:System.DateTime> 値が等しいことが示されます。

[!code-csharp[Conceptual.Calendars#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/instantiatehcdate1.cs#4)]
[!code-vb[Conceptual.Calendars#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/instantiatehcdate1.vb#4)]

### <a name="represent-dates-in-the-current-calendar"></a>現在のカレンダーの日付を表します

日時書式指定メソッドでは、日付を文字列に変換する際に、常に現在の暦を使用します。 つまり、年、月、および日の文字列形式には現在の暦が反映され、必ずしもグレゴリオ暦が反映されるとは限りません。

次の例は、現在の暦が日付の文字列形式にどのように影響するかを示しています。 この例では、現在のカルチャを中国語 (繁体字、台湾) に変更し、日付の値をインスタンス化します。 その後、現在の暦と日付を表示し、現在の暦を <xref:System.Globalization.TaiwanCalendar> に変更して、現在の暦と日付をもう一度表示します。 最初に日付を表示したときは、日付がグレゴリオ暦の日付として表され、 2 回目に表示したときは、台湾暦の日付として表されます。

[!code-csharp[Conceptual.Calendars#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/currentcalendar1.cs#5)]
[!code-vb[Conceptual.Calendars#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/currentcalendar1.vb#5)]

### <a name="represent-dates-in-a-non-current-calendar"></a>現在の暦以外の日付を表します

特定のカルチャの現在の暦以外の暦を使用して日付を表すには、その <xref:System.Globalization.Calendar> オブジェクトのメソッドを呼び出す必要があります。 たとえば、<xref:System.Globalization.Calendar.GetYear%2A?displayProperty=nameWithType>、<xref:System.Globalization.Calendar.GetMonth%2A?displayProperty=nameWithType>、および <xref:System.Globalization.Calendar.GetDayOfMonth%2A?displayProperty=nameWithType> の各メソッドは、それぞれ年、月、および日を、特定の暦を反映した値に変換します。

> [!WARNING]
> 一部の暦はどのカルチャのオプションの暦にもなっていないため、そのような暦で日付を表す場合は、常に Calendar のメソッドを呼び出す必要があります。 これは、<xref:System.Globalization.EastAsianLunisolarCalendar> クラス、<xref:System.Globalization.JulianCalendar> クラス、および <xref:System.Globalization.PersianCalendar> クラスから派生したすべての暦に当てはまります。

次の例では、<xref:System.Globalization.JulianCalendar> オブジェクトを使用して、ユリウス暦の 1905 年 1 月 9 日という日付をインスタンス化します。 この日付を既定の暦 (グレゴリオ暦) を使用して表示した場合は、1905 年 1 月 22 日と表されます。 <xref:System.Globalization.JulianCalendar> の各メソッドを呼び出すことで、この日付をユリウス暦で表すことができます。

[!code-csharp[Conceptual.Calendars#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/noncurrentcalendar1.cs#6)]
[!code-vb[Conceptual.Calendars#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/noncurrentcalendar1.vb#6)]

### <a name="calendars-and-date-ranges"></a>カレンダーと日付範囲

暦でサポートされている最も古い日付は、暦の <xref:System.Globalization.Calendar.MinSupportedDateTime%2A?displayProperty=nameWithType> プロパティによって示されます。 <xref:System.Globalization.GregorianCalendar> クラスでは、その日付は西暦 0001 年 1 月 1 日 です。 .NET の他のほとんどのカレンダーでは、その後の日付がサポートされています。 暦でサポートされている最も古い日付より前の日付と時刻の値を処理しようとすると、<xref:System.ArgumentOutOfRangeException> 例外がスローされます。

ただし、重要な例外が 1 つあります。 <xref:System.DateTime> オブジェクトと <xref:System.DateTimeOffset> オブジェクトの既定の (初期化されていない) 値は、<xref:System.Globalization.GregorianCalendar.MinSupportedDateTime%2A?displayProperty=nameWithType> 値と同じです。 西暦0001年1月1日をサポートしないカレンダーでこの日付の書式を設定しようとすると、 書式指定子を指定しない場合、書式設定メソッドでは、"G" (一般的な日付と時刻のパターン) 書式指定子の代わりに "s" (並べ替え可能な日付と時刻のパターン) 書式指定子が使用されます。 その結果、書式設定操作は <xref:System.ArgumentOutOfRangeException> 例外をスローしません。 代わりに、サポートされていない日付を返します。 この問題を、次の例で説明します。この例は、現在のカルチャが日本語 (日本) に設定されていれば和暦で、アラビア語 (エジプト) に設定されていればウムアルクラ暦で、<xref:System.DateTime.MinValue?displayProperty=nameWithType> の値を表示します。 また、現在のカルチャを英語 (米国) に設定し、これらの各 <xref:System.DateTime.ToString%28System.IFormatProvider%29?displayProperty=nameWithType> オブジェクトで <xref:System.Globalization.CultureInfo> メソッドを呼び出します。 どの場合も、並べ替え可能な日付と時刻のパターンを使用して、日付が表示されます。

[!code-csharp[Conceptual.Calendars#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/minsupporteddatetime1.cs#11)]
[!code-vb[Conceptual.Calendars#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/minsupporteddatetime1.vb#11)]

## <a name="work-with-eras"></a>時代 (年号) を使用する

暦では通常、日付が時代 (年号) に分けられます。 ただし、.NET の <xref:System.Globalization.Calendar> クラスは暦で定義されているすべての時代 (年号) をサポートしておらず、ほとんどの <xref:System.Globalization.Calendar> クラスは1つの時代 (年号) のみをサポートしています。 複数の時代 (年号) をサポートしているのは、<xref:System.Globalization.JapaneseCalendar> クラスと <xref:System.Globalization.JapaneseLunisolarCalendar> クラスだけです。

> [!IMPORTANT]
> <xref:System.Globalization.JapaneseCalendar> と <xref:System.Globalization.JapaneseLunisolarCalendar>の新しい時代 (年号) は、2019年5月1日から開始されます。 この変更は、これらのカレンダーを使用するすべてのアプリケーションに影響します。 詳細については、以下の記事を参照してください。
>
> - [.Net の日本語の暦で新しい時代 (年号) を処理](https://devblogs.microsoft.com/dotnet/handling-a-new-era-in-the-japanese-calendar-in-net/)する。これは、複数の時代 (年号) を持つ暦をサポートするために .net に追加された機能を文書にし、複数の時代 (年号) の暦を処理するときのベストプラクティスについて説明
> - [日本語時代の変更に備えてアプリケーションを準備](/windows/uwp/design/globalizing/japanese-era-change)します。これにより、Windows でのアプリケーションのテストに関する情報が提供され、時代 (年号) の変化に対応できるようになります。
> - [.NET Framework 用の新しい日本語時代 (年号) の更新の概要](https://support.microsoft.com/help/4477957/new-japanese-era-updates-for-net-framework)。新しい日本語のカレンダー時代 (年号) に関連する Windows の各バージョンの更新プログラムの一覧が .NET Framework 表示されます。また、複数の時代 (年号) をサポートするための .NET Framework 新機能について説明し、アプリケーションをテストする際の注意事項も記載しています。

ほとんどの暦の時代 (年号) は、非常に長い期間を表します。 たとえば、グレゴリオ暦では、現在の時代 (年号) は2つ以上の millennia にまたがります。 <xref:System.Globalization.JapaneseCalendar> と <xref:System.Globalization.JapaneseLunisolarCalendar>については、複数の時代 (年号) をサポートする2つのカレンダーではありません。 時代 (年号) は、皇帝の reign の期間に対応します。 複数の時代 (特に現在の時代 (年号) の上限が不明な場合) をサポートすることで、特別な課題が生じます。

### <a name="eras-and-era-names"></a>時代 (年号) と時代 (年号) の名前

.NET では、特定の暦の実装でサポートされる時代 (年号) を表す整数は、<xref:System.Globalization.Calendar.Eras%2A?displayProperty=nameWithType> 配列に逆順で格納されます。 現在の時代 (年号) はインデックス0にあり、複数の時代 (年号) をサポートする <xref:System.Globalization.Calendar> クラスの場合は、連続した各インデックスに前の時代 (年号) が反映されます。 <xref:System.Globalization.Calendar.CurrentEra?displayProperty=nameWithType> 配列における現在の時代 (年号) のインデックスは、静的な <xref:System.Globalization.Calendar.Eras%2A?displayProperty=nameWithType> プロパティで定義されます。これは定数であり、値は常に 0 になります。 個々の <xref:System.Globalization.Calendar> クラスには、現在の時代 (年号) の値を返す静的フィールドも含まれています。 これらを次の表に示します。

| Calendar クラス                                        | 現在の時代 (年号) のフィールド                                                 |
| ----------------------------------------------------- | ----------------------------------------------------------------- |
| <xref:System.Globalization.ChineseLunisolarCalendar>  | <xref:System.Globalization.ChineseLunisolarCalendar.ChineseEra>   |
| <xref:System.Globalization.GregorianCalendar>         | <xref:System.Globalization.GregorianCalendar.ADEra>               |
| <xref:System.Globalization.HebrewCalendar>            | <xref:System.Globalization.HebrewCalendar.HebrewEra>              |
| <xref:System.Globalization.HijriCalendar>             | <xref:System.Globalization.HijriCalendar.HijriEra>                |
| <xref:System.Globalization.JapaneseLunisolarCalendar> | <xref:System.Globalization.JapaneseLunisolarCalendar.JapaneseEra> |
| <xref:System.Globalization.JulianCalendar>            | <xref:System.Globalization.JulianCalendar.JulianEra>              |
| <xref:System.Globalization.KoreanCalendar>            | <xref:System.Globalization.KoreanCalendar.KoreanEra>              |
| <xref:System.Globalization.KoreanLunisolarCalendar>   | <xref:System.Globalization.KoreanLunisolarCalendar.GregorianEra>  |
| <xref:System.Globalization.PersianCalendar>           | <xref:System.Globalization.PersianCalendar.PersianEra>            |
| <xref:System.Globalization.ThaiBuddhistCalendar>      | <xref:System.Globalization.ThaiBuddhistCalendar.ThaiBuddhistEra>  |
| <xref:System.Globalization.UmAlQuraCalendar>          | <xref:System.Globalization.UmAlQuraCalendar.UmAlQuraEra>          |

特定の時代 (年号) を表す数値に対応する名前は、その数値を <xref:System.Globalization.DateTimeFormatInfo.GetEraName%2A?displayProperty=nameWithType> メソッドまたは <xref:System.Globalization.DateTimeFormatInfo.GetAbbreviatedEraName%2A?displayProperty=nameWithType> メソッドに渡すことで取得できます。 次の例では、これらのメソッドを呼び出して、<xref:System.Globalization.GregorianCalendar> クラスでサポートされる時代 (年号) に関する情報を取得しています。 現在の時代 (年号) の2年1月1日に対応するグレゴリオ暦の日付と、サポートされている各和暦の時代 (年号) の2年1月1日に対応するグレゴリオ暦の日付を表示します。

[!code-csharp[Conceptual.Calendars#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/instantiatewithera1.cs)]
[!code-vb[Conceptual.Calendars#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/instantiatewithera1.vb)]

また、"g" カスタム日時書式指定文字列では、日付と時刻の文字列形式に暦の時代 (年号) の名前が含まれます。 詳細については、「[カスタム日時書式指定文字列](../../../docs/standard/base-types/custom-date-and-time-format-strings.md)」を参照してください。

### <a name="instantiatie-a-date-with-an-era"></a>時代 (年号) で日付を Instantiatie

複数の時代 (年号) をサポートする2つの <xref:System.Globalization.Calendar> クラスでは、特定の年、月、および日の値で構成される日付をあいまいにすることができます。 たとえば、<xref:System.Globalization.JapaneseCalendar> でサポートされているすべての時代 (年号) は、数値が1である年です。 通常、時代 (年号) が指定されていない場合は、日時および暦のどちらのメソッドでも、値が現在の時代 (年号) に属すると見なされます。 これは、型 <xref:System.Globalization.Calendar>のパラメーターを含む <xref:System.DateTime.%23ctor%2A> および <xref:System.DateTimeOffset.%23ctor%2A> コンストラクターと、 [JapaneseCalendar](xref:System.Globalization.Calendar.ToDateTime(System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32))メソッドと[JapaneseLunisolarCalendar](xref:System.Globalization.Calendar.ToDateTime(System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32))メソッドにも当てはまります。 次の例では、指定されていない時代 (年号) の2番目の年の1月1日を表す日付をインスタンス化します。 Reiwa 時代 (年号) が現在の時代 (年号) である場合に例を実行すると、日付は Reiwa 時代 (年号) の2番目の年として解釈されます。 時代 (年号) 令和は、<xref:System.DateTime.ToString(System.String,System.IFormatProvider)?displayProperty=nameWithType> メソッドによって返される文字列の年の前にあり、グレゴリオ暦の2020年1月1日に対応します。 (Reiwa 時代 (年号) は、グレゴリオ暦の2019年から始まります)。

[!code-csharp[A date in the current era](~/samples/snippets/standard/datetime/calendars/current-era/cs/program.cs)]
[!code-vb[A date in the current era](~/samples/snippets/standard/datetime/calendars/current-era/vb/program.vb)]

ただし、時代 (年号) が変更された場合、このコードの目的はあいまいになります。 現在の時代 (年号) の2番目の年を表すために使用する日付を指定します。または、Heisei 時代 (年号) の2番目の年を表すことを目的としていますか? このようなあいまいさを回避するには、次の2つの方法があります。

- 既定の <xref:System.Globalization.GregorianCalendar> クラスを使用して、日付と時刻の値をインスタンス化します。 次の例に示すように、日付の文字列形式には、日本語の暦または日本の太陰太陽暦の暦を使用できます。

  [!code-csharp[Insantiating a Gregorian date](~/samples/snippets/standard/datetime/calendars/gregorian/cs/program.cs)]
  [!code-vb[Instantiating a Gregorian date](~/samples/snippets/standard/datetime/calendars/gregorian/vb/program.vb)]

- 時代 (年号) を明示的に指定する日付と時刻のメソッドを呼び出します。 これには次のメソッドが含まれます。

  - <xref:System.Globalization.JapaneseCalendar> または <xref:System.Globalization.JapaneseLunisolarCalendar> クラスの <xref:System.Globalization.Calendar.ToDateTime(System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32)> メソッド。

  - 解析する文字列を含む、<xref:System.DateTime.Parse%2A>、<xref:System.DateTime.TryParse%2A>、<xref:System.DateTime.ParseExact%2A>、<xref:System.DateTime.TryParseExact%2A>などの <xref:System.DateTime> または <xref:System.DateTimeOffset> の解析メソッド。現在のカルチャが日本 ("ja-jp") で、カルチャの暦が <xref:System.Globalization.DateTimeStyles> の場合は、省略可能な <xref:System.Globalization.JapaneseCalendar>引数。 解析する文字列には、時代 (年号) を含める必要があります。

  - <xref:System.IFormatProvider>型の `provider` パラメーターを含む、<xref:System.DateTime> または <xref:System.DateTimeOffset> の解析メソッド。 `provider` は、現在の暦が <xref:System.Globalization.JapaneseCalendar>、または <xref:System.Globalization.DateTimeFormatInfo.Calendar> プロパティが <xref:System.Globalization.JapaneseCalendar>である <xref:System.Globalization.DateTimeFormatInfo> オブジェクトである、日本-日本 ("ja-jp") カルチャを表す <xref:System.Globalization.CultureInfo> オブジェクトである必要があります。 解析する文字列には、時代 (年号) を含める必要があります。

  次の例では、これら3つのメソッドを使用して、1868年9月8日に開始し、1912年7月29日に終了した Meiji 時代 (年号) の日付と時刻をインスタンス化します。

  [!code-csharp[A date in a specified era](~/samples/snippets/standard/datetime/calendars/specify-era/cs/program.cs)]
  [!code-vb[A date in a specified era](~/samples/snippets/standard/datetime/calendars/specify-era/vb/program.vb)]

> [!TIP]
> 複数の時代 (年号) をサポートする暦を使用する場合は、日付をインスタンス化するためにグレゴリオ暦の日付を*常*に使用するか、そのカレンダーに基づいて日付と時刻をインスタンス化するときに時代 (年号) を指定します。

<xref:System.Globalization.Calendar.ToDateTime(System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32)> メソッドに時代 (年号) を指定する場合は、カレンダーの <xref:System.Globalization.Calendar.Eras> プロパティで時代 (年号) のインデックスを指定します。 ただし、時代 (年号) が変更される予定のカレンダーでは、これらのインデックスは定数値ではありません。現在の時代 (年号) はインデックス0で、最も古い時代 (年号) はインデックス `Eras.Length - 1`にあります。 新しい時代 (年号) が予定表に追加されると、前の時代 (年号) のインデックスが1つ増えます。 次のように、適切な era インデックスを指定できます。

- 現在の時代 (年号) の日付については、カレンダーの <xref:System.Globalization.Calendar.CurrentEra> プロパティを常に使用してください。

- 指定した時代 (年号) の日付については、<xref:System.Globalization.DateTimeFormatInfo.GetEraName%2A?displayProperty=nameWithType> メソッドを使用して、指定した時代 (年号) 名に対応するインデックスを取得します。 この場合、<xref:System.Globalization.JapaneseCalendar> は、ja-jp カルチャを表す <xref:System.Globalization.CultureInfo> オブジェクトの現在の暦である必要があります。  (この手法は、<xref:System.Globalization.JapaneseCalendar>と同じ時代 (年号) をサポートしているため、<xref:System.Globalization.JapaneseLunisolarCalendar> にも使用できます)。前の例では、この方法を示しています。

### <a name="calendars-eras-and-date-ranges-relaxed-range-checks"></a>カレンダー、時代 (年号)、および日付範囲: 緩やかな範囲のチェック

個々のカレンダーとほぼ同じように、日付範囲がサポートされています。また、<xref:System.Globalization.JapaneseCalendar> と <xref:System.Globalization.JapaneseLunisolarCalendar> クラスの時代 (年号) にもサポートされている範囲があります。 以前は、.NET は厳密な時代 (年号) の範囲チェックを使用して、時代 (年号) 固有の日付がその時代 (年号) の範囲内にあることを確認しました。 つまり、日付が指定された時代 (年号) の範囲外にある場合、メソッドは <xref:System.ArgumentOutOfRangeException>をスローします。 現在、.NET では、緩やかに使用される範囲検査が既定で使用されています。 .NET のすべてのバージョンの更新では、緩和された時代 (年号) のチェックが導入されました。指定された時代 (年号) の範囲外にある年号固有の日付を次の時代 (年号) にインスタンス化しようとすると、例外はスローされません。

次の例では、1926年12月25日に開始し、1989年1月7日に終了した、Showa 時代 (年号) の65番目の年の日付をインスタンス化しようとしています。 この日付は、<xref:System.Globalization.JapaneseCalendar>の Showa 時代 (年号) の範囲外にある1990年1月9日に相当します。 例の出力が示すように、この例で表示される日付は、Heisei 時代 (年号) の2年目の1990年1月9日になります。

  [!code-csharp[Relaxed range checks](~/samples/snippets/standard/datetime/calendars/relaxed-range/cs/program.cs)]
  [!code-vb[Relaxed range checks](~/samples/snippets/standard/datetime/calendars/relaxed-range/vb/program.vb)]

厳密でない範囲チェックが望ましくない場合は、アプリケーションが実行されている .NET のバージョンに応じて、さまざまな方法で厳密な範囲チェックを復元できます。

- **.Net Core:** 次のものを *. netcore. json*構成ファイルに追加します。

  ```json
  "runtimeOptions": {
    "configProperties": {
        "Switch.System.Globalization.EnforceJapaneseEraYearRanges": true
    }
  }
  ```

- **.NET Framework 4.6 以降:** *App.config*ファイルで次の appcontext スイッチを設定します。

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Globalization.EnforceJapaneseEraYearRanges=true" />
    </runtime>
  </configuration>
  ```

- **4.5.2 以前の .NET Framework:** 次のレジストリ値を設定します。

   |  |  |
   |--|--|
   | **Key** | **\Software\Microsoft\\を HKEY_LOCAL_MACHINE します。Netframework\ appcontext** |
   | **Name** | EnforceJapaneseEraYearRanges を切り替えます。 |
   | **Type** | REG_SZ |
   | **[値]** | true |

厳密な範囲チェックが有効になっている場合、前の例では <xref:System.ArgumentOutOfRangeException> がスローされ、次の出力が表示されます。

```console
Unhandled Exception: System.ArgumentOutOfRangeException: Valid values are between 1 and 64, inclusive.
Parameter name: year
   at System.Globalization.GregorianCalendarHelper.GetYearOffset(Int32 year, Int32 era, Boolean throwOnError)
   at System.Globalization.GregorianCalendarHelper.ToDateTime(Int32 year, Int32 month, Int32 day, Int32 hour, Int32 minute, Int32 second, Int32 millisecond, Int32 era)
   at Example.Main()
```

### <a name="represent-dates-in-calendars-with-multiple-eras"></a>複数の時代 (年号) のカレンダーで日付を表す

<xref:System.Globalization.Calendar> オブジェクトの現在の暦が、時代 (年号) をサポートする <xref:System.Globalization.CultureInfo> オブジェクトである場合は、完全な日付と時刻、長い日付、および短い日付の各パターンにおいて、日付と時刻の値の文字列形式に時代 (年号) が含まれます。 それらの日付パターンを表示する例を次に示します。現在のカルチャは日本 (日本語)、現在の暦は和暦です。

[!code-csharp[Conceptual.Calendars#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/formatstrings1.cs#8)]
[!code-vb[Conceptual.Calendars#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/formatstrings1.vb#8)]

> [!WARNING]
> <xref:System.Globalization.JapaneseCalendar> クラスは、.NET の calendar クラスであり、複数の時代 (年号) の日付をサポートしています。また、<xref:System.Globalization.CultureInfo> オブジェクトの現在の暦 (特に日本語 (日本) のカルチャを表す <xref:System.Globalization.CultureInfo> オブジェクト) である場合もあります。

どの暦の場合も、"g" カスタム書式指定子の結果の文字列には時代 (年号) が含まれます。 次の例では、"MM-dd-yyyy g" というカスタム書式指定文字列を使用して、結果の文字列に時代 (年号) を含めています。現在の暦はグレゴリオ暦です。

[!code-csharp[Conceptual.Calendars#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/formatstrings2.cs#9)]
[!code-vb[Conceptual.Calendars#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/formatstrings2.vb#9)]

日付の文字列形式を現在の暦以外の暦で表す場合は、<xref:System.Globalization.Calendar> クラスの <xref:System.Globalization.Calendar.GetEra%2A?displayProperty=nameWithType> メソッドを <xref:System.Globalization.Calendar.GetYear%2A?displayProperty=nameWithType>、<xref:System.Globalization.Calendar.GetMonth%2A?displayProperty=nameWithType>、および <xref:System.Globalization.Calendar.GetDayOfMonth%2A?displayProperty=nameWithType> の各メソッドと一緒に使用して、日付とそれが属する時代 (年号) を明確に示すことができます。 <xref:System.Globalization.JapaneseLunisolarCalendar> クラスを使用した具体的な例を次に示します。 ただし、時代 (年号) を表す整数ではなく、名前または省略形を結果の文字列に含めるには、<xref:System.Globalization.DateTimeFormatInfo> オブジェクトをインスタンス化し、その現在の暦を <xref:System.Globalization.JapaneseCalendar> にする必要があります (<xref:System.Globalization.JapaneseLunisolarCalendar> 暦をカルチャの現在の暦にすることはできませんが、この場合は 2 つの暦で同じ時代 (年号) が共有されます)。

[!code-csharp[Conceptual.Calendars#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/formatstrings3.cs#10)]
[!code-vb[Conceptual.Calendars#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/formatstrings3.vb#10)]

日本語のカレンダーでは、時代 (年号) の最初の年は元年と呼ばれています。 たとえば、平成 1 年ではなく、平成時代 (年号) の最初の年を平成元年として記述できます。 .NET では、次の標準またはカスタムの日時書式指定文字列で書式指定された日付と時刻の書式設定操作で、<xref:System.Globalization.JapaneseCalendar> クラスを使用した日本-日本 ("ja-jp") カルチャを表す <xref:System.Globalization.CultureInfo> オブジェクトと共に使用する場合に、この規則が適用されます。

- "D" 標準の日付と時刻の書式指定文字列で示される[長い形式の日付パターン](../base-types/standard-date-and-time-format-strings.md#LongDate)。
- "F" 標準の日付と時刻の書式指定文字列で示される、[完全な日付と長い形式の時刻パターン](../base-types/standard-date-and-time-format-strings.md#FullDateLongTime)。
- "F" 標準の日付と時刻の書式指定文字列で示さ[れる、完全な日付と短い形式の時刻パターン](../base-types/standard-date-and-time-format-strings.md#FullDateShortTime)。
- "Y" または "y" の標準の日時書式指定文字列で表さ[れる年/月パターン](../base-types/standard-date-and-time-format-strings.md#YearMonth)。
- ["Ggy ' 年 '" または "ggy年"[カスタム日時書式指定文字列](../base-types/custom-date-and-time-format-strings.md)。

たとえば、次の例では、<xref:System.Globalization.JapaneseCalendar>の Heisei 時代 (年号) の最初の年の日付を表示します。

  [!code-csharp[gannen](~/samples/snippets/standard/datetime/calendars/gannen/cs/program.cs)]
  [!code-vb[gannen](~/samples/snippets/standard/datetime/calendars/gannen/vb/gannen-fmt.vb)]

書式設定操作でこの動作が望ましくない場合は、.NET のバージョンに応じて次の操作を行うことで、以前の動作を復元できます。これは、常に時代 (年号) の最初の年を "ガント" ではなく "1" として表します。

- **.Net Core:** 次のものを *. netcore. json*構成ファイルに追加します。

  ```json
  "runtimeOptions": {
    "configProperties": {
        "Switch.System.Globalization.FormatJapaneseFirstYearAsANumber": true
    }
  }
  ```

- **.NET Framework 4.6 以降:** *App.config*ファイルで次の appcontext スイッチを設定します。

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Globalization.FormatJapaneseFirstYearAsANumber=true" />
    </runtime>
  </configuration>
  ```

- **4.5.2 以前の .NET Framework:** 次のレジストリ値を設定します。

   |  |  |
   |--|--|
   | **Key** | **\Software\Microsoft\\を HKEY_LOCAL_MACHINE します。Netframework\ appcontext** |
   | **Name** | FormatJapaneseFirstYearAsANumber を切り替えます。 |
   | **Type** | REG_SZ |
   | **[値]** | true |

書式設定操作で元年のサポートが無効になっている場合、前の例では次の出力が表示されます。

```console
Japanese calendar date: 平成1年8月18日 (Gregorian: Friday, August 18, 1989)
```

また、.NET も更新され、日付と時刻の解析操作で、"1" または "元年" として表される年を含む文字列がサポートされるようになりました。 これを行う必要はありませんが、以前の動作を復元して、時代 (年号) の最初の年として "1" だけを認識することができます。 .NET のバージョンに応じて、次のようにすることができます。

- **.Net Core:** 次のものを *. netcore. json*構成ファイルに追加します。

  ```json
  "runtimeOptions": {
    "configProperties": {
        "Switch.System.Globalization.EnforceLegacyJapaneseDateParsing": true
    }
  }
  ```

- **.NET Framework 4.6 以降:** *App.config*ファイルで次の appcontext スイッチを設定します。

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Globalization.EnforceLegacyJapaneseDateParsing=true" />
    </runtime>
  </configuration>
  ```

- **4.5.2 以前の .NET Framework:** 次のレジストリ値を設定します。

   |  |  |
   |--|--|
   | **Key** | **\Software\Microsoft\\を HKEY_LOCAL_MACHINE します。Netframework\ appcontext** |
   | **Name** | EnforceLegacyJapaneseDateParsing を切り替えます。 |
   | **Type** | REG_SZ |
   | **[値]** | true |

## <a name="see-also"></a>参照

- [方法: グレゴリオ暦以外の暦で日付を表示する](../../../docs/standard/base-types/how-to-display-dates-in-non-gregorian-calendars.md)
- [サンプル: Calendar week 範囲ユーティリティ](https://code.msdn.microsoft.com/NET-Framework-4-Calendar-3360a84a)
- [Calendar クラス](xref:System.Globalization.Calendar)
