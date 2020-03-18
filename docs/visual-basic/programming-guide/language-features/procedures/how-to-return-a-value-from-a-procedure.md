---
title: '方法 : プロシージャから値を返す'
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- procedures [Visual Basic], returning from
- procedures [Visual Basic], returning a value
ms.assetid: 4bcc4724-2b4e-4df8-9b4b-16054607f87d
ms.openlocfilehash: 1371e4ed0ff28f9caf56eabf2a1bb9290edbe75c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346024"
---
# <a name="how-to-return-a-value-from-a-procedure-visual-basic"></a>方法: プロシージャから値を返す (Visual Basic)
`Function` プロシージャは、`Return` ステートメントを実行するか `Exit Function` または `End Function` ステートメントを実行して、呼び出し元のコードに値を返します。  
  
### <a name="to-return-a-value-using-the-return-statement"></a>Return ステートメントを使用して値を返すには  
  
1. プロシージャのタスクが完了した位置に `Return` ステートメントを配置します。  
  
2. `Return` キーワードの後に、呼び出し元のコードに返す値を生成する式を入力します。  
  
3. 同じプロシージャ内に複数の `Return` ステートメントを含めることができます。  
  
     次の `Function` の手順では、直角三角形の最長の辺 (斜辺) を計算し、呼び出し元のコードに返します。  
  
     [!code-vb[VbVbcnProcedures#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#1)]  
  
     次の例は、戻り値を格納する `hypotenuse`の一般的な呼び出しを示しています。  
  
     [!code-vb[VbVbcnProcedures#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#6)]  
  
### <a name="to-return-a-value-using-exit-function-or-end-function"></a>Exit 関数または End 関数を使用して値を返すには  
  
1. `Function` プロシージャ内の少なくとも1つの場所で、プロシージャの名前に値を割り当てます。  
  
2. `Exit Function` または `End Function` ステートメントを実行すると、Visual Basic は、プロシージャの名前に最後に割り当てられた値を返します。  
  
3. 同じプロシージャ内に複数の `Exit Function` ステートメントを含めることができ、さらに同じプロシージャ内に `Return` ステートメントと `Exit Function` ステートメントを混在させることができます。  
  
4. `Function` プロシージャには、`End Function` ステートメントを1つだけ含めることができます。  
  
     詳細と例については、「[Function ステートメント](../../../../visual-basic/language-reference/statements/function-statement.md)」の「戻り値」を参照してください。  
  
## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [Sub プロシージャ](./sub-procedures.md)
- [Property プロシージャ](./property-procedures.md)
- [演算子プロシージャ](./operator-procedures.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [Function ステートメント](../../../../visual-basic/language-reference/statements/function-statement.md)
- [Return ステートメント](../../../../visual-basic/language-reference/statements/return-statement.md)
- [方法 : 値を返すプロシージャを作成する](./how-to-create-a-procedure-that-returns-a-value.md)
- [方法: 値を返すプロシージャを呼び出す](./how-to-call-a-procedure-that-returns-a-value.md)
