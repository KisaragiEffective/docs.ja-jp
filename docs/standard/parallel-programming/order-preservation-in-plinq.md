---
title: PLINQ における順序維持
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, order preservation
ms.assetid: 10d202bc-19e1-4b5c-bbf1-9a977322a9ca
ms.openlocfilehash: 5b067fa277816e6105d37047c6c4996a4cbb9b5a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73138217"
---
# <a name="order-preservation-in-plinq"></a>PLINQ における順序維持
PLINQ では、正確性を維持しながらパフォーマンスを最大にすることが重要です。 クエリをできるだけ速く実行する一方で、正確な結果を生成する必要があります。 正確性のために、ソース シーケンスの順序の維持が必要な場合がありますが、順序付けには負荷がかかります。 したがって、既定では、PLINQ はソース シーケンスの順序を維持しません。 この点で、PLINQ は [!INCLUDE[vbtecdlinq](../../../includes/vbtecdlinq-md.md)] と似ていますが、順序を維持する LINQ to Objects とは異なります。  
  
 既定の動作をオーバーライドするには、ソース シーケンス上で <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> 演算子を使用して、順序の維持を有効にします。 その後、<xref:System.Linq.ParallelEnumerable.AsUnordered%2A> メソッドを使用して、クエリでの順序の維持を無効にできます。 どちらの方法でも、クエリを並列実行するか順次実行するかを決定するヒューリスティックに基づいてクエリが処理されます。 詳細については、「[Understanding Speedup in PLINQ (PLINQ での高速化について)](../../../docs/standard/parallel-programming/understanding-speedup-in-plinq.md)」を参照してください。  
  
 次の例では、結果を順序付けず、条件に一致するすべての要素をフィルター処理する、順序なしの並列クエリを示しています。  
  
 [!code-csharp[PLINQ#8](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#8)]
 [!code-vb[PLINQ#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#8)]  
  
 このクエリでは、ソース シーケンスで条件を満たす最初の 1000 都市が生成されるとは限らず、条件を満たす一連の 1000 都市が生成されます。 PLINQ クエリ演算子は、同時実行タスクとして処理される複数のサブシーケンスにソース シーケンスをパーティション分割します。 順序の維持が指定されていない場合、パーティションごとの結果はクエリの次のステージに任意の順序で渡されます。 また、パーティションでは、残りの要素の処理が続行される前に、結果のサブセットが生成される場合があります。 結果の順序は毎回異なることがあります。 この動作は、オペレーティング システムによるスレッドのスケジュール方法に依存するため、アプリケーションでは制御できません。  
  
 次の例では、ソース シーケンス上で <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> 演算子を使用して、既定の動作をオーバーライドしています。 この例では <xref:System.Linq.ParallelEnumerable.Take%2A> メソッドにより、ソース シーケンスで条件を満たす最初の 1000 都市が返されます。  
  
 [!code-csharp[PLINQ#9](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#9)]
 [!code-vb[PLINQ#9](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#9)]  
  
 ただし、このクエリは順序なしのクエリと同じ速度では実行されません。パーティション全体とマージ時刻で元の順序を追跡し、順序が一貫していることを確認する必要があるためです。 したがって、<xref:System.Linq.ParallelEnumerable.AsOrdered%2A> は、必要な場合にのみ、クエリの該当部分に限って使用することをお勧めします。 順序を維持する必要がなくなったら、<xref:System.Linq.ParallelEnumerable.AsUnordered%2A> を使用して無効にします。 2 つのクエリを組み合わせてこの処理を行う例を次に示します。  
  
 [!code-csharp[PLINQ#6](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#6)]
 [!code-vb[PLINQ#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#6)]  
  
 PLINQ は、残りのクエリに対し、順序を強制する演算子によって生成されるシーケンスの順序を維持することに注意してください。 つまり、<xref:System.Linq.ParallelEnumerable.OrderBy%2A> や <xref:System.Linq.ParallelEnumerable.ThenBy%2A> の演算子に続いて <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> が呼び出されるのと同じように処理されます。  
  
## <a name="query-operators-and-ordering"></a>クエリ演算子と順序付け  
 次のクエリ演算子は、クエリ内のすべての後続演算子で順序を維持するか、または <xref:System.Linq.ParallelEnumerable.AsUnordered%2A> が呼び出されるまで順序を維持します。  
  
- <xref:System.Linq.ParallelEnumerable.OrderBy%2A>  
  
- <xref:System.Linq.ParallelEnumerable.OrderByDescending%2A>  
  
- <xref:System.Linq.ParallelEnumerable.ThenBy%2A>  
  
- <xref:System.Linq.ParallelEnumerable.ThenByDescending%2A>  
  
 次の PLINQ クエリ演算子では、正確な結果を生成するために順序ありのソース シーケンスが必要となる場合があります。  
  
- <xref:System.Linq.ParallelEnumerable.Reverse%2A>  
  
- <xref:System.Linq.ParallelEnumerable.SequenceEqual%2A>  
  
- <xref:System.Linq.ParallelEnumerable.TakeWhile%2A>  
  
- <xref:System.Linq.ParallelEnumerable.SkipWhile%2A>  
  
- <xref:System.Linq.ParallelEnumerable.Zip%2A>  
  
 PLINQ クエリ演算子の中には、ソース シーケンスが順序ありか順序なしかによって動作が異なるものがあります。 次の表に、これらの演算子の一覧を示します。  
  
|演算子|ソース シーケンスが順序ありの場合の結果|ソース シーケンスが順序なしの場合の結果|  
|--------------|------------------------------------------------|--------------------------------------------------|  
|<xref:System.Linq.ParallelEnumerable.Aggregate%2A>|非結合演算子または非可換演算子の場合は非確定の出力|非結合演算子または非可換演算子の場合は非確定の出力|  
|<xref:System.Linq.ParallelEnumerable.All%2A>|適用なし|適用なし|  
|<xref:System.Linq.ParallelEnumerable.Any%2A>|適用なし|適用なし|  
|<xref:System.Linq.ParallelEnumerable.AsEnumerable%2A>|適用なし|適用なし|  
|<xref:System.Linq.ParallelEnumerable.Average%2A>|非結合演算子または非可換演算子の場合は非確定の出力|非結合演算子または非可換演算子の場合は非確定の出力|  
|<xref:System.Linq.ParallelEnumerable.Cast%2A>|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.Concat%2A>|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.Count%2A>|適用なし|適用なし|  
|<xref:System.Linq.ParallelEnumerable.DefaultIfEmpty%2A>|適用なし|適用なし|  
|<xref:System.Linq.ParallelEnumerable.Distinct%2A>|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.ElementAt%2A>|指定された要素を返す|任意の要素|  
|<xref:System.Linq.ParallelEnumerable.ElementAtOrDefault%2A>|指定された要素を返す|任意の要素|  
|<xref:System.Linq.ParallelEnumerable.Except%2A>|順序なしの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.First%2A>|指定された要素を返す|任意の要素|  
|<xref:System.Linq.ParallelEnumerable.FirstOrDefault%2A>|指定された要素を返す|任意の要素|  
|<xref:System.Linq.ParallelEnumerable.ForAll%2A>|非確定的に並列実行|非確定的に並列実行|  
|<xref:System.Linq.ParallelEnumerable.GroupBy%2A>|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.GroupJoin%2A>|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.Intersect%2A>|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.Join%2A>|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.Last%2A>|指定された要素を返す|任意の要素|  
|<xref:System.Linq.ParallelEnumerable.LastOrDefault%2A>|指定された要素を返す|任意の要素|  
|<xref:System.Linq.ParallelEnumerable.LongCount%2A>|適用なし|適用なし|  
|<xref:System.Linq.ParallelEnumerable.Min%2A>|適用なし|適用なし|  
|<xref:System.Linq.ParallelEnumerable.OrderBy%2A>|シーケンスを並べ替え|新規に順序付けられたセクションを開始|  
|<xref:System.Linq.ParallelEnumerable.OrderByDescending%2A>|シーケンスを並べ替え|新規に順序付けられたセクションを開始|  
|<xref:System.Linq.ParallelEnumerable.Range%2A>|該当なし (<xref:System.Linq.ParallelEnumerable.AsParallel%2A> の既定と同じ)|適用なし|  
|<xref:System.Linq.ParallelEnumerable.Repeat%2A>|該当なし (<xref:System.Linq.ParallelEnumerable.AsParallel%2A> の既定と同じ)|適用なし|  
|<xref:System.Linq.ParallelEnumerable.Reverse%2A>|逆方向|処理を行わない|  
|<xref:System.Linq.ParallelEnumerable.Select%2A>|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.Select%2A> (インデックス付き)|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.SelectMany%2A>|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.SelectMany%2A> (インデックス付き)|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.SequenceEqual%2A>|順序ありの比較|順序なしの比較|  
|<xref:System.Linq.ParallelEnumerable.Single%2A>|適用なし|適用なし|  
|<xref:System.Linq.ParallelEnumerable.SingleOrDefault%2A>|適用なし|適用なし|  
|<xref:System.Linq.ParallelEnumerable.Skip%2A>|最初の *n* 要素をスキップ|任意の *n* 要素をスキップ|  
|<xref:System.Linq.ParallelEnumerable.SkipWhile%2A>|順序ありの結果|非確定。 現在の任意の順序で SkipWhile を実行|  
|<xref:System.Linq.ParallelEnumerable.Sum%2A>|非結合演算子または非可換演算子の場合は非確定の出力|非結合演算子または非可換演算子の場合は非確定の出力|  
|<xref:System.Linq.ParallelEnumerable.Take%2A>|最初の `n` 要素を取得|`n` 要素を取得|  
|<xref:System.Linq.ParallelEnumerable.TakeWhile%2A>|順序ありの結果|非確定。 現在の任意の順序で TakeWhile を実行|  
|<xref:System.Linq.ParallelEnumerable.ThenBy%2A>|`OrderBy` を補足|`OrderBy` を補足|  
|<xref:System.Linq.ParallelEnumerable.ThenByDescending%2A>|`OrderBy` を補足|`OrderBy` を補足|  
|<xref:System.Linq.ParallelEnumerable.ToArray%2A>|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.ToDictionary%2A>|適用なし|適用なし|  
|<xref:System.Linq.ParallelEnumerable.ToList%2A>|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.ToLookup%2A>|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.Union%2A>|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.Where%2A>|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.Where%2A> (インデックス付き)|順序ありの結果|順序なしの結果|  
|<xref:System.Linq.ParallelEnumerable.Zip%2A>|順序ありの結果|順序なしの結果|  
  
 順序なしの結果はアクティブにシャッフルされるわけではありません。適用される特別な順序ロジックがないだけです。 順序なしのクエリでソース シーケンスの順序が保持される場合もあります。 インデックス付きの Select 演算子を使用するクエリの場合、PLINQ ではインデックスが増加する順に出力要素が出力されることは保証しますが、どのインデックスがどの要素に割り当てられるかについては一切保証しません。  
  
## <a name="see-also"></a>参照

- [Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)
- [並列プログラミング](../../../docs/standard/parallel-programming/index.md)
