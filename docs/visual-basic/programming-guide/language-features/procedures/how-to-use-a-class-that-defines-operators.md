---
title: '方法: 演算子を定義するクラスを使用する'
ms.date: 07/20/2015
helpviewer_keywords:
- operator procedures [Visual Basic], calling
- procedures [Visual Basic], operator
- procedures [Visual Basic], calling
- examples [Visual Basic], CType
- syntax [Visual Basic], Operator procedures
- operators [Visual Basic], overloading
- return values [Visual Basic], Operator procedures
- operator overloading
ms.assetid: 7ccce94a-6ca0-47d1-9f3f-13385d34f5d5
ms.openlocfilehash: 455c839702b90738ec5aea37c1b09d72eba42ff4
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347888"
---
# <a name="how-to-use-a-class-that-defines-operators-visual-basic"></a>方法: 演算子を定義するクラスを使用する (Visual Basic)
独自の演算子を定義するクラスまたは構造体を使用している場合は、Visual Basic からこれらの演算子にアクセスできます。  
  
 クラスまたは構造体に対して演算子を定義することは、演算子の*オーバーロード*とも呼ばれます。  
  
## <a name="example"></a>使用例  
 次の例では、sql 構造 <xref:System.Data.SqlTypes.SqlString>にアクセスします。これにより、SQL 文字列と Visual Basic 文字列の双方向の変換演算子 ([CType 関数](../../../../visual-basic/language-reference/functions/ctype-function.md)) が定義されます。 `CType(`*sql 文字列式*、`String)` を使用して sql 文字列を Visual Basic 文字列に変換し、Visual Basic*文字列式*を `CType(`して、他の方向に変換します。  
  
 [!code-vb[VbVbcnProcedures#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#30)]  
  
 [!code-vb[VbVbcnProcedures#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#31)]  
  
 <xref:System.Data.SqlTypes.SqlString> 構造体は、`String` から <xref:System.Data.SqlTypes.SqlString> への変換演算子 (CType 関数) と、<xref:System.Data.SqlTypes.SqlString> から `String`への変換演算子 ([CType 関数](../../../../visual-basic/language-reference/functions/ctype-function.md)) を定義します。 `jobTitle` に `title` を割り当てるステートメントは、最初の演算子を使用し、<xref:Microsoft.VisualBasic.Interaction.MsgBox%2A> 関数呼び出しは2番目の演算子を使用します。  
  
## <a name="compile-the-code"></a>コードのコンパイル  
 使用するクラスまたは構造体で、使用する演算子が定義されていることを確認してください。 クラスまたは構造体で、オーバーロードに使用できるすべての演算子が定義されていると想定しないでください。 使用可能な演算子の一覧については、「 [Operator ステートメント](../../../../visual-basic/language-reference/statements/operator-statement.md)」を参照してください。  
  
 ソースファイルの先頭にある SQL 文字列に適切な `Imports` ステートメント (この場合は <xref:System.Data.SqlTypes>) を含めます。  
  
 プロジェクトは、system.string と SYSTEM.XML への参照を持っている必要があります。  
  
## <a name="see-also"></a>関連項目

- [演算子プロシージャ](./operator-procedures.md)
- [方法 : 演算子を定義する](./how-to-define-an-operator.md)
- [方法 : 変換演算子を定義する](./how-to-define-a-conversion-operator.md)
- [方法 : 演算子プロシージャを呼び出す](./how-to-call-an-operator-procedure.md)
- [Widening](../../../../visual-basic/language-reference/modifiers/widening.md)
- [Narrowing](../../../../visual-basic/language-reference/modifiers/narrowing.md)
- [Structure ステートメント](../../../../visual-basic/language-reference/statements/structure-statement.md)
- [方法 : 構造体を宣言する](../../../../visual-basic/programming-guide/language-features/data-types/how-to-declare-a-structure.md)
- [暗黙の型変換と明示的な型変換](../../../../visual-basic/programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
- [拡大変換と縮小変換](../../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
