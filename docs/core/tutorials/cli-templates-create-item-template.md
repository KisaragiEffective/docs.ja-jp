---
title: dotnet new 用の項目テンプレートを作成する - .NET Core CLI
description: dotnet new コマンド用の項目テンプレートを作成する方法を説明します。 項目テンプレートには、任意の数のファイルを含めることができます。
author: thraka
ms.date: 06/25/2019
ms.topic: tutorial
ms.author: adegeo
ms.openlocfilehash: 5f4038e863d9bb59df470d3516c08fd2ad29c078
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77503561"
---
# <a name="tutorial-create-an-item-template"></a>チュートリアル: 項目テンプレートを作成する

.NET Core では、プロジェクト、ファイル、さらにはリソースを生成するテンプレートを作成して配置することができます。 このチュートリアルは、`dotnet new` コマンドで使用するテンプレートの作成、インストール、アンインストール方法を説明するシリーズのパート 1 です。

シリーズのこのパートで学習する内容は次のとおりです。

> [!div class="checklist"]
>
> * 項目テンプレートのクラスを作成する
> * テンプレートの構成フォルダーとファイルを作成する
> * ファイル パスからテンプレートをインストールする
> * 項目テンプレートをテストする
> * 項目テンプレートをアンインストールする

## <a name="prerequisites"></a>必須コンポーネント

* [.NET Core 2.2 SDK](https://dotnet.microsoft.com/download) 以降のバージョン。
* 参照記事「[dotnet new のカスタム テンプレート](../tools/custom-templates.md)」を確認しておきます。

  この参照記事では、テンプレートの基本と、テンプレートをまとめる方法が説明されています。 ここでは、この情報の一部を繰り返して示します。

* ターミナルを開いて、_working\templates_ フォルダーに移動します。

## <a name="create-the-required-folders"></a>必要なフォルダーを作成する

このシリーズでは、テンプレートのソースが含まれる "作業フォルダー" と、テンプレートをテストするための "テスト フォルダー" を使用します。 作業フォルダーとテスト フォルダーは、同じ親フォルダーの下に配置する必要があります。

まず、親フォルダーを作成します。名前は何でもかまいません。 次に、_working_ という名前のサブフォルダーを作成します。 _working_ フォルダー内に、_templates_ という名前のサブフォルダーを作成します。

次に、親フォルダーの下に _test_ という名前のフォルダーを作成します。 フォルダー構造は次のようになります。

```console
parent_folder
├───test
└───working
    └───templates
```

## <a name="create-an-item-template"></a>項目テンプレートを作成する

項目テンプレートは、1 つ以上のファイルを含む特定の種類のテンプレートです。 この種類のテンプレートは、構成、コード、ソリューションなどのファイルを生成する場合に役立ちます。 この例では、文字列型に拡張メソッドを追加するクラスを作成します。

ターミナルで、_working\templates_ フォルダーに移動し、_extensions_ という名前の新しいサブフォルダーを作成します。 このフォルダーに入ります。

```console
working
└───templates
    └───extensions
```

_CommonExtensions.cs_ という名前の新しいファイルを作成し、お好みのテキスト エディターで開きます。 このクラスにより、文字列のコンテンツを反転させる `Reverse` という名前の拡張メソッドが提供されます。 次のコードを貼り付けて、ファイルを保存します。

```csharp
using System;

namespace System
{
    public static class StringExtensions
    {
        public static string Reverse(this string value)
        {
            var tempArray = value.ToCharArray();
            Array.Reverse(tempArray);
            return new string(tempArray);
        }
    }
}
```

テンプレートのコンテンツを作成したので、テンプレートのルート フォルダーでテンプレートの構成を作成する必要があります。

## <a name="create-the-template-config"></a>テンプレートの構成を作成する

.NET Core では、テンプレートが、テンプレートのルートに存在する特別なフォルダーと構成ファイルによって認識されます。 このチュートリアルでは、テンプレート フォルダーは _working\templates\extensions_ にあります。

テンプレートを作成すると、特別な構成フォルダーを除く、テンプレート フォルダー内のすべてのファイルとフォルダーがテンプレートの一部として含まれます。 この構成フォルダーの名前は _.template.config_ です。

まず、 _.template.config_ という名前の新しいサブフォルダーを作成し、このフォルダーに入ります。 次に、_template.json_ という名前の新しいファイルを作成します。 フォルダー構造は次のようになります。

```console
working
└───templates
    └───extensions
        └───.template.config
                template.json
```

お好みのテキスト エディターで _template.json_ を開き、次の JSON コードを貼り付けて保存します。

```json
{
  "$schema": "http://json.schemastore.org/template",
  "author": "Me",
  "classifications": [ "Common", "Code" ],
  "identity": "ExampleTemplate.StringExtensions",
  "name": "Example templates: string extensions",
  "shortName": "stringext",
  "tags": {
    "language": "C#",
    "type": "item"
  }
}
```

この構成ファイルには、テンプレートのすべての設定が含まれます。 `name` や `shortName` などの基本設定を確認できますが、`item` に設定された `tags/type` 値もあります。 これにより、テンプレートが項目テンプレートとして分類されます。 作成するテンプレートの種類に制限はありません。 `item` および `project` 値は、ユーザーが検索しているテンプレートの種類を簡単にフィルター処理できるように .NET Core で推奨されている一般的な名前です。

`classifications` 項目は、`dotnet new` を実行してテンプレートの一覧を取得したときに表示される **tags** 列を表します。 ユーザーは分類タグに基づいて検索することもできます。 \*.json ファイル内の `tags` プロパティと、`classifications` の tags 一覧を混同しないようにしてください。 これらは残念ながら同じ名前を付けられてしまった 2 つの異なるものです。 *template.json* ファイルの完全スキーマは [JSON Schema Store](http://json.schemastore.org/template) にあります。 *template.json* ファイルについて詳しくは、[dotnet テンプレート wiki](https://github.com/dotnet/templating/wiki) をご覧ください。

有効な _.template.config/template.json_ ファイルを用意したので、テンプレートをインストールする準備ができました。 ターミナルで _extensions_ フォルダーに移動し、次のコマンドを実行して現在のフォルダーにあるテンプレートをインストールします。

* **Windows の場合**: `dotnet new -i .\`
* **Linux または macOS の場合**: `dotnet new -i ./`

このコマンドにより、インストールされているテンプレートの一覧が出力されます。作成したテンプレートも含まれているはずです。

```console
C:\working\templates\extensions> dotnet new -i .\
Usage: new [options]

Options:
  -h, --help          Displays help for this command.
  -l, --list          Lists templates containing the specified name. If no name is specified, lists all templates.

... cut to save space ...

Templates                                         Short Name            Language          Tags
-------------------------------------------------------------------------------------------------------------------------------
Example templates: string extensions              stringext             [C#]              Common/Code
Console Application                               console               [C#], F#, VB      Common/Console
Class library                                     classlib              [C#], F#, VB      Common/Library
WPF Application                                   wpf                   [C#], VB          Common/WPF
Windows Forms (WinForms) Application              winforms              [C#], VB          Common/WinForms
Worker Service                                    worker                [C#]              Common/Worker/Web
```

## <a name="test-the-item-template"></a>項目テンプレートをテストする

項目テンプレートをインストールしたので、テストします。 _test/_ フォルダーに移動し、`dotnet new console` を使用して新しいコンソール アプリケーションを作成します。 これにより、`dotnet run` コマンドを使用して簡単にテストできる作業プロジェクトが生成されます。

```dotnetcli
dotnet new console
```

次のような出力が得られます。

```console
The template "Console Application" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on C:\test\test.csproj...
  Restore completed in 54.82 ms for C:\test\test.csproj.

Restore succeeded.
```

以下を使用してプロジェクトを実行します。

```dotnetcli
dotnet run
```

次の出力が得られます。

```console
Hello World!
```

次に、`dotnet new stringext` を実行して、テンプレートから _CommonExtensions.cs_ を生成します。

```dotnetcli
dotnet new stringext
```

次の出力が得られます。

```console
The template "Example templates: string extensions" was created successfully.
```

テンプレートによって提供される拡張メソッドを使用して、`"Hello World"` 文字列を反転するように _Program.cs_ 内のコードを変更します。

```csharp
Console.WriteLine("Hello World!".Reverse());
```

プログラムをもう一度実行すると、結果が反転されたことを確認できます。

```dotnetcli
dotnet run
```

次の出力が得られます。

```console
!dlroW olleH
```

おめでとうございます! .NET Core で項目テンプレートを作成し、配置しました。 このチュートリアル シリーズの次のパートの準備として、作成したテンプレートをアンインストールする必要があります。 また、必ず _test_ フォルダーからすべてのファイルを削除してください。 これにより、このチュートリアルの次の主要なセクションの準備が整った状態に戻ります。

## <a name="uninstall-the-template"></a>テンプレートをアンインストールする

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
  C:\working\templates\extensions
    Templates:
      Example templates: string extensions (stringext) C#
```

テンプレートをアンインストールするには、次のコマンドを実行します。

```dotnetcli
dotnet new -u C:\working\templates\extensions
```

## <a name="next-steps"></a>次の手順

このチュートリアルでは、項目テンプレートを作成しました。 プロジェクト テンプレートを作成する方法については、このチュートリアル シリーズの続きを参照してください。

> [!div class="nextstepaction"]
> [プロジェクト テンプレートを作成する](cli-templates-create-project-template.md)
