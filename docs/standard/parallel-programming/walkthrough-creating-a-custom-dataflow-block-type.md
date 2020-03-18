---
title: 'チュートリアル: カスタム データフロー ブロックの型の作成'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Task Parallel Library, dataflows
- TPL dataflow library, creating custom dataflow blocks
- dataflow blocks, creating custom in TPL
ms.assetid: a6147146-0a6a-4d9b-ab0f-237b3c1ac691
ms.openlocfilehash: cb953952bbed90edd2db799e92d44ec9f062babf
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73139884"
---
# <a name="walkthrough-creating-a-custom-dataflow-block-type"></a>チュートリアル: カスタム データフロー ブロックの型の作成
TPL データ フロー ライブラリには、さまざまな機能が有効になるいくつかのデータフロー ブロックの種類が用意されていますが、カスタム ブロックの種類を作成することもできます。 このドキュメントでは、カスタム動作を実装するデータフロー ブロックの種類を作成する方法について説明します。  
  
## <a name="prerequisites"></a>前提条件  
 [データフロー](../../../docs/standard/parallel-programming/dataflow-task-parallel-library.md)に関するページを読んでからこのドキュメントをお読みください。  

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]
  
## <a name="defining-the-sliding-window-dataflow-block"></a>スライディング ウィンドウのデータフロー ブロックを定義する  
 入力値をバッファリングしてからスライディング ウィンドウ方法で出力することを要求するデータフロー アプリケーションを考えてみましょう。 たとえば、入力値が {0, 1 ,2, 3, 4, 5} でウィンドウ サイズが 3 の場合、スライディング ウィンドウのデータフロー ブロックは {0, 1, 2}、{1, 2, 3 }、{2, 3, 4}、および {3, 4, 5} という出力配列になります。 以下のセクションでは、このカスタム動作を実装するデータフロー ブロックの種類を作成する 2 つの方法について説明します。 最初の方法では、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.Encapsulate%2A> メソッドを使用して <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601> オブジェクトと <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601> オブジェクトの機能を 1 つの伝達子ブロックに結合します。 2 番目の方法では、<xref:System.Threading.Tasks.Dataflow.IPropagatorBlock%602> から派生し、既存の機能を組み合わせてカスタム動作を実行するクラスを定義します。  
  
## <a name="using-the-encapsulate-method-to-define-the-sliding-window-dataflow-block"></a>カプセル化メソッドを使用してスライディング ウィンドウのデータフロー ブロックを定義する  
 次の例では、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.Encapsulate%2A> メソッドを使用して、ターゲットとソースから伝達子ブロックを作成します。 伝達子ブロックを使用すると、ソース ブロックとターゲット ブロックはデータの受信側と送信側として機能します。  
  
 この手法は、カスタムのデータフロー機能が必要で、追加のメソッド、プロパティ、またはフィールドを提供する種類は必要ない場合に便利です。  
  
 [!code-csharp[TPLDataflow_SlidingWindowBlock#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_slidingwindowblock/cs/slidingwindowblock.cs#1)]
 [!code-vb[TPLDataflow_SlidingWindowBlock#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_slidingwindowblock/vb/slidingwindowblock.vb#1)]  
  
## <a name="deriving-from-ipropagatorblock-to-define-the-sliding-window-dataflow-block"></a>IPropagatorBlock から派生してスライディング ウィンドウのデータフロー ブロックを定義する  
 `SlidingWindowBlock` クラスの例を次に示します。 このクラスは <xref:System.Threading.Tasks.Dataflow.IPropagatorBlock%602> から派生しているので、データのソースとターゲットの両方として機能することができます。 前の例と同様に、`SlidingWindowBlock` クラスは既存のデータフロー ブロックの種類に基づいて構築されています。 ただし、`SlidingWindowBlock` クラスは、<xref:System.Threading.Tasks.Dataflow.ISourceBlock%601>、<xref:System.Threading.Tasks.Dataflow.ITargetBlock%601>、および <xref:System.Threading.Tasks.Dataflow.IDataflowBlock> インターフェイスで必要なメソッドも実装しています。 これらのメソッドはすべて、処理を定義済みデータフロー ブロックの種類のメンバーに転送します。 たとえば、`Post` メソッドは `m_target` オブジェクトでもある <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601> データ メンバーに処理を委任します。  
  
 この手法は、カスタムのデータフロー機能が必要で、追加のメソッド、プロパティ、またはフィールドを提供する種類も必要な場合に便利です。 たとえば、`SlidingWindowBlock` クラスは <xref:System.Threading.Tasks.Dataflow.IReceivableSourceBlock%601> メソッドと <xref:System.Threading.Tasks.Dataflow.IReceivableSourceBlock%601.TryReceive%2A> メソッドを提供できるように <xref:System.Threading.Tasks.Dataflow.IReceivableSourceBlock%601.TryReceiveAll%2A> からも派生しています。 `SlidingWindowBlock` クラスには、`WindowSize` プロパティがあることからもその拡張性がわかります。このプロパティを使用すると、スライディング ウィンドウ内の要素の数を取得できます。  
  
 [!code-csharp[TPLDataflow_SlidingWindowBlock#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_slidingwindowblock/cs/slidingwindowblock.cs#2)]
 [!code-vb[TPLDataflow_SlidingWindowBlock#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_slidingwindowblock/vb/slidingwindowblock.vb#2)]  
  
## <a name="the-complete-example"></a>完全な例  
 次の例は、このチュートリアルのコード全体を示しています。 また、ブロックに書き込み、そこから読み取り、結果をコンソールに出力するメソッドで、両方のスライディング ウィンドウ ブロックを使用する方法を示します。  
  
 [!code-csharp[TPLDataflow_SlidingWindowBlock#100](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_slidingwindowblock/cs/slidingwindowblock.cs#100)]
 [!code-vb[TPLDataflow_SlidingWindowBlock#100](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_slidingwindowblock/vb/slidingwindowblock.vb#100)]  
  
## <a name="see-also"></a>参照

- [データフロー](../../../docs/standard/parallel-programming/dataflow-task-parallel-library.md)
