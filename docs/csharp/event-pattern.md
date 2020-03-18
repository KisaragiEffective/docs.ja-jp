---
title: 標準的な .NET イベント パターン
description: .NET イベント パターンに関する情報を提供するほか、標準的なイベント ソースを作成し、標準的なイベントをコードでサブスクライブおよび処理する方法について説明します。
ms.date: 06/20/2016
ms.technology: csharp-fundamentals
ms.assetid: 8a3133d6-4ef2-46f9-9c8d-a8ea8898e4c9
ms.openlocfilehash: dec516767e43a6bf4edfa555e34f3adcc21a46e3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79146142"
---
# <a name="standard-net-event-patterns"></a>標準的な .NET イベント パターン

[前へ](events-overview.md)

一般的に、.NET イベントはいくつかの既知のパターンに従います。 これらのパターンを標準化すれば、開発者はこうした標準的なパターンの知識を活用し、あらゆる .NET イベント プログラムに適用することができます。

これらの標準的なパターンを理解することにより、標準的なイベント ソースを作成し、標準的なイベントをコードでサブスクライブし処理するために必要な知識を習得してください。

## <a name="event-delegate-signatures"></a>イベント デリゲートのシグネチャ

.NET イベント デリゲートの標準的なシグネチャは、次のとおりです。

```csharp
void OnEventRaised(object sender, EventArgs args);
```

戻り値の型は void です。 イベントは、デリゲートを基本とするマルチキャスト デリゲートです。 このため、任意のイベント ソースに対して複数のサブスクライバーをサポートします。 メソッドが返す単一の戻り値が複数のイベント サブスクライバーに拡張されることはありません。 イベント発生後にイベントソースはどの戻り値を受け取るのでしょうか。 この記事の後半で、イベント サブスクライバーをサポートするイベント プロトコルの作成方法を説明します。イベント サブスクライバーはイベント ソースに情報をレポートします。

引数リストには、2 つの引数、すなわち送信元とイベント引数が含まれます。 `sender` のコンパイル時型は `System.Object` です。常に正しいより確実な派生型が存在する可能性はありますが、 慣例に従って、`object` を使用します。

2 番目の引数は、通常は `System.EventArgs` から派生した型です。 ([次のセクション](modern-events.md)で、この規則が適用されなくなっていることを説明します。)イベント型に引数を追加する必要がない場合でも、両方の引数を使用します。
特殊な値である `EventArgs.Empty` は、イベントにその他の情報が含まれていないことを示すために使用します。

これから、パターンに従うディレクトリ、またはそのサブディレクトリのいずれかでファイルを一覧表示するクラスをビルドしていきます。 このコンポーネントでは、パターンに一致することが確認されたファイルごとにイベントを発生させます。

イベント モデルを使用すると、設計上の利点が得られます。 要求されたファイルを検出すると、異なるアクションを実行する複数のイベント リスナーを作成できます。 別のリスナーと組み合わせることで、より堅牢なアルゴリズムを作成できます。

次に示すのは、要求されたファイルを検索するイベント引数を最初に宣言する部分です。

[!code-csharp[EventArgs](../../samples/snippets/csharp/events/Program.cs#EventArgsV1 "Define event arguments")]

この型はデータのみの小さな型のように見えますが、規則に従って、参照 (`class`) 型にする必要があります。 つまり、引数オブジェクトは参照によって渡され、データが更新されると、すべてのサブスクライバーから参照されます。 最初のバージョンは、変更不可のオブジェクトです。 イベント引数の型のプロパティを変更不可に設定しておいた方がよいでしょう。 このようにすれば、別のサブスクライバーが値を確認する前に、いずれかのサブスクライバーが値を変更することがありません。 (下記に説明するとおり、これには例外があります。)  

次に、FileSearcher クラスでイベント宣言を作成する必要があります。 `EventHandler<T>` 型を活用すれば、もう 1 つ別の型を定義する必要はありません。 ジェネリックの特殊化を使用するだけで済みます。

パターンに一致するファイルを検索し、一致が検出されると、適切なイベントを発生する FileSearcher クラスを記述します。

[!code-csharp[FileSearcher](../../samples/snippets/csharp/events/Program.cs#FileSearcherV1 "Create the initial file searcher")]

## <a name="defining-and-raising-field-like-events"></a>フィールドのように使用するイベントの定義と発生

クラスにイベントを追加する最も簡単な方法は、上記の例のように、そのイベントをパブリック フィールドとして宣言することです。

[!code-csharp[DeclareEvent](../../samples/snippets/csharp/events/Program.cs#DeclareEvent "Declare the file found event")]

これはパブリック フィールドを宣言しているように見えるため、不適切なオブジェクト指向プラクティスと考えられるかもしれません。 プロパティ、またはメソッドでデータ アクセスを保護したくなるところです。 これは一見すると不適切なプラクティスのように見えるかもしれませんが、安全な方法でしかイベント オブジェクトにアクセスできないように、コンパイラによって生成されたコードでは、ラッパーを作成します。 フィールドのように使用するイベントで使用できる唯一の操作は、ハンドラーの追加です。

[!code-csharp[DeclareEventHandler](../../samples/snippets/csharp/events/Program.cs#DeclareEventHandler "Declare the file found event handler")]

それと、ハンドラーの削除です。

[!code-csharp[RemoveEventHandler](../../samples/snippets/csharp/events/Program.cs#RemoveHandler "Remove the event handler")]

このハンドラーにはローカル変数があります。 ラムダの本体を使用する場合、削除は正しく動作しません。 ラムダ本体はデリゲートの別のインスタンスであるため、自動的に何か実行することはありません。

クラスの外側にあるコードはイベントを発生させることも、その他の操作を実行することもできません。

## <a name="returning-values-from-event-subscribers"></a>イベント サブスクライバーからの戻り値

この単純なバージョンは正常に動作しています。 別の機能を追加してみましょう。キャンセル機能です。

検出されたイベントを発生させるとき、このファイルが要求された最後のファイルである場合、リスナーはその後の処理を停止する必要があります。

イベント ハンドラーは値は返さないため、別の方法で値を伝達する必要があります。 標準的なイベント パターンでは、イベント サブスクライバーがキャンセルを伝達するために使用するフィールドを含めるために、EventArgs オブジェクトを使用します。

使用できるパターンには 2 種類あり、それらは Cancel コントラクトのセマンティクスに基づいています。 どちらの場合も、検出されたファイル イベントの EventArguments にブール型フィールドを追加します。

1 つのパターンでは、任意のサブスクライバーが単独で操作をキャンセルできます。
このパターンでは、新しいフィールドは `false` に初期化されます。 どのサブスクライバーでもこのフィールドを `true` に変更できます。 すべてのサブスクライバーがイベントの発生を確認すると、FileSearcher コンポーネントがブール値を検証し、アクションを実行します。

2 つ目のパターンでは、すべてのサブスクライバーが操作のキャンセルを認める場合に限り、操作がキャンセルされます。 このパターンでは、新しいフィールドは操作のキャンセルを示すように初期化され、任意のサブスクライバーが操作を続行するように変更できます。
すべてのサブスクライバーがイベントの発生を確認すると、FileSearcher コンポーネントがブール値を検証し、アクションを実行します。 このパターンには、手順がもう 1 つあり、コンポーネントは、いずれかのサブスクライバーがイベントを確認したか把握する必要があります。 サブスクライバーが存在しない場合、フィールドが示すキャンセルは誤りとなります。

次に、このサンプルの最初のバージョンを実装します。 `FileFoundArgs` 型に `CancelRequested` という名前のブール型フィールドを追加する必要があります。

[!code-csharp[EventArgs](../../samples/snippets/csharp/events/Program.cs#EventArgs "Update event arguments")]

誤ってキャンセルすることがないように、この新しいフィールドは `false` (ブール型フィールドの既定値) に自動的に初期化されます。 コンポーネントのもう 1 つの変更点は、イベントが発生したあと、フラグをチェックして、いずれかのサブスクライバーがキャンセルを要求しているか確認することです。

```csharp
public void List(string directory, string searchPattern)
{
    foreach (var file in Directory.EnumerateFiles(directory, searchPattern))
    {
        var args = new FileFoundArgs(file);
        FileFound?.Invoke(this, args);
        if (args.CancelRequested)
            break;
    }
}
```

このパターンの利点の 1 つは、この変更が重大な変更ではないことです。
いずれのサブスクライバーも前にキャンセルを要求していなければ、引き続き同じ状態が維持されます。 サブスクライバーが新しいキャンセル プロトコルをサポートしない限り、どのサブスクライバーのコードも更新する必要がありません。 非常にゆるやかな結合となっています。

次に、最初の実行可能ファイルが検索されると、キャンセルを要求するように、サブスクライバーを更新します。

```csharp
EventHandler<FileFoundArgs> onFileFound = (sender, eventArgs) =>
{
    Console.WriteLine(eventArgs.FoundFile);
    eventArgs.CancelRequested = true;
};
```

## <a name="adding-another-event-declaration"></a>別のイベント宣言の追加

ここで、1 つの機能を追加し、イベント用の他の慣用句について説明します。 すべてのサブディレクトリを走査してファイルを検索する `Search` メソッドのオーバーロードを追加します。

多くのサブディレクトリを持つディレクトリでは、これは時間のかかる操作となる場合があります。 次に、新しいディレクトリの検索が開始するたびに発生するイベントを追加します。 このイベントによって、サブスクライバーは進行状況を追跡し、進行状況の更新をユーザーに伝えます。 これまでに作成したすべてのサンプルは、パブリックです。 このイベントを内部イベントにします。 つまり、引数に使用される型を内部型にすることもできるということです。

最初に、新しいディレクトリと進行状況をレポートする新しい EventArgs 派生クラスを作成します。

[!code-csharp[DirEventArgs](../../samples/snippets/csharp/events/Program.cs#SearchDirEventArgs "Define search directory event arguments")]

ここでも、イベント引数に変更不可の参照型を使用することをお勧めします。

次に、イベントを定義します。 今回は、別の構文を使用します。 フィールドの構文を使用する以外に、明示的にプロパティを作成し、ハンドラーを追加、削除することができます。 このサンプルでは、ハンドラーにコードを追加する必要はありませんが、次に示したのはそれを作成する方法です。

[!code-csharp[Declare event with add and remove handlers](../../samples/snippets/csharp/events/Program.cs#DeclareSearchEvent "Declare the event with add and remove handlers")]

ここで作成するコードは、多くの点で、コンパイラがフィールドのイベントを定義するために作成した先ほどのコードとよく似ています。 イベントを作成するときに、[プロパティ](properties.md)で使用する構文と非常によく似た構文を使用します。 このハンドラーには、`add` および `remove` という別の名前があることに注意してください。 これらはイベントをサブスクライブするか、またはイベントのサブスクリプションを解除するために呼び出されます。 また、イベント変数を格納するために、プライベートなバッキング フィールドを宣言する必要があることにも注意してください。 このフィールドは null に初期化されます。

次に、サブディレクトリを走査して両方のイベントを発生させる `Search` メソッドのオーバー ロードを追加します。 これを実現する最も簡単な方法は、既定の引数を使用して、すべてのディレクトリの検索を指定することです。

[!code-csharp[SearchImplementation](../../samples/snippets/csharp/events/Program.cs#FinalImplementation "Implementation to search directories")]

この時点で、アプリケーションを実行し、すべてのサブディレクトリを検索するオーバーロードを呼び出すことができます。 新しい `ChangeDirectory` イベントでは、サブスクライバーが存在しませんが、`?.Invoke()` 慣用句を使用すれば、正常に動作させることができます。

 ここで、コンソール ウィンドウに進行状況を表示する行を記述するハンドラーを追加します。

[!code-csharp[Search](../../samples/snippets/csharp/events/Program.cs#Search "Declare event handler")]

このトピックでは、.NET エコシステム全体で使用されるパターンを確認しました。
これらのパターンと規則を学習することにより、慣用句を使用した C# および .NET をすばやく記述できるようになります。

次の項目では、.NET の最新のリリースで変更されたパターンを確認してください。

[次へ](modern-events.md)
