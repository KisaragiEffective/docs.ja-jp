---
title: データの並列化 (タスク並列ライブラリ)
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallelism, data
ms.assetid: 3f05f33f-f1da-4b16-81c2-9ceff1bef449
ms.openlocfilehash: 72696a41cd3b71f47fdcf43e4ece70ebeb7d34d1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73123161"
---
# <a name="data-parallelism-task-parallel-library"></a>データの並列化 (タスク並列ライブラリ)
"*データの並列化*" とは、ソース コレクションまたは配列の要素に対して、同じ操作を同時に (つまり、並列で) 実行するシナリオを意味します。 データの並列化操作では、複数のスレッドが異なるセグメント上で同時に操作できるようにソース コレクションがパーティション分割されます。  
  
 タスク並列ライブラリ (TPL) は <xref:System.Threading.Tasks.Parallel?displayProperty=nameWithType> クラスによって、データの並列化をサポートします。 このクラスでは、[for](../../csharp/language-reference/keywords/for.md) ループおよび [foreach](../../csharp/language-reference/keywords/foreach-in.md) ループ (Visual Basic では `For` および `For Each`) をメソッド ベースで並列実装できます。 <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> ループまたは <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> ループに対するループのロジックは、順次ループを記述する場合と同等に記述します。 スレッドまたはキューの作業項目を作成する必要はありません。 基本のループでは、ロックを取得する必要はありません。 TPL では低水準の作業はすべて自動的に行われます。 <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> および <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> の使い方について詳しくは、ドキュメント「[Patterns for Parallel Programming: Understanding and Applying Parallel Patterns with the .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=19222)」(並列プログラミングのパターン: .NET Framework 4 の並列パターンの理解と適用) をダウンロードしてご覧ください。 次のコード例では、単純な `foreach` ループおよびそのループに相当する並列を示しています。  
  
> [!NOTE]
> ここでは、ラムダ式を使用して TPL でデリゲートを定義します。 C# または Visual Basic のラムダ式についての情報が必要な場合は、「[PLINQ および TPL のラムダ式](../../../docs/standard/parallel-programming/lambda-expressions-in-plinq-and-tpl.md)」を参照してください。  
  
 [!code-csharp[TPL#20](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl/cs/tpl.cs#20)]
 [!code-vb[TPL#20](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl/vb/tpl_vb.vb#20)]  
  
 並列ループを実行すると、TPL はデータ ソースをパーティション分割して、ループで複数の部分を同時に操作できるようにします。 背後では、タスク スケジューラが、システム リソースおよび作業負荷に基づいてタスクをパーティション分割します。 作業負荷のバランスが崩れると、可能な場合、スケジューラは複数のスレッドとプロセッサ間で作業を再配分します。  
  
> [!NOTE]
> 固有のカスタム パーティショナーまたはスケジューラを使うこともできます。 詳細については、「[PLINQ および TPL 用のカスタム パーティショナー](../../../docs/standard/parallel-programming/custom-partitioners-for-plinq-and-tpl.md)」および[TaskScheduler](xref:System.Threading.Tasks.TaskScheduler)に関するページを参照してください。  
  
 <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> メソッドおよび <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> メソッドの両方に、ループの実行を停止または中断させるオーバーロードが複数ある場合は、その他のスレッドのループ状態の監視、スレッド ローカルの状態の維持、スレッド ローカル オブジェクトの終了、コンカレンシーの程度の制御などを行います。 この機能を有効にするヘルパー要素には、<xref:System.Threading.Tasks.ParallelLoopState>、<xref:System.Threading.Tasks.ParallelOptions>、<xref:System.Threading.Tasks.ParallelLoopResult>、<xref:System.Threading.CancellationToken>、および <xref:System.Threading.CancellationTokenSource> があります。  
  
 詳細については、「[Patterns for Parallel Programming: Understanding and Applying Parallel Patterns with the .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=19222)」 (並列プログラミングのパターン: .NET Framework 4 での並列パターンの理解と適用) を参照してください。  
  
 PLINQ では、宣言構文またはクエリ形式の構文によるデータの並列化がサポートされています。 詳細については、「[Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)」を参照してください。  
  
## <a name="related-topics"></a>関連トピック  
  
|タイトル|[説明]|  
|-----------|-----------------|  
|[方法: 単純な Parallel.For ループを記述する](../../../docs/standard/parallel-programming/how-to-write-a-simple-parallel-for-loop.md)|任意の配列またはインデックス可能な <xref:System.Threading.Tasks.Parallel.For%2A> ソース コレクションに対して <xref:System.Collections.Generic.IEnumerable%601> ループを記述する方法について説明します。|  
|[方法: 単純な Parallel.ForEach ループを記述する](../../../docs/standard/parallel-programming/how-to-write-a-simple-parallel-foreach-loop.md)|任意の <xref:System.Threading.Tasks.Parallel.ForEach%2A> ソース コレクションに対して <xref:System.Collections.Generic.IEnumerable%601> ループを記述する方法について説明します。|  
|[方法: Parallel.For ループを停止または中断する](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dd460721(v=vs.100))|並列ループを停止するかまたは抜けて、そのアクションがすべてのスレッドに通知されるようにする方法について説明します。|  
|[方法: スレッド ローカル変数を使用する Parallel.For ループを記述する](../../../docs/standard/parallel-programming/how-to-write-a-parallel-for-loop-with-thread-local-variables.md)|各スレッドが他のスレッドからは見えないプライベート変数を維持する <xref:System.Threading.Tasks.Parallel.For%2A> ループを記述する方法と、ループが完了したときにすべてのスレッドの結果を同期する方法について説明します。|  
|[方法: パーティション ローカル変数を使用する Parallel.ForEach ループを記述する](../../../docs/standard/parallel-programming/how-to-write-a-parallel-foreach-loop-with-partition-local-variables.md)|各スレッドが他のスレッドからは見えないプライベート変数を維持する <xref:System.Threading.Tasks.Parallel.ForEach%2A> ループを記述する方法と、ループが完了したときにすべてのスレッドの結果を同期する方法について説明します。|  
|[方法: Parallel.For または ForEach ループを取り消す](../../../docs/standard/parallel-programming/how-to-cancel-a-parallel-for-or-foreach-loop.md)|<xref:System.Threading.CancellationToken?displayProperty=nameWithType> を使用して並列ループを取り消す方法について説明します。|  
|[方法: 小さいループ本体を高速化する](../../../docs/standard/parallel-programming/how-to-speed-up-small-loop-bodies.md)|ループの本体がごく小さい場合に実行速度を向上させる方法の 1 つを説明します。|  
|[タスク並列ライブラリ (TPL)](../../../docs/standard/parallel-programming/task-parallel-library-tpl.md)|タスク並列ライブラリの概要を示します。|  
|[並列プログラミング](../../../docs/standard/parallel-programming/index.md)|.NET Framework の並列プログラミングについて説明します。|  
  
## <a name="see-also"></a>参照

- [並列プログラミング](../../../docs/standard/parallel-programming/index.md)
