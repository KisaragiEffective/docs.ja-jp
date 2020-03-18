---
title: '方法: 動的パーティションを実装する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, how to create a dynamic partitioner
ms.assetid: c875ad12-a161-43e6-ad1c-3d6927c536a7
ms.openlocfilehash: 3970566b4e3f51ce538c328d4e69b20ec22ec09b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73091414"
---
# <a name="how-to-implement-dynamic-partitions"></a>方法: 動的パーティションを実装する

次の例は、特定のオーバーロード <xref:System.Collections.Concurrent.OrderablePartitioner%601?displayProperty=nameWithType> と PLINQ から動的なパーティション分割を実装するカスタム <xref:System.Threading.Tasks.Parallel.ForEach%2A> を実装する方法を示します。  
  
## <a name="example"></a>例

列挙子でパーティションが <xref:System.Collections.IEnumerator.MoveNext%2A> を呼び出すたびに、列挙子はパーティションに 1 つのリスト要素を提供します。 PLINQ と <xref:System.Threading.Tasks.Parallel.ForEach%2A> では、パーティションは <xref:System.Threading.Tasks.Task> インスタンスです。 要求は、複数のスレッドで同時に発生しているため、現在のインデックスへのアクセスは同期されます。  

[!code-csharp[TPL_Partitioners#04](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_partitioners/cs/partitioner02.cs#OrderableListPartitioner)]
[!code-vb[TPL_Partitioners#04](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_partitioners/vb/dynamicpartitioner.vb#04)]  

これは、各チャンクが 1 つの要素で構成されるチャンク パーティション分割の例です。 一度に複数の要素を提供することにより、ロックの競合を減らし、理論的により高速なパフォーマンスを実現することができます。 ただし、チャンクが大きい場合、すべての作業が完了するまで、すべてのスレッドをビジーにするために、ある時点で追加の負荷分散ロジックが必要な場合があります。  
  
## <a name="see-also"></a>参照

* [PLINQ および TPL 用のカスタム パーティショナー](../../../docs/standard/parallel-programming/custom-partitioners-for-plinq-and-tpl.md)
* [方法: 静的パーティション分割用にパーティショナーを実装する](../../../docs/standard/parallel-programming/how-to-implement-a-partitioner-for-static-partitioning.md)
