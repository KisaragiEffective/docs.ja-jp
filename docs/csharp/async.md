---
title: 非同期プログラミング - C#
description: .NET Core で提供される、C# 言語レベルの非同期プログラミング モデルについて説明します。
author: cartermp
ms.date: 06/20/2016
ms.technology: csharp-async
ms.assetid: b878c34c-a78f-419e-a594-a2b44fa521a4
ms.openlocfilehash: 38d7c856e9a536db9ef26349175ad440a49f5fe2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713950"
---
# <a name="asynchronous-programming"></a>非同期プログラミング

I/O バインドのニーズ (ネットワークからのデータの要求、データベースへのアクセスなど) がある場合、非同期プログラミングを利用することになります。  CPU バインドのコードにも、コストのかかる計算の実行など、非同期コードに適したシナリオがあります。

C# は言語レベルで非同期プログラミング モデルを備えており、コールバックに苦労したり、非同期処理をサポートするライブラリに従ったりしなくても、非同期コードを簡単に記述できます。 C# は、[タスク ベースの非同期パターン (TAP)](../standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md) と呼ばれるものに従います。

## <a name="basic-overview-of-the-asynchronous-model"></a>非同期モデルの基本的な概要

非同期プログラミングの中心になるのは `Task` オブジェクトと `Task<T>` オブジェクトであり、非同期操作をモデル化します。  これらは、`async` および `await` キーワードによってサポートされています。  ほとんどの場合、モデルは非常に単純です。

I/O バインドのコードでは、`await` メソッドの内部で `Task` または `Task<T>` を返す操作を待機 (`async`) します。

CPU バインドのコードでは、`await` メソッドによってバックグラウンド スレッドで開始された操作を待機 (`Task.Run`) します。

`await` キーワードはマジックが行われる場所であり、 `await` を実行したメソッドの呼び出し元に制御が委譲されます。これによって最終的に、UI は応答できるようになり、サービスは柔軟性を持つようになります。

上でリンクされている TAP の記事で説明されているように `async` と `await` 以外にも非同期コードへのアプローチはありますが、このドキュメントではこれ以降、言語レベルの構造に注目します。

### <a name="io-bound-example-downloading-data-from-a-web-service"></a>I/O バインドの例: Web サービスからのデータのダウンロード

ボタンがクリックされたら Web サービスからデータをダウンロードする必要がありますが UI スレッドはブロックしたくない、といった場合があります。 これは、次のように簡単に実行できます。

```csharp
private readonly HttpClient _httpClient = new HttpClient();

downloadButton.Clicked += async (o, e) =>
{
    // This line will yield control to the UI as the request
    // from the web service is happening.
    //
    // The UI thread is now free to perform other work.
    var stringData = await _httpClient.GetStringAsync(URL);
    DoSomethingWithData(stringData);
};
```

以上です。 コードでは、Task オブジェクトとの対話に煩わされることなく意図すること (データを非同期的にダウンロードする) が表されています。

### <a name="cpu-bound-example-performing-a-calculation-for-a-game"></a>CPU バインドの例: ゲームの計算を実行する

ボタンをクリックすると画面上の多くの敵にダメージを与えることができるモバイル ゲームを作っています。  ダメージ計算の実行は負荷が大きく、UI スレッドで実行すると計算の実行中はゲームが停止しているように見えます。

これを処理する最善の方法は、バックグラウンド スレッドを開始し、その中で `Task.Run` を使って処理を実行して、結果を `await` することです。  このようにすると、処理が行われている間も UI が停止することはありません。

```csharp
private DamageResult CalculateDamageDone()
{
    // Code omitted:
    //
    // Does an expensive calculation and returns
    // the result of that calculation.
}

calculateButton.Clicked += async (o, e) =>
{
    // This line will yield control to the UI while CalculateDamageDone()
    // performs its work.  The UI thread is free to perform other work.
    var damageResult = await Task.Run(() => CalculateDamageDone());
    DisplayDamage(damageResult);
};
```

これで終了です。  このコードでは、ボタンのクリック イベントの意図が明瞭に表されています。バックグラウンド スレッドを手動で管理する必要はなく、ブロッキングが発生しない方法で行われます。

### <a name="what-happens-under-the-covers"></a>内部での処理

非同期操作には多数の動作要素が関係します。  `Task` と `Task<T>` の内部処理について詳しくは、「[非同期の詳細](../standard/async-in-depth.md)」をご覧ください。

C# 側では、コンパイラはコードをステート マシンに変換します。ステート マシンは、`await` に達したときの実行の委譲や、バックグラウンド ジョブが終了したときの実行の再開などを追跡します。

理論的には、これは[非同期処理の約束モデル](https://en.wikipedia.org/wiki/Futures_and_promises)の実装です。

## <a name="key-pieces-to-understand"></a>理解すべき重要事項

* 非同期コードは I/O バインドと CPU バインドの両方のコードで使うことができますが、使い方はシナリオにより異なります。
* 非同期コードで使われる `Task<T>` と `Task` は、バックグラウンドで行われる処理のモデル化に使われる構成要素です。
* キーワード `async` はメソッドを非同期メソッドに変換し、メソッドの本体で `await` キーワードを使用できるようにします。
* 適用された `await` キーワードは、呼び出しメソッドを中断し、待機中のタスクが完了するまで呼び出し元に制御を戻します。
* `await` は、非同期メソッドの中でのみ使用できます。

## <a name="recognize-cpu-bound-and-io-bound-work"></a>CPU バインドと I/O バインドの処理

このガイドの最初の 2 つの例では、CPU バインドと I/O バインドの処理に対して `async` および `await` を使う方法を示しました。  行う必要のあるジョブが I/O バインドか CPU バインドかを識別できることが重要です。これは、コードのパフォーマンスに大きく影響し、特定の構成の誤った使用につながる可能性があります。

コードを記述する前に次の 2 点について考える必要があります。

1. コードは何か (データベースのデータなど) を "待機" していますか。

    答えが "はい" の場合、その処理は **I/O バインド**です。

2. コードは、非常に負荷の大きい計算を実行しますか。

    答えが "はい" の場合、その処理は **CPU バインド**です。

処理が **I/O バインド**の場合は、`async` と `await` を使いますが、 *は "* 使いません`Task.Run`"。  タスク並列ライブラリは "*使わないでください*"。  その理由について詳しくは、「[非同期の詳細](../standard/async-in-depth.md)」をご覧ください。

処理が **CPU バインド**であり、応答性が重要な場合は、`async` と `await` を使い、 *を "* 使って`Task.Run`" 別のスレッドで処理を実行します。  処理がコンカレンシーと並列処理に適している場合は、[タスク並列ライブラリ](../standard/parallel-programming/task-parallel-library-tpl.md)を使うことも考慮する必要があります。

さらに、常にコードの実行を測定する必要があります。  たとえば、マルチスレッドでのコンテキスト切り替えのオーバーヘッドと比較して、CPU バインドの処理の負荷がそれほど大きくないことがわかる場合があります。  すべての選択肢にはトレードオフがあり、状況に合った適切なトレードオフを選ぶ必要があります。

## <a name="more-examples"></a>その他の例

以下では、C# で非同期コードを記述するさまざまな方法がわかる例を示します。  実際に遭遇する可能性があるいくつかの異なるシナリオを使います。

### <a name="extracting-data-from-a-network"></a>ネットワークからのデータの抽出

このスニペットでは、[www.dotnetfoundation.org](https://www.dotnetfoundation.org) にあるホームページから HTML がダウンロードされ、HTML に文字列 ".NET" が出現する回数が数えられます。  ASP.NET MVC を使って定義されている Web コントローラー メソッドが、このタスクを実行して、数を返します。

> [!NOTE]
> 運用コードで HTML の解析の実行を計画している場合は、正規表現を使用しないでください。 代わりに解析ライブラリを使用します。

```csharp
private readonly HttpClient _httpClient = new HttpClient();

[HttpGet]
[Route("DotNetCount")]
public async Task<int> GetDotNetCountAsync()
{
    // Suspends GetDotNetCountAsync() to allow the caller (the web server)
    // to accept another request, rather than blocking on this one.
    var html = await _httpClient.GetStringAsync("https://dotnetfoundation.org");

    return Regex.Matches(html, @"\.NET").Count;
}
```

次に示すのはユニバーサル Windows アプリ用に記述された同じシナリオであり、ボタンがクリックされたら同じタスクを実行します。

```csharp
private readonly HttpClient _httpClient = new HttpClient();

private async void SeeTheDotNets_Click(object sender, RoutedEventArgs e)
{
    // Capture the task handle here so we can await the background task later.
    var getDotNetFoundationHtmlTask = _httpClient.GetStringAsync("https://www.dotnetfoundation.org");

    // Any other work on the UI thread can be done here, such as enabling a Progress Bar.
    // This is important to do here, before the "await" call, so that the user
    // sees the progress bar before execution of this method is yielded.
    NetworkProgressBar.IsEnabled = true;
    NetworkProgressBar.Visibility = Visibility.Visible;

    // The await operator suspends SeeTheDotNets_Click, returning control to its caller.
    // This is what allows the app to be responsive and not block the UI thread.
    var html = await getDotNetFoundationHtmlTask;
    int count = Regex.Matches(html, @"\.NET").Count;

    DotNetCountLabel.Text = $"Number of .NETs on dotnetfoundation.org: {count}";

    NetworkProgressBar.IsEnabled = false;
    NetworkProgressBar.Visibility = Visibility.Collapsed;
}
```

### <a name="waiting-for-multiple-tasks-to-complete"></a>複数タスクの完了の待機

複数のデータを同時に取得することが必要になる場合があります。  `Task` API には 2 つのメソッド `Task.WhenAll` と `Task.WhenAny` が含まれており、これらを使用して、複数のバックグラウンド ジョブで非ブロッキング待機を実行する非同期コードを記述できます。

次の例では、一連の `User` に対する `userId` データを取得する方法を示します。

```csharp
public async Task<User> GetUserAsync(int userId)
{
    // Code omitted:
    //
    // Given a user Id {userId}, retrieves a User object corresponding
    // to the entry in the database with {userId} as its Id.
}

public static async Task<IEnumerable<User>> GetUsersAsync(IEnumerable<int> userIds)
{
    var getUserTasks = new List<Task<User>>();

    foreach (int userId in userIds)
    {
        getUserTasks.Add(GetUserAsync(userId));
    }

    return await Task.WhenAll(getUserTasks);
}
```

LINQ を使ってもう少し簡潔に記述する別の方法を次に示します。

```csharp
public async Task<User> GetUserAsync(int userId)
{
    // Code omitted:
    //
    // Given a user Id {userId}, retrieves a User object corresponding
    // to the entry in the database with {userId} as its Id.
}

public static async Task<User[]> GetUsersAsync(IEnumerable<int> userIds)
{
    var getUserTasks = userIds.Select(id => GetUserAsync(id));
    return await Task.WhenAll(getUserTasks);
}
```

コードは少ないですが、LINQ と非同期コードを併用するときは注意が必要です。  LINQ は遅延実行を使うので、生成されたシーケンスを `foreach()` または `.ToList()` の呼び出しで強制的に反復処理させない限り、`.ToArray()` ループ内で行われた非同期呼び出しはすぐに実行されません。

## <a name="important-info-and-advice"></a>重要な情報とアドバイス

非同期プログラミングはそれほど難しくありませんが、留意することで予期しない動作を防ぐことができる細かい事柄がいくつかあります。

* `async` **メソッドの本体に** `await` **キーワードが含まれないと、何も行われません。**

これは、忘れてはならない重要なことです。  `await` が `async` メソッドの本体で使われていない場合、C# コンパイラは警告を生成しますが、コードは通常のメソッドと同様にコンパイルされて実行されます。  また、非同期メソッドに対して C# コンパイラが生成するステート マシンは何も行わないので、非常に非効率的であることにも注意してください。

* **記述するすべての非同期メソッド名のサフィックスとして、"Async" を追加する必要があります**

これは、同期メソッドと非同期メソッドの区別をより簡単にするために、.NET で使われる規則です。 コードによって明示的に呼び出されない一部のメソッド (イベント ハンドラーや Web コントローラー メソッドなど) には必ずしも当てはまらないことに注意してください。 そのようなメソッドはコードでは明示的に呼び出されないため、明示的な命名はそれほど重要ではありません。

* `async void` **はイベント ハンドラーに対してのみ使う必要があります。**

イベントには戻り値の型がないため、`async void` は非同期イベント ハンドラーの動作を可能にする唯一の方法です (したがって、`Task` と `Task<T>` を使うことはできません)。 `async void` のその他の使用はすべて TAP モデルに従わないので、使うのが難しい場合があります。以下はその例です。

* `async void` メソッドでスローされた例外を、そのメソッドの外部でキャッチすることはできません。
* `async void` メソッドをテストするのは非常に困難です。
* `async void` メソッドは、呼び出し元がそれを非同期と予期していないと、悪い副作用が発生する可能性があります。

* **LINQ 式での非同期ラムダの使用は慎重に行う必要があります**

LINQ 内のラムダ式は遅延実行を使います。つまり、予期していないときにコードが実行される可能性があります。 これにブロッキング タスクを組み込んだ場合、正しく作成されていないと、簡単にデッドロックが発生します。 さらに、このように非同期コードを入れ子にすると、コードの実行についての推論も難しくなります。 非同期と LINQ は強力ですが、併用するときは可能な限り慎重かつ明確にする必要があります。

* **タスクを待機するコードは非ブロッキング方式で作成します**

タスクが完了するのを待機するための手段として現在のスレッドをブロックすると、デッドロックおよびコンテキスト スレッドのブロックが発生する可能性があり、非常に複雑なエラー処理が必要になることがあります。 次の表では、非ブロッキング方式でタスクの待機に対処する方法について説明します。

| 方法 | これは使わない | 行う処理 |
| --- | --- | --- |
| `await` | `Task.Wait` または `Task.Result` | バックグラウンド タスクの結果の取得 |
| `await Task.WhenAny` | `Task.WaitAny` | 任意のタスクの完了の待機 |
| `await Task.WhenAll` | `Task.WaitAll` | すべてのタスクの完了の待機 |
| `await Task.Delay` | `Thread.Sleep` | 一定期間の待機 |

* **ステートフル性の低いコードを記述します**

グローバル オブジェクトの状態または特定のメソッドの実行に依存しないでください。 代わりに、メソッドの戻り値のみに依存するようにします。 なぜでしょうか。

* コードを理解しやすくなります。
* コードをテストしやすくなります。
* 非同期コードと同期コードの混在がはるかに簡単になります。
* 一般には、競合状態を完全に回避できます。
* 戻り値に依存すると、非同期コードの調整が簡単になります。
* (ボーナス) 依存関係の挿入で問題なく動作します。

推奨される目標は、完全またはほぼ完全な[参照の透過性](https://en.wikipedia.org/wiki/Referential_transparency_%28computer_science%29)をコードで実現することです。 そうすることで、予測、テスト、保守が非常に容易なコードベースになります。

## <a name="other-resources"></a>その他のリソース

* 「[非同期の詳細](../standard/async-in-depth.md)」では、タスクの動作方法について詳しく説明します。
* [Async および Await を使用した非同期プログラミング (C#)](./programming-guide/concepts/async/index.md)
* Lucian Wischik の「[Six Essential Tips for Async](https://channel9.msdn.com/Series/Three-Essential-Tips-for-Async)」(非同期に関する 6 つの重要なヒント) は、非同期プログラミングのためのすばらしいリソースです。
