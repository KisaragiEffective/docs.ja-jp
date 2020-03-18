---
title: PLINQ の非利便性
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, pitfalls
ms.assetid: 75a38b55-4bc4-488a-87d5-89dbdbdc76a2
ms.openlocfilehash: 3ddc0c013335e6a7b4708a5dd8be0b2247b2f60c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74716254"
---
# <a name="potential-pitfalls-with-plinq"></a>PLINQ の非利便性

PLINQ を使用すると、多くの場合、連続した LINQ to Objects クエリのパフォーマンスが大幅に向上します。 ただし、クエリの実行を並列化すると、複雑性が増すため、シーケンシャルなコードにおいて一般的でない問題、またはめったにない問題を引き起こす場合があります。 このトピックでは、PLINQ クエリを記述するときに回避すべき点について示します。

## <a name="dont-assume-that-parallel-is-always-faster"></a>並列処理が常に高速であると思い込まない

並列化することで、PLINQ クエリの実行速度が同等の LINQ to Objects より遅くなる場合があります。 基本的な経験則では、ソース要素が少なく、高速ユーザー デリゲートを使用するクエリの速度が大幅に向上することはほとんどありません。 ただし、パフォーマンスには多くの要因が関係するため、実際の結果を測定してから、PLINQ を使用するかどうかを決定することをお勧めします。 詳細については、「[Understanding Speedup in PLINQ (PLINQ での高速化について)](understanding-speedup-in-plinq.md)」を参照してください。

## <a name="avoid-writing-to-shared-memory-locations"></a>共有メモリの位置への書き込みを回避する

逐次コードでは、静的変数またはクラス フィールドから読み取ることや、これらの場所に書き込むことはよくあります。 ただし、複数のスレッドからこのような変数に同時にアクセスしているときは、著しい競合状態になる場合がよくあります。 ロックを使用して変数へのアクセスを同期できる場合でも、同期のコストでパフォーマンスが低下する可能性があります。 このため、PLINQ クエリにおける共有状態へのアクセスは、できるだけ回避するか、少なくとも制限することをお勧めします。

## <a name="avoid-over-parallelization"></a>過剰な並列化を回避する

`AsParallel` のメソッドを使用することで、ソース コレクションのパーティション分割とワーカー スレッドの同期によるオーバーヘッド コストが発生します。 並列化の利点は、コンピューター上のプロセッサ数によってさらに制限されます。 1 つのプロセッサで複数の計算主体のスレッドを実行しても、高速化は実現しません。 そのため、クエリを過剰に並列処理しないように注意する必要があります。

過剰な並列化が発生する可能性が特に高い一般的な状況は、次のスニペットで示すように、入れ子になったクエリの場合です。

[!code-csharp[PLINQ#20](~/samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#20)]
[!code-vb[PLINQ#20](~/samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#20)]

この場合、次の条件のうち 1 つ以上該当しない限り、外側のデータ ソース (customers) のみを並列化することをお勧めします。

- 内部データ ソース (cust.Orders) が非常に長い。

- 各順序で高負荷の計算を実行している (この例に示す操作の負荷は大きくありません)。

- 対象システムに、`cust.Orders` でクエリを並列化することで生成されるスレッドの数を十分に処理できるプロセッサが存在している。

どの場合も、最適なクエリの形式を決定する最善の方法は、テストおよび測定することです。 詳細については、[PLINQ クエリのパフォーマンスを測定する](../../../docs/standard/parallel-programming/how-to-measure-plinq-query-performance.md)」をご覧ください。

## <a name="avoid-calls-to-non-thread-safe-methods"></a>スレッド セーフでないメソッドの呼び出しを回避する

PLINQ クエリからスレッド セーフでないインスタンス メソッドに書き込むと、プログラムで検出されない可能性のあるデータ破損が起こる可能性があります。 例外が発生する可能性もあります。 次の例では、複数のスレッドが、クラスでサポートされていない `FileStream.Write` メソッドの同時呼び出しを試みます。

```vb
Dim fs As FileStream = File.OpenWrite(…)
a.AsParallel().Where(...).OrderBy(...).Select(...).ForAll(Sub(x) fs.Write(x))
```

```csharp
FileStream fs = File.OpenWrite(...);
a.AsParallel().Where(...).OrderBy(...).Select(...).ForAll(x => fs.Write(x));
```

## <a name="limit-calls-to-thread-safe-methods"></a>スレッド セーフなメソッドの呼び出しを制限する

.NET Framework のほとんどの静的メソッドはスレッド セーフであり、複数のスレッドから同時に呼び出すことができます。 ただし、このような場合でも、関連する同期によっては、クエリの処理速度が大幅に低下する可能性があります。

> [!NOTE]
> これは、クエリに <xref:System.Console.WriteLine%2A> の呼び出しをいくつか挿入することで自分でテストできます。 このメソッドは、ドキュメントの例でデモのために使用されていますが、PLINQ クエリでは使用しないでください。

## <a name="avoid-unnecessary-ordering-operations"></a>不要な並べ替え操作を回避する

PLINQ を使用して 1 つのクエリを並列で実行すると、ソース シーケンスがパーティションに分割され、複数のスレッド上で同時に操作できるようになります。 既定では、パーティションが処理される順序と生成される結果は予測できません (`OrderBy` などの演算子を除く)。 PLINQ に、ソース シーケンスの順序を維持するように指示することもできますが、この操作はパフォーマンスの低下を招きます。 推奨される手順は、できる限り、順序の維持に依存しないようにクエリを構成することです。 詳細については、「[Order Preservation in PLINQ (PLINQ における順序維持)](order-preservation-in-plinq.md)」を参照してください。

## <a name="prefer-forall-to-foreach-when-it-is-possible"></a>できる限り ForEach より ForAll を優先する

PLINQ では複数のスレッドで 1 つのクエリを実行しますが、`foreach` ループ (Visual Basic の場合は `For Each`) の結果を処理する場合、クエリ結果は 1 つのスレッドにマージされてから列挙子によって連続してアクセスされる必要があります。 これが避けられない場合もありますが、できる限り `ForAll` メソッドを使用して各スレッドで固有の結果を出力できるようにします。たとえば、<xref:System.Collections.Concurrent.ConcurrentBag%601?displayProperty=nameWithType> などのスレッド セーフなコレクションに書き込みます。

同じイシューが <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> に適用されます。 つまり、`source.AsParallel().Where().ForAll(...)` が `Parallel.ForEach(source.AsParallel().Where(), ...)` よりも強く求められます。

## <a name="be-aware-of-thread-affinity-issues"></a>スレッド アフィニティのイシューに注意する

シングル スレッド アパートメント (STA) コンポーネント向けの COM 相互運用性、Windows フォーム、Windows Presentation Foundation (WPF) などの一部のテクノロジでは、特定のスレッド上で実行するコードを必要とするスレッド アフィニティが制限される場合があります。 たとえば、Windows フォームと WPF では、コントロールへのアクセスは、そのコントロールが作成されたスレッド上でしか行うことができません。 PLINQ クエリで Windows フォーム コントロールの共有状態にアクセスしようとすると、デバッガーを実行中である場合は例外が発生します (この設定はオフにできます)。ただし、クエリが UI スレッドで処理される場合、コードは 1 つのスレッドでのみ実行されるので、クエリ結果を列挙する `foreach` ループからコントロールにアクセスできます。

## <a name="dont-assume-that-iterations-of-foreach-for-and-forall-always-execute-in-parallel"></a>ForEach、For および ForAll のイテレーションが必ず並列実行されているとは限らない

<xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType>、または <xref:System.Linq.ParallelEnumerable.ForAll%2A> の各ループにおける個々のイテレーションが必ずしも並列実行されるとは限らないことに注意してください。 そのため、イテレーションの並列実行の正確性、または特定の順序でのイテレーションの実行の正確性に依存するコードを記述しないでください。

たとえば、次のコードはデッドロックが起こる可能性があります。

```vb
Dim mre = New ManualResetEventSlim()
Enumerable.Range(0, Environment.ProcessorCount * 100).AsParallel().ForAll(Sub(j)
   If j = Environment.ProcessorCount Then
       Console.WriteLine("Set on {0} with value of {1}", Thread.CurrentThread.ManagedThreadId, j)
       mre.Set()
   Else
       Console.WriteLine("Waiting on {0} with value of {1}", Thread.CurrentThread.ManagedThreadId, j)
       mre.Wait()
   End If
End Sub) ' deadlocks
```

```csharp
ManualResetEventSlim mre = new ManualResetEventSlim();
Enumerable.Range(0, Environment.ProcessorCount * 100).AsParallel().ForAll((j) =>
{
    if (j == Environment.ProcessorCount)
    {
        Console.WriteLine("Set on {0} with value of {1}", Thread.CurrentThread.ManagedThreadId, j);
        mre.Set();
    }
    else
    {
        Console.WriteLine("Waiting on {0} with value of {1}", Thread.CurrentThread.ManagedThreadId, j);
        mre.Wait();
    }
}); //deadlocks
```

この例では、1 つのイテレーションでイベントを設定し、その他のすべてのイテレーションでイベントを待機します。 待機のイテレーションは、イベント設定のイテレーションが完了するまで完了できません。 ただし、待機のイテレーションによって、並列ループの実行に使用されるすべてのスレッドがブロックされ、イベント設定のイテレーションがまったく実行されなくなる可能性があります。 これにより、イベント設定のイテレーションが実行されず、待機のイテレーションが開始されないままの状態になるデッドロックが発生します。

具体的には、処理を適切に進めるには、並列ループの特定のイテレーションでそのループの別のイテレーションを待機するのは避ける必要があります。 並列ループで、イテレーションが逆の順序で順次スケジュールされた場合、デッドロックが発生します。

## <a name="see-also"></a>関連項目

- [Parallel LINQ (PLINQ)](parallel-linq-plinq.md)
