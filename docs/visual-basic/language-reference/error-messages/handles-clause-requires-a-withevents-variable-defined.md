---
description: '詳細情報: BC30506:Handles 句には、含んでいる型またはその基本型の 1 つで定義した WithEvents 変数が必要です。'
title: Handles 句には、含んでいる型またはその基本型の 1 つで定義した WithEvents 変数が必要です。
ms.date: 07/20/2015
f1_keywords:
- vbc30506
- bc30506
helpviewer_keywords:
- BC30506
ms.assetid: 5b66f6a8-f050-4e03-a57f-a64e85f80cb5
ms.openlocfilehash: 7253a4766b2015ccbe0ae62f64bc10aaca073dc3
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99796120"
---
# <a name="bc30506-handles-clause-requires-a-withevents-variable-defined-in-the-containing-type-or-one-of-its-base-types"></a>BC30506:Handles 句には、含んでいる型またはその基本型の 1 つで定義した WithEvents 変数が必要です。

`Handles` 句に `WithEvents` 変数を指定していません。 プロシージャ宣言の最後にある `Handles` キーワードによって、`WithEvents` キーワードを使用して宣言されたオブジェクト変数によって発生したイベントが処理されるようになります。

**エラー ID:** BC30506

## <a name="to-correct-this-error"></a>このエラーを解決するには

必要な `WithEvents` 変数を指定します。

## <a name="example"></a>例

次の例では、Visual Basic によってコンパイラ エラー `BC30506` が生成されます。これは、<xref:System.Timers.Timer?displayProperty=nameWithType> インスタンスの定義で [WithEvents](../modifiers/withevents.md) キーワードが使用されていないためです。

```vb
Imports System.Timers

Module Module1
    Private _timer1 As New Timer() With {.Interval = 1000, .Enabled = True}

    Sub Main()
        Console.WriteLine("Press any key to start the timer...")
        Console.ReadKey()
        _timer1.Start()
        Console.ReadKey()
    End Sub

    Private Sub Timer1_Tick(sender As Object, args As EventArgs) Handles _timer1.Elapsed
        Console.WriteLine("Press any key to terminate...")
    End Sub
End Module
```

次の例では、`_timer1` 変数が `WithEvents` キーワードで定義されているため、正常にコンパイルされます。

```vb
Imports System.Timers

Module Module1
    Private WithEvents _timer1 As New Timer() With {.Interval = 1000}

    Sub Main()
        Console.WriteLine("Press any key to start the timer...")
        Console.ReadKey()
        _timer1.Start()
        Console.ReadKey()
    End Sub

    Private Sub Timer1_Tick(sender As Object, args As EventArgs) Handles _timer1.Elapsed
        Console.WriteLine("Press any key to terminate...")
    End Sub

End Module
```

## <a name="see-also"></a>関連項目

- [Handles](../statements/handles-clause.md)
