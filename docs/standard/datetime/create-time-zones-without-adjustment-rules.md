---
title: '方法: 調整規則のないタイムゾーンを作成する'
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET Framework], adjustment rule
- time zones [.NET Framework], creating
- adjustment rule [.NET Framework]
ms.assetid: a6af8647-7893-4f29-95a9-d94c65a6e8dd
ms.openlocfilehash: 344d8307318d5a2e50eddb39ef488cd8c5f2fdac
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73129108"
---
# <a name="how-to-create-time-zones-without-adjustment-rules"></a>方法: 調整規則のないタイムゾーンを作成する

アプリケーションで必要とされる正確なタイムゾーン情報は、次のような理由で特定のシステムに存在しない場合があります。

- タイムゾーンがローカルシステムのレジストリに定義されていません。

- タイムゾーンに関するデータは、レジストリから変更または削除されています。

- タイムゾーンは存在しますが、特定の履歴期間のタイムゾーン調整に関する正確な情報がありません。

このような場合は、<xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> メソッドを呼び出して、アプリケーションで必要とされるタイムゾーンを定義できます。 このメソッドのオーバーロードを使用して、調整規則の有無に関係なくタイムゾーンを作成できます。 タイムゾーンで夏時間がサポートされている場合は、固定調整規則または浮動調整規則のいずれかを使用して調整を定義できます。 (これらの用語の定義については、「タイムゾーンの[概要](../../../docs/standard/datetime/time-zone-overview.md)」の「タイムゾーンの用語」セクションを参照してください)。

> [!IMPORTANT]
> <xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> メソッドを呼び出すことによって作成されたカスタムタイムゾーンは、レジストリには追加されません。 代わりに、<xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> メソッド呼び出しによって返されるオブジェクト参照を使用してのみアクセスできます。

このトピックでは、調整規則を使用せずにタイムゾーンを作成する方法について説明します。 夏時間調整規則をサポートするタイムゾーンを作成するには、「[方法: 調整規則を使用してタイムゾーンを作成](../../../docs/standard/datetime/create-time-zones-with-adjustment-rules.md)する」を参照してください。

### <a name="to-create-a-time-zone-without-adjustment-rules"></a>調整規則のないタイムゾーンを作成するには

1. タイムゾーンの表示名を定義します。

   表示名は、標準の形式に準拠しています。この形式では、世界協定時刻 (UTC) からのタイムゾーンのオフセットがかっこで囲まれ、その後にタイムゾーンを識別する文字列、タイムゾーン内の1つ以上の都市、または1つ以上の cou が続きます。タイムゾーンの ntries またはリージョン。

2. タイムゾーンの標準時刻の名前を定義します。 通常、この文字列はタイムゾーンの識別子としても使用されます。

3. タイムゾーンの標準名とは異なる識別子を使用する場合は、タイムゾーン識別子を定義します。

4. タイムゾーンの UTC からのオフセットを定義する <xref:System.TimeSpan> オブジェクトをインスタンス化します。 時刻が UTC より遅いタイムゾーンには、正のオフセットがあります。 時刻が UTC より前のタイムゾーンでは、負のオフセットが使用されます。

5. <xref:System.TimeZoneInfo.CreateCustomTimeZone%28System.String%2CSystem.TimeSpan%2CSystem.String%2CSystem.String%29?displayProperty=nameWithType> メソッドを呼び出して、新しいタイムゾーンをインスタンス化します。

## <a name="example"></a>例

次の例では、調整規則のない Mawson、南極のカスタムタイムゾーンを定義します。

[!code-csharp[System.TimeZone2.CreateTimeZone#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/cs/System.TimeZone2.CreateTimeZone.cs#1)]
[!code-vb[System.TimeZone2.CreateTimeZone#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/vb/System.TimeZone2.CreateTimeZone.vb#1)]

<xref:System.TimeZoneInfo.DisplayName%2A> プロパティに割り当てられた文字列は、標準形式に従います。この形式では、UTC からのタイムゾーンのオフセットの後にタイムゾーンのわかりやすい説明が続きます。

## <a name="compiling-the-code"></a>コードのコンパイル

この例で必要な要素は次のとおりです。

- 次の名前空間がインポートされます。

  [!code-csharp[System.TimeZone2.CreateTimeZone#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/cs/System.TimeZone2.CreateTimeZone.cs#6)]
  [!code-vb[System.TimeZone2.CreateTimeZone#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/vb/System.TimeZone2.CreateTimeZone.vb#6)]

## <a name="see-also"></a>関連項目

- [日付、時刻およびタイム ゾーン](../../../docs/standard/datetime/index.md)
- [タイム ゾーンの概要](../../../docs/standard/datetime/time-zone-overview.md)
- [方法: 調整規則のあるタイム ゾーンを作成する](../../../docs/standard/datetime/create-time-zones-with-adjustment-rules.md)
