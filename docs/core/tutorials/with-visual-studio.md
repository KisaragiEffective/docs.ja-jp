---
title: Visual Studio での .NET Core を使用した Hello World アプリケーションの作成
description: Visual Studio を使用して、C# または Visual Basic で最初の .NET Core コンソール アプリケーションを作成する方法について説明します。
author: BillWagner
ms.author: wiwagn
ms.date: 12/09/2019
ms.custom: vs-dotnet
ms.openlocfilehash: ba996e4add1cfe44681154b00a6530b1f3e70b37
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75714007"
---
# <a name="tutorial-create-your-first-net-core-console-application-in-visual-studio-2019"></a>チュートリアル: Visual Studio 2019 で最初の .NET Core コンソール アプリケーションを作成する

この記事では、Visual Studio 2019 で Hello World .NET Core コンソール アプリケーションを作成して実行するためのステップ バイ ステップの概要を説明します。 Hello World アプリケーションは、従来、初心者に新しいプログラミング言語に紹介するために使用されています。 このプログラムは、単に "Hello World!" という語句を 画面に出力しました。

## <a name="prerequisites"></a>必須コンポーネント

- **.NET Core クロスプラットフォーム開発**ワークロードがインストールされている [Visual Studio 2019 バージョン 16.4 以降](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)。 このワークロードを選択すると、.NET Core 3.1 SDK が自動的にインストールされます。

詳細については、記事「[.NET Core SDK をインストールする](../install/sdk.md?pivots=os-windows)」の「[Visual Studio を使用してインストールする](../install/sdk.md?pivots=os-windows#install-with-visual-studio)」のセクションを参照してください。

## <a name="create-the-app"></a>アプリを作成する

次の手順では、単純な Hello World コンソール アプリケーションを作成します。

<!-- markdownlint-disable MD025 -->

# <a name="c"></a>[C#](#tab/csharp)

1. Visual Studio 2019 を開きます。

1. "HelloWorld" という名前の新しい C# .NET Core コンソール アプリ プロジェクトを作成します。

   1. スタート ウィンドウで、 **[新しいプロジェクトの作成]** を選択します。

      ![Visual Studio の [スタート] ウィンドウで [新しいプロジェクトの作成] ボタンが選択されています](./media/with-visual-studio/start-window.png)

   1. **[新しいプロジェクトの作成]** ページで、検索ボックスに「**コンソール**」と入力します。 次に、言語の一覧から **[C#]** を選択し、続いて、プラットフォームの一覧から **[すべてのプラットフォーム]** を選択します。 **[コンソール アプリ (.NET Core)]** テンプレートを選択し、 **[次へ]** を選択します。

      ![フィルターが選択された状態の [新しいプロジェクトの作成] ウィンドウ](./media/with-visual-studio/create-new-project.png)

      > [!TIP]
      > .NET Core テンプレートが表示されない場合は、必要なワークロードがインストールされていない可能性があります。 **[お探しの情報が見つかりませんでしたか?]** メッセージで、 **[さらにツールと機能をインストールする]** リンクを選択します。 Visual Studio インストーラーが開きます。 **.NET Core クロスプラットフォーム開発**ワークロードがインストールされていることを確認してください。

   1. **[新しいプロジェクトの構成]** ページで、 **[プロジェクト名]** ボックスに「**HelloWorld**」と入力します。 次に、 **[作成]** を選択します。

      ![プロジェクト名、場所、およびソリューション名のフィールドを使用して新しいプロジェクト ウィンドウを構成します](./media/with-visual-studio/configure-new-project.png)

   .NET Core の C# コンソール アプリケーション テンプレートで、`Program` というクラスが、<xref:System.String> 配列を引数として必要とする単一のメソッド `Main` とともに自動的に定義されます。 `Main` はアプリケーションのエントリ ポイントで、アプリケーションを起動するときに、ランタイムによって自動的に呼び出されるメソッドです。 アプリケーションが起動されるときに提供されるコマンドライン引数はすべて *args* 配列にあります。

   ![Visual Studio と新しい HelloWorld プロジェクト](./media/with-visual-studio/visual-studio-main-window.png)

# <a name="visual-basic"></a>[Visual Basic](#tab/vb)

1. Visual Studio 2019 を開きます。

1. "HelloWorld" という名前の新しい Visual Basic .NET Core コンソール アプリを作成します。

   1. スタート ウィンドウで、 **[新しいプロジェクトの作成]** を選択します。

      ![Visual Studio の [スタート] ウィンドウで [新しいプロジェクトの作成] ボタンが選択されています](./media/with-visual-studio/start-window.png)

   1. **[新しいプロジェクトの作成]** ページで、検索ボックスに「**コンソール**」と入力します。 次に、言語の一覧から **[Visual Basic]** を選択し、続いて、プラットフォームの一覧から **[すべてのプラットフォーム]** を選択します。 **[コンソール アプリ (.NET Core)]** テンプレートを選択し、 **[次へ]** を選択します。

      ![コンソール アプリ (.NET Framework) 用の Visual Basic テンプレートを選択します。](./media/with-visual-studio/vb/create-new-project.png)

      > [!TIP]
      > .NET Core テンプレートが表示されない場合は、必要なワークロードがインストールされていない可能性があります。 **[お探しの情報が見つかりませんでしたか?]** メッセージで、 **[さらにツールと機能をインストールする]** リンクを選択します。 Visual Studio インストーラーが開きます。 **.NET Core クロスプラットフォーム開発**ワークロードがインストールされていることを確認してください。

   1. **[新しいプロジェクトの構成]** ページで、 **[プロジェクト名]** ボックスに「**HelloWorld**」と入力します。 次に、 **[作成]** を選択します。

   .NET Core のコンソール アプリ テンプレートで、`Program` というクラスが、<xref:System.String> 配列を引数として受け取る単一のメソッド `Main` とともに自動的に定義されます。 `Main` はアプリケーションのエントリ ポイントで、アプリケーションを起動するときに、ランタイムによって自動的に呼び出されるメソッドです。 アプリケーションが起動されるときに提供されるコマンド ライン引数はすべて `args` パラメーターにあります。

   ![Visual Studio と新しい HelloWorld プロジェクト](./media/with-visual-studio/vb/visual-studio-main-window.png)

---

   このテンプレートでは、シンプルな "Hello World" アプリケーションを作成します。 <xref:System.Console.WriteLine(System.String)?displayProperty=nameWithType> メソッドを呼び出し、リテラル文字列 "Hello World!" を コンソール ウィンドウに表示します。

## <a name="run-the-app"></a>アプリを実行する

1. プログラムを実行するには、ツール バーで **[HelloWorld]** を選択するか、**F5** キーを押します。

   ![HelloWorld の実行ボタンが選択された Visual Studio ツールバー](./media/with-visual-studio/run-program.png)

   コンソール ウィンドウが開き、"Hello World!" というテキストが 画面に出力され、Visual Studio のデバッグ情報が表示されます。

   ![Hello World Press any key to continue と表示されているコンソール ウィンドウ](./media/with-visual-studio/hello-world-console.png)

1. 任意のキーを押して、コンソール ウィンドウを閉じます。

## <a name="enhance-the-app"></a>アプリを拡張する

アプリケーションを拡張し、ユーザーに名前の入力を求め、日付と時刻と共にそれを表示するようにします。 次の手順では、アプリを変更してから再度実行します。

# <a name="c"></a>[C#](#tab/csharp)

1. `Main` メソッド (現在は `Console.WriteLine` を呼び出す行のみ) の内容を以下のコードに置き換えます。

   [!code-csharp[GettingStarted#1](~/samples/snippets/csharp/getting_started/with_visual_studio/helloworld.cs#1)]

   このコードは、"What is your name?" と コンソール ウィンドウに表示して、ユーザーが文字列を入力して Enter キーを押すまで待機します。 これは文字列を `name` という変数に格納します。 さらに現在の現地時刻を含む <xref:System.DateTime.Now?displayProperty=nameWithType> プロパティの値を取得して、それを `date` という変数に代入します。 最後に[挿入文字列](../../csharp/language-reference/tokens/interpolated.md)を使用して、これらの値をコンソール ウィンドウに表示します。

1. **[ビルド]**  >  **[ソリューションのビルド]** と選択して、プログラムをコンパイルします。

1. プログラムを実行するには、ツール バーで **[HelloWorld]** を選択するか、**F5** キーを押します。

1. プロンプトに対し、名前を入力し、**Enter** キーを押します。

   ![プログラムの出力が変更されたコンソール ウィンドウ](./media/with-visual-studio/hello-world-update.png)

1. 任意のキーを押して、コンソール ウィンドウを閉じます。

# <a name="visual-basic"></a>[Visual Basic](#tab/vb)

1. `Main` メソッド (現在は `Console.WriteLine` を呼び出す行のみ) の内容を以下のコードに置き換えます。

   [!code-vb[GettingStarted#1](~/samples/snippets/core/tutorials/vb-with-visual-studio/helloworld.vb#1)]

   このコードは、"What is your name?" と コンソール ウィンドウに表示して、ユーザーが文字列を入力して Enter キーを押すまで待機します。 これは文字列を `name` という変数に格納します。 さらに現在の現地時刻を含む <xref:System.DateTime.Now?displayProperty=nameWithType> プロパティの値を取得して、それを `date` という変数に代入します。 最後に[挿入文字列](../../visual-basic/programming-guide/language-features/strings/interpolated-strings.md)を使用して、これらの値をコンソール ウィンドウに表示します。

1. **[ビルド]**  >  **[ソリューションのビルド]** と選択して、プログラムをコンパイルします。

1. プログラムを実行するには、ツール バーで **[HelloWorld]** を選択するか、**F5** キーを押します。

1. プロンプトに対し、名前を入力し、**Enter** キーを押します。

   ![プログラムの出力が変更されたコンソール ウィンドウ](./media/with-visual-studio/hello-world-update.png)

1. 任意のキーを押して、コンソール ウィンドウを閉じます。

---

## <a name="next-steps"></a>次の手順

この記事では、最初の .NET Core アプリケーションを作成して実行しました。 次の手順では、このアプリをデバッグします。

> [!div class="nextstepaction"]
> [Visual Studio で .NET Core Hello World アプリケーションをデバッグする](debugging-with-visual-studio.md)
