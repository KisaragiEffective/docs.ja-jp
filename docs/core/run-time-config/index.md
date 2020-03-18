---
title: ランタイムの構成オプション
description: ランタイム構成設定を使用して .NET Core アプリケーションを構成する方法について説明します。
ms.date: 01/21/2020
ms.openlocfilehash: ddf68c30e620a06856f65e71bd050e1b77618f20
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79397612"
---
# <a name="net-core-run-time-configuration-settings"></a>.NET Core ランタイム構成設定

.NET Core では、実行時に .NET Core アプリケーションの動作を構成するために、構成ファイルと環境変数の使用がサポートされています。 ランタイムの構成は、次の場合に適しています。

- アプリケーションのソースコードを所有または制御していないため、プログラムで構成することができない。

- アプリケーションの複数のインスタンスが 1 つのシステムで同時に実行され、最適なパフォーマンスを実現するために各インスタンスを構成する必要がある。

> [!NOTE]
> このドキュメントは現在作成中です。 ここに記載されている情報が不完全であるか、不正確であることがわかった場合は、[イシューを作成](https://github.com/dotnet/docs/issues)してお知らせになるか、[pull request を送信](https://github.com/dotnet/docs/pulls)してイシューに対処してください。 dotnet/docs リポジトリに対する pull request の送信については、[共同作成者のガイド](https://github.com/dotnet/docs/blob/master/CONTRIBUTING.md)のページを参照してください。

.NET Core には、実行時にアプリケーションを構成する次のメカニズムがあります。

- [runtimeconfig.json ファイル](#runtimeconfigjson)

- [MSBuild プロパティ](#msbuild-properties)

- [環境変数](#environment-variables)

一部の構成値は、<xref:System.AppContext.SetSwitch%2A?displayProperty=nameWithType> メソッドを呼び出すことによって、プログラムで設定することもできます。

ドキュメントのこのセクションの記事は、[デバッグ](debugging-profiling.md)や[ガベージ コレクション](garbage-collector.md)などのカテゴリに分類されています。 必要に応じて、*runtimeconfig.json* ファイル、MSBuild のプロパティ、環境変数用の構成オプションを示しています。また相互参照用に .NET Framework プロジェクトの *app.config* ファイルも示しています。

## <a name="runtimeconfigjson"></a>runtimeconfig.json

プロジェクトが[ビルドされた](../tools/dotnet-build.md)場合、出力ディレクトリに *[appname].runtimeconfig.json* ファイルが生成されます。 *runtimeconfig.template.json* ファイルがプロジェクト ファイルと同じフォルダーにある場合、それに設定されているすべてのオプションは *[appname].runtimeconfig.json* ファイルにマージされます。 そのアプリをご自分でビルドする場合は、構成オプションはすべて *runtimeconfig.template.json* ファイルに追加してください。 そのアプリを実行しているだけの場合は、 *[appname].runtimeconfig.json* ファイルに直接挿入します。

> [!NOTE]
> この *[appname].runtimeconfig.json* ファイルは、以降のビルドで上書きされます。

**runtimeconfig.json** ファイルの *configProperties* セクションに、ランタイム構成オプションを指定します。 このセクションには次の形式が含まれます。

```json
"configProperties": {
  "config-property-name1": "config-value1",
  "config-property-name2": "config-value2"
}
```

### <a name="example-appnameruntimeconfigjson-file"></a>[appname].runtimeconfig.json ファイルの例

このオプションを出力 JSON ファイルに入力する場合、`runtimeOptions` プロパティの下に入れます。

```json
{
  "runtimeOptions": {
    "tfm": "netcoreapp3.1",
    "framework": {
      "name": "Microsoft.NETCore.App",
      "version": "3.1.0"
    },
    "configProperties": {
      "System.GC.Concurrent": false,
      "System.Threading.ThreadPool.MinThreads": 4,
      "System.Threading.ThreadPool.MaxThreads": 25
    }
  }
}
```

### <a name="example-runtimeconfigtemplatejson-file"></a>runtimeconfig.template.json ファイルの例

このオプションをテンプレート JSON ファイルに入力する場合、`runtimeOptions` プロパティを省略します。

```json
{
  "configProperties": {
    "System.GC.Concurrent": false,
    "System.Threading.ThreadPool.MinThreads": "4",
    "System.Threading.ThreadPool.MaxThreads": "25"
  }
}
```

## <a name="msbuild-properties"></a>MSBuild プロパティ

一部の実行構成オプションは、 *.csproj* の MSBuild のプロパティ、または SDK 形式の .NET Core プロジェクトの *.vbproj* ファイルを使用して設定できます。 MSBuild のプロパティは、*runtimeconfig.template.json* ファイルで設定されているオプションよりも優先されます。 また、 *[appname].runtimeconfig.json* ファイルに設定されているすべてのオプションもビルド時に上書きします。

次に、ランタイム動作を構成する MSBuild のプロパティがある SDK 形式のプロジェクト ファイルの例を示します。

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp3.1</TargetFramework>
  </PropertyGroup>

  <PropertyGroup>
    <ConcurrentGarbageCollection>false</ConcurrentGarbageCollection>
    <ThreadPoolMinThreads>4</ThreadPoolMinThreads>
    <ThreadPoolMaxThreads>25</ThreadPoolMaxThreads>
  </PropertyGroup>

</Project>
```

実行時の動作を構成する MSBuild プロパティについては、[ガベージ コレクション](garbage-collector.md)に関するページなど、各領域の個々の記事に記載されています。

## <a name="environment-variables"></a>環境変数

環境変数を使用すると、ランタイムの構成情報を指定できます。 環境変数として指定された構成ノブには、一般に **COMPlus_** のプレフィックスが付いています。

環境変数は、Windows のコントロール パネル、コマンド ライン、または Windows と Unix ベースのシステムの両方で <xref:System.Environment.SetEnvironmentVariable(System.String,System.String)?displayProperty=nameWithType> メソッドを呼び出すことによって、プログラムで定義できます。

次の例は、コマンド ラインで環境変数を設定する方法を示しています。

```shell
# Windows
set COMPlus_GCRetainVM=1

# Powershell
$env:COMPlus_GCRetainVM="1"

# Unix
export COMPlus_GCRetainVM=1
```
