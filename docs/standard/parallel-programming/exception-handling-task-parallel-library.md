---
title: 例外処理 (タスク並列ライブラリ)
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, exceptions
ms.assetid: beb51e50-9061-4d3d-908c-56a4f7c2e8c1
ms.openlocfilehash: 12777a5f34b8aadcc80977b8796fc2cd53c626a8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73134251"
---
# <a name="exception-handling-task-parallel-library"></a>例外処理 (タスク並列ライブラリ)

タスク内で実行中のユーザー コードによってスローされた、ハンドルされない例外は、呼び出し元のスレッドに反映されます。ただし、このトピックの後半で説明している特定の状況を除きます。 静的な、またはインスタンスの <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> メソッドの 1 つを使用し、その呼び出しを `try`/`catch` ステートメント内に入れて例外を処理すると、例外が反映されます。 タスクが、アタッチされた子タスクの親である場合、または複数のタスクを待機している場合、複数の例外がスローされることがあります。

呼び出し元のスレッドにすべての例外を反映するために、Task インフラストラクチャが例外を <xref:System.AggregateException> インスタンスにラップします。 <xref:System.AggregateException> 例外には、スローされた元のすべての例外を調べるために列挙できる <xref:System.AggregateException.InnerExceptions%2A> プロパティがあり、個々に処理したり未処理にしたりできます。 また、 <xref:System.AggregateException.Handle%2A?displayProperty=nameWithType> メソッドを使用して元の例外を処理することもできます。

例外が 1 つだけスローされた場合でも、次の例のように <xref:System.AggregateException> 例外にラップされます。

[!code-csharp[TPL_Exceptions#21](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/handling21.cs#21)]
[!code-vb[TPL_Exceptions#21](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/handling21.vb#21)]

ハンドルされない例外は、 <xref:System.AggregateException> をキャッチして、内部例外を確認しないことで回避できます。 ただし、並列でない状況で基本的な <xref:System.Exception> の種類をキャッチする場合と類似しているため、この操作はお勧めしません。 特定の操作を取得することなく例外をキャッチして回復しようとすると、プログラムが中間状態のままになるおそれがあります。

<xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> メソッドを呼び出してタスクの完了を待機する処理を行わない場合は、次の例のようにタスクの <xref:System.AggregateException> プロパティから <xref:System.Threading.Tasks.Task.Exception%2A> 例外を取得することもできます。 詳細については、このトピックの「[Task.Exception プロパティによる例外の確認](#observing-exceptions-by-using-the-taskexception-property)」セクションを参照してください。

[!code-csharp[TPL_Exceptions#29](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/handling22.cs#29)]
[!code-vb[TPL_Exceptions#29](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/handling22.vb#29)]

例外を反映するタスクを待機しない場合、またはタスクの <xref:System.Threading.Tasks.Task.Exception%2A> プロパティにアクセスする場合、例外はタスクがガベージ コレクトされるときに .NET の例外ポリシーに従ってエスカレートされます。

連結しているスレッドへ例外が上方向に通知されると、例外が発生した後も、タスクによって一部の項目の処理が続行される可能性があります。

> [!NOTE]
> [マイ コードのみ] が有効になっている場合、Visual Studio では、例外をスローする行で処理が中断され、"ユーザー コードで処理されない例外" に関するエラー メッセージが表示されることがあります。 このエラーは問題にはなりません。 F5 キーを押して続行し、以下の例に示す例外処理動作を確認できます。 Visual Studio による処理が最初のエラーで中断しないようにするには、 **[ツール]** メニューの [オプション]、[デバッグ] の順にクリックし、[全般] で **[マイ コードのみを有効にする]** チェック ボックスをオフにします。

## <a name="attached-child-tasks-and-nested-aggregateexceptions"></a>アタッチされた子タスクと入れ子の AggregateExceptions

タスクに、例外をスローする子タスクがアタッチされている場合、その例外は親タスクに反映される前に <xref:System.AggregateException> でラップされます。つまり、呼び出し元のスレッドに反映される前に、その例外が固有の <xref:System.AggregateException> でラップされるということです。 このような場合、<xref:System.AggregateException.InnerExceptions%2A>、<xref:System.AggregateException>、<xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> の各メソッドでキャッチされた <xref:System.Threading.Tasks.Task.WaitAny%2A> 例外の <xref:System.Threading.Tasks.Task.WaitAll%2A> プロパティには、違反の原因となった元の例外ではなく、1 つ以上の <xref:System.AggregateException> インスタンスが含まれます。 入れ子の <xref:System.AggregateException> 例外を反復処理しなくて済むようにするには、 <xref:System.AggregateException.Flatten%2A> メソッドを使用して入れ子の <xref:System.AggregateException> をすべて削除します。これにより、 <xref:System.AggregateException.InnerExceptions%2A?displayProperty=nameWithType> プロパティに元の例外が含まれるようになります。 次の例では、入れ子の <xref:System.AggregateException> インスタンスが 1 つのループで平坦化され、処理されています。

[!code-csharp[TPL_Exceptions#22](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/flatten2.cs#22)]
[!code-vb[TPL_Exceptions#22](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/flatten2.vb#22)]

また、次の例のように、 <xref:System.AggregateException.Flatten%2A?displayProperty=nameWithType> メソッドを使用して、単一の <xref:System.AggregateException> インスタンス内の複数のタスクによってスローされた複数の <xref:System.AggregateException> インスタンスから内部例外を再スローすることもできます。

[!code-csharp[TPL_Exceptions#13](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/taskexceptions2.cs#13)]
[!code-vb[TPL_Exceptions#13](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/taskexceptions2.vb#13)]

## <a name="exceptions-from-detached-child-tasks"></a>デタッチされた子タスクの例外

既定では、子タスクはデタッチされた状態で作成されます。 デタッチされたタスクからスローされた例外は、処理されるか、直接の親に再スローされる必要があります。これらの例外は、アタッチされた子タスクが反映されるのとは異なる方法で、呼び出し元のスレッドに反映されます。 最上位の親では、デタッチされた子からの例外を手動で再スローすることで、 <xref:System.AggregateException> にラップして、呼び出し元のスレッドに反映させることができます。

[!code-csharp[TPL_Exceptions#23](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/detached21.cs#23)]
[!code-vb[TPL_Exceptions#23](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/detached21.vb#23)]

子タスク内の例外を確認するために継続を使用する場合でも、親タスクによって例外を確認する必要があります。

## <a name="exceptions-that-indicate-cooperative-cancellation"></a>他の処理との連携によるキャンセル処理を示す例外

タスク内のユーザー コードがキャンセル要求に応答する場合の正しいプロシージャは、その要求が伝えられたキャセル トークンに渡すために <xref:System.OperationCanceledException> をスローすることです。 例外が反映される前に、タスク インスタンスによって、例外内のトークンがそのタスクが作成されたときに渡されたトークンと比較されます。 それらが同じである場合、タスクは <xref:System.Threading.Tasks.TaskCanceledException> でラップされた <xref:System.AggregateException>を反映します。これは、内部例外を調べると確認できます。 ただし、呼び出し元のスレッドがタスクを待機していない場合、このような特定の例外は反映されません。 詳細については、「[タスクのキャンセル](../../../docs/standard/parallel-programming/task-cancellation.md)」をご覧ください。

[!code-csharp[TPL_Exceptions#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/exceptions.cs#4)]
[!code-vb[TPL_Exceptions#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/tpl_exceptions.vb#4)]

## <a name="using-the-handle-method-to-filter-inner-exceptions"></a>内部例外をフィルター処理する Handle メソッドの使用

<xref:System.AggregateException.Handle%2A?displayProperty=nameWithType> メソッドを使用すると、"処理済み" として扱うことのできる例外をフィルターで除外できます。追加のロジックを使用する必要はありません。 <xref:System.AggregateException.Handle%28System.Func%7BSystem.Exception%2CSystem.Boolean%7D%29?displayProperty=nameWithType> メソッドに用意されているユーザー デリゲートでは、例外の種類、その <xref:System.Exception.Message%2A> プロパティ、またはその例外に問題がないかどうかを判別できるその他の情報を調べることができます。 デリゲートが `false` を返す例外は、<xref:System.AggregateException> メソッドが返された直後に新しい <xref:System.AggregateException.Handle%2A?displayProperty=nameWithType> インスタンスで再スローされます。

次の例は、機能的には、<xref:System.AggregateException.InnerExceptions%2A?displayProperty=nameWithType> コレクション内の各例外を調べるこのトピックの最初の例と同じです。  代わりに、この例外ハンドラーでは、例外ごとに <xref:System.AggregateException.Handle%2A?displayProperty=nameWithType> メソッドのオブジェクトを呼び出し、`CustomException` インスタンスでない例外のみを再スローしています。

[!code-csharp[TPL_Exceptions#26](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/handlemethod21.cs#26)]
[!code-vb[TPL_Exceptions#26](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/handlemethod21.vb#26)]

次の例はさらに複雑です。 <xref:System.AggregateException.Handle%2A?displayProperty=nameWithType> メソッドを使用して、ファイルの列挙時に <xref:System.UnauthorizedAccessException> 例外の特別な処理を行っています。

[!code-csharp[TPL_Exceptions#12](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/taskexceptions.cs#12)]
[!code-vb[TPL_Exceptions#12](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/taskexceptions.vb#12)]

## <a name="observing-exceptions-by-using-the-taskexception-property"></a>Task.Exception プロパティによる例外の確認

タスクが <xref:System.Threading.Tasks.TaskStatus.Faulted?displayProperty=nameWithType> 状態で完了した場合、その <xref:System.Threading.Tasks.Task.Exception%2A> プロパティを調べることで違反の原因となった例外を見つけることができます。 <xref:System.Threading.Tasks.Task.Exception%2A> プロパティを確認する場合は、次の例に示すように、継続元タスクが違反した場合にのみ実行される継続を使用することをお勧めします。

[!code-csharp[TPL_Exceptions#27](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/exceptionprop21.cs#27)]
[!code-vb[TPL_Exceptions#27](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/exceptionprop21.vb#27)]

実際のアプリケーションでは、継続のデリゲートで例外に関する詳細情報を記録して、新しいタスクを作成して例外から回復することが考えられます。

## <a name="unobservedtaskexception-event"></a>UnobservedTaskException イベント

信頼関係のないプラグインをホストするときなど、シナリオによっては、問題のない例外がよく発生する場合や、難しすぎてすべてを手動で観察できなくなる場合があります。 このような場合、 <xref:System.Threading.Tasks.TaskScheduler.UnobservedTaskException?displayProperty=nameWithType> イベントを処理できます。 ハンドラーに渡される <xref:System.Threading.Tasks.UnobservedTaskExceptionEventArgs?displayProperty=nameWithType> インスタンスを使用して、観察されない例外が連結しているスレッドに反映されないようにすることができます。

## <a name="see-also"></a>参照

- [タスク並列ライブラリ (TPL)](../../../docs/standard/parallel-programming/task-parallel-library-tpl.md)
