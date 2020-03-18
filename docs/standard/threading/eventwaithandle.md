---
title: EventWaitHandle
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- threading [.NET Framework], EventWaitHandle class
- EventWaitHandle class
- event wait handles [.NET Framework]
- threading [.NET Framework], cross-process synchronization
ms.assetid: 11ee0b38-d663-4617-b793-35eb6c64e9fc
ms.openlocfilehash: 80c90254978495a58d228c4302eda84d6165c800
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73138072"
---
# <a name="eventwaithandle"></a>EventWaitHandle
<xref:System.Threading.EventWaitHandle> クラスを使用すると、スレッドは通知および通知の待機により、互いに通信できます。 イベント待機ハンドル (単にイベントとも呼ばれます) は、通知を受けて、1 つ以上の待機中のイベントを解放できる待機ハンドルです。 通知を受けると、イベント待機ハンドルは手動または自動でリセットされます。 <xref:System.Threading.EventWaitHandle> クラスは、ローカルのイベント待機ハンドル (ローカル イベント) または名前付きのシステム イベント待機ハンドル (名前付きのイベントまたはシステム イベント。すべてのプロセスから参照できます) を表すことができます。  
  
> [!NOTE]
> イベント待機ハンドルは、.NET の[イベント](../events/index.md)ではありません。 デリゲートやイベント ハンドラーは関連していません。 "イベント" という言葉で説明されているのは、それらがこれまでオペレーティング システム イベントと呼ばれており、待機ハンドルの通知はイベントが発生した待機中のスレッドを示すためです。  
  
 ローカル イベント待機ハンドルと名前付きイベント待機ハンドルはどちらも、システム同期オブジェクトを使用します。これは <xref:Microsoft.Win32.SafeHandles.SafeWaitHandle> ラッパーによって保護されており、リソースが確実に解放されるようにします。 <xref:System.Threading.WaitHandle.Dispose%2A> メソッドを使用すると、オブジェクトを使い終わったらすぐにリソースを解放できます。  
  
## <a name="event-wait-handles-that-reset-automatically"></a>自動的にリセットされるイベント待機ハンドル  
 自動リセット イベントを作成するには、<xref:System.Threading.EventResetMode.AutoReset?displayProperty=nameWithType> オブジェクトを作成するときに <xref:System.Threading.EventWaitHandle> を指定します。 その名前が示すとおり、この同期イベントは通知を受けたときに、単一の待機中のスレッドを解放した後、自動的にリセットされます。 イベントを通知するには、その <xref:System.Threading.EventWaitHandle.Set%2A> メソッドを呼び出します。  
  
 通常、自動リセット イベントは、単一のスレッドのリソースに一度に排他アクセスを提供するために使用されます。 スレッドは、<xref:System.Threading.WaitHandle.WaitOne%2A> メソッドを呼び出してリソースを要求します。 他のスレッドに待機ハンドルがない場合、メソッドは `true` を返し、呼び出し元スレッドはリソースの制御権を持ちます。  
  
> [!IMPORTANT]
> あらゆる同期機構と同様に、すべてのコード パスが適切な待機ハンドルを待ってから、保護されているリソースにアクセスする必要があります。 スレッドの同期は連携しています。  
  
 自動リセット イベントが通知を受けたとき、待機中のスレッドがない場合は、スレッドが待機中になるまでシグナル状態のままです。 イベントはスレッドを解放してすぐにリセットされ、以降のスレッドをブロックします。  
  
## <a name="event-wait-handles-that-reset-manually"></a>手動でリセットされるイベント待機ハンドル  
 手動リセット イベントを作成するには、<xref:System.Threading.EventResetMode.ManualReset?displayProperty=nameWithType> オブジェクトを作成するときに <xref:System.Threading.EventWaitHandle> を指定します。 その名前が示すとおり、この同期イベントは、通知を受けた後に手動でリセットする必要があります。 <xref:System.Threading.EventWaitHandle.Reset%2A> メソッドを呼び出してリセットするまで、イベント ハンドルを待っているスレッドはすぐに続行され、ブロックされません。  
  
 手動リセット イベントは、家畜を飼う囲いのゲートのように動作します。 イベントが通知を受けないと、待機中のスレッドは囲いの中の馬のようにブロックされます。 イベントが通知を受けると、その <xref:System.Threading.EventWaitHandle.Set%2A> メソッドを呼び出して、すべての待機中のスレッドは自由に続行できます。 イベントは、その <xref:System.Threading.EventWaitHandle.Reset%2A> メソッドが呼び出されるまで、シグナル状態のままです。 これにより、手動リセット イベントは、1 つのスレッドがタスクを終えるまで待機させる必要のあるスレッドを保持する理想的な方法となっています。  
  
 馬が囲いから出るように、解放されたスレッドがオペレーティング システムによってスケジュールされ、実行が再開されるには時間がかかります。 すべてのスレッドの実行が再開される前に <xref:System.Threading.EventWaitHandle.Reset%2A> メソッドが呼び出された場合、残りのスレッドは再びブロックされます。 再開されるスレッドとブロックされるスレッドは、システム上の負荷や、スケジューラを待っているスレッドの数など、ランダムな要素によって異なります。 イベントを通知したスレッドが通知後に終了した場合 (これは最も一般的な使用パターンです)、これは問題ではありません。 すべての待機中のスレッドが再開された後で、イベントを通知したスレッドに新しいタスクを開始させる場合は、すべての待機中のスレッドが再開されるまで、そのスレッドをブロックする必要があります。 それ以外の場合は競合状態で、コードの動作は予測できません。  
  
## <a name="features-common-to-automatic-and-manual-events"></a>自動イベントと手動イベントの共通の機能  
 通常、ブロックされていないスレッドが <xref:System.Threading.EventWaitHandle> メソッドを呼び出して、いずれかの待機中のスレッド (自動リセット イベントの場合) またはそれらすべて (手動リセット イベントの場合) を解放するまで、1 つ以上のスレッドが <xref:System.Threading.EventWaitHandle.Set%2A> でブロックします。 スレッドは、静的な <xref:System.Threading.EventWaitHandle> メソッドを呼び出すことにより、<xref:System.Threading.WaitHandle.SignalAndWait%2A?displayProperty=nameWithType> の通知とブロックを分割できない操作として行うことができます。  
  
 <xref:System.Threading.EventWaitHandle> オブジェクトは、静的な <xref:System.Threading.WaitHandle.WaitAll%2A?displayProperty=nameWithType> および <xref:System.Threading.WaitHandle.WaitAny%2A?displayProperty=nameWithType> メソッドと共に使用できます。 <xref:System.Threading.EventWaitHandle> および <xref:System.Threading.Mutex> クラスはいずれも <xref:System.Threading.WaitHandle> から派生するため、両方のクラスをこれらのメソッドで使用できます。  
  
### <a name="named-events"></a>名前付きのイベント  
 Windows オペレーティング システムでは、イベント待機ハンドルに名前を付けることができます。 名前付きのイベントはシステム全体で使用されます。 つまり、いったん名前付きのイベントを作成すると、すべてのプロセスのすべてのスレッドがそれを参照できます。 したがって、名前付きのイベントを使用して、スレッドだけでなくプロセスのアクティビティも同期できます。  
  
 名前付きのシステム イベントを表す <xref:System.Threading.EventWaitHandle> オブジェクトを作成するには、イベントの名前を指定するいずれかのコンストラクターを使用します。  
  
> [!NOTE]
> 名前付きのイベントはシステム全体で使用されるため、複数の <xref:System.Threading.EventWaitHandle> オブジェクトで同じ名前付きイベントを表すことができます。 コンストラクターまたは <xref:System.Threading.EventWaitHandle.OpenExisting%2A> メソッドを呼び出すたびに、新しい <xref:System.Threading.EventWaitHandle> オブジェクトが作成されます。 同じ名前を繰り返し指定すると、同じ名前付きイベントを表す複数のオブジェクトを作成できます。  
  
 名前付きのイベントを使用する際には注意が必要です。 それらはシステム全体で使用されるので、別のプロセスが同じ名前を使用すると、スレッドが予期せずにブロックされる場合があります。 同じコンピューター上で実行される悪意のあるコードが、これをサービス拒否攻撃の土台として使用する可能性があります。  
  
 アクセス制御セキュリティを使用して、名前付きのイベントを表す <xref:System.Threading.EventWaitHandle> オブジェクトを保護します。可能であれば <xref:System.Security.AccessControl.EventWaitHandleSecurity> オブジェクトを指定するコンストラクターを使用します。 また、<xref:System.Threading.EventWaitHandle.SetAccessControl%2A> メソッドを使用してアクセス制御セキュリティを適用できますが、この場合、イベント待機ハンドルが作成されてから保護されるまでの間に無防備な時間帯が生じてしまいます。 アクセス制御セキュリティでイベントを保護すると、悪意のある攻撃を防ぐのに役立ちますが、予期しない名前の衝突の問題解決にはなりません。  
  
> [!NOTE]
> <xref:System.Threading.EventWaitHandle> クラスとは異なり、派生したクラスである <xref:System.Threading.AutoResetEvent> および <xref:System.Threading.ManualResetEvent> は、ローカルの待機ハンドルのみを表すことができます。 名前付きのシステム イベントを表すことはできません。  
  
## <a name="see-also"></a>参照

- <xref:System.Threading.EventWaitHandle>
- <xref:System.Threading.WaitHandle>
- <xref:System.Threading.AutoResetEvent>
- <xref:System.Threading.ManualResetEvent>
