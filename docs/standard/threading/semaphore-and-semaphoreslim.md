---
title: Semaphore と SemaphoreSlim
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- counting semaphores
- semaphores
- threading [.NET Framework], cross-process synchronization
- Semaphore class, about Semaphore class
- SemaphoreSlim class, about SemaphoreSlim class
- threading [.NET Framework], Semaphore class
ms.assetid: 7722a333-b974-47a2-a7c0-f09097fb644e
ms.openlocfilehash: b9f7c122ac8acf34f740aca5f0fafc162edcea82
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73127585"
---
# <a name="semaphore-and-semaphoreslim"></a>Semaphore と SemaphoreSlim
<xref:System.Threading.Semaphore?displayProperty=nameWithType> クラスは、名前付きセマフォ (システム全体) またはローカル セマフォを表します。 これは、Win32 セマフォ オブジェクトの Thin ラッパーです。 Win32 セマフォは、リソースのプールへのアクセスの制御に使用できるカウント セマフォです。  
  
 <xref:System.Threading.SemaphoreSlim> クラスは軽量で高速のセマフォを表しており、非常に短い待機時間が期待されている場合に単一プロセス内で待機のために使用できます。 <xref:System.Threading.SemaphoreSlim> は、共通言語ランタイム (CLR) により提供される同期プリミティブに可能な限り依存します。 ただし、複数のセマフォでの待機をサポートする必要がある場合は、遅れて初期化されるカーネル ベースの待機ハンドルも提供します。 <xref:System.Threading.SemaphoreSlim> はキャンセル トークンの使用もサポートしていますが、名前付きセマフォや、同期での待機ハンドルの使用はサポートしていません。  
  
## <a name="managing-a-limited-resource"></a>制限されているリソースの管理  
 スレッドは、<xref:System.Threading.WaitHandle.WaitOne%2A> メソッドを呼び出してセマフォに入ります。このメソッドは <xref:System.Threading.WaitHandle> クラスから継承されるか (<xref:System.Threading.Semaphore?displayProperty=nameWithType> オブジェクトの場合)、<xref:System.Threading.SemaphoreSlim.Wait%2A?displayProperty=nameWithType> メソッドまたは <xref:System.Threading.SemaphoreSlim.WaitAsync%2A?displayProperty=nameWithType> メソッドから継承されます (<xref:System.Threading.SemaphoreSlim> オブジェクトの場合)。 呼び出しが戻されると、セマフォのカウントがデクリメントします。 スレッドがエントリを要求し、カウントがゼロの場合、そのスレッドはブロックされます。 スレッドが <xref:System.Threading.Semaphore.Release%2A?displayProperty=nameWithType> メソッドまたは <xref:System.Threading.SemaphoreSlim.Release%2A?displayProperty=nameWithType> メソッドを呼び出してセマフォを解放すると、ブロックされたスレッドがセマフォに入ることができるようになります。 ブロックされたスレッドがセマフォに入る順序には、先入れ先出し (FIFO) や後入れ先出し (LIFO) などの決まった順序がありません。  
  
 スレッドは、<xref:System.Threading.Semaphore?displayProperty=nameWithType> オブジェクトの <xref:System.Threading.WaitHandle.WaitOne%2A> メソッドまたは <xref:System.Threading.SemaphoreSlim> オブジェクトの <xref:System.Threading.SemaphoreSlim.Wait%2A> メソッドを繰り返し呼び出すことで、セマフォに複数回入ることができます。 セマフォを解放するために、スレッドは、<xref:System.Threading.Semaphore.Release?displayProperty=nameWithType> メソッド オーバーロードまたは <xref:System.Threading.SemaphoreSlim.Release?displayProperty=nameWithType> メソッド オーバーロードを同じ回数呼び出すか、<xref:System.Threading.Semaphore.Release%28System.Int32%29?displayProperty=nameWithType> メソッド オーバーロードまたは <xref:System.Threading.SemaphoreSlim.Release%28System.Int32%29?displayProperty=nameWithType> メソッド オーバーロードを呼び出して、解放するエントリ数を指定します。  
  
### <a name="semaphores-and-thread-identity"></a>セマフォとスレッド ID  
 この 2 つの種類のセマフォは、<xref:System.Threading.WaitHandle.WaitOne%2A>、<xref:System.Threading.SemaphoreSlim.Wait%2A>、<xref:System.Threading.Semaphore.Release%2A>、および <xref:System.Threading.SemaphoreSlim.Release%2A?displayProperty=nameWithType> の各メソッドの呼び出しでスレッド ID を適用しません。 たとえば、セマフォの一般的な使用シナリオでは、producer スレッドと consumer スレッドが使用され、一方のスレッドが常にセマフォ カウントをインクリメントし、もう一方のスレッドが常にセマフォ カウントをデクリメントします。  
  
 プログラマは、スレッドによるセマフォの解放回数が多くなりすぎないようにする必要があります。 たとえば、セマフォの最大カウントが 2 で、スレッド A とスレッド B が両方ともセマフォに入るとします。 スレッド B がプログラミング エラーのために `Release` を 2 回呼び出した場合、この呼び出しは両方とも成功します。 セマフォのカウントがいっぱいになっているときに、スレッド A も `Release` を呼び出すと、<xref:System.Threading.SemaphoreFullException> がスローされます。  
  
## <a name="named-semaphores"></a>名前付きセマフォ  
 Windows オペレーティング システムでは、セマフォに名前を付けることができます。 名前付きセマフォはシステム全体でのセマフォです。 つまり、名前付きセマフォは、作成されると、すべてのプロセスのすべてのスレッドで認識されるようになります。 そのため、名前付きセマフォは、プロセスおよびスレッドのアクティビティを同期するために使用できます。  
  
 名前を指定するコンストラクターのいずれかを使用して、名前付きシステム セマフォを表す <xref:System.Threading.Semaphore> オブジェクトを作成できます。  
  
> [!NOTE]
> 名前付きセマフォはシステム全体でのセマフォであるため、同じ名前付きセマフォを表す複数の <xref:System.Threading.Semaphore> オブジェクトが存在する可能性があります。 コンストラクターまたは <xref:System.Threading.Semaphore.OpenExisting%2A?displayProperty=nameWithType> メソッドを呼び出すたびに、新しい <xref:System.Threading.Semaphore> オブジェクトが作成されます。 同じ名前を繰り返し指定すると、同じ名前付きセマフォを表す複数のオブジェクトが作成されます。  
  
 名前付きセマフォを使用する際には注意する必要があります。 システム全体でのセマフォであるため、同じ名前を使用する別のプロセスが予期せずセマフォに入る可能性があります。 同じコンピューター上で実行される悪意のあるコードが、これをサービス拒否攻撃の土台として使用する可能性があります。  
  
 アクセス制御セキュリティを使用して、名前付きセマフォを表す <xref:System.Threading.Semaphore> オブジェクトを保護します。可能であれば <xref:System.Security.AccessControl.SemaphoreSecurity?displayProperty=nameWithType> オブジェクトを指定するコンストラクターを使用して保護します。 <xref:System.Threading.Semaphore.SetAccessControl%2A?displayProperty=nameWithType> メソッドを使用してアクセス制御セキュリティを適用することもできますが、この場合、セマフォが作成されてから保護されるまでの間に無防備な時間帯が生じてしまいます。 アクセス制御セキュリティによりセマフォを保護すると、悪意のある攻撃を防ぐことができますが、意図しない名前の競合の問題の解決にはなりません。  
  
## <a name="see-also"></a>参照

- <xref:System.Threading.Semaphore>
- <xref:System.Threading.SemaphoreSlim>
- [スレッド処理オブジェクトと機能](../../../docs/standard/threading/threading-objects-and-features.md)
