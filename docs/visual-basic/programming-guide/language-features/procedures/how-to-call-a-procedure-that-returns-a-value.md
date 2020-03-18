---
title: '方法: 値を返すプロシージャを呼び出す'
ms.date: 07/20/2015
helpviewer_keywords:
- procedure calls [Visual Basic], returning values
- Visual Basic code, procedures
- procedures [Visual Basic], calling
- procedures [Visual Basic], returning a value
ms.assetid: a445127b-0f5f-465a-98fb-3e514b93d115
ms.openlocfilehash: 7f5d46babf31ea3c6babb29c0f1c08a23e51d598
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74340739"
---
# <a name="how-to-call-a-procedure-that-returns-a-value-visual-basic"></a>方法: 値を返すプロシージャを呼び出す (Visual Basic)
`Function` プロシージャは、呼び出し元のコードに値を返します。 これを呼び出すには、代入ステートメントの右側または式に名前と引数を含めます。  
  
### <a name="to-call-a-function-procedure-within-an-expression"></a>式内で関数プロシージャを呼び出すには  
  
1. `Function` プロシージャ名は、変数を使用する場合と同じ方法で使用します。 式で変数または定数を使用できる場所であればどこでも、`Function` プロシージャ呼び出しを使用できます。  
  
2. 引数リストを囲むには、プロシージャ名をかっこで囲みます。 引数がない場合は、必要に応じてかっこを省略できます。 ただし、かっこを使用すると、コードが読みやすくなります。  
  
3. 引数リストに引数をコンマで区切ってかっこ内に配置します。 引数は、`Function` プロシージャが対応するパラメーターを定義する順序と同じ順序で指定してください。  
  
     または、名前で1つ以上の引数を渡すこともできます。 詳細については、「[位置と名前による引数の引き渡し](./passing-arguments-by-position-and-by-name.md)」を参照してください。  
  
4. プロシージャから返される値は、変数または定数の値と同じように、式に含まれます。  
  
### <a name="to-call-a-function-procedure-in-an-assignment-statement"></a>代入ステートメントで Function プロシージャを呼び出すには  
  
1. 代入ステートメントで等号 (`=`) の後に `Function` プロシージャ名を使用します。  
  
2. 引数リストを囲むには、プロシージャ名をかっこで囲みます。 引数がない場合は、必要に応じてかっこを省略できます。 ただし、かっこを使用すると、コードが読みやすくなります。  
  
3. 引数リストに引数をコンマで区切ってかっこ内に配置します。 引数は、名前で渡す場合を除き、`Function` プロシージャが対応するパラメーターを定義する順序と同じ順序で指定してください。  
  
4. プロシージャから返される値は、代入ステートメントの左側にある変数またはプロパティに格納されます。  
  
## <a name="example"></a>例  
 次の例では、Visual Basic <xref:Microsoft.VisualBasic.Interaction.Environ%2A> を呼び出して、オペレーティングシステム環境変数の値を取得します。 最初の行は、式の中で `Environ` を呼び出し、2番目の行は代入ステートメントでそれを呼び出します。 `Environ` は、変数名を唯一の引数として受け取ります。 このメソッドは、呼び出し元のコードに変数の値を返します。  
  
 [!code-vb[VbVbcnProcedures#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#7)]  
  
## <a name="see-also"></a>参照

- [Function プロシージャ](./function-procedures.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [Function ステートメント](../../../../visual-basic/language-reference/statements/function-statement.md)
- [方法 : 値を返すプロシージャを作成する](./how-to-create-a-procedure-that-returns-a-value.md)
- [方法 : プロシージャから値を返す](./how-to-return-a-value-from-a-procedure.md)
- [方法 : 値を返さないプロシージャを呼び出す](./how-to-call-a-procedure-that-does-not-return-a-value.md)
