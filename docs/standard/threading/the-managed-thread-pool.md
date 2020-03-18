---
title: マネージド スレッド プール
description: バックグラウンド ワーカー スレッドを提供する .NET スレッド プールについて説明します
ms.date: 08/02/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- thread pooling [.NET]
- thread pools [.NET]
- threading [.NET], thread pool
- threading [.NET], pooling
ms.assetid: 2be05b06-a42e-4c9d-a739-96c21d673927
ms.openlocfilehash: 2671ce7c9721b15de8a3805da27040e973a62804
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79398557"
---
# <a name="the-managed-thread-pool"></a>マネージド スレッド プール

<xref:System.Threading.ThreadPool?displayProperty=nameWithType> クラスを使用すると、システムによって管理されるワーカー スレッドのプールがアプリケーションに提供されます。これを利用すると、開発者は、スレッド管理ではなくアプリケーション タスクに注意を集中できるようになります。 バックグラウンド処理が必要な短いタスクがある場合、マネージド スレッド プールを使用すると、複数のスレッドを簡単に利用できます。 Framework 4 以降ではスレッド プールのスレッドで非同期タスクを実行する <xref:System.Threading.Tasks.Task> オブジェクトと <xref:System.Threading.Tasks.Task%601> オブジェクトを作成できるため、スレッド プールが飛躍的に使いやすくなっています。  
  
.NET では、[タスク並列ライブラリ (TPL)](../parallel-programming/task-parallel-library-tpl.md)の操作、非同期 I/O 完了、[タイマー](timers.md) コールバック、登録済みの待機操作、デリゲートを使用した非同期メソッド呼び出し、<xref:System.Net?displayProperty=nameWithType> ソケット接続などのさまざまな目的でスレッド プールのスレッドを使用します。  

## <a name="thread-pool-characteristics"></a>スレッド プールの特徴

スレッド プールのスレッドは[バックグラウンド](foreground-and-background-threads.md) スレッドです。 各スレッドは、既定のスタック サイズを使用し、既定の優先順位で実行されます。また、各スレッドはマルチスレッド アパートメント内にあります。 タスクを完了したスレッド プール内のスレッドは待機スレッドのキューに戻ります。 再利用はこの時点から可能です。 これにより、タスクごとに新しいスレッドを作成するという負荷がアプリケーションにかからなくなります。
  
プロセスごとにスレッド プールが 1 つだけあります。  
  
### <a name="exceptions-in-thread-pool-threads"></a>スレッド プールのスレッドでの例外

スレッド プールのスレッドで未処理の例外が発生すると、プロセスが終了します。 このルールには次の 3 つの例外があります。  
  
- <xref:System.Threading.ThreadAbortException?displayProperty=nameWithType> が呼び出されたため、スレッド プールのスレッドに <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> がスローされる。  
- アプリケーション ドメインがアンロードされるため、スレッド プールのスレッドに <xref:System.AppDomainUnloadedException?displayProperty=nameWithType> がスローされる。  
- 共通言語ランタイムまたはホスト プロセスがスレッドを終了する。  
  
詳しくは、「[マネージド スレッドの例外](exceptions-in-managed-threads.md)」をご覧ください。  
  
### <a name="maximum-number-of-thread-pool-threads"></a>スレッド プールのスレッドの最大数

スレッド プールのキューに登録できる操作の数は、使用可能なメモリによってのみ制限されます。 ただし、スレッド プールによって、プロセスで同時にアクティブにできるスレッドの数が制限されます。 すべてのスレッド プールのスレッドがビジー状態の場合、追加の作業項目は、それらを実行するスレッドが空くまでキューに登録されます。 .NET Framework 4 以降では、プロセスのスレッド プールの既定のサイズは、仮想アドレス空間のサイズなど、いくつかの要素によって決まります。 スレッドの数は、プロセスで <xref:System.Threading.ThreadPool.GetMaxThreads%2A?displayProperty=nameWithType> メソッドを呼び出せば確認できます。  
  
スレッドの最大数を制御するには、<xref:System.Threading.ThreadPool.GetMaxThreads%2A?displayProperty=nameWithType> メソッドと <xref:System.Threading.ThreadPool.SetMaxThreads%2A?displayProperty=nameWithType> メソッドを使用します。  

> [!NOTE]
> 共通言語ランタイムをホストするコードでは、[`ICorThreadpool::CorSetMaxThreads`](../../framework/unmanaged-api/hosting/icorthreadpool-corsetmaxthreads-method.md) メソッドを使用してサイズを設定できます。  
  
### <a name="thread-pool-minimums"></a>スレッド プールの最小値

スレッド プールでは、カテゴリごとに指定された最小値に達するまで、要求に応じて新しいワーカー スレッドまたは I/O 完了スレッドが提供されます。 これらの最小値は、<xref:System.Threading.ThreadPool.GetMinThreads%2A?displayProperty=nameWithType> メソッドを使用して取得できます。  
  
> [!NOTE]
> 要求が少ないときは、スレッド プールの実際のスレッド数が最小値を下回る場合があります。  
  
スレッド プールの最小値に達すると、追加のスレッドが作成されるか、いくつかのタスクが完了するまで待機状態になります。 .NET Framework 4 以降では、スループットを最適化するために、スレッド プールでワーカー スレッドの作成と破棄が行われます。スループットは、タスクの単位時間あたりの完了数として定義されます。 スレッドが少なすぎると使用可能なリソースが最適に使用されない可能性があり、スレッドが多すぎるとリソースの競合が増える可能性があります。  
  
> [!CAUTION]
> アイドル スレッドの最小数は、<xref:System.Threading.ThreadPool.SetMinThreads%2A?displayProperty=nameWithType> メソッドを使用して増やすことができます。 ただし、これらの値を必要以上に大きくすると、パフォーマンスの問題が発生する可能性があります。 同時に開始するタスクの数が多すぎる場合は、すべてのタスクで処理速度が低下する可能性があります。 ほとんどの場合、スレッドを割り当てるためのスレッド プール独自のアルゴリズムを使用することでスレッド プールのパフォーマンスが向上します。  

## <a name="using-the-thread-pool"></a>スレッド プールの使用

.NET Framework 4 以降でスレッド プールを使用する場合は、[タスク並列ライブラリ (TPL)](../parallel-programming/task-parallel-library-tpl.md) を使用すると最も簡単です。 <xref:System.Threading.Tasks.Task> や <xref:System.Threading.Tasks.Task%601> などの TPL の型では、既定でスレッド プールのスレッドを使用してタスクが実行されます。

また、マネージド コードから <xref:System.Threading.ThreadPool.QueueUserWorkItem%2A?displayProperty=nameWithType> (またはアンマネージド コードから [`ICorThreadpool::CorQueueUserWorkItem`](../../framework/unmanaged-api/hosting/icorthreadpool-corqueueuserworkitem-method.md)) を呼び出し、タスクを実行するメソッドを表す <xref:System.Threading.WaitCallback?displayProperty=nameWithType> デリゲートを渡すことによってスレッド プールを使用することもできます。

スレッド プールを使用するもう 1 つの方法として、待機操作の関連ワーク アイテムをキューに置く方法もあります。これには、<xref:System.Threading.ThreadPool.RegisterWaitForSingleObject%2A?displayProperty=nameWithType> メソッドを使用し、<xref:System.Threading.WaitHandle?displayProperty=nameWithType> を渡します。その WaitHandle がシグナル状態になるかタイムアウトになると、<xref:System.Threading.WaitOrTimerCallback?displayProperty=nameWithType> デリゲートによって表されるメソッドが呼び出されます。 スレッド プールのスレッドを使用してコールバック メソッドが呼び出されます。  

参照先の API ページを例としてご確認ください。
  
## <a name="skipping-security-checks"></a>セキュリティ チェックのスキップ

スレッド プールでは、<xref:System.Threading.ThreadPool.UnsafeQueueUserWorkItem%2A?displayProperty=nameWithType> メソッドと <xref:System.Threading.ThreadPool.UnsafeRegisterWaitForSingleObject%2A?displayProperty=nameWithType> メソッドも使用できます。 これらのメソッドは、呼び出し元のスタックが、キューに配置されているタスクの実行時に行われるセキュリティ チェックと無関係であることが確認できる場合にのみ使用します。 <xref:System.Threading.ThreadPool.QueueUserWorkItem%2A?displayProperty=nameWithType> と <xref:System.Threading.ThreadPool.RegisterWaitForSingleObject%2A?displayProperty=nameWithType> はどちらも呼び出し元のスタックを取り込みます。このスタックは、スレッドがタスクの実行を開始するときにスレッド プールのスレッドのスタックにマージされます。 セキュリティ チェックが必要な場合は、そのスタック全体をチェックする必要があります。 チェックは安全性を提供しますが、パフォーマンスへの影響もあります。  

## <a name="when-not-to-use-thread-pool-threads"></a>スレッド プールのスレッドを使用しない場合

次のような場合は、スレッド プールのスレッドを使用する代わりに独自のスレッドを作成して管理する方が適切です。  
  
- フォア グラウンド スレッドが必要な場合。  
- スレッドに特定の優先順位を設定する必要がある場合。  
- スレッドを長時間にわたってブロックするタスクがある場合。 スレッド プールには最大数のスレッドが存在するため、多数のスレッド プール スレッドがブロックされると、タスクを開始できなくなることがあります。  
- スレッドをシングルスレッド アパートメント内に配置する必要がある場合。 <xref:System.Threading.ThreadPool> スレッドはすべてマルチスレッド アパートメント内にあります。  
- 安定した ID をスレッドに関連付ける必要がある場合、またはスレッドを特定のタスク専用にする必要がある場合。  
  
## <a name="see-also"></a>参照

- <xref:System.Threading.ThreadPool?displayProperty=nameWithType>
- <xref:System.Threading.Tasks.Task?displayProperty=nameWithType>
- <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType>
- [タスク並列ライブラリ (TPL)](../parallel-programming/task-parallel-library-tpl.md)
- [方法: タスクから値を返す](../parallel-programming/how-to-return-a-value-from-a-task.md)
- [スレッド処理オブジェクトと機能](threading-objects-and-features.md)
- [スレッドおよびスレッド処理](threads-and-threading.md)
- [非同期ファイル I/O](../io/asynchronous-file-i-o.md)
- [タイマー](timers.md)
