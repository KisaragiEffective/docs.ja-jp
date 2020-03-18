---
title: ラムダ式
ms.date: 07/20/2015
f1_keywords:
- vb.LambdaFunction
helpviewer_keywords:
- functions [Visual Basic], lambda expressions
- lambda expressions [Visual Basic]
- expressions [Visual Basic], lambda
- inline functions [Visual Basic]
ms.assetid: 137064b0-3928-4bfa-ba71-c3f9cbd951e2
ms.openlocfilehash: f3f963167e1b3633cc5fe6e1f435e374cd272cce
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345976"
---
# <a name="lambda-expressions-visual-basic"></a>ラムダ式 (Visual Basic)

*ラムダ式*は、デリゲートが有効な場所であれば使用できる名前のない関数またはサブルーチンです。 ラムダ式は関数またはサブルーチンにすることができ、単一行または複数行にすることができます。 現在のスコープからラムダ式に値を渡すことができます。

> [!NOTE]
> `RemoveHandler` ステートメントは例外です。 `RemoveHandler`のデリゲートパラメーターに対してラムダ式を渡すことはできません。

ラムダ式を作成するには、標準の関数またはサブルーチンを作成する場合と同様に、`Function` または `Sub` キーワードを使用します。 ただし、ラムダ式はステートメントに含まれています。

次の例は、引数をインクリメントして値を返すラムダ式です。 この例では、関数の単一行と複数行のラムダ式の構文の両方を示しています。

[!code-vb[VbVbalrLambdas#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#14)]

次の例は、値をコンソールに書き込むラムダ式です。 この例では、サブルーチンの単一行と複数行のラムダ式の構文の両方を示しています。

[!code-vb[VbVbalrLambdas#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#15)]

前の例では、ラムダ式は変数名に割り当てられていることに注意してください。 変数を参照するときは常に、ラムダ式を呼び出します。 次の例に示すように、同時にラムダ式を宣言して呼び出すこともできます。

[!code-vb[VbVbalrLambdas#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#3)]

ラムダ式は、次の例に示すように、関数呼び出しの値として返すことができます (このトピックで後述する[コンテキスト](#context)セクションの例に示すように)。または、デリゲート型を受け取るパラメーターに引数として渡されます。

[!code-vb[VbVbalrLambdas#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class2.vb#8)]

## <a name="lambda-expression-syntax"></a>ラムダ式の構文

ラムダ式の構文は、標準の関数またはサブルーチンの構文に似ています。 相違点は、次のとおりです。

- ラムダ式に名前がありません。

- ラムダ式には、`Overloads` や `Overrides`などの修飾子を含めることはできません。

- 単一行のラムダ関数は、戻り値の型を指定するために `As` 句を使用しません。 代わりに、ラムダ式の本体が評価される値から型が推論されます。 たとえば、ラムダ式の本体が `cust.City = "London"`場合、その戻り値の型は `Boolean`になります。

- 複数行のラムダ関数では、`As` 句を使用して戻り値の型を指定することも、戻り値の型が推論されるように `As` 句を省略することもできます。 複数行のラムダ関数に対して `As` 句を省略した場合、戻り値の型は、複数行のラムダ関数のすべての `Return` ステートメントから最も優先的な型になります。 最も優先される*型*は、他のすべての型の幅を広げることができる一意の型です。 この一意の型を特定できない場合、最も優先される型は、配列内の他のすべての型を絞り込むことができる一意の型になります。 これらの一意の型をどちらも特定できない場合は、 `Object`が最も優先度の高い型になります。 この場合、`Option Strict` が `On`に設定されていると、コンパイラエラーが発生します。

     たとえば、`Return` ステートメントに指定された式に `Integer`、`Long`、、および `Double`型の値が含まれている場合、結果の配列の型は `Double`になります。`Integer` と `Long` はどちらも `Double` に拡張され、`Double` のみになります。そのため、`Double` が最も優先度の高い型になります。 詳細については、「[拡大変換と縮小変換](../../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)」を参照してください。	


- 単一行関数の本体は、ステートメントではなく、値を返す式である必要があります。 単一行関数の `Return` ステートメントはありません。 単一行関数によって返される値は、関数本体の式の値です。

- 単一行のサブルーチンの本体は単一行ステートメントである必要があります。

- 単一行関数とサブルーチンには、`End Function` または `End Sub` ステートメントは含まれません。

- ラムダ式のパラメーターのデータ型は、`As` キーワードを使用して指定できます。また、パラメーターのデータ型を推論することもできます。 すべてのパラメーターのデータ型が指定されているか、すべてのパラメーターが推論される必要があります。

- `Optional` パラメーターと `Paramarray` パラメーターは使用できません。

- ジェネリックパラメーターは使用できません。

## <a name="async-lambdas"></a>非同期ラムダ

[Async](../../../../visual-basic/language-reference/modifiers/async.md)および[Await 演算子](../../../../visual-basic/language-reference/operators/await-operator.md)キーワードを使用して、非同期処理を組み込むラムダ式およびステートメントを簡単に作成できます。 たとえば、次に示す Windows フォーム例には、非同期メソッド `ExampleMethodAsync`を呼び出して待機するイベント ハンドラーが含まれています。

```vb
Public Class Form1

    Async Sub Button1_Click(sender As Object, e As EventArgs) Handles Button1.Click
        ' ExampleMethodAsync returns a Task.
        Await ExampleMethodAsync()
        TextBox1.Text = vbCrLf & "Control returned to button1_Click."
    End Sub

    Async Function ExampleMethodAsync() As Task
        ' The following line simulates a task-returning asynchronous process.
        Await Task.Delay(1000)
    End Function

End Class
```

[AddHandler ステートメント](../../../../visual-basic/language-reference/statements/addhandler-statement.md)で非同期ラムダを使用して、同じイベントハンドラーを追加できます。 次の例に示すように、このハンドラーを追加するには、ラムダ パラメーター リストの前に `Async` 修飾子を追加します。

```vb
Public Class Form1

    Private Sub Form1_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        AddHandler Button1.Click,
            Async Sub(sender1, e1)
                ' ExampleMethodAsync returns a Task.
                Await ExampleMethodAsync()
                TextBox1.Text = vbCrLf & "Control returned to Button1_ Click."
            End Sub
    End Sub

    Async Function ExampleMethodAsync() As Task
        ' The following line simulates a task-returning asynchronous process.
        Await Task.Delay(1000)
    End Function

End Class
```

非同期メソッドを作成して使用する方法の詳細については、「[async および Await を使用した非同期プログラミング](../../../../visual-basic/programming-guide/concepts/async/index.md)」を参照してください。

## <a name="context"></a>Context

ラムダ式は、そのコンテキストを定義されているスコープと共有します。 コンテナースコープで記述されたコードと同じアクセス権を持っています。 これには、コンテナースコープ内のメンバー変数、関数、および sub、`Me`、およびパラメーターとローカル変数へのアクセスが含まれます。

外側のスコープ内のローカル変数とパラメーターへのアクセスは、そのスコープの有効期間を超えて拡張できます。 ラムダ式を参照するデリゲートがガベージコレクションに使用できない限り、元の環境の変数へのアクセスは保持されます。 次の例では、変数 `target` が `makeTheGame`に対してローカルであり、ラムダ式 `playTheGame` が定義されているメソッドが定義されています。 `Main`の `takeAGuess` に割り当てられたラムダ式が、ローカル変数 `target`に引き続きアクセスできることに注意してください。

[!code-vb[VbVbalrLambdas#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class6.vb#12)]

次の例は、入れ子になったラムダ式の幅広いアクセス権を示しています。 返されたラムダ式が `aDel`として `Main` から実行されると、次の要素にアクセスします。

- 定義されているクラスのフィールド: `aField`

- 定義されているクラスのプロパティ: `aProp`

- 定義されているメソッド `functionWithNestedLambda`のパラメーター: `level1`

- `functionWithNestedLambda`のローカル変数: `localVar`

- 入れ子になっているラムダ式のパラメーター: `level2`

 [!code-vb[VbVbalrLambdas#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class3.vb#9)]

## <a name="converting-to-a-delegate-type"></a>デリゲート型への変換

ラムダ式は、互換性のあるデリゲート型に暗黙的に変換できます。 互換性に関する一般的な要件の詳細については、「[厳密でないデリゲート変換](../../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)」を参照してください。 たとえば、次のコード例は、`Func(Of Integer, Boolean)` または一致するデリゲートシグネチャに暗黙的に変換するラムダ式を示しています。

[!code-vb[VbVbalrLambdas#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#16)]

次のコード例は、`Sub(Of Double, String, Double)` または一致するデリゲートシグネチャに暗黙的に変換するラムダ式を示しています。

[!code-vb[VbVbalrLambdas#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/class7.vb#23)]

デリゲートにラムダ式を割り当てたり、プロシージャに引数として渡したりする場合は、パラメーター名を指定し、データ型を省略して、デリゲートから型を取得することができます。

## <a name="examples"></a>使用例

- 次の例では、null 許容型の引数に値が割り当てられている場合に `True` を返すラムダ式を定義し、その値が `Nothing`場合は `False` します。

     [!code-vb[VbVbalrLambdas#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#4)]

- 次の例では、配列内の最後の要素のインデックスを返すラムダ式を定義します。

     [!code-vb[VbVbalrLambdas#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#5)]

## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [Visual Basic における LINQ の概要](../../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [デリゲート](../../../../visual-basic/programming-guide/language-features/delegates/index.md)
- [Function ステートメント](../../../../visual-basic/language-reference/statements/function-statement.md)
- [Sub ステートメント](../../../../visual-basic/language-reference/statements/sub-statement.md)
- [null 許容値型](../../../../visual-basic/programming-guide/language-features/data-types/nullable-value-types.md)
- [方法 : Visual Basic でプロシージャを別のプロシージャに渡す](../../../../visual-basic/programming-guide/language-features/delegates/how-to-pass-procedures-to-another-procedure.md)
- [方法 : ラムダ式を作成する](./how-to-create-a-lambda-expression.md)
- [厳密でないデリゲート変換](../../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)
