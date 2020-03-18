---
title: Hello World -- Windows または Mac 上での Visual Studio を使用する最初のプログラム -C# プログラミング ガイド
ms.date: 09/12/2019
f1_keywords:
- cs.program
- vs.csharp.startpage.firstapplication
helpviewer_keywords:
- examples [C#], Hello World
- Hello World example [C#]
ms.assetid: 6493182a-b0b6-4539-a719-518a168cb730
ms.openlocfilehash: 910fa4af1b4e45ce627b589a06910dc168490047
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712144"
---
# <a name="hello-world----your-first-program"></a>Hello World -- 最初のプログラム

この記事では、Visual Studio を使用して従来の "Hello World!" プログラムを 作成します。 Visual Studio は、.NET 開発向けに設計された多くの機能を備えた、本格的な統合開発環境 (IDE) です。 Visual Studio の一部の機能のみを使用して、このプログラムを作成します。 Visual Studio の詳細については、[Visual C# の概要](/visualstudio/ide/quickstart-csharp-console)に関するページを参照してください。

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="create-a-new-application"></a>新しいアプリケーションを作成する

<!-- markdownlint-disable MD025 -->

# <a name="windows"></a>[Windows](#tab/windows)

Visual Studio を起動します。 Windows 上に次のようなイメージが表示されます。

![Windows 上の Visual Studio のようこそ画面](./media/hello-world-your-first-program/visual-studio-windows-start-screen.png)

イメージの右下隅にある **[新しいプロジェクトの作成]** を選択します。 Visual Studio で、 **[新しいプロジェクト]** ダイアログが開きます。

![Windows 上の Visual Studio の [新しいプロジェクト] 画面](./media/hello-world-your-first-program/visual-studio-windows-new-project.png)

> [!NOTE]
> 初めて Visual Studio を起動した場合、 **[最近使用したプロジェクト テンプレート]** の一覧は空の状態です。

[新しいプロジェクト] ダイアログで、[コンソールアプリ (.NET Core)] を選択し、 **[次へ]** を押します。 「HelloWorld」などプロジェクトの名前を指定して、 **[OK]** を押します。

Visual Studio でプロジェクトが開かれます。 これが既に、基本的な "Hello World!" の例です。 `Ctrl` + `F5` を押してプロジェクトを実行します。 Visual Studio によってプロジェクトがビルドされ、ソース コードが実行可能ファイルに変換されます。 次に、新しいアプリケーションを実行するコマンド ウィンドウが起動されます。 ウィンドウには次のテキストが表示されます。

```console
Hello World!

C:\Program Files\dotnet\dotnet.exe (process 11964) exited with code 0.
Press any key to close this window . . .
```

任意のキーを押して、ウィンドウを閉じます。

# <a name="macos"></a>[macOS](#tab/macos)

Visual Studio for Mac を起動します。 Mac 上に次のようなイメージが表示されます。

![Mac 上の Visual Studio のようこそ画面](./media/hello-world-your-first-program/visual-studio-mac-start-screen.png)

> [!NOTE]
> 初めて Visual Studio for Mac を起動した場合、 **[最近使用したプロジェクト]** の一覧は空の状態です。

イメージの右上隅にある **[新規]** を選択します。 Visual Studio for Mac で、 **[新しいプロジェクト]** ダイアログが開きます。

![Mac 上の Visual Studio の [新しいプロジェクト] 画面](./media/hello-world-your-first-program/visual-studio-mac-new-project.png)

[新しいプロジェクト] ダイアログで、[.NET Core]、[コンソール アプリ] の順に選択し、 **[次へ]** を押します。 ターゲット フレームワークを選択する必要があります。 既定値で問題ありませんので、[次へ] を押します。 「HelloWorld」などプロジェクトの名前を指定して、 **[OK]** を押します。 既定のプロジェクトの場所を使用できます。 ソース管理にこのプロジェクトを追加しないでください。

Visual Studio for Mac でプロジェクトが開かれます。 これが既に、基本的な "Hello World!" の例です。 `Ctrl` + `Fn` + `F5` を押してプロジェクトを実行します。 Visual Studio for Mac によってプロジェクトがビルドされ、ソース コードが実行可能ファイルに変換されます。 次に、新しいアプリケーションを実行するコマンド ウィンドウが起動されます。 ウィンドウには次のテキストが表示されます。

```console
Hello World!

Press any key to close this window . . .
```

任意のキーを押して、セッションを終了します。

---

## <a name="elements-of-a-c-program"></a>C# プログラムの要素

このプログラムの重要な部分を調べてみましょう。 最初の行はコメントになっています。 「`//`」という文字があると、これ以降その行はコメントになります。

[!code-csharp[csProgGuide#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#32)]

テキスト ブロックを `/*` 文字と `*/` 文字で囲んでコメントにすることもできます。 次の例を参照してください。

[!code-csharp[csProgGuide#33](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#33)]

C# コンソールアプリケーションには、`Main` メソッドが必要です。このメソッドの中で制御を開始して終了します。 `Main` メソッドでは、オブジェクトを作成し、ほかのメソッドを実行します。

`Main` メソッドはクラスまたは構造体の中に存在する [static](../../language-reference/keywords/static.md) メソッドです。 前の "Hello World!" の 例では、`Hello` という名前のクラスに存在していました。 次の方法のいずれかで `Main` メソッドを宣言できます。

- `void` 型を返すことができます。 これは、プログラムが値を返さないことを意味します。

[!code-csharp[csProgGuideMain#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#12)]

- 整数を返すこともできます。 整数は、アプリケーションの**終了コード**です。

[!code-csharp[csProgGuideMain#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#13)]

- どちらの戻り値の型でも、次のように引数を受け取ることができます。

[!code-csharp[csProgGuideMain#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#19)]

または

[!code-csharp[csProgGuideMain#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#18)]

`Main` メソッドのパラメーターである `args` は、`string` の配列で、プログラムの実行時に使用したコマンドライン引数を含みます。

コマンドライン引数の使用方法の詳細については、「[Main() とコマンドライン引数](../main-and-command-args/index.md)」にある例を参照してください。

## <a name="input-and-output"></a>入力と出力

C# プログラムは、普通、.NET Framework のランタイムライブラリが提供する入出力サービスを使用します。 `System.Console.WriteLine("Hello World!");` 命令文では、<xref:System.Console.WriteLine%2A> メソッドを使用しています。 これは、ランタイムライブラリの <xref:System.Console> クラスの出力メソッドの 1 つです。 文字列パラメーターを標準出力ストリームに出力し、最後に改行を付け加えます。 別の入出力操作には、他の <xref:System.Console> メソッドを使用できます。 `using System;` ディレクティブをプログラムの開始時にインクルードした場合は、完全に修飾せずに <xref:System> クラスおよびメソッドを直接使用できます。 たとえば、`Console.WriteLine` の代わりに `System.Console.WriteLine` を呼び出すことができます。

[!code-csharp[csProgGuide#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/using.cs#1)]

[!code-csharp[csProgGuide#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#23)]

入出力メソッドの詳細については、「<xref:System.IO>」を参照してください。

## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [サンプルおよびチュートリアル](../../../samples-and-tutorials/index.md)
- [Main() とコマンドライン引数](../main-and-command-args/index.md)
- [Visual C# 入門](/visualstudio/ide/quickstart-csharp-console)
