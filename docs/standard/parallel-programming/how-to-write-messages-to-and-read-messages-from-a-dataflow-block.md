---
title: '方法: データフロー ブロックに対してメッセージの読み取りと書き込みを行う'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Task Parallel Library, dataflows
- TPL dataflow library, reading and writing messages
ms.assetid: 1a9bf078-aa82-46eb-b95a-f87237f028c5
ms.openlocfilehash: 58927803b741acf6c1964b35f6603e6901f9cbf1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73139284"
---
# <a name="how-to-write-messages-to-and-read-messages-from-a-dataflow-block"></a>方法: データフロー ブロックに対してメッセージの読み取りと書き込みを行う
このドキュメントでは、TPL データフロー ライブラリを使用して、データフロー ブロックとの間でメッセージを読み書きする方法を説明します。 TPL データフロー ライブラリには、データフロー ブロックへのメッセージの書き込みとデータフロー ブロックからのメッセージの読み取りのための、同期と非同期の両方のメソッドが用意されています。 このドキュメントでは、<xref:System.Threading.Tasks.Dataflow.BufferBlock%601?displayProperty=nameWithType> クラスを使用します。 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> クラスは、メッセージをバッファーし、メッセージ ソースとメッセージ ターゲットの両方として動作します。  

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="writing-to-and-reading-from-a-dataflow-block-synchronously"></a>データフロー ブロックへの同期書き込みとデータフロー ブロックからの同期読み取り  
 次の例では、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.Post%2A> メソッドを使用して <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> データフロー ブロックを書き込み、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.Receive%2A> メソッドを使用して同じオブジェクトから読み取ります。  
  
 [!code-csharp[TPLDataflow_ReadWrite#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_readwrite/cs/dataflowreadwrite.cs#2)]
 [!code-vb[TPLDataflow_ReadWrite#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_readwrite/vb/dataflowreadwrite.vb#2)]  
  
 また、次の例に示すように、<xref:System.Threading.Tasks.Dataflow.IReceivableSourceBlock%601.TryReceive%2A> メソッドを使用してデータフロー ブロックから読み取ることもできます。 <xref:System.Threading.Tasks.Dataflow.IReceivableSourceBlock%601.TryReceive%2A> メソッドは、現在のスレッドをブロックしないため、データを時々ポーリングする場合に便利です。  
  
 [!code-csharp[TPLDataflow_ReadWrite#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_readwrite/cs/dataflowreadwrite.cs#3)]
 [!code-vb[TPLDataflow_ReadWrite#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_readwrite/vb/dataflowreadwrite.vb#3)]  
  
 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Post%2A> メソッドは同期的に動作するため、前の例の <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> オブジェクトは、2 つ目のループがデータを読み取る前にすべてのデータを受け取ります。 次の例では、<xref:System.Threading.Tasks.Parallel.Invoke%2A> を使用して最初の例を拡張し、メッセージ ブロックからの読み取りとメッセージ ブロックへの書き込みを同時に行います。 <xref:System.Threading.Tasks.Parallel.Invoke%2A> は同時にアクションを実行するため、値が特定の順序で <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> オブジェクトに書き込まれることはありません。  
  
 [!code-csharp[TPLDataflow_ReadWrite#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_readwrite/cs/dataflowreadwrite.cs#4)]
 [!code-vb[TPLDataflow_ReadWrite#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_readwrite/vb/dataflowreadwrite.vb#4)]  
  
## <a name="writing-to-and-reading-from-a-dataflow-block-asynchronously"></a>データフロー ブロックへの非同期書き込みとデータフロー ブロックからの非同期読み取り  
 次の例では、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.SendAsync%2A> メソッドを使用して <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> オブジェクトに非同期的に書き込み、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.ReceiveAsync%2A> メソッドを使用して同じオブジェクトから非同期的に読み取ります。 この例では、[async](../../csharp/language-reference/keywords/async.md) 演算子と [await](../../csharp/language-reference/operators/await.md) 演算子 (Visual Basic では [Async](../../visual-basic/language-reference/modifiers/async.md) と [Await](../../visual-basic/language-reference/operators/await-operator.md)) を使用して、ターゲット ブロックへのデータの送信とターゲット ブロックからのデータの読み取りを非同期的に行います。 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.SendAsync%2A> メソッドは、メッセージを延期するデータフロー ブロックを有効にする必要があるときに便利です。 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.ReceiveAsync%2A> メソッドは、データが利用できるようになったときにデータを操作する必要がある場合に便利です。 メッセージ ブロック間でメッセージが伝達されるしくみの詳細については、[データフロー](../../../docs/standard/parallel-programming/dataflow-task-parallel-library.md)に関するページの「Message Passing (メッセージ パッシング)」のセクションを参照してください。  
  
 [!code-csharp[TPLDataflow_ReadWrite#5](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_readwrite/cs/dataflowreadwrite.cs#5)]
 [!code-vb[TPLDataflow_ReadWrite#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_readwrite/vb/dataflowreadwrite.vb#5)]  
  
## <a name="a-complete-example"></a>コード例全体  
 次の例は、このドキュメントのコード全体を示しています。  
  
 [!code-csharp[TPLDataflow_ReadWrite#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_readwrite/cs/dataflowreadwrite.cs#1)]
 [!code-vb[TPLDataflow_ReadWrite#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_readwrite/vb/dataflowreadwrite.vb#1)]  
  
## <a name="next-steps"></a>次の手順  
 この例は、メッセージ ブロックからの読み取りとメッセージ ブロックへの書き込みを直接行う方法を示しています。 データフロー ブロックを接続して、データフロー ブロックのリニア シーケンスである "*パイプライン*" を作成するか、またはデータフロー ブロックのグラフである "*ネットワーク*" を作成することもできます。 パイプラインまたはネットワークでは、データが使用可能になると、ソースはターゲットに非同期的にデータを伝達します。 基本的なデータフロー パイプラインを作成する例については、「[Walkthrough: Creating a Dataflow Pipeline (チュートリアル: データフロー パイプラインの作成)](../../../docs/standard/parallel-programming/walkthrough-creating-a-dataflow-pipeline.md)」を参照してください。 さらに複雑なデータフロー ネットワークを作成する例については、「[Walkthrough: Using Dataflow in a Windows Forms Application (チュートリアル: Windows フォーム アプリケーションでのデータフローの使用)](../../../docs/standard/parallel-programming/walkthrough-using-dataflow-in-a-windows-forms-application.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [データフロー](../../../docs/standard/parallel-programming/dataflow-task-parallel-library.md)
