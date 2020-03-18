---
title: Windows フォーム アプリを .NET Core に移植する
description: .NET Framework Windows フォーム アプリケーションを .NET Core for Windows に移植する方法について説明します。
author: Thraka
ms.author: adegeo
ms.date: 03/01/2019
ms.openlocfilehash: dbd522851faa0a4fe435199914a034ee230d3455
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76116030"
---
# <a name="how-to-port-a-windows-forms-desktop-app-to-net-core"></a>Windows フォーム デスクトップ アプリを .NET Core に移植する方法

この記事では、Windows フォームベースのデスクトップ アプリを、.NET Framework から .NET Core 3.0 以降に移植する方法について説明します。 .NET Core 3.0 SDK には、Windows Forms アプリケーションのサポートが含まれています。 Windows Forms は、まだ Windows 専用のフレームワークであるため、Windows 上でのみ実行されます。 この例では、.NET Core CLI を使用して、プロジェクトの作成と管理を行います。

この記事では、さまざまな名前が使用して、移行で使用されるファイルの種類を識別しています。 実際のファイルの名前とは異なっているため、自分のプロジェクトを移行するときは、次の一覧の名前に置き換えて説明をお読みください。

| ファイル | [説明] |
| ---- | ----------- |
| **MyApps.sln** | ソリューション ファイルの名前。 |
| **MyForms.csproj** | 移植する .NET Framework Windows Forms プロジェクトの名前。 |
| **MyFormsCore.csproj** | 作成する新しい .NET Core プロジェクトの名前。 |
| **MyAppCore.exe** | .NET Core Windows Forms アプリの実行可能ファイル。 |

## <a name="prerequisites"></a>前提条件

- 実行したいデザイナー作業用の [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)。

  次の Visual Studio ワークロードをインストールします。
  - .NET デスクトップ開発
  - .NET Core クロスプラットフォームの開発

- 問題なくビルドされて実行されているソリューション内の作業用 Windows Forms プロジェクト。
- C# でコードを書いたプロジェクト。
- [.NET Core](https://dotnet.microsoft.com/download/dotnet-core) 3.0 以降

> [!NOTE]
> **Visual Studio 2017** では、.NET Core 3.0 プロジェクトはサポートされていません。 **Visual Studio 2019** では .NET Core 3.0 プロジェクトはサポートされていますが、.NET Core 3.0 Windows Forms プロジェクト用のビジュアル デザイナーはまだサポートされていません。 ビジュアル デザイナーを使用するには、.NET Core プロジェクトとフォーム ファイルを共有するソリューション内に .NET Windows フォーム プロジェクトを配置する必要です。

### <a name="consider"></a>次の例を考えてみましょう

.NET Framework Windows Forms アプリケーションを移植するときは、いくつかの点を考慮する必要があります。

01. アプリケーションが移行に適した候補であることを確認する。

    [.NET Portability Analyzer](../../standard/analyzers/portability-analyzer.md) を使用して、プロジェクトを .NET Core 3.0 に移行できるかどうかを判断します。 プロジェクトが .NET Core 3.0 では問題が発生する場合は、アナライザーによってこれらの問題を特定できます。

01. Windows Forms の別のバージョンを使用している。

    .NET Core 3.0 Preview 1 がリリースされたときに、Windows フォームは GitHub でオープン ソースになりました。 .NET Core Windows フォームのコードは、.NET Framework Windows フォーム コード ベースからの 1 つの分岐です。 違いの存在によって、アプリが移植されない可能性があります。

01. [Windows 互換機能パック][compat-pack]が移行に役立つ可能性がある。

    .NET Framework で利用できる一部の API は、.NET Core 3.0 では利用できません。 [Windows 互換機能パック][compat-pack]によって多数の API が追加され、Windows Forms アプリの .NET Core との互換性が促進される可能性があります。

01. 自分のプロジェクトで使用されている NuGet パッケージを更新する。

    移行する前に、常に NuGet パッケージの最新バージョンを使用することをお勧めします。 アプリケーションで NuGet パッケージを参照している場合は、最新バージョンに更新してください。 アプリケーションが正常にビルドされることを確認します。 アップグレード後にパッケージ エラーが発生した場合は、コードを壊さない最新のバージョンにパッケージをダウングレードします。

01. Visual Studio 2019 では、.NET Core 3.0 のフォーム デザイナーはまだサポートされていない

    現時点では、Visual Studio のフォーム デザイナーを使用する場合は、既存の .NET Framework Windows Forms プロジェクト ファイルを保持する必要があります。

## <a name="create-a-new-sdk-project"></a>新しい SDK プロジェクトを作成する

作成する新しい .NET Core 3.0 プロジェクトは、.NET Framework プロジェクトとは別のディレクトリに配置する必要があります。 両方が同じディレクトリに配置されている場合は、**obj** ディレクトリ内に生成されているファイルと競合する可能性があります。 この例では、**SolutionFolder** ディレクトリに **MyFormsAppCore** という名前のディレクトリを作成します。

```
SolutionFolder
├───MyApps.sln
├───MyFormsApp
│   └───MyForms.csproj
└───MyFormsAppCore      <--- New folder for core project
```

次に、**MyFormsAppCore** ディレクトリに **MyFormsCore.csproj** プロジェクトを作成する必要があります。 このファイルは、任意のテキスト エディターを使用して手動で作成できます。 次の XML に貼り付けます。

```xml
<Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>netcoreapp3.0</TargetFramework>
    <UseWindowsForms>true</UseWindowsForms>
  </PropertyGroup>

</Project>
```

プロジェクト ファイルを手動で作成しない場合は、Visual Studio または .NET Core SDK を使用して生成できます。 ただし、プロジェクト ファイル以外のプロジェクト テンプレートによって生成されるすべてのファイルを削除する必要があります。 SDK を使用するには、次のコマンドを **SolutionFolder** ディレクトリから実行します。

```dotnetcli
dotnet new winforms -o MyFormsAppCore -n MyFormsCore
```

**MyFormsCore.csproj** を作成した後、ディレクトリ構造は次のようになります。

```
SolutionFolder
├───MyApps.sln
├───MyFormsApp
│   └───MyForms.csproj
└───MyFormsAppCore
    └───MyFormsCore.csproj
```

**SolutionFolder** ディレクトリから Visual Studio または .NET Core CLI を使用して、**MyFormsCore.csproj** プロジェクトを **MyApps.sln** に追加します。

```dotnetcli
dotnet sln add .\MyFormsAppCore\MyFormsCore.csproj
```

## <a name="fix-assembly-info-generation"></a>アセンブリ情報の生成を修正する

.NET Framework で作成された Windows Forms プロジェクトには、生成されるアセンブリのバージョンなどのアセンブリの属性を格納する `AssemblyInfo.cs` ファイルが含まれています。 SDK スタイルのプロジェクトでは、SDK プロジェクト ファイルに基づいて、この情報が自動的に生成されます。 両方の種類の "アセンブリ情報" があると、競合が発生します。 この問題は、自動生成を無効にして、既存の `AssemblyInfo.cs` ファイルをプロジェクトに強制的に使用させることで解決します。

メインの `<PropertyGroup>` ノードに追加する 3 つの設定があります。

- **GenerateAssemblyInfo**\
このプロパティを `false` に設定すると、アセンブリ属性が生成されなくなります。 これにより、.NET Framework プロジェクトからの既存の `AssemblyInfo.cs` ファイルとの競合を回避できます。

- **AssemblyName**\
このプロパティの値は、コンパイル時に作成される出力バイナリです。 名前には、拡張子を追加する必要はありません。 たとえば、`MyCoreApp` を使用すると、`MyCoreApp.exe` が作成されます。

- **RootNamespace**\
プロジェクトで使用される既定の名前空間。 これは、.NET Framework プロジェクトの既定の名前空間と一致させる必要があります。

これら 3 つの要素を、`<PropertyGroup>` ファイルの `MyFormsCore.csproj` ノードに追加します。

```xml
<Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>netcoreapp3.0</TargetFramework>
    <UseWindowsForms>true</UseWindowsForms>

    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    <AssemblyName>MyCoreApp</AssemblyName>
    <RootNamespace>WindowsFormsApp1</RootNamespace>
  </PropertyGroup>

</Project>
```

## <a name="add-source-code"></a>ソース コードを追加する

現時点では、**MyFormsCore.csproj** プロジェクトには、コンパイルされるコードはありません。 既定では、.NET Core プロジェクトには、現在のディレクトリとすべての子ディレクトリ内のすべてのソース コードが自動的に含まれます。 相対パスを使用して、.NET Framework プロジェクトのコードが含まれるようにプロジェクトを構成する必要があります。 .NET Framework プロジェクトで、フォームのアイコンとリソース用の **.resx** ファイルが使用されている場合は、それらも含める必要があります。

次の `<ItemGroup>` コードを、プロジェクトに追加します。 各ステートメントには、子ディレクトリを含むファイルの glob パターンが含まれています。

```xml
  <ItemGroup>
    <Compile Include="..\MyFormsApp\**\*.cs" />
    <EmbeddedResource Include="..\MyFormsApp\**\*.resx" />
  </ItemGroup>
```

または、.NET Framework プロジェクトの各ファイルに対する `<Compile>` または `<EmbeddedResource>` エントリを作成できます。

## <a name="add-nuget-packages"></a>NuGet パッケージの追加

.NET Framework プロジェクトによって参照される各 NuGet パッケージを.NET Core プロジェクトに追加します。

ほとんどの場合、.NET Framework Windows Forms アプリには、**packages.config** プロジェクトによって参照されるすべての NuGet パッケージの一覧を含むファイルがあります。 この一覧を調べて、.NET Core プロジェクトに追加する NuGet パッケージを決定できます。 たとえば、.NET Framework プロジェクトで `MetroFramework`、`MetroFramework.Design`、および `MetroFramework.Fonts` NuGet パッケージが参照されている場合は、**SolutionFolder** ディレクトリから Visual Studio または .NET Core CLI のいずれかを使用して、それぞれをプロジェクトに追加します。

```dotnetcli
dotnet add .\MyFormsAppCore\MyFormsCore.csproj package MetroFramework
dotnet add .\MyFormsAppCore\MyFormsCore.csproj package MetroFramework.Design
dotnet add .\MyFormsAppCore\MyFormsCore.csproj package MetroFramework.Fonts
```

上のコマンドによって、次の NuGet 参照が **MyFormsCore.csproj** プロジェクトに追加されます。

```xml
  <ItemGroup>
    <PackageReference Include="MetroFramework" Version="1.2.0.3" />
    <PackageReference Include="MetroFramework.Design" Version="1.2.0.3" />
    <PackageReference Include="MetroFramework.Fonts" Version="1.2.0.3" />
  </ItemGroup>
```

## <a name="port-control-libraries"></a>移植制御ライブラリ

Windows Forms 制御ライブラリ プロジェクトを移植する場合の指示は、.NET Framework Windows Forms アプリ プロジェクトの移植と同じですが、いくつかの設定が異なります。 また、実行可能ファイルにコンパイルするのではなく、ライブラリにコンパイルします。 実行可能プロジェクトとライブラリ プロジェクト間の違いは、ソース コードを含むファイルの glob パス以外はごくわずかです。

前の手順の例を使用して、作業するプロジェクトとファイルを展開しましょう。

| ファイル | [説明] |
| ---- | ----------- |
| **MyApps.sln** | ソリューション ファイルの名前。 |
| **MyControls.csproj** | 移植する .NET Framework Windows Forms 制御プロジェクトの名前。 |
| **MyControlsCore.csproj** | 作成する新しい .NET Core ライブラリ プロジェクトの名前。 |
| **MyCoreControls.dll** | .NET Core Windows Forms 制御ライブラリ。 |

```
SolutionFolder
├───MyApps.sln
├───MyFormsApp
│   └───MyForms.csproj
├───MyFormsAppCore
│   └───MyFormsCore.csproj
│
├───MyFormsControls
│   └───MyControls.csproj
└───MyFormsControlsCore
    └───MyControlsCore.csproj   <--- New project for core controls
```

`MyControlsCore.csproj` プロジェクトと前に作成した `MyFormsCore.csproj` プロジェクト間の違いを考えてみてください。

```diff
 <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

   <PropertyGroup>
-    <OutputType>WinExe</OutputType>
     <TargetFramework>netcoreapp3.0</TargetFramework>
     <UseWindowsForms>true</UseWindowsForms>

     <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
-    <AssemblyName>MyCoreApp</AssemblyName>
-    <RootNamespace>WindowsFormsApp1</RootNamespace>
+    <AssemblyName>MyControlsCore</AssemblyName>
+    <RootNamespace>WindowsFormsControlLibrary1</RootNamespace>
   </PropertyGroup>

   <ItemGroup>
-    <Compile Include="..\MyFormsApp\**\*.cs" />
-    <EmbeddedResource Include="..\MyFormsApp\**\*.resx" />
+    <Compile Include="..\MyFormsControls\**\*.cs" />
+    <EmbeddedResource Include="..\MyFormsControls\**\*.resx" />
   </ItemGroup>

 </Project>
```

.NET Core Windows Forms 制御ライブラリ プロジェクト ファイルの例を次に示します。

```xml
<Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

  <PropertyGroup>

    <TargetFramework>netcoreapp3.0</TargetFramework>
    <UseWindowsForms>true</UseWindowsForms>

    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    <AssemblyName>MyCoreControls</AssemblyName>
    <RootNamespace>WindowsFormsControlLibrary1</RootNamespace>
  </PropertyGroup>

  <ItemGroup>
    <Compile Include="..\MyFormsControls\**\*.cs" />
    <EmbeddedResource Include="..\MyFormsControls\**\*.resx" />
  </ItemGroup>

</Project>
```

ご覧のとおり、`<OutputType>` ノードが削除され、コンパイラでは、実行可能ファイルではなくライブラリが既定で生成されます。 `<AssemblyName>` と `<RootNamespace>` が変更されました。 具体的には、`<RootNamespace>` が、移植する Windows Forms 制御ライブラリの名前空間と一致するようになりました。 最後に、`<Compile>` と `<EmbeddedResource>` ノードが、移植する Windows Forms 制御ライブラリのフォルダーを指すように調整されました。

次に、メインの .NET Core **MyFormsCore.csproj** プロジェクトに、新しい .NET Core Windows フォーム コントロール ライブラリへの参照を追加します。 **SolutionFolder** ディレクトリから、Visual Studio または .NET Core CLI のいずれかを使用して、参照を追加します。

```dotnetcli
dotnet add .\MyFormsAppCore\MyFormsCore.csproj reference .\MyFormsControlsCore\MyControlsCore.csproj
```

前のコマンドにより、以下が **MyFormsCore.csproj** プロジェクトに追加されます。

```xml
  <ItemGroup>
    <ProjectReference Include="..\MyFormsControlsCore\MyControlsCore.csproj" />
  </ItemGroup>
```

## <a name="compilation-problems"></a>コンパイルの問題

プロジェクトのコンパイルで問題が発生した場合は、.NET Framework では使用できるが .NET Core では使用できない Windows 専用 API が使用されている可能性があります。 [Windows 互換機能パック][compat-pack] NuGet パッケージのプロジェクトへの追加を試すことができます。 このパッケージは Windows でのみ実行され、約 20,000 の Windows API を .NET Core と .NET Standard プロジェクトに追加します。

```dotnetcli
dotnet add .\MyFormsAppCore\MyFormsCore.csproj package Microsoft.Windows.Compatibility
```

前のコマンドにより、以下が **MyFormsCore.csproj** プロジェクトに追加されます。

```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.Windows.Compatibility" Version="3.1.0" />
  </ItemGroup>
```

## <a name="windows-forms-designer"></a>Windows フォーム デザイナー

この記事で説明したように、Visual Studio 2019 では、.NET Framework プロジェクトでのみフォーム デザイナーがサポートされます。 サイドバイサイドの .NET Core プロジェクトを作成することで、.NET Framework プロジェクトを使用してフォームをデザインしながら、.NET Core でプロジェクトをテストできます。 ソリューション ファイルには、.NET Framework と .NET Core の両方のプロジェクトが含まれます。 .NET Framework プロジェクトにフォームとコントロールを追加してデザインし、.NET Core プロジェクトに追加されたファイルの glob パターンに基づいて、新しいファイルまたは変更されたファイルが、.NET Core プロジェクトに自動的に追加されます。

Visual Studio 2019 で Windows Forms デザイナーがサポートされるようになったら、.NET Core プロジェクト ファイルの内容を .NET Framework プロジェクト ファイルにコピーして貼り付けることができます。 その後、`<Source>` と `<EmbeddedResource>` 項目を使用して追加されたファイルの glob パターンを削除します。 アプリで使用されるすべてのプロジェクト参照のパスを修正します。 これにより、.NET Framework プロジェクトが .NET Core プロジェクトに効率的にアップグレードされます。

## <a name="next-steps"></a>次のステップ

- [.NET Framework から .NET Core への破壊的変更](../compatibility/fx-core.md)について学習する。
- [Windows 互換機能パック][compat-pack]の詳細を確認する。
- .NET Framework Windows Forms プロジェクトの .NET Core への[移植に関するビデオ](https://www.youtube.com/watch?v=upVQEUc_KwU)を視聴する。

[compat-pack]: windows-compat-pack.md
