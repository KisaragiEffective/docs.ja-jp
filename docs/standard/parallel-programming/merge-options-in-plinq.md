---
title: PLINQ のマージ オプション
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, merge options
ms.assetid: e8f7be3b-88de-4f33-ab14-dc008e76c1ba
ms.openlocfilehash: 18f233ac4c5afa63ec31e83d5fff8f0a57f9146f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74203999"
---
# <a name="merge-options-in-plinq"></a>PLINQ のマージ オプション
クエリが並列として実行される場合、PLINQ はソース シーケンスをパーティション分割し、複数のスレッドが同時に異なる部分 (通常は別個のスレッド) で動作できるようにします。 結果を 1 つのスレッドで、たとえば、`foreach` (Visual Basic では `For Each`) ループで使用する場合、すべてのスレッドからの結果を 1 つのシーケンスに再マージする必要があります。 PLINQ で実行されるマージの種類は、クエリに存在する演算子によって異なります。 たとえば、結果に新しい順序を適用する演算子は、すべてのスレッドのすべての要素をバッファリングする必要があります。 消費スレッド (アプリケーション ユーザーのものでもある) 観点から、完全にバッファリングされたクエリは、最初の結果が生成される前に非常に長い期間実行される可能性があります。 既定では、その他の演算子は部分的にバッファリングされ、結果はバッチ単位で生成されます。 既定では、1 つの演算子 <xref:System.Linq.ParallelEnumerable.ForAll%2A> がバッファリングされることはありません。 すべてのスレッドのすべての要素はすぐに生成されます。  
  
 以下の例に示すように、<xref:System.Linq.ParallelEnumerable.WithMergeOptions%2A> メソッドを使用することで、PLINQ に、実行するマージの種類を示すヒントを提供できます。  
  
 [!code-csharp[PLINQ#26](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#26)]
 [!code-vb[PLINQ#26](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#26)]  
  
 完全な例については、「[方法: PLINQ のマージ オプションを指定する](../../../docs/standard/parallel-programming/how-to-specify-merge-options-in-plinq.md)」を参照してください。  
  
 特定のクエリで要求されたオプションをサポートできない場合は、オプションが無視されるだけです。 ほとんどの場合、PLINQ クエリのマージ オプションを指定する必要はありません。 ただし、場合によっては、テストや測定を行うことで、クエリが既定以外のモードで最適に実行されることがわかることもあります。 このオプションは、より応答性の高いユーザー インターフェイスを提供するために、チャンク マージ演算子で結果を強制的にストリーミングする場合に一般的に使用されます。  
  
## <a name="parallelmergeoptions"></a>ParallelMergeOptions  
 <xref:System.Linq.ParallelMergeOptions> 列挙には、サポートされるクエリ形状について、結果が 1 つのスレッドで使用されるときにクエリの最終出力がどのように生成されるかを指定する以下のオプションが含まれます。  
  
- `Not Buffered`  
  
     <xref:System.Linq.ParallelMergeOptions.NotBuffered> オプションを指定すると、処理された各要素は、生成されるとすぐに各スレッドから返されます。 この動作は、出力の "ストリーミング" に似ています。 <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> 演算子がクエリに存在する場合、`NotBuffered` ではソース要素の順序が保持されます。 `NotBuffered` は使用可能になるとすぐに結果を生成し始めますが、それでも、すべての結果を生成するための合計時間は、他のマージ オプションのいずれかを使用する場合より長くなることがあります。  
  
- `Auto Buffered`  
  
     <xref:System.Linq.ParallelMergeOptions.AutoBuffered> オプションを指定すると、クエリで要素がバッファーに収集され、定期的にバッファー コンテンツがすべて一度に消費スレッドに譲渡されます。 これは、`NotBuffered` の "ストリーミング" 動作を使用する代わりに、"チャンク" 単位でソース データを生成する動作に似ています。 消費スレッドで最初の要素が使用できるようになるまで、`AutoBuffered` よりも `NotBuffered` のほうが長くかかる場合があります。 バッファーのサイズと正確な生成動作を構成することはできず、これらはクエリに関するさまざまな要因によって異なる場合があります。  
  
- `FullyBuffered`  
  
     <xref:System.Linq.ParallelMergeOptions.FullyBuffered> オプションを指定すると、要素が生成される前にクエリ全体の出力がバッファリングされます。 このオプションを使用すると、最初の要素が消費スレッドで使用可能になるまで時間がかかる場合がありますが、それでも他のオプションを使用する場合よりも速く完全な結果が生成される可能性があります。  
  
## <a name="query-operators-that-support-merge-options"></a>マージ オプションをサポートするクエリ演算子  
 次の表に、指定された制約に従って、すべてのマージ オプション モードをサポートする演算子をリストします。  
  
|演算子|制限事項|  
|--------------|------------------|  
|<xref:System.Linq.ParallelEnumerable.AsEnumerable%2A>|なし|  
|<xref:System.Linq.ParallelEnumerable.Cast%2A>|なし|  
|<xref:System.Linq.ParallelEnumerable.Concat%2A>|配列またはリスト ソースのみがある順序付けされていないクエリ。|  
|<xref:System.Linq.ParallelEnumerable.DefaultIfEmpty%2A>|なし|  
|<xref:System.Linq.ParallelEnumerable.OfType%2A>|なし|  
|<xref:System.Linq.ParallelEnumerable.Reverse%2A>|配列またはリスト ソースのみがある順序付けされていないクエリ。|  
|<xref:System.Linq.ParallelEnumerable.Select%2A>|なし|  
|<xref:System.Linq.ParallelEnumerable.SelectMany%2A>|なし|  
|<xref:System.Linq.ParallelEnumerable.Skip%2A>|なし|  
|<xref:System.Linq.ParallelEnumerable.Take%2A>|なし|  
|<xref:System.Linq.ParallelEnumerable.Where%2A>|なし|  
  
 他のすべての PLINQ クエリ演算子では、ユーザー指定のマージ オプションが無視される場合があります。 一部のクエリ演算子 (<xref:System.Linq.ParallelEnumerable.Reverse%2A> や <xref:System.Linq.ParallelEnumerable.OrderBy%2A> など) では、すべて生成されて並べ替えられるまで要素を譲渡できません。 したがって、<xref:System.Linq.ParallelMergeOptions> などの演算子も含むクエリで <xref:System.Linq.ParallelEnumerable.Reverse%2A> を使用すると、演算子でその結果が生成されるまでクエリではマージ動作が適用されません。  
  
 マージ オプションを処理するいくつかの演算子の機能は、ソース シーケンスの種類と、<xref:System.Linq.ParallelEnumerable.AsOrdered%2A> 演算子がクエリで既に使用されているかどうかによって異なります。 <xref:System.Linq.ParallelEnumerable.ForAll%2A> は常に <xref:System.Linq.ParallelMergeOptions.NotBuffered> であり、その要素をすぐに生成します。 <xref:System.Linq.ParallelEnumerable.OrderBy%2A> は常に<xref:System.Linq.ParallelMergeOptions.FullyBuffered> であり、生成の前にリスト全体を並べ替える必要があります。  
  
## <a name="see-also"></a>参照

- [Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)
- [方法: PLINQ のマージ オプションを指定する](../../../docs/standard/parallel-programming/how-to-specify-merge-options-in-plinq.md)
