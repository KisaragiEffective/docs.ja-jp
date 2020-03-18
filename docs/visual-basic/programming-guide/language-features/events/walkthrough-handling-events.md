---
title: イベントの処理
ms.date: 07/20/2015
helpviewer_keywords:
- event handling [Visual Basic], walkthroughs
- walkthroughs [Visual Basic], event handling
- variables [Visual Basic], WithEvents
- events [Visual Basic], walkthroughs
- WithEvents keyword [Visual Basic], walkthroughs
- event handlers [Visual Basic], walkthroughs
ms.assetid: f145b3fc-5ae0-4509-a2aa-1ff6934706bd
ms.openlocfilehash: cb42f2e41e366cf8765cb9395d1a5641434bab40
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345084"
---
# <a name="walkthrough-handling-events-visual-basic"></a>チュートリアル: イベントの処理 (Visual Basic)
これは、イベントを操作する方法を示す2つのトピックのうち2番目のトピックです。 最初のトピック「[チュートリアル: イベントの宣言と発生](../../../../visual-basic/programming-guide/language-features/events/walkthrough-declaring-and-raising-events.md)」では、イベントを宣言して発生させる方法を示します。 このセクションでは、そのチュートリアルのフォームとクラスを使用して、発生したイベントの処理方法を示します。  
  
 `Widget` クラスの例では、従来のイベント処理ステートメントを使用します。 Visual Basic には、イベントを操作するためのその他の手法が用意されています。 演習として、この例を変更して、`AddHandler` および `Handles` ステートメントを使用することができます。  
  
### <a name="to-handle-the-percentdone-event-of-the-widget-class"></a>ウィジェットクラスの PercentDone イベントを処理するには  
  
1. `Form1`に次のコードを配置します。  
  
     [!code-vb[VbVbcnWalkthroughDeclaringAndRaisingEvents#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnWalkthroughDeclaringAndRaisingEvents/VB/Form1.vb#4)]  
  
     `WithEvents` キーワードは、オブジェクトのイベントを処理するために `mWidget` 変数が使用されることを指定します。 オブジェクトの種類を指定するには、オブジェクトの作成元となるクラスの名前を指定します。  
  
     変数 `mWidget` は `Form1` で宣言されています。 `WithEvents` 変数はクラスレベルである必要があるためです。 これは、配置したクラスの型に関係なく当てはまります。  
  
     `LongTask` メソッドを取り消すには、`mblnCancel` 変数を使用します。  
  
## <a name="writing-code-to-handle-an-event"></a>イベントを処理するコードの記述  
 `WithEvents`を使用して変数を宣言するとすぐに、クラスの**コードエディター**の左側のドロップダウンリストに変数名が表示されます。 [`mWidget`] を選択すると、`Widget` クラスのイベントが右のドロップダウンリストに表示されます。 イベントを選択すると、対応するイベントプロシージャにプレフィックス `mWidget` とアンダースコアが表示されます。 `WithEvents` 変数に関連付けられているすべてのイベントプロシージャには、プレフィックスとして変数名が割り当てられます。  
  
#### <a name="to-handle-an-event"></a>イベントを処理するには  
  
1. **コードエディター**の左側のドロップダウンリストから [`mWidget`] を選択します。  
  
2. 右側のドロップダウンリストから `PercentDone` イベントを選択します。 **コードエディター**で、`mWidget_PercentDone` イベントプロシージャが開きます。  
  
    > [!NOTE]
    > **コードエディター**は、新しいイベントハンドラーを挿入する場合には便利ですが、必須ではありません。 このチュートリアルでは、コードに直接イベントハンドラーをコピーするだけで済みます。  
  
3. `mWidget_PercentDone` イベント ハンドラーに次のコードを追加します。  
  
     [!code-vb[VbVbcnWalkthroughDeclaringAndRaisingEvents#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnWalkthroughDeclaringAndRaisingEvents/VB/Form1.vb#5)]  
  
     イベントプロシージャは、`PercentDone` イベントが発生するたびに、`Label` コントロールの完了率を表示します。 `DoEvents` メソッドを使用すると、ラベルを再描画できます。また、ユーザーは **[キャンセル]** ボタンをクリックすることもできます。  
  
4. `Button2_Click` イベントハンドラーに次のコードを追加します。  
  
     [!code-vb[VbVbcnWalkthroughDeclaringAndRaisingEvents#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnWalkthroughDeclaringAndRaisingEvents/VB/Form1.vb#6)]  
  
 `LongTask` の実行中にユーザーが **[キャンセル**] ボタンをクリックした場合、`DoEvents` ステートメントによってイベント処理が可能になるとすぐに、`Button2_Click` イベントが実行されます。 クラスレベルの変数 `mblnCancel` が `True`に設定され、`mWidget_PercentDone` イベントによってテストされ、`ByRef Cancel` 引数が `True`に設定されます。  
  
## <a name="connecting-a-withevents-variable-to-an-object"></a>WithEvents 変数をオブジェクトに接続する  
 `Form1` は、`Widget` オブジェクトのイベントを処理するようにセットアップされました。 残っているのは、どこかの `Widget` を見つけることだけです。  
  
 デザイン時に `WithEvents` 変数を宣言しても、オブジェクトにはオブジェクトが関連付けられていません。 `WithEvents` 変数は、他のオブジェクト変数と同じです。 オブジェクトを作成し、そのオブジェクトへの参照を `WithEvents` 変数で割り当てる必要があります。  
  
#### <a name="to-create-an-object-and-assign-a-reference-to-it"></a>オブジェクトを作成し、そのオブジェクトへの参照を割り当てるには  
  
1. **コードエディター**の左側のドロップダウンリストで **[(Form1 イベント)]** を選択します。  
  
2. 右側のドロップダウンリストから `Load` イベントを選択します。 **コードエディター**で、`Form1_Load` イベントプロシージャが開きます。  
  
3. `Widget`を作成するには、`Form1_Load` イベントプロシージャに次のコードを追加します。  
  
     [!code-vb[VbVbcnWalkthroughDeclaringAndRaisingEvents#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnWalkthroughDeclaringAndRaisingEvents/VB/Form1.vb#7)]  
  
 このコードが実行されると、Visual Basic は `Widget` オブジェクトを作成し、そのイベントを `mWidget`に関連付けられているイベントプロシージャに接続します。 その時点から、`Widget` が `PercentDone` イベントを発生させるたびに、`mWidget_PercentDone` イベントプロシージャが実行されます。  
  
#### <a name="to-call-the-longtask-method"></a>LongTask メソッドを呼び出すには  
  
- `Button1_Click` イベント ハンドラーに次のコードを追加します。  
  
     [!code-vb[VbVbcnWalkthroughDeclaringAndRaisingEvents#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnWalkthroughDeclaringAndRaisingEvents/VB/Form1.vb#8)]  
  
 `LongTask` メソッドが呼び出される前に、完了率を示すラベルを初期化する必要があります。また、メソッドをキャンセルするためのクラスレベルの `Boolean` フラグを `False`に設定する必要があります。  
  
 `LongTask` は、タスク期間が12.2 秒の場合に呼び出されます。 `PercentDone` イベントは、1秒間に1回発生します。 イベントが発生するたびに、`mWidget_PercentDone` イベントプロシージャが実行されます。  
  
 `LongTask` が完了すると、`mblnCancel` は `LongTask` が正常に終了したかどうか、または `mblnCancel` が `True`に設定されているために停止したかどうかをテストします。 完了率は、前者の場合にのみ更新されます。  
  
#### <a name="to-run-the-program"></a>プログラムを実行するには  
  
1. F5 キーを押して、プロジェクトを実行モードにします。  
  
2. **[タスクの開始]** ボタンをクリックします。 `PercentDone` イベントが発生するたびに、完了したタスクの割合でラベルが更新されます。  
  
3. **[キャンセル]** ボタンをクリックして、タスクを停止します。 **[キャンセル]** ボタンの外観は、クリックしてもすぐには変更されないことに注意してください。 `My.Application.DoEvents` ステートメントによってイベント処理が許可されるまで、`Click` イベントは発生しません。  
  
    > [!NOTE]
    > `My.Application.DoEvents` メソッドは、フォームとまったく同じ方法でイベントを処理しません。 たとえば、このチュートリアルでは、 **[キャンセル]** ボタンを2回クリックする必要があります。 フォームがイベントを直接処理できるようにするには、マルチスレッドを使用します。 詳細については、「[マネージスレッド処理](../../../../standard/threading/index.md)」を参照してください。
  
 F11 キーを使用してプログラムを実行し、一度に1行ずつコードをステップ実行することをお勧めします。 `LongTask`実行がどのようになるかを明確に確認し、`PercentDone` イベントが発生するたびに `Form1` を再入力することができます。  
  
 `Form1`のコードで実行が戻ったときに、`LongTask` メソッドが再度呼び出された場合はどうなるでしょうか。 最悪の場合、イベントが発生するたびに `LongTask` が呼び出されると、スタックオーバーフローが発生する可能性があります。  
  
 新しい `Widget` への参照を `mWidget`に割り当てることによって、変数 `mWidget` が別の `Widget` オブジェクトのイベントを処理するようにすることができます。 実際には、ボタンをクリックするたびに、`Button1_Click` コードを作成できます。  
  
#### <a name="to-handle-events-for-a-different-widget"></a>別のウィジェットのイベントを処理するには  
  
- `mWidget.LongTask(12.2, 0.33)`を読み取る行の直前に、次のコード行を `Button1_Click` プロシージャに追加します。  
  
     [!code-vb[VbVbcnWalkthroughDeclaringAndRaisingEvents#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnWalkthroughDeclaringAndRaisingEvents/VB/Form1.vb#9)]  
  
 上記のコードでは、ボタンがクリックされるたびに新しい `Widget` が作成されます。 `LongTask` メソッドが完了するとすぐに、`Widget` への参照が解放され、`Widget` が破棄されます。  
  
 `WithEvents` 変数には一度に1つのオブジェクト参照のみを含めることができます。したがって、別の `Widget` オブジェクトを `mWidget`に割り当てると、前の `Widget` オブジェクトのイベントは処理されなくなります。 `mWidget` が古い `Widget`への参照を含む唯一のオブジェクト変数である場合、オブジェクトは破棄されます。 複数の `Widget` オブジェクトのイベントを処理する場合は、`AddHandler` ステートメントを使用して各オブジェクトのイベントを個別に処理します。  
  
> [!NOTE]
> `WithEvents` 変数は必要な数だけ宣言できますが、`WithEvents` 変数の配列はサポートされていません。  
  
## <a name="see-also"></a>参照

- [チュートリアル : イベントの宣言と発生](../../../../visual-basic/programming-guide/language-features/events/walkthrough-declaring-and-raising-events.md)
- [イベント](../../../../visual-basic/programming-guide/language-features/events/index.md)
