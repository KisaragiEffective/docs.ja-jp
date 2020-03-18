---
title: Iterator
ms.date: 07/20/2015
f1_keywords:
- vb.Iterator
helpviewer_keywords:
- Iterator keyword [Visual Basic]
ms.assetid: 69cb0b04-ac87-49d0-bcfe-810c0d60daff
ms.openlocfilehash: 9d7eaf186bae544031487ce027505e1e0d4ae0f3
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351524"
---
# <a name="iterator-visual-basic"></a>Iterator (Visual Basic)
関数または `Get` アクセサーが反復子であることを指定します。  
  
## <a name="remarks"></a>コメント  
 *反復子*は、コレクションに対してカスタムの反復処理を実行します。 反復子は、 [Yield](../../../visual-basic/language-reference/statements/yield-statement.md)ステートメントを使用して、コレクション内の各要素を1回に1つ返します。 `Yield` ステートメントに到達すると、コード内の現在の場所が保持されます。 次回、反復子メソッドが呼び出されると、この位置から実行が再開されます。  
  
 反復子は、関数として実装することも、プロパティ定義の `Get` アクセサーとして実装することもできます。 `Iterator` 修飾子は、iterator 関数または `Get` アクセサーの宣言に記述されます。  
  
 For Each を使用して、クライアントコードから反復子を呼び出します。 [次のステートメント](../../../visual-basic/language-reference/statements/for-each-next-statement.md)。  
  
 反復子関数または `Get` アクセサーの戻り値の型は、<xref:System.Collections.IEnumerable>、<xref:System.Collections.Generic.IEnumerable%601>、<xref:System.Collections.IEnumerator>、または <xref:System.Collections.Generic.IEnumerator%601>にすることができます。  
  
 反復子に `ByRef` パラメーターを含めることはできません。  
  
 反復子を、イベント、インスタンス コンストラクター、静的コンストラクター、静的デストラクターで指定することはできません。  
  
 反復子は、匿名関数にすることができます。 詳細については、「[Iterator](../../programming-guide/concepts/iterators.md)」をご覧ください。  
  
## <a name="usage"></a>使用方法  
 `Iterator` 修飾子は、次のコンテキストで使用できます。  
  
- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)  
  
- [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)  
  
## <a name="example"></a>例  
 次の例は、反復子メソッドを示します。 Iterator 関数には、For... の内部にある `Yield` ステートメントがあります。 [次](../../../visual-basic/language-reference/statements/for-next-statement.md)のループ。 `Main` 内の[各](../../../visual-basic/language-reference/statements/for-each-next-statement.md)ステートメント本体の各反復処理では、`Power` iterator 関数の呼び出しが作成されます。 Iterator 関数を呼び出すごとに、`Yield` ステートメントの次の実行に進みます。これは、`For…Next` ループの次の反復処理で行われます。  
  
 [!code-vb[VbVbalrStatements#98](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class2.vb#98)]  
  
## <a name="example"></a>例  
 次の例は、反復子である `Get` アクセサーを示しています。 `Iterator` 修飾子は、プロパティ宣言にあります。  
  
 [!code-vb[VbVbalrStatements#99](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class2.vb#99)]  
  
 その他の例については、「[Iterator](../../programming-guide/concepts/iterators.md)」をご覧ください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Runtime.CompilerServices.IteratorStateMachineAttribute>
- [Iterator](../../programming-guide/concepts/iterators.md)
- [Yield ステートメント](../../../visual-basic/language-reference/statements/yield-statement.md)
