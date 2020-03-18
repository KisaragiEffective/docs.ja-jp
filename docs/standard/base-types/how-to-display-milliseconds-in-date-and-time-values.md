---
title: '方法: 日付および時刻の値のミリ秒部分を表示する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DateTime.ToString method
- displaying date and time data
- time [.NET Framework], milliseconds
- dates [.NET Framework], milliseconds
- milliseconds [.NET Framework]
ms.assetid: ae1a0610-90b9-4877-8eb6-4e30bc5e00cf
ms.openlocfilehash: 36d99753503d9ba4b1bde4143c86ba184674e53e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74960398"
---
# <a name="how-to-display-milliseconds-in-date-and-time-values"></a>方法: 日付および時刻の値のミリ秒部分を表示する
<xref:System.DateTime.ToString?displayProperty=nameWithType> などの既定の日付および時刻書式指定メソッドは時刻値の時間、分、秒を含めますが、ミリ秒の部分は含めません。 ここでは、書式設定された日付および時刻文字列の中にミリ秒部分を含める方法について説明します。  
  
### <a name="to-display-the-millisecond-component-of-a-datetime-value"></a>DateTime 値のミリ秒部分を表示するには  
  
1. 文字列形式の日付を処理している場合には、静的 <xref:System.DateTime> または <xref:System.DateTimeOffset> メソッドを使用して、その日付を<xref:System.DateTime.Parse%28System.String%29?displayProperty=nameWithType> 値または <xref:System.DateTimeOffset.Parse%28System.String%29?displayProperty=nameWithType> 値に変換します。  
  
2. 時刻のミリ秒部分の文字列表現を抽出するには、日付および時刻の値の <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType> メソッドまたは <xref:System.DateTimeOffset.ToString%2A> メソッドを呼び出して、カスタム書式パターン `fff` または `FFF` を単独で、あるいは他のカスタム書式指定子と共に `format` パラメーターとして渡します。  
  
## <a name="example"></a>例  
 この例では、<xref:System.DateTime> および <xref:System.DateTimeOffset> 値のミリ秒の部分をコンソールに表示します。単独で表示する場合と、より長い日付および時刻文字列に含める場合の両方を示します。  
  
 [!code-csharp[Formatting.HowTo.Millisecond#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.Millisecond/cs/Millisecond.cs#1)]
 [!code-vb[Formatting.HowTo.Millisecond#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.Millisecond/vb/Millisecond.vb#1)]  
  
 `fff` 書式パターンを使うと、ミリ秒値に後続のゼロがあればこれは含められます。 `FFF` 書式パターンでは、これが除外されます。 この違いを次の例に示します。  
  
 [!code-csharp[Formatting.HowTo.Millisecond#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.Millisecond/cs/Millisecond.cs#2)]
 [!code-vb[Formatting.HowTo.Millisecond#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.Millisecond/vb/Millisecond.vb#2)]  
  
 日付および時刻のミリ秒部分を含む完全なカスタム書式指定子を定義する場合、アプリケーションの現在のカルチャの時間要素の取り決めに対応しない可能性のあるハードコーディングされた書式を定義するという問題が生じます。 これに代わるより良い方法は、現在のカルチャの <xref:System.Globalization.DateTimeFormatInfo> オブジェクトで定義されているいずれかの日付および時刻表示パターンを取得して、ミリ秒を含むようにそれを修正することです。 この例では、この方法も示されています。 現在のカルチャの完全な日付および時刻パターンを <xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A?displayProperty=nameWithType> プロパティから取得した後、その秒パターンの後にカスタム パターン `.ffff` を挿入します。 この例では、1 つのメソッド呼び出しでこの操作を実行するために正規表現が使われていることに注意してください。  
  
 また、カスタム書式指定子を使用して、ミリ秒以外の秒の端数を表示することもできます。 たとえば、カスタム書式指定子 `f` または `F` は 1/10 秒を表示し、カスタム書式指定子 `ff` または `FF` は 1/100 秒、カスタム書式指定子 `ffff` または `FFFF` は 1/10000 秒をそれぞれ表示します。 返される文字列内では、ミリ秒の端数は丸められるのではなく、切り捨てられます。 次の例では、これらの書式指定子が使用されています。  
  
 [!code-csharp[Formatting.HowTo.Millisecond#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.Millisecond/cs/Millisecond.cs#3)]
 [!code-vb[Formatting.HowTo.Millisecond#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.Millisecond/vb/Millisecond.vb#3)]  
  
> [!NOTE]
> 1/10000 秒、1/100000 秒などの非常に小さな端数単位を表示することが可能です。 ただし、このような値を表示してもあまり意味がない可能性があります。 日付および時刻の値の精度は、システム クロックの分解能に依存します。 Windows NT 3.5 以降および Windows Vista オペレーティング システムでは、クロックの解像力は約 10-15 ミリ秒です。  
  
## <a name="see-also"></a>参照

- <xref:System.Globalization.DateTimeFormatInfo>
- [カスタム日時形式文字列](../../../docs/standard/base-types/custom-date-and-time-format-strings.md)
