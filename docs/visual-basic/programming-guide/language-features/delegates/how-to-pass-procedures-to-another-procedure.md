---
title: '方法: プロシージャを別のプロシージャに渡す'
ms.date: 07/20/2015
helpviewer_keywords:
- AddressOf operator [Visual Basic]
- delegates [Visual Basic], passing procedures
ms.assetid: 5adbba15-5a1d-413f-ab3e-3ff6cc0a4669
ms.openlocfilehash: 300489935ce54d78b989d09211a7f6ba95c2f514
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345247"
---
# <a name="how-to-pass-procedures-to-another-procedure-in-visual-basic"></a>方法 : Visual Basic でプロシージャを別のプロシージャに渡す
この例では、デリゲートを使用してプロシージャを別のプロシージャに渡す方法を示します。  
  
 デリゲートは、Visual Basic の他の型と同様に使用できる型です。 `AddressOf` 演算子は、プロシージャ名に適用されるときにデリゲートオブジェクトを返します。  
  
 この例には、`AddressOf` 演算子で取得した別のプロシージャへの参照を受け取ることができるデリゲートパラメーターを持つプロシージャがあります。  
  
### <a name="create-the-delegate-and-matching-procedures"></a>デリゲートと一致するプロシージャを作成する  
  
1. `MathOperator`という名前のデリゲートを作成します。  
  
     [!code-vb[VbVbalrDelegates#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#1)]  
  
2. シグネチャが一致するように、パラメーターと戻り値が `MathOperator`と一致する `AddNumbers` という名前のプロシージャを作成します。  
  
     [!code-vb[VbVbalrDelegates#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#2)]  
  
3. `MathOperator`に一致する署名を持つ `SubtractNumbers` という名前のプロシージャを作成します。  
  
     [!code-vb[VbVbalrDelegates#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#3)]  
  
4. デリゲートをパラメーターとして受け取る `DelegateTest` という名前のプロシージャを作成します。  
  
     署名が `MathOperator` シグネチャと一致するため、このプロシージャは `AddNumbers` または `SubtractNumbers`への参照を受け入れることができます。  
  
     [!code-vb[VbVbalrDelegates#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#4)]  
  
5. パラメーターとして `AddNumbers` のデリゲートを使用して `DelegateTest` を1回呼び出す `Test` という名前のプロシージャを作成し、さらに、パラメーターとして `SubtractNumbers` のデリゲートを使用します。  
  
     [!code-vb[VbVbalrDelegates#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#5)]  
  
     `Test` が呼び出されると、最初に `5` および `3`(8) で動作している `AddNumbers` の結果が表示されます。 次に、`9` と `3` に作用する `SubtractNumbers` の結果が表示されます。これは6です。  
  
## <a name="see-also"></a>参照

- [デリゲート](../../../../visual-basic/programming-guide/language-features/delegates/index.md)
- [AddressOf 演算子](../../../../visual-basic/language-reference/operators/addressof-operator.md)
- [Delegate ステートメント](../../../../visual-basic/language-reference/statements/delegate-statement.md)
- [方法: デリゲート メソッドを呼び出す](../../../../visual-basic/programming-guide/language-features/delegates/how-to-invoke-a-delegate-method.md)
