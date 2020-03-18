---
title: データとタスクの並列化における注意点
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallel programming, pitfalls
ms.assetid: 1e357177-e699-4b8f-9e49-56d3513ed128
ms.openlocfilehash: ff6ac9e8c41ee203ae72e1b28c088f462ddf6a54
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73140019"
---
# <a name="potential-pitfalls-in-data-and-task-parallelism"></a>データとタスクの並列化における注意点
<xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> および <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> を使用すると、多くの場合、通常の順次ループよりもパフォーマンスが大幅に向上します。 ただし、ループを並列化すると複雑になるため、逐次コードでは一般的でない、またはまったく発生しない問題の原因になる可能性があります。 このトピックでは、並列ループを記述するときに回避すべきプラクティスをいくつか説明します。  
  
## <a name="do-not-assume-that-parallel-is-always-faster"></a>並列処理が常に高速であると思い込まない  
 並列ループは、場合によっては対応する順次処理よりも時間がかかる可能性があります。 基本的な経験則では、イテレーションが少なく、高速ユーザー デリゲートを使用する並列ループの速度が大幅に向上することはほとんどありません。 ただし、パフォーマンスには多くの要因が関係するため、常に実際の結果を測定することをお勧めします。  
  
## <a name="avoid-writing-to-shared-memory-locations"></a>共有メモリの位置への書き込みを回避する  
 逐次コードでは、静的変数またはクラス フィールドから読み取ることや、これらの場所に書き込むことはよくあります。 ただし、複数のスレッドからこのような変数に同時にアクセスしているときは、著しい競合状態になる場合がよくあります。 ロックを使用して変数へのアクセスを同期できる場合でも、同期のコストでパフォーマンスが低下する可能性があります。 そのため、並列ループにおける共有状態へのアクセスは、可能な限り回避するか、少なくとも制限することをお勧めします。 この場合の最適な方法は、ループの実行中に <xref:System.Threading.ThreadLocal%601?displayProperty=nameWithType> 変数を使用してスレッド ローカルの状態を格納する、<xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> および <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> のオーバーロードを使用することです。 詳細については、[スレッド ローカル変数を使用する Parallel.For ループを記述する](../../../docs/standard/parallel-programming/how-to-write-a-parallel-for-loop-with-thread-local-variables.md)」および「[方法:パーティション ローカル変数を使用する Parallel.ForEach ループを記述する](../../../docs/standard/parallel-programming/how-to-write-a-parallel-foreach-loop-with-partition-local-variables.md)」をご覧ください。  
  
## <a name="avoid-over-parallelization"></a>過剰な並列化を回避する  
 並列ループを使用することで、ソース コレクションのパーティション分割とワーカー スレッドの同期によるオーバヘッド コストが発生します。 並列化の利点は、コンピューター上のプロセッサ数によってさらに制限されます。 1 つのプロセッサで複数の計算主体のスレッドを実行しても、高速化は実現しません。 そのため、ループを過剰に並列処理しないように注意する必要があります。  
  
 過剰な並列化が発生する可能性が特に高い一般的な状況が、入れ子になったループ内です。 ほとんどの場合、次の条件のうち 1 つ以上該当しない限り、外側のループのみを並列化することをお勧めします。  
  
- 内側のループが非常に長いことが判明している。  
  
- 各順序で高負荷の計算を実行している (この例に示す操作の負荷は大きくありません)。  
  
- 対象システムに、`cust.Orders` でクエリを並列化することで生成されるスレッドの数を十分に処理できるプロセッサが存在している。  
  
 どの場合も、最適なクエリの形式を決定する最善の方法は、テストおよび測定することです。  
  
## <a name="avoid-calls-to-non-thread-safe-methods"></a>スレッド セーフでないメソッドの呼び出しを回避する  
 並列ループからスレッド セーフでないインスタンス メソッドに書き込むと、プログラムで検出されない可能性のあるデータ破損が起こる可能性があります。 例外が発生する可能性もあります。 次の例では、複数のスレッドが、クラスでサポートされていない <xref:System.IO.FileStream.WriteByte%2A?displayProperty=nameWithType> メソッドの同時呼び出しを試みます。  
  
 [!code-csharp[TPL_Pitfalls#04](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#04)]
 [!code-vb[TPL_Pitfalls#04](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#04)]  
  
## <a name="limit-calls-to-thread-safe-methods"></a>スレッド セーフなメソッドの呼び出しを制限する  
 .NET Framework のほとんどの静的メソッドはスレッド セーフであり、複数のスレッドから同時に呼び出すことができます。 ただし、このような場合でも、関連する同期によっては、クエリの処理速度が大幅に低下する可能性があります。  
  
> [!NOTE]
> これは、クエリに <xref:System.Console.WriteLine%2A> の呼び出しをいくつか挿入することで自分でテストできます。 このメソッドは、ドキュメントの例でデモのために使用されていますが、必要がない限り並列ループでは使用しないでください。  
  
## <a name="be-aware-of-thread-affinity-issues"></a>スレッド アフィニティの問題に注意する  
 シングル スレッド アパートメント (STA) コンポーネント向けの COM 相互運用性、Windows フォーム、Windows Presentation Foundation (WPF) などの一部のテクノロジでは、特定のスレッド上で実行するコードを必要とするスレッド アフィニティが制限される場合があります。 たとえば、Windows フォームと WPF では、コントロールへのアクセスは、そのコントロールが作成されたスレッド上でしか行うことができません。 つまり、たとえば、UI スレッドのみで処理をスケジュールするようにスレッド スケジューラを構成していない限り、並列ループからはリスト コントロールを更新できません。 詳細については、「[同期コンテキストの指定](xref:System.Threading.Tasks.TaskScheduler#specifying-a-synchronization-context)」を参照してください。  
  
## <a name="use-caution-when-waiting-in-delegates-that-are-called-by-parallelinvoke"></a>Parallel.Invoke によって呼び出されるデリゲートで待機する場合は注意する  
 状況によっては、タスク並列ライブラリでタスクをインライン展開します。これは、現在実行中のスレッドでタスクが実行されることを意味します (詳細については、[タスク スケジューラ](xref:System.Threading.Tasks.TaskScheduler)に関するページを参照してください)。場合によっては、このパフォーマンスの最適化によって、デッドロックが発生する可能性があります。 たとえば、2 つのタスクで同じデリゲート コードを実行していて、そのデリゲート コードは、イベントの発生時に通知し、もう 1 つのタスクが通知するまで待機するとします。 この場合、2 番目のタスクが 1 番目のタスクと同じスレッド情にインライン展開され、1 番目のタスクが待機状態になると、2 番目のタスクではそのイベントを通知できなくなります。 このような状況を回避するために、待機操作のタイムアウトを指定するか、明示的なスレッド コンストラクターを使用して、1 つのタスクがもう 1 つのタスクをブロックしないようにすることができます。  
  
## <a name="do-not-assume-that-iterations-of-foreach-for-and-forall-always-execute-in-parallel"></a>ForEach、For および ForAll のイテレーションが必ず並列実行されているとは限らない  
 <xref:System.Threading.Tasks.Parallel.For%2A>、<xref:System.Threading.Tasks.Parallel.ForEach%2A>、または <xref:System.Linq.ParallelEnumerable.ForAll%2A> の各ループにおける個々の反復処理が必ずしも並列実行されるとは限らないことに注意してください。 そのため、イテレーションの並列実行の正確性、または特定の順序でのイテレーションの実行の正確性に依存するコードを記述しないでください。 たとえば、次のコードはデッドロックが起こる可能性があります。  
  
 [!code-csharp[TPL_Pitfalls#01](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#01)]
 [!code-vb[TPL_Pitfalls#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#01)]  
  
 この例では、1 つのイテレーションでイベントを設定し、その他のすべてのイテレーションでイベントを待機します。 待機のイテレーションは、イベント設定のイテレーションが完了するまで完了できません。 ただし、待機のイテレーションによって、並列ループの実行に使用されるすべてのスレッドがブロックされ、イベント設定のイテレーションがまったく実行されなくなる可能性があります。 これにより、イベント設定のイテレーションが実行されず、待機のイテレーションが開始されないままの状態になるデッドロックが発生します。  
  
 具体的には、処理を適切に進めるには、並列ループの特定のイテレーションでそのループの別のイテレーションを待機するのは避ける必要があります。 並列ループで、イテレーションが逆の順序で順次スケジュールされた場合、デッドロックが発生します。  
  
## <a name="avoid-executing-parallel-loops-on-the-ui-thread"></a>UI スレッドでの並列ループの実行を回避する  
 アプリケーションのユーザー インターフェイス (UI) では、その応答性を維持することが重要です。 並列処理が必要になる量の処理が 1 つの操作で行われる場合、UI スレッドでその操作を実行しない方がよい可能性があります。  代わりに、バック グラウンド スレッドで実行されるように、その操作をオフロードしてください。 たとえば、特定のデータを計算し、その結果を UI コントロールに表示するために、並列ループを使用する必要がある場合は、UI イベント ハンドラーで直接実行するではなく、タスクのインスタンス内でループを実行することを検討してください。  中心的な計算が完了したときにのみ、UI 更新を UI スレッドにマーシャ リングするようにします。  
  
 UI スレッドで並列ループを実行する場合は、そのループ内から UI コントロールを更新しないように注意してください。 UI スレッドで実行されている並列ループ内から UI コントロールを更新しようとすると、UI 更新の呼び出し方法によっては、状態の破損、例外、更新の遅延、場合によってはデッドロックまで発生する可能性があります。 次の例では、並列ループが実行されている UI スレッドは、すべてのイテレーションが完了するまでループによってブロックされます。 ただし、このループのイテレーションがバックグラウンド スレッドで実行されると (<xref:System.Threading.Tasks.Parallel.For%2A> と同じ処理を行う)、Invoke の呼び出しによって UI スレッドにメッセージが送信され、そのメッセージが処理されるのを待機することになります。 <xref:System.Threading.Tasks.Parallel.For%2A> を実行している UI スレッドがブロックされるため、メッセージが処理されなくなり、UI スレッドでデッドロックが発生します。  
  
 [!code-csharp[TPL_Pitfalls#02](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#02)]
 [!code-vb[TPL_Pitfalls#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#02)]  
  
 次の例では、タスク インスタンス内でループを実行して、デッドロックを回避する方法を示します。 UI スレッドはループによってブロックされず、メッセージを処理することができます。  
  
 [!code-csharp[TPL_Pitfalls#03](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#03)]
 [!code-vb[TPL_Pitfalls#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#03)]  
  
## <a name="see-also"></a>関連項目

- [並列プログラミング](../../../docs/standard/parallel-programming/index.md)
- [PLINQ の非利便性](../../../docs/standard/parallel-programming/potential-pitfalls-with-plinq.md)
- [並列プログラミングのパターン:.NET Framework 4 での並列パターンの理解と適用](https://www.microsoft.com/download/details.aspx?id=19222)
