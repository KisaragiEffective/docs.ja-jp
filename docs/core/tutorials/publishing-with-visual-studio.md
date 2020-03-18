---
title: Visual Studio での .NET Core Hello World アプリケーションの発行
description: 発行では、.NET Core アプリケーションを実行するために必要なファイルのセットを作成します。
author: BillWagner
ms.author: wiwagn
ms.date: 12/10/2019
ms.custom: vs-dotnet
ms.openlocfilehash: bdd6e28713bdece2bd144e6763bd84d719e91449
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78156635"
---
# <a name="publish-your-net-core-hello-world-application-with-visual-studio"></a>Visual Studio での .NET Core Hello World アプリケーションの発行

「[Visual Studio での .NET Core を使用した Hello World アプリケーションの作成](with-visual-studio.md)」では、Hello World コンソール アプリケーションを作成しました。 「[Visual Studio での Hello World アプリケーションのデバッグ](debugging-with-visual-studio.md)」では、Visual Studio デバッガーを使ってそれをテストしました。 正しく動作することが確認できたので、他のユーザーが使用できるように発行することができます。 発行では、アプリケーションを実行するために必要なファイルのセットを作成します。 ファイルを配置するには、それをターゲット マシンにコピーします。

## <a name="publish-the-app"></a>アプリの発行

1. Visual Studio がアプリケーションのリリース バージョンをビルドしていることを確認します。 必要に応じて、ツール バーのビルド構成の設定を **[デバッグ]** から **[リリース]** に変更します。

   ![リリース ビルドが選択された Visual Studio のツールバー](media/publishing-with-visual-studio/visual-studio-toolbar-release.png)

1. **HelloWorld** プロジェクト (HelloWorld ソリューションではなく) を右クリックし、メニューから **[発行]** を選びます。 (メイン メニューの **[ビルド]** から **[HelloWorld を発行]** を選択することもできます。)

   ![Visual Studio の [発行] コンテキスト メニュー](media/publishing-with-visual-studio/publish-context-menu.png)

1. **[発行先を選択]** ページで、 **[フォルダー]** を選択し、次に **[プロファイルの作成]** を選択します。

   ![Visual Studio で発行先を選択します](media/publishing-with-visual-studio/pick-publish-target.png)

1. **[発行]** ページで **[発行]** を選択します。

   ![Visual Studio の [発行] ウィンドウ](media/publishing-with-visual-studio/publish-page.png)

## <a name="inspect-the-files"></a>ファイルを検査する

発行プロセスでは、フレームワークに依存する配置が作成されます。これは、.NET Core がシステムにインストールされていれば、.NET Core によってサポートされる任意のプラットフォームで、発行されたアプリケーションが実行される配置の種類です。 ユーザーは、実行可能ファイルをダブルクリックするか、コマンドプロンプトから `dotnet HelloWorld.dll` コマンドを実行することで、発行されたアプリを実行できます。

次の手順で、発行プロセスによって作成されるファイルを確認します。

1. コマンド プロンプトを開きます。

   コマンド プロンプトを開く方法の 1 つとして、Windows タスク バーの検索ボックスに**コマンド プロンプト** (または短縮形の**cmd**) を入力する方法があります。 **[コマンド プロンプト]** デスクトップ アプリを選択するか、検索結果で既に選択されている場合は、**Enter** キーを押します。

1. アプリケーションのプロジェクト ディレクトリの *bin\Release\netcoreapp3.1\publish* サブディレクトリで、発行されたアプリケーションに移動します。

   ![発行されたファイルが表示されているコンソール ウィンドウ](media/publishing-with-visual-studio/published-files-output.png)

   この図に示すように、発行された出力には次のファイルが含まれます。

      * *HelloWorld.deps.json*

         このファイルは、アプリケーションのランタイム依存関係ファイルです。 これは、アプリの実行に必要な .NET Core コンポーネントとライブラリ (アプリケーションが含まれる動的リンク ライブラリを含む) を定義します。 詳細については、「[ランタイム構成ファイル](https://github.com/dotnet/cli/blob/85ca206d84633d658d7363894c4ea9d59e515c1a/Documentation/specs/runtime-configuration-file.md)」を参照してください。

      * *HelloWorld.dll*

         これは、[フレームワークに依存する展開](../deploying/deploy-with-cli.md#framework-dependent-deployment)バージョンのアプリケーションです。 このダイナミック リンク ライブラリを実行するには、コマンド プロンプトで`dotnet HelloWorld.dll` を入力します。

      * *HelloWorld.exe*

         これは、[フレームワークに依存する実行可能ファイル](../deploying/deploy-with-cli.md#framework-dependent-executable) バージョンのアプリケーションです。 これを実行するには、コマンド プロンプトで `HelloWorld.exe` を入力します。

      * *HelloWorld.pdb* (配置は省略可能)

         これは、デバッグ シンボル ファイルです。 このファイルはアプリケーションと一緒に配置する必要はありませんが、発行されるバージョンのアプリケーションをデバッグする必要がある場合に保存しておく必要があります。

      * *HelloWorld.runtimeconfig.json*

         これは、アプリケーションのランタイム構成ファイルです。 ビルドされたアプリケーションが実行時に基盤とする .NET Core のバージョンを識別します。 構成オプションを追加することもできます。 詳細については、「[.NET Core ランタイム構成設定](../run-time-config/index.md#runtimeconfigjson)」を参照してください。

## <a name="additional-resources"></a>その他のリソース

- [.NET Core アプリケーションの展開](../deploying/index.md)
