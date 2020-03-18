---
title: '方法: パイプラインでブロッキング コレクションの配列を使用する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- thread-safe collections, blocking collections in pipeline
ms.assetid: a39c7ec3-3ad7-4f4d-8fe4-b3e9dbabe2ed
ms.openlocfilehash: 397c438bacd1cfed1613efef61e9d7266d55ea47
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75711260"
---
# <a name="how-to-use-arrays-of-blocking-collections-in-a-pipeline"></a>方法: パイプラインでブロッキング コレクションの配列を使用する
次の例は、<xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType> や <xref:System.Collections.Concurrent.BlockingCollection%601.TryAddToAny%2A> などの静的メソッドで <xref:System.Collections.Concurrent.BlockingCollection%601.TryTakeFromAny%2A> オブジェクトの配列を使用し、コンポーネント間の高速で柔軟なデータ転送を実装する方法を示しています。  
  
## <a name="example"></a>例  
 次の例では、各オブジェクトが同時にデータを入力コレクションから取得し、変換して、出力コレクションに渡す、基本的なパイプラインの実装を示します。  
  
 [!code-csharp[CDS_BlockingCollection#07](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/example07.cs#07)]
 [!code-vb[CDS_BlockingCollection#07](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/bcpipeline.vb#07)]  
  
## <a name="see-also"></a>参照

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [スレッドセーフなコレクション](../../../../docs/standard/collections/thread-safe/index.md)
