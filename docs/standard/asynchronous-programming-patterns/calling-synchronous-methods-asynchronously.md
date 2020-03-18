---
title: 同期メソッドの非同期呼び出し
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- asynchronous programming, delegates
- asynchronous delegates
- AsyncWaitHandle property
- callback methods
- calling synchronous methods in asynchronous manner
- WaitHandle class, code examples
- asynchronous programming, status polling
- polling asynchronous operation status
- delegates [.NET Framework], asynchronous
- synchronous calling in asynchronous manner
- waiting for asynchronous calls
- status information [.NET Framework], asynchronous operations
ms.assetid: 41972034-92ed-450a-9664-ab93fcc6f1fb
ms.openlocfilehash: 06df584f0120fbd4978e18647854a3ee844a2095
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73105125"
---
# <a name="calling-synchronous-methods-asynchronously"></a>同期メソッドの非同期呼び出し

.NET Framework では、すべてのメソッドを非同期的に呼び出すことができます。 これを行うには、呼び出すメソッドと同じシグネチャを持つデリゲートを定義します。これにより、共通言語ランタイムによって、適切なシグネチャを持つ、このデリゲートの `BeginInvoke` メソッドと `EndInvoke` メソッドが自動的に定義されます。

> [!NOTE]
> 非同期デリゲート (具体的には `BeginInvoke` メソッドと `EndInvoke` メソッド) は、.NET Compact Framework ではサポートされていません。

`BeginInvoke` メソッドは、非同期呼び出しを開始します。 このメソッドは、非同期的に実行するメソッドと同じパラメーターと共に、2 つの省略可能な追加パラメーターを持っています。 最初のパラメーターは、同期呼び出しが完了したときに呼び出されるメソッドを参照する <xref:System.AsyncCallback> デリゲートです。 2 番目のパラメーターは、コールバック メソッドに情報を渡すユーザー定義オブジェクトです。 `BeginInvoke` からは制御がすぐに戻り、非同期呼び出しが完了するまで待機しません。 `BeginInvoke` は <xref:System.IAsyncResult>を返します。これを使用して非同期呼び出しの進捗状況を監視できます。

`EndInvoke` メソッドは、非同期呼び出しの結果を取得します。 このメソッドは、 `BeginInvoke`の後であればいつでも呼び出すことができます。 非同期呼び出しがまだ完了していない場合は、 `EndInvoke` は非同期呼び出しが完了するまで呼び出し元スレッドをブロックします。 `EndInvoke` のパラメーターには、非同期実行するメソッドの `out` パラメーターと `ref` パラメーター (Visual Basic では `<Out>` `ByRef` と `ByRef`) と、`BeginInvoke` によって返された <xref:System.IAsyncResult> が含まれます。

> [!NOTE]
> Visual Studio の IntelliSense 機能によって `BeginInvoke` と `EndInvoke` のパラメーターが表示されます。 Visual Studio や類似のツールを使っていない場合や、Visual Studio で C# を使っている場合、これらのメソッドについて定義されているパラメーターについては、「[非同期プログラミング モデル (APM)](../../../docs/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm.md)」を参照してください。

このトピックのコード例では、 `BeginInvoke` と `EndInvoke` を使用して非同期呼び出しを行う 4 つの一般的な方法を示します。 `BeginInvoke` を呼び出した後、次の処理を行うことができます。

- 何か処理を実行した後、呼び出しが完了するまでブロックする `EndInvoke` を呼び出します。

- <xref:System.Threading.WaitHandle> プロパティを使用して <xref:System.IAsyncResult.AsyncWaitHandle%2A?displayProperty=nameWithType> を取得し、その <xref:System.Threading.WaitHandle.WaitOne%2A> メソッドを使用して <xref:System.Threading.WaitHandle> が通知されるまで実行をブロックし、 `EndInvoke`を呼び出します。

- <xref:System.IAsyncResult> によって返される `BeginInvoke` をポーリングして非同期呼び出しが完了したかどうかを確認した後、 `EndInvoke`を呼び出します。

- コールバック メソッドのデリゲートを `BeginInvoke`に渡します。 このメソッドは、非同期呼び出しが完了すると、 <xref:System.Threading.ThreadPool> スレッドで実行されます。 コールバック メソッドは `EndInvoke`を呼び出します。

> [!IMPORTANT]
> どの手法を使用する場合でも、常に `EndInvoke` を呼び出して、非同期呼び出しを完了します。

## <a name="defining-the-test-method-and-asynchronous-delegate"></a>テスト メソッドと非同期デリゲートの定義
 次のコード例は、長時間実行される `TestMethod`メソッドの非同期呼び出しを行うさまざまな方法を示します。 `TestMethod` メソッドはコンソール メッセージを表示して処理が開始されたことを示し、しばらくスリープした後、終了します。 `TestMethod` には `out` パラメーターがあり、 `BeginInvoke` および `EndInvoke`のシグネチャへのそのようなパラメーターの追加方法を示します。 `ref` パラメーターは同様に処理できます。

 次のコード例は、 `TestMethod` と、 `AsyncMethodCaller` の非同期呼び出しに使用できる `TestMethod` という名前のデリゲートの定義を示します。 コード例をコンパイルするには、 `TestMethod` および `AsyncMethodCaller` デリゲートの定義を含める必要があります。

 [!code-cpp[AsyncDelegateExamples#1](../../../samples/snippets/cpp/VS_Snippets_CLR/AsyncDelegateExamples/cpp/TestMethod.cpp#1)]
 [!code-csharp[AsyncDelegateExamples#1](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDelegateExamples/CS/TestMethod.cs#1)]
 [!code-vb[AsyncDelegateExamples#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDelegateExamples/VB/TestMethod.vb#1)]

## <a name="waiting-for-an-asynchronous-call-with-endinvoke"></a>EndInvoke による非同期呼び出しの待機
 メソッドを非同期実行する最も簡単な方法は、デリゲートの `BeginInvoke` メソッドを呼び出してメソッドの実行を開始し、メイン スレッドで何かの処理を実行した後、デリゲートの `EndInvoke` メソッドを呼び出す方法です。 `EndInvoke` は非同期呼び出しが完了するまで戻らないので、呼び出し元スレッドがブロックされる場合があります。 この手法はファイルやネットワーク操作を使用するときに適しています。

> [!IMPORTANT]
> `EndInvoke` でブロックするため、ユーザー インターフェイスにサービスを提供するスレッドからは呼び出さないでください。

 [!code-cpp[AsyncDelegateExamples#2](../../../samples/snippets/cpp/VS_Snippets_CLR/AsyncDelegateExamples/cpp/EndInvoke.cpp#2)]
 [!code-csharp[AsyncDelegateExamples#2](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDelegateExamples/CS/EndInvoke.cs#2)]
 [!code-vb[AsyncDelegateExamples#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDelegateExamples/VB/EndInvoke.vb#2)]

## <a name="waiting-for-an-asynchronous-call-with-waithandle"></a>WaitHandle による非同期呼び出しの待機
 <xref:System.Threading.WaitHandle> を取得するには、 <xref:System.IAsyncResult.AsyncWaitHandle%2A> によって返される <xref:System.IAsyncResult> の `BeginInvoke`プロパティを使用します。 <xref:System.Threading.WaitHandle> は非同期呼び出しが完了すると通知され、 <xref:System.Threading.WaitHandle.WaitOne%2A> メソッドを呼び出すことによってこれを待機できます。

 <xref:System.Threading.WaitHandle>を使用する場合は、非同期呼び出しの完了前または完了後、 `EndInvoke` を呼び出して結果を取得する前に、追加の処理を実行できます。

> [!NOTE]
> `EndInvoke`を呼び出す場合、待機ハンドルは自動的に閉じられません。 待機ハンドルへのすべての参照を解放すると、ガベージ コレクションが待機ハンドルをクリアするときにシステム リソースが解放されます。 待機ハンドルの使用が終了すると同時にシステム リソースを解放するには、 <xref:System.Threading.WaitHandle.Close%2A?displayProperty=nameWithType> メソッドを呼び出して破棄します。 破棄可能なオブジェクトが明示的に破棄されると、ガベージ コレクションはより効率的に動作します。

 [!code-cpp[AsyncDelegateExamples#3](../../../samples/snippets/cpp/VS_Snippets_CLR/AsyncDelegateExamples/cpp/waithandle.cpp#3)]
 [!code-csharp[AsyncDelegateExamples#3](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDelegateExamples/CS/waithandle.cs#3)]
 [!code-vb[AsyncDelegateExamples#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDelegateExamples/VB/WaitHandle.vb#3)]

## <a name="polling-for-asynchronous-call-completion"></a>非同期呼び出し完了のポーリング
 <xref:System.IAsyncResult.IsCompleted%2A> によって返された <xref:System.IAsyncResult> の `BeginInvoke` プロパティを使用して、非同期呼び出しが完了したことを検出できます。 この方法は、ユーザー インターフェイスにサービスを提供するスレッドから非同期呼び出しを行う場合に使用します。 完了をポーリングすると、呼び出し元スレッドは、 <xref:System.Threading.ThreadPool> スレッドで非同期呼び出しを実行しながら、実行を継続できます。

 [!code-cpp[AsyncDelegateExamples#4](../../../samples/snippets/cpp/VS_Snippets_CLR/AsyncDelegateExamples/cpp/polling.cpp#4)]
 [!code-csharp[AsyncDelegateExamples#4](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDelegateExamples/CS/polling.cs#4)]
 [!code-vb[AsyncDelegateExamples#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDelegateExamples/VB/polling.vb#4)]

## <a name="executing-a-callback-method-when-an-asynchronous-call-completes"></a>非同期呼び出し完了時のコールバック メソッドの実行
 非同期呼び出しを開始したスレッドが結果を処理するスレッドである必要がない場合は、呼び出しが完了したときにコールバック メソッドを実行できます。 コールバック メソッドは <xref:System.Threading.ThreadPool> スレッドで実行されます。

 コールバック メソッドを使用するには、コールバック メソッドを表す `BeginInvoke` デリゲートを <xref:System.AsyncCallback> に渡す必要があります。 コールバック メソッドで使用される情報を含むオブジェクトを渡すこともできます。 コールバック メソッドでは、唯一のパラメーターである <xref:System.IAsyncResult>を <xref:System.Runtime.Remoting.Messaging.AsyncResult> オブジェクトにキャストします。 こうすると、 <xref:System.Runtime.Remoting.Messaging.AsyncResult.AsyncDelegate%2A?displayProperty=nameWithType> プロパティを使用して、呼び出しを開始するために使用したデリゲートを取得し、 `EndInvoke`を呼び出すことができるようになります。

 例に関する注意事項

- `TestMethod` の `threadId` パラメーターは `out` パラメーター (Visual Basic では [`<Out>` `ByRef`) であるため、その入力値が `TestMethod` で使用されることはありません。 `BeginInvoke` 呼び出しにはダミー変数が渡されます。 `threadId` パラメーターが `ref` パラメーター (Visual Basic では`ByRef` ) であった場合、 `BeginInvoke` と `EndInvoke`の両方に渡すことができるように、変数はクラス レベルのフィールドであることが必要です。

- `BeginInvoke` に渡される状態情報は、コールバック メソッドが出力メッセージを書式指定するために使用する書式指定文字列です。 型 <xref:System.Object>として渡されるため、状態情報を使用するには適切な型にキャストする必要があります。

- コールバックは <xref:System.Threading.ThreadPool> スレッドで作成されます。 <xref:System.Threading.ThreadPool> スレッドは、メイン スレッドが終了した場合はアプリケーションの実行を継続しないバックグラウンド スレッドであるため、例で使用するメイン スレッドは、コールバックが終了できるまでスリープする必要があります。

 [!code-cpp[AsyncDelegateExamples#5](../../../samples/snippets/cpp/VS_Snippets_CLR/AsyncDelegateExamples/cpp/callback.cpp#5)]
 [!code-csharp[AsyncDelegateExamples#5](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDelegateExamples/CS/callback.cs#5)]
 [!code-vb[AsyncDelegateExamples#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDelegateExamples/VB/callback.vb#5)]

## <a name="see-also"></a>関連項目

- <xref:System.Delegate>
- [イベント ベースの非同期パターン (EAP)](../../../docs/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-eap.md)
