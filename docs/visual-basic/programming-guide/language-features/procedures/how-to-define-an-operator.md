---
title: '方法 : 演算子を定義する'
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- operators [Visual Basic], defining
- procedures [Visual Basic], operator
- Visual Basic code, operators
- syntax [Visual Basic], Operator procedures
- operators [Visual Basic], overloading
- operator procedures [Visual Basic], about operator procedures
- return values [Visual Basic], Operator procedures
- operator overloading
ms.assetid: d4b0e253-092a-4e6e-9fe2-01f562140a29
ms.openlocfilehash: b99af8ff4d5428f1749bfc1a4c51a136f12405ee
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344872"
---
# <a name="how-to-define-an-operator-visual-basic"></a>方法: 演算子を定義する (Visual Basic)
クラスまたは構造体を定義している場合は、オペランドの1つまたは両方がクラスまたは構造体の型である場合に、標準の演算子 (`*`、`<>`、`And`など) の動作を定義できます。  
  
 クラスまたは構造体内で、標準の演算子を演算子プロシージャとして定義します。 すべての演算子プロシージャは `Shared``Public` である必要があります。  
  
 クラスまたは構造体に対して演算子を定義することは、演算子の*オーバーロード*とも呼ばれます。  
  
## <a name="example"></a>例  
 次の例では、`height`と呼ばれる構造体の `+` 演算子を定義します。 この構造体は、フィートとインチで計測された高さを使用します。 1*インチ*は2.54 センチメートル、1*フィート*は12インチです。 正規化された値 (インチ < 12.0) を確保するために、コンストラクターは*モジュロ*12 の算術演算を実行します。 `+` 演算子は、コンストラクターを使用して正規化された値を生成します。  
  
 [!code-vb[VbVbcnProcedures#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#25)]  
  
 構造 `height` をテストするには、次のコードを使用します。  
  
 [!code-vb[VbVbcnProcedures#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#26)]  

## <a name="see-also"></a>関連項目

- [演算子プロシージャ](./operator-procedures.md)
- [方法 : 変換演算子を定義する](./how-to-define-a-conversion-operator.md)
- [方法 : 演算子プロシージャを呼び出す](./how-to-call-an-operator-procedure.md)
- [方法: 演算子を定義するクラスを使用する](./how-to-use-a-class-that-defines-operators.md)
- [Operator ステートメント](../../../../visual-basic/language-reference/statements/operator-statement.md)
- [Structure ステートメント](../../../../visual-basic/language-reference/statements/structure-statement.md)
- [方法 : 構造体を宣言する](../../../../visual-basic/programming-guide/language-features/data-types/how-to-declare-a-structure.md)
- [Mod 演算子](../../../../visual-basic/language-reference/operators/mod-operator.md)
