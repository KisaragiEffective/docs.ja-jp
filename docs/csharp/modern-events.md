---
title: 更新された .NET Core イベント パターン
description: .NET Core イベント パターンによって旧バージョンとの互換性を提供する柔軟性を実現する方法と、非同期サブスクライバーによる安全なイベント処理を実装する方法について説明します。
ms.date: 06/20/2016
ms.technology: csharp-fundamentals
ms.assetid: 9aa627c3-3222-4094-9ca8-7e88e1071e06
ms.openlocfilehash: c8858158ede761db8a3002beb26e521880f77feb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79170437"
---
# <a name="the-updated-net-core-event-pattern"></a>更新された .NET Core イベント パターン

[前へ](event-pattern.md)

前回の記事では、最も一般的なイベント パターンについて説明しました。 .NET Core には、もっと柔軟なパターンがあります。 このバージョンでは、`EventHandler<TEventArgs>` 定義に、`TEventArgs` は `System.EventArgs` から派生したクラスでなければならないという制約がなくなりました。

これにより、柔軟性が向上し、旧バージョンとの互換性が与えられます。 柔軟性から始めましょう。 クラス System.EventArgs で `MemberwiseClone()` というメソッドが導入されました。これはオブジェクトの簡易コピーを作成するメソッドです。
そのメソッドでは、`EventArgs` から派生したあらゆるクラスのための機能を実装する目的で、リフレクションを利用する必要があります。 その機能では、特定の派生クラスでの作成が簡単になります。 つまり、System.EventArgs からの派生は設計を制限する制約ですが、他に利点はありません。
実際、`EventArgs` から派生しないように `FileFoundArgs` と `SearchDirectoryArgs` の定義を変更できます。
このプログラムはまったく同じように機能します。

さらに 1 つ変更するのであれば、`SearchDirectoryArgs` を構造体に変更することもできます。

```csharp
internal struct SearchDirectoryArgs
{
    internal string CurrentSearchDirectory { get; }
    internal int TotalDirs { get; }
    internal int CompletedDirs { get; }

    internal SearchDirectoryArgs(string dir, int totalDirs, int completedDirs) : this()
    {
        CurrentSearchDirectory = dir;
        TotalDirs = totalDirs;
        CompletedDirs = completedDirs;
    }
}
```

追加の変更は、すべてのフィールドを初期化するコンストラクターに入る前にパラメータ―なしのコンストラクターを呼び出すことです。 その追加がなければ、C# のルールは、割り当てられる前にプロパティがアクセスされていると報告するでしょう。

`FileFoundArgs` はクラス (参照型) から構造体 (値型) に変更しないでください。 キャンセルを処理するプロトコルで、イベント引数が参照で渡されることが要求されるためです。 同じ変更をした場合、ファイル検索クラスは、イベント サブスクライバーが行った変更を観察できなくなる可能性があります。 サブスクライバーごとに構造体の新しいコピーが使用され、そのコピーは、ファイル検索オブジェクトで確認されるものとは別のコピーになるでしょう。

次に、この変更を下位互換可能にする方法について考えましょう。
制約を削除しても、既存のコードには影響を与えません。 既存のイベントの引数の型は引き続き `System.EventArgs` から派生します。
`System.EventArgs` から引き続き派生する理由の 1 つが下位互換性です。 既存のイベント サブスクライバーは、従来のパターンに従ったイベントのサブスクライバーになります。

同様の論理に従うと、今、イベントの引数の型を作成すると、それには既存のコードベースのサブスクライバーが与えられないでしょう。 `System.EventArgs` から派生しない新しいイベントの型は、そのようなコードベースを壊しません。

## <a name="events-with-async-subscribers"></a>非同期サブスクライバーのあるイベント

学習する最後のパターンは、非同期コードを呼び出すイベント サブスクライバーを正しく記述する方法です。 この難題の説明は、[async と await](async.md) に関する記事にあります。 非同期メソッドには戻り値の型 void を指定できますが、指定しないことが推奨されます。 イベント サブスクライバーのコードが非同期メソッドを呼び出すとき、`async void` メソッドを作成する以外の選択肢はありません。 イベント ハンドラー シグネチャでそれが要求されます。

このような相反する指示と何とか折り合いを付け、 無難な `async void` メソッドを作成する必要があります。 実装が必要なパターンの基本は次のようになります。

```csharp
worker.StartWorking += async (sender, eventArgs) =>
{
    try
    {
        await DoWorkAsync();
    }
    catch (Exception e)
    {
        //Some form of logging.
        Console.WriteLine($"Async task failure: {e.ToString()}");
        // Consider gracefully, and quickly exiting.
    }
};
```

最初に、ハンドラーが非同期ハンドラーとしてマークされていることに注目してください。 イベント ハンドラーのデリゲート型に割り当てられているため、戻り値の型 void が与えられます。 つまり、ハンドラーに示されるパターンに従う必要があります。非同期ハンドラーのコンテキストから例外をスローすることを許可しません。 タスクを返さないため、エラーが発生してもタスクはそれを報告できません。 メソッドは非同期であるため、例外をスローできません。 (呼び出しメソッドは `async` であるため、実行を続行しています。)実際の実行時動作は、環境が異なれば各様に定義されます。 スレッドまたはスレッドを所有するプロセスを終了させたり、またはプロセスを中間状態のままにする場合があります。 これらの潜在的な結果はすべて、非常に望ましくありません。

そのため、独自の try ブロックに非同期タスクの await ステートメントをラップする必要があります。 それでタスクにエラーが発生した場合、エラーをログに記録できます。 アプリケーションがエラーから復旧できない場合、プログラムを至急、正常に終了できます。

以上が .NET イベント パターンの主要な更新でした。 使用するライブラリには以前のバージョンのサンプルがたくさんあります。 しかしながら、最新のパターンも理解してください。

このシリーズの次の記事では、設計における `delegates` と `events` の使い分けについて説明します。 概念は似ており、最良のプログラムを作るための知識をその記事で得ることができます。

[次へ](distinguish-delegates-events.md)
