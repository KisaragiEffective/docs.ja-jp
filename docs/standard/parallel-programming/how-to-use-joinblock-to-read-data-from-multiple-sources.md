---
title: '方法: JoinBlock を使用して複数のソースからデータを読み込む'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Task Parallel Library, dataflows
- TPL dataflow library, joining blocks in
- dataflow blocks, joining in TPL
ms.assetid: e9c1ada4-ac57-4704-87cb-2f5117f8151d
ms.openlocfilehash: 66fd7ed7a98b8be8f88f65ecb52710a1e40af778
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73139744"
---
# <a name="how-to-use-joinblock-to-read-data-from-multiple-sources"></a>方法: JoinBlock を使用して複数のソースからデータを読み込む
このドキュメントでは、複数のソースからデータを使用できるときに <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> クラスを使用して操作を実行する方法について説明します。 また、最短一致モードを使い、複数の結合ブロックを有効にして、データ ソースをより効率的に共有する方法についても説明します。

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="example"></a>例  
 次の例では、3 つのリソースの種類 (`NetworkResource`、`FileResource`、および `MemoryResource`) を定義し、リソースが使用できるようになったときに操作を実行します。 この例では、最初の操作を実行するために `NetworkResource` と `MemoryResource` のペアが必要であり、2 番目の操作を実行するために `FileResource` と `MemoryResource` のペアが必要です。 必要なすべてのリソースを使用できるようになったときにこれらの操作を実行できるようにするために、この例では <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> クラスを使用します。 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> オブジェクトがすべてのソースからデータを受け取ると、そのデータをターゲット (この例では <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> オブジェクト) に伝達します。 どちらの <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> オブジェクトも `MemoryResource` オブジェクトの共有プールから読み取ります。  
  
 [!code-csharp[TPLDataflow_NonGreedyJoin#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_nongreedyjoin/cs/nongreedyjoin.cs#1)]
 [!code-vb[TPLDataflow_NonGreedyJoin#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_nongreedyjoin/vb/nongreedyjoin.vb#1)]  
  
 `MemoryResource` オブジェクトの共有プールを効率的に使用するために、この例では、<xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions> プロパティを <xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions.Greedy%2A> に設定した `False` オブジェクトを指定して、最短一致モードで動作する <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> オブジェクトを作成します。 最短一致の結合ブロックの場合、各ソースから使用できるようになるまで、すべての受信メッセージは延期されます。 延期されたメッセージのいずれかが別のブロックで受け入れられた場合、結合ブロックはプロセスを再開します。 最短一致モードでは、1 つ以上のソース ブロックを共有する結合ブロックが、他のブロックがデータを待機するときに転送を進めることができます。 この例では、`MemoryResource` オブジェクトが `memoryResources` プールに追加された場合、その 2 番目のデータ ソースを受け取る最初の結合ブロックが転送を進めることができます。 この例で、既定である最長一致モードを使用する場合、1 つの結合ブロックが `MemoryResource` オブジェクトを受け取り、2 番目のリソースを使用できるようになるまで待機することができます。 ただし、他の結合ブロックに使用できる 2 番目のデータ ソースがある場合は、`MemoryResource` オブジェクトが他の結合ブロックによって取得されているため、転送を進めることはできません。  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  
 最短一致の結合を使用すると、アプリケーションのデッドロックを防ぐのにも役立ちます。 ソフトウェア アプリケーションで、2 つ以上のプロセスがそれぞれリソースを確保し、別のプロセスがリソースを解放するのをお互いに待機すると、*デッドロック*が発生します。 2 つの <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> オブジェクトを定義するアプリケーションを考えてみましょう。 両方のオブジェクトは、それぞれ 2 つの共有ソース ブロックからデータを読み取ります。 最長一致モードでは、一方の結合ブロックが最初のソースから読み取り、もう一方の結合ブロックが 2 番目のソースから読み取る場合、どちらの結合ブロックも他方がリソースを解放するまで待機するため、アプリケーションがデッドロックする可能性があります。 最短一致モードの場合、各結合ブロックは、すべてのデータを使用できる場合にのみソースから読み取ります。そのため、デッドロックのリスクはなくなります。  
  
## <a name="see-also"></a>参照

- [データフロー](../../../docs/standard/parallel-programming/dataflow-task-parallel-library.md)
