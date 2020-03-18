---
title: Function プロシージャ
ms.date: 07/20/2015
helpviewer_keywords:
- Function procedures
- return values [Visual Basic], function procedures
- Visual Basic code, procedures
- procedures [Visual Basic], calling
- procedures [Visual Basic], Function procedures
- syntax [Visual Basic], function procedures
ms.assetid: 1b9f632c-553b-4cb6-920a-ded117ead8c0
ms.openlocfilehash: d7a0293e2ec520c2278f67156be56315d1def2b5
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76780078"
---
# <a name="function-procedures-visual-basic"></a>Function プロシージャ (Visual Basic)

`Function` プロシージャは、`Function` および `End Function` ステートメントで囲まれた一連の Visual Basic ステートメントです。 `Function` プロシージャはタスクを実行し、呼び出し元のコードに制御を戻します。 コントロールが返されると、呼び出し元のコードにも値が返されます。

プロシージャが呼び出されるたびに、ステートメントが実行され、`Function` ステートメントの後の最初の実行可能ステートメントから開始し、最初の `End Function`、`Exit Function`、または `Return` ステートメントで終了します。

モジュール、クラス、または構造体で `Function` プロシージャを定義できます。 既定では `Public` されています。これは、アプリケーション内の任意の場所から、これを定義したモジュール、クラス、または構造体にアクセスできることを意味します。

`Function` プロシージャは、呼び出し元のコードによって渡される定数、変数、式などの引数を受け取ることができます。

## <a name="declaration-syntax"></a>宣言の構文

`Function` プロシージャを宣言する構文は次のとおりです。

```vb
[Modifiers] Function FunctionName [(ParameterList)] As ReturnType
    [Statements]
End Function
```

*修飾子*では、アクセスレベルと、オーバーロード、オーバーライド、共有、およびシャドウに関する情報を指定できます。 詳細については、「 [Function ステートメント](../../../language-reference/statements/function-statement.md)」を参照してください。

各パラメーターは、[サブプロシージャ](./sub-procedures.md)に対して実行するのと同じ方法で宣言します。

### <a name="data-type"></a>[データ型]

すべての `Function` プロシージャには、すべての変数と同様にデータ型があります。 このデータ型は、`Function` ステートメントの `As` 句によって指定され、関数が呼び出し元のコードに返す値のデータ型を決定します。 この例を次の宣言に示します。

```vb
Function Yesterday() As Date
End Function

Function FindSqrt(radicand As Single) As Single
End Function
```

詳細については、「 [Function ステートメント](../../../language-reference/statements/function-statement.md)」の「Parts」を参照してください。

### <a name="returning-values"></a>戻り値

`Function` プロシージャから呼び出し元のコードに返される値は、戻り値と呼ばれます。 このプロシージャは、次の2つの方法のいずれかでこの値を返します。

- `Return` ステートメントを使用して戻り値を指定し、呼び出し元のプログラムに制御を直ちに返します。 これを次の例に示します。

  ```vb
  Function FunctionName [(ParameterList)] As ReturnType
      ' The following statement immediately transfers control back
      ' to the calling code and returns the value of Expression.
      Return Expression
  End Function
  ```

- このメソッドは、プロシージャの1つ以上のステートメントで、独自の関数名に値を割り当てます。 `Exit Function` または `End Function` ステートメントが実行されるまで、コントロールは呼び出し元のプログラムに戻りません。 これを次の例に示します。

  ```vb
  Function FunctionName [(ParameterList)] As ReturnType
      ' The following statement does not transfer control back to the calling code.
      FunctionName = Expression
      ' When control returns to the calling code, Expression is the return value.
  End Function
  ```

関数名に戻り値を代入する利点は、コントロールが、`Exit Function` または `End Function` ステートメントを検出するまでプロシージャから戻らないことです。 これにより、必要に応じて暫定値を割り当て、後で調整することができます。

値を返す方法の詳細については、「 [Function ステートメント](../../../language-reference/statements/function-statement.md)」を参照してください。 配列を返す方法については、「[配列](../arrays/index.md)」を参照してください。

## <a name="calling-syntax"></a>呼び出し構文

`Function` プロシージャを呼び出すには、代入ステートメントの右側または式に名前と引数を含めます。 省略可能なすべての引数の値を指定する必要があり、引数リストをかっこで囲む必要があります。 引数を指定しない場合は、必要に応じてかっこを省略できます。

`Function` プロシージャの呼び出しの構文は次のとおりです。

*左辺*値`=`*functionname* `[(` *argumentlist* `)]`

`If ((` *functionname* `[(` *argumentlist* `)] / 3) <=`*式*`) Then`

`Function` プロシージャを呼び出す場合、その戻り値を使用する必要はありません。 そうしないと、関数のすべてのアクションが実行されますが、戻り値は無視されます。 <xref:Microsoft.VisualBasic.Interaction.MsgBox%2A> は、多くの場合、この方法で呼び出されます。

### <a name="illustration-of-declaration-and-call"></a>宣言と呼び出しの図

次の `Function` プロシージャは、他の2つの辺の値を指定して、直角三角形の最長の辺 (斜辺) を計算します。

[!code-vb[VbVbcnProcedures#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#1)]

次の例は、`hypotenuse`の一般的な呼び出しを示しています。

[!code-vb[VbVbcnProcedures#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#6)]

## <a name="see-also"></a>関連項目

- [手順](./index.md)
- [Sub プロシージャ](./sub-procedures.md)
- [Property プロシージャ](./property-procedures.md)
- [演算子プロシージャ](./operator-procedures.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [Function ステートメント](../../../../visual-basic/language-reference/statements/function-statement.md)
- [方法 : 値を返すプロシージャを作成する](./how-to-create-a-procedure-that-returns-a-value.md)
- [方法 : プロシージャから値を返す](./how-to-return-a-value-from-a-procedure.md)
- [方法: 値を返すプロシージャを呼び出す](./how-to-call-a-procedure-that-returns-a-value.md)
