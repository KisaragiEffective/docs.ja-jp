---
title: 日付と時刻を使用した算術演算の実行
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- times [.NET Framework], arithmetic operations
- dates [.NET Framework], arithmetic operations
- time zones [.NET Framework], arithmetic operations
- arithmetic operations [.NET Framework], dates and times
- dates [.NET Framework], comparing
- DateTime structure, arithmetic operations
- DateTimeOffset structure, arithmetic operations
ms.assetid: 87c7ddf2-f15e-48af-8602-b3642237e6d0
ms.openlocfilehash: 2e668cf3f909ed4b89f05ca63dfe69051c1d9066
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73122259"
---
# <a name="performing-arithmetic-operations-with-dates-and-times"></a>日付と時刻を使用した算術演算の実行

<xref:System.DateTime> と <xref:System.DateTimeOffset> の構造体は両方とも、値に対して算術演算を実行するメンバーを提供しますが、算術演算の結果は大きく異なります。 このトピックでは、これらの相違点を確認し、日付と時刻のデータのタイムゾーン認識の程度に関連付けて、日付と時刻のデータを使用して完全なタイムゾーン対応操作を実行する方法について説明します。

## <a name="comparisons-and-arithmetic-operations-with-datetime-values"></a>DateTime 値を使用した比較と算術演算

<xref:System.DateTime.Kind%2A?displayProperty=nameWithType> プロパティを使用すると、日付と時刻に <xref:System.DateTimeKind> 値を割り当てて、現地時刻、世界協定時刻 (UTC)、または指定されていないタイムゾーンの時刻を表すかどうかを示すことができます。 ただし、この制限されたタイムゾーン情報は、<xref:System.DateTimeKind> 値の日付と時刻の演算を比較または実行するときには無視されます。 次の例では、現在の現地時刻と現在の UTC 時刻を比較することで、この問題を示しています。

[!code-csharp[System.DateTimeOffset.Conceptual#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/cs/Conceptual2.cs#2)]
[!code-vb[System.DateTimeOffset.Conceptual#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/vb/Conceptual2.vb#2)]

<xref:System.DateTime.CompareTo%28System.DateTime%29> メソッドは、現地時刻が UTC 時刻よりも前 (またはそれ未満) であることを報告します。減算操作は、UTC と米国太平洋標準時ゾーンのシステムの現地時刻との差が7時間であることを示します。 しかし、これら 2 つの値は、ある特定の時点を異なる表現で示したものであるため、この場合、時間差の原因は、UTC を基準としたローカル タイム ゾーンのオフセットにあることは明らかです。

一般に、<xref:System.DateTime.Kind%2A?displayProperty=nameWithType> プロパティは、<xref:System.DateTime.Kind> の比較および算術メソッドによって返される結果には影響しません (2 つの同じ時点の比較が示されます)。ただし、これらの結果の解釈に影響する可能性があります。 (例:

- 2つの日付と時刻の値に対して実行された、2つの値の間の実際の時間間隔を反映する <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> <xref:System.DateTimeKind> プロパティを持つ、算術演算の結果。 同様に、そのような 2 つの日付と時刻の値を比較した結果には、2 つの時間の関係が正確に反映されます。

- 2つの日付と時刻の値に対して実行される算術演算または比較演算の結果では、<xref:System.DateTime.Kind%2A?displayProperty=nameWithType> プロパティの両方が <xref:System.DateTimeKind> 同じであるか、または異なる <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> プロパティ値を持つ2つの日付と時刻の値が、2つの間のクロック時間の差を反映しています。値.

- ローカルの日付と時刻の値に対する算術演算または比較操作では、特定の値があいまいであるかや、無効であるかどうかは考慮されず、ローカル タイム ゾーンでの夏時間の開始や終了による調整規則の影響についても考慮されません。

- UTC と現地時刻の差を比較または計算する操作の結果には、UTC を基準としたローカル タイム ゾーンのオフセットに等しい時間差が含まれます。

- タイム ゾーンが指定されていない時刻と UTC または現地時刻との差を比較または計算する操作では、単純な時刻差が反映されます。 タイム ゾーンの違いは考慮されず、結果にはタイム ゾーン調整規則の適用が反映されません。

- タイム ゾーンが指定されていない 2 つの時刻の差を比較または計算する操作の結果には、2 つの異なるタイム ゾーンの時刻の差を反映した未知の時間差が含まれる場合があります。

タイムゾーンの違いが日付と時刻の計算に影響しないシナリオは多数あります (これらのいくつかの詳細については、「 [DateTime、DateTimeOffset、TimeSpan、TimeZoneInfo の選択](../../../docs/standard/datetime/choosing-between-datetime.md)」を参照してください)。または日付と時刻のデータのコンテキストについても説明します。比較または算術演算の意味を定義します。

## <a name="comparisons-and-arithmetic-operations-with-datetimeoffset-values"></a>DateTimeOffset 値を使用した比較と算術演算

<xref:System.DateTimeOffset> 値には、日付と時刻だけでなく、UTC を基準とする日付と時刻を明確に定義するオフセットも含まれます。 これにより、<xref:System.DateTimeOffset> の値とは異なる方法で等しいかどうかを定義できます。 <xref:System.DateTime> の値は同じ日付と時刻の値を持つ場合は同じですが、両者が同じ時点を参照している場合、<xref:System.DateTimeOffset> 値は等しいと見なされます。 これにより、<xref:System.DateTimeOffset> の値の精度が向上し、比較で使用する場合は解釈が不要になり、2つの日付と時刻の間の間隔を決定するほとんどの算術演算にもなります。 次の例は、ローカルと UTC <xref:System.DateTimeOffset> の値を比較した前の例と同等の <xref:System.DateTimeOffset> であり、動作の違いを示しています。

[!code-csharp[System.DateTimeOffset.Conceptual#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/cs/Conceptual3.cs#3)]
[!code-vb[System.DateTimeOffset.Conceptual#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/vb/Conceptual3.vb#3)]

この例では、<xref:System.DateTimeOffset.CompareTo%2A> メソッドは、現在の現地時刻と現在の UTC 時刻が等しいことを示し、<xref:System.DateTimeOffset.CompareTo(System.DateTimeOffset)> 値を減算することで、2つの時刻の差が <xref:System.TimeSpan.Zero?displayProperty=nameWithType>ことを示します。

日付と時刻の演算で <xref:System.DateTimeOffset> 値を使用する場合の主な制限事項は、<xref:System.DateTimeOffset> 値にはタイムゾーンの認識があるものの、完全なタイムゾーンを認識していないことです。 <xref:System.DateTimeOffset> 値のオフセットは、<xref:System.DateTimeOffset> 変数に最初に値が割り当てられたときの UTC からのタイムゾーンのオフセットを反映しますが、それ以降はタイムゾーンとの関連付けが解除されます。 特定可能な時刻との直接の関連付けは失われるため、日付と時刻の間隔に対して加算や減算を行っても、タイム ゾーンの調整規則は考慮されません。

例として、米国中部標準時ゾーンの夏時間への移行は午前2:00 に行われます。 前のコードと似ています。 つまり、中部標準時の 2008 年 3 月 9 日の午前 1 時 30 分に 2 時間 30 分の間隔を加算すると、日付と時刻は 2008 年 3 月 9 日の 前のコードと似ています。 しかし、次の例に示すように、加算の結果は 2008 年 3 月 9 日の 前のコードと似ています。 この操作の結果は正しい時点を表していますが、目的のタイム ゾーンの時刻ではありません (つまり、予想されるタイム ゾーンのオフセットが適用されていません)。

[!code-csharp[System.DateTimeOffset.Conceptual#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/cs/Conceptual4.cs#4)]
[!code-vb[System.DateTimeOffset.Conceptual#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/vb/Conceptual4.vb#4)]

## <a name="arithmetic-operations-with-times-in-time-zones"></a>タイムゾーンの時刻を使用した算術演算

<xref:System.TimeZoneInfo> クラスには、あるタイムゾーンから別のタイムゾーンに時刻を変換するときに調整を自動的に適用する多くの変換メソッドが含まれています。 次に例を示します。

- <xref:System.TimeZoneInfo.ConvertTime%2A> メソッドと <xref:System.TimeZoneInfo.ConvertTimeBySystemTimeZoneId%2A> メソッド。2つのタイムゾーン間で時刻を変換します。

- <xref:System.TimeZoneInfo.ConvertTimeFromUtc%2A> メソッドと <xref:System.TimeZoneInfo.ConvertTimeToUtc%2A> メソッドは、特定のタイムゾーンの時刻を UTC に変換するか、UTC を特定のタイムゾーンの時刻に変換します。

詳細については、「[タイムゾーン間での時刻の変換](../../../docs/standard/datetime/converting-between-time-zones.md)」を参照してください。

<xref:System.TimeZoneInfo.ConvertTimeToUtc(System.DateTime)> クラスには、日付と時刻の演算を実行するときに調整規則を自動的に適用するメソッドは用意されていません。 ただし、あるタイム ゾーンの時刻を UTC に変換してから算術演算を実行し、その後 UTC から元のタイム ゾーンの時刻に再変換することで、調整規則を適用したときと同じ結果を得ることができます。 詳細については、「[方法: 日付と時刻の演算でタイムゾーンを使用する](../../../docs/standard/datetime/use-time-zones-in-arithmetic.md)」を参照してください。

たとえば、次のコードは、2008 年 3 月 9 日の午前 2 時に 2 時間 30 分を加算する 前のコードと似ています。 ただし、中部標準時を UTC に変換した後に日付と時刻の算術演算を実行し、その結果を UTC から中部標準時に変換するため、得られた時刻は中部標準時タイム ゾーンの夏時間への移行を反映しています。

[!code-csharp[System.DateTimeOffset.Conceptual#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/cs/Conceptual5.cs#5)]
[!code-vb[System.DateTimeOffset.Conceptual#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/vb/Conceptual5.vb#5)]

## <a name="see-also"></a>関連項目

- [日付、時刻およびタイム ゾーン](../../../docs/standard/datetime/index.md)
- [方法: 日付と時刻の演算でタイム ゾーンを使用する](../../../docs/standard/datetime/use-time-zones-in-arithmetic.md)
