---
title: '方法: 定義済みの UTC オブジェクトおよびローカルタイムゾーンオブジェクトにアクセスする'
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET Framework], local
- predefined time zones
- UTC times, predefined
- local time zone access
- time zones [.NET Framework], retrieving
- time zones [.NET Framework], UTC
ms.assetid: 961fb70b-83f0-4dab-a042-cb5fcd817cf5
ms.openlocfilehash: ef22753d9934a52d955412a4493b608f265519aa
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73132608"
---
# <a name="how-to-access-the-predefined-utc-and-local-time-zone-objects"></a>方法: 定義済みの UTC オブジェクトおよびローカルタイムゾーンオブジェクトにアクセスする

<xref:System.TimeZoneInfo> クラスには、定義済みのタイムゾーンオブジェクトへのコードアクセスを提供する、<xref:System.TimeZoneInfo.Utc%2A> と <xref:System.TimeZoneInfo.Local%2A>という2つのプロパティが用意されています。 このトピックでは、これらのプロパティから返される <xref:System.TimeZoneInfo> オブジェクトにアクセスする方法について説明します。

### <a name="to-access-the-coordinated-universal-time-utc-timezoneinfo-object"></a>世界協定時刻 (UTC) の TimeZoneInfo オブジェクトにアクセスするには

1. `static` (Visual Basic で`Shared`) <xref:System.TimeZoneInfo.Utc%2A?displayProperty=nameWithType> プロパティを使用して、世界協定時刻にアクセスします。

2. プロパティによって返される <xref:System.TimeZoneInfo> オブジェクトをオブジェクト変数に割り当てるのではなく、<xref:System.TimeZoneInfo.Utc%2A?displayProperty=nameWithType> プロパティを使用して、世界協定時刻に引き続きアクセスします。

### <a name="to-access-the-local-time-zone"></a>ローカル タイム ゾーンにアクセスするには

1. ローカルシステムのタイムゾーンにアクセスするには、`static` (Visual Basic で`Shared`) <xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> プロパティを使用します。

2. プロパティによって返される <xref:System.TimeZoneInfo> オブジェクトをオブジェクト変数に割り当てるのではなく、引き続き <xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> プロパティを使用してローカルタイムゾーンにアクセスします。

## <a name="example"></a>例

次のコードでは、<xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> と <xref:System.TimeZoneInfo.Utc%2A?displayProperty=nameWithType> のプロパティを使用して、米国およびカナダ東部標準時のタイムゾーンから時刻を変換し、タイムゾーン名をコンソールに表示します。

[!code-csharp[System.TimeZone2.Concepts#13](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#13)]
[!code-vb[System.TimeZone2.Concepts#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#13)]

ローカルタイムゾーンには、ローカルタイムゾーンを <xref:System.TimeZoneInfo> オブジェクト変数に割り当てるのではなく、常に <xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> プロパティを使用してアクセスする必要があります。 同様に、UTC ゾーンを <xref:System.TimeZoneInfo> オブジェクト変数に割り当てるのではなく、常に <xref:System.TimeZoneInfo.Utc%2A?displayProperty=nameWithType> プロパティを使用して世界協定時刻にアクセスする必要があります。 これにより、<xref:System.TimeZoneInfo.ClearCachedData%2A?displayProperty=nameWithType> メソッドの呼び出しによって <xref:System.TimeZoneInfo> オブジェクト変数が無効になるのを防ぐことができます。

## <a name="compiling-the-code"></a>コードのコンパイル

この例で必要な要素は次のとおりです。

- <xref:System> 名前空間は、`using` ステートメント (コードでC#必要) を使用してインポートされます。

## <a name="see-also"></a>関連項目

- [日付、時刻およびタイム ゾーン](../../../docs/standard/datetime/index.md)
- [ローカル システムで定義されているタイム ゾーンの検索](../../../docs/standard/datetime/finding-the-time-zones-on-local-system.md)
- [方法: TimeZoneInfo オブジェクトをインスタンス化する](../../../docs/standard/datetime/instantiate-time-zone-info.md)
