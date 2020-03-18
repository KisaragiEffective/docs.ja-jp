---
title: スレッドの一時中断および中断
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- interrupting threads
- threading [.NET Framework], pausing
- pausing threads
ms.assetid: 9fce4859-a19d-4506-b082-7dd0792688ca
ms.openlocfilehash: 3020694b93479d5f1d64d31c203f8fe033a10320
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73129002"
---
# <a name="pausing-and-interrupting-threads"></a>スレッドの一時中断および中断

スレッドの活動を同期する最も一般的な方法は、スレッドのブロックと解放を行うか、コード領域またはオブジェクトをロックすることです。 ロックとブロックのしくみの詳細については、「[同期プリミティブの概要](../../../docs/standard/threading/overview-of-synchronization-primitives.md)」を参照してください。  
  
 また、スレッドそのものをスリープ状態にすることもできます。 スレッドがブロックされているかまたはスリープ状態の場合は、<xref:System.Threading.ThreadInterruptedException> を使用して待機状態を解除できます。  
  
## <a name="the-threadsleep-method"></a>Thread.Sleep メソッド

 <xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType> メソッドを呼び出すと、メソッドに渡した時間の間または数ミリ秒間、現在のスレッドがすぐにブロックされ、残りのタイム スライスは別のスレッドに生成されます。 時間が経過すると、スリープ状態のスレッドは実行を再開します。  
  
 もう一方のスレッドが他方のスレッドに対して <xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType> を呼び出すことはできません。  <xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType> は静的メソッドであり、常に現在のスレッドがスリープ状態になります。  
  
 <xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType> の値を指定して <xref:System.Threading.Timeout.Infinite?displayProperty=nameWithType> を呼び出すと、スリープ状態のスレッドで <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> メソッドを呼び出す別のスレッドによって中断されるか、<xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> メソッドの呼び出しによって中止されるまで、スリープ状態になります。  次の例は、スリープ状態のスレッドを中断する両方の方法を示しています。  
  
 [!code-csharp[Conceptual.Threading.Resuming#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.Threading.Resuming/cs/Sleep1.cs#1)]
 [!code-vb[Conceptual.Threading.Resuming#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.Threading.Resuming/vb/Sleep1.vb#1)]  
  
## <a name="interrupting-threads"></a>スレッドの中断

 待機中のスレッドを中断するには、ブロックされているスレッドに対して <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> メソッドを呼び出して <xref:System.Threading.ThreadInterruptedException> をスローさせます。これにより、スレッドは中断され、ブロックしている呼び出しから抜け出します。 スレッドは、<xref:System.Threading.ThreadInterruptedException> をキャッチし、操作を継続するために適切な処理を行う必要があります。 スレッドがこの例外を無視した場合は、ランタイムがこの例外をキャッチし、そのスレッドを停止します。  
  
> [!NOTE]
> <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> が呼び出されたときに対象となるスレッドがブロックされていない場合、スレッドはブロックされるまで中断されません。 スレッドがまったくブロックされない場合は、中断されることなく完了することがあります。  
  
 待機がマネージド待機である場合、<xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> と <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> はどちらもすぐにスレッドを起動します。 待機がアンマネージ待機の場合 (プラットフォームが Win32 [WaitForSingleObject](/windows/desktop/api/synchapi/nf-synchapi-waitforsingleobject) 関数を呼び出した場合など)、<xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> と <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> はどちらも、スレッドがマネージド コードに戻るか、またはマネージド コードを呼び出すまで、そのスレッドを制御できません。 マネージド コードの動作は次のとおりです。  
  
- <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> はスレッドをどのような待機からも起動し、これによって起動先のスレッドで <xref:System.Threading.ThreadInterruptedException> がスローされます。  
  
- <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> はスレッドをどのような待機からも起動し、これによってスレッドで <xref:System.Threading.ThreadAbortException> がスローされます。 詳細については、「[スレッドの破棄](../../../docs/standard/threading/destroying-threads.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Threading.Thread>
- <xref:System.Threading.ThreadInterruptedException>
- <xref:System.Threading.ThreadAbortException>
- [スレッド化](../../../docs/standard/threading/index.md)
- [スレッドの使用とスレッド処理](../../../docs/standard/threading/using-threads-and-threading.md)
- [同期プリミティブの概要](../../../docs/standard/threading/overview-of-synchronization-primitives.md)
