---
title: CountdownEvent
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- synchronization primitives, CountdownEvent
ms.assetid: eec3812a-e20f-4ecd-bfef-6921d508b708
ms.openlocfilehash: 628d6a96606117d447c61d01595d13dd4a957ce4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73138108"
---
# <a name="countdownevent"></a>CountdownEvent
<xref:System.Threading.CountdownEvent?displayProperty=nameWithType> は同期プリミティブであり、特定の回数シグナル状態になった後、待機スレッドのブロックを解除します。 <xref:System.Threading.CountdownEvent> は、通常は <xref:System.Threading.ManualResetEvent> または <xref:System.Threading.ManualResetEventSlim> を使用して、イベントをシグナル化する前に変数を手動でデクリメントする必要があるシナリオ用に設計されています。 たとえば、fork/join シナリオでは、単にシグナル数 5 の <xref:System.Threading.CountdownEvent> を作成し、スレッド プールで 5 つの作業項目を開始し、完了時に各作業項目呼び出し <xref:System.Threading.CountdownEvent.Signal%2A> を使用できます。 <xref:System.Threading.CountdownEvent.Signal%2A> を呼び出すたびに、シグナル数が 1 だけデクリメントします。 メイン スレッドでは、シグナル数がゼロになるまで、<xref:System.Threading.CountdownEvent.Wait%2A> の呼び出しがブロックされます。  
  
> [!NOTE]
> レガシ .NET Framework の同期 API と対話する必要がないコードの場合は、fork と join の並列実行をさらに簡単に表すために <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> オブジェクトまたは <xref:System.Threading.Tasks.Parallel.Invoke%2A> メソッドの使用を検討してください。  
  
 <xref:System.Threading.CountdownEvent> には以下の追加機能があります。  
  
- 取り消しトークンを使用して、待機操作を取り消すことができます。  
  
- インスタンスの作成後に、シグナル数をインクリメントできます。  
  
- <xref:System.Threading.CountdownEvent.Wait%2A> メソッドを呼び出すことで <xref:System.Threading.CountdownEvent.Reset%2A> が返された後、インスタンスを再利用できます。  
  
- インスタンスは、<xref:System.Threading.WaitHandle> などの他の .NET Framework の同期 API との統合のために <xref:System.Threading.WaitHandle.WaitAll%2A> を公開します。  
  
## <a name="basic-usage"></a>基本的な使用方法  
 <xref:System.Threading.CountdownEvent> と <xref:System.Threading.ThreadPool> 作業項目を使用する方法を次の例に示します。  
  
 [!code-csharp[CDS_CountdownEvent#01](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_countdownevent/cs/countdownevent.cs#01)]
 [!code-vb[CDS_CountdownEvent#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_countdownevent/vb/module1.vb#01)]  
  
## <a name="countdownevent-with-cancellation"></a>CountdownEvent と Cancellation  
 次の例は、取り消しトークンを使用して、<xref:System.Threading.CountdownEvent> での待機操作を取り消す方法を示しています。 基本的なパターンでは、.NET Framework 4 で導入された統合取り消しのモデルに従います。 詳細については、「[マネージド スレッドのキャンセル](../../../docs/standard/threading/cancellation-in-managed-threads.md)」を参照してください。  
  
 [!code-csharp[CDS_CountdownEvent#02](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_countdownevent/cs/countdownevent.cs#02)]
 [!code-vb[CDS_CountdownEvent#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_countdownevent/vb/canceleventwait.vb#02)]  
  
 待機操作では、それをシグナル化するスレッドが取り消されないことに注意してください。 通常、取り消しは論理操作に適用され、待機が同期中のすべての作業項目だけでなく、イベントでの待機を含めることができます。 この例では、各作業項目には同じ取り消しトークンのコピーが渡されるため、取り消し要求に応答することができます。  
  
## <a name="see-also"></a>参照

- <xref:System.Threading.Semaphore?displayProperty=nameWithType>
