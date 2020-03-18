---
title: お試しください...キャッチ...Finally ステートメント
description: Visual Basic Try/Catch/Finally ステートメントで例外処理を使用する方法について説明します。
ms.date: 12/07/2018
f1_keywords:
- vb.Try...Catch...Finally
- vb.when
- vb.Finally
- vb.Catch
- vb.Try
helpviewer_keywords:
- Try...Catch...Finally statements
- Try statement [Visual Basic]
- try-catch exception handling, Try...Catch...Finally statements
- error handling, while running code
- Try statement [Visual Basic], Try...Catch...Finally
- Finally keyword [Visual Basic], Try...Catch...Finally
- Catch statement [Visual Basic]
- When keyword [Visual Basic]
- Visual Basic code, handling errors while running
- structured exception handling, Try...Catch...Finally statements
ms.assetid: d6488026-ccb3-42b8-a810-0d97b9d6472b
ms.openlocfilehash: bb6f17f7ce88caea0b9d30ec880194f2bb71c6a6
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75705770"
---
# <a name="trycatchfinally-statement-visual-basic"></a>Try...Catch...Finally ステートメント (Visual Basic)

コードを実行したまま、特定のコードブロックで発生する可能性のあるエラーの一部またはすべてを処理する方法を提供します。

## <a name="syntax"></a>構文

```vb
Try
    [ tryStatements ]
    [ Exit Try ]
[ Catch [ exception [ As type ] ] [ When expression ]
    [ catchStatements ]
    [ Exit Try ] ]
[ Catch ... ]
[ Finally
    [ finallyStatements ] ]
End Try
```

## <a name="parts"></a>のコンポーネント

|用語|Definition|
|---|---|
|`tryStatements`|省略可。 エラーが発生する可能性のあるステートメント。 複合ステートメントにすることもできます。|
|`Catch`|省略可。 複数の `Catch` ブロックが許可されています。 `Try` ブロックの処理中に例外が発生した場合、各 `Catch` ステートメントは、スローされた例外を表す `exception` を使用して、例外を処理するかどうかを判断するためにテキスト順に検証されます。|
|`exception`|省略可。 任意の変数名を指定します。 `exception` の初期値は、スローされたエラーの値です。 キャッチされたエラーを指定するために `Catch` と共に使用します。 省略した場合、`Catch` ステートメントは例外をキャッチします。|
|`type`|省略可。 クラスフィルターの種類を指定します。 `exception` の値が、`type` または派生型のによって指定された型である場合、識別子は例外オブジェクトにバインドされます。|
|`When`|省略可。 `When` 句を含む `Catch` ステートメントは、`expression` が `True`に評価された場合にのみ例外をキャッチします。 `When` 句は、例外の型をチェックした後にのみ適用され、`expression` は例外を表す識別子を参照する場合があります。|
|`expression`|省略可。 `Boolean`に暗黙的に変換可能である必要があります。 汎用フィルターを記述する任意の式。 通常、エラー番号でフィルター処理するために使用されます。 `When` キーワードと共に使用して、エラーがキャッチされる状況を指定します。|
|`catchStatements`|省略可。 関連付けられている `Try` ブロックで発生したエラーを処理するステートメント。 複合ステートメントにすることもできます。|
|`Exit Try`|省略可。 `Try...Catch...Finally` 構造体を分割するキーワード。 `End Try` ステートメントの直後のコードで実行が再開されます。 `Finally` ステートメントは引き続き実行されます。 `Finally` ブロックでは許可されていません。|
|`Finally`|省略可。 `Finally` ブロックは、実行が `Try...Catch` ステートメントの任意の部分を離れると常に実行されます。|
|`finallyStatements`|省略可。 他のすべてのエラー処理が発生した後に実行されるステートメントです。|
|`End Try`|`Try...Catch...Finally` 構造体を終了します。|

## <a name="remarks"></a>Remarks

特定の例外がコードの特定のセクションで発生することが予想される場合は、コードを `Try` ブロックに配置し、`Catch` ブロックを使用してコントロールを保持し、発生した場合は例外を処理します。

`Try…Catch` ステートメントは、`Try` ブロックと、さまざまな例外のハンドラーを指定する1つ以上の `Catch` 句で構成されます。 `Try` ブロックで例外がスローされると、Visual Basic は例外を処理する `Catch` ステートメントを検索します。 一致する `Catch` ステートメントが見つからない場合、Visual Basic は、現在のメソッドを呼び出したメソッド、および呼び出し履歴を調べます。 `Catch` ブロックが見つからない場合、Visual Basic はハンドルされない例外メッセージをユーザーに表示し、プログラムの実行を停止します。

`Try…Catch` ステートメントでは、複数の `Catch` ステートメントを使用できます。 これを行うと、`Catch` 句の順序は順番に検査されるため、重要になります。 例外は、特殊性の高い順にキャッチしてください。

次の `Catch` ステートメントの条件は最も限定的ではなく、<xref:System.Exception> クラスから派生したすべての例外をキャッチします。 通常は、必要なすべての例外をキャッチした後、これらのバリエーションのいずれかを `Try...Catch...Finally` 構造の最後の `Catch` ブロックとして使用する必要があります。 制御フローは、これらのいずれかのバリエーションに従う `Catch` ブロックにはできません。

- `type` は `Exception`です。例: `Catch ex As Exception`

- ステートメントには `exception` の変数がありません。例: `Catch`

`Try…Catch…Finally` ステートメントが別の `Try` ブロックに入れ子になっている場合、Visual Basic は、最も内側の `Try` ブロック内の各 `Catch` ステートメントをまず調べます。 一致する `Catch` ステートメントが見つからない場合は、外側の `Try…Catch…Finally` ブロックの `Catch` ステートメントに進みます。

`Try` ブロックのローカル変数は、個別のブロックであるため、`Catch` ブロックでは使用できません。 複数のブロックで変数を使用する場合は、`Try...Catch...Finally` 構造体の外で変数を宣言します。

> [!TIP]
> `Try…Catch…Finally` ステートメントは、IntelliSense コードスニペットとして使用できます。 コードスニペットマネージャーで、[コードパターン-If] を展開し、[ **Catch]、[プロパティ]** などをクリックして、 **[エラー処理 (例外)]** をクリックします。 詳細については、「 [Code Snippets](/visualstudio/ide/code-snippets)」を参照してください。

## <a name="finally-block"></a>Finally ブロック

`Try` 構造体を終了する前に実行する必要のあるステートメントが1つ以上ある場合は、`Finally` ブロックを使用します。 制御は、`Try…Catch` 構造から渡される直前に `Finally` ブロックに渡されます。 これは、`Try` 構造内のどこかで例外が発生した場合でも当てはまります。

`Finally` ブロックは、例外が発生した場合でも実行する必要があるコードを実行する場合に便利です。 コントロールは、`Try...Catch` ブロックの終了方法に関係なく、`Finally` ブロックに渡されます。

`Finally` ブロック内のコードは、コードが `Try` または `Catch` ブロックで `Return` ステートメントを検出した場合でも実行されます。 次の場合、コントロールは、`Try` または `Catch` ブロックから対応する `Finally` ブロックに渡されません。

- `Try` または `Catch` ブロックで[End ステートメント](end-statement.md)が見つかりました。

- <xref:System.StackOverflowException>でスローされたが、`Try`または`Catch`ブロックします。

実行を明示的に `Finally` ブロックに転送することは無効です。 例外を除き、`Finally` ブロックの実行を転送することはできません。

`Try` ステートメントに少なくとも1つの `Catch` ブロックが含まれていない場合は、`Finally` ブロックが含まれている必要があります。

> [!TIP]
> 特定の例外をキャッチする必要がない場合、`Using` ステートメントは、ブロックを終了する方法に関係なく、`Try…Finally` ブロックのように動作し、リソースの破棄を保証します。 これは、ハンドルされない例外が発生した場合でも同様です。 詳細については、「[sing ステートメント](using-statement.md)」を参照してください。

## <a name="exception-argument"></a>Exception 引数

`Catch` block `exception` 引数は <xref:System.Exception> クラスのインスタンス、または `Exception` クラスから派生したクラスです。 `Exception` クラスのインスタンスは、`Try` ブロックで発生したエラーに対応します。

`Exception` オブジェクトのプロパティは、例外の原因と場所を特定するのに役立ちます。 たとえば、<xref:System.Exception.StackTrace%2A> プロパティには、例外の原因となったメソッドが一覧表示され、コード内でエラーが発生した場所を見つけるのに役立ちます。 <xref:System.Exception.Message%2A> は、例外を説明するメッセージを返します。 <xref:System.Exception.HelpLink%2A> は、関連付けられているヘルプファイルへのリンクを返します。 <xref:System.Exception.InnerException%2A> は、現在の例外の原因となった `Exception` オブジェクトを返すか、元の `Exception`がない場合は `Nothing` を返します。

## <a name="considerations-when-using-a-trycatch-statement"></a>Try…Catchステートメントを使用に関する注意点

`Try…Catch` ステートメントのみを使用して、異常なプログラムイベントまたは予期しないプログラムイベントの発生を通知します。 これには、次のような理由があります。

- 実行時に例外をキャッチするとオーバーヘッドが増加するため、例外を回避するための事前チェックよりも低速になる可能性があります。

- `Catch` ブロックが正しく処理されない場合、例外はユーザーに正しく報告されない可能性があります。

- 例外処理を使用すると、プログラムがより複雑になります。

常に、発生する可能性がある条件をチェックするために `Try…Catch` ステートメントは必要ありません。 次の例では、ファイルが存在するかどうかを確認してから開こうとします。 これにより、<xref:System.IO.File.OpenText%2A> メソッドによってスローされた例外をキャッチする必要がなくなります。

[!code-vb[VbVbalrStatements#94](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#94)]

スレッドセーフなログ記録と適切なメッセージのどちらを使用しても、`Catch` ブロック内のコードがユーザーに例外を正しく報告できることを確認します。 それ以外の場合、例外は不明なままになる可能性があります。

## <a name="async-methods"></a>非同期メソッド

[非同期](../modifiers/async.md)修飾子を使用してメソッドをマークした場合は、メソッドで[Await](../operators/await-operator.md)演算子を使用できます。 `Await` 演算子を持つステートメントは、待機中のタスクが完了するまでメソッドの実行を中断します。 このタスクは、進行中の作業を表します。 `Await` オペレーターに関連付けられているタスクが完了すると、同じ方法で実行が再開されます。 詳細については、「[非同期プログラムの制御フロー](../../../visual-basic/programming-guide/concepts/async/control-flow-in-async-programs.md)」を参照してください。

非同期メソッドによって返されるタスクは、未処理の例外によって完了したことを示す、faulted 状態で終了することがあります。 また、タスクは取り消された状態で終了することもあります。その結果、await 式から `OperationCanceledException` がスローされます。 いずれかの種類の例外をキャッチするには、タスクに関連付けられている `Await` 式を `Try` ブロックに配置し、`Catch` ブロックで例外をキャッチします。 例については、このトピックの後半で説明します。

複数の例外がエラーの原因になっているため、タスクが faulted 状態になることがあります。 たとえば、タスクは <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> の呼び出しの結果になることがあります。 このようなタスクを待機すると、キャッチされた例外は例外の1つにすぎないため、どの例外がキャッチされるかを予測することはできません。 例については、このトピックの後半で説明します。

`Await` 式は、`Catch` ブロックまたは `Finally` ブロック内には指定できません。

## <a name="iterators"></a>Iterators

反復子メソッドまたは`Get`アクセサーは、コレクションに対するカスタム イテレーションを実行します。 反復子は[Yield](yield-statement.md)ステートメントを使用して、コレクションの各要素を一度に 1 つずつ返します。 [For Each...Next ステートメント](for-each-next-statement.md)を使用して反復子メソッドを呼び出します。

`Yield`は`Try`ブロック内に置くことができます。 `Yield`ステートメントを含む`Try`ブロックは、`Catch`ブロックを持つことができ、そして、`Finally`ブロックを持つことができます。 例については、[反復子](../../programming-guide/concepts/iterators.md)の`Try`ブロックを参照してください。

`Yield`ステートメントは、`Catch`ブロックまたは`Finally`ブロック内で使用できません。

`For Each`本体 (反復子メソッドの外側)が例外をスローした場合、反復子メソッド内の`Catch`ブロックは実行されませんが、反復子メソッド内の`Finally`ブロックは実行されます。 反復子メソッド内の`Catch`ブロックは、反復子メソッド内で発生する例外のみをキャッチします。

## <a name="partial-trust-situations"></a>部分信頼の状況

部分的に信頼された状況 (ネットワーク共有でホストされているアプリケーションなど) では、`Try...Catch...Finally` は、呼び出しを含むメソッドが呼び出される前に発生したセキュリティ例外をキャッチしません。 次の例では、サーバー共有に配置し、そこから実行すると、"SecurityException: Request Failed" というエラーが生成されます。 セキュリティ例外の詳細については、<xref:System.Security.SecurityException> クラスを参照してください。

[!code-vb[VbVbalrStatements#85](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#85)]

このような部分信頼の場合は、`Process.Start` ステートメントを別の `Sub`に配置する必要があります。 `Sub` の最初の呼び出しは失敗します。 これにより、`Process.Start` を含む `Sub` が開始され、セキュリティ例外が生成される前に、`Try...Catch` をキャッチできます。

## <a name="examples"></a>使用例

### <a name="the-structure-of-trycatchfinally"></a>Try...Catch...Finallyの構造

次の例は`Try...Catch...Finally`ステートメントの構造を示しています。

[!code-vb[VbVbalrStatements#86](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#86)]  

### <a name="exception-in-a-method-called-from-a-try-block"></a>Try ブロックから呼び出されたメソッドで例外が発生しました

次の例では、`CreateException` メソッドによって `NullReferenceException`がスローされます。 例外を生成するコードが `Try` ブロックに含まれていません。 したがって、`CreateException` メソッドは例外を処理しません。 `RunSample` メソッドは、`CreateException` メソッドの呼び出しが `Try` ブロック内にあるため、例外を処理します。

この例には、いくつかの種類の例外の `Catch` ステートメントが含まれており、最も一般的なものから順に説明されています。

[!code-vb[VbVbalrStatements#91](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#91)]

### <a name="the-catch-when-statement"></a>Catch When ステートメント

次の例では、`Catch When` ステートメントを使用して条件式をフィルター処理する方法を示します。 条件式が `True`と評価された場合、`Catch` ブロック内のコードが実行されます。

[!code-vb[VbVbalrStatements#92](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#92)]

### <a name="nested-try-statements"></a>入れ子になった Try ステートメント

次の例では、`Try` ブロックに含まれる `Try…Catch` ステートメントを使用します。 内部 `Catch` ブロックは、`InnerException` プロパティが元の例外に設定されている例外をスローします。 外側の `Catch` ブロックは、独自の例外と内部例外を報告します。

[!code-vb[VbVbalrStatements#93](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#93)]

### <a name="exception-handling-for-async-methods"></a>非同期メソッドの例外処理

次の例では、非同期メソッドの例外処理を示します。 非同期タスクに適用される例外をキャッチするために、`Await` 式は呼び出し元の `Try` ブロックにあり、例外は `Catch` ブロックでキャッチされます。

例外処理を示すために、この例の `Throw New Exception` 行のコメントを解除します。 例外は `Catch` ブロックでキャッチされ、タスクの `IsFaulted` プロパティは `True`に設定され、タスクの `Exception.InnerException` プロパティは例外に設定されます。

`Throw New OperationCancelledException` 行のコメントを解除して、非同期処理を取り消したときに何が起こるかを示します。 例外は `Catch` ブロックでキャッチされ、タスクの `IsCanceled` プロパティは `True`に設定されます。 ただし、この例には適用されない条件がある場合、`IsFaulted` は `True` に設定され `IsCanceled` が `False`に設定されます。

[!code-vb[csAsyncExceptions#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csasyncexceptions/vb/class1.vb#1)]

### <a name="handling-multiple-exceptions-in-async-methods"></a>非同期メソッドでの複数の例外の処理

次の例では、複数のタスクで複数の例外が発生する可能性がある例外処理について説明します。 `Try` ブロックには、返される <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> タスクの `Await` 式が含まれています。 <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> が適用される3つのタスクが完了すると、タスクが完了します。

3 つのタスクでそれぞれ例外が発生します。 `Catch` ブロックは、返される `Task.WhenAll` タスクの `Exception.InnerExceptions` プロパティで検出された例外を反復処理します。

[!code-vb[csAsyncExceptions#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csasyncexceptions/vb/class1.vb#3)]

## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.Information.Err%2A>
- <xref:System.Exception>
- [Exit ステートメント](exit-statement.md)
- [On Error ステートメント](on-error-statement.md)
- [コード スニペットを使用するためのベスト プラクティス](/visualstudio/ide/best-practices-for-using-code-snippets)
- [例外処理](../../../standard/parallel-programming/exception-handling-task-parallel-library.md)
- [Throw ステートメント](throw-statement.md)
