---
title: 入れ子になった制御構造
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, control flow
- control structures [Visual Basic], nested
- conditional statements [Visual Basic], nested
- statements [Visual Basic], control flow
- control flow [Visual Basic], nested control statements
- structures [Visual Basic], nested control
- nested control statements [Visual Basic]
ms.assetid: cf60b061-65d9-44a8-81f2-b0bdccd23a05
ms.openlocfilehash: 5818b13661fb4415c6f531b741b8a963a80bd2b8
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348151"
---
# <a name="nested-control-structures-visual-basic"></a>入れ子になった制御構造 (Visual Basic)
制御ステートメントは、`For...Next` ループ内の `If...Then...Else` ブロックなど、他のコントロールステートメント内に配置できます。 別の control ステートメント内に配置された control ステートメントは、*入れ子になっ*ていると言います。  
  
## <a name="nesting-levels"></a>入れ子のレベル  
 Visual Basic の制御構造は、必要な数のレベルに入れ子にすることができます。 入れ子構造体を読みやすくするには、それぞれの本文をインデントするのが一般的です。 これは、統合開発環境 (IDE) エディターによって自動的に行われます。  
  
 次の例では、プロシージャ `sumRows`、マトリックスの各行の正の要素を加算します。  
  
```vb
Public Sub sumRows(ByVal a(,) As Double, ByRef r() As Double)  
    Dim i, j As Integer  
    For i = 0 To UBound(a, 1)  
        r(i) = 0  
        For j = 0 To UBound(a, 2)  
            If a(i, j) > 0 Then  
                r(i) = r(i) + a(i, j)  
            End If  
        Next j  
    Next i  
End Sub  
```  
  
 前の例では、最初の `Next` ステートメントは内側の `For` ループを閉じ、最後の `Next` ステートメントは外側の `For` ループを閉じます。  
  
 同様に、入れ子になった `If` ステートメントでは、`End If` ステートメントが最も近い直前の `If` ステートメントに自動的に適用されます。 入れ子になった `Do` ループは同様の方法で動作し、最も内側の `Loop` ステートメントは最も内側の `Do` ステートメントと一致します。  
  
> [!NOTE]
> 多くの制御構造では、キーワードをクリックすると、構造内のすべてのキーワードが強調表示されます。 たとえば、`If...Then...Else` の構築で [`If`] をクリックすると、構築内の `If`、`Then`、`ElseIf`、`Else`、および `End If` のすべてのインスタンスが強調表示されます。 次または前の強調表示されたキーワードに移動するには、CTRL + SHIFT + ↓キーを押すか、CTRL + SHIFT + 上方向キーを押します。  
  
## <a name="nesting-different-kinds-of-control-structures"></a>さまざまな種類の制御構造の入れ子  
 1つの種類のコントロール構造を別の種類に入れ子にすることができます。 次の例では、`With` ブロック内で `For Each` ループと入れ子になった `If` ブロック内の `With` ブロックを使用します。  
  
```vb
For Each ctl As System.Windows.Forms.Control In Me.Controls  
    With ctl  
        .BackColor = System.Drawing.Color.Yellow  
        .ForeColor = System.Drawing.Color.Black  
        If .CanFocus Then  
            .Text = "Colors changed"  
            If Not .Focus() Then  
                ' Insert code to process failed focus.  
            End If  
        End If  
    End With  
Next ctl  
```  
  
## <a name="overlapping-control-structures"></a>重複する制御構造  
 制御構造を重ねることはできません。 つまり、入れ子構造は、次の最も内側の構造体内に完全に含まれている必要があります。 たとえば、次の配置は、内部 `With` ブロックが終了する前に `For` ループが終了するため無効です。  
  
 ![無効な入れ子の例を示す図。](./media/nested-control-structures/example-invalid-nesting.gif) 
  
 Visual Basic コンパイラは、このような重複する制御構造を検出し、コンパイル時エラーを通知します。  
  
## <a name="see-also"></a>参照

- [制御フロー](../../../../visual-basic/programming-guide/language-features/control-flow/index.md)
- [条件判断構造](../../../../visual-basic/programming-guide/language-features/control-flow/decision-structures.md)
- [ループ構造](../../../../visual-basic/programming-guide/language-features/control-flow/loop-structures.md)
- [その他の制御構造](../../../../visual-basic/programming-guide/language-features/control-flow/other-control-structures.md)
