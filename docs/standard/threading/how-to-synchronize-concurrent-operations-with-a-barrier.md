---
title: '方法: バリアを使用して同時実行操作を同期する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Barrier, how to use
ms.assetid: e1a253ff-e0fb-4df8-95ff-d01a90d4cb19
ms.openlocfilehash: 33098878764c2f8a8c1f83a122028da40b984243
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73137970"
---
# <a name="how-to-synchronize-concurrent-operations-with-a-barrier"></a>方法: バリアを使用して同時実行操作を同期する
次の例は、<xref:System.Threading.Barrier> を使用して同時実行タスクを同期する方法を示しています。  
  
## <a name="example"></a>例  
 次のプログラムの目的は、単語を再シャッフルするためにランダム化アルゴリズムを使用して、同じフェーズで 2 つのスレッドのそれぞれのソリューションの半分を検索するのに必要な反復 (またはフェーズ) の数をカウントすることです。 各スレッドで単語をシャッフルした後、バリアのフェーズ後操作で 2 つの結果が比較され、完全な文が正しい語順でレンダリングされているかどうかが確認されます。  
  
 [!code-csharp[CDS_Barrier#01](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_barrier/cs/barrier.cs#01)]
 [!code-vb[CDS_Barrier#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_barrier/vb/barrier_vb.vb#01)]  
  
 <xref:System.Threading.Barrier> はオブジェクトであり、すべてのタスクがバリアに到達するまで、並列操作の個々のタスクが続行されないようにします。 これは、並列操作を段階的に行う場合、および各フェーズでタスク間の同期が必要な場合に便利です。 この例では、2 つの操作フェーズがあります。 最初のフェーズでは、各タスクでバッファーのセクションにデータが読み込まれます。 各タスクでそのセクションへの読み込みが終了すると、タスクは続行する準備ができていることをバリアに通知してから、待機します。 すべてのタスクがバリアに通知した時点で、ブロックが解除され、2 番目のフェーズが開始されます。 2 番目のフェーズでは、各タスクがこれまでに生成されたすべてのデータにアクセスできる必要があるため、バリアが必要です。 バリアがないと、実行する最初のタスクで、他のタスクによってまだデータが読み込まれていないバッファーからの読み取りが試行される場合があります。 このように任意の数のフェーズを同期することができます。  
  
## <a name="see-also"></a>参照

- [並列プログラミングのデータ構造](../../../docs/standard/parallel-programming/data-structures-for-parallel-programming.md)
