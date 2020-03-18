---
title: '方法 : グレゴリオ暦以外の暦の日付を表示する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- formatting [.NET Framework], dates
- dates [.NET Framework], formatting
- calendars [.NET Framework], displaying dates
- displaying date and time data
ms.assetid: ed324eff-4aff-4a76-b6c0-04e6c0d8f5a9
ms.openlocfilehash: 455996d091f92367667e7077a4524898cd8face6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73138750"
---
# <a name="how-to-display-dates-in-non-gregorian-calendars"></a>方法 : グレゴリオ暦以外の暦の日付を表示する
<xref:System.DateTime> 型と <xref:System.DateTimeOffset> 型は既定の暦としてグレゴリオ暦を使用しています。 つまり、日付と時刻値の `ToString` メソッドを呼び出すと、その日付の時刻が別の暦を使用して作成された場合でも、その日付の時刻はグレゴリオ暦の文字列形式で表示されます。 これを次の例で示します。この例では、2 つの方法を使用してペルシャ暦で日付と時刻の値を作成していますが、<xref:System.DateTime.ToString%2A> メソッドを呼び出すと、これらの日付と時刻の値はグレゴリオ暦で表示されます。 この例では、一般的に使われているものの、特定の暦で日付を表示するには正しくない 2 つの手法が反映されています。  
  
 [!code-csharp[Formatting.HowTo.Calendar#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.Calendar/cs/Calendar1.cs#1)]
 [!code-vb[Formatting.HowTo.Calendar#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.Calendar/vb/Calendar1.vb#1)]  
  
 2 つの手法は、特定の暦で日付を表示するために使用できます。 1 番目の手法では、暦は特定のカルチャの既定の暦である必要があります。 2 番目の手法は、任意の暦で使用できます。  
  
### <a name="to-display-the-date-for-a-cultures-default-calendar"></a>カルチャの既定の暦の日付を表示するには  
  
1. 使用する暦を表す <xref:System.Globalization.Calendar> クラスから派生した暦オブジェクトをインスタンス化します。  
  
2. 日付を表示するために使用される書式のカルチャを表す <xref:System.Globalization.CultureInfo> オブジェクトをインスタンス化します。  
  
3. <xref:System.Array.Exists%2A?displayProperty=nameWithType> メソッドを呼び出し、暦オブジェクトが <xref:System.Globalization.CultureInfo.OptionalCalendars%2A?displayProperty=nameWithType> プロパティによって返される配列のメンバーかどうかを判断します。 これは、暦が <xref:System.Globalization.CultureInfo> オブジェクトの既定の暦として使用できることを示します。 配列のメンバーでない場合は、「任意の暦で日付を表示するには」セクションの手順に従います。  
  
4. <xref:System.Globalization.DateTimeFormatInfo.Calendar%2A> プロパティから返される <xref:System.Globalization.DateTimeFormatInfo> オブジェクトの <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType> プロパティに暦オブジェクトを割り当てます。  
  
    > [!NOTE]
    > <xref:System.Globalization.CultureInfo> クラスには <xref:System.Globalization.CultureInfo.Calendar%2A> プロパティもあります。 ただし、これは読み取り専用で定数のため、<xref:System.Globalization.DateTimeFormatInfo.Calendar%2A?displayProperty=nameWithType> プロパティに割り当てられた新しい既定の暦を反映するために変更されることはありません。  
  
5. <xref:System.DateTime.ToString%2A> と <xref:System.DateTime.ToString%2A> メソッドのいずれかを呼び出し、前の手順で既定の暦を変更した <xref:System.Globalization.CultureInfo> オブジェクトを渡します。  
  
### <a name="to-display-the-date-in-any-calendar"></a>任意の暦で日付を表示するには  
  
1. 使用する暦を表す <xref:System.Globalization.Calendar> クラスから派生した暦オブジェクトをインスタンス化します。  
  
2. 日付と時刻の値の文字列形式で表示する日付と時刻の要素を決定します。  
  
3. 表示する日付と時刻の要素ごとに、暦オブジェクトの `Get` メソッドをオーバーライドします。 次のメソッドが使用できます。  
  
    - <xref:System.Globalization.Calendar.GetYear%2A>: 適切な暦で年を表示します。  
  
    - <xref:System.Globalization.Calendar.GetMonth%2A>: 適切な暦で月を表示します。  
  
    - <xref:System.Globalization.Calendar.GetDayOfMonth%2A>: 適切な暦で月の日付を表示します。  
  
    - <xref:System.Globalization.Calendar.GetHour%2A>: 適切な暦で日の時間を表示します。  
  
    - <xref:System.Globalization.Calendar.GetMinute%2A>: 適切な暦で時間の分を表示します。  
  
    - <xref:System.Globalization.Calendar.GetSecond%2A>: 適切な暦で分の秒を表示します。  
  
    - <xref:System.Globalization.Calendar.GetMilliseconds%2A>: 適切な暦で秒のミリ秒を表示します。  
  
## <a name="example"></a>例  
 この例では、2 つの異なる暦を使用して日付を表示します。 ar-JO カルチャの既定の暦としてイスラム暦を定義した後の日付を表示し、その日付を fa-IR カルチャでオプションの暦としてサポートされていないペルシャ暦を使用して表示します。  
  
 [!code-csharp[Formatting.HowTo.Calendar#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.Calendar/cs/Calendar1.cs#2)]
 [!code-vb[Formatting.HowTo.Calendar#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.Calendar/vb/Calendar1.vb#2)]  
  
 各 <xref:System.Globalization.CultureInfo> オブジェクトは、<xref:System.Globalization.CultureInfo.OptionalCalendars%2A> プロパティに示されている 1 つ以上の暦をサポートできます。 これらのいずれかがカルチャの既定の暦として指定され、読み取り専用の <xref:System.Globalization.CultureInfo.Calendar%2A?displayProperty=nameWithType> プロパティによって返されます。 オプションの暦のもう 1 つは、その暦を表す <xref:System.Globalization.Calendar> オブジェクトを <xref:System.Globalization.DateTimeFormatInfo.Calendar%2A?displayProperty=nameWithType> プロパティによって返された <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType> プロパティに割り当てることで、既定値として指定することができます。 ただし、<xref:System.Globalization.PersianCalendar> クラスによって表されるペルシャ暦などの一部の暦は、どのカルチャのオプションの暦としても機能しません。  
  
 例では、特定の暦を使用して日付の文字列形式を生成する詳細の多くを処理するため、再利用可能な暦ユーティリティ クラス `CalendarUtility` を定義しています。 `CalendarUtility` クラスには次のメンバーがあります。  
  
- パラメーター化されたコンストラクター。その単一のパラメーターが <xref:System.Globalization.Calendar> オブジェクトで、この中で日付が表示されます。 これは、クラスのプライベート フィールドに割り当てられます。  
  
- `CalendarExists` は、`CalendarUtility` オブジェクトによって表される暦が、パラメーターとしてメソッドに渡される <xref:System.Globalization.CultureInfo> オブジェクトによってサポートされているかどうかを示すブール値を返すプライベート メソッドです。 このメソッドは、<xref:System.Array.Exists%2A?displayProperty=nameWithType> 配列が渡される <xref:System.Globalization.CultureInfo.OptionalCalendars%2A?displayProperty=nameWithType> メソッドの呼び出しをラップします。  
  
- `HasSameName` は、パラメーターとして <xref:System.Predicate%601> メソッドに渡される <xref:System.Array.Exists%2A?displayProperty=nameWithType> デリゲートに割り当てられるプライベート メソッドです。 メソッドが `true` を返すまで、配列の各メンバーがメソッドに渡されます。 このメソッドは、オプションの暦の名前が `CalendarUtility` オブジェクトによって表される暦と同じかどうかを判断します。  
  
- `DisplayDate` は、2 つのパラメーターに渡されるオーバーロードされたパブリック メソッドです。<xref:System.DateTime> または <xref:System.DateTimeOffset> のいずれかの値を `CalendarUtility` オブジェクトによって表される暦で表し、カルチャの書式指定規則が使用されます。 日付の文字列表現を返す際の動作は、ターゲットの暦が、使用される書式指定規則のカルチャでサポートされているかどうかによって異なります。  
  
 この例では、<xref:System.DateTime> または <xref:System.DateTimeOffset> 値を作成するために使用する暦に関係なく、その値は通常、グレゴリオ暦の日付として表現されます。 これは、<xref:System.DateTime> と <xref:System.DateTimeOffset> 型は、どの暦情報も保持しないからです。 これらは内部的に 0001 年 1 月 1 日の午前 0 時から経過したタイマー刻みの数として表されます。 その数の解釈は、暦に依存します。 ほとんどのカルチャでは、既定の暦はグレゴリオ暦です。  
  
## <a name="see-also"></a>参照

- [書式設定操作の実行](../../../docs/standard/base-types/performing-formatting-operations.md)
