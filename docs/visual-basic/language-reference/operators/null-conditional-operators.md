---
title: Null 条件演算子
ms.date: 10/19/2018
helpviewer_keywords:
- null-conditional operators [Visual Basic]
- ?. operator [Visual Basic]
- ?[] operator [C#]
- ?[] operator [Visual Basic]
ms.openlocfilehash: 003f579a7128bbe2462b7fbe7057de03e61bfbe6
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348289"
---
# <a name="-and--null-conditional-operators-visual-basic"></a>?. そして。() null 条件演算子 (Visual Basic)

メンバーアクセス (`?.`) 操作またはインデックス (`?()`) 操作を実行する前に、左側のオペランドの値を null (`Nothing`) に対してテストします。左側のオペランドが `Nothing`に評価された場合に `Nothing` を返します。 通常、値型を返す式では、null 条件演算子は <xref:System.Nullable%601>を返します。

これらの演算子を使用すると、特にデータ構造への降順で、null チェックを処理するコードを記述しやすくなります。 次に例を示します。

```vb
' Nothing if customers is Nothing
Dim length As Integer? = customers?.Length

' Nothing if customers is Nothing
Dim first As Customer = customers?(0)

' Nothing if customers, the first customer, or Orders is Nothing
Dim count As Integer? = customers?(0)?.Orders?.Count()
```

比較のために、null 条件演算子を使用しない最初の式の代替コードは次のとおりです。

```vb
Dim length As Integer
If customers IsNot Nothing Then
   length = customers.Length
End If
```

場合によっては、オブジェクトのブール型の値に基づいて、null である可能性のあるオブジェクトに対してアクションを実行する必要があります (次の例で `IsAllowedFreeShipping` ブール型プロパティと同様)。

```vb
Dim customer = FindCustomerByID(123) 'customer will be Nothing if not found.

If customer IsNot Nothing AndAlso customer.IsAllowedFreeShipping Then
  ApplyFreeShippingToOrders(customer)
End If
```

次のように、null 条件演算子を使用すると、コードを短くして、null を手動でチェックすることを回避できます。

```vb
Dim customer = FindCustomerByID(123) 'customer will be Nothing if not found.

If customer?.IsAllowedFreeShipping Then ApplyFreeShippingToOrders(customer)
```

Null 条件演算子はショートサーキットです。  条件付きメンバーアクセスとインデックス操作のチェーンの1つの操作が `Nothing`を返す場合、チェーンの残りの実行は停止します。  次の例では、`A`、`B`、または `C` が `Nothing`に評価される場合、`C(E)` は評価されません。

```vb
A?.B?.C?(E)
```

Null 条件メンバーアクセスのもう1つの用途は、はるかに少ないコードでスレッドセーフな方法でデリゲートを呼び出すことです。  次の例では、`NewsBroadcaster` と `NewsReceiver`の2つの型を定義しています。 ニュース項目は `NewsBroadcaster.SendNews` デリゲートによって受信者に送信されます。

```vb
Public Module NewsBroadcaster
   Dim SendNews As Action(Of String)

   Public Sub Main()
      Dim rec As New NewsReceiver()
      Dim rec2 As New NewsReceiver()
      SendNews?.Invoke("Just in: A newsworthy item...")
   End Sub

   Public Sub Register(client As Action(Of String))
      SendNews = SendNews.Combine({SendNews, client})
   End Sub
End Module

Public Class NewsReceiver
   Public Sub New()
      NewsBroadcaster.Register(AddressOf Me.DisplayNews)
   End Sub

   Public Sub DisplayNews(newsItem As String)
      Console.WriteLine(newsItem)
   End Sub
End Class
```

`SendNews` の呼び出しリストに要素がない場合、`SendNews` デリゲートは <xref:System.NullReferenceException>をスローします。 Null 条件演算子の前に、次のようなコードは、デリゲート呼び出しリストが `Nothing`されていないことを保証しています。

```vb
SendNews = SendNews.Combine({SendNews, client})
If SendNews IsNot Nothing Then
   SendNews("Just in...")
End If
```

新しい方法は格段に単純です。

```vb
SendNews = SendNews.Combine({SendNews, client})
SendNews?.Invoke("Just in...")
```

コンパイラが `SendNews` を評価するためのコードを一度しか生成せず、一時変数に結果が保持されるため、新しい方法はスレッド セーフです。 null 条件デリゲート呼び出し構文 `Invoke` がないため、`SendNews?(String)` メソッドを明示的に呼び出す必要があります。

## <a name="see-also"></a>関連項目

- [演算子 (Visual Basic)](index.md)
- [Visual Basic のプログラミング ガイド](../../../visual-basic/programming-guide/index.md)
- [Visual Basic の言語リファレンス](../../../visual-basic/language-reference/index.md)
