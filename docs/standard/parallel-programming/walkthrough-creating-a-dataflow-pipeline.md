---
title: 'チュートリアル: データフロー パイプラインの作成'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- dataflow pipelines, creating with TPL
- Task Parallel Library, dataflows
- TPL dataflow library, creating dataflow pipeline
ms.assetid: 69308f82-aa22-4ac5-833d-e748533b58e8
ms.openlocfilehash: 284be7789b6411055a6421fd07cc1b0605f6ea0c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73139871"
---
# <a name="walkthrough-creating-a-dataflow-pipeline"></a>チュートリアル: データフロー パイプラインの作成
<xref:System.Threading.Tasks.Dataflow.DataflowBlock.Receive%2A?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.ReceiveAsync%2A?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.TryReceive%2A?displayProperty=nameWithType> の各メソッドを使用してソース ブロックからメッセージを受信できますが、メッセージ ブロックを接続して*データフロー パイプライン*を形成することもできます。 データフロー パイプラインは一連の*データフロー ブロック*で構成されます。それぞれが特定のタスクを実行し、全体として 1 つの大きな目標を達成します。 データフロー パイプラインのすべてのデータフロー ブロックは、他のデータフロー ブロックからメッセージを受け取ったときに処理を実行します。 これは、自動車製造の組み立てラインに例えることができます。 各車両が組み立てラインを通過する際、あるステーションではフレームを組み立て、次のステーションではエンジンを設置するなどです。 組み立てラインでは、複数の車両を同時に組み立てることができるため、一度に車両全体を組み立てるよりスループットが向上します。

 このドキュメントで説明するデータフロー パイプラインでは、『*The Iliad of Homer*』(ホメロスのイリアス) という英語の書籍を Web サイトからダウンロードしてから、テキストの中の単語ごとに、その単語の文字を逆に並べた単語を探します。 このドキュメントでは、データフロー パイプラインは、次の手順で構成しています。  
  
1. パイプラインに参加しているデータフロー ブロックを作成します。  
  
2. 各データフロー ブロックを、パイプラインの次のブロックに接続します。 各ブロックは、入力として、パイプラインの前のブロックの出力を受信します。  
  
3. 各データフロー ブロックでは、前のブロックの終了後、次のブロックで完了状態に設定する継続タスクを作成します。  
  
4. データをパイプラインの先頭に送信します。  
  
5. パイプラインの先頭を完了としてマークします。  
  
6. パイプラインのすべての作業が完了するまで待機します。  
  
## <a name="prerequisites"></a>前提条件  
 このチュートリアルを開始する前に、「[Dataflow (データフロー)](../../../docs/standard/parallel-programming/dataflow-task-parallel-library.md)」をお読みください。  
  
## <a name="creating-a-console-application"></a>コンソール アプリケーションの作成  
 Visual Studio で、Visual C# または Visual Basic のコンソール アプリケーション プロジェクトを作成します。 System.Threading.Tasks.Dataflow NuGet パッケージをインストールします。

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

 次のコードをプロジェクトに追加して、基本のアプリケーションを作成します。  
  
 [!code-csharp[TPLDataflow_Palindromes#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#2)]
 [!code-vb[TPLDataflow_Palindromes#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromesemptymain.vb#2)]  
  
## <a name="creating-the-dataflow-blocks"></a>データフロー ブロックの作成  
 次のコードを `Main` メソッドに追加して、パイプラインに参加するデータフロー ブロックを作成します。 次の表は、パイプラインの各メンバーの役割をまとめたものです。  
  
 [!code-csharp[TPLDataflow_Palindromes#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#3)]
 [!code-vb[TPLDataflow_Palindromes#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#3)]  
  
|メンバー|[種類]|[説明]|  
|------------|----------|-----------------|  
|`downloadString`|<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>|Web から書籍のテキストをダウンロードします。|  
|`createWordList`|<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>|書籍のテキストを単語の配列に区切ります。|  
|`filterWordList`|<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>|短い単語や重複するものを配列から削除します。|  
|`findReversedWords`|<xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602>|フィルター処理した単語の配列のコレクションにある全単語のうち、逆方向の単語の配列も発生するものを検索します。|  
|`printReversedWords`|<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>|単語と、関連する逆方向の単語をコンソールに表示します。|  
  
 この例のデータフロー パイプラインの複数の手順を 1 つの手順に結合することができますが、この例では、大規模なタスクを実行するために複数の独立したデータ フロー タスクを構成する概念を示します。 この例では、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602> を使用して、パイプラインの各メンバーが入力データで操作を実行し、結果をパイプラインの次の手順に送ることができるようにします。 パイプラインの `findReversedWords` のメンバーは、各入力に複数の独立した出力を生成するため、<xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> オブジェクトになります。 パイプラインの末尾の `printReversedWords` は <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> オブジェクトです。このメンバーは入力に対して操作を実行しますが、結果を生成しないからです。  
  
## <a name="forming-the-pipeline"></a>パイプラインの形成  
 各ブロックをパイプラインの次のブロックに接続するには、次のコードを追加します。  
  
 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.LinkTo%2A> メソッドを呼び出してソース データフロー ブロックをターゲット データフロー ブロックに接続すると、データが使用可能になったときにソース データフロー ブロックがターゲット ブロックにデータを反映させます。 <xref:System.Threading.Tasks.Dataflow.DataflowLinkOptions> も指定して <xref:System.Threading.Tasks.Dataflow.DataflowLinkOptions.PropagateCompletion> を true に設定した場合は、パイプライン内のブロックの 1 つが成功か不成功かを問わず完了すると、そのパイプライン内の次のブロックも完了となります。
  
 [!code-csharp[TPLDataflow_Palindromes#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#4)]
 [!code-vb[TPLDataflow_Palindromes#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#4)]  
  
## <a name="posting-data-to-the-pipeline"></a>パイプラインへのデータの送信  
 『*The Iliad of Homer*』(ホメロスのイリアス) という書籍の URL をデータフロー パイプラインの先頭に送信するために、次のコードを追加します。  
  
 [!code-csharp[TPLDataflow_Palindromes#6](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#6)]
 [!code-vb[TPLDataflow_Palindromes#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#6)]  
  
 この例では、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.Post%2A?displayProperty=nameWithType> を使用して、同期的にパイプラインの先頭にデータを送信します。 データフロー ノードにデータを非同期的に送信する必要がある場合は、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.SendAsync%2A?displayProperty=nameWithType> メソッドを使用します。  
  
## <a name="completing-pipeline-activity"></a>パイプラインのアクティビティの完了  
 パイプラインの先頭を完了済みとしてマークするには、次のコードを追加します。 パイプラインの先頭は、バッファ内のメッセージをすべて処理した後に自身の完了を伝達します。
  
 [!code-csharp[TPLDataflow_Palindromes#7](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#7)]
 [!code-vb[TPLDataflow_Palindromes#7](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#7)]  
  
 この例では、処理するために 1 つの URL をデータフロー パイプラインを介して送信します。 パイプラインを介して複数の入力を送信する場合は、すべての入力を送信した後に <xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Complete%2A?displayProperty=nameWithType> メソッドを呼び出します。 アプリケーションに、適切に定義されたポイントがない (データが使用できなくなっているか、アプリケーションがパイプラインの終了を待つ必要がない) 場合は、この手順を省略できます。  
  
## <a name="waiting-for-the-pipeline-to-finish"></a>パイプラインの終了までの待機  
 パイプラインの終了まで待機するには、次のコードを追加します。 全体的な操作が終了となるのは、パイプラインの末尾が終了したときです。  
  
 [!code-csharp[TPLDataflow_Palindromes#8](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#8)]
 [!code-vb[TPLDataflow_Palindromes#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#8)]  
  
 データフローの完了は、いずれかのスレッドから、または複数のスレッドから同時に待つことができます。  
  
## <a name="the-complete-example"></a>完全な例  
 次の例は、このチュートリアルのコード全体を示しています。  
  
 [!code-csharp[TPLDataflow_Palindromes#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#1)]
 [!code-vb[TPLDataflow_Palindromes#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#1)]  
  
## <a name="next-steps"></a>次の手順  
 この例では、データフロー パイプラインを介して処理するために 1 つの URL を送信しています。 パイプラインを介して複数の入力値を送信する場合は、自動車工場で部品が通過する方法に似た並列処理の形式をアプリケーションに導入することができます。 パイプラインの最初のメンバーが結果を 2 番目のメンバーに送信する場合、最初のメンバーは、2 番目のメンバーが最初のメンバーの結果を処理するときに、並行して別のアイテムを処理できます。  
  
 データフロー パイプラインを使用して実現される並列処理は、*粒度の粗い並列処理*と呼ばれます。一般的に、少数の大きなタスクで構成されているためです。 *粒度の細かい並列処理*を使用する、つまりデータフロー パイプラインのタスクを小さく、実行時間を短くすることもできます。 この例では、パイプラインの `findReversedWords` メンバーが [PLINQ](parallel-linq-plinq.md) を使用して作業リストの複数のアイテムを並列処理します。 粒度の粗いパイプラインで粒度の細かい並列処理を行うと、全体のスループットが向上します。  
  
 また、1 つのソース データフロー ブロックを複数のターゲット ブロックに接続して*データフロー ネットワーク*を作成することもできます。 オーバー ロードされたバージョンの <xref:System.Threading.Tasks.Dataflow.DataflowBlock.LinkTo%2A> メソッドは、ターゲット ブロックがその値に基づいて各メッセージを受け入れるかどうかを定義する <xref:System.Predicate%601> オブジェクトを受け取ります。 ソースとして動作するほとんどのデータフロー ブロック型では、接続されたすべてのターゲット ブロックにメッセージを提供します。これは、いずれかのブロックがそのメッセージを受け入れるまで、ターゲット ブロックが接続された順序で行われます。 このフィルター機構を使用すると、特定のデータはあるパスを通り、その他のデータは別のパスを通るように仕向ける、接続されたデータフロー ブロックの体系を作成することができます。 フィルター処理を使用してデータフロー ネットワークを作成する例については、「[チュートリアル: Windows フォーム アプリケーションでデータ フローを使用する](../../../docs/standard/parallel-programming/walkthrough-using-dataflow-in-a-windows-forms-application.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [データフロー](../../../docs/standard/parallel-programming/dataflow-task-parallel-library.md)
