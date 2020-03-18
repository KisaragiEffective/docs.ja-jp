---
title: '方法: 2 つのオブジェクトが等しいかどうかをテストする'
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], reference
- Is operator [Visual Basic], comparing objects
- reference variables [Visual Basic]
- variables [Visual Basic], referring to same object
- objects [Visual Basic], variables referring to same
- Visual Basic code, operators
ms.assetid: f760e828-8704-4256-bc2d-c22a4c93b524
ms.openlocfilehash: 22e8e1e688d9e3bc3804899103ee78814aac235b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343618"
---
# <a name="how-to-test-whether-two-objects-are-the-same-visual-basic"></a>方法: 2 つのオブジェクトが等しいかどうかをテストする (Visual Basic)
オブジェクトを参照する変数が2つある場合は、`Is` または `IsNot` のいずれかの演算子、またはその両方を使用して、同じインスタンスを参照しているかどうかを判断できます。  
  
### <a name="to-test-whether-two-objects-are-the-same"></a>2つのオブジェクトが同一かどうかをテストするには  
  
- 2つの変数がオペランドとして使用されて[いる場合は、Is 演算子](../../../../visual-basic/language-reference/operators/is-operator.md)または[IsNot 演算子](../../../../visual-basic/language-reference/operators/isnot-operator.md)を使用します。  
  
     [!code-vb[VbVbalrOperators#69](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#69)]  
  
 2つのオブジェクトが同じインスタンスを参照しているかどうかによって、特定のアクションを実行することができます。 前の例では、コントロールの `c` とフォーム `f`のアクティブコントロールとを比較しています。 アクティブなコントロールがない場合、または存在していても `c`と同じコントロールインスタンスではない場合、`If` ステートメントは失敗し、プロシージャはそれ以上の処理を行わずに戻ります。  
  
 `Is` と `IsNot` のどちらを使用する場合でも、個人的に便利です。 指定された式では、もう一方の方よりも読みやすくなる場合があります。  
  
## <a name="see-also"></a>関連項目

- [Visual Basic の比較演算子](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/comparison-operators.md)
