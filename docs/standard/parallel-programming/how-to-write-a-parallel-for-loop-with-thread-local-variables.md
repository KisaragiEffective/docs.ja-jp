---
title: '方法: スレッド ローカル変数を使用する Parallel.For ループを記述する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallel for loops, how to use local state
ms.assetid: 68384064-7ee7-41e2-90e3-71f00bde01bb
ms.openlocfilehash: 14f4f1402f564d38bb508e893521a3951c1509f4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73139716"
---
# <a name="how-to-write-a-parallelfor-loop-with-thread-local-variables"></a>方法: スレッド ローカル変数を使用する Parallel.For ループを記述する
次の例に、<xref:System.Threading.Tasks.Parallel.For%2A> ループによって生成される個別のタスクごとの状態を、スレッド ローカル変数を使用して格納および取得する方法を示します。 スレッド ローカル変数を使用することで、共有状態への多数のアクセスを同期するオーバーヘッドを回避できます。 反復処理ごとに共有リソースを作成する代わりに、タスクの反復処理のすべてが完了するまで、値を計算して格納します。 この場合、最終結果を共有リソースに 1 回書き込んだり、別のメソッドに渡したりすることができます。  
  
## <a name="example"></a>例  
 <xref:System.Threading.Tasks.Parallel.For%60%601%28System.Int32%2CSystem.Int32%2CSystem.Func%7B%60%600%7D%2CSystem.Func%7BSystem.Int32%2CSystem.Threading.Tasks.ParallelLoopState%2C%60%600%2C%60%600%7D%2CSystem.Action%7B%60%600%7D%29> メソッドを呼び出して、100 万個の要素からなる配列の値の合計を計算する例を次に示します。 各要素の値は、そのインデックスに相当します。  
  
 [!code-csharp[TPL_Parallel#05](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/forandforeach_simple.cs#05)]
 [!code-vb[TPL_Parallel#05](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/forwiththreadlocal.vb#05)]  
  
 すべての <xref:System.Threading.Tasks.Parallel.For%2A> メソッドで、最初の 2 つのパラメーターが最初と最後の反復値を指定します。 メソッドのこのオーバーロードでは、3 番目のパラメーターでローカル状態を初期化します。 このコンテキストでのローカル状態は、現在のスレッドで実行されるループの最初の反復処理の直前から最後の反復処理の直後までの有効期限を持つ変数を意味します。  
  
 3 番目のパラメーターの型は <xref:System.Func%601> です。ここで、`TResult` はスレッド ローカル状態を格納する変数の型です。 この型は、ジェネリックの <xref:System.Threading.Tasks.Parallel.For%60%601%28System.Int32%2CSystem.Int32%2CSystem.Func%7B%60%600%7D%2CSystem.Func%7BSystem.Int32%2CSystem.Threading.Tasks.ParallelLoopState%2C%60%600%2C%60%600%7D%2CSystem.Action%7B%60%600%7D%29> メソッドの呼び出し時に提供されるジェネリック型引数によって定義されます (この例では、<xref:System.Int64>)。 型引数は、コンパイラに対し、スレッド ローカル状態を格納するために使用する一時変数の型を指定します。 この例では、式 `() => 0` (Visual Basic の場合は `Function() 0`) でスレッド ローカル変数をゼロに初期化します。 ジェネリック型引数が参照型またはユーザー定義の値型である場合は、以下のような式になります。  
  
```csharp  
() => new MyClass()  
```  
  
```vb  
Function() new MyClass()  
```  
  
 4 番目のパラメーターは、ループのロジックを定義します。 パラメーター値は、シグネチャが `Func<int, ParallelLoopState, long, long>` (C# の場合) または `Func(Of Integer, ParallelLoopState, Long, Long)` (Visual Basic の場合) となっているデリゲートまたはラムダ式でなければなりません。 最初のパラメーターは、ループのその特定の反復処理に対するループ カウンターの値です。 2 番目のパラメーターは、ループを抜けるために使用できる <xref:System.Threading.Tasks.ParallelLoopState> オブジェクトです。このオブジェクトは、<xref:System.Threading.Tasks.Parallel> クラスによって各ループの発生時に提供されます。 3 番目のパラメーターは、スレッド ローカル変数です。 最後のパラメーターは、戻り値の型です。 この例の場合、型は <xref:System.Int64> 型引数で指定されているため、<xref:System.Threading.Tasks.Parallel.For%2A> になります。 その変数の名前は `subtotal` です。これは、ラムダ式によって返されます。 この戻り値が、ループの次の反復処理で `subtotal` を初期化するために使用されます。 この最後のパラメーターは、各反復処理に渡されて、最後の反復処理が完了した時点で `localFinally` デリゲートに渡される値であるとも考えられます。  
  
 5 番目のパラメーターが定義するメソッドは、特定のスレッドでのすべての反復処理が完了した時点で 1 回だけ呼び出されます。 この場合も、入力引数の型は <xref:System.Threading.Tasks.Parallel.For%60%601%28System.Int32%2CSystem.Int32%2CSystem.Func%7B%60%600%7D%2CSystem.Func%7BSystem.Int32%2CSystem.Threading.Tasks.ParallelLoopState%2C%60%600%2C%60%600%7D%2CSystem.Action%7B%60%600%7D%29> メソッドの型引数、および本体のラムダ式によって返される型と一致します。 この例では、スレッド セーフな方法で、この値をクラス スコープで変数に追加するために、<xref:System.Threading.Interlocked.Add%2A?displayProperty=nameWithType> メソッドを呼び出します。 スレッド ローカル変数を使用することで、このクラス変数をループのすべての反復処理で作成する手間を省きました。  
  
 ラムダ式の使用方法の詳細については、「[PLINQ および TPL のラムダ式](../../../docs/standard/parallel-programming/lambda-expressions-in-plinq-and-tpl.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [データの並列化](../../../docs/standard/parallel-programming/data-parallelism-task-parallel-library.md)
- [並列プログラミング](../../../docs/standard/parallel-programming/index.md)
- [タスク並列ライブラリ (TPL)](../../../docs/standard/parallel-programming/task-parallel-library-tpl.md)
- [PLINQ および TPL のラムダ式](../../../docs/standard/parallel-programming/lambda-expressions-in-plinq-and-tpl.md)
