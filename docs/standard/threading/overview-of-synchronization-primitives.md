---
title: 同期プリミティブの概要
description: 共有リソースへのアクセスを同期する場合や、スレッド相互作用を制御する場合に使用される .NET スレッド同期プリミティブについて説明します
ms.date: 10/01/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- synchronization, threads
- threading [.NET],synchronizing threads
- managed threading
ms.assetid: b782bcb8-da6a-4c6a-805f-2eb46d504309
ms.openlocfilehash: 43f78c914b7cb01f9b0de4c258d5882548e52790
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73106593"
---
# <a name="overview-of-synchronization-primitives"></a>同期プリミティブの概要

.NET では、共有リソースへのアクセスを同期する場合や、スレッド相互作用を調整する場合に使用できるさまざまな型が提供されます。

> [!IMPORTANT]
> 共有リソースでのアクセスを保護するには、同じ同期プリミティブ インスタンスを使用します。 異なる同期プリミティブ インスタンスを使用して同じリソースを保護する場合は、同期プリミティブによって提供される保護を回避します。

## <a name="waithandle-class-and-lightweight-synchronization-types"></a>WaitHandle クラスと軽量の同期型

複数の .NET 同期プリミティブは <xref:System.Threading.WaitHandle?displayProperty=nameWithType> クラスから派生します。このクラスでは、ネイティブ オペレーティング システムの同期ハンドルをカプセル化し、スレッド相互作用のシグナリング メカニズムを使用します。 次のようなクラスが含まれます。

- <xref:System.Threading.Mutex?displayProperty=nameWithType>。共有リソースへの排他アクセスが許可されます。 所有しているスレッドがない場合、ミューテックスはシグナル状態になります。
- <xref:System.Threading.Semaphore?displayProperty=nameWithType>。共有リソースまたはリソースのプールに同時にアクセスできるスレッドの数を制限します。 セマフォの状態は、そのカウントが 0 より大きい場合はシグナル状態に、0 の場合は非シグナル状態に設定されます。
- <xref:System.Threading.EventWaitHandle?displayProperty=nameWithType>。スレッド同期イベントを表し、シグナル状態または非シグナル状態のいずれかになります。
- <xref:System.Threading.AutoResetEvent?displayProperty=nameWithType>。<xref:System.Threading.EventWaitHandle> から派生し、シグナル状態の場合、単一の待ちスレッドを解放した後、自動的に非シグナル状態にリセットされます。
- <xref:System.Threading.ManualResetEvent?displayProperty=nameWithType>。<xref:System.Threading.EventWaitHandle> から派生し、シグナル状態の場合、<xref:System.Threading.EventWaitHandle.Reset%2A> メソッドが呼び出されるまでシグナル状態のままです。

.NET Framework では、<xref:System.Threading.WaitHandle> は <xref:System.MarshalByRefObject?displayProperty=nameWithType> から派生するため、これらの型を使用して、アプリケーション ドメインの境界を越えてスレッドのアクティビティを同期させることができます。

.NET Framework と .NET Core では、これらの型のいくつかで名前付きのシステム同期ハンドルを表すことができます。ハンドルはオペレーティング システム全体で表示され、プロセス間同期で使用できます。

- <xref:System.Threading.Mutex> (.NET Framework と .NET Core)、
- <xref:System.Threading.Semaphore> (Windows 上の .NET Framework と .NET Core)、
- <xref:System.Threading.EventWaitHandle> (Windows 上の .NET Framework と .NET Core)。

詳細については、<xref:System.Threading.WaitHandle> API リファレンスを参照してください。

軽量の同期型は、基になるオペレーティング システム ハンドルに依存しておらず、通常はパフォーマンスが向上します。 しかし、プロセス間同期には使用できません。 これらの型は、1 つのアプリケーション内のスレッド同期で使用します。

これらの型のいくつかは、<xref:System.Threading.WaitHandle> から派生した型の代わりに使用できます。 たとえば、<xref:System.Threading.SemaphoreSlim> は軽量であり、<xref:System.Threading.Semaphore> の代わりに使用できます。

## <a name="synchronization-of-access-to-a-shared-resource"></a>共有リソースへのアクセスの同期

.NET では、複数のスレッドによる共有リソースへのアクセスを制御するためのさまざまな同期プリミティブが提供されます。

### <a name="monitor-class"></a>Monitor クラス

<xref:System.Threading.Monitor?displayProperty=nameWithType> クラスでは、リソースを識別するオブジェクトに対するロックを取得または解放することで、共有リソースへの排他アクセスを許可します。 ロックが保持されている間、ロックを保持するスレッドではロックを再度取得し、解放することができます。 他のスレッドはブロックされてロックを取得できず、<xref:System.Threading.Monitor.Enter%2A?displayProperty=nameWithType> メソッドではロックが解放されるまで待機します。 <xref:System.Threading.Monitor.Enter%2A> メソッドでは解放されたロックを取得します。 また、<xref:System.Threading.Monitor.TryEnter%2A?displayProperty=nameWithType> メソッドを使用して、スレッドでのロック取得の試行時間を指定することもできます。 <xref:System.Threading.Monitor> クラスにはスレッド アフィニティがあるため、ロックを取得したスレッドでは、<xref:System.Threading.Monitor.Exit%2A?displayProperty=nameWithType> メソッドを呼び出してロックを解放する必要があります。

<xref:System.Threading.Monitor.Wait%2A?displayProperty=nameWithType>、<xref:System.Threading.Monitor.Pulse%2A?displayProperty=nameWithType>、および <xref:System.Threading.Monitor.PulseAll%2A?displayProperty=nameWithType> メソッドを使用して、同じオブジェクトに対するロックを取得するスレッドの相互作用を調整することができます。

詳細については、<xref:System.Threading.Monitor> API リファレンスを参照してください。

> [!NOTE]
> C# では [lock](../../csharp/language-reference/keywords/lock-statement.md) ステートメントを、Visual Basic では [SyncLock](../../visual-basic/language-reference/statements/synclock-statement.md) ステートメントを使って、<xref:System.Threading.Monitor> クラスを直接使用せずに、共有リソースへのアクセスを同期します。 これらのステートメントは、<xref:System.Threading.Monitor.Enter%2A> および <xref:System.Threading.Monitor.Exit%2A> メソッドを使用して実装されます。また、このステートメントでは、取得されたロックが常に確実に解除されるように、`try…finally` ブロックが使用されます。

### <a name="mutex-class"></a>Mutex クラス

<xref:System.Threading.Mutex?displayProperty=nameWithType> クラスでは、<xref:System.Threading.Monitor> と同様に、共有リソースへの排他アクセスを許可します。 [Mutex.WaitOne](<xref:System.Threading.WaitHandle.WaitOne%2A?displayProperty=nameWithType>) メソッドのオーバーロードのいずれかを使用して、ミューテックスの所有権を要求します。 <xref:System.Threading.Monitor> と同様に、<xref:System.Threading.Mutex> にはスレッド アフィニティがあり、ミューテックスを取得したスレッドで、<xref:System.Threading.Mutex.ReleaseMutex%2A?displayProperty=nameWithType> メソッドを呼び出してそれを解放する必要があります。

<xref:System.Threading.Monitor> とは異なり、<xref:System.Threading.Mutex> クラスをプロセス間同期に使用することができます。 そのためには、オペレーティング システム全体で表示される、名前付きミューテックスを使用します。 名前付きミュー テックスのインスタンスを作成するには、名前を指定する [Mutex コンストラクター](<xref:System.Threading.Mutex.%23ctor%2A>)を使用します。 また、<xref:System.Threading.Mutex.OpenExisting%2A?displayProperty=nameWithType> メソッドを呼び出して、既存の名前付きシステム ミューテックスを開くこともできます。
  
詳細については、「[ミューテックス](mutexes.md)」の記事と、<xref:System.Threading.Mutex> API リファレンスを参照してください。

### <a name="spinlock-structure"></a>SpinLock 構造体

<xref:System.Threading.SpinLock?displayProperty=nameWithType> 構造体では、<xref:System.Threading.Monitor> と同様に、ロックの可用性に基づいて、共有リソースへの排他アクセスを許可します。 <xref:System.Threading.SpinLock> で使用できないロックの取得が試行された場合、ループ内で待機し、ロックが使用できるようになるまで繰り返し確認されます。

スピン ロックを使用する利点と欠点の詳細については、「[SpinLock](spinlock.md)」の記事と、<xref:System.Threading.SpinLock> API リファレンスを参照してください。

### <a name="readerwriterlockslim-class"></a>ReaderWriterLockSlim クラス

<xref:System.Threading.ReaderWriterLockSlim?displayProperty=nameWithType> クラスでは、書き込み用の共有リソースへの排他アクセスを許可し、複数のスレッドでの読み取り用のリソースへの同時アクセスを許可します。 スレッドセーフ読み取り操作をサポートするが、書き込み操作を行うために排他アクセスを必要とする共有データ構造体へのアクセスを同期する場合は、<xref:System.Threading.ReaderWriterLockSlim> を使用できます。 スレッドで (たとえば、<xref:System.Threading.ReaderWriterLockSlim.EnterWriteLock%2A?displayProperty=nameWithType> メソッドを呼び出して) 排他アクセスを要求すると、後続のリーダーとライターの要求は、既存のすべてのリーダーがロックを終了し、ライターがロックに参加して終了するまでブロックされます。
  
詳細については、<xref:System.Threading.ReaderWriterLockSlim> API リファレンスを参照してください。

### <a name="semaphore-and-semaphoreslim-classes"></a>Semaphore および SemaphoreSlim クラス

<xref:System.Threading.Semaphore?displayProperty=nameWithType> および <xref:System.Threading.SemaphoreSlim?displayProperty=nameWithType> クラスでは、共有リソースまたはリソースのプールに同時にアクセスできるスレッドの数を制限します。 リソースを要求する追加のスレッドは、任意のスレッドでセマフォが解放されるまで待機します。 セマフォにはスレッド アフィニティがないため、あるスレッドでセマフォを取得し、別のスレッドで解放することができます。

<xref:System.Threading.SemaphoreSlim> は軽量であり、<xref:System.Threading.Semaphore> の代わりに使用され、単一のプロセス境界内の同期化でのみ使用できます。

Windows では、プロセス間同期で <xref:System.Threading.Semaphore> を使用できます。 そのためには、名前または <xref:System.Threading.Semaphore> メソッドを指定する [Semaphore コンストラクター](<xref:System.Threading.Semaphore.%23ctor%2A>)のいずれかを使用して、名前付きシステム セマフォを表す <xref:System.Threading.Semaphore.OpenExisting%2A?displayProperty=nameWithType> インスタンスを作成します。 <xref:System.Threading.SemaphoreSlim> では、名前付きシステム セマフォはサポートされていません。

詳細については、「[Semaphore と SemaphoreSlim](semaphore-and-semaphoreslim.md)」の記事と、<xref:System.Threading.Semaphore> または <xref:System.Threading.SemaphoreSlim> API リファレンスを参照してください。

## <a name="thread-interaction-or-signaling"></a>スレッドの相互作用、またはシグナリング

スレッドの相互作用 (またはスレッドのシグナリング) は、あるスレッドで、先に進むために 1 つ以上のスレッドからの通知 (または、シグナル) を待機する必要があることを意味します。 たとえば、スレッド A でスレッド B の<xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType> メソッドを呼び出す場合、スレッド A は、スレッド B が完了するまでブロックされます。 前のセクションで説明されている同期プリミティブでは、シグナリングに関する別のメカニズムが提供されます。その場合、あるスレッドによって、ロックを獲得することで先に進むことができる別のスレッドに通知されます。

このセクションでは、.NET によって提供される追加のシグナリング構造について説明します。

### <a name="eventwaithandle-autoresetevent-manualresetevent-and-manualreseteventslim-classes"></a>EventWaitHandle、AutoResetEvent、ManualResetEvent、および ManualResetEventSlim クラス

<xref:System.Threading.EventWaitHandle?displayProperty=nameWithType> クラスはスレッドの同期イベントを表します。

同期イベントには、非シグナル状態またはシグナル状態のいずれかを指定できます。 イベントの状態が非シグナルの場合、イベントの <xref:System.Threading.WaitHandle.WaitOne%2A?> オーバーロードを呼び出すスレッドは、イベントがシグナル状態になるまでブロックされます。 <xref:System.Threading.EventWaitHandle.Set%2A?displayProperty=nameWithType> メソッドでは、イベントの状態をシグナル状態に設定します。

シグナル状態になった <xref:System.Threading.EventWaitHandle> の動作は、そのリセット モードによって異なります。

- <xref:System.Threading.EventWaitHandle> フラグで作成された <xref:System.Threading.EventResetMode.AutoReset?displayProperty=nameWithType> では、単一の待ちスレッドが解放された後、自動的にリセットされます。 これは、シグナル状態になるたびに 1 つのスレッドのみが通れる回転ドアのようなものです。 <xref:System.Threading.AutoResetEvent?displayProperty=nameWithType> から派生する、<xref:System.Threading.EventWaitHandle> クラスはその動作を表します。
- <xref:System.Threading.EventWaitHandle> フラグで作成された <xref:System.Threading.EventResetMode.ManualReset?displayProperty=nameWithType> は、その <xref:System.Threading.EventWaitHandle.Reset%2A> メソッドが呼び出されるまで、シグナル状態のままです。 これは、シグナル状態になるまで閉じられ、誰かが閉めるまで開いたままになっている門のようなものです。 <xref:System.Threading.ManualResetEvent?displayProperty=nameWithType> から派生する、<xref:System.Threading.EventWaitHandle> クラスはその動作を表します。 <xref:System.Threading.ManualResetEventSlim?displayProperty=nameWithType> クラスは軽量であり、<xref:System.Threading.ManualResetEvent> の代わりに使用できます。

Windows では、プロセス間同期で <xref:System.Threading.EventWaitHandle> を使用できます。 そのためには、名前または <xref:System.Threading.EventWaitHandle> メソッドを指定する [EventWaitHandle コンストラクター](<xref:System.Threading.EventWaitHandle.%23ctor%2A>)のいずれかを使用して、名前付きシステム同期イベントを表す <xref:System.Threading.EventWaitHandle.OpenExisting%2A?displayProperty=nameWithType> インスタンスを作成します。

詳細については、「[EventWaitHandle](eventwaithandle.md)」の記事を参照してください。 API リファレンスについては、<xref:System.Threading.EventWaitHandle>、<xref:System.Threading.AutoResetEvent>、<xref:System.Threading.ManualResetEvent>、および <xref:System.Threading.ManualResetEventSlim> に関する記述を参照してください。

### <a name="countdownevent-class"></a>CountdownEvent クラス

<xref:System.Threading.CountdownEvent?displayProperty=nameWithType> クラスは、そのカウントが 0 になると設定されるようになるイベントを表します。 <xref:System.Threading.CountdownEvent.CurrentCount?displayProperty=nameWithType> が 0 より大きい間は、<xref:System.Threading.CountdownEvent.Wait%2A?displayProperty=nameWithType> を呼び出すスレッドがブロックされます。 イベントのカウントをデクリメントするには、<xref:System.Threading.CountdownEvent.Signal%2A?displayProperty=nameWithType> を呼び出します。

1 つのスレッドからのシグナルで複数のスレッドのブロックを解除するために使用できる、<xref:System.Threading.ManualResetEvent> や <xref:System.Threading.ManualResetEventSlim> とは対照的に、<xref:System.Threading.CountdownEvent> を使用すると、複数のスレッドからのシグナルで 1 つ以上のスレッドのブロックを解除できます。

詳細については、「[CountdownEvent](countdownevent.md)」の記事と、<xref:System.Threading.CountdownEvent> API リファレンスを参照してください。

### <a name="barrier-class"></a>Barrier クラス

<xref:System.Threading.Barrier?displayProperty=nameWithType> クラスはスレッド実行のバリアを表します。 <xref:System.Threading.Barrier.SignalAndWait%2A?displayProperty=nameWithType> メソッドを呼び出すスレッドでは、バリアに到達したことを知らせ、他の参加スレッドがバリアに到達するまで待機します。 すべての参加スレッドは、バリアに到達したときに先に進み、バリアはリセットされて再度使用することができます。

1 つ以上のスレッドが次の計算フェーズに進む前に、他のスレッドの結果を必要とする場合は、<xref:System.Threading.Barrier> を使用できます。

詳細については、「[バリア](barrier.md)」の記事と、<xref:System.Threading.Barrier> API リファレンスを参照してください。

## <a name="interlocked-class"></a>Interlocked クラス

<xref:System.Threading.Interlocked?displayProperty=nameWithType> クラスでは、変数に対してシンプルなアトミック操作を実行する静的メソッドが提供されます。 それらのアトミック操作としては、64 ビット整数値を対象とした追加、インクリメントとデクリメント、交換、比較による条件付き交換、読み取りの各操作があります。

詳細については、<xref:System.Threading.Interlocked> API リファレンスを参照してください。

## <a name="spinwait-structure"></a>SpinWait 構造体

<xref:System.Threading.SpinWait?displayProperty=nameWithType> 構造体では、スピンベースの待機のサポートが提供されます。 これは、イベントが通知されるか条件が満たされるのをスレッドが待機する必要があるが、待機ハンドルを使用するかあるいはスレッドをブロックする際に必要な待機時間よりも実際の待機時間が短いと思われる場合に使用できます。 <xref:System.Threading.SpinWait> を使用することにより、待機中はスピンし、指定した時間内に条件が満たされなかった場合のみ (たとえば待機またはスリープして) 譲渡するための短い時間を指定できます。

詳細については、「[SpinWait](spinwait.md)」の記事と、<xref:System.Threading.SpinWait> API リファレンスを参照してください。

## <a name="see-also"></a>参照

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [スレッド セーフなコレクション](../collections/thread-safe/index.md)
- [スレッド処理オブジェクトと機能](threading-objects-and-features.md)
