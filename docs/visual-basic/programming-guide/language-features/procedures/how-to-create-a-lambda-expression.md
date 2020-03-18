---
title: '方法 : ラムダ式を作成する'
ms.date: 07/20/2015
helpviewer_keywords:
- lambda expressions [Visual Basic]
- expressions [Visual Basic], lambda
ms.assetid: 3279bd5c-80f7-410a-a7ba-f7085ed36aa5
ms.openlocfilehash: 1c65841e4c124252cfa41bcd4d0c305a426687ee
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75632351"
---
# <a name="how-to-create-a-lambda-expression-visual-basic"></a>方法: ラムダ式を作成する (Visual Basic)
*ラムダ式*は、名前のない関数またはサブルーチンです。 ラムダ式は、デリゲート型が有効であればどこでも使用できます。  
  
### <a name="to-create-a-single-line-lambda-expression-function"></a>単一行のラムダ式関数を作成するには  
  
1. デリゲート型を使用できる状況では、次の例に示すように、キーワード `Function`を入力します。  
  
     `Dim add1 =`   `Function`  
  
2. かっこ内で `Function`直後に、関数のパラメーターを入力します。 `Function`の後に名前を指定しないことに注意してください。  
  
     `Dim add1 = Function`   `(num As Integer)`  
  
3. パラメーターリストの後に、関数の本体として1つの式を入力します。 式が評価される値は、関数によって返される値です。 戻り値の型を指定するために `As` 句は使用しません。  
  
     [!code-vb[VbVbalrLambdas#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#1)]  
  
     ラムダ式を呼び出すには、整数の引数を渡します。  
  
     [!code-vb[VbVbalrLambdas#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#2)]  
  
4. また、次の例でも同じ結果が得られます。  
  
     [!code-vb[VbVbalrLambdas#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#3)]  
  
### <a name="to-create-a-single-line-lambda-expression-subroutine"></a>単一行のラムダ式のサブルーチンを作成するには  
  
1. デリゲート型を使用できる状況では、次の例に示すように、キーワード `Sub`を入力します。  
  
     `Dim add1 =`   `Sub`  
  
2. かっこ内で `Sub`直後に、サブルーチンのパラメーターを入力します。 `Sub`の後に名前を指定しないことに注意してください。  
  
     `Dim add1 = Sub`   `(msg As String)`  
  
3. パラメーターリストの後に、サブルーチンの本体として1つのステートメントを入力します。  
  
     [!code-vb[VbVbalrLambdas#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#17)]  
  
     ラムダ式を呼び出すには、文字列引数を渡します。  
  
     [!code-vb[VbVbalrLambdas#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#18)]  
  
### <a name="to-create-a-multiline-lambda-expression-function"></a>複数行ラムダ式関数を作成するには  
  
1. デリゲート型を使用できる状況では、次の例に示すように、キーワード `Function`を入力します。  
  
     `Dim add1 =`   `Function`  
  
2. かっこ内で `Function`直後に、関数のパラメーターを入力します。 `Function`の後に名前を指定しないことに注意してください。  
  
     `Dim add1 = Function`   `(index As Integer)`  
  
3. ENTER キーを押します。 `End Function` ステートメントが自動的に追加されます。  
  
4. 関数の本体内で、次のコードを追加して式を作成し、値を返します。 戻り値の型を指定するために `As` 句は使用しません。  
  
     [!code-vb[VbVbalrLambdas#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#19)]  
  
     ラムダ式を呼び出すには、整数の引数を渡します。  
  
     [!code-vb[VbVbalrLambdas#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#20)]  
  
### <a name="to-create-a-multiline-lambda-expression-subroutine"></a>複数行ラムダ式のサブルーチンを作成するには  
  
1. デリゲート型を使用できる状況では、次の例に示すように、キーワード `Sub`を入力します。  
  
     `Dim add1 =`   `Sub`  
  
2. かっこ内で `Sub`直後に、サブルーチンのパラメーターを入力します。 `Sub`の後に名前を指定しないことに注意してください。  
  
     `Dim add1 = Sub`  `(msg As String)`  
  
3. ENTER キーを押します。 `End Sub` ステートメントが自動的に追加されます。  
  
4. 関数の本体内に、サブルーチンが呼び出されたときに実行する次のコードを追加します。  
  
     [!code-vb[VbVbalrLambdas#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#21)]  
  
     ラムダ式を呼び出すには、文字列引数を渡します。  
  
     [!code-vb[VbVbalrLambdas#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#22)]  
  
## <a name="example"></a>使用例  
 ラムダ式の一般的な用途は、型が `Delegate`パラメーターの引数として渡すことができる関数を定義することです。 次の例では、<xref:System.Diagnostics.Process.GetProcesses%2A> メソッドによって、ローカルコンピューター上で実行されているプロセスの配列が返されます。 <xref:System.Linq.Enumerable> クラスの <xref:System.Linq.Enumerable.Where%2A> メソッドには、引数として `Boolean` デリゲートが必要です。 この例のラムダ式は、この目的に使用されます。 このメソッドは、1つのスレッドのみを持ち、`filteredList`で選択されているプロセスごとに `True` を返します。  
  
 [!code-vb[VbVbalrLambdas#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class4.vb#10)]  
  
 前の例は、統合言語クエリ (LINQ) 構文で記述された次のコードと同じです。  
  
 [!code-vb[VbVbalrLambdas#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class5.vb#11)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Linq.Enumerable>
- [ラムダ式](./lambda-expressions.md)
- [Function ステートメント](../../../../visual-basic/language-reference/statements/function-statement.md)
- [Sub ステートメント](../../../../visual-basic/language-reference/statements/sub-statement.md)
- [デリゲート](../../../../visual-basic/programming-guide/language-features/delegates/index.md)
- 方法 : [Visual Basic でプロシージャを別のプロシージャに渡す](../../../../visual-basic/programming-guide/language-features/delegates/how-to-pass-procedures-to-another-procedure.md)
- [Delegate ステートメント](../../../../visual-basic/language-reference/statements/delegate-statement.md)
- [Visual Basic における LINQ の概要](../../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
