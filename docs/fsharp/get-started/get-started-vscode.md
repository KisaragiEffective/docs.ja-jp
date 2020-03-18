---
title: Visual Studio Code での F# の概要
description: を Visual Studio Code と Ionide F# plugin suite と共に使用する方法について説明します。
ms.date: 12/23/2018
ms.openlocfilehash: 2aa62bb1afc220348f884865e55c4d7de4359b7f
ms.sourcegitcommit: 19014f9c081ca2ff19652ca12503828db8239d48
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980354"
---
# <a name="get-started-with-f-in-visual-studio-code"></a>Visual Studio Code での F# の概要

[Ionide プラグイン](https://marketplace.visualstudio.com/items?itemName=Ionide.Ionide-fsharp)をF#使用して[Visual Studio Code](https://code.visualstudio.com)に記述すると、IntelliSense とコードリファクタリングを使用して、優れたクロスプラットフォームで軽量の統合開発環境 (IDE) エクスペリエンスを実現できます。 プラグインの詳細については、 [Ionide.io](http://ionide.io) を参照してください。

まず、 [ F#と Ionide プラグインが正しくインストールさ](install-fsharp.md#install-f-with-visual-studio-code)れていることを確認します。

## <a name="create-your-first-project-with-ionide"></a>Ionide を使用して最初のプロジェクトを作成する

新しいF#プロジェクトを作成するには、コマンドラインを開き、.NET Core CLI を使用して新しいプロジェクトを作成します。

```dotnetcli
dotnet new console -lang "F#" -o FirstIonideProject
```

完了したら、ディレクトリをプロジェクトに変更し、Visual Studio Code を開きます。

```console
cd FirstIonideProject
code .
```

Visual Studio Code にプロジェクトが読み込まれると、ウィンドウのF#左側に [ソリューションエクスプローラー] ウィンドウが表示されます。 これは、Ionide によって作成したプロジェクトが正常に読み込まれたことを意味します。 この時点より前に、エディターでコードを記述できますが、これが発生すると、すべての読み込みが完了します。

## <a name="configure-f-interactive"></a>対話型F#の構成

最初に、.NET Core スクリプトが既定のスクリプト環境であることを確認します。

1. Visual Studio Code 設定 ([**コード** ** > の設定]**  > **設定**) を開きます。
1. 「  **F#スクリプト**」という用語を検索します。
1. **「Fsharp.core: USE SDK scripts**」というチェックボックスをオンにします。

これは現在、.NET Core スクリプトでは動作しない .NET Framework ベースのスクリプトのいくつかのレガシ動作のために必要です。また、Ionide は現在、旧バージョンとの互換性を維持するために努力しています。 今後、.NET Core スクリプトが既定値になります。

### <a name="write-your-first-script"></a>最初のスクリプトを作成する

.NET Core スクリプトを使用するように Visual Studio Code を構成したら、Visual Studio Code の [エクスプローラー] ビューに移動し、新しいファイルを作成します。 名前を*MyFirstScript script.fsx*にします。

ここで、次のコードを追加します。

[!code-fsharp[ToPigLatin](~/samples/snippets/fsharp/getting-started/to-pig-latin.fsx)]

この関数は、単語を [Pig Latin](https://en.wikipedia.org/wiki/Pig_Latin) の形式に変換します。 次の手順では、F# Interactive (FSI) を使用して評価します。

関数全体をハイライト表示します (長さは 11 行である必要があります)。 強調表示されたら、 **Alt**キーを押しながら**Enter**キーを押します。 画面の下部にターミナルウィンドウがポップアップ表示され、次のように表示されます。

![Ionide の概要での F# Interactive の出力例](./media/getting-started-vscode/vscode-fsi.png)

これは次の 3 つのことを行いました。

1. FSI.EXE プロセスを開始しました。
2. FSI.EXE プロセスでハイライト表示したコードが送信されました。
3. FSI.EXE プロセスは、送信したコードを評価しました。

送信した [関数](../language-reference/functions/index.md) により、FSI でその関数を呼び出すことができます。 対話型ウィンドウで、次のように入力します。 対話型ウィンドウで、次のように入力します。

```fsharp
toPigLatin "banana";;
```

次の結果が表示されます。

```fsharp
val it : string = "ananabay"
```

では、母音を最初の文字として試してみましょう。 次のように入力します。

```fsharp
toPigLatin "apple";;
```

次の結果が表示されます。

```fsharp
val it : string = "appleyay"
```

関数が期待どおりに動作しているようです。 これで、先ほど Visual Studio Code で初めて F# 関数を記述し、FSI でそれを評価しました。

> [!NOTE]
> ご存知かもしれませんが、FSI の行は`;;`で終了します。 これは、FSI で複数の行を入力できるためです。 最後`;;`は、コードが終了したことを FSI に知らせます。

## <a name="explaining-the-code"></a>コードの説明

実際にコードが何をしているかわからない場合は、次のステップで説明します。

ご覧のように、`toPigLatin`は単語を入力として受け取り、その単語を Pig 表現に変換する関数です。 この規則は次のとおりです。

単語の最初の文字が母音で始まる場合は、単語の末尾に "yay" を追加します。 母音で始まらない場合は、その最初の文字を単語の末尾に移動し、それに "ay" を追加します。

FSI.EXE では、次のことに気づいたかもしれません。

```fsharp
val toPigLatin : word:string -> string
```

これは、 `toPigLatin` `string`を入力として受け取り (`word`と呼びます)、別の`string`を返す関数であることを示します。 これは[関数の型シグネチャ](https://fsharpforfunandprofit.com/posts/function-signatures/) と呼ばれ、F# コードの基本的な部分を理解するために重要です。 Visual Studio Code でカーソルを関数に合わせると、この点にも気づくでしょう。

関数の本体には、次の 2 つの異なる部分があります。

1. `isVowel`と呼ばれる内部関数。指定された文字 (`c`) が、指定されたパターンのいずれかと一致するかどうかを[パターン一致](../language-reference/pattern-matching.md)によって確認することによって母音であるかどうかを判断します。

   [!code-fsharp[ToPigLatin](~/samples/snippets/fsharp/getting-started/to-pig-latin.fsx#L2-L6)]

2. 最初の文字が母音かどうかをチェックし、最初の文字が母音であるかどうかに基づいて、入力文字から戻り値を構築する[`if..then..else`](../language-reference/conditional-expressions-if-then-else.md) 式。

   [!code-fsharp[ToPigLatin](~/samples/snippets/fsharp/getting-started/to-pig-latin.fsx#L8-L11)]

したがって、 `toPigLatin`のフローは次の通りです。

入力された単語の最初の文字が母音であるかどうかを確認します。 母音の場合は、単語の末尾に "yay" を付加します。 それ以外の場合は、最初の文字を単語の末尾に移動し、それに "ay" を追加します。

この点については最後に説明しますF#。では、関数から戻る明示的な命令がありません。 これは、 F#が式ベースであり、関数の本体で評価された最後の式によってその関数の戻り値が決定されるためです。 `if..then..else` 自体が式であるため、`then` ブロックの本体または `else` ブロックの本体を評価すると、`toPigLatin` 関数によって返される値が決まります。

## <a name="turn-the-console-app-into-a-pig-latin-generator"></a>コンソールアプリを Pig Latin ジェネレーターにする

この記事の前のセクションでは、F# コードを記述する一般的な最初の手順が示されています。 最初に関数を記述し、FSI を使用して対話的に実行します。 これは REPL ドリブン開発と呼ばれます [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) は "Read Evaluate-Print Loop" の略称です。 この機能を試してみることをお勧めします。

REPL 駆動型開発の次の手順では、作業コードを F# 実装ファイルに移動します。 その後、F# コンパイラによって実行可能なアセンブリにコンパイルできます。

まず、.NET Core CLI で作成した*プログラム*のファイルを開きます。 いくつかのコードが既にあることがわかるでしょう。

次に、`PigLatin` という名前の新しい[`module`](../language-reference/modules.md)を作成し、先ほど作成した `toPigLatin` 関数を次のようにコピーします。

[!code-fsharp[ToPigLatin](~/samples/snippets/fsharp/getting-started/pig-latin.fs#L3-L14)]

このモジュールは、`main` 関数の上、`open System` 宣言の下に配置する必要があります。 では、宣言のF#順序が重要であるため、関数をファイルで呼び出す前に定義する必要があります。

次に、`main` 関数で、引数に対して Pig Latin generator 関数を呼び出します。

```fsharp
[<EntryPoint>]
let main argv =
    for name in argv do
        let newName = PigLatin.toPigLatin name
        printfn "%s in Pig Latin is: %s" name newName

    0
```

これで、コマンドラインからコンソールアプリを実行できるようになりました。

```dotnetcli
dotnet run apple banana
```

スクリプトファイルと同じ結果が出力されますが、今回は実行中のプログラムとして出力されます。

## <a name="troubleshooting-ionide"></a>Ionide のトラブルシューティング

発生する可能性のある特定の問題をトラブルシューティングする方法を次にします。

1. Ionide のコード編集機能を利用するにはF# 、ファイルをディスクに保存し、[Visual Studio Code] ワークスペースで開いているフォルダー内に保存する必要があります。
1. システムに変更を加えた場合、または Visual Studio Code を開いた状態で Ionide の前提条件をインストールした場合は、Visual Studio Code を再起動します。
1. プロジェクトディレクトリに無効な文字が含まれていると、Ionide が機能しない可能性があります。  この場合は、プロジェクトディレクトリの名前を変更します。
1. Ionide コマンドがいずれも動作していない場合は、 [Visual Studio Code キーバインド](https://code.visualstudio.com/docs/getstarted/keybindings#_advanced-customization)を調べて、誤って上書きしているかどうかを確認します。
1. Ionide がマシン上で壊れていて、上記のいずれも問題を解決しない場合は、コンピューターの`ionide-fsharp`ディレクトリを削除して、プラグインスイートを再インストールしてみてください。
1. プロジェクトの読み込みに失敗した場合F# (ソリューションエクスプローラーに表示されます)、そのプロジェクトを右クリックし、 **[詳細の表示]** をクリックして診断情報を取得します。

Ionide は、 F#コミュニティのメンバーによって構築および管理されるオープンソースプロジェクトです。 問題を報告して、 [ionide-vscode-Fsharp.core GitHub リポジトリ](https://github.com/ionide/ionide-vscode-fsharp)に投稿してください。

[Ionide Gitter チャネル](https://gitter.im/ionide/ionide-project) の Ionide 開発者と F# コミュニティからさらなる支援を求めることもできます。

## <a name="next-steps"></a>次のステップ:

F# と言語機能の詳細について学ぶには、[F# のツアー](../tour.md) を参照してください。
