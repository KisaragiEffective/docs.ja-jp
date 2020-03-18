---
title: タスクのキャンセル
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, cancellation
- asynchronous task cancellation
ms.assetid: 3ecf1ea9-e399-4a6a-a0d6-8475f48dcb28
ms.openlocfilehash: 17cabde95644dbc1584dd85b99e26ff7c5cb686d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73139974"
---
# <a name="task-cancellation"></a>タスクのキャンセル
<xref:System.Threading.Tasks.Task?displayProperty=nameWithType> クラスおよび <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> クラスは、.NET Framework のキャンセル トークンを使用したキャンセルをサポートしています。 詳細については、「[マネージド スレッドのキャンセル](../../../docs/standard/threading/cancellation-in-managed-threads.md)」を参照してください。 Task クラスのキャンセル処理では、キャンセル可能な操作を表すユーザー デリゲートと、キャンセルを要求したコードが連携します。  キャンセル処理が正常に実行されるためには、要求コードが <xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=nameWithType> メソッドを呼び出し、ユーザー デリゲートが操作を適時に終了する必要があります。 次のオプションのいずれかを使用して操作を終了できます。  
  
- デリゲートから戻ります。 多くの場合、この処理で十分ですが、この方法で取り消されたタスク インスタンスは、 <xref:System.Threading.Tasks.TaskStatus.RanToCompletion?displayProperty=nameWithType> 状態ではなく、 <xref:System.Threading.Tasks.TaskStatus.Canceled?displayProperty=nameWithType> 状態に遷移します。  
  
- <xref:System.OperationCanceledException> をスローし、これをキャンセルが要求されたトークンに渡します。 これを行うには、 <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A> メソッドを使用する方法をお勧めします。 この方法で取り消されたタスクは Canceled 状態に遷移し、タスクがキャンセル要求に応答したことを確認するために呼び出し元のコードによって使用されます。  
  
 次の例は、例外をスローするタスクのキャンセルの基本的なパターンを示しています。 ユーザー デリゲートとタスク インスタンス自体にトークンが渡されることに注意してください。  
  
 [!code-csharp[TPL_Cancellation#02](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_cancellation/cs/snippet02.cs#02)]
 [!code-vb[TPL_Cancellation#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_cancellation/vb/module1.vb#02)]  
  
 より詳細な例については、「[方法: タスクとその子を取り消す](../../../docs/standard/parallel-programming/how-to-cancel-a-task-and-its-children.md)」を参照してください。  
  
 タスク インスタンスがユーザー コードによってスローされた <xref:System.OperationCanceledException> を確認した場合は、例外のトークンと関連付けられたトークン (タスクを作成した API に渡されたトークン) とを比較します。 これらのトークンが同一であり、トークンの <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> プロパティから true が返されると、タスクはこれをキャンセルの受信確認と解釈し、Canceled 状態に遷移します。 <xref:System.Threading.Tasks.Task.Wait%2A> メソッドまたは <xref:System.Threading.Tasks.Task.WaitAll%2A> メソッドを使用してタスクを待機しない場合、タスクの状態は <xref:System.Threading.Tasks.TaskStatus.Canceled>に設定されます。  
  
 タスクが Canceled 状態に遷移するのを待っていると、<xref:System.Threading.Tasks.TaskCanceledException?displayProperty=nameWithType> 例外 (<xref:System.AggregateException> 例外にラップされている) がスローされます。 この例外は、障害のある状況ではなく、正常なキャンセル処理を示すことに注意してください。 このため、タスクの <xref:System.Threading.Tasks.Task.Exception%2A> プロパティは `null`を返します。  
  
 トークンの <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> プロパティが false を返した場合、または例外のトークンがタスクのトークンと一致しない場合、 <xref:System.OperationCanceledException> は標準の例外のように扱われるため、タスクは Faulted 状態に遷移します。 他の例外が存在する場合も、タスクが Faulted 状態に遷移することに注意してください。 完了したタスクの状態は <xref:System.Threading.Tasks.Task.Status%2A> プロパティで取得できます。  
  
 キャンセルが要求された後も、タスクが一部の項目の処理を継続する可能性があります。  
  
## <a name="see-also"></a>参照

- [マネージド スレッドのキャンセル](../../../docs/standard/threading/cancellation-in-managed-threads.md)
- [方法: タスクとその子を取り消す](../../../docs/standard/parallel-programming/how-to-cancel-a-task-and-its-children.md)
