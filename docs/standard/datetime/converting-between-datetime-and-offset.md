---
title: DateTime と DateTimeOffset 間の変換
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DateTime structure, converting
- time zones [.NET Framework], conversions
- UTC times, converting
- DateTimeOffset structure, converting
- converting DateTimeOffset and DateTime values
- dates [.NET Framework], converting
- converting times
- Date data type, converting
- local time conversions
ms.assetid: b605ff97-0c45-4c24-833f-4c6a3e8be64c
ms.openlocfilehash: 5c19296f75e9e002e88263c5e5efa9917e185ebc
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78156037"
---
# <a name="converting-between-datetime-and-datetimeoffset"></a>DateTime と DateTimeOffset 間の変換

<xref:System.DateTimeOffset> 構造体は、<xref:System.DateTime> の構造よりも高い範囲のタイムゾーンを認識しますが、<xref:System.DateTime> のパラメーターは、メソッドの呼び出しでよく使用されます。 このため、<xref:System.DateTimeOffset> 値を <xref:System.DateTime> 値に変換したり、その逆の変換を行ったりすることは特に重要です。 このトピックでは、できるだけ多くのタイムゾーン情報を保持する方法でこれらの変換を実行する方法について説明します。

> [!NOTE]
> <xref:System.DateTime> と <xref:System.DateTimeOffset> の両方の型には、タイムゾーンの時刻を表すときにいくつかの制限があります。 <xref:System.DateTime.Kind%2A> プロパティを使用すると、<xref:System.DateTime> は世界協定時刻 (UTC) とシステムのローカルタイムゾーンのみを反映できます。 <xref:System.DateTimeOffset> は UTC からの時刻のオフセットを反映しますが、そのオフセットが属する実際のタイムゾーンは反映されません。 時刻の値とタイムゾーンのサポートの詳細については、「 [DateTime、DateTimeOffset、TimeSpan、および TimeZoneInfo の使い分け](../../../docs/standard/datetime/choosing-between-datetime.md)」を参照してください。

## <a name="conversions-from-datetime-to-datetimeoffset"></a>DateTime から DateTimeOffset への変換

<xref:System.DateTimeOffset> 構造体は、ほとんどの変換に適した変換を <xref:System.DateTimeOffset> するために <xref:System.DateTime> を実行する2つの同等の方法を提供します。

- <xref:System.DateTimeOffset.%23ctor%2A> コンストラクター。 <xref:System.DateTime> 値に基づいて新しい <xref:System.DateTimeOffset> オブジェクトを作成します。

- 暗黙的な変換演算子。 <xref:System.DateTime> 値を <xref:System.DateTimeOffset> オブジェクトに割り当てることができます。

UTC およびローカル <xref:System.DateTime> 値の場合、結果の <xref:System.DateTimeOffset> 値の <xref:System.DateTimeOffset.Offset%2A> プロパティに UTC またはローカルタイムゾーンオフセットが正確に反映されます。 たとえば、次のコードは、UTC 時刻をそれと等価な <xref:System.DateTimeOffset> 値に変換します。

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#1)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#1)]

この場合、`utcTime2` 変数のオフセットは 00:00 です。 同様に、次のコードは、現地時刻をそれと等価な <xref:System.DateTimeOffset> 値に変換します。

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#2)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#2)]

ただし、<xref:System.DateTime.Kind%2A> プロパティが <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType>されている <xref:System.DateTime> 値については、これらの2つの変換メソッドによって <xref:System.DateTimeOffset> 値が生成されます。この値は、ローカルタイムゾーンのオフセットとなります。 これを次の例に示します。この例は、米国太平洋標準時ゾーンで実行されています。

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#3)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#3)]

<xref:System.DateTime> 値にローカルタイムゾーンまたは UTC 以外の日付と時刻が反映されている場合は、オーバーロードされた <xref:System.DateTimeOffset.%23ctor%2A> コンストラクターを呼び出すことによって、<xref:System.DateTimeOffset> 値に変換し、そのタイムゾーン情報を保持することができます。 たとえば、次の例では、中部標準時を反映する <xref:System.DateTimeOffset> オブジェクトをインスタンス化しています。

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#4)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#4)]

このコンストラクターオーバーロードへの2番目のパラメーターは、時刻の UTC からのオフセットを表す <xref:System.TimeSpan> オブジェクトで、時刻に対応するタイムゾーンの <xref:System.TimeZoneInfo.GetUtcOffset%28System.DateTime%29?displayProperty=nameWithType> メソッドを呼び出すことによって取得する必要があります。 メソッドの1つのパラメーターは、変換する日付と時刻を表す <xref:System.DateTime> 値です。 タイム ゾーンで夏時間がサポートされている場合、このパラメーターにより、このメソッドはその特定の日時に対して適切なオフセットを決定できます。

## <a name="conversions-from-datetimeoffset-to-datetime"></a>DateTimeOffset から DateTime への変換

<xref:System.DateTimeOffset.DateTime%2A> プロパティは、変換を <xref:System.DateTime> <xref:System.DateTimeOffset> を実行するために最も一般的に使用されます。 ただし、次の例に示すように <xref:System.DateTime.Kind%2A> プロパティが <xref:System.DateTimeKind.Unspecified>の <xref:System.DateTime> 値が返されます。

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#5)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#5)]

これは、<xref:System.DateTimeOffset.DateTime%2A> プロパティを使用するときに、変換によって <xref:System.DateTimeOffset> 値の UTC との関係に関するすべての情報が失われることを意味します。 これは、<xref:System.DateTime.Kind%2A> プロパティの2つのタイムゾーンのみを反映する <xref:System.DateTimeOffset.DateTime%2A> 構造体であるため、UTC 時刻またはシステムの現地時刻に対応する <xref:System.DateTimeOffset> 値に影響します。

<xref:System.DateTimeOffset> を <xref:System.DateTime> 値に変換するときにできるだけ多くのタイムゾーン情報を保持するには、<xref:System.DateTimeOffset.UtcDateTime%2A?displayProperty=nameWithType> プロパティと <xref:System.DateTimeOffset.LocalDateTime%2A?displayProperty=nameWithType> プロパティを使用できます。

### <a name="converting-a-utc-time"></a>UTC 時刻の変換

変換された <xref:System.DateTimeOffset.DateTime%2A> 値が UTC 時刻であることを示すには、<xref:System.DateTimeOffset.UtcDateTime%2A?displayProperty=nameWithType> プロパティの値を取得します。 これは、<xref:System.DateTimeOffset.DateTime%2A> プロパティと次の2つの点で異なります。

- <xref:System.DateTime.Kind%2A> プロパティが <xref:System.DateTimeKind.Utc>の <xref:System.DateTime> 値を返します。

- <xref:System.DateTimeOffset.Offset%2A> プロパティ値が <xref:System.TimeSpan.Zero?displayProperty=nameWithType>と等しくない場合、時刻は UTC に変換されます。

> [!NOTE]
> 変換された <xref:System.DateTime> 値が明確に特定の時点を識別するようにアプリケーションで要求されている場合は、<xref:System.DateTimeOffset.UtcDateTime%2A?displayProperty=nameWithType> プロパティを使用して、<xref:System.DateTime> 変換のすべての <xref:System.DateTimeOffset> を処理することを検討してください。

次のコードでは、<xref:System.DateTimeOffset.UtcDateTime%2A> プロパティを使用して、オフセットが <xref:System.TimeSpan.Zero?displayProperty=nameWithType> である <xref:System.DateTimeOffset> 値を <xref:System.DateTime> の値に変換します。

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#6)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#6)]

次のコードでは、<xref:System.DateTimeOffset.UtcDateTime%2A> プロパティを使用して、<xref:System.DateTimeOffset> 値に対してタイムゾーン変換と型変換の両方を実行します。

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#12](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#12)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#12)]

### <a name="converting-a-local-time"></a>現地時刻の変換

<xref:System.DateTimeOffset> 値が現地時刻を表すことを示すには、<xref:System.DateTimeOffset.DateTime%2A?displayProperty=nameWithType> プロパティによって返された <xref:System.DateTime> 値を `static` (`Shared` で Visual Basic) <xref:System.DateTime.SpecifyKind%2A> メソッドに渡すことができます。 メソッドは、最初のパラメーターとして渡された日付と時刻を返しますが、<xref:System.DateTime.Kind%2A> のプロパティは、2番目のパラメーターで指定した値に設定されます。 次のコードでは、オフセットがローカルタイムゾーンのオフセットに対応する <xref:System.DateTimeOffset> 値を変換するときに、<xref:System.DateTime.SpecifyKind%2A> メソッドを使用しています。

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#7](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#7)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#7)]

また、<xref:System.DateTimeOffset.LocalDateTime%2A?displayProperty=nameWithType> プロパティを使用して、<xref:System.DateTimeOffset> 値をローカルの <xref:System.DateTime> 値に変換することもできます。 返された <xref:System.DateTime> 値の <xref:System.DateTime.Kind%2A> プロパティは <xref:System.DateTimeKind.Local>です。 次のコードでは、オフセットがローカルタイムゾーンのオフセットに対応する <xref:System.DateTimeOffset> 値を変換するときに、<xref:System.DateTimeOffset.LocalDateTime%2A?displayProperty=nameWithType> プロパティを使用しています。

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#10](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#10)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#10)]

<xref:System.DateTimeOffset.LocalDateTime%2A?displayProperty=nameWithType> プロパティを使用して <xref:System.DateTime> 値を取得すると、プロパティの `get` アクセサーは、最初に <xref:System.DateTimeOffset> 値を UTC に変換し、次に <xref:System.DateTimeOffset.ToLocalTime%2A> メソッドを呼び出してローカル時刻に変換します。 これは、<xref:System.DateTimeOffset.LocalDateTime%2A?displayProperty=nameWithType> プロパティから値を取得して、型変換を実行するときと同時にタイムゾーン変換を実行できることを意味します。 また、変換の実行にローカル タイム ゾーンの調整規則が適用されることも意味します。 次のコードは、<xref:System.DateTimeOffset.LocalDateTime%2A?displayProperty=nameWithType> プロパティを使用して、型とタイムゾーン変換の両方を実行する方法を示しています。

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#11](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#11)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#11)]

### <a name="a-general-purpose-conversion-method"></a>汎用変換メソッド

次の例では、<xref:System.DateTimeOffset> 値を <xref:System.DateTime> 値に変換する `ConvertFromDateTimeOffset` という名前のメソッドを定義します。 オフセットに基づいて、<xref:System.DateTimeOffset> 値が UTC 時刻、現地時刻、またはその他の時刻であるかどうかを判断し、返された日付と時刻の値の <xref:System.DateTime.Kind%2A> プロパティをそれに応じて定義します。

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#8](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#8)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#8)]

次の例では、`ConvertFromDateTimeOffset` メソッドを呼び出して、UTC 時刻、現地時刻、および米国中部標準時ゾーンの時刻を表す <xref:System.DateTimeOffset> 値を変換します。

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#9](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#9)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#9)]

このコードは次の 2 つのことを前提としますが、アプリケーションおよびその日時値のソースによっては常に有効とは限らないことに注意してください。

- オフセットが <xref:System.TimeSpan.Zero?displayProperty=nameWithType> 日付と時刻の値が UTC を表すことを前提としています。 実際には、UTC は特定のタイム ゾーンの時刻ではなく、世界のタイム ゾーンの時刻を標準化する際に基準となる時刻です。 タイムゾーンに <xref:System.TimeSpan.Zero>のオフセットを設定することもできます。

- オフセットがローカル タイム ゾーンのオフセットと等しい日時が、ローカル タイム ゾーンを表すことを前提とします。 日時値は元のタイム ゾーンとの関連付けが解除されているので、これは該当しない場合があります。日時は、同じオフセットを持つ別のタイム ゾーンに由来する可能性があります。

## <a name="see-also"></a>参照

- [日付、時刻およびタイム ゾーン](../../../docs/standard/datetime/index.md)
