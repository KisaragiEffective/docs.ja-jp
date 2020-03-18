---
title: Sub 式 (Visual Basic)
ms.date: 07/20/2015
helpviewer_keywords:
- lambda expressions [Visual Basic], sub expression
- Sub Expression [Visual Basic]
- subroutines [Visual Basic], sub expressions
ms.assetid: 36b6bfd1-6539-4d8f-a5eb-6541a745ffde
ms.openlocfilehash: d284e629ea0b0a4e9b6eb9e098612bb07c38723b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350899"
---
# <a name="sub-expression-visual-basic"></a>Sub 式 (Visual Basic)
サブルーチンラムダ式を定義するパラメーターとコードを宣言します。  
  
## <a name="syntax"></a>構文  
  
```vb  
Sub ( [ parameterlist ] ) statement  
- or -  
Sub ( [ parameterlist ] )  
  [ statements ]  
End Sub  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|---|---|  
|`parameterlist`|省略可。 プロシージャのパラメーターを表すローカル変数名の一覧です。 リストが空の場合でも、かっこは存在する必要があります。 詳細については、「[パラメーター リスト](../../../visual-basic/language-reference/statements/parameter-list.md)」を参照してください。|  
|`statement`|必須。 1つのステートメント。|  
|`statements`|必須。 ステートメントの一覧。|  
  
## <a name="remarks"></a>コメント  
 *ラムダ式*は、名前がなく、1つ以上のステートメントを実行するサブルーチンです。 ラムダ式は、デリゲート型を使用できる場所であればどこでも使用できます。ただし、`RemoveHandler`の引数として使用することはできません。 デリゲートの詳細と、デリゲートでのラムダ式の使用については、「[Delegate ステートメント](../../../visual-basic/language-reference/statements/delegate-statement.md)」および「[厳密でないデリゲート変換](../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)」を参照してください。  
  
## <a name="lambda-expression-syntax"></a>ラムダ式の構文  
 ラムダ式の構文は、標準のサブルーチンの構文に似ています。 相違点は、次のとおりです。  
  
- ラムダ式に名前がありません。  
  
- ラムダ式には、`Overloads` や `Overrides`などの修飾子を含めることはできません。  
  
- 単一行のラムダ式の本体は、式ではなく、ステートメントでなければなりません。 本文は、Sub プロシージャの呼び出しで構成できますが、Function プロシージャを呼び出すことはできません。  
  
- ラムダ式では、すべてのパラメーターのデータ型が指定されているか、すべてのパラメーターが推論される必要があります。  
  
- ラムダ式では、省略可能なパラメーターと `ParamArray` パラメーターは使用できません。  
  
- ラムダ式ではジェネリックパラメーターは使用できません。  
  
## <a name="example"></a>例  
 値をコンソールに書き込むラムダ式の例を次に示します。 この例では、サブルーチンの単一行と複数行のラムダ式の構文の両方を示しています。 その他の例については、「[ラムダ式](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)」を参照してください。  
  
 [!code-vb[VbVbalrLambdas#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#15)]  
  
## <a name="see-also"></a>参照

- [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)
- [ラムダ式](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)
- [演算子および式](../../../visual-basic/programming-guide/language-features/operators-and-expressions/index.md)
- [ステートメント](../../../visual-basic/programming-guide/language-features/statements.md)
- [厳密でないデリゲート変換](../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)
