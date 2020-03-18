---
title: スレッド処理オブジェクトと機能
ms.date: 10/01/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- threading [.NET Framework], features
- managed threading
ms.assetid: 239b2e8d-581b-4ca3-992b-0e8525b9321c
ms.openlocfilehash: dd9b7b8cb194353d0a1c285af10d54dc7366896e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73128968"
---
# <a name="threading-objects-and-features"></a>スレッド処理オブジェクトと機能

.NET では、<xref:System.Threading.Thread?displayProperty=nameWithType> クラスの他に、マルチ スレッド アプリケーションを開発するのに役立つ複数のクラスが提供されます。 次の記事では、それらのクラスの概要を説明します。

|タイトル|[説明]|  
|-----------|-----------------|  
|[マネージド スレッド プール](the-managed-thread-pool.md)|.NET によって管理されるワーカー スレッドのプールを提供する <xref:System.Threading.ThreadPool?displayProperty=nameWithType> について説明します。|  
|[タイマー](timers.md)|マルチスレッド環境で使用できる .NET タイマーについて説明します。|
|[同期プリミティブの概要](overview-of-synchronization-primitives.md)|共有リソースへのアクセスを同期化する場合や、スレッドの相互作用を制御する場合に使用できる型について説明します。|
|[EventWaitHandle](eventwaithandle.md)|スレッドの同期イベントを表す <xref:System.Threading.EventWaitHandle?displayProperty=nameWithType> クラスについて説明します。|
|[CountdownEvent](countdownevent.md)|そのカウントがゼロのときに設定されるスレッド同期イベントを表す <xref:System.Threading.CountdownEvent?displayProperty=nameWithType> クラスについて説明します。|
|[ミューテックス](mutexes.md)|共有リソースへの排他アクセスを付与する <xref:System.Threading.Mutex?displayProperty=nameWithType> クラスについて説明します。|
|[Semaphore と SemaphoreSlim](semaphore-and-semaphoreslim.md)|共有リソースまたはリソースのプールに同時にアクセスできるスレッドの数を制限する <xref:System.Threading.Semaphore?displayProperty=nameWithType> について説明します。|
|[バリア](barrier.md)|段階的な操作におけるスレッドの調整のためのバリア パターンを実装する <xref:System.Threading.Barrier?displayProperty=nameWithType> クラスについて説明します。|
|[SpinLock](spinlock.md)|特定の下位レベルのシナリオで <xref:System.Threading.SpinLock?displayProperty=nameWithType> ロックの代わりに軽量クラスとして使用できる <xref:System.Threading.Monitor?displayProperty=nameWithType> 構造体について説明します。|
|[SpinWait](spinwait.md)|スピンベースの待機のサポートを提供する <xref:System.Threading.SpinWait?displayProperty=nameWithType> 構造体について説明します。|

## <a name="see-also"></a>参照

- <xref:System.Threading.Monitor?displayProperty=nameWithType>
- <xref:System.Threading.WaitHandle?displayProperty=nameWithType>
- <xref:System.ComponentModel.BackgroundWorker?displayProperty=nameWithType>
- <xref:System.Threading.Tasks.Parallel?displayProperty=nameWithType>
- <xref:System.Threading.Tasks.Task?displayProperty=nameWithType>
- [スレッドの使用とスレッド処理](using-threads-and-threading.md)
- [非同期ファイル I/O](../io/asynchronous-file-i-o.md)
- [並列プログラミング](../parallel-programming/index.md)
- [タスク並列ライブラリ (TPL)](../parallel-programming/task-parallel-library-tpl.md)
