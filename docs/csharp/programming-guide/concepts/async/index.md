---
title: C# での非同期プログラミング
description: C# 言語での async、await、Task、Task<T> を使用した非同期プログラミングのサポートの概要です
ms.date: 06/04/2020
ms.openlocfilehash: ffc2289f3b5abfe3865e1a096ee91e2e649a6427
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2021
ms.locfileid: "103624241"
---
# <a name="asynchronous-programming-with-async-and-await"></a>async および await を使用した非同期プログラミング

[タスク非同期プログラミング モデル (TAP)](task-asynchronous-programming-model.md) では、非同期コードに対する抽象化が提供されます。 コードは、通常と同じようにステートメントのシーケンスとして記述します。 次のステートメントが始まる前に、各ステートメントが完了するものとして、コードを読むことができます。 これらのステートメントの一部は処理を開始し、進行中の作業を表す <xref:System.Threading.Tasks.Task> を返す可能性があるので、コンパイラでは多数の変換が実行されます。

それがこの構文の目的です。コードは、順番に実行されるステートメントのように書かれていますが、外部リソースの割り当てとタスク完了のタイミングに基づいて、はるかに複雑な順序で実行されます。 それは、人が非同期のタスクを含むプロセスの指示を与えるのと似ています。 この記事では、朝食を作る手順を例として使用し、`async` キーワードと `await` キーワードによって、一連の非同期命令を含むコードがどのように理解しやすくなるのかを見ていきます。 朝食を作る方法について説明する次の一覧のような手順を作成します。

1. コーヒーをカップに注ぐ。
1. フライパンを熱し、卵を 2 個焼く。
1. ベーコンを 3 切れ焼く。
1. パンを 2 枚焼く。
1. トーストにバターとジャムを塗る。
1. オレンジ ジュースをグラスに注ぐ。

料理の経験があれば、これらの手順を **非同期的に** 実行するでしょう。 卵用のフライパンを熱し始めてから、ベーコンを始めます。 トースターにパンを入れたら、卵を焼き始めます。 プロセスの各ステップで、あるタスクを開始したら、準備ができているタスクに注意を向けます。

朝食の準備は、並列ではない非同期作業のよい例です。 1 人 (つまり 1 つのスレッド) で、これらすべてのタスクを処理できます。 朝食の例を続けると、1 人で、最初の作業が完了する前に次の作業を開始して、非同期に朝食を作ることができます。 調理はそれを監視している人がいるかどうかに関係なく進行します。 卵用のフライパンを熱し始めたらすぐに、ベーコンを焼き始めることができます。 ベーコンを焼き始めたら、パンをトースターに入れることができます。

並列アルゴリズムの場合は、複数の料理人 (つまりスレッド) が必要です。 1 人は卵を焼き、1 人はベーコンを焼く、といった具合です。 それぞれは、1 つのタスクだけに集中します。 各料理人 (つまりスレッド) は、ベーコンを裏返すことができるようになるまで、またはトーストが飛び出すまで待つ間は、同期的にブロックされます。

それでは、これと同じ命令が C# ステートメントとして書かれている場合を考えてみましょう。

:::code language="csharp" source="snippets/index/AsyncBreakfast-starter/Program.cs" highlight="8-27":::

:::image type="content" source="media/synchronous-breakfast.png" alt-text="同期的な朝食":::

同期的に準備された朝食は、合計が個々のタスクの合計であるため、約 30 分かかりました。

> [!NOTE]
> `Coffee`、`Egg`、`Bacon`、`Toast`、および `Juice` クラスは空です。 これらは、デモンストレーション目的の単なるマーカー クラスであり、プロパティは含まれず、その他の目的はありません。

コンピューターによって、人と同じように命令が解釈されることはありません。 コンピューターでは、作業が完了するまで各ステートメントはブロックされ、完了すると次のステートメントに進みます。 それではおいしい朝食はできません。 後のタスクは、前のタスクが完了するまで開始されません。 朝食の支度でこんなことをしていると、ずっと長い時間がかかり、提供される前に冷めてしまう料理もあるでしょう。

上のような手順をコンピューターに非同期に実行させたい場合は、非同期のコードを書く必要があります。

今日書いているプログラムでは、このようなことを考慮することが重要です。 クライアント プログラムを書くときは、ユーザー入力に対する UI の応答性をよくする必要です。 Web からデータをダウンロードしている間、電話がフリーズしたようになるアプリケーションではいけません。 サーバーのプログラムを書くときは、スレッドがブロックされては困ります。 それらのスレッドは、他の要求を処理しているかもしれません。 非同期の代替手段が存在する場合に同期コードを使用すると、低コストでスケールアウトする能力が低下してしまいます。 ブロックされたスレッドに料金を支払うことになります。

よくできた最新のアプリケーションには、非同期コードが必要です。 言語のサポートがない場合、非同期コードを書くには、コールバック、完了イベント、またはコードの本来の意図をわかりにくくしてしまうような他の手段が必要でした。 同期コードの利点は、段階的なアクションによりスキャンと理解が容易なことです。 従来の非同期モデルでは、開発者はコードの基本的な処理ではなく、コードの非同期的性質に注目することを余儀なくされました。

## <a name="dont-block-await-instead"></a>ブロックするのではなく待機する

上記のコードは、非同期的な操作を実行するために同期的なコードを作成するという、不適切な手法の例です。 このようなコードを書くと、それを実行するスレッドは、他の作業を行うことができません。 何らかのタスクの処理中は割り込まれません。 パンを入れた後でトースターをじっと見詰めているようなものです。 トーストが飛び出すまで、誰かから話し掛けられても無視するでしょう。

タスクの実行中にスレッドをブロックしないように、このコードを更新することから始めましょう。 `await` キーワードを使用すると、ブロックしない方法でタスクを開始し、タスクが完了したら実行を継続できます。 朝食作成コードの簡単な非同期バージョンは、次のスニペットのようになります。

:::code language="csharp" source="snippets/index/AsyncBreakfast-V2/Program.cs" ID="SnippetMain":::

> [!IMPORTANT]
> 合計経過時間は、初期の同期バージョンとほぼ同じです。 このコードでは、非同期プログラミングの主要な機能のいくつかがまだ利用されています。

> [!TIP]
> `FryEggsAsync`、`FryBaconAsync`、`ToastBreadAsync` のメソッド本体がすべて更新され、それぞれ `Task<Egg>`、`Task<Bacon>`、および `Task<Toast>` が返されます。 メソッドは、元のバージョンから名前が変更され、"Async" サフィックスが含まれるようになっています。 これらの実装は、この記事で後述する[最終バージョン](#final-version)の一部として表示されます。

このコードでは、卵またはベーコンを調理している間、ブロックすることはありません。 ただし、このコードは他のタスクを開始しません。 まだ、トースターにトーストを入れた後、トーストが飛び出すまで眺めています。 ただし、少なくとも誰かが注意を引こうとしたら反応するようにはなります。 複数の注文を受けるレストランでは、料理人は最初の朝食を作っている間に、別の朝食を作り始めます。

この場合、開始してまだ完了していないタスクを待っている間、朝食作業スレッドはブロックされません。 一部のアプリケーションでは、必要な変更はこれですべてです。 GUI アプリケーションは、この変更だけで引き続きユーザーに応答します。 ただし、このシナリオでは、これだけでは十分ではありません。 各コンポーネントのタスクを順番に実行したくはありません。 前のタスクの完了を待機する前に、各コンポーネントのタスクを開始した方がよさそうです。

## <a name="start-tasks-concurrently"></a>タスクを同時に開始する

多くのシナリオでは、複数の独立したタスクをすぐに開始する必要があります。 そうすれば、各タスクが完了したら、準備のできている他のタスクを続行できます。 朝食でたとえるなら、もっと早く朝食を準備できます。 また、すべての作業がほぼ同じタイミングで終了します。 できたての朝食を食べられます。

<xref:System.Threading.Tasks.Task?displayProperty=nameWithType> および関連する型を使用して、進行中のタスクについて判断できます。 それを使用すると、実際の朝食作成方法にさらに近いコードを記述できます。 卵、ベーコン、トーストの調理を同時に始めます。 それぞれでアクションが必要になったら、そのタスクに注意を向け、次のアクションを行ってから、注意が必要な他のタスクを待ちます。

タスクを開始し、作業を表す <xref:System.Threading.Tasks.Task> オブジェクトを保持します。 各タスクを `await` (待機) してから、結果を処理します。

朝食コードに対してこれらの変更を行いましょう。 最初のステップは、操作のタスクを開始するときに、それらを待機するのではなく、保存することです。

```csharp
Coffee cup = PourCoffee();
Console.WriteLine("coffee is ready");

Task<Egg> eggsTask = FryEggsAsync(2);
Egg eggs = await eggsTask;
Console.WriteLine("eggs are ready");

Task<Bacon> baconTask = FryBaconAsync(3);
Bacon bacon = await baconTask;
Console.WriteLine("bacon is ready");

Task<Toast> toastTask = ToastBreadAsync(2);
Toast toast = await toastTask;
ApplyButter(toast);
ApplyJam(toast);
Console.WriteLine("toast is ready");

Juice oj = PourOJ();
Console.WriteLine("oj is ready");
Console.WriteLine("Breakfast is ready!");
```

次に、ベーコンと卵の `await` ステートメントを、メソッドの最後の朝食提供前に移動します。

```csharp
Coffee cup = PourCoffee();
Console.WriteLine("coffee is ready");

Task<Egg> eggsTask = FryEggsAsync(2);
Task<Bacon> baconTask = FryBaconAsync(3);
Task<Toast> toastTask = ToastBreadAsync(2);

Toast toast = await toastTask;
ApplyButter(toast);
ApplyJam(toast);
Console.WriteLine("toast is ready");
Juice oj = PourOJ();
Console.WriteLine("oj is ready");

Egg eggs = await eggsTask;
Console.WriteLine("eggs are ready");
Bacon bacon = await baconTask;
Console.WriteLine("bacon is ready");

Console.WriteLine("Breakfast is ready!");
```

:::image type="content" source="media/asynchronous-breakfast.png" alt-text="非同期的な朝食":::

非同期的に準備された朝食は、約 20 分かかりました。この時間節約は、いくつかのタスクを同時に実行できたことが理由です。

上のコードの方がより適切に動作します。 すべての非同期タスクを一度に開始します。 結果が必要なときにのみ、各タスクを待機します。 上記のコードは、異なるマイクロサービスに要求を行って 1 つのページに結果をまとめる Web アプリケーションのコードに似ているかもしれません。 すべての要求をすぐに行った後、すべてのタスクを `await` して、Web ページを作成します。

## <a name="composition-with-tasks"></a>タスクの合成

 トーストを除き、朝食のすべての準備が同時に整います。 トーストの作成は、非同期操作 (パンを焼く) と同期操作 (バターとジャムを塗る) の合成です。 このコードの更新では、重要な概念が示されています。

> [!IMPORTANT]
> 非同期操作とその後の同期操作の合成は、非同期操作です。 言い方を変えれば、操作の一部が非同期である場合、操作全体が非同期になります。

上記のコードでは、<xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.Task%601> オブジェクトを使用して実行中のタスクを保持できることを示しました。 結果を使用する前に、各タスクを `await` します。 次のステップは、他の作業の組み合わせを表すメソッドを作成することです。 朝食を提供するには、バターとジャムを塗る前にパンを焼くタスクを待機します。 次のコードでその作業を表すことができます。

:::code language="csharp" source="snippets/index/AsyncBreakfast-V3/Program.cs" ID="SnippetComposeToastTask":::

上のメソッドのシグニチャには `async` 修飾子が付いています。 それにより、コンパイラに対して、このメソッドに `await` ステートメントが含まれることが通知されます。それには非同期操作が含まれています。 このメソッドは、パンを焼いてからバターとジャムを塗るタスクを表します。 このメソッドからは、これら 3 つの操作の合成を表す <xref:System.Threading.Tasks.Task%601> が返されます。 これで、コードのメイン ブロックは次のようになります。

:::code language="csharp" source="snippets/index/AsyncBreakfast-V3/Program.cs" ID="SnippetMain":::

この変更では、非同期コードを使用するための重要な手法が示されています。 タスクを返す新しいメソッドに操作を分離することで、タスクを合成します。 そのタスクを待機するタイミングを選択できます。 他のタスクを同時に開始できます。

## <a name="asynchronous-exceptions"></a>非同期例外

この時点までは、これらのすべてのタスクは正常に完了したものと想定しています。 非同期メソッドでも、対応する同期メソッドと同様に、例外がスローされます。 例外とエラー処理の非同期サポートでも、一般的な非同期サポートと同じ目標を達成する必要があり、一連の同期ステートメントと同様に読み取るコードを記述する必要があります。 正常に完了できない場合、タスクによって例外がスローされます。 クライアント コードでは、開始されたタスクが `awaited` のときに、これらの例外をキャッチすることができます。 たとえば、トーストの作成中にトースターが燃えだしたものとします。 これをシミュレートするには、次のコードと一致するように `ToastBreadAsync` メソッドを変更します。

```csharp
private static async Task<Toast> ToastBreadAsync(int slices)
{
    for (int slice = 0; slice < slices; slice++)
    {
        Console.WriteLine("Putting a slice of bread in the toaster");
    }
    Console.WriteLine("Start toasting...");
    await Task.Delay(2000);
    Console.WriteLine("Fire! Toast is ruined!");
    throw new InvalidOperationException("The toaster is on fire");
    await Task.Delay(1000);
    Console.WriteLine("Remove toast from toaster");

    return new Toast();
}
```

> [!NOTE]
> 上のコードをコンパイルすると、到達できないコードに関する警告が表示されます。 トースターが燃えだしたら、操作は正常に続行されないため、それは意図的なものです。

これらの変更を行った後、アプリケーションを実行すると、次のテキストのような出力が表示されます。

```console
Pouring coffee
coffee is ready
Warming the egg pan...
putting 3 slices of bacon in the pan
cooking first side of bacon...
Putting a slice of bread in the toaster
Putting a slice of bread in the toaster
Start toasting...
Fire! Toast is ruined!
flipping a slice of bacon
flipping a slice of bacon
flipping a slice of bacon
cooking the second side of bacon...
cracking 2 eggs
cooking the eggs ...
Put bacon on plate
Put eggs on plate
eggs are ready
bacon is ready
Unhandled exception. System.InvalidOperationException: The toaster is on fire
   at AsyncBreakfast.Program.ToastBreadAsync(Int32 slices) in Program.cs:line 65
   at AsyncBreakfast.Program.MakeToastWithButterAndJamAsync(Int32 number) in Program.cs:line 36
   at AsyncBreakfast.Program.Main(String[] args) in Program.cs:line 24
   at AsyncBreakfast.Program.<Main>(String[] args)
```

トースターが燃えだしてから、例外が検出されるまでの間に、多くのタスクが完了することに注意してください。 非同期に実行されるタスクが例外をスローすると、そのタスクは "***エラー***" になります。 タスク オブジェクトは、<xref:System.Threading.Tasks.Task.Exception?displayProperty=nameWithType> プロパティでスローされた例外を保持します。 エラーが発生したタスクは、待機しているときに例外をスローします。

理解すべき重要なメカニズムが 2 つあります。エラーが発生したタスクに例外が格納される方法と、コードでエラーが発生したタスクを待機しているときに、例外がパッケージ解除されて再スローされる方法です。

非同期的に実行されているコードで例外がスローされると、その例外は `Task` に格納されます。 非同期処理中に複数の例外がスローされる可能性があるため、<xref:System.Threading.Tasks.Task.Exception?displayProperty=nameWithType> プロパティは <xref:System.AggregateException?displayProperty=nameWithType> です。 スローされた例外は、<xref:System.AggregateException.InnerExceptions?displayProperty=nameWithType> コレクションに追加されます。 その `Exception` プロパティが null の場合は、新しい `AggregateException` が作成され、スローされた例外がコレクションの最初の項目になります。

エラーが発生したタスクの最も一般的なシナリオは、`Exception` プロパティに含まれる例外が 1 つだけの場合です。 エラーが発生したタスクをコードで `awaits` する場合、<xref:System.AggregateException.InnerExceptions?displayProperty=nameWithType> コレクション内の最初の例外が再スローされます。 そのため、この例の出力では、`AggregateException` ではなく `InvalidOperationException` が表示されます。 最初の内部例外を抽出することにより、非同期メソッドの処理は、対応する同期メソッドの処理と、可能な限り似たものになります。 シナリオで複数の例外が生成される可能性がある場合は、コード内の `Exception` プロパティを調べることができます。

開始する前に、`ToastBreadAsync` メソッドでこれらの 2 行をコメントアウトします。 別の火災を発生させたくはありません。

```csharp
Console.WriteLine("Fire! Toast is ruined!");
throw new InvalidOperationException("The toaster is on fire");
```

## <a name="await-tasks-efficiently"></a>タスクを効率的に待機する

上記のコードの最後にある一連の `await` ステートメントは、`Task` クラスのメソッドを使用することによって改良できます。 それらの API の 1 つは <xref:System.Threading.Tasks.Task.WhenAll%2A> であり、これにより次のコードで示すように、引数リストのすべてのタスクが完了すると完了する <xref:System.Threading.Tasks.Task> が返されます。

```csharp
await Task.WhenAll(eggsTask, baconTask, toastTask);
Console.WriteLine("eggs are ready");
Console.WriteLine("bacon is ready");
Console.WriteLine("toast is ready");
Console.WriteLine("Breakfast is ready!");
```

もう 1 つのオプションは、<xref:System.Threading.Tasks.Task.WhenAny%2A> を使用することです。これは、引数のいずれかが完了すると完了する `Task<Task>` が返されます。 返されたタスクを待機して、既に完了したことを把握できます。 次のコードでは、<xref:System.Threading.Tasks.Task.WhenAny%2A> を使用して最初のタスクが完了するのを待った後、その結果を処理する方法が示されています。 完了したタスクの結果を処理した後、`WhenAny` に渡すタスクの一覧からその完了したタスクを削除します。

```csharp
var breakfastTasks = new List<Task> { eggsTask, baconTask, toastTask };
while (breakfastTasks.Count > 0)
{
    Task finishedTask = await Task.WhenAny(breakfastTasks);
    if (finishedTask == eggsTask)
    {
        Console.WriteLine("eggs are ready");
    }
    else if (finishedTask == baconTask)
    {
        Console.WriteLine("bacon is ready");
    }
    else if (finishedTask == toastTask)
    {
        Console.WriteLine("toast is ready");
    }
    breakfastTasks.Remove(finishedTask);
}
```

すべてを変更した後、コードの最終バージョンは <a id="final-version"></a> のようになります。
:::code language="csharp" source="snippets/index/AsyncBreakfast-final/Program.cs" highlight="9-40":::

:::image type="content" source="media/whenany-async-breakfast.png" alt-text="非同期的な朝食がある場合":::

非同期に準備された朝食の最終バージョンは、約 15 分かかりました。これは、いくつかのタスクが同時に実行され、コードで一度に複数のタスクを監視し、必要なときにのみアクションを実行するようになったためです。

この最後のコードは非同期です。 人が朝食を作る方法が、より正確に反映されています。 上のコードを、この記事の最初のコード サンプルと比較してください。 中核となるアクションはコードを読むと明らかです。 このコードは、この記事の最初にある朝食の作成手順と同じように読むことができます。 `async` および `await` の言語機能により、手順書に従うためにすべての人が行う変換が提供されます。つまり、可能になったらタスクを開始し、タスク完了の待機をブロックしないようにします。

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [非同期プログラムの実際のシナリオを調べる](../../../async.md)
