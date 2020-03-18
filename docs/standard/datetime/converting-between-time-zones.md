---
title: タイム ゾーン間での時刻の変換
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- times [.NET Framework], converting
- time zones [.NET Framework], conversions
- UTC times, converting
- converting times
- local time conversions
ms.assetid: a51e1a3b-c983-4320-b31a-1f9fa3cf824a
ms.openlocfilehash: fbb59dbe364763209f44a4e2241d1d5275036c40
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78156024"
---
# <a name="converting-times-between-time-zones"></a>タイム ゾーン間での時刻の変換

日時を使用するすべてのアプリケーションで、タイム ゾーン間の違いを処理することの重要性が高まっています。 アプリケーションでは、すべての時刻が現地時刻で表されると想定できなくなりました。これは、<xref:System.DateTime> 構造体から使用可能な時間です。 たとえば、米国東部の現在の時刻を表示する Web ページには、東アジアの顧客に対する信頼性を欠いています。 このトピックでは、あるタイムゾーンから別のタイムゾーンに時刻を変換する方法と、タイムゾーンを認識する期間が限られている <xref:System.DateTimeOffset> 値を変換する方法について説明します。

## <a name="converting-to-coordinated-universal-time"></a>世界協定時刻への変換

世界協定時刻 (UTC) は、高精度の原子時標準です。 世界のタイム ゾーンは、UTC からの正または負のオフセットとして表現されます。 したがって、UTC はタイム ゾーンの影響を受けない、またはタイム ゾーンに依存しない種類の時刻を提供します。 コンピューター間の日時の移植性が重要となる場合には、UTC 時刻の使用が推奨されます。 (日付と時刻を使用した詳細およびその他のベストプラクティスについては、「 [.NET Framework での DateTime を使用したコーディングのベストプラクティス](https://docs.microsoft.com/previous-versions/dotnet/articles/ms973825(v=msdn.10))」を参照してください)。個々のタイムゾーンを UTC に変換すると、時間の比較が簡単になります。

> [!NOTE]
> また、<xref:System.DateTimeOffset> 構造体をシリアル化して、特定の時点を明確に表すこともできます。 <xref:System.DateTimeOffset> のオブジェクトは日付と時刻の値を UTC からのオフセットと共に格納するので、UTC との関係において常に特定の時点を表します。

時刻を UTC に変換する最も簡単な方法は、`static` (`Shared` Visual Basic) <xref:System.TimeZoneInfo.ConvertTimeToUtc%28System.DateTime%29?displayProperty=nameWithType> メソッドを呼び出すことです。 メソッドによって実行される正確な変換は、次の表に示すように、`dateTime` パラメーターの <xref:System.DateTime.Kind%2A> プロパティの値によって異なります。

| `DateTime.Kind`            | 変換                                                                     |
| -------------------------- | ------------------------------------------------------------------------------ |
| `DateTimeKind.Local`       | 現地時刻を UTC に変換します。                                                    |
| `DateTimeKind.Unspecified` | `dateTime` パラメーターが現地時刻であることを前提とし、現地時刻を UTC に変換します。 |
| `DateTimeKind.Utc`         | `dateTime` パラメーターを変更せずに返します。                                    |

次のコードは、現在の現地時刻を UTC に変換し、コンソールに結果を表示します。

[!code-csharp[System.TimeZone2.Concepts#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#6)]
[!code-vb[System.TimeZone2.Concepts#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#6)]

日付と時刻の値が現地時刻と UTC のどちらも表さない場合、<xref:System.DateTime.ToUniversalTime%2A> メソッドは間違った結果を返す可能性があります。 ただし、<xref:System.TimeZoneInfo.ConvertTimeToUtc%2A?displayProperty=nameWithType> メソッドを使用して、指定したタイムゾーンから日付と時刻を変換することができます。 (変換先のタイムゾーンを表す <xref:System.TimeZoneInfo> オブジェクトを取得する方法の詳細については、「[ローカルシステムで定義されているタイムゾーンの検索](../../../docs/standard/datetime/finding-the-time-zones-on-local-system.md)」を参照してください)。次のコードでは、<xref:System.TimeZoneInfo.ConvertTimeToUtc%2A?displayProperty=nameWithType> メソッドを使用して、東部標準時を UTC に変換しています。

[!code-csharp[System.TimeZone2.Concepts#7](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#7)]
[!code-vb[System.TimeZone2.Concepts#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#7)]

<xref:System.DateTime> オブジェクトの <xref:System.DateTime.Kind%2A> プロパティとタイムゾーンが一致しない場合、このメソッドは <xref:System.ArgumentException> をスローすることに注意してください。 <xref:System.DateTime.Kind%2A> プロパティが <xref:System.DateTimeKind.Local?displayProperty=nameWithType> であるにもかかわらず、<xref:System.TimeZoneInfo> オブジェクトがローカルタイムゾーンを表していない場合、または <xref:System.DateTime.Kind%2A> プロパティが <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> でも <xref:System.TimeZoneInfo> オブジェクトが <xref:System.TimeZoneInfo.Utc?displayProperty=nameWithType>と等しくない場合は、不一致が発生します。

これらのメソッドはすべて <xref:System.DateTime> 値をパラメーターとして受け取り、<xref:System.DateTime> 値を返します。 <xref:System.DateTimeOffset> 値の場合、<xref:System.DateTimeOffset> 構造体には、現在のインスタンスの日付と時刻を UTC に変換する <xref:System.DateTimeOffset.ToUniversalTime%2A> インスタンスメソッドがあります。 次の例では、<xref:System.DateTimeOffset.ToUniversalTime%2A> メソッドを呼び出して、現地時刻と他のいくつかの時刻を世界協定時刻 (UTC) に変換します。

[!code-csharp[System.DateTimeOffset.Methods#16](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Methods/cs/Methods.cs#16)]
[!code-vb[System.DateTimeOffset.Methods#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Methods/vb/Methods.vb#16)]

## <a name="converting-utc-to-a-designated-time-zone"></a>UTC から指定したタイム ゾーンへの変換

UTC を現地時刻に変換するには、後述の「UTC からローカル時刻への変換」を参照してください。 UTC を、指定した任意のタイムゾーンの時刻に変換するには、<xref:System.TimeZoneInfo.ConvertTimeFromUtc%2A> メソッドを呼び出します。 メソッドには、

- 変換対象の UTC。 <xref:System.DateTime.Kind%2A> プロパティが `Unspecified` または `Utc`に設定されている <xref:System.DateTime> 値を指定する必要があります。

- UTC の変換先のタイム ゾーン。

次のコードは、UTC を中部標準時に変換します。

[!code-csharp[System.TimeZone2.Concepts#8](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#8)]
[!code-vb[System.TimeZone2.Concepts#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#8)]

## <a name="converting-utc-to-local-time"></a>UTC から現地時刻への変換

UTC を現地時刻に変換するには、時刻を変換する <xref:System.DateTime> オブジェクトの <xref:System.DateTime.ToLocalTime%2A> メソッドを呼び出します。 メソッドの正確な動作は、次の表に示すように、オブジェクトの <xref:System.DateTime.Kind%2A> プロパティの値によって異なります。

| `DateTime.Kind`            | 変換                                                                               |
| -------------------------- | ---------------------------------------------------------------------------------------- |
| `DateTimeKind.Local`       | <xref:System.DateTime> 値を変更せずに返します。                                      |
| `DateTimeKind.Unspecified` | <xref:System.DateTime> 値が UTC であると想定し、UTC を現地時刻に変換します。 |
| `DateTimeKind.Utc`         | <xref:System.DateTime> 値を現地時刻に変換します。                                 |

> [!NOTE]
> <xref:System.TimeZone.ToLocalTime%2A?displayProperty=nameWithType> メソッドは、`DateTime.ToLocalTime` メソッドと同じように動作します。 1つのパラメーターを受け取ります。これは、変換する日付と時刻の値です。

また、指定したタイムゾーンの時刻をローカル時間に変換するには、`static` (Visual Basic では`Shared`) <xref:System.TimeZoneInfo.ConvertTime%2A?displayProperty=nameWithType> 方法を使用します。 この手法については、次のセクションで説明します。

## <a name="converting-between-any-two-time-zones"></a>任意の 2 つのタイム ゾーン間での変換

<xref:System.TimeZoneInfo> クラスの次の2つの `static` (`Shared` Visual Basic) メソッドのいずれかを使用して、任意の2つのタイムゾーン間で変換を行うことができます。

- <xref:System.TimeZoneInfo.ConvertTime%2A>

  このメソッドのパラメーターは、変換する日付と時刻の値、日付と時刻の値のタイムゾーンを表す `TimeZoneInfo` オブジェクト、および日付と時刻の値を変換するタイムゾーンを表す `TimeZoneInfo` オブジェクトです。

- <xref:System.TimeZoneInfo.ConvertTimeBySystemTimeZoneId%2A>

  このメソッドのパラメーターは、変換する日付と時刻の値、日付と時刻の値のタイムゾーンの識別子、および日付と時刻の値をに変換するタイムゾーンの識別子です。

どちらの方法でも、変換する日付と時刻の値の <xref:System.DateTime.Kind%2A> プロパティと、そのタイムゾーンを表す <xref:System.TimeZoneInfo> オブジェクトまたはタイムゾーン識別子が互いに対応している必要があります。 それ以外の場合は、<xref:System.ArgumentException> がスローされます。 たとえば、日付と時刻の値の `Kind` プロパティが `DateTimeKind.Local`場合、メソッドにパラメーターとして渡された `TimeZoneInfo` オブジェクトが `TimeZoneInfo.Local`と等しくないと、例外がスローされます。 また、メソッドにパラメーターとして渡された識別子が `TimeZoneInfo.Local.Id`と等しくない場合にも例外がスローされます。

次の例では、<xref:System.TimeZoneInfo.ConvertTime%2A> メソッドを使用して、ハワイ標準時から現地時刻に変換します。

[!code-csharp[System.TimeZone2.Concepts#9](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#9)]
[!code-vb[System.TimeZone2.Concepts#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#9)]

## <a name="converting-datetimeoffset-values"></a>DateTimeOffset 値の変換

オブジェクトがインスタンス化されるときにタイムゾーンとの関連付けが解除されるため、<xref:System.DateTimeOffset> オブジェクトによって表される日付と時刻の値は、完全なタイムゾーンに対応していません。 ただし、アプリケーションでは多くの場合、特定のタイム ゾーンの時刻ではなく、単に UTC からの 2 つの異なるオフセットに基づいて日時を変換する必要があります。 この変換を実行するには、現在のインスタンスの <xref:System.DateTimeOffset.ToOffset%2A> メソッドを呼び出すことができます。 メソッドの1つのパラメーターは、メソッドが返す新しい日付と時刻の値のオフセットです。

たとえば、Web ページに対するユーザー要求の日時が既知であり、MM/dd/yyyy hh:mm:ss zzzz の形式で文字列としてシリアル化される場合、次の `ReturnTimeOnServer` メソッドは、この日時値を Web サーバー上の日時に変換します。

[!code-csharp[System.DateTimeOffset.Conceptual.OffsetConversions#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.OffsetConversions/cs/TimeConversions.cs#1)]
[!code-vb[System.DateTimeOffset.Conceptual.OffsetConversions#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.OffsetConversions/vb/TimeConversions.vb#1)]

メソッドに "9/1/2007 5:32:07-05:00" という文字列が渡された場合、その日付と時刻は UTC より5時間前のタイムゾーンで表され、米国太平洋標準時ゾーンにあるサーバーでは 9/1/2007 3:32:07 AM-07:00 が返されます。

<xref:System.TimeZoneInfo> クラスには、<xref:System.DateTimeOffset.ToOffset(System.TimeSpan)> 値を使用してタイムゾーン変換を実行する <xref:System.TimeZoneInfo.ConvertTime%28System.DateTimeOffset%2CSystem.TimeZoneInfo%29?displayProperty=nameWithType> メソッドのオーバーロードも含まれています。 メソッドのパラメーターは、<xref:System.DateTimeOffset> 値と、時刻を変換するタイムゾーンへの参照です。 メソッドの呼び出しで <xref:System.DateTimeOffset> 値が返されます。 たとえば、前の例の `ReturnTimeOnServer` メソッドは、次のように書き直して、<xref:System.TimeZoneInfo.ConvertTime%28System.DateTimeOffset%2CSystem.TimeZoneInfo%29> メソッドを呼び出すことができます。

[!code-csharp[System.DateTimeOffset.Conceptual.OffsetConversions#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.OffsetConversions/cs/timeconversions2.cs#2)]
[!code-vb[System.DateTimeOffset.Conceptual.OffsetConversions#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.OffsetConversions/vb/TimeConversions2.vb#2)]

## <a name="see-also"></a>参照

- <xref:System.TimeZoneInfo>
- [日付、時刻およびタイム ゾーン](../../../docs/standard/datetime/index.md)
- [ローカル システムで定義されているタイム ゾーンの検索](../../../docs/standard/datetime/finding-the-time-zones-on-local-system.md)
