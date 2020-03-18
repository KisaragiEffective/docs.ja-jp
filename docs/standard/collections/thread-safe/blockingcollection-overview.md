---
title: BlockingCollection の概要
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- BlockingCollection, overview
ms.assetid: 987ea3d7-0ad5-4238-8b64-331ce4eb3f0b
ms.openlocfilehash: fb01d29c723962e28d8ec4afc984cb4d6c48f9b5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75711325"
---
# <a name="blockingcollection-overview"></a>BlockingCollection の概要
<xref:System.Collections.Concurrent.BlockingCollection%601> は、次の機能を提供するスレッド セーフなコレクション クラスです。  
  
- Producer-Consumer パターンの実装。  
  
- 複数のスレッドから同時に項目を追加し、取得する。  
  
- 最大容量 (オプション)。  
  
- コレクションが空またはいっぱいになったときにブロックする挿入操作と削除操作。  
  
- ブロックせずに実行されるか、指定された時間が経過するまでブロックする、挿入および削除の "試行" 操作。  
  
- <xref:System.Collections.Concurrent.IProducerConsumerCollection%601> を実装する任意のコレクション型のカプセル化。  
  
- キャンセル トークンを使用したキャンセル。  
  
- `foreach` を使用した 2 種類の列挙 (Visual Basic の場合は `For Each`)。  
  
    1. 読み取り専用の列挙。  
  
    2. 列挙された項目を削除する列挙。  
  
## <a name="bounding-and-blocking-support"></a>境界とブロッキングのサポート  
 <xref:System.Collections.Concurrent.BlockingCollection%601> はバインドとブロックに対応しています。 境界は、コレクションの最大容量を設定できることを意味します。 境界を使用すると、メモリ内のコレクションの最大サイズを制御し、producer スレッドが consumer スレッドよりも先に進行しすぎるのを防ぐことができます。これは、特定のシナリオで重要になります。  
  
 コレクションには、複数のスレッドやタスクが同時に項目を追加できます。コレクションが指定された最大容量に達すると、producer スレッドは項目が削除されるまでブロックします。 複数の cosumer が同時に項目を削除できます。コレクションが空になった場合、consumer スレッドは、producer が項目を追加するまでブロックします。 producer スレッドでは、それ以上項目が追加されないことを示すために、<xref:System.Collections.Concurrent.BlockingCollection%601.CompleteAdding%2A> を呼び出すことができます。 consumer では、<xref:System.Collections.Concurrent.BlockingCollection%601.IsCompleted%2A> プロパティを監視して、コレクションが空になったときや、それ以上の項目は追加されないことになったときを把握できます。 次の例は、容量の上限が 100 に設定された単純な BlockingCollection を示しています。 いくつかの外部条件が true である間、producer タスクはコレクションに項目を追加し、<xref:System.Collections.Concurrent.BlockingCollection%601.CompleteAdding%2A> を呼び出します。 consumer タスクは、<xref:System.Collections.Concurrent.BlockingCollection%601.IsCompleted%2A> プロパティが true になるまで項目を取得します。  
  
 [!code-csharp[CDS_BlockingCollection#04](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/blockingcollection.cs#04)]
 [!code-vb[CDS_BlockingCollection#04](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/introsnippetsbc.vb#04)]  
  
 コード例全体については、「[方法: BlockingCollection の項目を個別に追加および取得する](../../../../docs/standard/collections/thread-safe/how-to-add-and-take-items.md)」を参照してください。  
  
## <a name="timed-blocking-operations"></a>時間制限付きのブロッキング操作  
 境界のあるコレクションにおける時間制限付きの <xref:System.Collections.Concurrent.BlockingCollection%601.TryAdd%2A> 操作および <xref:System.Collections.Concurrent.BlockingCollection%601.TryTake%2A> 操作では、メソッドは項目を追加または取得しようと試みます。 項目を使用できる場合、その項目は参照渡しで渡された変数に格納され、メソッドは true を返します。 指定されたタイムアウト期間を経過しても項目が取得されない場合、メソッドは false を返します。 その後、再びコレクションへのアクセスを試みる前に、他の必要な処理をスレッドで自由に実行できます。 時間制限付きのブロッキング アクセスの例については、「[方法: BlockingCollection の項目を個別に追加および取得する](../../../../docs/standard/collections/thread-safe/how-to-add-and-take-items.md)」の 2 番目の例を参照してください。  
  
## <a name="cancelling-add-and-take-operations"></a>追加操作と取得操作の取り消し  
 追加操作と取得操作は、通常、ループ内で実行されます。 <xref:System.Threading.CancellationToken> メソッドまたは <xref:System.Collections.Concurrent.BlockingCollection%601.TryAdd%2A> メソッドに <xref:System.Collections.Concurrent.BlockingCollection%601.TryTake%2A> を渡し、各イテレーションでトークンの <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> プロパティの値を確認するようにすると、ループを取り消すことができます。 値が true である場合は、キャンセル要求に応答するかどうかを決定できます。応答するには、リソースをクリーンアップし、ループを終了します。 次の例は、キャンセル トークンを受け取る <xref:System.Collections.Concurrent.BlockingCollection%601.TryAdd%2A> のオーバーロードと、それを使用するコードを示しています。  
  
 [!code-csharp[CDS_BlockingCollection#05](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/blockingcollection.cs#05)]
 [!code-vb[CDS_BlockingCollection#05](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/introsnippetsbc.vb#05)]  
  
 キャンセル サポートの追加方法の例については、「[方法: BlockingCollection の項目を個別に追加および取得する](../../../../docs/standard/collections/thread-safe/how-to-add-and-take-items.md)」の 2 番目の例を参照してください。  
  
## <a name="specifying-the-collection-type"></a>コレクションの型の指定  
 <xref:System.Collections.Concurrent.BlockingCollection%601> の作成時には、容量の上限だけでなく、使用するコレクションの型も指定できます。 たとえば、先入れ先出し法 (FIFO) に <xref:System.Collections.Concurrent.ConcurrentQueue%601> を指定したり、後入れ先出し法 (LIFO) に <xref:System.Collections.Concurrent.ConcurrentStack%601> を指定したりできます。 <xref:System.Collections.Concurrent.IProducerConsumerCollection%601> インターフェイスを実装する任意のコレクション クラスを使用できます。 <xref:System.Collections.Concurrent.BlockingCollection%601> の既定のコレクション型は <xref:System.Collections.Concurrent.ConcurrentQueue%601> です。 次のコード例は、1000 という容量を持ち、<xref:System.Collections.Concurrent.BlockingCollection%601> を使用する文字列の <xref:System.Collections.Concurrent.ConcurrentBag%601> を作成する方法を示しています。  
  
```vb  
Dim bc = New BlockingCollection(Of String)(New ConcurrentBag(Of String()), 1000)  
```  
  
```csharp  
BlockingCollection<string> bc = new BlockingCollection<string>(new ConcurrentBag<string>(), 1000 );  
```  
  
 詳細については、「[方法: 境界ブロッキング機能をコレクションに追加する](../../../../docs/standard/collections/thread-safe/how-to-add-bounding-and-blocking.md)」を参照してください。  
  
## <a name="ienumerable-support"></a>IEnumerable のサポート  
 <xref:System.Collections.Concurrent.BlockingCollection%601> は <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A> メソッドを提供します。これにより、コンシューマーは `foreach` (Visual Basic では `For Each`) を使用し、コレクションが完了するまで (コレクションが空になり、それ以上項目が追加されなくなるまで)、項目を削除できます。 詳細については、「[方法: ForEach を使用して BlockingCollection 内の項目を削除する](../../../../docs/standard/collections/thread-safe/how-to-use-foreach-to-remove.md)」を参照してください。  
  
## <a name="using-many-blockingcollections-as-one"></a>多数の BlockingCollection を 1 つとして使用する  
 consumer が複数のコレクションから同時に項目を取得する必要のあるシナリオでは、<xref:System.Collections.Concurrent.BlockingCollection%601> の配列を作成し、<xref:System.Collections.Concurrent.BlockingCollection%601.TakeFromAny%2A> や <xref:System.Collections.Concurrent.BlockingCollection%601.AddToAny%2A> などの静的メソッドを使用できます。これらのメソッドでは、配列内の任意のコレクションを対象に追加または取得の操作を実行できます。 いずれかのコレクションがブロックしている場合、メソッドはすぐに別のコレクションを試します。これは、操作を実行できるコレクションが見つかるまで続行されます。 詳細については、「[方法: パイプラインでブロッキング コレクションの配列を使用する](../../../../docs/standard/collections/thread-safe/how-to-use-arrays-of-blockingcollections.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [コレクションとデータ構造体](../../../../docs/standard/collections/index.md)
- [スレッドセーフなコレクション](../../../../docs/standard/collections/thread-safe/index.md)
