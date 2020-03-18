---
title: Function 式
ms.date: 07/20/2015
helpviewer_keywords:
- Function expression [Visual Basic]
- functions [Visual Basic], function expressions
- lambda expressions [Visual Basic], function expression
ms.assetid: e8a47a45-4b8a-4f45-a623-7653625dffbc
ms.openlocfilehash: 454c4e3d926640934a8edc4fcb16e4308a89dd50
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75632338"
---
# <a name="function-expression-visual-basic"></a>Function 式 (Visual Basic)
関数ラムダ式を定義するパラメーターとコードを宣言します。  
  
## <a name="syntax"></a>構文  
  
```vb  
Function ( [ parameterlist ] ) expression  
- or -  
Function ( [ parameterlist ] )  
  [ statements ]  
End Function  
```  
  
## <a name="parts"></a>のコンポーネント  
  
|用語|Definition|  
|---|---|  
|`parameterlist`|省略可。 このプロシージャのパラメーターを表すローカル変数名の一覧です。 リストが空の場合でも、かっこは存在する必要があります。 「[パラメーターリスト](../../../visual-basic/language-reference/statements/parameter-list.md)」を参照してください。|  
|`expression`|必ず指定します。 1つの式。 式の型は、関数の戻り値の型です。|  
|`statements`|必ず指定します。 `Return` ステートメントを使用して値を返すステートメントの一覧。 ( [Return ステートメント](../../../visual-basic/language-reference/statements/return-statement.md)を参照してください)。返される値の型は、関数の戻り値の型です。|  
  
## <a name="remarks"></a>コメント  
 *ラムダ式*は、値を計算して返す名前のない関数です。 ラムダ式は、デリゲート型を使用できる場所であればどこでも使用できます。ただし、`RemoveHandler`の引数として使用することはできません。 デリゲートの詳細と、デリゲートでのラムダ式の使用については、「[デリゲートステートメント](../../../visual-basic/language-reference/statements/delegate-statement.md)」および「厳密でない[デリゲート変換](../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)」を参照してください。  
  
## <a name="lambda-expression-syntax"></a>ラムダ式の構文  
 ラムダ式の構文は、標準関数の構文に似ています。 相違点は、次のとおりです。  
  
- ラムダ式に名前がありません。  
  
- ラムダ式には、`Overloads` や `Overrides`などの修飾子を含めることはできません。  
  
- ラムダ式では、関数の戻り値の型を指定するために `As` 句を使用しません。 代わりに、型は、単一行のラムダ式の結果が評価される値、または複数行ラムダ式の戻り値から推論されます。 たとえば、単一行のラムダ式の本体が `Where cust.City = "London"`場合、その戻り値の型は `Boolean`になります。  
  
- 単一行のラムダ式の本体は、ステートメントではなく、式である必要があります。 本文は、関数プロシージャの呼び出しで構成できますが、サブプロシージャの呼び出しでは使用できません。  
  
- すべてのパラメーターのデータ型が指定されているか、すべてのパラメーターが推論される必要があります。  
  
- 省略可能なおよび Paramarray パラメーターは使用できません。  
  
- ジェネリックパラメーターは使用できません。  
  
## <a name="example"></a>使用例  
 次の例は、単純なラムダ式を作成する2つの方法を示しています。 最初のは、`Dim` を使用して、関数の名前を指定します。 関数を呼び出すには、パラメーターの値を送信します。  
  
 [!code-vb[VbVbalrLambdas#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#1)]  
  
 [!code-vb[VbVbalrLambdas#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#2)]  
  
## <a name="example"></a>使用例  
 または、関数を同時に宣言して実行することもできます。  
  
 [!code-vb[VbVbalrLambdas#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#3)]  
  
## <a name="example"></a>使用例  
 引数をインクリメントし、値を返すラムダ式の例を次に示します。 この例では、関数の単一行と複数行のラムダ式の構文を示します。 その他の例については、「[ラムダ式](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)」を参照してください。  
  
 [!code-vb[VbVbalrLambdas#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#14)]  
  
## <a name="example"></a>使用例  
 ラムダ式は、統合言語クエリ (LINQ) のクエリ演算子の多くを基にしており、メソッドベースのクエリで明示的に使用することができます。 次の例では、一般的な LINQ クエリを示し、その後にクエリをメソッド形式に変換します。  
  
```vb  
Dim londonCusts = From cust In db.Customers  
                       Where cust.City = "London"  
                       Select cust  
  
' This query is compiled to the following code:  
Dim londonCusts = db.Customers.  
                  Where(Function(cust) cust.City = "London").  
                  Select(Function(cust) cust)  
```  
  
 クエリメソッドの詳細については、「[クエリ](../../../visual-basic/language-reference/queries/index.md)」を参照してください。 標準クエリ演算子の詳細については、「[標準クエリ演算子の概要](../../programming-guide/concepts/linq/standard-query-operators-overview.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)
- [ラムダ式](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)
- [演算子および式](../../../visual-basic/programming-guide/language-features/operators-and-expressions/index.md)
- [ステートメント](../../../visual-basic/programming-guide/language-features/statements.md)
- [値の比較](../../../visual-basic/programming-guide/language-features/operators-and-expressions/value-comparisons.md)
- [ブール式](../../../visual-basic/programming-guide/language-features/operators-and-expressions/boolean-expressions.md)
- [If 演算子](../../../visual-basic/language-reference/operators/if-operator.md)
- [厳密でないデリゲート変換](../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)
