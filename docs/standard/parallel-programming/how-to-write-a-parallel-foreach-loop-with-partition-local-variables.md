---
title: '方法: パーティション ローカル変数を使用する Parallel.ForEach ループを記述する'
ms.date: 06/26/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallel foreach loop, how to use local state
ms.assetid: 24b10041-b30b-45cb-aa65-66cf568ca76d
ms.openlocfilehash: cca48889670c3bd67366c879ccede94c89542c8d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73139688"
---
# <a name="how-to-write-a-parallelforeach-loop-with-partition-local-variables"></a>方法: パーティション ローカル変数を使用する Parallel.ForEach ループを記述する

パーティション ローカル変数を使用する <xref:System.Threading.Tasks.Parallel.ForEach%2A> メソッドを記述する方法を次の例に示します。 <xref:System.Threading.Tasks.Parallel.ForEach%2A> ループが実行されると、そのソース コレクションが複数のパーティションに分割されます。 各パーティションは、パーティション ローカル変数の独自のコピーを所有しています。 パーティション ローカル変数は、1 つのスレッドに対して複数のパーティションを実行できる点を除き、[スレッド ローカル変数](xref:System.Threading.ThreadLocal%601)と似ています。

この例のコードおよびパラメーターは、対応する <xref:System.Threading.Tasks.Parallel.For%2A> メソッドによく似ています。 詳細については、「[方法: スレッド ローカル変数を使用する Parallel.For ループを記述する](../../../docs/standard/parallel-programming/how-to-write-a-parallel-for-loop-with-thread-local-variables.md)」を参照してください。

<xref:System.Threading.Tasks.Parallel.ForEach%2A> ループでパーティション ローカル変数を使用するには、2 つのタイプのパラメーターを取るメソッド オーバーロードのうち、いずれか 1 つを呼び出す必要があります。 最初の型パラメーター `TSource` でソース要素の型を指定し、2 番目の型パラメーター `TLocal` でパーティション ローカル変数の型を指定します。

## <a name="example"></a>例

以下の例では、<xref:System.Threading.Tasks.Parallel.ForEach%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%601%7D%2CSystem.Func%7B%60%600%2CSystem.Threading.Tasks.ParallelLoopState%2C%60%601%2C%60%601%7D%2CSystem.Action%7B%60%601%7D%29?displayProperty=nameWithType> のオーバーロードを呼び出して、100 万個の要素からなる配列の合計を計算します。 このオーバーロードには、4 つのパラメータがあります。

- `source`。データ ソースです。 これは、<xref:System.Collections.Generic.IEnumerable%601> を実装する必要があります。 この例のデータ ソースは、`IEnumerable<Int32>` によって返される、100 万個のメンバーからなる <xref:System.Linq.Enumerable.Range%2A?displayProperty=nameWithType> オブジェクトです。

- `localInit`。またはパーティション ローカル変数を初期化する関数です。 この関数は、<xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> 操作が実行される各パーティションで 1 回呼び出されます。 この例では、パーティション ローカル変数がゼロに初期化されます。

- `body`。ループの各反復処理に対して並列ループで呼び出される <xref:System.Func%604> です。 シグニチャは `Func\<TSource, ParallelLoopState, TLocal, TLocal>` です。 プログラマはデリゲートのコードを作成します。入力パラメーターはループから渡されます。これらの入力パラメーターは以下のとおりです。

  - <xref:System.Collections.Generic.IEnumerable%601> の現在の要素。

  - <xref:System.Threading.Tasks.ParallelLoopState> 変数。デリゲートのコードで、ループの状態を確認するために使用できます。

  - パーティション ローカル変数。

  デリゲートからパーティション ローカル変数が返されると、その変数は、その特定のパーティションで実行されるループの次の反復処理に渡されます。 ループ パーティションごとに、この変数の個別のインスタンスが保持されます。

  この例では、デリゲートによって個々の整数の値がパーティション ローカル変数に追加されます。スレッド ローカル変数には、そのパーティションで順次追加される整数要素の値の現在の合計が保持されます。

- `localFinally`。各パーティションでのループ操作が完了した時点で、`Action<TLocal>` によって呼び出される <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> デリゲート。 <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> メソッドは、`Action<TLocal>` デリゲートに、このループ パーティションのパーティション ローカル変数の最終値を返します。プログラマは、このパーティションの結果と他のパーティションの結果を結合するために必要な操作を実行するコードを作成します。 このデリゲートは、複数のタスクで同時に呼び出すことができます。 このため、この例では <xref:System.Threading.Interlocked.Add%28System.Int32%40%2CSystem.Int32%29?displayProperty=nameWithType> 変数へのアクセスを同期するために `total` メソッドが使用されます。 デリゲート型は <xref:System.Action%601> であるため、戻り値はありません。

[!code-csharp[TPL_Parallel#04](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/foreachthreadlocal.cs#04)]
[!code-vb[TPL_Parallel#04](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/foreachthreadlocal.vb#04)]

## <a name="see-also"></a>参照

- [データの並列化](../../../docs/standard/parallel-programming/data-parallelism-task-parallel-library.md)
- [方法: スレッド ローカル変数を使用する Parallel.For ループを記述する](../../../docs/standard/parallel-programming/how-to-write-a-parallel-for-loop-with-thread-local-variables.md)
- [PLINQ および TPL のラムダ式](../../../docs/standard/parallel-programming/lambda-expressions-in-plinq-and-tpl.md)
