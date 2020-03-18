---
title: '方法: BlockingCollection の項目を個別に追加および取得する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- thread-safe collections, blocking dictionary
ms.assetid: 38f2f3d8-15e5-4bf4-9c83-2b5b6f22bad1
ms.openlocfilehash: 3f4270d2ec71421bad8974a3e5cd8f1d65db3b74
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75711299"
---
# <a name="how-to-add-and-take-items-individually-from-a-blockingcollection"></a>方法: BlockingCollection の項目を個別に追加および取得する
この例では、ブロッキングと非ブロッキングの 2 つの方法で <xref:System.Collections.Concurrent.BlockingCollection%601> の項目を追加、削除する方法を示します。 <xref:System.Collections.Concurrent.BlockingCollection%601> の詳細については、「[BlockingCollection の概要](../../../../docs/standard/collections/thread-safe/blockingcollection-overview.md)」を参照してください。  
  
 空になって追加する要素がなくなるまで <xref:System.Collections.Concurrent.BlockingCollection%601> を列挙する方法の例については、「[方法: ForEach を使用して BlockingCollection 内の項目を削除する](../../../../docs/standard/collections/thread-safe/how-to-use-foreach-to-remove.md)」を参照してください。
  
## <a name="example"></a>例  
 この最初の例では、コレクションが一時的に空 (取得時) または最大容量 (追加時) になるか、指定されたタイムアウト期間が経過したときに、操作をブロックするように、項目を追加および取得する方法を示します。 最大容量のブロックは、BlockingCollection が、コンストラクターで指定された最大容量で作成された場合にのみ有効になることに注意してください。  
  
 [!code-csharp[CDS_BlockingCollection#01](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/example01.cs#01)]
 [!code-vb[CDS_BlockingCollection#01](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/simpleblocking.vb#01)]  
  
## <a name="example"></a>例  
 この 2 番目の例では、操作をブロックしないように、項目を追加および取得する方法を示します。 項目が存在しない場合、サイズ制限のあるコレクションの最大容量に達した場合、またはタイムアウト期間が経過した場合、<xref:System.Collections.Concurrent.BlockingCollection%601.TryAdd%2A> または <xref:System.Collections.Concurrent.BlockingCollection%601.TryTake%2A> 操作は false を返します。 これによりスレッドはしばらくの間、他の何らかの有益な処理を行い、後からもう一度新しいアイテムを取得するか、以前に追加できなかった項目を追加しようとします。 このプログラムはまた、<xref:System.Collections.Concurrent.BlockingCollection%601> へのアクセス時にキャンセルを実装する方法も示しています。  
  
 [!code-csharp[CDS_BlockingCollection#02](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/example02.cs#02)]
 [!code-vb[CDS_BlockingCollection#02](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/nonblockingbc.vb#02)]  
  
## <a name="see-also"></a>参照

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [BlockingCollection の概要](../../../../docs/standard/collections/thread-safe/blockingcollection-overview.md)
