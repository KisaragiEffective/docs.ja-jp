---
title: await 演算子 - C# リファレンス
ms.date: 11/08/2019
f1_keywords:
- await_CSharpKeyword
helpviewer_keywords:
- await keyword [C#]
- await [C#]
ms.assetid: 50725c24-ac76-4ca7-bca1-dd57642ffedb
ms.openlocfilehash: 6dc058f3850e30d8c424d4372c47b127c7d361b6
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75712742"
---
# <a name="await-operator-c-reference"></a>await 演算子 (C# リファレンス)

`await` 演算子では、そのオペランドによって表わされる非同期操作が完了するまで、外側の [async](../keywords/async.md) メソッドの評価が保留になります。 非同期操作が完了すると、`await` 演算子から演算の結果が返されます (結果がある場合)。 既に完了している操作を表わすオペランドに `await` 演算子が適用されると、外側のメソッドを保留にすることなく、演算の結果がすぐに返されます。 `await` 演算子では、async メソッドを評価するスレッドがブロックされません。 `await` 演算子によって外側の async メソッドが保留になるとき、メソッドの呼び出し元にコントロールが戻ります。

次の例では、完了時にバイト配列を生成する非同期操作を表わす `Task<byte[]>` インスタンスが、<xref:System.Net.Http.HttpClient.GetByteArrayAsync%2A?displayProperty=nameWithType> メソッドから返されます。 操作が完了するまで、`await` 演算子によって `DownloadDocsMainPageAsync` メソッドが保留になります。 `DownloadDocsMainPageAsync` が保留になると、`DownloadDocsMainPageAsync` の呼び出し元である `Main` メソッドにコントロールが返されます。 `DownloadDocsMainPageAsync` メソッドで実行される非同期操作の結果が必要になるまで `Main` メソッドが実行されます。 <xref:System.Net.Http.HttpClient.GetByteArrayAsync%2A> ですべてのバイトが得られると、`DownloadDocsMainPageAsync` メソッドの残りが評価されます。 その後、`Main` メソッドの残りが評価されます。

[!code-csharp[await example](~/samples/csharp/language-reference/operators/AwaitOperator.cs)]

前の例では、[async `Main` メソッド](../../programming-guide/main-and-command-args/index.md)が使用されています。これは C# 7.1 以降で可能です。 詳細は、「[await operator in the Main method](#await-operator-in-the-main-method)」 (Main メソッドの await 演算子) セクションを参照してください。

> [!NOTE]
> 非同期プログラミングの概要については、「[async および await を使用した非同期プログラミング](../../programming-guide/concepts/async/index.md)」を参照してください。 `async` と `await` による非同期プログラミングは、[タスクベースの非同期パターン](../../../standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md)に続きます。

メソッド、[ラムダ式](../../programming-guide/statements-expressions-operators/lambda-expressions.md)、[async](../keywords/async.md) キーワードで修飾される[匿名メソッド](delegate-operator.md)でのみ `await` 演算子を使用できます。 async メソッド内では、同期関数の本文の中、[lock ステートメント](../keywords/lock-statement.md)のブロックの内部、[安全でない](../keywords/unsafe.md)コンテキストの中で `await` 演算子を使用することはできません。

.NET の型として <xref:System.Threading.Tasks.Task>、<xref:System.Threading.Tasks.Task%601>、<xref:System.Threading.Tasks.ValueTask>、<xref:System.Threading.Tasks.ValueTask%601> がありますが、`await` 演算子のオペランドはそのいずれかになります。 ただし、待機可能な式であれば `await` 演算子のオペランドになります。 詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の「[待機可能な式](~/_csharplang/spec/expressions.md#awaitable-expressions)」セクションを参照してください。

C# 8.0 以降では、`await foreach` ステートメントを使用して、データの非同期ストリームを利用できます。 詳細については、「[C# 8.0 の新機能](../../whats-new/csharp-8.md)」記事の「[非同期ストリーム](../../whats-new/csharp-8.md#asynchronous-streams)」セクションを参照してください。

式 `t` の型が <xref:System.Threading.Tasks.Task%601> または <xref:System.Threading.Tasks.ValueTask%601> の場合、式 `await t` の型は `TResult` になります。 `t` の型が <xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.ValueTask> の場合、`await t` の型は `void` になります。 いずれの場合も、`t` で例外がスローされる場合、`await t` で再び例外がスローされます。 例外処理の詳細については、[try-catch ステートメント](../keywords/try-catch.md)に関する記事の「[非同期メソッドの例外](../keywords/try-catch.md#exceptions-in-async-methods)」を参照してください。

`async` キーワードと `await` キーワードは、C# 5 以降で使用できます。

## <a name="await-operator-in-the-main-method"></a>Main メソッドの await 演算子

C# 7.1 以降、アプリケーション エントリ ポイントである [`Main` メソッド](../../programming-guide/main-and-command-args/index.md)から `Task` または `Task<int>` が返され、その本文で `await` 演算子を使用できるよう、非同期にすることができます。 以前の C# バージョンでは、非同期操作の完了を `Main` メソッドが待つようにする目的で、対応する async メソッドから返される <xref:System.Threading.Tasks.Task%601> インスタンスの <xref:System.Threading.Tasks.Task%601.Result?displayProperty=nameWithType> プロパティの値を取得できます。 値を生成しない非同期操作の場合、<xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> メソッドを呼び出すことができます。 言語のバージョンを選択する方法については、「[C# 言語のバージョン管理](../configure-language-version.md)」を参照してください。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の「[Await 式](~/_csharplang/spec/expressions.md#await-expressions)」セクションを参照してください。

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- [async](../keywords/async.md)
- [タスクの非同期プログラミング モデル](../../programming-guide/concepts/async/task-asynchronous-programming-model.md)
- [非同期プログラミング](../../async.md)
- [非同期の詳細](../../../standard/async-in-depth.md)
- [チュートリアル: async と await を使用した Web へのアクセス](../../programming-guide/concepts/async/walkthrough-accessing-the-web-by-using-async-and-await.md)
- [チュートリアル: C#8.0 および .NET Core 3.0 を使用して非同期ストリームを生成および使用する](../../tutorials/generate-consume-asynchronous-stream.md)
