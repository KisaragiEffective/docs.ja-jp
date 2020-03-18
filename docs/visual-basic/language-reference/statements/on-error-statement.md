---
title: On Error ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.OnError
helpviewer_keywords:
- Visual Basic code, control flow
- Resume Next statement [Visual Basic]
- errors [Visual Basic], trapping
- error handling, On Error statement
- Next statement [Visual Basic], On Error
- control flow [Visual Basic], branching
- Error keyword [Visual Basic]
- execution [Visual Basic], conditional
- Resume statement [Visual Basic], and On Error statement
- Error statement [Visual Basic], and On Error statement
- GoTo statement [Visual Basic], and On Error statement
- branching [Visual Basic], on error
- conditional statements [Visual Basic], On Error
- On Error statement [Visual Basic], syntax
- On keyword [Visual Basic]
- run-time errors [Visual Basic], handling
- On Error statement [Visual Basic]
ms.assetid: ff947930-fb84-40cf-bd66-1ea219561d5c
ms.openlocfilehash: d62c2ba1849b7015ed877d503220026a2dfeff57
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353820"
---
# <a name="on-error-statement-visual-basic"></a>On Error ステートメント (Visual Basic)
エラー処理ルーチンを有効にし、プロシージャ内でルーチンの場所を指定します。は、エラー処理ルーチンを無効にするためにも使用できます。 `On Error` ステートメントは非構造化エラー処理で使用されるため、構造化例外処理の代わりに使用できます。 [構造化例外処理](../../../standard/exceptions/index.md)は .net に組み込まれているので、一般に効率が向上するため、アプリケーションで実行時エラーを処理する場合に推奨されます。

 エラー処理または例外処理を使用しない場合、発生したすべての実行時エラーは致命的なエラーであり、エラーメッセージが表示され、実行が停止します。

> [!NOTE]
> `Error` キーワードは、旧バージョンとの互換性のためにサポートされている[Error ステートメント](../../../visual-basic/language-reference/statements/error-statement.md)でも使用されます。

## <a name="syntax"></a>構文

```vb
On Error { GoTo [ line | 0 | -1 ] | Resume Next }
```

## <a name="parts"></a>指定項目

|用語|Definition|
|---|---|
|`GoTo`*線*|必須の*line*引数で指定された行から開始するエラー処理ルーチンを有効にします。 *Line*引数は、任意の行ラベルまたは行番号です。 実行時エラーが発生した場合、コントロールは指定された行に分岐し、エラーハンドラーがアクティブになります。 指定された行は、`On Error` ステートメントと同じプロシージャ内に存在する必要があります。指定しないと、コンパイル時エラーが発生します。|
|`GoTo 0`|現在のプロシージャで有効になっているエラーハンドラーを無効にし、`Nothing`にリセットします。|
|`GoTo -1`|現在のプロシージャで有効になっている例外を無効にして、`Nothing`にリセットします。|
|`Resume Next`|実行時エラーが発生したときに、エラーが発生したステートメントの直後のステートメントに制御を移動し、その時点から実行を続行することを指定します。 オブジェクトにアクセスする場合は、`On Error GoTo` ではなく、この形式を使用します。|

## <a name="remarks"></a>コメント

> [!NOTE]
> 構造化例外処理は、構造化されていない例外処理と `On Error` ステートメントを使用するのではなく、可能な限りコードで使用することをお勧めします。 詳しくは、「[Try...Catch...Finally ステートメント](../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)」をご覧ください。

 "Enabled" エラーハンドラーは、`On Error` ステートメントによって有効にされているものです。 "アクティブ" エラーハンドラーは、エラーを処理するための有効なハンドラーです。

 エラーハンドラーがアクティブになっている間にエラーが発生した場合 (エラーが発生した場合と、`Resume`、`Exit Sub`、`Exit Function`、または `Exit Property` ステートメントの場合)、現在のプロシージャのエラーハンドラーではエラーを処理できません。 呼び出し元のプロシージャに制御が戻ります。
  
 呼び出し元のプロシージャに有効なエラーハンドラーがある場合は、エラーを処理するためにアクティブ化されます。 呼び出し元のプロシージャのエラーハンドラーもアクティブになっている場合は、有効になっているが非アクティブなエラーハンドラーが見つかるまで、制御は前の呼び出しプロシージャを通過します。 このようなエラーハンドラーが見つからない場合、エラーは実際に発生した時点で致命的です。
  
 エラーハンドラーが呼び出し元のプロシージャに制御を戻すたびに、そのプロシージャが現在のプロシージャになります。 任意のプロシージャのエラーハンドラーによってエラーが処理されると、`Resume` ステートメントによって指定された時点で、現在のプロシージャで実行が再開されます。
  
> [!NOTE]
> エラー処理ルーチンは、`Sub` プロシージャでも `Function` プロシージャでもありません。 これは、行ラベルまたは行番号でマークされたコードのセクションです。
  
## <a name="number-property"></a>Number プロパティ
 エラー処理ルーチンは、エラーの原因を特定するために、`Err` オブジェクトの `Number` プロパティの値に依存します。 ルーチンは、他のエラーが発生する前、またはエラーが発生する可能性のあるプロシージャが呼び出される前に、`Err` オブジェクト内の関連するプロパティ値をテストまたは保存する必要があります。 `Err` オブジェクトのプロパティ値は、最新のエラーのみを反映します。 `Err.Number` に関連付けられたエラーメッセージは `Err.Description`に含まれています。  
  
## <a name="throw-statement"></a>Throw ステートメント  
 `Err.Raise` メソッドで発生したエラーは、`Exception` プロパティを <xref:System.Exception> クラスの新しく作成されたインスタンスに設定します。 派生した例外の種類の例外の発生をサポートするために、言語で `Throw` ステートメントがサポートされています。 これは、スローされる例外インスタンスである1つのパラメーターを受け取ります。 次の例は、これらの機能を既存の例外処理サポートと共に使用する方法を示しています。

 [!code-vb[VbVbalrErrorHandling#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrErrorHandling/VB/Class1.vb#17)]  
  
 `On Error GoTo` ステートメントでは、exception クラスに関係なく、すべてのエラーがトラップされることに注意してください。
  
## <a name="on-error-resume-next"></a>エラー時に再開する
 `On Error Resume Next` により、実行は、実行時エラーの原因となったステートメントの直後のステートメント、または、`On Error Resume Next` ステートメントを含むプロシージャの最後の呼び出しの直後にあるステートメントで続行されます。 このステートメントを使用すると、実行時エラーが発生しても実行を続行できます。 プロシージャ内の別の場所に制御を転送するのではなく、エラーが発生するエラー処理ルーチンを配置できます。 別のプロシージャが呼び出されると、`On Error Resume Next` ステートメントは非アクティブになります。そのため、ルーチン内でインラインエラー処理が必要な場合は、呼び出された各ルーチンで `On Error Resume Next` ステートメントを実行する必要があります。
  
> [!NOTE]
> `On Error Resume Next` コンストラクトは、他のオブジェクトへのアクセス中に発生したエラーを処理する場合に `On Error GoTo` することをお勧めします。 オブジェクトを操作するたびに `Err` をチェックすると、コードによってアクセスされたオブジェクトがあいまいになります。 エラーコードを `Err.Number`に配置したオブジェクトと、最初にエラーを生成したオブジェクト (`Err.Source`で指定されたオブジェクト) を確認できます。

## <a name="on-error-goto-0"></a>On Error GoTo 0
 `On Error GoTo 0` は、現在のプロシージャのエラー処理を無効にします。 プロシージャに行番号0が含まれている場合でも、エラー処理コードの先頭として行0が指定されていません。 `On Error GoTo 0` ステートメントを使用しない場合、プロシージャが終了すると、エラーハンドラーが自動的に無効になります。

## <a name="on-error-goto--1"></a>エラー時の GoTo-1
 `On Error GoTo -1` は、現在のプロシージャの例外を無効にします。 プロシージャに1行の番号が含まれている場合でも、エラー処理コードの開始として行-1 を指定しません。 `On Error GoTo -1` ステートメントを使用しない場合、プロシージャが終了すると、例外が自動的に無効になります。

 エラーが発生しなかったときにエラー処理コードが実行されないようにするには、次のように、エラー処理ルーチンの直前に、`Exit Sub`、`Exit Function`、または `Exit Property` ステートメントを配置します。

 [!code-vb[VbVbalrErrorHandling#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrErrorHandling/VB/Class1.vb#18)]

 ここでは、エラー処理コードは、`Exit Sub` ステートメントに従い、`End Sub` ステートメントの前に、プロシージャフローから分離します。 プロシージャ内の任意の場所にエラー処理コードを配置できます。

## <a name="untrapped-errors"></a>トラップエラー
 オブジェクトのトラップされていないエラーは、オブジェクトが実行可能ファイルとして実行されている場合に、制御を行うアプリケーションに返されます。 開発環境では、適切なオプションが設定されている場合にのみ、トラップされないエラーが制御アプリケーションに返されます。 デバッグ中に設定するオプション、設定方法、およびホストでクラスを作成できるかどうかの説明については、ホストアプリケーションのドキュメントを参照してください。

 他のオブジェクトにアクセスするオブジェクトを作成する場合は、返されたハンドルされていないエラーを処理する必要があります。 できない場合は、`Err.Number` のエラーコードを独自のエラーのいずれかにマップし、オブジェクトの呼び出し元に渡します。 エラーコードを `VbObjectError` 定数に追加して、エラーを指定する必要があります。 たとえば、エラーコードが1052の場合は、次のように割り当てます。

 [!code-vb[VbVbalrErrorHandling#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrErrorHandling/VB/Class1.vb#19)]

> [!CAUTION]
> Windows ダイナミックリンクライブラリ (Dll) の呼び出し中にシステムエラーが発生しても、例外は発生せず、Visual Basic エラートラップでトラップすることはできません。 DLL 関数を呼び出すときは、API の仕様に従って、成功または失敗の各戻り値を確認し、エラーが発生した場合は `Err` オブジェクトの `LastDLLError` プロパティの値を確認する必要があります。

## <a name="example"></a>例
 この例では、最初に `On Error GoTo` ステートメントを使用して、プロシージャ内のエラー処理ルーチンの場所を指定します。 この例では、0で除算しようとするとエラー番号6が生成されます。 エラー処理ルーチンでエラーが処理され、エラーの原因となったステートメントに制御が返されます。 `On Error GoTo 0` ステートメントは、エラートラップをオフにします。 次に、`On Error Resume Next` ステートメントを使用して、次のステートメントによって生成されるエラーのコンテキストが特定ので認識されるように、エラートラップを遅延します。 `Err.Clear` は、エラーが処理された後に `Err` オブジェクトのプロパティをクリアするために使用されることに注意してください。

 [!code-vb[VbVbalrErrorHandling#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrErrorHandling/VB/Class1.vb#20)]

## <a name="requirements"></a>要件
 **名前空間:** [Microsoft. visual basic](../../../visual-basic/language-reference/runtime-library-members.md)

 **アセンブリ:** Visual Basic ランタイムライブラリ (Microsoft... .dll)

## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Information.Err%2A>
- <xref:Microsoft.VisualBasic.ErrObject.Number%2A>
- <xref:Microsoft.VisualBasic.ErrObject.Description%2A>
- <xref:Microsoft.VisualBasic.ErrObject.LastDllError%2A>
- [End ステートメント](../../../visual-basic/language-reference/statements/end-statement.md)
- [Exit ステートメント](../../../visual-basic/language-reference/statements/exit-statement.md)
- [Resume ステートメント](../../../visual-basic/language-reference/statements/resume-statement.md)
- [エラー メッセージ](../../../visual-basic/language-reference/error-messages/index.md)
- [Try...Catch...Finally ステートメント](../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)
