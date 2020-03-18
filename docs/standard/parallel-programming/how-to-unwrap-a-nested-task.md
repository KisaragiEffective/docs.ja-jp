---
title: '方法: 入れ子のタスクのラップを解除する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, how to unwrap nested tasks
ms.assetid: a0769dd2-0f6d-48ca-8418-a9d39de7f450
ms.openlocfilehash: c72654a2bc21035fe706d76018bb163d8ba01ee8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73106900"
---
# <a name="how-to-unwrap-a-nested-task"></a>方法: 入れ子のタスクのラップを解除する
以下の例に示すように、メソッドからタスクを返して、そのタスクで待機またはそのタスクから続行することができます。  
  
 [!code-csharp[TPL_Unwrap#01](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_unwrap/cs/unwrapprogram.cs#01)]
 [!code-vb[TPL_Unwrap#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_unwrap/vb/snippets1-3.vb#01)]  
  
 前の例では、<xref:System.Threading.Tasks.Task%601.Result%2A> プロパティは `string` 型 (Visual Basic では `String`) です。  
  
 ただし、一部のシナリオでは、あるタスクを別のタスク内で作成してから、入れ子のタスクを返すことができます。 その場合、外側のタスクの `TResult` は、それ自体がタスクとなります。 次の例では、Result プロパティは、C# の場合は `Task<Task<string>>`、Visual Basic の場合は `Task(Of Task(Of String))` になります。  
  
 [!code-csharp[TPL_Unwrap#02](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_unwrap/cs/unwrapprogram.cs#02)]
 [!code-vb[TPL_Unwrap#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_unwrap/vb/snippets1-3.vb#02)]  
  
 外側のタスクのラップを解除し、元のタスクとその <xref:System.Threading.Tasks.Task%601.Result%2A> プロパティを取得するようにコードを作成できますが、例外を処理して、キャンセル要求も処理する必要があるため、そのようなコードを作成するのは簡単ではありません。 このような場合は、以下の例に示すように、<xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A> 拡張メソッドのいずれかを使用することをお勧めします。  
  
 [!code-csharp[TPL_UnWrap#03](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_unwrap/cs/unwrapprogram.cs#03)]
 [!code-vb[TPL_UnWrap#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_unwrap/vb/snippets1-3.vb#03)]  
  
 <xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A> メソッドを使用して、`Task<Task>` または `Task<Task<TResult>>` (Visual Basic では `Task(Of Task)` または `Task(Of Task(Of TResult))`) を `Task` または `Task<TResult>` (Visual Basic では `Task(Of TResult)`) に変換することができます。 新しいタスクは完全に内側の入れ子のタスクを表し、キャンセル状態とすべての例外を含みます。  
  
## <a name="example"></a>例  
 <xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A> 拡張メソッドの使用方法を次の例に示します。  
  
 [!code-csharp[TPL_UnWrap#04](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_unwrap/cs/unwrapprogram.cs#04)]
 [!code-vb[TPL_UnWrap#04](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_unwrap/vb/snippet04.vb#04)]  
  
## <a name="see-also"></a>参照

- <xref:System.Threading.Tasks.TaskExtensions?displayProperty=nameWithType>
- [タスク ベースの非同期プログラミング](../../../docs/standard/parallel-programming/task-based-asynchronous-programming.md)
