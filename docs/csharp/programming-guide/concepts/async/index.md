---
title: C# での非同期プログラミング
description: C# 言語での async、await、Task、Task<T> を使用した非同期プログラミングのサポートの概要です
ms.date: 03/18/2019
ms.openlocfilehash: 4cbbff0f2c48f0ec2f8befa234ea5023465a1c5d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79169910"
---
# <a name="asynchronous-programming-with-async-and-await"></a>async および await を使用した非同期プログラミング

タスク非同期プログラミング モデル (TAP) では、非同期コードに対する抽象化が提供されます。 コードは、通常と同じようにステートメントのシーケンスとして記述します。 次のステートメントが始まる前に、各ステートメントが完了するものとして、コードを読むことができます。 これらのステートメントの一部は処理を開始し、進行中の作業を表す <xref:System.Threading.Tasks.Task> を返す可能性があるので、コンパイラではいくつかの変換が実行されます。

それがこの構文の目的です。コードは、順番に実行されるステートメントのように書かれていますが、外部リソースの割り当てとタスク完了のタイミングに基づいて、はるかに複雑な順序で実行されます。 それは、人が非同期のタスクを含むプロセスの指示を与えるのと似ています。 この記事では、朝食を作る手順を例として使用し、`async` キーワードと `await` キーワードによって、一連の非同期命令を含むコードがどのように理解しやすくなるのかを見ていきます。 朝食を作る方法について説明する次の一覧のような手順を作成します。

1. コーヒーをカップに注ぐ。
1. フライパンを熱し、卵を 2 個焼く。
1. ベーコンを 3 切れ焼く。
1. パンを 2 枚焼く。
1. トーストにバターとジャムを塗る。
1. オレンジ ジュースをグラスに注ぐ。

料理の経験があれば、これらの手順を**非同期的に**実行するでしょう。 卵用のフライパンを熱し始めてから、ベーコンを始めます。 トースターにパンを入れたら、卵を焼き始めます。 プロセスの各ステップで、あるタスクを開始したら、準備ができているタスクに注意を向けます。

朝食の準備は、並列ではない非同期作業のよい例です。 1 人 (つまり 1 つのスレッド) で、これらすべてのタスクを処理できます。 朝食の例を続けると、1 人で、最初の作業が完了する前に次の作業を開始して、非同期に朝食を作ることができます。 調理はそれを監視している人がいるかどうかに関係なく進行します。 卵用のフライパンを熱し始めたらすぐに、ベーコンを焼き始めることができます。 ベーコンを焼き始めたら、パンをトースターに入れることができます。

並列アルゴリズムの場合は、複数の料理人 (つまりスレッド) が必要です。 1 人は卵を焼き、1 人はベーコンを焼く、といった具合です。 それぞれは、1 つのタスクだけに集中します。 各料理人 (つまりスレッド) は、ベーコンを裏返すことができるようになるまで、またはトーストが飛び出すまで待つ間は、同期的にブロックされます。

それでは、これと同じ命令が C# ステートメントとして書かれている場合を考えてみましょう。

[!code-csharp[SynchronousBreakfast](~/samples/snippets/csharp/tour-of-async/AsyncBreakfast-starter/Program.cs#Main)]

コンピューターによって、人と同じように命令が解釈されることはありません。 コンピューターでは、作業が完了するまで各ステートメントはブロックされ、完了すると次のステートメントに進みます。 それではおいしい朝食はできません。 後のタスクは、前のタスクが完了するまで開始されません。 朝食の支度でこんなことをしていると、ずっと長い時間がかかり、提供される前に冷めてしまう料理もあるでしょう。

上のような手順をコンピューターに非同期に実行させたい場合は、非同期のコードを書く必要があります。

今日書いているプログラムでは、このようなことを考慮することが重要です。 クライアント プログラムを書くときは、ユーザー入力に対する UI の応答性をよくする必要です。 Web からデータをダウンロードしている間、電話がフリーズしたようになるアプリケーションではいけません。 サーバーのプログラムを書くときは、スレッドがブロックされては困ります。 それらのスレッドは、他の要求を処理しているかもしれません。 非同期の代替手段が存在する場合に同期コードを使用すると、低コストでスケールアウトする能力が低下してしまいます。 ブロックされたスレッドに料金を支払うことになります。

よくできた最新のアプリケーションには、非同期コードが必要です。 言語のサポートがない場合、非同期コードを書くには、コールバック、完了イベント、またはコードの本来の意図をわかりにくくしてしまうような他の手段が必要でした。 同期コードの利点は、理解しやすいことです。 アクションが 1 ステップずつ行われれば、スキャンも理解も容易です。 従来の非同期モデルでは、開発者はコードの基本的な処理ではなく、コードの非同期的性質に注目することを余儀なくされました。

## <a name="dont-block-await-instead"></a>ブロックするのではなく待機する

上記のコードは、非同期的な操作を実行するために同期的なコードを作成するという、不適切な手法の例です。 このようなコードを書くと、それを実行するスレッドは、他の作業を行うことができません。 何らかのタスクの処理中は割り込まれません。 パンを入れた後でトースターをじっと見詰めているようなものです。 トーストが飛び出すまで、誰かから話し掛けられても無視するでしょう。

タスクの実行中にスレッドをブロックしないように、このコードを更新することから始めましょう。 `await` キーワードを使用すると、ブロックしない方法でタスクを開始し、タスクが完了したら実行を継続できます。 朝食作成コードの簡単な非同期バージョンは、次のスニペットのようになります。

[!code-csharp[SimpleAsyncBreakfast](~/samples/snippets/csharp/tour-of-async/AsyncBreakfast-V2/Program.cs#Main)]

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
Task<Egg> eggsTask = FryEggs(2);
Egg eggs = await eggsTask;
Console.WriteLine("eggs are ready");
Task<Bacon> baconTask = FryBacon(3);
Bacon bacon = await baconTask;
Console.WriteLine("bacon is ready");
Task<Toast> toastTask = ToastBread(2);
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
Task<Egg> eggsTask = FryEggs(2);
Task<Bacon> baconTask = FryBacon(3);
Task<Toast> toastTask = ToastBread(2);
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

上のコードの方がより適切に動作します。 すべての非同期タスクを一度に開始します。 結果が必要なときにのみ、各タスクを待機します。 上記のコードは、異なるマイクロサービスに要求を行って 1 つのページに結果をまとめる Web アプリケーションのコードに似ているかもしれません。 すべての要求をすぐに行った後、すべてのタスクを `await` して、Web ページを作成します。

## <a name="composition-with-tasks"></a>タスクの合成

 トーストを除き、朝食のすべての準備が同時に整います。 トーストの作成は、非同期操作 (パンを焼く) と同期操作 (バターとジャムを塗る) の合成です。 このコードの更新では、重要な概念が示されています。

> [!IMPORTANT]
> 非同期操作とその後の同期操作の合成は、非同期操作です。 言い方を変えれば、操作の一部が非同期である場合、操作全体が非同期になります。

上記のコードでは、<xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.Task%601> オブジェクトを使用して実行中のタスクを保持できることを示しました。 結果を使用する前に、各タスクを `await` します。 次のステップは、他の作業の組み合わせを表すメソッドを作成することです。 朝食を提供するには、バターとジャムを塗る前にパンを焼くタスクを待機します。 次のコードでその作業を表すことができます。

[!code-csharp[ComposeToastTask](~/samples/snippets/csharp/tour-of-async/AsyncBreakfast-V3/Program.cs#ComposeToastTask)]

上のメソッドのシグニチャには `async` 修飾子が付いています。 それにより、コンパイラに対して、このメソッドに `await` ステートメントが含まれることが通知されます。それには非同期操作が含まれています。 このメソッドは、パンを焼いてからバターとジャムを塗るタスクを表します。 このメソッドからは、これら 3 つの操作の合成を表す <xref:System.Threading.Tasks.Task%601> が返されます。 これで、コードのメイン ブロックは次のようになります。

[!code-csharp[StartConcurrentTasks](~/samples/snippets/csharp/tour-of-async/AsyncBreakfast-V3/Program.cs#Main)]

この変更では、非同期コードを使用するための重要な手法が示されています。 タスクを返す新しいメソッドに操作を分離することで、タスクを合成します。 そのタスクを待機するタイミングを選択できます。 他のタスクを同時に開始できます。

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

[!code-csharp[AwaitAnyTask](~/samples/snippets/csharp/tour-of-async/AsyncBreakfast-final/Program.cs#AwaitAnyTask)]

すべてを変更した後、最終バージョンの `Main` は次のコードのようになります。

[!code-csharp[Final](~/samples/snippets/csharp/tour-of-async/AsyncBreakfast-final/Program.cs#Main)]

この最後のコードは非同期です。 人が朝食を作る方法が、より正確に反映されています。 上のコードを、この記事の最初のコード サンプルと比較してください。 中核となるアクションはコードを読むと明らかです。 このコードは、この記事の最初にある朝食の作成手順と同じように読むことができます。 `async` および `await` の言語機能により、手順書に従うためにすべての人が行う変換が提供されます。つまり、可能になったらタスクを開始し、タスク完了の待機をブロックしないようにします。
