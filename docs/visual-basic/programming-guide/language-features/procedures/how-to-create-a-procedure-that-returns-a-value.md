---
title: '方法 : 値を返すプロシージャを作成する'
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- procedures [Visual Basic], returning a value
ms.assetid: 8ee19f95-a9ef-4033-963b-d224dca207c4
ms.openlocfilehash: 218dbb52abc0100724d38d10be91ef24252d5226
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349722"
---
# <a name="how-to-create-a-procedure-that-returns-a-value-visual-basic"></a>方法: 値を返すプロシージャを作成する (Visual Basic)
呼び出し元のコードに値を返すには、`Function` プロシージャを使用します。  
  
### <a name="to-create-a-procedure-that-returns-a-value"></a>値を返すプロシージャを作成するには  
  
1. 他のプロシージャの外部では、`Function` ステートメントを使用し、その後に `End Function` ステートメントを使用します。  
  
2. `Function` ステートメントで、`Function` キーワードにプロシージャの名前を指定し、その後にかっこで囲んだパラメーターリストを指定します。  
  
3. かっこの後に `As` 句を入力し、戻り値のデータ型を指定します。  
  
4. プロシージャのコードステートメントを `Function` と `End Function` ステートメントの間に配置します。  
  
5. `Return` ステートメントを使用して、呼び出し元のコードに値を返します。  
  
     次の `Function` プロシージャは、他の2つの辺の値を指定して、直角三角形の最長の辺 (斜辺) を計算します。  
  
     [!code-vb[VbVbcnProcedures#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#1)]  
  
     次の例は、`hypotenuse`の一般的な呼び出しを示しています。  
  
     [!code-vb[VbVbcnProcedures#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [Sub プロシージャ](./sub-procedures.md)
- [Property プロシージャ](./property-procedures.md)
- [演算子プロシージャ](./operator-procedures.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [Function ステートメント](../../../../visual-basic/language-reference/statements/function-statement.md)
- [方法 : プロシージャから値を返す](./how-to-return-a-value-from-a-procedure.md)
- [方法: 値を返すプロシージャを呼び出す](./how-to-call-a-procedure-that-returns-a-value.md)
