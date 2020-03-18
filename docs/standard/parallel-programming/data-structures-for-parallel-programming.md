---
title: 並列プログラミング向けのデータ構造
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- data structures, multi-threading
ms.assetid: bdc82f2f-4754-45a1-a81e-fe2e9c30cef9
ms.openlocfilehash: a2271feae78100940b4ecac3c42c9bfefa7e1769
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73123147"
---
# <a name="data-structures-for-parallel-programming"></a>並列プログラミング向けのデータ構造
.NET Framework バージョン 4 では、同時実行コレクション クラスのセット、軽量な同期プリミティブ、遅延初期化用の型など、並列プログラミングに役立つ複数の新しい型が導入されています。 これらの型は、タスク並列ライブラリや PLINQ などの任意のマルチスレッド アプリケーション コードで使うことができます。  
  
## <a name="concurrent-collection-classes"></a>同時実行コレクション クラス  
 <xref:System.Collections.Concurrent?displayProperty=nameWithType> 名前空間のコレクション クラスは、できる限りロックを回避するスレッドセーフな追加と削除の操作を提供し、ロックが必要なときは粒度の細かいロックを使います。 .NET Framework バージョン 1.0 および 2.0 で導入されたコレクションとは異なり、同時実行コレクション クラスでは、項目にアクセスするときにユーザー コードでロックを取得する必要はありません。 複数のスレッドがコレクションの項目を追加および削除するシナリオでは、同時実行コレクション クラスを使うと、<xref:System.Collections.ArrayList?displayProperty=nameWithType> や <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> (ユーザー実装のロックを使用) などの型に対するパフォーマンスが大幅に向上します。  
  
 次の表は、新しい同時実行コレクション クラスの一覧です。  
  
|[種類]|[説明]|  
|----------|-----------------|  
|<xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType>|<xref:System.Collections.Concurrent.IProducerConsumerCollection%601?displayProperty=nameWithType> を実装するスレッド セーフなコレクションに、ブロッキングと範囲指定の機能を提供します。 利用できるスロットがない場合、またはコレクションがいっぱいの場合は、プロデューサー スレッドがブロックします。 コレクションが空の場合は、コンシューマー スレッドがブロックします。 この型は、コンシューマーとプロデューサーによる非ブロッキング アクセスもサポートします。 <xref:System.Collections.Concurrent.BlockingCollection%601> は、基底クラスとして、または <xref:System.Collections.Generic.IEnumerable%601> をサポートする任意のコレクション クラスにブロッキングと範囲指定を提供するバッキング ストアとして、使うことができます。|  
|<xref:System.Collections.Concurrent.ConcurrentBag%601?displayProperty=nameWithType>|スケーラブルな追加と取得の操作を提供するスレッドセーフなバッグの実装です。|  
|<xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=nameWithType>|同時実行のスケーラブルなディクショナリ型です。|  
|<xref:System.Collections.Concurrent.ConcurrentQueue%601?displayProperty=nameWithType>|同時実行のスケーラブルな FIFO キューです。|  
|<xref:System.Collections.Concurrent.ConcurrentStack%601?displayProperty=nameWithType>|同時実行のスケーラブルな LIFO スタックです。|  
  
 詳しくは、「[スレッド セーフなコレクション](../../../docs/standard/collections/thread-safe/index.md)」を参照してください。  
  
## <a name="synchronization-primitives"></a>同期プリミティブ  
 <xref:System.Threading?displayProperty=nameWithType> 名前空間の新しい同期プリミティブを使うと、従来のマルチスレッド コードに見られるような高コストのロック メカニズムを回避することで、粒度の細かいコンカレンシーと高速のパフォーマンスが提供されます。 <xref:System.Threading.Barrier?displayProperty=nameWithType> や <xref:System.Threading.CountdownEvent?displayProperty=nameWithType> などの一部の新しい型には、.NET Framework の以前のリリースに対応するものがありません。  
  
 次の表は、新しい同期型の一覧です。  
  
|[種類]|[説明]|  
|----------|-----------------|  
|<xref:System.Threading.Barrier?displayProperty=nameWithType>|各タスクが到着を通知し、一部または全部のタスクが到着するまでブロックすることができるポイントを提供することにより、複数のスレッドが 1 つのアルゴリズムで並列に動作できるようにします。 詳細については、「[バリア](../../../docs/standard/threading/barrier.md)」を参照してください|  
|<xref:System.Threading.CountdownEvent?displayProperty=nameWithType>|簡単なランデブー メカニズムを提供することにより、フォークと結合のシナリオを簡略化します。 詳しくは、「[CountdownEvent](../../../docs/standard/threading/countdownevent.md)」をご覧ください。|  
|<xref:System.Threading.ManualResetEventSlim?displayProperty=nameWithType>|<xref:System.Threading.ManualResetEvent?displayProperty=nameWithType> と同様の同期プリミティブです。 <xref:System.Threading.ManualResetEventSlim> の方が軽量ですが、プロセス内通信にしか使えません。|  
|<xref:System.Threading.SemaphoreSlim?displayProperty=nameWithType>|リソースまたはリソースのプールに同時にアクセスできるスレッドの数を制限する同期プリミティブです。 詳しくは、「[Semaphore と SemaphoreSlim](../../../docs/standard/threading/semaphore-and-semaphoreslim.md)」をご覧ください。|  
|<xref:System.Threading.SpinLock?displayProperty=nameWithType>|ロックを取得しようとしているスレッドがクォンタムを生成する前にしばらくループ ("*スピン*") で待機するようになる、相互排他ロック プリミティブです。 ロックの待機が短いと予想されるシナリオでは、他のロック形式より <xref:System.Threading.SpinLock> の方がよいパフォーマンスを提供します。 詳しくは、「[SpinLock](../../../docs/standard/threading/spinlock.md)」をご覧ください。|  
|<xref:System.Threading.SpinWait?displayProperty=nameWithType>|指定された時間だけスピンし、スピン カウントを超過した場合は最終的にスレッドを待機状態にする、小型で軽量の型です。  詳しくは、「[SpinWait](../../../docs/standard/threading/spinwait.md)」をご覧ください。|  
  
 詳細については次を参照してください:  
  
- [方法: 下位レベルの同期に SpinLock を使用する](../../../docs/standard/threading/how-to-use-spinlock-for-low-level-synchronization.md)  
  
- [方法: バリアを使用して同時実行操作を同期する](../../../docs/standard/threading/how-to-synchronize-concurrent-operations-with-a-barrier.md)。  
  
## <a name="lazy-initialization-classes"></a>遅延初期化クラス  
 遅延初期化では、オブジェクトのメモリは必要になるまで割り当てられません。 遅延初期化を使うと、オブジェクトの割り当てがプログラムの有効期間全体に均等に分散されるので、パフォーマンスが向上します。 <xref:System.Lazy%601> 型をラッピングすることにより、任意のカスタム型で遅延初期化を有効にできます。  
  
 次の表は、遅延初期化型の一覧です。  
  
|[種類]|[説明]|  
|----------|-----------------|  
|<xref:System.Lazy%601?displayProperty=nameWithType>|軽量でスレッドセーフの遅延初期化を提供します。|  
|<xref:System.Threading.ThreadLocal%601?displayProperty=nameWithType>|スレッドごとに遅延初期化された値を提供し、各スレッドは初期化関数を遅延して呼び出します。|  
|<xref:System.Threading.LazyInitializer?displayProperty=nameWithType>|専用の遅延初期化インスタンスを割り当てる必要のない静的メソッドを提供します。 代わりに、参照を使って、ターゲットがアクセスされたときは初期化されているようにします。|  
  
 詳細については、「[限定的な初期化](../../../docs/framework/performance/lazy-initialization.md)」を参照してください。  
  
## <a name="aggregate-exceptions"></a>例外集約  
 <xref:System.AggregateException?displayProperty=nameWithType> 型を使うと、別のスレッドで同時にスローされた複数の例外をキャプチャし、1 つの例外として結合スレッドに戻すことができます。 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 型、<xref:System.Threading.Tasks.Parallel?displayProperty=nameWithType> 型、PLINQ は、この目的に <xref:System.AggregateException> を広範に使います。 詳細については、「[例外処理](../../../docs/standard/parallel-programming/exception-handling-task-parallel-library.md)」および「[方法: PLINQ クエリの例外を処理する](../../../docs/standard/parallel-programming/how-to-handle-exceptions-in-a-plinq-query.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- <xref:System.Threading?displayProperty=nameWithType>
- [並列プログラミング](../../../docs/standard/parallel-programming/index.md)
