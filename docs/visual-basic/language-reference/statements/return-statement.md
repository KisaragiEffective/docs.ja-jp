---
title: Return ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.Return
helpviewer_keywords:
- Return statement [Visual Basic], syntax
- control flow [Visual Basic], returning control to expressions
- Return statement [Visual Basic]
- expressions [Visual Basic], returning control to
ms.assetid: ac86e7f0-5a67-42c3-9834-0e0381efa3ec
ms.openlocfilehash: efc85a3a844898345aa2d16926ba0e35d7346d1b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74333014"
---
# <a name="return-statement-visual-basic"></a>Return ステートメント (Visual Basic)
`Function`、`Sub`、`Get`、`Set`、または `Operator` プロシージャを呼び出したコードに制御を戻します。  
  
## <a name="syntax"></a>構文  
  
```vb  
Return  
' -or-  
Return expression  
```  
  
## <a name="part"></a>要素  
 `expression`  
 `Function`、`Get`、または `Operator` プロシージャで必要です。 呼び出し元のコードに返される値を表す式。  
  
## <a name="remarks"></a>コメント  
 `Sub` または `Set` プロシージャでは、`Return` ステートメントは `Exit Sub` または `Exit Property` ステートメントと同じであり、`expression` を指定することはできません。  
  
 `Function`、`Get`、または `Operator` プロシージャでは、`Return` ステートメントに `expression`を含める必要があります。また、`expression` は、プロシージャの戻り値の型に変換可能なデータ型に評価される必要があります。 `Function` または `Get` の手順では、プロシージャ名に式を代入して戻り値として使用し、`Exit Function` または `Exit Property` ステートメントを実行する方法もあります。 `Operator` の手順では、`Return expression`を使用する必要があります。  
  
 同じ手順で、必要に応じて `Return` ステートメントをいくつでも含めることができます。  
  
> [!NOTE]
> `Finally` ブロック内のコードは、`Try` または `Catch` ブロック内の `Return` ステートメントが検出された後、その `Return` ステートメントが実行される前に実行されます。 `Return` ステートメントを `Finally` ブロックに含めることはできません。  
  
## <a name="example"></a>例  
 次の例では、`Return` ステートメントを複数回使用して、プロシージャが他の操作を行う必要がない場合に、呼び出し元のコードに戻ります。  
  
 [!code-vb[VbVbalrStatements#53](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#53)]  
  
## <a name="see-also"></a>参照

- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)
- [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)
- [Get ステートメント](../../../visual-basic/language-reference/statements/get-statement.md)
- [Set ステートメント](../../../visual-basic/language-reference/statements/set-statement.md)
- [Operator Statement](../../../visual-basic/language-reference/statements/operator-statement.md)
- [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)
- [Exit ステートメント](../../../visual-basic/language-reference/statements/exit-statement.md)
- [Try...Catch...Finally ステートメント](../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)
