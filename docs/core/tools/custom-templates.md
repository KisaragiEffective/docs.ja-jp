---
title: dotnet new のカスタム テンプレート
description: あらゆる種類の .NET プロジェクトまたはファイルのカスタム テンプレートについて説明します。
author: thraka
ms.date: 06/14/2019
ms.openlocfilehash: 8e1ac4ca21a8a90ad0f7c9bd3dd11281eb4a6e02
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73420876"
---
# <a name="custom-templates-for-dotnet-new"></a>dotnet new のカスタム テンプレート

[.NET Core SDK](https://dotnet.microsoft.com/download) には、既にインストールされて使用できる状態になっている多くのテンプレートが付属します。 [`dotnet new` コマンド](dotnet-new.md)は、テンプレートを使用する手段であるだけでなく、テンプレートをインストールおよびアンインストールする方法でもあります。 .NET Core 2.0 以降から、アプリ、サービス、ツール、クラス ライブラリなど、あらゆる種類のプロジェクトを対象に独自のテンプレートを作成できるようになりました。 構成ファイルなど、1 つまたは複数の独立ファイルを出力するテンプレートを作成することもできます。

NuGet の *.nupkg* ファイルを直接参照するか、テンプレートが含まれるファイル システム ディレクトリを指定することで、任意の NuGet フィード上の NuGet パッケージからカスタム テンプレートをインストールできます。 テンプレート エンジンの機能を利用すると、テンプレートを使うときに、値を置き換えたり、ファイルを含めたり除外したり、独自の処理操作を実行したりできます。

テンプレート エンジンはオープン ソースです。オンライン コード リポジトリは GitHub の [dotnet/templating](https://github.com/dotnet/templating/) にあります。 テンプレートのサンプルが必要であれば、[dotnet/dotnet-template-samples](https://github.com/dotnet/dotnet-template-samples) リポジトリをご覧ください。 GitHub の「[Available templates for dotnet new](https://github.com/dotnet/templating/wiki/Available-templates-for-dotnet-new)」 (dotnet new で利用できるテンプレート) には、サードパーティのテンプレートなど、テンプレートが他にもあります。 カスタム テンプレートの作成と利用の詳細については、「[dotnet new の独自のテンプレートを作成する方法](https://devblogs.microsoft.com/dotnet/how-to-create-your-own-templates-for-dotnet-new/)」と「[dotnet/templating GitHub リポジトリ Wiki](https://github.com/dotnet/templating/wiki)」を参照してください。

チュートリアルでテンプレートを作成するには、「[dotnet new のカスタム テンプレートを作成する](../tutorials/cli-templates-create-item-template.md)」チュートリアルをご利用ください。

### <a name="net-default-templates"></a>.NET の既定のテンプレート

[.NET Core SDK](https://dotnet.microsoft.com/download) をインストールすると、コンソール アプリ、クラス ライブラリ、単体テスト プロジェクト、ASP.NET Core アプリ ([Angular](https://angular.io/) プロジェクトと [React](https://facebook.github.io/react/) プロジェクトを含む)、構成ファイルなど、プロジェクトやファイルを作成するための 12 個を超える組み込みテンプレートが与えられます。 組み込みテンプレートの一覧を表示するには、`dotnet new` オプションを指定して `-l|--list` コマンドを実行します。

```dotnetcli
dotnet new --list
```

## <a name="configuration"></a>構成

テンプレートは次の部分から構成されます。

- ソース ファイルとフォルダー。
- 構成ファイル (*template.json*)。

### <a name="source-files-and-folders"></a>ソース ファイルとフォルダー

ソース ファイルとフォルダーには、`dotnet new <TEMPLATE>` コマンドの実行時にテンプレート エンジンで使用されるすべてのファイルとフォルダーが含まれます。 テンプレート エンジンは、ソース コードとして*実行可能なプロジェクト*を利用し、プロジェクトを生成するように設計されています。 これにはいくつかの利点があります。

- テンプレート エンジンでは、プロジェクトのソース コードに特別なトークンを挿入することを必要としません。
- コード ファイルは特別なファイルではなく、テンプレート エンジンで利用するために変更されることはありません。 そのため、プロジェクトの利用時に通常利用しているツールでテンプレート コンテンツに対応できます。
- 他のプロジェクトの場合と同じように、テンプレート プロジェクトをビルドし、実行し、デバッグします。
- *./.template.config/template.json* 構成ファイルをプロジェクトに追加するだけで、既存のプロジェクトからテンプレートを簡単に作成できます。

テンプレートに格納されるファイルやフォルダーは、正式な種類の .NET プロジェクトに限定されません。 テンプレート エンジンで出力として生成されるファイルが 1 つだけであっても、ソース ファイルとフォルダーは、テンプレートを使って作成するすべてのコンテンツで構成できます。

テンプレートによって生成されるファイルは、*template.json* 構成ファイルで提供するロジックと設定に基づいて変更できます。 ユーザーは、`dotnet new <TEMPLATE>` コマンドにオプションを渡すことによって、これらの設定を上書きすることができます。 カスタム ロジックの一般的な例は、テンプレートによって展開されるコード ファイル内のクラスや変数に名前を提供する場合です。

### <a name="templatejson"></a>template.json

*template.json* ファイルは、テンプレートのルート ディレクトリの *.template.config* フォルダーにあります。 このファイルは、テンプレート エンジンに構成情報を提供します。 最小構成には、次の表に示すメンバーが必要です。この最小構成があれば、実際に機能するテンプレートが作成されます。

| メンバー            | [種類]          | [説明] |
| ----------------- | ------------- | ----------- |
| `$schema`         | URI           | *template.json* ファイルの JSON スキーマ。 エディターが JSON スキーマ対応であれば、スキーマの指定時、JSON 編集機能が有効になります。 たとえば、[Visual Studio Code](https://code.visualstudio.com/) の場合、IntelliSense を有効にするためにこのメンバーが必要になります。 値として `http://json.schemastore.org/template` を使用します。 |
| `author`          | string        | テンプレートの作成者。 |
| `classifications` | array(string) | テンプレートのゼロ以上の特性。ユーザーがこれを利用し、テンプレートを探すことがあります。 *コマンドでテンプレートの一覧を生成したとき、* Tags`dotnet new -l|--list` 列が表示される場合、この列にも分類が表示されます。 |
| `identity`        | string        | このテンプレートの一意の名前。 |
| `name`            | string        | ユーザーに対して表示されるテンプレートの名前。 |
| `shortName`       | string        | テンプレートを選択するための既定の省略名。テンプレート名が GUI 経由で選択されるのではなく、ユーザーによって指定される環境に適用されます。 たとえば、CLI コマンドでコマンド プロンプトからテンプレートを利用するときに省略名が便利です。 |

*template.json* ファイルの完全スキーマは [JSON Schema Store](http://json.schemastore.org/template) にあります。 *template.json* ファイルについて詳しくは、[dotnet テンプレート wiki](https://github.com/dotnet/templating/wiki) をご覧ください。

#### <a name="example"></a>例

たとえば、次に示すテンプレート フォルダーには、2 つのコンテンツ ファイル *console.cs* と *readme.txt* が含まれます。 *template.json* ファイルが含まれる、 *.template.config* という名前の必須フォルダーがあることに注意してください。

```text
└───mytemplate
    │   console.cs
    │   readme.txt
    │
    └───.template.config
            template.json
```

*template.json* ファイルは次のようなものです。

```json
{
  "$schema": "http://json.schemastore.org/template",
  "author": "Travis Chau",
  "classifications": [ "Common", "Console" ],
  "identity": "AdatumCorporation.ConsoleTemplate.CSharp",
  "name": "Adatum Corporation Console Application",
  "shortName": "adatumconsole"
}
```

*mytemplate* フォルダーは、インストール可能なテンプレート パックです。 パックがインストールされたら、`shortName` を `dotnet new` コマンドで使うことができます。 たとえば、`dotnet new adatumconsole` では、`console.cs` および `readme.txt` ファイルが現在のフォルダーに出力されます。

## <a name="packing-a-template-into-a-nuget-package-nupkg-file"></a>テンプレートをパッケージ化し、NuGet パッケージ (nupkg ファイル) を作成する

カスタム テンプレートは、[dotnet pack](dotnet-pack.md) コマンドと *.csproj* ファイルでパッケージ化されます。 または、[nuget pack](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference) コマンドと [.nuspec](https://docs.microsoft.com/nuget/tools/cli-ref-pack) ファイルで *NuGet* を使うこともできます。 ただし、NuGet の場合、Windows では .NET Framework が、Linux と MacOS では [Mono](https://www.mono-project.com/) が必要です。

*.csproj* ファイルは、従来のコード プロジェクトの *.csproj* ファイルと若干異なります。 以下の設定に注意してください。

01. 設定 `<PackageType>` が追加され、`Template` に設定されます。
01. 設定 `<PackageVersion>` が追加され、有効な [NuGet のバージョン番号](/nuget/reference/package-versioning)に設定されます。
01. 設定 `<PackageId>` が追加され、一意の識別子に設定されます。 この識別子は、テンプレート パックのアンインストールに使われ、テンプレート パックを登録するために NuGet フィードによって使われます。
01. ジェネリック メタデータの設定 `<Title>`、`<Authors>`、`<Description>`、`<PackageTags>` を設定する必要があります。
01. テンプレート プロセスによって生成されるバイナリを使わない場合でも、設定 `<TargetFramework>` を設定する必要があります。 次の例では、`netstandard2.0` に設定されています。

*.nupkg* NuGet パッケージの形式のテンプレート パックでは、すべてのテンプレートをパッケージ内の *content* フォルダーに格納する必要があります。 生成された *.nupkg* をテンプレート パックとしてインストールできるようにするため、 *.csproj* ファイルに追加する設定がさらにいくつかあります。

01. プロジェクトで`<IncludeContentInPack>`コンテンツ`true`として設定されるすべてのファイルを NuGet パッケージに含めるには、設定 **を** に設定します。
01. コンパイラによって生成されたすべてのバイナリを NuGet パッケージから除外するには、設定 `<IncludeBuildOutput>` を `false` に設定します。
01. 設定 `<ContentTargetFolders>` を `content` に設定します。 これにより、**コンテンツ**として設定されたファイルは、NuGet パッケージ内の *content* フォルダーに格納されます。 NuGet パッケージのこのフォルダーは、dotnet テンプレート システムによって解析されます。

すべてのコード ファイルをテンプレート プロジェクトによってコンパイルされないように除外する簡単な方法は、プロジェクト ファイルの `<Compile Remove="**\*" />` 要素内で `<ItemGroup>` 項目を使うことです。

テンプレート パックを構成する簡単な方法は、すべてのテンプレートを個別のフォルダーに格納した後、 *.csproj* ファイルと同じディレクトリにある *templates* フォルダーの内部に各テンプレート フォルダーを格納することです。 これにより、1 つのプロジェクト項目を使って、すべてのファイルとフォルダーを*コンテンツ*として **templates** に含めることができます。 `<ItemGroup>` 要素の内部に、`<Content Include="templates\**\*" Exclude="templates\**\bin\**;templates\**\obj\**" />` 項目を作成します。

上記のすべてのガイドラインに従う *.csproj* ファイルの例を次に示します。 *templates* 子フォルダーを *content* パッケージ フォルダーにパッケージ化し、すべてのコード ファイルをコンパイルから除外します。

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

次の例では、 *.csproj* を使用してテンプレート パックを作成するファイルとフォルダーの構造を示します。 *MyDotnetTemplates.csproj* ファイルと *templates* フォルダーはどちらも、*project_folder* という名前のディレクトリのルートにあります。 *templates* フォルダーには、*mytemplate1* と *mytemplate2* の 2 つのテンプレートが含まれています。 各テンプレートには、コンテンツ ファイルと、*template.json* 構成ファイルが格納された *.template.config* フォルダーが含まれます。

```text
project_folder
│   MyDotnetTemplates.csproj
│
└───templates
    ├───mytemplate1
    │   │   console.cs
    │   │   readme.txt
    │   │
    │   └───.template.config
    │           template.json
    │
    └───mytemplate2
        │   otherfile.cs
        │
        └───.template.config
                template.json
```

## <a name="installing-a-template"></a>テンプレートをインストールする

パッケージをインストールするには、[dotnet new -i|--install](dotnet-new.md) コマンドを使います。

### <a name="to-install-a-template-from-a-nuget-package-stored-at-nugetorg"></a>nuget.org に保存されている NuGet パッケージからテンプレートをインストールするには

テンプレート パッケージをインストールするには、NuGet パッケージ識別子を使います。

```dotnetcli
dotnet new -i <NUGET_PACKAGE_ID>
```

### <a name="to-install-a-template-from-a-local-nupkg-file"></a>ローカル nupkg ファイルからテンプレートをインストールするには

*.nupkg* NuGet パッケージ ファイルに対するパスを指定します。

```dotnetcli
dotnet new -i <PATH_TO_NUPKG_FILE>
```

### <a name="to-install-a-template-from-a-file-system-directory"></a>ファイル システム ディレクトリからテンプレートをインストールするには

テンプレートはテンプレート フォルダーからインストールできます (上の例の *mytemplate1* フォルダーなど)。 *.template.config* フォルダーのフォルダー パスを指定します。 テンプレート ディレクトリへのパスは、絶対パスである必要はありません。 ただし、フォルダーからインストールされたテンプレートをアンインストールするには、絶対パスが必要です。

```dotnetcli
dotnet new -i <FILE_SYSTEM_DIRECTORY>
```

## <a name="get-a-list-of-installed-templates"></a>インストールされているテンプレートの一覧を取得する

他のパラメーターを何も指定しない uninstall コマンドでは、インストールされているすべてのテンプレートの一覧が表示されます。

```dotnetcli
dotnet new -u
```

そのコマンドでは、次のような出力が返されます。

```console
Template Instantiation Commands for .NET Core CLI

Currently installed items:
  Microsoft.DotNet.Common.ItemTemplates
    Templates:
      global.json file (globaljson)
      NuGet Config (nugetconfig)
      Solution File (sln)
      Dotnet local tool manifest file (tool-manifest)
      Web Config (webconfig)
  Microsoft.DotNet.Common.ProjectTemplates.3.0
    Templates:
      Class library (classlib) C#
      Class library (classlib) F#
      Class library (classlib) VB
      Console Application (console) C#
      Console Application (console) F#
      Console Application (console) VB
...
```

`Currently installed items:` の後の第 1 レベルの項目は、テンプレートのアンインストールで使われる識別子です。 上の例では、`Microsoft.DotNet.Common.ItemTemplates` と `Microsoft.DotNet.Common.ProjectTemplates.3.0` の一覧が表示されています。 ファイル システム パスを使ってテンプレートをインストールした場合、この識別子は *.template.config* フォルダーのフォルダー パスです。

## <a name="uninstalling-a-template"></a>テンプレートをアンインストールする

パッケージをアンインストールするには、[dotnet new -u|--uninstall](dotnet-new.md) コマンドを使います。

NuGet フィードまたは直接 *.nupkg* ファイルでパッケージをインストールした場合は、識別子を指定します。

```dotnetcli
dotnet new -u <NUGET_PACKAGE_ID>
```

*.template.config* フォルダーに対するパスを指定してパッケージをインストールした場合は、その**絶対**パスを使ってパッケージをアンインストールします。 テンプレートの絶対パスは、`dotnet new -u` コマンドによって提供される出力で確認できます。 詳しくは、前の「[インストールされているテンプレートの一覧を取得する](#get-a-list-of-installed-templates)」セクションをご覧ください。

```dotnetcli
dotnet new -u <ABSOLUTE_FILE_SYSTEM_DIRECTORY>
```

## <a name="create-a-project-using-a-custom-template"></a>カスタム テンプレートを利用してプロジェクトを作成する

テンプレートがインストールされたら、事前インストールされている他のテンプレートの場合と同様に、`dotnet new <TEMPLATE>` コマンドを実行してテンプレートを使用します。 テンプレートの設定で構成したテンプレート固有のオプションなど、[オプション](dotnet-new.md#options)を `dotnet new` コマンドに対して指定することもできます。 テンプレートの省略名 (短い名前) をコマンドに直接指定します。

```dotnetcli
dotnet new <TEMPLATE>
```

## <a name="see-also"></a>参照

- [dotnet new のカスタム テンプレートを作成する (チュートリアル)](../tutorials/cli-templates-create-item-template.md)
- [dotnet/templating GitHub リポジトリ Wiki](https://github.com/dotnet/templating/wiki)
- [dotnet/dotnet-template-samples GitHub リポジトリ](https://github.com/dotnet/dotnet-template-samples)
- [dotnet new の独自のテンプレートを作成する方法](https://devblogs.microsoft.com/dotnet/how-to-create-your-own-templates-for-dotnet-new/)
- [JSON Schema Store の *template.json* スキーマ](http://json.schemastore.org/template)
