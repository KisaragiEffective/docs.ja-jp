---
title: Sub プロシージャ
ms.date: 07/20/2015
helpviewer_keywords:
- Sub procedures [Visual Basic], about Sub procedures
- statement blocks
- Sub procedures [Visual Basic], calling
- procedures [Visual Basic], calling
- Sub procedures [Visual Basic], syntax
- Sub procedures
- procedures [Visual Basic], Sub
- syntax [Visual Basic], Sub procedures
ms.assetid: 6a0a4958-ed0a-4d3d-8d31-0772c82bda58
ms.openlocfilehash: 9ca1d302a0bc8e989e0b2dddf8cce68e89211d57
ms.sourcegitcommit: 5d769956a04b6d68484dd717077fabc191c21da5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/17/2020
ms.locfileid: "76163814"
---
# <a name="sub-procedures-visual-basic"></a>Sub プロシージャ (Visual Basic)

`Sub` プロシージャは、`Sub` および `End Sub` ステートメントで囲まれた一連の Visual Basic ステートメントです。 `Sub` プロシージャはタスクを実行した後、呼び出し元のコードに制御を戻しますが、呼び出し元のコードには値を返しません。

プロシージャが呼び出されるたびに、ステートメントが実行されます。これは、`Sub` ステートメントの後の最初の実行可能ステートメントから開始し、最初の `End Sub`、`Exit Sub`、または `Return` ステートメントで終了します。

モジュール、クラス、および構造体で `Sub` プロシージャを定義できます。 既定では `Public`になっています。これは、アプリケーション内の任意の場所から、これを定義したモジュール、クラス、または構造体にアクセスできることを意味します。 *メソッド*という用語は、定義されているモジュール、クラス、または構造体の外部からアクセスされる、`Sub` または `Function` プロシージャを記述します。 詳細については、「[プロシージャ](./index.md)」を参照してください。

`Sub` プロシージャは、呼び出し元のコードによって渡される定数、変数、式などの引数を受け取ることができます。

## <a name="declaration-syntax"></a>宣言の構文

`Sub` プロシージャを宣言する構文は次のとおりです。

```vb
[modifiers] Sub SubName[(parameterList)]
    ' Statements of the Sub procedure.
End Sub
```

`modifiers` では、アクセスレベルと、オーバーロード、オーバーライド、共有、およびシャドウに関する情報を指定できます。 詳細については、「[Sub ステートメント](../../../language-reference/statements/sub-statement.md)」を参照してください。

## <a name="parameter-declaration"></a>パラメーターの宣言

パラメーター名とデータ型を指定して、変数を宣言するのと同じように各プロシージャパラメーターを宣言します。 また、渡す機構と、パラメーターが省略可能またはパラメーター配列であるかどうかを指定することもできます。

パラメーターリストの各パラメーターの構文は次のとおりです。

```vb
[Optional] [ByVal | ByRef] [ParamArray] parameterName As DataType
```

パラメーターが省略可能な場合は、宣言の一部として既定値を指定する必要もあります。 既定値を指定する構文は次のとおりです。

```vb
Optional [ByVal | ByRef]  parameterName As DataType = defaultValue
```

### <a name="parameters-as-local-variables"></a>ローカル変数としてのパラメーター

コントロールがプロシージャに渡されると、各パラメーターはローカル変数として扱われます。 これは、その有効期間がプロシージャの有効期間と同じであり、そのスコープがプロシージャ全体であることを意味します。

## <a name="calling-syntax"></a>呼び出し構文

`Sub` プロシージャは、スタンドアロンの呼び出しステートメントを使用して明示的に呼び出すことができます。 式で名前を使用して呼び出すことはできません。 省略可能なすべての引数の値を指定する必要があり、引数リストをかっこで囲む必要があります。 引数を指定しない場合は、必要に応じてかっこを省略できます。 `Call` キーワードの使用は省略可能ですが、推奨されません。

`Sub` プロシージャの呼び出しの構文は次のとおりです。

```vb
[Call] SubName[(argumentlist)]
```

`Sub` メソッドは、それを定義するクラスの外部から呼び出すことができます。 最初に、`New` キーワードを使用してクラスのインスタンスを作成するか、クラスのインスタンスを返すメソッドを呼び出す必要があります。 詳細については、「[New 演算子](../../../language-reference/operators/new-operator.md)」を参照してください。 その後、次の構文を使用して、インスタンスオブジェクトの `Sub` メソッドを呼び出すことができます。

```vb
object.MethodName[(argumentList)]
```

### <a name="illustration-of-declaration-and-call"></a>宣言と呼び出しの図

次の `Sub` 手順では、アプリケーションが実行しようとしているタスクをコンピューターオペレーターに指示し、タイムスタンプも表示します。 すべてのタスクの開始時にこのコードを複製するのではなく、アプリケーションはさまざまな場所から `tellOperator` を呼び出します。 各呼び出しは、開始されるタスクを識別する `task` 引数に文字列を渡します。

[!code-vb[VbVbcnProcedures#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#2)]

次の例は、`tellOperator`の一般的な呼び出しを示しています。

[!code-vb[VbVbcnProcedures#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#3)]

## <a name="see-also"></a>関連項目

- [手順](./index.md)
- [Function プロシージャ](./function-procedures.md)
- [Property プロシージャ](./property-procedures.md)
- [演算子プロシージャ](./operator-procedures.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [Sub ステートメント](../../../language-reference/statements/sub-statement.md)
- [方法 : 値を返さないプロシージャを呼び出す](./how-to-call-a-procedure-that-does-not-return-a-value.md)
- [方法: Visual Basic でイベントハンドラーを呼び出す](./how-to-call-an-event-handler.md)
