---
title: 'チュートリアル: .NET Core ツールを作成する'
description: .NET Core ツールを作成する方法について説明します。 ツールは、.NET Core CLI を使用してインストールされるコンソール アプリケーションです。
ms.date: 02/12/2020
ms.openlocfilehash: 88cc3be7b149834ace0c5f3ba8ac8c039199908f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78156726"
---
# <a name="tutorial-create-a-net-core-tool-using-the-net-core-cli"></a>チュートリアル: .NET Core CLI を使用して .NET Core ツールを作成する

**この記事の対象:** ✔️ .NET Core 2.1 SDK 以降のバージョン

このチュートリアルでは、.NET Core ツールを作成およびパッケージ化する方法について説明します。 .NET Core CLI を使ってコンソール アプリケーションをツールとして作成し、他のユーザーがインストールおよび実行できるようにすることができます。 .NET Core ツールは、.NET Core CLI からインストールされる NuGet パッケージです。 ツールの詳細については、[.NET Core ツールの概要](global-tools.md)に関する記事を参照してください。

作成するツールは、メッセージを入力として受け取り、ロボットの画像を作成するテキスト行と共にメッセージを表示するコンソール アプリケーションです。

これは、3 つのチュートリアル シリーズの第 1 回です。 このチュートリアルでは、ツールを作成してパッケージ化します。 次の 2 つのチュートリアルでは、[グローバル ツールとしてツールを使用する](global-tools-how-to-use.md)方法と、[ローカル ツールとしてツールを使用する](local-tools-how-to-use.md)方法を説明します。

## <a name="prerequisites"></a>必須コンポーネント

- [.NET Core SDK 3.1](https://dotnet.microsoft.com/download) 以降のバージョン。

  このチュートリアルと次の[グローバル ツールのチュートリアル](global-tools-how-to-use.md)は、.NET Core SDK 2.1 以降のバージョンに適用されます。これは、そのバージョンからグローバル ツールを使用できるようになったためです。 ただし、このチュートリアルでは、[ローカル ツールのチュートリアル](local-tools-how-to-use.md)に進むことができるように、3.1 以降がインストールされていることを前提としています。 ローカル ツールは .NET Core SDK 3.0 以降で使用できます。 ツールを作成する手順は、グローバル ツールとローカル ツールのどちらとして使用する場合でも同じです。
  
- ユーザーが選んだテキスト エディターまたはコード エディター。

## <a name="create-a-project"></a>プロジェクトを作成する

1. コマンド プロンプトを開き、*repository* という名前のフォルダーを作成します。

1. *repository* フォルダーに移動し、次のコマンドを入力します。

   ```dotnetcli
   dotnet new console -n microsoft.botsay
   ```

   このコマンドを実行すると、*repository* フォルダーの下に *microsoft.botsay* という名前の新しいフォルダーが作成されます。

1. *microsoft.botsay* フォルダーに移動します。

   ```console
   cd microsoft.botsay
   ```

## <a name="add-the-code"></a>コードの追加

1. コード エディターを使用して `Program.cs` ファイルを開きます。

1. 次の `using` ディレクティブをファイルの先頭に追加します。

   ```csharp
   using System.Reflection;
   ```

1. `Main` メソッドを次のコードに置き換えて、アプリケーションのコマンドライン引数を処理します。

   ```csharp
   static void Main(string[] args)
   {
       if (args.Length == 0)
       {
           var versionString = Assembly.GetEntryAssembly()
                                   .GetCustomAttribute<AssemblyInformationalVersionAttribute>()
                                   .InformationalVersion
                                   .ToString();

           Console.WriteLine($"botsay v{versionString}");
           Console.WriteLine("-------------");
           Console.WriteLine("\nUsage:");
           Console.WriteLine("  botsay <message>");
           return;
       }

       ShowBot(string.Join(' ', args));
   }
   ```

   引数が渡されなかった場合、短いヘルプ メッセージが表示されます。 それ以外の場合、次の手順で作成する `ShowBot` メソッドを呼び出すと、すべての引数が 1 つの文字列に連結され、出力されます。

1. 文字列パラメーターを受け取る `ShowBot` という新しいメソッドを追加します。 このメソッドを実行すると、テキストの行を使用してメッセージとロボットの画像を出力されます。

   ```csharp
   static void ShowBot(string message)
   {
       string bot = $"\n        {message}";
       bot += @"
       __________________
                         \
                          \
                             ....
                             ....'
                              ....
                           ..........
                       .............'..'..
                    ................'..'.....
                  .......'..........'..'..'....
                 ........'..........'..'..'.....
                .'....'..'..........'..'.......'.
                .'..................'...   ......
                .  ......'.........         .....
                .    _            __        ......
               ..    #            ##        ......
              ....       .                 .......
              ......  .......          ............
               ................  ......................
               ........................'................
              ......................'..'......    .......
           .........................'..'.....       .......
        ........    ..'.............'..'....      ..........
      ..'..'...      ...............'.......      ..........
     ...'......     ...... ..........  ......         .......
    ...........   .......              ........        ......
   .......        '...'.'.              '.'.'.'         ....
   .......       .....'..               ..'.....
      ..       ..........               ..'........
             ............               ..............
            .............               '..............
           ...........'..              .'.'............
          ...............              .'.'.............
         .............'..               ..'..'...........
         ...............                 .'..............
          .........                        ..............
           .....
   ";
       Console.WriteLine(bot);
   }
   ```

1. 変更内容を保存します。

## <a name="test-the-application"></a>アプリケーションをテストする

プロジェクトを実行して出力を確認します。 次のようにコマンド ラインを変えて、異なる結果を表示してみてください。

```dotnetcli
dotnet run
dotnet run -- "Hello from the bot"
dotnet run -- Hello from the bot
```

区切り記号 `--` の後の引数は、すべてアプリケーションに渡されます。

## <a name="package-the-tool"></a>ツールをパッケージ化する

アプリケーションをパッケージ化してツールとして配布する前に、プロジェクト ファイルを変更する必要があります。

1. *microsoft.botsay.csproj* ファイルを開き、3 つの新しい XML ノードを `<PropertyGroup>` ノードの最後に追加します。

   ```xml
   <PackAsTool>true</PackAsTool>
   <ToolCommandName>botsay</ToolCommandName>
   <PackageOutputPath>./nupkg</PackageOutputPath>
   ```

   `<ToolCommandName>` は、インストール後にツールを呼び出すコマンドを指定する省略可能な要素です。 この要素を指定しない場合、ツールのコマンド名は、 *.csproj* 拡張子のないプロジェクト ファイル名になります。

   `<PackageOutputPath>` は、NuGet パッケージが生成される場所を決定する省略可能な要素です。 NuGet パッケージは、.NET Core CLI でツールのインストールに使用されるものです。

   プロジェクト ファイルは次の例のようになります。

   ```xml
   <Project Sdk="Microsoft.NET.Sdk">
  
     <PropertyGroup>

       <OutputType>Exe</OutputType>
       <TargetFramework>netcoreapp3.1</TargetFramework>
  
       <PackAsTool>true</PackAsTool>
       <ToolCommandName>botsay</ToolCommandName>
       <PackageOutputPath>./nupkg</PackageOutputPath>
  
     </PropertyGroup>

   </Project>
   ```

1. [dotnet pack](dotnet-pack.md) コマンドを実行して、NuGet パッケージを作成します。

   ```dotnetcli
   dotnet pack
   ```

   *microsoft.botsay.1.0.0.nupkg* ファイルは、*microsoft.botsay.csproj* ファイルの `<PackageOutputPath>` 値で識別されるフォルダー (この例では、 *./nupkg* フォルダー) に作成されます。
  
   ツールをリリースする場合は、`https://www.nuget.org` にアップロードすることができます。 NuGet 上でツールを使用できるようになると、開発者は [dotnet tool install](dotnet-tool-install.md) コマンドを使用してツールをインストールできます。 このチュートリアルでは、ローカルの *nupkg* フォルダーからパッケージを直接インストールするため、NuGet にパッケージをアップロードする必要はありません。

## <a name="troubleshoot"></a>トラブルシューティング

チュートリアルの実行中にエラー メッセージが表示された場合は、「[.NET Core ツールの使用に関する問題のトラブルシューティング](troubleshoot-usage-issues.md)」を参照してください。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、コンソール アプリケーションを作成し、ツールとしてパッケージ化しました。 このツールをグローバル ツールとして使用する方法については、次のチュートリアルに進んでください。

> [!div class="nextstepaction"]
> [グローバル ツールをインストールして使用する](global-tools-how-to-use.md)

必要に応じて、グローバル ツール チュートリアルをスキップし、ローカル ツール チュートリアルに直接移動できます。

> [!div class="nextstepaction"]
> [ローカル ツールをインストールして使用する](local-tools-how-to-use.md)
