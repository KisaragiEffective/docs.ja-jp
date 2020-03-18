---
title: '方法: ユーザーがあいまいな時刻を解決できるようにする'
ms.date: 04/10/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- time zones [.NET Framework], ambiguous time
- ambiguous time [.NET Framework]
ms.assetid: bca874ee-5b68-4654-8bbd-3711220ef332
ms.openlocfilehash: f988616a4b2a5d8202c87e3be3cb23c7f9f1f130
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73122276"
---
# <a name="how-to-let-users-resolve-ambiguous-times"></a>方法: ユーザーがあいまいな時刻を解決できるようにする

あいまいな時刻とは、複数の世界協定時刻 (UTC) にマップされる時刻です。 これは、あるタイム ゾーンの夏時間から標準時間に移行する際など、時計の時刻を前に戻すときに発生します。 あいまいな時刻を処理する場合は、次のいずれかの操作を行います。

- あいまいな時刻が、ユーザーによって入力されたデータ項目である場合は、あいまいさの解決をユーザーに任せることができます。

- 時刻が UTC にどのようにマップされるかを想定します。 たとえば、あいまいな時刻は常にタイム ゾーンの標準時刻で表されると想定できます。

このトピックでは、ユーザーがあいまいな時刻を解決できるようにする方法について説明します。

### <a name="to-let-a-user-resolve-an-ambiguous-time"></a>ユーザーにあいまいな時刻を解決させるには

1. ユーザーによって入力された日付と時刻を取得します。

2. <xref:System.TimeZoneInfo.IsAmbiguousTime%2A> メソッドを呼び出して、時刻があいまいであるかどうかを確認します。

3. 時刻があいまいな場合は、<xref:System.TimeZoneInfo.GetAmbiguousTimeOffsets%2A> メソッドを呼び出して、<xref:System.TimeSpan> オブジェクトの配列を取得します。 配列の各要素には、あいまいな時刻をマップできる UTC オフセットが含まれています。

4. ユーザーに目的のオフセットを選択させます。

5. ローカル時刻からユーザーによって選択されたオフセットを減算して、UTC の日時を取得します。

6. `static` (Visual Basic .NET で`Shared`) <xref:System.DateTime.SpecifyKind%2A> メソッドを呼び出して、UTC の日付と時刻の値の <xref:System.DateTime.Kind%2A> プロパティを <xref:System.DateTimeKind.Utc?displayProperty=nameWithType>に設定します。

## <a name="example"></a>例

次の例では、ユーザーに日付と時刻を入力するように求め、それがあいまいである場合は、ユーザーにあいまいな時刻をマップする UTC 時刻を選択させています。

[!code-csharp[System.TimeZone2.Concepts#11](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#11)]
[!code-vb[System.TimeZone2.Concepts#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#11)]

この例のコードの中核となるのは、<xref:System.TimeSpan> オブジェクトの配列を使用して、UTC からのあいまいな時刻のオフセットを示すことです。 ただし、これらのオフセットは、ユーザーにとって意味がない可能性があります。 オフセットの意味を明確にするには、コードで、オフセットがローカル タイム ゾーンの標準時刻を表すか、または夏時間を表すかに注意します。 このコードでは、オフセットと <xref:System.TimeZoneInfo.BaseUtcOffset%2A> プロパティの値を比較することによって、標準の時間と夏時間の時間を決定します。 このプロパティは、UTC とタイム ゾーンの標準時間の差を示します。

この例では、ローカルタイムゾーンへのすべての参照は、<xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> プロパティを使用して作成されます。ローカルタイムゾーンがオブジェクト変数に割り当てられることはありません。 これは、<xref:System.TimeZoneInfo.ClearCachedData%2A?displayProperty=nameWithType> メソッドを呼び出すと、ローカルタイムゾーンが割り当てられているすべてのオブジェクトが無効になるため、推奨される方法です。

## <a name="compiling-the-code"></a>コードのコンパイル

この例で必要な要素は次のとおりです。

- <xref:System> 名前空間は、`using` ステートメント (コードでC#必要) を使用してインポートされます。

## <a name="see-also"></a>関連項目

- [日付、時刻およびタイム ゾーン](../../../docs/standard/datetime/index.md)
- [方法: あいまいな時刻を解決する](../../../docs/standard/datetime/resolve-ambiguous-times.md)
