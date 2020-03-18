---
title: スレッド セーフなコレクション
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- thread-safe collections, overview
ms.assetid: 2e7ca21f-786c-4367-96be-0cf3f3dcc6bd
ms.openlocfilehash: 790543118b18b0422f41c3249512b62aae0cfb03
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75938106"
---
# <a name="thread-safe-collections"></a>スレッド セーフなコレクション
.NET Framework 4 では、スレッド セーフかつスケーラブルなコレクション クラスをいくつか含む <xref:System.Collections.Concurrent?displayProperty=nameWithType> 名前空間が導入されています。 ユーザー コードで同期を追加することなく、複数のスレッドでこのようなコレクションの項目を安全かつ効率的に追加または削除できます。 新しいコードを記述する場合、コレクションに対して複数のスレッドが同時に書き込みを行うときは常に同時実行コレクション クラスを使用します。 共有コレクションの読み取りのみを行う場合は、<xref:System.Collections.Generic?displayProperty=nameWithType> 名前空間のクラスを使用できます。 .NET Framework 1.1 以前のランタイムを対象にする必要がない場合は、1.0 コレクション クラスを使用しないことをお勧めします。  
  
## <a name="thread-synchronization-in-the-net-framework-10-and-20-collections"></a>.NET Framework 1.0 と 2.0 のコレクションのスレッドの同期  
 .NET Framework 1.0 で導入されたコレクションは、<xref:System.Collections?displayProperty=nameWithType> 名前空間にあります。 一般的に使用される <xref:System.Collections.ArrayList> や <xref:System.Collections.Hashtable> を含むこのコレクションは、コレクションのスレッド セーフ ラッパーを返す `Synchronized` プロパティを通じてスレッド セーフを確保します。 このラッパーは、すべての追加操作または削除操作でコレクション全体をロックすることで機能します。 したがって、コレクションにアクセスしようとする各スレッドは、ロックを取得する順番を待機する必要があります。 これはスケーラブルではなく、大規模なコレクションの場合はパフォーマンスが大幅に低下するおそれがあります。 また、競合状態を完全に防ぐことはできません。 詳細については、「[Synchronization in Generic Collections](https://docs.microsoft.com/archive/blogs/bclteam/synchronization-in-generic-collections-brian-grunkemeyer)」 (ジェネリック コレクションでの同期) を参照してください。  
  
 .NET Framework 2.0 で導入されたコレクション クラスは、<xref:System.Collections.Generic?displayProperty=nameWithType> 名前空間にあります。 これには、<xref:System.Collections.Generic.List%601>、<xref:System.Collections.Generic.Dictionary%602> などがあります。 これらのクラスを使用すると、.NET Framework 1.0 クラスと比較して、タイプ セーフおよびパフォーマンスが向上します。 ただし、.NET Framework 2.0 コレクション クラスではスレッドの同期は行われません。複数のスレッドで同時に項目を追加または削除する場合は、ユーザー コードですべての同期を行う必要があります。  
  
 .NET Framework 4 の同時実行コレクション クラスを使用することをお勧めします。このクラスは、.NET Framework 2.0 コレクション クラスのタイプ セーフを確保するだけでなく、.NET Framework 1.0 コレクションよりも効率的で完全なスレッド セーフも確保します。  
  
## <a name="fine-grained-locking-and-lock-free-mechanisms"></a>粒度の細かいロック機構とロック制御の不要な機構  
 同時実行コレクション型には、.NET Framework 4 の新機能である <xref:System.Threading.SpinLock>、<xref:System.Threading.SpinWait>、<xref:System.Threading.SemaphoreSlim>、<xref:System.Threading.CountdownEvent> などの軽量な同期機構を使用するものもあります。 これらの同期型では、通常、スレッドを実際の待機状態にする前の短期間に*ビジー スピン*が使用されます。 待機時間が非常に短くなると予測される場合は、スピンを使用すると、負荷がかかるカーネル遷移を伴う待機を行うよりも負荷が格段に小さくなります。 スピンを使用するコレクション クラスでは、この効率性は、複数のスレッドで項目を高速で追加および削除できることを意味します。 スピンとブロッキングの詳細については、「[SpinLock](../../../../docs/standard/threading/spinlock.md)」および「[SpinWait](../../../../docs/standard/threading/spinwait.md)」を参照してください。  
  
 <xref:System.Collections.Concurrent.ConcurrentQueue%601> クラスと <xref:System.Collections.Concurrent.ConcurrentStack%601> クラスでは、ロックは使用されません。 代わりに、<xref:System.Threading.Interlocked> 操作によってスレッド セーフを確保します。  
  
> [!NOTE]
> 同時実行コレクション クラスでは <xref:System.Collections.ICollection> がサポートされるので、<xref:System.Collections.ICollection.IsSynchronized%2A> プロパティと <xref:System.Collections.ICollection.SyncRoot%2A> プロパティの実装が、これらのプロパティが無関係の場合でも提供されます。 `IsSynchronized` は常に `false` を返し、`SyncRoot` は常に `null` (Visual Basic の場合は `Nothing`) になります。  
  
 <xref:System.Collections.Concurrent?displayProperty=nameWithType> 名前空間に属するコレクション型を次の表に示します。  
  
|[種類]|[説明]|  
|----------|-----------------|  
|<xref:System.Collections.Concurrent.BlockingCollection%601>|<xref:System.Collections.Concurrent.IProducerConsumerCollection%601> を実装する任意の型の境界ブロッキング機能を提供します。 詳細については、「[BlockingCollection の概要](../../../../docs/standard/collections/thread-safe/blockingcollection-overview.md)」を参照してください。|  
|<xref:System.Collections.Concurrent.ConcurrentDictionary%602>|キーと値のペアのディクショナリのスレッド セーフな実装。|  
|<xref:System.Collections.Concurrent.ConcurrentQueue%601>|先入れ先出し (FIFO: First In First Out) キューのスレッド セーフな実装。|  
|<xref:System.Collections.Concurrent.ConcurrentStack%601>|後入れ先出し (LIFO: Last In First Out) スタックのスレッド セーフな実装。|  
|<xref:System.Collections.Concurrent.ConcurrentBag%601>|要素の順序付けられていないコレクションのスレッド セーフな実装。|  
|<xref:System.Collections.Concurrent.IProducerConsumerCollection%601>|`BlockingCollection` で使用するために型が実装する必要があるインターフェイス。|  
  
## <a name="related-topics"></a>関連トピック  
  
|タイトル|[説明]|  
|-----------|-----------------|  
|[BlockingCollection の概要](../../../../docs/standard/collections/thread-safe/blockingcollection-overview.md)|<xref:System.Collections.Concurrent.BlockingCollection%601> 型で提供される機能について説明します。|  
|[方法: ConcurrentDictionary の項目を追加および削除する](../../../../docs/standard/collections/thread-safe/how-to-add-and-remove-items.md)|<xref:System.Collections.Concurrent.ConcurrentDictionary%602> の要素を追加および削除する方法について説明します。|  
|[方法: BlockingCollection の項目を個別に追加および取得する](../../../../docs/standard/collections/thread-safe/how-to-add-and-take-items.md)|読み取り専用の列挙子を使用せずにブロッキング コレクションの項目を追加および取得する方法について説明します。|  
|[方法: 境界ブロッキング機能をコレクションに追加する](../../../../docs/standard/collections/thread-safe/how-to-add-bounding-and-blocking.md)|任意のコレクション クラスを <xref:System.Collections.Concurrent.IProducerConsumerCollection%601> コレクションの基になるストレージ機構として使用する方法について説明します。|  
|[方法: ForEach を使用して BlockingCollection 内の項目を削除する](../../../../docs/standard/collections/thread-safe/how-to-use-foreach-to-remove.md)|`foreach` (Visual Basic の場合は `For Each`) を使用してブロッキング コレクションのすべての項目を削除する方法について説明します。|  
|[方法: パイプラインでブロッキング コレクションの配列を使用する](../../../../docs/standard/collections/thread-safe/how-to-use-arrays-of-blockingcollections.md)|複数のブロッキング コレクションを同時に使用してパイプラインを実装する方法について説明します。|  
|[方法: ConcurrentBag を使用してオブジェクト プールを作成する](../../../../docs/standard/collections/thread-safe/how-to-create-an-object-pool.md)|新しいオブジェクトを頻繁に作成する代わりにオブジェクトを再利用できるシナリオで、同時実行バッグを使用してパフォーマンスを向上させる方法について説明します。|  
  
## <a name="reference"></a>リファレンス  
 <xref:System.Collections.Concurrent?displayProperty=nameWithType>
