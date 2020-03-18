---
title: dotnet new 用のテンプレート パックを作成する
description: dotnet new コマンド用のテンプレート パックをビルドする csproj ファイルを作成する方法を説明します。
author: thraka
ms.date: 12/10/2019
ms.topic: tutorial
ms.author: adegeo
ms.openlocfilehash: 5bc926861dd6a501d7c2d24bd5f7c4116cc78b2c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77503489"
---
# <a name="tutorial-create-a-template-pack"></a>チュートリアル: テンプレート パックを作成する

.NET Core では、プロジェクト、ファイル、さらにはリソースを生成するテンプレートを作成して配置することができます。 このチュートリアルは、`dotnet new` コマンドで使用するテンプレートの作成、インストール、アンインストール方法を説明するシリーズのパート 3 です。

シリーズのこのパートで学習する内容は次のとおりです。

> [!div class="checklist"]
>
> * テンプレート パックをビルドするための \*.csproj プロジェクトを作成する
> * パックのためのプロジェクト ファイルを構成する
> * NuGet パッケージ ファイルからテンプレートをインストールする
> * パッケージ ID を使用してテンプレートをアンインストールする

## <a name="prerequisites"></a>必須コンポーネント

* このチュートリアル シリーズの[パート 1](cli-templates-create-item-template.md) と[パート 2](cli-templates-create-project-template.md) を完了します。

  このチュートリアルでは、チュートリアルの最初の 2 つのパートで作成した 2 つのテンプレートを使用します。 そのテンプレートをフォルダーとして _working\templates\\_ フォルダー内にコピーしていれば、別のテンプレートを使用することもできます。

* ターミナルを開いて _working\\_ フォルダーに移動します。

## <a name="create-a-template-pack-project"></a>テンプレート パック プロジェクトを作成する

テンプレート パックは、1 つのファイルにパッケージ化された 1 つ以上のテンプレートです。 パックをインストール/アンインストールすると、そのパックに含まれているすべてのテンプレートが追加/削除されます。 このチュートリアル シリーズのこれまでのパートでは、個別のテンプレートのみを扱いました。 パックされていないテンプレートを共有するには、テンプレート フォルダーをコピーし、そのフォルダーを介してインストールする必要があります。 テンプレート パックは複数のテンプレートを含めることができ、1つのファイルであるため、より簡単に共有できます。

テンプレート パックは、NuGet パッケージ ( _.nupkg_) ファイルによって表されます。 また、すべての NuGet パッケージと同様に、テンプレート パックを NuGet フィードにアップロードできます。 `dotnet new -i` コマンドでは、NuGet パッケージ フィードからのテンプレート パックのインストールがサポートされています。 さらに、 _.nupkg_ ファイルからテンプレート パックを直接インストールすることもできます。

通常は、C# プロジェクト ファイルを使用してコードをコンパイルし、バイナリを生成します。 しかし、プロジェクトを使用してテンプレート パックを生成することもできます。 _.csproj_ の設定を変更して、コードがコンパイルされないようにし、代わりにテンプレートのすべての資産をリソースとして含めることができます。 このプロジェクトをビルドすると、テンプレート パックの NuGet パッケージが生成されます。

作成するパックには、これまでに作成した[項目テンプレート](cli-templates-create-item-template.md)と[パッケージ テンプレート](cli-templates-create-project-template.md)が含まれます。 これらの 2 つのテンプレートを _working\templates\\_ フォルダーでグループ化したので、 _.csproj_ ファイル用に _working_ フォルダーを使用できます。

ターミナルで _working_ フォルダーに移動します。 新しいプロジェクトを作成し、名前を `templatepack`、出力フォルダーを現在のフォルダーに設定します。

```dotnetcli
dotnet new console -n templatepack -o .
```

`-n` パラメーターを指定すると、 _.csproj_ のファイル名が _templatepack.csproj_ に設定されます。 `-o` パラメーターを指定すると、現在のディレクトリにファイルが作成されます。 次の出力のような結果が表示されます。

```dotnetcli
dotnet new console -n templatepack -o .
```

```console
The template "Console Application" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on .\templatepack.csproj...
  Restore completed in 52.38 ms for C:\working\templatepack.csproj.

Restore succeeded.
```

次に、お好みのテキスト エディターで _templatepack.csproj_ ファイルを開き、コンテンツを次の XML で置き換えます。

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <PackageType>Template</PackageType>
    <PackageVersion>1.0</PackageVersion>
    <PackageId>AdatumCorporation.Utility.Templates</PackageId>
    <Title>AdatumCorporation Templates</Title>
    <Authors>Me</Authors>
    <Description>Templates to use when creating an application for Adatum Corporation.</Description>
    <PackageTags>dotnet-new;templates;contoso</PackageTags>

    <TargetFramework>netstandard2.0</TargetFramework>

    <IncludeContentInPack>true</IncludeContentInPack>
    <IncludeBuildOutput>false</IncludeBuildOutput>
    <ContentTargetFolders>content</ContentTargetFolders>
  </PropertyGroup>

  <ItemGroup>
    <Content Include="templates\**\*" Exclude="templates\**\bin\**;templates\**\obj\**" />
    <Compile Remove="**\*" />
  </ItemGroup>

</Project>
```

上の XML 内の `<PropertyGroup>` の設定は、3 つのグループに分かれています。 最初のグループでは、NuGet パッケージに必要なプロパティが処理されます。 `<Package` の 3 つの設定は、NuGet フィード上でパッケージを識別するための NuGet パッケージ プロパティと関係しています。 具体的には、`<PackageId>` 値は、ディレクトリ パスではなく単一の名前を使用してテンプレート パックをアンインストールするために使用されます。 また、NuGet フィードからテンプレート パックをインストールするためにも使用されます。 `<Title>`、`<PackageTags>` などの残りの設定は、NuGet フィードに表示されるメタデータに関連しています。 NuGet の設定の詳細については、[NuGet と MSBuild のプロパティ](/nuget/reference/msbuild-targets)に関する記事を参照してください。

`<TargetFramework>` の設定は、パック コマンドを実行してプロジェクトのコンパイルとパッケージ化を行うときに MSBuild が正しく実行されるように設定する必要があります。

最後の 3 つの設定は、プロジェクトが作成されたときにテンプレートが NuGet パック内の適切なフォルダーに含まれるよう、プロジェクトを正しく構成することに関連しています。

`<ItemGroup>` には 2 つの設定が含まれています。 まず、`<Content>` 設定では、_templates_ フォルダー内のすべてのものがコンテンツとして含められます。 また、コンパイル済みのコード (テンプレートをテストしてコンパイルした場合) が含まれないようにするために、すべての _bin_ または _obj_ フォルダーを除外するよう設定されます。 次に、`<Compile>` 設定では、すべてのコード ファイルがその場所に関係なくコンパイルから除外されます。 この設定により、テンプレート パックの作成に使用されているプロジェクトで _templates_ フォルダー階層内のコードのコンパイルが試行されなくなります。

## <a name="build-and-install"></a>ビルドしてインストールする

このファイルを保存してから、パック コマンドを実行します。

```dotnetcli
dotnet pack
```

このコマンドを実行すると、プロジェクトがビルドされ、_working\bin\Debug_ フォルダー内に NuGet パッケージが作成されます。

```dotnetcli
dotnet pack
```

```console
Microsoft (R) Build Engine version 16.2.0-preview-19278-01+d635043bd for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 123.86 ms for C:\working\templatepack.csproj.

  templatepack -> C:\working\bin\Debug\netstandard2.0\templatepack.dll
  Successfully created package 'C:\working\bin\Debug\AdatumCorporation.Utility.Templates.1.0.0.nupkg'.
```

次に、`dotnet new -i PATH_TO_NUPKG_FILE` コマンドを使用してテンプレート パック ファイルをインストールします。

```console
C:\working> dotnet new -i C:\working\bin\Debug\AdatumCorporation.Utility.Templates.1.0.0.nupkg
Usage: new [options]

Options:
  -h, --help          Displays help for this command.
  -l, --list          Lists templates containing the specified name. If no name is specified, lists all templates.

... cut to save space ...

Templates                                         Short Name            Language          Tags
-------------------------------------------------------------------------------------------------------------------------------
Example templates: string extensions              stringext             [C#]              Common/Code
Console Application                               console               [C#], F#, VB      Common/Console
Example templates: async project                  consoleasync          [C#]              Common/Console/C#8
Class library                                     classlib              [C#], F#, VB      Common/Library
```

NuGet パッケージを NuGet フィードにアップロードした場合は、`dotnet new -i PACKAGEID` コマンドを使用できます。ここで、`PACKAGEID` は _.csproj_ ファイルに含まれる `<PackageId>` 設定と同じです。 このパッケージ ID は、NuGet パッケージ識別子と同じです。

## <a name="uninstall-the-template-pack"></a>テンプレート パックをアンインストールする

テンプレート パックを削除する方法は、テンプレート パックをインストールした方法 ( _.nupkg_ ファイルを使用して直接、または NuGet フィードを使用) にかかわらず同じです。 アンインストールするテンプレートの `<PackageId>` を使用します。 `dotnet new -u` コマンドを実行すると、インストールされているテンプレートの一覧を取得できます。

```dotnetcli
dotnet new -u
```

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
  AdatumCorporation.Utility.Templates
    Templates:
      Example templates: async project (consoleasync) C#
      Example templates: string extensions (stringext) C#
```

`dotnet new -u AdatumCorporation.Utility.Templates` を実行してテンプレートをアンインストールします。 `dotnet new` コマンドを実行するとヘルプ情報が出力され、前にインストールしたテンプレートが除外されているはずです。

おめでとうございます! テンプレート パックをインストールしてアンインストールしました。

## <a name="next-steps"></a>次の手順

テンプレートの詳細については、ほとんどを既に説明しましたが、記事「[dotnet new のカスタム テンプレート](../tools/custom-templates.md)」を参照してください。

* [dotnet/templating GitHub リポジトリ Wiki](https://github.com/dotnet/templating/wiki)
* [dotnet/dotnet-template-samples GitHub リポジトリ](https://github.com/dotnet/dotnet-template-samples)
* [JSON Schema Store の *template.json* スキーマ](http://json.schemastore.org/template)
