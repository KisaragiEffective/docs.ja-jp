---
title: ByVal
ms.date: 07/20/2015
f1_keywords:
- vb.ByVal
- ByVal
helpviewer_keywords:
- ByVal keyword [Visual Basic], contexts
- ByVal keyword [Visual Basic]
ms.assetid: 1eaf4e58-b305-4785-9e3d-e416b9c75598
ms.openlocfilehash: a96f871c6ce119f65ebbec54fdb1471ae105d504
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351595"
---
# <a name="byval-visual-basic"></a>ByVal (Visual Basic)
引数が[値によって](../../programming-guide/language-features/procedures/passing-arguments-by-value-and-by-reference.md)渡されることを指定します。これにより、呼び出されたプロシージャまたはプロパティが、呼び出し元のコードの引数の基になる変数の値を変更できなくなります。 修飾子が指定されていない場合、ByVal が既定値になります。

> [!NOTE]
> これは既定であるため、メソッドシグネチャで `ByVal` キーワードを明示的に指定する必要はありません。 ノイズコードが生成される傾向があり、既定以外の `ByRef` キーワードが見落とされることがよくあります。

## <a name="remarks"></a>コメント
 `ByVal` 修飾子は、次のコンテキストで使用できます。

 [Declare Statement](../../../visual-basic/language-reference/statements/declare-statement.md)

 [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)
  
 [Operator Statement](../../../visual-basic/language-reference/statements/operator-statement.md)
  
 [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)
  
 [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)

## <a name="example"></a>例
 次の例では、参照型の引数を指定して `ByVal` パラメーター渡し機構を使用する方法を示します。 この例では、引数は `c1`、クラス `Class1`のインスタンスです。 `ByVal` を指定すると、プロシージャ内のコードが参照引数の基になる値を変更できなくなりますが、`c1`、`c1`のアクセス可能なフィールドとプロパティは保護されません。

 [!code-vb[VbVbalrKeywords#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class5.vb#10)]

## <a name="see-also"></a>参照

- [キーワード](../../../visual-basic/language-reference/keywords/index.md)
- [引数の値渡しと参照渡し](../../../visual-basic/programming-guide/language-features/procedures/passing-arguments-by-value-and-by-reference.md)
