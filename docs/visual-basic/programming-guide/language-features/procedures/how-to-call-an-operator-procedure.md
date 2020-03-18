---
title: '方法 : 演算子プロシージャを呼び出す'
ms.date: 07/20/2015
helpviewer_keywords:
- operator procedures [Visual Basic], calling
- procedures [Visual Basic], operator
- procedure calls [Visual Basic], operator overloading
- syntax [Visual Basic], Operator procedures
- operators [Visual Basic], overloading
- return values [Visual Basic], Operator procedures
- overloaded operators [Visual Basic], calling
- operator overloading
ms.assetid: 0dce42cc-f0b0-4c14-9f62-018b21f33497
ms.openlocfilehash: a977b17d4b2c797bbe38d289a57f3d9d31fa64fa
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75345967"
---
# <a name="how-to-call-an-operator-procedure-visual-basic"></a>方法: 演算子プロシージャを呼び出す (Visual Basic)
演算子プロシージャを呼び出すには、式の中で演算子記号を使用します。 変換演算子の場合は、 [CType 関数](../../../../visual-basic/language-reference/functions/ctype-function.md)を呼び出して、あるデータ型から別のデータ型に値を変換します。  
  
 演算子プロシージャを明示的に呼び出すことはできません。 演算子を使用するのは、通常、演算子を使用する場合と同じように、代入ステートメントまたは式で `CType` 関数を使用することだけです。 Visual Basic は、演算子プロシージャを呼び出します。  
  
 クラスまたは構造体に対して演算子を定義することは、演算子の*オーバーロード*とも呼ばれます。  
  
### <a name="to-call-an-operator-procedure"></a>演算子プロシージャを呼び出すには  
  
1. 通常の方法では、演算子記号を式の中で使用します。  
  
2. オペランドのデータ型が演算子に適していること、および正しい順序であることを確認してください。  
  
3. 演算子は、式の値に想定どおりに貢献します。  
  
### <a name="to-call-a-conversion-operator-procedure"></a>変換演算子プロシージャを呼び出すには  
  
1. 式の内部で `CType` を使用します。  
  
2. オペランドのデータ型が変換に適していること、および正しい順序であることを確認してください。  
  
3. `CType` は変換演算子プロシージャを呼び出し、変換された値を返します。  
  
## <a name="example"></a>使用例  
 次の例では、2つの <xref:System.TimeSpan> 構造体を作成し、それらをまとめて追加し、結果を3番目の <xref:System.TimeSpan> 構造体に格納します。 <xref:System.TimeSpan> 構造体は、いくつかの標準演算子をオーバーロードする演算子プロシージャを定義します。  
  
 [!code-vb[VbVbcnProcedures#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#29)]  
  
 <xref:System.TimeSpan> は標準の `+` 演算子をオーバーロードするので、前の例では `combinedSpan`の値を計算するときに演算子プロシージャを呼び出しています。  
  
 メッセージ交換演算子プロシージャを呼び出す例については、「[方法: 演算子を定義するクラスを使用する](./how-to-use-a-class-that-defines-operators.md)」を参照してください。  
  
## <a name="compile-the-code"></a>コードのコンパイル  
 使用するクラスまたは構造体で、使用する演算子が定義されていることを確認してください。  
  
## <a name="see-also"></a>関連項目

- [演算子プロシージャ](./operator-procedures.md)
- [方法 : 演算子を定義する](./how-to-define-an-operator.md)
- [方法 : 変換演算子を定義する](./how-to-define-a-conversion-operator.md)
- [Operator ステートメント](../../../../visual-basic/language-reference/statements/operator-statement.md)
- [Widening](../../../../visual-basic/language-reference/modifiers/widening.md)
- [Narrowing](../../../../visual-basic/language-reference/modifiers/narrowing.md)
- [Structure ステートメント](../../../../visual-basic/language-reference/statements/structure-statement.md)
- [方法 : 構造体を宣言する](../../../../visual-basic/programming-guide/language-features/data-types/how-to-declare-a-structure.md)
- [暗黙の型変換と明示的な型変換](../../../../visual-basic/programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
- [拡大変換と縮小変換](../../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
