---
title: Async および Await を使用したタスク非同期プログラミング モデル (TAP) (C#)
ms.date: 05/22/2017
ms.assetid: 9bcf896a-5826-4189-8c1a-3e35fa08243a
ms.openlocfilehash: 2700f5bfaa2746c1ea6f4862bee3af60cf83d693
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78848979"
---
# <a name="task-asynchronous-programming-model"></a>非同期プログラミング モデル

パフォーマンスのボトルネックを回避しアプリケーション全体の応答性を向上させるために、非同期プログラミングを使用できます。 ただ、非同期アプリケーションを作成する従来の方法は複雑で、プログラムの作成、デバッグ、保守が困難な場合があります。

[C# 5](../../../whats-new/csharp-version-history.md#c-version-50) では、.NET Framework 4.5 以降、.NET Core および Windows ランタイムの非同期サポートを利用した "非同期プログラミング" と呼ばれる簡単な方法が導入されました。 コンパイラがこれまで開発者が行っていた難しい作業を実行し、アプリケーションは同期コードに類似した論理構造を保持します。 その結果、わずかな作業量で非同期プログラミングのすべての利点を得られます。

このトピックでは、非同期プログラミングをいつ、どのように使用するかの概要を紹介します。詳細と例を含むをサポート トピックへのリンクもあります。

## <a name="BKMK_WhentoUseAsynchrony"></a>非同期による応答性の改善

Web アクセスなど、ブロックされる可能性がある操作には、非同期が必要となります。 Web リソースへのアクセスには、遅延が発生することがあります。 このような操作が同期処理内でブロックされた場合、アプリケーション全体が待機する必要があります。 非同期処理では、ブロックする可能性のあるタスク終了するまで、アプリケーションは Web リソースに依存しない他の操作を続行できます。

非同期プログラミングによって応答性を向上する一般的な領域を、次の表に示します。 .NET および Windows ランタイムの API の一覧には、非同期のプログラミングをサポートするメソッドが含まれます。

| アプリケーション領域    | 非同期メソッドがある .NET 型     | 非同期メソッドがある Windows ランタイム型  |
|---------------------|-----------------------------------|-------------------------------------------|
|Web アクセス|<xref:System.Net.Http.HttpClient>|<xref:Windows.Web.Syndication.SyndicationClient>|
|ファイルの処理|<xref:System.IO.StreamWriter>、 <xref:System.IO.StreamReader>、 <xref:System.Xml.XmlReader>|<xref:Windows.Storage.StorageFile>|
|イメージの処理||<xref:Windows.Media.Capture.MediaCapture>、 <xref:Windows.Graphics.Imaging.BitmapEncoder>、 <xref:Windows.Graphics.Imaging.BitmapDecoder>|
|WCF プログラミング|[同期操作と非同期操作](../../../../framework/wcf/synchronous-and-asynchronous-operations.md)||

非同期性は、UI スレッドにアクセスするアプリケーションに対して特に有効です。これは、すべての UI 関連のアクティビティが一般的に 1 つのスレッドを共有するためです。 同期アプリケーションでは、1 つのプロセスがブロックされるとすべてがブロックされます。 アプリケーションが応答を停止するため、待機状態であるとは考えずに失敗したと結論付けることもあります。

非同期メソッドを使用すると、アプリケーションは UI に応答し続けます。 たとえば、ウィンドウのサイズ変更や最小化を実行したり、アプリケーション処理の完了待たずに、アプリケーションを閉じたりできます。

非同期ベースの方法は、非同期操作を設計する場合に選択できるオプションの一覧に、自動送信に相当するものを追加します。 つまり、開発者の少しの作業量で、従来の非同期プログラミングのすべての利点を取得できます。

## <a name="BKMK_HowtoWriteanAsyncMethod"></a>非同期メソッドの作成の簡素化

C# の [async](../../../language-reference/keywords/async.md) キーワードと [await](../../../language-reference/operators/await.md) キーワードは、非同期プログラミングの中核です。 これら 2 つのキーワードを使用すると、同期メソッドの作成とほぼ同様の容易さで、.NET Framework、.NET Core または Windows ランタイムのリソースを使用して非同期メソッドを作成できます。 `async` キーワードを使用して定義する非同期メソッドは、"*async メソッド*" として参照されます。

async メソッドの例を次に示します。 コードのほとんどは、見たことのあるものと思います。

このトピックの最後に完全な Windows Presentation Foundation (WPF) サンプル ファイルがあります。また、「[Async Sample: Example from "Asynchronous Programming with Async and Await" (非同期のサンプル: 「Async および Await を使用した非同期プログラミング」の例)](https://docs.microsoft.com/samples/dotnet/samples/async-and-await-cs/)」からサンプルをダウンロードできます。

```csharp
async Task<int> AccessTheWebAsync()
{
    // You need to add a reference to System.Net.Http to declare client.
    var client = new HttpClient();

    // GetStringAsync returns a Task<string>. That means that when you await the
    // task you'll get a string (urlContents).
    Task<string> getStringTask = client.GetStringAsync("https://docs.microsoft.com/dotnet");

    // You can do work here that doesn't rely on the string from GetStringAsync.
    DoIndependentWork();

    // The await operator suspends AccessTheWebAsync.
    //  - AccessTheWebAsync can't continue until getStringTask is complete.
    //  - Meanwhile, control returns to the caller of AccessTheWebAsync.
    //  - Control resumes here when getStringTask is complete.
    //  - The await operator then retrieves the string result from getStringTask.
    string urlContents = await getStringTask;

    // The return statement specifies an integer result.
    // Any methods that are awaiting AccessTheWebAsync retrieve the length value.
    return urlContents.Length;
}
```

上のサンプルからいくつかの方法を習得できます。 メソッド シグネチャから始めます。 これには `async` 修飾子が含まれています。 戻り値の型は `Task<int>` です (他のオプションについては "戻り値の型" セクションを参照してください)。 メソッド名の末尾は `Async` です。 メソッドの本体で、`GetStringAsync` により `Task<string>` が返されます。 つまり、タスクに `await` を指定すると、`string` が与えられます (`urlContents`)。  タスクを待つ前に、`string` の `GetStringAsync` に依存しない作業を実行できます。

`await` 演算子に特に注意してください。 これは `AccessTheWebAsync` を一時停止させます。

- `AccessTheWebAsync` は `getStringTask` が完了するまで続行できません。
- その間、コントロールは `AccessTheWebAsync` の呼び出し元に戻されます。
- `getStringTask` が完了すると、コントロールがここに戻ります。
- 次に、`await` 演算子は `string` から `getStringTask` の結果を取得します。

return ステートメントによって整数の結果が指定されます。 `AccessTheWebAsync` を待つメソッドは長さ値を取得します。

`AccessTheWebAsync` に `GetStringAsync` を呼び出してその完了を待機する間で実行できる作業がない場合、次の 1 つのステートメントで呼び出しと待機をするようにコードを簡略化できます。

```csharp
string urlContents = await client.GetStringAsync("https://docs.microsoft.com/dotnet");
```

次の特徴は、前の例を非同期のメソッドにするための概略です。

- メソッド シグネチャは `async` 修飾子を含みます。
- 非同期メソッドの名前は、慣例により「Async」というサフィックスで終わります。
- 戻り値の型は次のいずれかになります:

  - メソッドが、オペランドに <xref:System.Threading.Tasks.Task%601> 型を持つステートメントを戻す場合、`TResult`。
  - メソッドがステートメントを戻さない、またはオペランドを持たないステートメントを戻す場合、<xref:System.Threading.Tasks.Task>。
  - 非同期のイベント ハンドラーを作成する場合、`void`。
  - `GetAwaiter` メソッドがあるその他の任意の型 (C# 7.0 以降)。

  詳細については、「[戻り値の型およびパラメーター](#BKMK_ReturnTypesandParameters)」セクションを参照してください。

- メソッドには、通常は 1 つ以上の `await` 式があり、待機中の非同期操作が完了するまでメソッドを続行できないポイントをマークします。 この間メソッドは中断し、メソッドの呼び出し元にコントロールを戻します。 このトピックの次のセクションでは、中断ポイントで何が発生するかを説明します。

非同期のメソッドでは、指定のキーワードと型を使用して何を実行するかを示すと、コンパイラがその作業を引き継ぎます。作業には、中断されたメソッドの待機ポイントにコントロールが戻された場合に実行される作業を、継続的に追跡することも含まれます。 ループおよび例外処理など一部のルーチンのプロセスは、従来の非同期コードによる操作が困難な場合があります。 非同期のメソッドでは、同期ソリューションの場合と同様にこれらの要素を記述すると、問題が解決します。

.NET Framework の以前のバージョンでの非同期性の詳細については、「[TPL と従来の .NET Framework 非同期プログラミング](../../../../standard/parallel-programming/tpl-and-traditional-async-programming.md)」を参照してください。

## <a name="BKMK_WhatHappensUnderstandinganAsyncMethod"></a>非同期メソッドでの動作

非同期プログラミングでは理解が必要な最も重要なことは、コントロール フローがどのようにメソッドからのメソッドに移動するかということです。 次の図では、このプロセスについて説明します。

![非同期プログラムのトレース](./media/task-asynchronous-programming-model/navigation-trace-async-program.png "NavigationTrace")

図の番号は次の手順に対応しています。ユーザーが "開始" ボタンをクリックすると始まります。

1. イベント ハンドラーは `AccessTheWebAsync` 非同期のメソッドを呼び出して待機します。

2. `AccessTheWebAsync` は <xref:System.Net.Http.HttpClient> インスタンスを作成し、文字列として Web サイトのコンテンツをダウンロードする <xref:System.Net.Http.HttpClient.GetStringAsync%2A> 非同期メソッドを呼び出します。

3. `GetStringAsync` に何かが発生するとプロセスが中断します。 Web サイトからのダウンロード処理、または他のブロックしているアクティビティを待機する必要が考えられます。 リソースのブロックを回避するために、`GetStringAsync` は呼び出し元の `AccessTheWebAsync` にコントロールを戻します。

     `GetStringAsync` は <xref:System.Threading.Tasks.Task%601> が文字列である `TResult` を返し、`AccessTheWebAsync` は `getStringTask` 変数にタスクを割り当てます。 タスクには `GetStringAsync` への呼び出しの進行中のプロセスを表し、作業が完了すると実際の文字列値を生成するコミットメントがあります。

4. `getStringTask` が待機しないため、`AccessTheWebAsync` は `GetStringAsync` からの最終結果に依存しない他の作業を続行できます。 この作業は同期メソッド `DoIndependentWork` への呼び出しによって表されます。

5. `DoIndependentWork` は、作業を実行し、呼び出し元に戻る同期メソッドです。

6. `AccessTheWebAsync` は `getStringTask` からの結果なしで実行できる作業を使い果たしました。 `AccessTheWebAsync` は次に、ダウンロードする文字列の長さを計算しますが、メソッドに文字列が戻されるまで、メソッドはその値を計算できません。

     そのため、`AccessTheWebAsync` は await 演算子を使用してその進行を中断し、`AccessTheWebAsync` を呼び出したメソッドにコントロールを戻します。 `AccessTheWebAsync` は呼び出し元に `Task<int>` を返します。 タスクは、ダウンロードされた文字列の長さの整数値を生成することの保証を表します。

    > [!NOTE]
    > `GetStringAsync` (結果として `getStringTask`) が `AccessTheWebAsync` がそれを待機する前に完了した場合、コントロールは `AccessTheWebAsync` に残ります。 `AccessTheWebAsync` を中断して後から戻ることは、呼び出された非同期プロセス (`getStringTask`) が既に完了していて、`AccessTheWebAsync` が最終結果を待つ必要がない場合に、無駄になることがあります。

     呼び出し元 (この例ではイベント ハンドラー) の内部で、処理パターンが続行されます。 呼び出し元は `AccessTheWebAsync` からの結果に依存しない他の作業をすることもあり、または直ちに待機状態になることもあります。   イベント ハンドラーは `AccessTheWebAsync` を待機し、`AccessTheWebAsync` は、`GetStringAsync` を待機します。

7. `GetStringAsync` が完了し、文字列の結果を生成します。 文字列の結果は、`GetStringAsync` への呼び出しによって、意図した形式では戻されません。 (メソッドは既に手順 3. でタスクを戻しています。) 代わりに、文字列の結果は、`getStringTask` メソッドの完了を示すタスク内に格納されます。 await 演算子は、`getStringTask` から結果を取得します。 代入ステートメントは `urlContents` に取得された結果を割り当てます。

8. `AccessTheWebAsync` に文字列の結果がある場合、メソッドは文字列の長さを計算できます。 次に `AccessTheWebAsync` の作業も完了し、待機しているイベント ハンドラーが再開できます。 トピックの最後にある完全なサンプルでは、イベント ハンドラーが長さの結果の値を取得して印刷することを確認できます。
非同期プログラミングの経験がない場合、同期および非同期の動作の違いを、少し時間を割いて考慮してください。 同期メソッドは作業が完了すると戻されます (手順 5.) が、非同期のメソッドは、作業が中断されるとタスクの値を戻します。(手順 3. および 6.) 非同期のメソッドが最終的に作業を完了すると、タスクは完了とマークされ、結果が存在する場合はタスクに格納されます。

制御フローの詳細については、「[非同期プログラムにおける制御フロー (C#)](control-flow-in-async-programs.md)」を参照してください。

## <a name="BKMK_APIAsyncMethods"></a> API の非同期メソッド

非同期のプログラミングをサポートする `GetStringAsync` などのメソッドがどこにあるのかということです。 .NET Framework 4.5 以降および .NET Core には、`async` および `await` で使用する多くのメンバーが含まれています。 メンバー名に付記されている "Async" というサフィックスと、その戻り値の型である <xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.Task%601> から識別できます。 たとえば、`System.IO.Stream` のクラスには、同期メソッドの <xref:System.IO.Stream.CopyToAsync%2A>、<xref:System.IO.Stream.ReadAsync%2A>、および <xref:System.IO.Stream.WriteAsync%2A> と共に、<xref:System.IO.Stream.CopyTo%2A>、<xref:System.IO.Stream.Read%2A> および <xref:System.IO.Stream.Write%2A> という同期メソッドが含まれています。

Windows ランタイムにも、Windows アプリの `async` と `await` で使用できる多くのメソッドが含まれています。 UWP 開発の詳細については、「[スレッド化と非同期プログラミング](/windows/uwp/threading-async/)」を参照してください。旧バージョンの Windows ランタイムを使用する場合は、「[Asynchronous programming (Windows Store apps)](https://docs.microsoft.com/previous-versions/windows/apps/hh464924(v=win.10))」 (非同期プログラミング (Windows ストア アプリ)) および「[Quickstart: Calling asynchronous APIs in C# or Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/hh452713(v=win.10))」(クイック スタート: C# または Visual Basic での非同期 API の呼び出し) を参照してください。

## <a name="BKMK_Threads"></a>スレッド

非同期のメソッドは非ブロッキング操作を意図しています。 非同期のメソッドの `await` 式では、待機中のタスクの実行時に現在のスレッドはブロックされません。 代わりに、式はメソッドの残りの部分の継続を登録し、非同期のメソッドの呼び出し元にコントロールを戻します。

`async` および `await` キーワードは、追加のスレッドを作成する要因にはなりません。 非同期のメソッドは自分自身のスレッドで実行しないため、マルチスレッドは必要ありません。 メソッドは、現在の同期コンテキストで実行し、メソッドがアクティブな場合に限りスレッドの時間を使用します。 <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType> を使用して、CPU バインディングの作業をバックグラウンド スレッドに移動できますが、バックグラウンド スレッドは、結果を待つだけのプロセスを援助しません。

非同期プログラミングへの非同期ベースのアプローチは、ほぼすべてのケースの既存のアプローチに推奨されます。 特に、このアプローチはコードがシンプルで競合状態からの保護の必要がないため、I/O バウンドの操作では、<xref:System.ComponentModel.BackgroundWorker> クラスよりも優れています。 <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType> メソッドと組み合わせると、非同期のプログラミングは CPU バインディングの操作に関して <xref:System.ComponentModel.BackgroundWorker> よりも優れています。非同期のプログラミングは、`Task.Run` がスレッド プールから移動する作業から、実行するコードの調整の詳細を分離するためです。

## <a name="BKMK_AsyncandAwait"></a>async と await

[async](../../../language-reference/keywords/async.md) 修飾子を使用して、メソッドが非同期メソッドであることを指定すると、次の 2 つの機能が有効になります。

- マークされた非同期のメソッドは中断ポイントを示すために [await](../../../language-reference/operators/await.md) を使用できます。 `await` 演算子は、非同期のメソッドが、待機中の非同期のプロセスが完了するまでこのポイント以降を続行できないことを、コンパイラに指示します。 その間、コントロールは非同期のメソッドの呼び出し元に戻されます。

     非同期のメソッドの `await` 式での中断は、メソッドからの終了を意図するものではなく、`finally` ブロックは実行されません。

- マークされた非同期のメソッド自体は、呼び出し元のメソッドによって待機できます。

非同期のメソッドには、通常の `await` 演算子が 1 つ以上ありますが、`await` 式がない場合もコンパイラ エラーの原因にはなりません。 中断ポイントをマークするために非同期のメソッドが `await` 演算子を使用しない場合、`async` 修飾子が存在しても、メソッドは同期メソッドと同様に実行されます。 このようなメソッドには、コンパイラが警告を発行します。

`async` と `await` は、コンテキスト キーワードです。 詳細と例については、次のトピックを参照してください。

- [async](../../../language-reference/keywords/async.md)
- [await](../../../language-reference/operators/await.md)

## <a name="BKMK_ReturnTypesandParameters"></a>戻り値の型およびパラメーター

非同期メソッドは、通常 <xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.Task%601> を返します。 非同期のメソッド内で、`await` 演算子は、他の非同期のメソッドへの呼び出しから戻されたタスクに適用されます。

メソッドが、<xref:System.Threading.Tasks.Task%601> 型のオペランドを指定する [`return`](../../../language-reference/keywords/return.md) ステートメントを含む場合、`TResult` を戻り値の型として指定します。

メソッドに Return ステートメントがない場合、または Return ステートメントがオペランドを戻さない場合、<xref:System.Threading.Tasks.Task> を戻り値の型として使用します。

C# 7.0 以降では、その型に `GetAwaiter` メソッドがある場合に限り、別の戻り値の型を指定できます。 このような型の例として、<xref:System.Threading.Tasks.ValueTask%601> が挙げられます。 これは、[System.Threading.Tasks.Extension](https://www.nuget.org/packages/System.Threading.Tasks.Extensions/) NuGet パッケージにあります。

次のサンプルは、<xref:System.Threading.Tasks.Task%601> または <xref:System.Threading.Tasks.Task> を戻すメソッドを宣言して呼び出す方法を示します。

```csharp
// Signature specifies Task<TResult>
async Task<int> GetTaskOfTResultAsync()
{
    int hours = 0;
    await Task.Delay(0);
    // Return statement specifies an integer result.
    return hours;
}

// Calls to GetTaskOfTResultAsync
Task<int> returnedTaskTResult = GetTaskOfTResultAsync();
int intResult = await returnedTaskTResult;
// or, in a single statement
int intResult = await GetTaskOfTResultAsync();

// Signature specifies Task
async Task GetTaskAsync()
{
    await Task.Delay(0);
    // The method has no return statement.
}

// Calls to GetTaskAsync
Task returnedTask = GetTaskAsync();
await returnedTask;
// or, in a single statement
await GetTaskAsync();
```

それぞれ、進行中の作業を示すタスクを戻します。 タスクに非同期処理の状態に関する情報、および最終的にはプロセスからの最終結果、またはプロセスが成功しなかった場合に発生する例外をカプセル化します。

非同期のメソッドの戻り値の型としては、`void` を指定できます。 この戻り値の型は主として、`void` の戻り値の型が必要なイベント ハンドラーの定義に使用されます。 非同期のイベント ハンドラーは通常、非同期のプログラムの開始点として機能します。

`void` の戻り値の型を持つ非同期のメソッドは、待機できません。void を戻すメソッドの呼び出し元では、このメソッドがスローする例外をキャッチできません。

非同期のメソッドで [in](../../../language-reference/keywords/in-parameter-modifier.md)、[ref](../../../language-reference/keywords/ref.md)、または [out](../../../language-reference/keywords/out-parameter-modifier.md) パラメーターを宣言することはできませんが、これらのパラメーターを持つメソッドを呼び出すことはできます。 同様に、非同期メソッドは ref 戻り値を使用してメソッドを呼び出すことはできますが、参照を使用して値を返すことはできません。

使用例を含む詳細については、「[非同期の戻り値の型 (C#)](./async-return-types.md)」を参照してください。 非同期のメソッドで例外をキャッチする方法の詳細については、「[try-catch](../../../language-reference/keywords/try-catch.md)」を参照してください。

Windows ランタイム プログラミングの非同期 API には、タスクに類似した次のような戻り値の型の 1 つがあります。

- <xref:Windows.Foundation.IAsyncOperation%601> は <xref:System.Threading.Tasks.Task%601> に対応します
- <xref:Windows.Foundation.IAsyncAction> は <xref:System.Threading.Tasks.Task> に対応します
- <xref:Windows.Foundation.IAsyncActionWithProgress%601>
- <xref:Windows.Foundation.IAsyncOperationWithProgress%602>

## <a name="BKMK_NamingConvention"></a>名前付け規則

慣例により、一般的に待機可能な型 (たとえば `Task`、`Task<T>`、`ValueTask`、`ValueTask<T>`) を返すメソッドについては、その名前の末尾に "Async" を付けます。 非同期操作を開始するメソッドであっても、そのメソッドが待機可能な型を返さない場合は、メソッド名の末尾に "Async" を付けてはなりませんが、このメソッドが操作の結果を返したりスローしたりしないことを示す "Begin" や "Start" などの動詞を先頭に付けることはかまいません。

イベント、基底クラス、またはインターフェイスのコントラクトが別の名前を表示している場合は、この慣例を無視できます。 たとえば、`Button1_Click` などの共通のイベント ハンドラーの名前は、変更しないことをお勧めします。

## <a name="BKMK_RelatedTopics"></a>関連トピックとサンプル (Visual Studio)

|タイトル|[説明]|サンプル|
|-----------|-----------------|------------|
|[チュートリアル: async と await を使用した Web へのアクセス (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md)|同期 WPF のソリューションを非同期 WPF のソリューションに変換する方法を示します。 アプリケーションは、一連の Web サイトをダウンロードします。|[Async Sample: Accessing the Web Walkthrough (非同期のサンプル: Web サイトへのアクセスのチュートリアル)](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f)|
|[Task.WhenAll を使用して AsyncWalkthrough を拡張する方法 (C#)](./how-to-extend-the-async-walkthrough-by-using-task-whenall.md)|前のチュートリアルに <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> を追加します。 `WhenAll` を使用すると、すべてのダウンロードが同時に開始します。||
|[async と await を使用して複数の Web 要求を並列実行する方法 (C#)](./how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await.md)|複数のタスクを同時に開始する方法を示します。|[Async Sample: Make Multiple Web Requests in Parallel (非同期のサンプル: 複数の並行 Web 要求の作成)](https://code.msdn.microsoft.com/Async-Make-Multiple-Web-49adb82e)|
|[非同期の戻り値の型 (C#)](./async-return-types.md)|非同期のメソッドが戻す型、および各型の適切な使用方法を説明します。||
|[非同期プログラムにおける制御フロー (C#)](./control-flow-in-async-programs.md)|非同期プログラムでの await 式を継続して、コントロールのフローの詳細をトレースします。|[Async Sample: Control Flow in Async Programs (非同期のサンプル: 非同期プログラムにおける制御フロー)](https://code.msdn.microsoft.com/Async-Sample-Control-Flow-5c804fc0)|
|[非同期アプリケーションの微調整 (C#)](./fine-tuning-your-async-application.md)|非同期のソリューションに次の機能を追加する方法を示します:<br /><br /> - [非同期タスクまたはタスクの一覧のキャンセル (C#)](./cancel-an-async-task-or-a-list-of-tasks.md)<br />- [指定した時間の経過後の非同期タスクのキャンセル (C#)](./cancel-async-tasks-after-a-period-of-time.md)<br />- [完了後の残りの非同期タスクのキャンセル (C#)](./cancel-remaining-async-tasks-after-one-is-complete.md)<br />- [完了時での複数の非同期タスクとプロセスの実行 (C#)](./start-multiple-async-tasks-and-process-them-as-they-complete.md)|[非同期のサンプル: アプリケーションの微調整](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)|
|[非同期アプリにおける再入の処理 (C#)](./handling-reentrancy-in-async-apps.md)|実行中にアクティブな非同期操作を再起動するケースの処理方法を示します。||
|[WhenAny: .NET Framework と Windows ランタイム間のブリッジ](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/jj635140(v=vs.120))|Windows ランタイムのメソッドで <xref:System.Threading.Tasks.Task.WhenAny%2A> を使えるようにするため、Windows ランタイムで .NET Framework および IAsyncOperations のタスクの種類間をブリッジする方法を示します。|[Async Sample: Bridging between .NET and Windows Runtime (AsTask and WhenAny) (非同期のサンプル: .NET と Windows ランタイム間のブリッジ (AsTask と WhenAny))](https://code.msdn.microsoft.com/Async-Sample-Bridging-d6a2f739)|
|非同期のキャンセル: .NET Framework と Windows ランタイム間のブリッジ|Windows ランタイムのメソッドで <xref:System.Threading.CancellationTokenSource> を使えるようにするため、Windows ランタイムで .NET Framework および IAsyncOperations のタスクの種類間をブリッジする方法を示します。|[Async Sample: Bridging between .NET and Windows Runtime (AsTask & Cancellation) (非同期のサンプル: .NET と Windows ランタイム間のブリッジ (AsTask と Cancellation))](https://code.msdn.microsoft.com/Async-Sample-Bridging-9479eca3)|
|[ファイル アクセスにおける非同期の使用 (C#)](./using-async-for-file-access.md)|async および await を使用してファイルにアクセスすることの利点の一覧と紹介です。||
|[タスク ベースの非同期パターン (TAP)](../../../../standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md)|.NET Framework での非同期性の新しいパターンについて説明します。 パターンは <xref:System.Threading.Tasks.Task> および <xref:System.Threading.Tasks.Task%601> の型に基づいています。||
|[Channel 9 の非同期に関するビデオ](https://channel9.msdn.com/search?term=async%20&type=All#pubDate=year&ch9Search&lang-en=en)|非同期のプログラミングに関するさまざまなビデオへのリンクを示します。||

## <a name="BKMK_CompleteExample"></a> コード例全体

次のコードは、この記事で説明する WPF アプリケーションの *MainWindow.xaml.cs* ファイルです。 サンプルは、「[Async Sample: Example from "Asynchronous Programming with Async and Await" (非同期のサンプル: 「Async および Await を使用した非同期プログラミング」の例)](https://docs.microsoft.com/samples/dotnet/samples/async-and-await-cs/)」からダウンロードできます。

[!code-csharp[async](~/samples/snippets/standard/async/async-and-await/cs/MainWindow.xaml.cs)]

## <a name="see-also"></a>参照

- [async](../../../language-reference/keywords/async.md)
- [await](../../../language-reference/operators/await.md)
- [非同期プログラミング](../../../async.md)
- [非同期の概要](../../../../standard/async.md)
