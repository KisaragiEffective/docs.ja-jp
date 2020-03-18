---
title: Visual Studio for Mac を使用した .NET Core の概要
description: このトピックでは、Visual Studio for Mac と .NET Core を使用して、単純なコンソール アプリケーションをビルドする手順を示します。
ms.date: 12/19/2019
ms.openlocfilehash: 4cd7e311411bce62698e291e763227496877ea39
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75740493"
---
# <a name="get-started-with-net-core-on-macos-using-visual-studio-for-mac"></a>Visual Studio for Mac を使用した macOS での .NET Core の概要

Visual Studio for Mac では、.NET Core アプリケーション開発用の機能をすべて備えた統合開発環境 (IDE) が提供されます。 この記事では、Visual Studio for Mac と .NET Core を使用して、単純なコンソール アプリケーションをビルドする手順を示します。

> [!NOTE]
> お客様のフィードバックは非常に貴重です。 次の 2 つの方法で Visual Studio for Mac の開発チームにフィードバックを送信できます。
>
> * Visual Studio for Mac で、メニューから **[ヘルプ]**  >  **[問題の報告]** の順に選択するか、ようこそ画面から **[問題の報告]** を選択して、バグ報告を提出するためのウィンドウを開きます。 お客様のフィードバックは、[開発者コミュニティ](https://developercommunity.visualstudio.com/spaces/8/index.html) ポータルで追跡することができます。
> * 提案するには、メニューから **[ヘルプ]**  >  **[提案の送信]** の順に選択するか、ようこそ画面から **[提案の送信]** を選択し、[Visual Studio for Mac の開発者コミュニティの Web ページ](https://developercommunity.visualstudio.com/content/idea/post.html?space=41)に移動します。

## <a name="prerequisites"></a>前提条件

[.NET Core の依存関係と要件](../install/dependencies.md?pivots=os-macos)に関する記事を参照してください。

サポートされているバージョンの .NET Core を使用していることを確認するには、「[.NET Core サポート](/visualstudio/mac/net-core-support)」の記事を参照してください。

## <a name="get-started"></a>はじめに

必須コンポーネントと Visual Studio for Mac を既にインストールしている場合は、このセクションをスキップして、「[プロジェクトの作成](#creating-a-project)」に進みます。 以下の手順に従って、必須コンポーネントと Visual Studio for Mac をインストールします。

[Visual Studio for Mac インストーラー](https://visualstudio.microsoft.com/vs/mac/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link)をダウンロードします。 インストーラーを実行します。 使用許諾契約を読み、同意します。 インストール時に、.NET Core をインストールするオプションを選択します。 Xamarin (クロスプラットフォーム モバイル アプリ開発テクノロジ) をインストールすることができます。 .NET Core の開発の場合、Xamarin とその関連コンポーネントのインストールは省略できます。 Visual Studio for Mac のインストール プロセスのチュートリアルについては、[Visual Studio for Mac のドキュメント](/visualstudio/mac/)をご覧ください。 インストールが完了したら、Visual Studio for Mac IDE を起動します。

## <a name="creating-a-project"></a>プロジェクトの作成

1. スタート ウィンドウで **[新規]** を選択します。

   ![Visual Studio for Mac のスタート ウィンドウの [新規] ボタン](./media/using-on-mac-vs/visual-studio-mac-new-project.png)

1. **[新しいプロジェクト]** ダイアログで、 **[.NET Core]** ノードの下にある **[アプリ]** を選択します。 **[コンソール アプリケーション]** テンプレートを選択してから **[次へ]** を選択します。

   ![新しいプロジェクト テンプレートのリスト](./media/using-on-mac-vs/visual-studio-mac-new-dialog.png)

1. 複数のバージョンの .NET Core をインストールしている場合は、プロジェクトのターゲット フレームワークを選択します。

1. **[プロジェクト名]** には「HelloWorld」と入力します。 **[作成]** を選択します。

   ![[Configure your new Console Application (新しいコンソール アプリケーションの構成)] ダイアログ](./media/using-on-mac-vs/visual-studio-mac-new-options.png)

1. プロジェクトの依存関係が復元されるまで待ちます。 プロジェクトには、*メソッドを持つ* クラスを含む `Program`Program.cs`Main` という C# ファイルが 1 つあります。 `Console.WriteLine` ステートメントは、アプリの実行時に コンソールに "Hello World!" と出力します。

   ![Program.cs ファイルが開かれた状態のメイン ウィンドウ](./media/using-on-mac-vs/visual-studio-mac-editor.png)

## <a name="run-the-application"></a>アプリケーションの実行

⌘ ↵ (command + enter) を使用してデバッグ モードで、または ⌥ ⌘ ↵ (option + command + enter) を使用してリリース モードでアプリを実行します。

![[アプリケーション出力] ウィンドウに Hello World! が表示された状態](./media/using-on-mac-vs/visual-studio-mac-output.png)

## <a name="next-step"></a>次のステップ

「[Visual Studio for Mac を使用した macOS での完全な .NET Core ソリューションの構築](using-on-mac-vs-full-solution.md)」トピックでは、再利用可能なライブラリと単体テストを含む完全な .NET Core ソリューションを構築する方法を示します。
