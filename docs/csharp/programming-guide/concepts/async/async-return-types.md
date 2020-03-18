---
title: 非同期の戻り値の型 (C#)
ms.date: 05/29/2017
ms.assetid: ddb2539c-c898-48c1-ad92-245e4a996df8
ms.openlocfilehash: 9926fea5308f9088ad924bcc98d8deed319c6300
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79170040"
---
# <a name="async-return-types-c"></a>非同期の戻り値の型 (C#)
非同期メソッドには、次の戻り値の型があります。

- <xref:System.Threading.Tasks.Task%601>: 値を返す非同期メソッドの場合。

- <xref:System.Threading.Tasks.Task>: 操作を実行し、値を返さない非同期メソッドの場合。

- `void`: イベント ハンドラーの場合。

- C# 7.0 以降、アクセス可能な `GetAwaiter` を持つ任意の型です。 `GetAwaiter` メソッドによって返されるオブジェクトは、<xref:System.Runtime.CompilerServices.ICriticalNotifyCompletion?displayProperty=nameWithType> インターフェイスを実装する必要があります。
  
非同期メソッドの詳細については、「[Async および Await を使用した非同期プログラミング (C#)](./index.md)」を参照してください。  
  
それぞれの戻り値の型は、次のセクションの 1 つで確認でき、トピックの最後で 3 種類のすべてを使用する例を参照できます。  
  
## <a name="BKMK_TaskTReturnType"></a> Task\<TResult\> の戻り値の型  
<xref:System.Threading.Tasks.Task%601> 型のオペランドを持つ [return](../../../language-reference/keywords/return.md) (C#) ステートメントを含む非同期メソッドには、`TResult` 戻り値の型が使用されます。  
  
次の例では、`GetLeisureHours` 非同期メソッドには整数を返す `return` ステートメントが含まれます。 そのため、メソッド宣言では、戻り値の型を `Task<int>` と指定する必要があります。  <xref:System.Threading.Tasks.Task.FromResult%2A> 非同期メソッドは、文字列を返す操作を表すプレースホルダーです。
  
[!code-csharp[return-value](../../../../../samples/snippets/csharp/programming-guide/async/async-returns1.cs)]

`GetLeisureHours` が `ShowTodaysInfo` メソッドの await 式の中から呼び出されると、await 式は `leisureHours` メソッドから返されるタスクに格納されている整数値 (`GetLeisureHours` の値) を取得します。 await 式の詳細については、「[await](../../../language-reference/operators/await.md)」を参照してください。  
  
次のコードに示すように、`GetLeisureHours` の呼び出しと、`await` の適用を分離すると、この仕組みをよく理解できます。 メソッドの宣言から予想されるように、直ちに待機しない `GetLeisureHours` メソッドの呼び出しは、`Task<int>` を返します。 タスクは、この例の `integerTask` 変数に割り当てられます。 `integerTask` は <xref:System.Threading.Tasks.Task%601> であるため、<xref:System.Threading.Tasks.Task%601.Result> 型の `TResult` プロパティが含まれています。 この場合、`TResult` は整数型を表します。 `await` が `integerTask` に適用されると、<xref:System.Threading.Tasks.Task%601.Result%2A> の `integerTask` プロパティの内容が await 式の評価となります。 この値は `ret` 変数に割り当てられます。  
  
> [!IMPORTANT]
> <xref:System.Threading.Tasks.Task%601.Result%2A> プロパティは Blocking プロパティです。 タスクが終了する前にアクセスしようとすると、現在アクティブなスレッドは、タスクが完了して値が使用可能になるまで、ブロックされます。 多くの場合、プロパティに直接アクセスする代わりに、`await` を使用して値にアクセスする必要があります。 <br/> 前の例では、アプリケーションが終了する前に <xref:System.Threading.Tasks.Task%601.Result%2A> メソッドが実行を終了できるように、`ShowTodaysInfo` プロパティの値を取得してメイン スレッドをブロックしました。  

[!code-csharp[return-value](../../../../../samples/snippets/csharp/programming-guide/async/async-returns1a.cs#1)]
  
## <a name="BKMK_TaskReturnType"></a>Task 型  
`return` ステートメントを含まない非同期メソッド、またはオペランドを返さない `return` ステートメントを含む非同期メソッドは、通常は <xref:System.Threading.Tasks.Task> の戻り値の型を指定します。 こうしたメソッドは、同期的に実行するように作成されている場合に `void` を返します。 非同期メソッドに戻り値の型 <xref:System.Threading.Tasks.Task> を使用した場合、呼び出し元のメソッドは `await` 演算子を使って、呼び出された async のメソッドが終了するまで、呼び出し元の完了を中断します。  
  
次の例では、`WaitAndApologize` 非同期メソッドには `return` ステートメントが含まれていないため、メソッドは <xref:System.Threading.Tasks.Task> オブジェクトを返します。 これにより、`WaitAndApologize` を待機できます。 <xref:System.Threading.Tasks.Task> 型には戻り値がないため、`Result` プロパティを含みません。  

[!code-csharp[return-value](../../../../../samples/snippets/csharp/programming-guide/async/async-returns2.cs)]  
  
`WaitAndApologize` を待機するには、void を返す同期メソッドを呼び出す場合と同様に、await 式でなく、await ステートメントを使用します。 この場合、await 演算子の適用によって値は生成されません。  
  
前の <xref:System.Threading.Tasks.Task%601> の例のように、次のコードに示すとおり、`WaitAndApologize` の呼び出しを await 演算子の適用から分離することができます。 ただし `Task` は `Result` プロパティを持たないこと、また await 演算子が `Task` に適用されるときに値は生成されないことに注意します。  
  
次のコードは、`WaitAndApologize` メソッドの呼び出しを、そのメソッドが返すタスクの待機から分離します。  

[!code-csharp[return-value](../../../../../samples/snippets/csharp/programming-guide/async/async-returns2a.cs#1)]  

## <a name="BKMK_VoidReturnType"></a> Void 戻り値の型

`void` 戻り値の型は、`void` 戻り値の型が必要な非同期イベント ハンドラーで使用します。 値を返さないイベント ハンドラー以外のメソッドについては、<xref:System.Threading.Tasks.Task> を返す必要があります。これは、`void` を返す非同期メソッドを待機できないためです。 このようなメソッドの呼び出し元は、呼び出した非同期メソッドが完了するのを待たずに、完了まで継続できる必要があります。また呼び出し元は、非同期メソッドが生成する値または例外とは無関係である必要があります。  
  
void を返す非同期メソッドの呼び出し元は、メソッドがスローする例外をキャッチすることはできません。そのようなハンドルされない例外によって、アプリケーションが失敗する可能性が高くなります。 <xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.Task%601> を返す非同期メソッドで例外が発生すると、例外は返されたタスクに格納され、タスクが待機するときに再スローされます。 したがって、例外を生成する場合がある非同期メソッドは <xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.Task%601> の戻り値の型を持つこと、またメソッドの呼び出しが待機することを確認します。  
  
非同期メソッドで例外をキャッチする方法の詳細については、「[try-catch](../../../language-reference/keywords/try-catch.md#exceptions-in-async-methods)」トピックの「[非同期メソッドの例外](../../../language-reference/keywords/try-catch.md)」を参照してください。  
  
次の例では、非同期イベント ハンドラーの動作を示します。 コード例では、非同期イベント ハンドラーが終了したとき、その非同期イベント ハンドラーからメイン スレッドに通知が送られる必要があることに注目してください。 このため、メイン スレッドでは、非同期イベント ハンドラーの終了を待ってから、プログラムを終了することができます。

[!code-csharp[return-value](../../../../../samples/snippets/csharp/programming-guide/async/async-returns3.cs)]  

## <a name="generalized-async-return-types-and-valuetasktresult"></a>一般化された非同期の戻り値の型と ValueTask\<TResult\>

C# 7.0 以降、非同期メソッドで、アクセス可能な `GetAwaiter` メソッドを持つ任意の型を返すことができます。

<xref:System.Threading.Tasks.Task> および <xref:System.Threading.Tasks.Task%601> は参照型であるため、特に厳密なループ処理で割り当てが発生すると、パフォーマンスが重要なパスのメモリ割り当てが、パフォーマンスに悪影響を及ぼすことがあります。 一般化された戻り値の型のサポートにより、参照型ではなく、軽量な値の型を返すことができ、追加のメモリ割り当てを回避することが可能です。

.NET には、一般化されたタスク戻り値の軽量な実装として、<xref:System.Threading.Tasks.ValueTask%601?displayProperty=nameWithType> 構造が用意されています。 <xref:System.Threading.Tasks.ValueTask%601?displayProperty=nameWithType> 型を使用するには、`System.Threading.Tasks.Extensions` NuGet パッケージをプロジェクトに追加する必要があります。 次の例では、<xref:System.Threading.Tasks.ValueTask%601> 構造を使用して、2 つのさいころを転がしたときの値を取得します。
  
[!code-csharp[return-value](../../../../../samples/snippets/csharp/programming-guide/async/async-valuetask.cs)]

## <a name="see-also"></a>参照

- <xref:System.Threading.Tasks.Task.FromResult%2A>
- [チュートリアル: async と await を使用した Web へのアクセス (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md)
- [非同期プログラムにおける制御フロー (C#)](./control-flow-in-async-programs.md)
- [async](../../../language-reference/keywords/async.md)
- [await](../../../language-reference/operators/await.md)
