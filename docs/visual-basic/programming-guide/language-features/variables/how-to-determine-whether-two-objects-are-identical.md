---
title: '方法 : 2 つのオブジェクトが同一であるかどうか判別する'
ms.date: 07/20/2015
helpviewer_keywords:
- testing [Visual Basic], objects
- objects [Visual Basic], comparing
- object variables [Visual Basic], determining identity
ms.assetid: 7829f817-0d1f-4749-a707-de0b95e0cf5c
ms.openlocfilehash: 5deebd4ffc5b277c94f5ae36c00fd6e5010a1551
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348595"
---
# <a name="how-to-determine-whether-two-objects-are-identical-visual-basic"></a>方法: 2 つのオブジェクトが同一であるかどうか判別する (Visual Basic)
Visual Basic では、ポインターが同じである場合は2つの変数参照が同一であると見なされます。つまり、両方の変数がメモリ内の同じクラスインスタンスを指している場合はです。 たとえば、Windows フォームアプリケーションでは、現在のインスタンス (`Me`) が `Form2`などの特定のインスタンスと同じかどうかを判断するための比較を行うことができます。  
  
 Visual Basic には、ポインターを比較する2つの演算子が用意されています。 [Is 演算子](../../../../visual-basic/language-reference/operators/is-operator.md)は `True` を返します。オブジェクトが同一である場合は、 [IsNot 演算子](../../../../visual-basic/language-reference/operators/isnot-operator.md)が `True` を返します (存在しない場合)。  
  
## <a name="determining-if-two-objects-are-identical"></a>2つのオブジェクトが同一かどうかを判断する  
  
#### <a name="to-determine-if-two-objects-are-identical"></a>2つのオブジェクトが同一かどうかを確認するには  
  
1. 2つのオブジェクトをテストするための `Boolean` 式を設定します。  
  
2. テスト式では、2つのオブジェクトをオペランドとして使用して、`Is` 演算子を使用します。  
  
     オブジェクトが同じクラスインスタンスを指している場合、`Is` は `True` を返します。  
  
## <a name="determining-if-two-objects-are-not-identical"></a>2つのオブジェクトが同一でないかどうかを判断する  
 2つのオブジェクトが同一でない場合にアクションを実行することが必要になる場合があります。たとえば、`If Not obj1 Is obj2`のような `Not` と `Is`を組み合わせて使用するのは不便です。 このような場合は、`IsNot` 演算子を使用できます。  
  
#### <a name="to-determine-if-two-objects-are-not-identical"></a>2つのオブジェクトが同一でないかどうかを調べるには  
  
1. 2つのオブジェクトをテストするための `Boolean` 式を設定します。  
  
2. テスト式では、2つのオブジェクトをオペランドとして使用して、`IsNot` 演算子を使用します。  
  
     オブジェクトが同じクラスインスタンスを指していない場合、`IsNot` は `True` を返します。  
  
## <a name="example"></a>例  
 次の例では、`Object` 変数のペアをテストして、それらが同じクラスインスタンスを指しているかどうかを確認します。  
  
 [!code-vb[VbVbalrKeywords#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/class7.vb#14)]  
  
 前の例では、次の出力が表示されます。  
  
 `objA different from objB? True`  
  
 `objA identical to objC? True`  
  
## <a name="see-also"></a>参照

- [Object Data Type](../../../../visual-basic/language-reference/data-types/object-data-type.md)
- [オブジェクト変数](../../../../visual-basic/programming-guide/language-features/variables/object-variables.md)
- [オブジェクト変数の値](../../../../visual-basic/programming-guide/language-features/variables/object-variable-values.md)
- [Is 演算子](../../../../visual-basic/language-reference/operators/is-operator.md)
- [IsNot 演算子](../../../../visual-basic/language-reference/operators/isnot-operator.md)
- [方法: 2 つのオブジェクトが関連しているかどうかを決める](../../../../visual-basic/programming-guide/language-features/variables/how-to-determine-whether-two-objects-are-related.md)
- [Me、My、MyBase、および MyClass](../../../../visual-basic/programming-guide/program-structure/me-my-mybase-and-myclass.md)
