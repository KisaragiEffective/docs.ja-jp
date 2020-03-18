---
title: '方法 : 特定の日付から曜日を抽出する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- formatting [.NET Framework], dates
- DateTime.DayOfWeek property
- DateTime.ToString method
- dates [.NET Framework], retrieving week information
- DateTimeOffset.DayOfWeek property
- dates [.NET Framework], day of week
- Weekday function
- day of week [.NET Framework]
- extracting day of week
- weekday names
- WeekdayName function
- numbers [.NET Framework], day of week
- formatting [.NET Framework], time
- DateTimeOffset.ToString method
- full weekday names
ms.assetid: 1c9bef76-5634-46cf-b91c-9b9eb72091d7
ms.openlocfilehash: 771bd0276310eecb534fb80836faadb1a8aa10bb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73084204"
---
# <a name="how-to-extract-the-day-of-the-week-from-a-specific-date"></a>方法 : 特定の日付から曜日を抽出する
.NET Framework では、特定の日付が週の何日目であるかを容易に判別でき、また特定の日付のローカライズされた曜日名を表示できます。 特定の日付に対応する曜日を示す列挙値は、<xref:System.DateTime.DayOfWeek%2A> または <xref:System.DateTimeOffset.DayOfWeek%2A> プロパティから取得できます。 対照的に、曜日名の取得は、書式指定メソッド （日付と時刻の値の `ToString` メソッドや <xref:System.String.Format%2A?displayProperty=nameWithType> メソッドなど） を呼び出して実行できる書式指定操作です。 このトピックでは、このような書式指定操作を実行する方法について説明します。  
  
### <a name="to-extract-a-number-indicating-the-day-of-the-week-from-a-specific-date"></a>特定の日付から曜日を示す番号を抽出するには  
  
1. 文字列形式の日付を処理している場合には、静的 <xref:System.DateTime> または <xref:System.DateTimeOffset> メソッドを使用して、その日付を<xref:System.DateTime.Parse%2A?displayProperty=nameWithType> 値または <xref:System.DateTimeOffset.Parse%2A?displayProperty=nameWithType> 値に変換します。  
  
2. <xref:System.DateTime.DayOfWeek%2A?displayProperty=nameWithType> プロパティまたは <xref:System.DateTimeOffset.DayOfWeek%2A?displayProperty=nameWithType> プロパティを使用して、曜日を示す <xref:System.DayOfWeek> 値を取得します。  
  
3. 必要に応じて、<xref:System.DayOfWeek> 値を整数にキャスト （C#） または変換 （Visual Basic） します。  
  
 次の例は、特定の日付の曜日を表す整数を表示します。  
  
 [!code-csharp[Formatting.Howto.WeekdayName#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/weekdaynumber7.cs#7)]
 [!code-vb[Formatting.Howto.WeekdayName#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/weekdaynumber7.vb#7)]  
  
### <a name="to-extract-the-abbreviated-weekday-name-from-a-specific-date"></a>特定の日付から曜日の省略名を抽出するには  
  
1. 文字列形式の日付を処理している場合には、静的 <xref:System.DateTime> または <xref:System.DateTimeOffset> メソッドを使用して、その日付を<xref:System.DateTime.Parse%2A?displayProperty=nameWithType> 値または <xref:System.DateTimeOffset.Parse%2A?displayProperty=nameWithType> 値に変換します。  
  
2. 次の手順で現在のカルチャまたは特定のカルチャの曜日の省略名を抽出できます。  
  
    1. 現在のカルチャの曜日の省略名を抽出するには、日付と時刻の値の <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType> インスタンス メソッドまたは <xref:System.DateTimeOffset.ToString%28System.String%29?displayProperty=nameWithType> インスタンス メソッドを呼び出し、`format` パラメーターとして文字列 "ddd" を渡します。 次の例に、<xref:System.DateTime.ToString%28System.String%29> メソッドの呼び出しを示します。  
  
         [!code-csharp[Formatting.Howto.WeekdayName#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/abbrname1.cs#1)]
         [!code-vb[Formatting.Howto.WeekdayName#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/abbrname1.vb#1)]  
  
    2. 特定のカルチャの曜日の省略名を抽出するには、日付と時刻の値の <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> インスタンス メソッドまたは <xref:System.DateTimeOffset.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> インスタンス メソッドを呼び出します。 `format` パラメーターとして文字列 "ddd" を渡します。 曜日名を取得するカルチャを表す <xref:System.Globalization.CultureInfo> または <xref:System.Globalization.DateTimeFormatInfo> オブジェクトのいずれかを`provider` パラメーターとして渡します。 次のコードは、fr-FR カルチャを表す <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29> オブジェクトを使用した <xref:System.Globalization.CultureInfo> メソッドの呼び出しを示します。  
  
         [!code-csharp[Formatting.Howto.WeekdayName#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/abbrname2.cs#2)]
         [!code-vb[Formatting.Howto.WeekdayName#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/abbrname2.vb#2)]  
  
### <a name="to-extract-the-full-weekday-name-from-a-specific-date"></a>特定の日付から曜日の正式な名前を抽出するには  
  
1. 文字列形式の日付を処理している場合には、静的 <xref:System.DateTime> または <xref:System.DateTimeOffset> メソッドを使用して、その日付を<xref:System.DateTime.Parse%2A?displayProperty=nameWithType> 値または <xref:System.DateTimeOffset.Parse%2A?displayProperty=nameWithType> 値に変換します。  
  
2. 現在のカルチャまたは特定のカルチャでの曜日の正式な名前を抽出できます。  
  
    1. 現在のカルチャの曜日名を抽出するには、日付と時刻の値の <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType> インスタンス メソッドまたは <xref:System.DateTimeOffset.ToString%28System.String%29?displayProperty=nameWithType> インスタンス メソッドを呼び出し、`format` パラメーターとして文字列 "dddd" を渡します。 次の例に、<xref:System.DateTime.ToString%28System.String%29> メソッドの呼び出しを示します。  
  
         [!code-csharp[Formatting.Howto.WeekdayName#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/fullname4.cs#4)]
         [!code-vb[Formatting.Howto.WeekdayName#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/fullname4.vb#4)]  
  
    2. 特定のカルチャの曜日名を抽出するには、日付と時刻の値の <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> インスタンス メソッドまたは <xref:System.DateTimeOffset.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> インスタンス メソッドを呼び出します。 `format` パラメーターとして文字列 "dddd" を渡します。 曜日名を取得するカルチャを表す <xref:System.Globalization.CultureInfo> または <xref:System.Globalization.DateTimeFormatInfo> オブジェクトのいずれかを`provider` パラメーターとして渡します。 次のコードは、es-ES カルチャを表す <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29> オブジェクトを使用した <xref:System.Globalization.CultureInfo> メソッドの呼び出しを示します。  
  
         [!code-csharp[Formatting.Howto.WeekdayName#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/fullname5.cs#5)]
         [!code-vb[Formatting.Howto.WeekdayName#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/fullname5.vb#5)]  
  
## <a name="example"></a>例  
 この例は、<xref:System.DateTime.DayOfWeek%2A?displayProperty=nameWithType> プロパティと<xref:System.DateTimeOffset.DayOfWeek%2A?displayProperty=nameWithType> プロパティ、および<xref:System.DateTime.ToString%2A?displayProperty=nameWithType> メソッドと <xref:System.DateTimeOffset.ToString%2A?displayProperty=nameWithType> メソッドを呼び出し、特定の日付の曜日を表す番号、曜日の省略名、および正式な名前を取得します。  
  
 [!code-csharp[Formatting.Howto.WeekdayName#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/example6.cs#6)]
 [!code-vb[Formatting.Howto.WeekdayName#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/example6.vb#6)]  
  
 それぞれの言語には、.NET Framework の機能と重複する機能または補足する機能が存在していることがあります。 たとえば Visual Basic には次の 2 つの関数があります。  
  
- `Weekday`: 特定の日付の曜日を示す番号を返します。 この関数では週の初日の序数値が 1 ですが、<xref:System.DateTime.DayOfWeek%2A?displayProperty=nameWithType> プロパティでは週の初日の序数値はゼロです。  
  
- `WeekdayName`: 現在のカルチャで、特定の曜日番号に対応する曜日名を返します。  
  
 次の例は、Visual Basic の `Weekday` 関数と `WeekdayName` 関数の使い方を示します。  
  
 [!code-vb[Formatting.HowTo.WeekdayName#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/example9.vb#9)]  
  
 また、特定の日付の曜日名を取得するには <xref:System.DateTime.DayOfWeek%2A?displayProperty=nameWithType> プロパティから返される値も使用できます。 このためには、プロパティから返された <xref:System.Enum.ToString%2A> 値に対して <xref:System.DayOfWeek> メソッドを呼び出す操作だけが必要です。 ただしこの手法では、次の例に示すように、現在のカルチャでのローカライズされた曜日名は作成されません。  
  
 [!code-csharp[Formatting.HowTo.WeekdayName#8](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/Howto1.cs#8)]
 [!code-vb[Formatting.HowTo.WeekdayName#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/Howto1.vb#8)]  
  
## <a name="see-also"></a>参照

- [書式設定操作の実行](../../../docs/standard/base-types/performing-formatting-operations.md)
- [標準の日時形式文字列](../../../docs/standard/base-types/standard-date-and-time-format-strings.md)
- [カスタム日時形式文字列](../../../docs/standard/base-types/custom-date-and-time-format-strings.md)
