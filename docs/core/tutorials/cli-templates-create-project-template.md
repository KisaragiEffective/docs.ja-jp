---
title: dotnet new 用のプロジェクト テンプレートを作成する
description: dotnet new コマンド用のプロジェクト テンプレートを作成する方法を説明します。
author: thraka
ms.date: 06/25/2019
ms.topic: tutorial
ms.author: adegeo
ms.openlocfilehash: f53f4037f832265a35f65bf2e5096c7e5a37bcf1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77503533"
---
# <a name="tutorial-create-a-project-template"></a>チュートリアル: プロジェクト テンプレートを作成する

.NET Core では、プロジェクト、ファイル、さらにはリソースを生成するテンプレートを作成して配置することができます。 このチュートリアルは、`dotnet new` コマンドで使用するテンプレートの作成、インストール、アンインストール方法を説明するシリーズのパート 2 です。

シリーズのこのパートで学習する内容は次のとおりです。

> [!div class="checklist"]
>
> * プロジェクト テンプレートのリソースを作成する
> * テンプレートの構成フォルダーとファイルを作成する
> * ファイル パスからテンプレートをインストールする
> * 項目テンプレートをテストする
> * 項目テンプレートをアンインストールする

## <a name="prerequisites"></a>必須コンポーネント

* このチュートリアル シリーズの[パート 1](cli-templates-create-item-template.md) を完了します。
* ターミナルを開いて _working\templates_ フォルダーに移動します。

## <a name="create-a-project-template"></a>プロジェクト テンプレートを作成する

プロジェクト テンプレートを使用すると、ユーザーがコードのワーキング セットを使用して簡単に作業を開始できる、すぐに実行できるプロジェクトが作成されます。 .NET Core には、コンソール アプリケーションやクラス ライブラリなど、いくつかのプロジェクト テンプレートが含まれています。 この例では、C# 8.0 を有効にし、`async main` エントリ ポイントを生成する、新しいコンソール プロジェクトを作成します。

ターミナルで _working\templates_ フォルダーに移動し、_consoleasync_ という名前の新しいサブフォルダーを作成します。 このサブフォルダーに入り、`dotnet new console` を実行して標準コンソール アプリケーションを生成します。 このテンプレートによって生成されたファイルを編集して、新しいテンプレートを作成します。

```console
working
└───templates
    └───consoleasync
            consoleasync.csproj
            Program.cs
```

## <a name="modify-programcs"></a>Program.cs を変更する

_program.cs_ ファイルを開きます。 コンソール プロジェクトでは非同期エントリ ポイントを使用しないので、それを追加しましょう。 コードを次のように変更し、ファイルを保存します。

```csharp
using System;
using System.Threading.Tasks;

namespace consoleasync
{
    class Program
    {
        static async Task Main(string[] args)
        {
            await Console.Out.WriteAsync("Hello World with C# 8.0!");
        }
    }
}
```

## <a name="modify-consoleasynccsproj"></a>consoleasync.csproj を変更する

プロジェクトで使用される C# 言語のバージョンをバージョン 8.0 に更新しましょう。 _consoleasync.csproj_ ファイルを編集し、`<LangVersion>` 設定を `<PropertyGroup>` ノードに追加します。

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.2</TargetFramework>

    <LangVersion>8.0</LangVersion>

  </PropertyGroup>
  
</Project>
```

## <a name="build-the-project"></a>プロジェクトをビルドする

プロジェクト テンプレートを完成させる前にテストして、正しくコンパイルされ実行されることを確認する必要があります。

ご利用のターミナルで、次のコマンドを実行します。

```dotnetcli
dotnet run
```

次の出力が得られます。

```console
Hello World with C# 8.0!
```

`dotnet run` の使用によって作成された _obj_ および _bin_ フォルダーは削除できます。 これらのファイルを削除すると、テンプレートに関連するファイルのみがテンプレートに含まれ、ビルド アクションの結果として生じたファイルが含まれなくなります。

テンプレートのコンテンツを作成したので、テンプレートのルート フォルダーでテンプレートの構成を作成する必要があります。

## <a name="create-the-template-config"></a>テンプレートの構成を作成する

.NET Core では、テンプレートが、テンプレートのルートに存在する特別なフォルダーと構成ファイルによって認識されます。 このチュートリアルでは、テンプレート フォルダーは _working\templates\consoleasync_ にあります。

テンプレートを作成すると、特別な構成フォルダーを除く、テンプレート フォルダー内のすべてのファイルとフォルダーがテンプレートの一部として含まれます。 この構成フォルダーの名前は _.template.config_ です。

まず、 _.template.config_ という名前の新しいサブフォルダーを作成し、このフォルダーに入ります。 次に、_template.json_ という名前の新しいファイルを作成します。 フォルダー構造は次のようになります。

```console
working
└───templates
    └───consoleasync
        └───.template.config
                template.json
```

お好みのテキスト エディターで _template.json_ を開き、次の json コードを貼り付けて保存します。

```json
{
  "$schema": "http://json.schemastore.org/template",
  "author": "Me",
  "classifications": [ "Common", "Console", "C#8" ],
  "identity": "ExampleTemplate.AsyncProject",
  "name": "Example templates: async project",
  "shortName": "consoleasync",
  "tags": {
    "language": "C#",
    "type": "project"
  }
}
```

この構成ファイルには、テンプレートのすべての設定が含まれます。 `name` や `shortName` などの基本設定を確認できますが、`project` に設定された `tags/type` 値もあります。 これにより、テンプレートがプロジェクト テンプレートとして指定されます。 作成するテンプレートの種類に制限はありません。 `item` および `project` 値は、ユーザーが検索しているテンプレートの種類を簡単にフィルター処理できるように .NET Core で推奨されている一般的な名前です。

`classifications` 項目は、`dotnet new` を実行してテンプレートの一覧を取得したときに表示される **tags** 列を表します。 ユーザーは分類タグに基づいて検索することもできます。 json ファイル内の `tags` プロパティと、`classifications` の tags 一覧を混同しないようにしてください。 これらは残念ながら同じ名前を付けられてしまった 2 つの異なるものです。 *template.json* ファイルの完全スキーマは [JSON Schema Store](http://json.schemastore.org/template) にあります。 *template.json* ファイルについて詳しくは、[dotnet テンプレート wiki](https://github.com/dotnet/templating/wiki) をご覧ください。

有効な _.template.config/template.json_ ファイルを用意したので、テンプレートをインストールする準備ができました。 テンプレートをインストールする前に、テンプレートに含めたくない余分なファイルとフォルダー (_bin_ フォルダーや _obj_ フォルダーなど) を必ず削除してください。 ターミナルで _consoleasync_ フォルダーに移動し、`dotnet new -i .\` を実行して現在のフォルダーにあるテンプレートをインストールします。 Linux または macOS オペレーティング システムを使用している場合は、`dotnet new -i ./` のようにスラッシュを使用します。

このコマンドにより、インストールされているテンプレートの一覧が出力されます。作成したテンプレートも含まれているはずです。

```dotnetcli
dotnet new -i .\
```

次のような出力が得られます。

```console
Usage: new [options]

Options:
  -h, --help          Displays help for this command.
  -l, --list          Lists templates containing the specified name. If no name is specified, lists all templates.

... cut to save space ...

Templates                                         Short Name            Language          Tags
-------------------------------------------------------------------------------------------------------------------------------
Console Application                               console               [C#], F#, VB      Common/Console
Example templates: async project                  consoleasync          [C#]              Common/Console/C#8
Class library                                     classlib              [C#], F#, VB      Common/Library
WPF Application                                   wpf                   [C#], VB          Common/WPF
Windows Forms (WinForms) Application              winforms              [C#], VB          Common/WinForms
Worker Service                                    worker                [C#]              Common/Worker/Web
```

### <a name="test-the-project-template"></a>プロジェクト テンプレートをテストする

項目テンプレートをインストールしたので、テストします。

1. _test_ フォルダーに移動します。

1. 次のコマンドを使用して、新しいコンソール アプリケーションを作成します。これにより、`dotnet run` コマンドを使用して簡単にテストできる作業プロジェクトが生成されます。

    ```dotnetcli
    dotnet new consoleasync
    ```

    次の出力が得られます。

    ```console
    The template "Example templates: async project" was created successfully.
    ```

1. 次のコマンドを使用して、プロジェクトを実行します。

    ```dotnetcli
    dotnet run
    ```

    次の出力が得られます。

    ```console
    Hello World with C# 8.0!
    ```

おめでとうございます! .NET Core でプロジェクト テンプレートを作成し、配置しました。 このチュートリアル シリーズの次のパートの準備として、作成したテンプレートをアンインストールする必要があります。 また、必ず _test_ フォルダーからすべてのファイルを削除してください。 これにより、このチュートリアルの次の主要なセクションの準備が整った状態に戻ります。

### <a name="uninstall-the-template"></a>テンプレートをアンインストールする

ファイル パスを使用してテンプレートをインストールしたので、**絶対**ファイル パスを使用してアンインストールする必要があります。 `dotnet new -u` コマンドを実行すると、インストールされているテンプレートの一覧を表示できます。 作成したテンプレートは最後に表示されているはずです。 一覧にあるパスを使用して、`dotnet new -u <ABSOLUTE PATH TO TEMPLATE DIRECTORY>` コマンドでテンプレートをアンインストールします。

```dotnetcli
dotnet new -u
```

次のような出力が得られます。

```console
Template Instantiation Commands for .NET Core CLI

Currently installed items:
  Microsoft.DotNet.Common.ItemTemplates
    Templates:
      dotnet gitignore file (gitignore)
      global.json file (globaljson)
      NuGet Config (nugetconfig)
      Solution File (sln)
      Dotnet local tool manifest file (tool-manifest)
      Web Config (webconfig)

... cut to save space ...

  NUnit3.DotNetNew.Template
    Templates:
      NUnit 3 Test Project (nunit) C#
      NUnit 3 Test Item (nunit-test) C#
      NUnit 3 Test Project (nunit) F#
      NUnit 3 Test Item (nunit-test) F#
      NUnit 3 Test Project (nunit) VB
      NUnit 3 Test Item (nunit-test) VB
  C:\working\templates\consoleasync
    Templates:
      Example templates: async project (consoleasync) C#
```

テンプレートをアンインストールするには、次のコマンドを実行します。

```dotnetcli
dotnet new -u C:\working\templates\consoleasync
```

## <a name="next-steps"></a>次の手順

このチュートリアルでは、プロジェクト テンプレートを作成しました。 項目テンプレートとプロジェクト テンプレートの両方を使いやすいファイルにパッケージ化する方法については、このチュートリアル シリーズの続きを参照してください。

> [!div class="nextstepaction"]
> [テンプレート パックを作成する](cli-templates-create-template-pack.md)
