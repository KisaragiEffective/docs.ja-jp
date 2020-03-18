---
title: '方法: 調整規則のあるタイムゾーンを作成する'
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET Framework], creating
- time zones [.NET Framework], and adjustment rules
- adjustment rule [.NET Framework]
ms.assetid: c52ef192-13a9-435f-8015-3b12eae8c47c
ms.openlocfilehash: 4ef3d93746c5688dc15fc7e45d9be054dcfba4c8
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73132553"
---
# <a name="how-to-create-time-zones-with-adjustment-rules"></a>方法: 調整規則のあるタイムゾーンを作成する

アプリケーションで必要とされる正確なタイムゾーン情報は、次のような理由で特定のシステムに存在しない場合があります。

- タイムゾーンがローカルシステムのレジストリに定義されていません。

- タイムゾーンに関するデータは、レジストリから変更または削除されています。

- タイムゾーンには、特定の履歴期間のタイムゾーン調整に関する正確な情報がありません。

このような場合は、<xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> メソッドを呼び出して、アプリケーションで必要とされるタイムゾーンを定義できます。 このメソッドのオーバーロードを使用して、調整規則の有無に関係なくタイムゾーンを作成できます。 タイムゾーンで夏時間がサポートされている場合は、固定調整規則または浮動調整規則のいずれかを使用して調整を定義できます。 (これらの用語の定義については、「タイムゾーンの[概要](../../../docs/standard/datetime/time-zone-overview.md)」の「タイムゾーンの用語」セクションを参照してください)。

> [!IMPORTANT]
> <xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> メソッドを呼び出すことによって作成されたカスタムタイムゾーンは、レジストリには追加されません。 代わりに、<xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> メソッド呼び出しによって返されるオブジェクト参照を使用してのみアクセスできます。

このトピックでは、調整規則を使用してタイムゾーンを作成する方法について説明します。 夏時間調整規則をサポートしていないタイムゾーンを作成するには、「[方法: 調整規則のないタイムゾーンを作成](../../../docs/standard/datetime/create-time-zones-without-adjustment-rules.md)する」を参照してください。

### <a name="to-create-a-time-zone-with-floating-adjustment-rules"></a>浮動調整規則を使用してタイムゾーンを作成するには

1. それぞれの調整について (つまり、特定の期間にわたって標準時間外に移行するたびに)、次の手順を実行します。

    1. タイムゾーン調整の開始遷移時間を定義します。

       <xref:System.TimeZoneInfo.TransitionTime.CreateFloatingDateRule%2A?displayProperty=nameWithType> メソッドを呼び出して、移行の時間を定義する <xref:System.DateTime> 値、遷移の月を定義する整数値、遷移が発生する曜日を定義する整数値、およびを定義する <xref:System.DayOfWeek> 値を渡す必要があります。移行が発生する曜日。 このメソッド呼び出しは、<xref:System.TimeZoneInfo.TransitionTime> オブジェクトをインスタンス化します。

    2. タイムゾーン調整の終了遷移時間を定義します。 これには、<xref:System.TimeZoneInfo.TransitionTime.CreateFloatingDateRule%2A?displayProperty=nameWithType> メソッドの別の呼び出しが必要です。 このメソッド呼び出しは、2つ目の <xref:System.TimeZoneInfo.TransitionTime> オブジェクトをインスタンス化します。

    3. <xref:System.TimeZoneInfo.AdjustmentRule.CreateAdjustmentRule%2A> メソッドを呼び出し、調整の有効な開始日と終了日、遷移の時間を定義する <xref:System.TimeSpan> オブジェクト、および夏時間との間の切り替えが発生するタイミングを定義する2つの <xref:System.TimeZoneInfo.TransitionTime> オブジェクトを渡します。 このメソッド呼び出しは、<xref:System.TimeZoneInfo.AdjustmentRule> オブジェクトをインスタンス化します。

    4. <xref:System.TimeZoneInfo.AdjustmentRule> オブジェクトを <xref:System.TimeZoneInfo.AdjustmentRule> オブジェクトの配列に割り当てます。

2. タイムゾーンの表示名を定義します。 表示名は、標準の形式に準拠しています。この形式では、世界協定時刻 (UTC) からのタイムゾーンのオフセットがかっこで囲まれ、その後にタイムゾーンを識別する文字列、タイムゾーン内の1つ以上の都市、または1つ以上の cou が続きます。タイムゾーンの ntries またはリージョン。

3. タイムゾーンの標準時刻の名前を定義します。 通常、この文字列はタイムゾーンの識別子としても使用されます。

4. タイムゾーンの夏時間の名前を定義します。

5. タイムゾーンの標準名とは異なる識別子を使用する場合は、タイムゾーン識別子を定義します。

6. タイムゾーンの UTC からのオフセットを定義する <xref:System.TimeSpan> オブジェクトをインスタンス化します。 時刻が UTC より遅いタイムゾーンには、正のオフセットがあります。 時刻が UTC より前のタイムゾーンでは、負のオフセットが使用されます。

7. <xref:System.TimeZoneInfo.CreateCustomTimeZone%28System.String%2CSystem.TimeSpan%2CSystem.String%2CSystem.String%2CSystem.String%2CSystem.TimeZoneInfo.AdjustmentRule%5B%5D%29?displayProperty=nameWithType> メソッドを呼び出して、新しいタイムゾーンをインスタンス化します。

## <a name="example"></a>例

次の例では、1918から現在のまでのさまざまな時間間隔の調整規則を含む米国の中部標準時のタイムゾーンを定義します。

[!code-csharp[System.TimeZone2.CreateTimeZone#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/cs/System.TimeZone2.CreateTimeZone.cs#5)]
[!code-vb[System.TimeZone2.CreateTimeZone#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/vb/System.TimeZone2.CreateTimeZone.vb#5)]

この例で作成したタイムゾーンには、複数の調整規則があります。 調整規則の有効な開始日と終了日が、別の調整規則の日付と重複しないように注意する必要があります。 重複がある場合は、<xref:System.InvalidTimeZoneException> がスローされます。

浮動調整規則の場合、<xref:System.TimeZoneInfo.TransitionTime.CreateFloatingDateRule%2A> メソッドの `week` パラメーターに値5が渡され、特定の月の最後の週に移行が行われることを示します。

<xref:System.TimeZoneInfo.CreateCustomTimeZone%28System.String%2CSystem.TimeSpan%2CSystem.String%2CSystem.String%2CSystem.String%2CSystem.TimeZoneInfo.AdjustmentRule%5B%5D%29?displayProperty=nameWithType> メソッドの呼び出しで使用する <xref:System.TimeZoneInfo.AdjustmentRule> オブジェクトの配列を作成する場合、コードは、タイムゾーンに対して作成される調整の数に必要なサイズに配列を初期化できます。 代わりに、このコード例では、<xref:System.Collections.Generic.List%601.Add%2A> メソッドを呼び出して、各調整規則を <xref:System.TimeZoneInfo.AdjustmentRule> オブジェクトのジェネリック <xref:System.Collections.Generic.List%601> コレクションに追加します。 次に、このコードは <xref:System.Collections.Generic.List%601.CopyTo%2A> メソッドを呼び出して、このコレクションのメンバーを配列にコピーします。

また、この例では、<xref:System.TimeZoneInfo.TransitionTime.CreateFixedDateRule%2A> メソッドを使用して、固定日付調整を定義します。 これは、<xref:System.TimeZoneInfo.TransitionTime.CreateFloatingDateRule%2A> メソッドの呼び出しと似ていますが、移行パラメーターの時刻、月、および日のみが必要である点が異なります。

この例は、次のようなコードを使用してテストできます。

[!code-csharp[System.TimeZone2.CreateTimeZone#7](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/cs/System.TimeZone2.CreateTimeZone.cs#7)]
[!code-vb[System.TimeZone2.CreateTimeZone#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/vb/System.TimeZone2.CreateTimeZone.vb#7)]

## <a name="compiling-the-code"></a>コードのコンパイル

この例で必要な要素は次のとおりです。

- 次の名前空間がインポートされます。

  [!code-csharp[System.TimeZone2.CreateTimeZone#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/cs/System.TimeZone2.CreateTimeZone.cs#6)]
  [!code-vb[System.TimeZone2.CreateTimeZone#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/vb/System.TimeZone2.CreateTimeZone.vb#6)]

## <a name="see-also"></a>関連項目

- [日付、時刻およびタイム ゾーン](../../../docs/standard/datetime/index.md)
- [タイム ゾーンの概要](../../../docs/standard/datetime/time-zone-overview.md)
- [方法: 調整規則のないタイム ゾーンを作成する](../../../docs/standard/datetime/create-time-zones-without-adjustment-rules.md)
