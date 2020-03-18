---
title: '方法: ポーリングによりキャンセル要求を待機する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cancellation, how to poll for requests
ms.assetid: c7f2f022-d08e-4e00-b4eb-ae84844cb1bc
ms.openlocfilehash: df76674e3003bbb77ef062e90b1dc3283f681d35
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73138022"
---
# <a name="how-to-listen-for-cancellation-requests-by-polling"></a>方法: ポーリングによりキャンセル要求を待機する
次の例は、ユーザー コードで取り消しトークンを定期的にポーリングし、呼び出し元のスレッドから取り消しが要求されているかどうかを確認する 1 つの方法を示しています。 この例では <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 型を使用しますが、<xref:System.Threading.ThreadPool?displayProperty=nameWithType> 型または <xref:System.Threading.Thread?displayProperty=nameWithType> 型で直接作成される非同期操作にも同じパターンが適用されます。  
  
## <a name="example"></a>例  
 ポーリングには、ブール <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> プロパティの値を定期的に読み取ることができる、ある種のループまたは再帰的なコードが必要になります。 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 型を使用し、タスクが呼び出し元スレッドで完了するまで待機する場合は、<xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A> スレッドを使用してプロパティを確認し、例外をスローすることができます。 このメソッドを使用することで、要求に応じて、正しい例外がスローされるようになります。 <xref:System.Threading.Tasks.Task> を使用する場合は、<xref:System.OperationCanceledException> を手動でスローするよりもこのスレッドを呼び出す方が効果的です。 例外をスローする必要がない場合は、単にプロパティを確認し、プロパティが `true` である場合はメソッドから制御を返すことができます。  
  
 [!code-csharp[Cancellation#11](../../../samples/snippets/csharp/VS_Snippets_Misc/cancellation/cs/cancellationex11.cs#11)]
 [!code-vb[Cancellation#11](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cancellation/vb/cancellationex11.vb#11)]  
  
 <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A> の呼び出しは非常に高速で、ループで大きなオーバーヘッドが発生することはありません。  
  
 <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A> を呼び出す場合に、例外をスローするだけではなく、取り消しに応じて他の作業を行う場合は <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> プロパティを明示的に確認するだけで済みます。 この例では、コードで実際にプロパティに 2 回アクセスするのがわかります。つまり、明示的なアクセスで 1 回、<xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A> メソッドでもう 1 回です。 ただし、<xref:System.Threading.CancellationToken.IsCancellationRequested%2A> プロパティの読み取り操作ではアクセスごとに 1 つの volatile 読み取りのみが含まれるため、パフォーマンスの観点からは二重アクセスは重要ではありません。 それでも、<xref:System.OperationCanceledException> を手動でスローするよりメソッドを呼び出すことをお勧めします。  
  
## <a name="see-also"></a>参照

- [マネージド スレッドのキャンセル](../../../docs/standard/threading/cancellation-in-managed-threads.md)
