---
title: 継続タスクを使用したタスクの連結
ms.date: 02/11/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, continuations
ms.assetid: 0b45e9a2-de28-46ce-8212-1817280ed42d
ms.openlocfilehash: 7de8c4e44e1866e3df36c666c9ecc210dc6a7d83
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78159365"
---
# <a name="chaining-tasks-by-using-continuation-tasks"></a>継続タスクを使用したタスクの連結
非同期プログラミングでは、非同期操作で完了時に 2 番目の操作を呼び出してデータを渡すのが一般的です。 これまで、継続はコールバック メソッドを使用して行っていました。 タスク並列ライブラリでは、 *継続タスク*に同じ機能が用意されています。 継続タスク (単に "継続" とも呼ばれます) とは、別のタスク (" *継続元*" と呼ばれます) が終了したときにそのタスクによって呼び出される非同期タスクのことです。  
  
 継続は比較的簡単に使用できますが、強力な機能と柔軟性を備えています。 たとえば、次のように操作できます。  
  
- 継続元のデータを継続に渡します。  
  
- 継続を呼び出す場合、呼び出さない場合についての正確な条件を指定します。  
  
- 継続が開始される前、または継続の実行中に継続を取り消します。  
  
- 継続をスケジュールする方法についてのヒントを提供します。  
  
- 同じ継続元から複数の継続を呼び出します。  
  
- 複数の継続元のすべてまたはいずれか 1 つが完了したときに 1 つの継続を呼び出します。  
  
- 任意の長さで連続して継続を実行します。  
  
- 継続元によってスローされた例外を処理するために継続を使用します。  
  
## <a name="about-continuations"></a>継続について  
 継続とは、 <xref:System.Threading.Tasks.TaskStatus.WaitingForActivation> 状態で作成されるタスクです。 継続は、その継続元タスクが完了すると自動的に開始されます。 ユーザー コード内の継続で <xref:System.Threading.Tasks.Task.Start%2A?displayProperty=nameWithType> を呼び出すと、 <xref:System.InvalidOperationException?displayProperty=nameWithType> 例外がスローされます。  
  
 継続はそれ自体が <xref:System.Threading.Tasks.Task> であり、継続が開始されたスレッドをブロックするわけではありません。 継続タスクが終了するまでブロックするには、 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> メソッドを呼び出します。  
  
## <a name="creating-a-continuation-for-a-single-antecedent"></a>1 つの継続元に対する継続の作成  
 <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> メソッドを呼び出して、継続元が完了した時点で実行される継続を作成します。 次の例は、基本的なパターンを示しています (わかりやすくするために、例外処理は省略されています)。 現在の曜日の名前を示す `taskA`オブジェクトを返す継続元タスク <xref:System.DayOfWeek> が実行されます。 継続元が完了すると、継続タスク `continuation` に継続元が渡され、その結果を示す文字列が表示されます。

> [!NOTE]
> この記事の C# サンプルでは、`async` メソッド上で `Main` 修飾子が利用されています。 この機能は C# 7.1 以降で使用できます。 以前のバージョンでは、このサンプル コードをコンパイルすると、[`CS5001`](../../csharp/misc/cs5001.md) が生成されます。 言語バージョンを C# 7.1 以降に設定する必要があります。 言語バージョンの構成方法については、[言語バージョンの構成](../../csharp/language-reference/configure-language-version.md)に関する記事を参照してください。
  
 [!code-csharp[TPL_Continuations#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_continuations/cs/simple1.cs#1)]
 [!code-vb[TPL_Continuations#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/simple1.vb#1)]  
  
## <a name="creating-a-continuation-for-multiple-antecedents"></a>複数の継続元に対する継続の作成  
 タスク グループの一部または全部のタスクが完了したときに実行される継続を作成することもできます。 すべての継続元タスクが完了した時点で継続を実行するには、静的 (Visual Basic では`Shared` ) <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> メソッドまたはインスタンス <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A?displayProperty=nameWithType> メソッドを呼び出します。 いずれかの継続元タスクが完了した時点で継続を実行するには、静的 (Visual Basic では`Shared` ) <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> メソッドまたはインスタンス <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAny%2A?displayProperty=nameWithType> メソッドを呼び出します。  
  
 <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> および <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> オーバーロードの呼び出しによって、呼び出しスレッドがブロックされることはない点に注意してください。  ただし、通常、返された <xref:System.Threading.Tasks.Task.WhenAll%28System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7D%29?displayProperty=nameWithType> プロパティを取得するために <xref:System.Threading.Tasks.Task.WhenAll%28System.Threading.Tasks.Task%5B%5D%29?displayProperty=nameWithType> メソッドと <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> メソッドを除いたすべてを呼び出しますが、そうした場合は呼び出しスレッドがブロックされます。  
  
 次の例では、<xref:System.Threading.Tasks.Task.WhenAll%28System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7D%29?displayProperty=nameWithType> メソッドを呼び出して、10 個の継続元タスクの結果を反映する継続タスクを作成します。 各継続元タスクでは、1 から 10 までの範囲内のインデックス値が二乗されます。 継続元が正常に完了すると (その <xref:System.Threading.Tasks.Task.Status%2A?displayProperty=nameWithType> プロパティが <xref:System.Threading.Tasks.TaskStatus.RanToCompletion?displayProperty=nameWithType>)、継続の <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> プロパティは、各継続元から返される <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 値の配列になります。 この例では、1 から 10 までのすべての数値の二乗の合計を計算するため、これらを追加します。  
  
 [!code-csharp[TPL_Continuations#5](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_continuations/cs/whenall1.cs#5)]
 [!code-vb[TPL_Continuations#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/whenall1.vb#5)]  
  
## <a name="continuation-options"></a>継続のオプション  
 単一タスクの継続を作成する場合、 <xref:System.Threading.Tasks.Task.ContinueWith%2A> 列挙体の値を取得する <xref:System.Threading.Tasks.TaskContinuationOptions?displayProperty=nameWithType> オーバーロードを使用して、継続の開始条件を指定できます。 たとえば、継続元が正常に完了した場合のみ、または違反状態で完了した場合にのみ継続を実行するように指定できます。 継続元で継続を呼び出す準備ができているときに条件が true でない場合、継続は直接 <xref:System.Threading.Tasks.TaskStatus.Canceled?displayProperty=nameWithType> 状態に遷移し、それ以降は継続タスクを開始できません。  
  
 複数タスク継続メソッド ( <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A?displayProperty=nameWithType> メソッドのオーバーロードなど) の多くには、 <xref:System.Threading.Tasks.TaskContinuationOptions?displayProperty=nameWithType> パラメーターも含まれています。 ただし、すべての <xref:System.Threading.Tasks.TaskContinuationOptions?displayProperty=nameWithType> 列挙体メンバーのサブセットだけが有効です。 <xref:System.Threading.Tasks.TaskContinuationOptions?displayProperty=nameWithType> 列挙体に対応する値がある <xref:System.Threading.Tasks.TaskCreationOptions?displayProperty=nameWithType> 値を指定できます ( <xref:System.Threading.Tasks.TaskContinuationOptions.AttachedToParent?displayProperty=nameWithType>、 <xref:System.Threading.Tasks.TaskContinuationOptions.LongRunning?displayProperty=nameWithType>、 <xref:System.Threading.Tasks.TaskContinuationOptions.PreferFairness?displayProperty=nameWithType>など)。 複数タスクの継続で `NotOn` または `OnlyOn` オプションのいずれかを指定すると、実行時に <xref:System.ArgumentOutOfRangeException> 例外がスローされます。  
  
 タスクの継続のオプションの詳細については、「 <xref:System.Threading.Tasks.TaskContinuationOptions> 」を参照してください。  
  
## <a name="passing-data-to-a-continuation"></a>継続へのデータの受け渡し  
 <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> メソッドは、継続元への参照を、継続のユーザー デリゲートに引数として渡します。 継続元が <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> オブジェクトで、そのタスクが完了まで実行された場合、継続はそのタスクの <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> プロパティにアクセスできます。  
  
 <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> プロパティは、タスクが完了するまでブロックします。 ただし、タスクがキャンセルまたは違反になった場合は、 <xref:System.Threading.Tasks.Task%601.Result%2A> プロパティにアクセスしようとすると <xref:System.AggregateException> 例外がスローされます。 この問題を回避するには、次の例で示すように <xref:System.Threading.Tasks.TaskContinuationOptions.OnlyOnRanToCompletion> オプションを使用します。  
  
 [!code-csharp[TPL_Continuations#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_continuations/cs/result1.cs#2)]
 [!code-vb[TPL_Continuations#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/result1.vb#2)]  
  
 継続元が正常に完了するまで実行されなかった場合でも継続を実行する場合は、例外を防ぐ必要があります。 1 つの方法として、継続元の <xref:System.Threading.Tasks.Task.Status%2A?displayProperty=nameWithType> プロパティをテストし、状態が <xref:System.Threading.Tasks.Task%601.Result%2A> でも <xref:System.Threading.Tasks.TaskStatus.Faulted> でもない場合にのみ <xref:System.Threading.Tasks.TaskStatus.Canceled>プロパティへのアクセスを試みるようにします。 また、継続元の <xref:System.Threading.Tasks.Task.Exception%2A> プロパティを調べることもできます。 詳細については、「[例外処理](../../../docs/standard/parallel-programming/exception-handling-task-parallel-library.md)」を参照してください。 次の例では、前の例を変更して、状態が <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> の場合にだけ継続元の <xref:System.Threading.Tasks.TaskStatus.RanToCompletion?displayProperty=nameWithType>プロパティにアクセスするようにします。  
  
 [!code-csharp[TPL_Continuations#7](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_continuations/cs/result2.cs#7)]
 [!code-vb[TPL_Continuations#7](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/result2.vb#7)]  
  
## <a name="canceling-a-continuation"></a>継続のキャンセル  
 継続の <xref:System.Threading.Tasks.Task.Status%2A?displayProperty=nameWithType> プロパティが <xref:System.Threading.Tasks.TaskStatus.Canceled?displayProperty=nameWithType> 状態になるのは、次のような場合です。  
  
- キャンセル要求への応答として <xref:System.OperationCanceledException> 例外をスローした場合。 他のタスクと同様に、例外には継続に渡されたのと同じトークンが含まれ、連携によるキャンセルの受信確認として扱われます。  
  
- <xref:System.Threading.CancellationToken?displayProperty=nameWithType> プロパティが <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> である `true`が継続に渡された場合。 この場合、継続は開始されずに、 <xref:System.Threading.Tasks.TaskStatus.Canceled?displayProperty=nameWithType> 状態に遷移します。  
  
- 継続の <xref:System.Threading.Tasks.TaskContinuationOptions> 引数により設定された条件が満たされないために、継続が実行されない場合。 たとえば、継続元が <xref:System.Threading.Tasks.TaskStatus.Faulted?displayProperty=nameWithType> 状態になった場合、 <xref:System.Threading.Tasks.TaskContinuationOptions.NotOnFaulted?displayProperty=nameWithType> オプションが渡された継続は <xref:System.Threading.Tasks.TaskStatus.Canceled> 状態に遷移し、実行されません。  
  
 タスクとその継続が同じ論理的操作の 2 つの部分を表す場合は、次の例に示すように、同じキャンセル トークンを両方のタスクに渡すことができます。 これは、33 で割り切れる整数のリストを生成する継続元からなります。このリストは継続に渡されます。 次に継続により、このリストが表示されます。 継続元と継続の両方は、ランダムな間隔で定期的に一時停止します。 さらに、 <xref:System.Threading.Timer?displayProperty=nameWithType> オブジェクトを使用して、5 秒のタイムアウト間隔の経過後に `Elapsed` メソッドが実行されます。 この例では、<xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=nameWithType> メソッドが呼び出され、これにより現在実行中のタスクが <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A?displayProperty=nameWithType> メソッドを呼び出します。 継続元またはその継続の実行時に <xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=nameWithType> メソッドが呼び出されるかどうかは、ランダムに生成される一時停止の期間に基づきます。 継続元が取り消された場合、継続は開始されません。 継続元が取り消されない場合は、継続を取り消すためにそのトークンを引き続き使用できます。  
  
 [!code-csharp[TPL_Continuations#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_continuations/cs/cancellation1.cs#3)]
 [!code-vb[TPL_Continuations#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/cancellation1.vb#3)]  
  
 また、継続の作成時に <xref:System.Threading.Tasks.TaskContinuationOptions.NotOnCanceled?displayProperty=nameWithType> オプションを指定することで、キャンセル トークンを継続に指定せずに、継続元が取り消された場合に継続を実行しないようにできます。 単純な例を次に示します。  
  
 [!code-csharp[TPL_Continuations#8](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_continuations/cs/cancellation2.cs#8)]
 [!code-vb[TPL_Continuations#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/cancellation2.vb#8)]  
  
 継続が <xref:System.Threading.Tasks.TaskStatus.Canceled> 状態になると、それらの継続に指定された <xref:System.Threading.Tasks.TaskContinuationOptions> によっては後続の継続に影響を与える場合があります。  
  
 破棄された継続は開始されません。  
  
## <a name="continuations-and-child-tasks"></a>継続タスクと子タスク  
 継続は、継続元とアタッチされたすべての子タスクが完了するまでは実行されません。 継続は、デタッチされた子タスクの終了は待機しません。 次の 2 つの例に、継続を作成する継続元にアタッチされる子タスクと、継続元からデタッチされる子タスクを示します。 次の例では、すべての子タスクの完了後にのみ継続が実行されます。この例を複数回実行する場合、出力は毎回同一になります。 この例では、<xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=nameWithType> メソッドを呼び出して継続元を起動しています。この理由としては、既定では、<xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType> メソッドによって、<xref:System.Threading.Tasks.TaskCreationOptions.DenyChildAttach?displayProperty=nameWithType> を既定のタスク作成オプションとする親タスクが作成されることが挙げられます。  
  
 [!code-csharp[TPL_Continuations#9](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_continuations/cs/attached1.cs#9)]
 [!code-vb[TPL_Continuations#9](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/attached1.vb#9)]  
  
 ただし子タスクが継続元からデタッチされると、子タスクの状態に関係なく、継続元が終了するとただちに継続が実行されます。 その結果、次の例を複数回実行すると、タスク スケジューラによる子タスクの処理方法に応じて、異なる出力が生成されることがあります。  
  
 [!code-csharp[TPL_Continuations#10](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_continuations/cs/detached1.cs#10)]
 [!code-vb[TPL_Continuations#10](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/detached1.vb#10)]  
  
 継続元の最終的な状態は、アタッチされた子タスクの最終的な状態に依存します。 デタッチされた子タスクの状態は、親には影響しません。 詳細については、「[アタッチされた子タスクとデタッチされた子タスク](../../../docs/standard/parallel-programming/attached-and-detached-child-tasks.md)」を参照してください。  
  
## <a name="associating-state-with-continuations"></a>状態と継続の関連付け  
 任意の状態とタスクの継続を関連付けることができます。 <xref:System.Threading.Tasks.Task.ContinueWith%2A> メソッドは、それぞれが継続の状態を表す <xref:System.Object> 値を受け取る、オーバーロードされたバージョンを提供します。 後でこの状態オブジェクトにアクセスするには、 <xref:System.Threading.Tasks.Task.AsyncState%2A?displayProperty=nameWithType> プロパティを使用します。 値を指定しない場合、この状態オブジェクトは `null` です。  
  
 継続の状態は、TPL を使用するために、[非同期プログラミング モデル (APM)](../../../docs/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm.md) を使った既存のコードを変換する場合に有用です。 APM では通常、**Begin**_Method_ メソッドでオブジェクト状態を指定し、後から <xref:System.IAsyncResult.AsyncState%2A?displayProperty=nameWithType> プロパティを使って、その状態にアクセスします。 <xref:System.Threading.Tasks.Task.ContinueWith%2A> メソッドを使用すると、APM を使用するコードを TPL を使用するように変換するときに、この状態を維持できます。  
  
 継続の状態は、Visual Studio デバッガーで <xref:System.Threading.Tasks.Task> オブジェクトを使用するときにも有用です。 たとえば、 **[並列タスク]** ウィンドウの **[タスク]** の列に、各タスクの状態オブジェクトの文字列表現が表示されます。 **[並列タスク]** ウィンドウの詳細については、「[[タスク] ウィンドウの使用](/visualstudio/debugger/using-the-tasks-window)」を参照してください。  
  
 継続の状態の使用方法を次の例に示します。 この例では、継続タスクのチェーンを作成します。 各タスクは <xref:System.DateTime> メソッドの `state` パラメーターの現在時刻である、 <xref:System.Threading.Tasks.Task.ContinueWith%2A> オブジェクトを提供します。 <xref:System.DateTime> の各オブジェクトは継続タスクが作成された時刻を表します。 各タスクは、結果として、タスクが終了する時刻を表す秒の <xref:System.DateTime> オブジェクトを生成します。 この例では、すべてのタスクが終了した後に、作成時刻および各継続タスクの終了時刻が表示されます。  
  
 [!code-csharp[TPL_ContinuationState#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_continuationstate/cs/continuationstate.cs#1)]
 [!code-vb[TPL_ContinuationState#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuationstate/vb/continuationstate.vb#1)]  
  
## <a name="handling-exceptions-thrown-from-continuations"></a>継続からスローされた例外の処理  
 継続元と継続の関係は、親と子の関係とは異なります。 継続によってスローされた例外は継続元へは反映されません。 したがって、継続によってスローされた例外は、他のタスクでの処理と同様、次のように処理します。  
  
- <xref:System.Threading.Tasks.Task.Wait%2A>メソッド、 <xref:System.Threading.Tasks.Task.WaitAll%2A>メソッド、または <xref:System.Threading.Tasks.Task.WaitAny%2A> メソッドを使用するか、ジェネリック メソッドを使用して、継続での待機を指定します。 次の例に示すように、継続元とその継続は、同じ `try` ステートメントで待機できます。  
  
     [!code-csharp[TPL_Continuations#6](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_continuations/cs/exception1.cs#6)]
     [!code-vb[TPL_Continuations#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/exception1.vb#6)]  
  
- 最初の継続の <xref:System.Threading.Tasks.Task.Exception%2A> プロパティを確認するには、2 番目の継続を使用します。 次の例でタスクは、存在しないファイルからの読み取りを試行します。 その後、継続により、継続元タスクの例外に関する情報が表示されます。  
  
     [!code-csharp[TPL_Continuations#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_continuations/cs/exception2.cs#4)]
     [!code-vb[TPL_Continuations#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/exception2.vb#4)]  
  
     <xref:System.Threading.Tasks.TaskContinuationOptions.OnlyOnFaulted?displayProperty=nameWithType> オプションを指定して実行されたため、継続が実行されるのは、継続元で例外が発生し、継続元の <xref:System.Threading.Tasks.Task.Exception%2A> プロパティが `null`ではないと推測できる場合だけです。 継続元で例外がスローされたかどうかに関係なく継続が実行される場合は、次のコード フラグメントに示すように、例外処理を試行する前に、継続元の <xref:System.Threading.Tasks.Task.Exception%2A> プロパティが `null` ではないことを確認する必要があります。  
  
     [!code-csharp[TPL_Continuations#11](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_continuations/cs/exception2.cs#11)]
     [!code-vb[TPL_Continuations#11](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/exception2.vb#11)]  
  
     詳細については、「[例外処理](../../../docs/standard/parallel-programming/exception-handling-task-parallel-library.md)」を参照してください。  
  
- アタッチされた子タスクで、 <xref:System.Threading.Tasks.TaskContinuationOptions.AttachedToParent?displayProperty=nameWithType> オプションを使用して継続が作成された場合、アタッチされているその他の子と同様、その例外は親によって呼び出し元のスレッドに反映されます。 詳細については、「[アタッチされた子タスクとデタッチされた子タスク](../../../docs/standard/parallel-programming/attached-and-detached-child-tasks.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [タスク並列ライブラリ (TPL)](../../../docs/standard/parallel-programming/task-parallel-library-tpl.md)
