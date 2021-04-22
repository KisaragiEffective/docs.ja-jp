---
title: C++/CLI プロジェクトの .NET Core への移行
description: C++/CLI プロジェクトの .NET Core への移植について説明します。
author: mjrousos
ms.date: 01/10/2020
ms.openlocfilehash: 60c4cc2040b5cfb7aadc2147832d3f0a763085dc
ms.sourcegitcommit: 5ddbd1f65d0369b4cc8c8ff91c72f1b524c90221
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/15/2021
ms.locfileid: "107514397"
---
# <a name="how-to-port-a-ccli-project-to-net-core"></a>C++/CLI プロジェクトを .NET Core に移植する方法

.NET Core 3.1 以降および Visual Studio 2019 バージョン 16.4 以降では、[C++/CLI プロジェクト](/cpp/dotnet/dotnet-programming-with-cpp-cli-visual-cpp) で .NET Core をターゲットにすることができます。 このサポートにより、Windows デスクトップ アプリケーションを、C++/CLI 相互運用層を使用して .NET Core に移植できるようになります。 この記事では、C++/CLI プロジェクトを .NET Framework から .NET Core 3.1 に移植する方法について説明します。

## <a name="ccli-net-core-limitations"></a>C++/CLI .NET Core の制限事項

他の言語と比較して、C++/CLI プロジェクトを .NET Core に移植する場合、いくつかの重要な制限があります。

* .NET Core の C++/CLI サポートは Windows のみです。
* C++/CLI プロジェクトで .NET Standard をターゲットにすることはできません。できるのは .NET Core (または .NET Framework) だけです。
* C++/CLI プロジェクトでは、SDK スタイルの新しいプロジェクト ファイル形式はサポートされていません。 代わりに、C++/CLI プロジェクトでは、.NET Core をターゲットにしている場合でも、既存の vcxproj ファイル形式が使用されます。
* C++/CLI プロジェクトでは、複数の .NET プラットフォームをマルチターゲットに設定することはできません。 .NET Framework と .NET Core の両方に対して C++/CLI プロジェクトをビルドする必要がある場合は、個別のプロジェクト ファイルを使用します。
* .NET Core では、`-clr:pure` または `-clr:safe` のコンパイルはサポートされておらず、新しい `-clr:netcore` オプション (.NET Framework の `-clr` に相当) のみがサポートされています。

## <a name="port-a-ccli-project"></a>C++/CLI プロジェクトを移植する

C++/CLI プロジェクトを .NET Core に移植するには、次のように vcxproj ファイルに変更を加えます。 C++/CLI プロジェクトでは SDK スタイルのプロジェクト ファイルが使用されていないため、これらの移行手順は、他のプロジェクト タイプに必要な手順とは異なります。

1. `<CLRSupport>true</CLRSupport>` プロパティを `<CLRSupport>NetCore</CLRSupport>` に置き換えます。 このプロパティは、多くの場合、構成固有のプロパティ グループに含まれているため、場合によっては、複数箇所でこれを置き換える必要があります。
2. `<TargetFrameworkVersion>` プロパティを `<TargetFramework>netcoreapp3.1</TargetFramework>` に置き換えます。
3. .NET Framework の参照 (`<Reference Include="System" />` など) をすべて削除します。 `<CLRSupport>NetCore</CLRSupport>` を使用すると、.NET Core SDK アセンブリが自動的に参照されます。
4. 必要に応じて、cpp ファイルの API 使用状況を更新して、.NET Core で使用できない API を削除します。 C++/CLI プロジェクトはかなり薄い相互運用層になる傾向があるため、多くの変更が必要になることはほとんどありません。 [.NET Portability Analyzer](../../standard/analyzers/portability-analyzer.md) を使用すると、純粋なマネージド バイナリと同じように、C++/CLI バイナリで使用されるサポートされていない .NET API を識別できます。 コードの移植性の判断と、.NET Core API で動作するプロジェクトの更新のガイドラインについては、[ライブラリの移植に関するガイダンス](./libraries.md#determine-portability)を参照してください。

### <a name="wpf-and-windows-forms-usage"></a>WPF と Windows フォームの使用方法

.NET Core C++/CLI プロジェクトでは、Windows フォーム API と WPF API を使用できます。 これらの Windows デスクトップ API を使用するには、明示的なフレームワーク参照を UI ライブラリに追加する必要があります。 Windows デスクトップ API を使用する SDK スタイルのプロジェクトは、`Microsoft.NET.Sdk.WindowsDesktop` SDK を使用して、必要なフレームワーク ライブラリを自動的に参照します。 C++/CLI プロジェクトでは SDK スタイルのプロジェクト形式を使用していないため、.NET Core をターゲットにするときに、明示的なフレームワーク参照を追加する必要があります。

Windows フォーム API を使用するには、次の参照を vcxproj ファイルに追加します。

```xml
<!-- Reference all of Windows Forms -->
<FrameworkReference Include="Microsoft.WindowsDesktop.App.WindowsForms" />
```

WPF API を使用するには、次の参照を vcxproj ファイルに追加します。

```xml
<!-- Reference all of WPF -->
<FrameworkReference Include="Microsoft.WindowsDesktop.App.WPF" />
```

Windows フォーム API と WPF API の両方を使用するには、次の参照を vcxproj ファイルに追加します。

```xml
<!-- Reference the entirety of the Windows desktop framework:
     Windows Forms, WPF, and the types that provide integration between them -->
<FrameworkReference Include="Microsoft.WindowsDesktop.App" />
```

現時点では、Visual Studio の参照マネージャーを使用してこれらの参照を追加することはできません。 代わりに、プロジェクト ファイルを手動で更新します。 この更新は、Visual Studio でプロジェクトをアンロードしてからプロジェクト ファイルを編集することによって行うことができます。 VS Code などの別のエディターを使用することもできます。

## <a name="build-without-msbuild"></a>MSBuild なしでビルドする

MSBuild を使用せずに C++/CLI プロジェクトをビルドすることもできます。 次の手順に従って、*cl.exe* と *link.exe* を使用して、.NET Core 向けの C++/CLI プロジェクトを直接ビルドします。

1. コンパイル時に、*cl.exe* に `-clr:netcore` を渡します。
2. 必要な .NET Core 参照アセンブリを参照します。
3. リンクするときに、.NET Core アプリのホスト ディレクトリを `LibPath` として指定します (*ijwhost.lib* が検出されるようにします)。
4. *ijwhost.dll* を (.NET Core アプリのホスト ディレクトリから) プロジェクトの出力ディレクトリにコピーします。
5. マネージド コードを実行するアプリケーションの最初のコンポーネント用に [runtimeconfig. json](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md) ファイルが存在することを確認します。 アプリケーションにマネージド エントリ ポイントがある場合は、`runtime.config` ファイルが自動的に作成されてコピーされます。 ただし、アプリケーションにネイティブ エントリ ポイントがある場合は、NET Core ランタイムを使用するために最初の C++/CLI ライブラリ用に `runtimeconfig.json` ファイルを作成する必要があります。

## <a name="known-issues"></a>既知の問題

.NET Core をターゲットとする C++/CLI プロジェクトを使用する場合は、注意すべき既知の問題がいくつかあります。

* .NET Core C++/CLI プロジェクトの WPF フレームワーク参照では、現在、シンボルをインポートできないという無関係な警告がいくつか発生しています。 これらの警告は無視してかまいません。まもなく修正されるはずです。
* アプリケーションにネイティブ エントリ ポイントがある場合、最初にマネージド コードを実行する C++/CLI ライブラリには [runtimeconfig.json](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md) ファイルが必要です。 この構成ファイルは、.NET Core ランタイムの起動時に使用されます。 C++/CLI プロジェクトでは、ビルド時に `runtimeconfig.json` ファイルが自動的に作成されないため、このファイルを手動で生成する必要があります。 C++/CLI ライブラリがマネージド エントリ ポイントから呼び出された場合、その C++/CLI ライブラリには `runtimeconfig.json` ファイルは必要ありません (ランタイムの起動時に使用されるものがエントリ ポイント アセンブリに含まれているため)。 簡単なサンプルの `runtimeconfig.json` ファイルを次に示します。 詳しくは、[GitHub で仕様](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md)を参照してください。

    ```json
    {
          "runtimeOptions": {
             "tfm": "netcoreapp3.1",
             "framework": {
                "name": "Microsoft.NETCore.App",
                "version": "3.1.0"
             }
          }
    }
    ```

* Windows 7 では、エントリ アプリケーションがネイティブの場合に .NET Core C++/CLI アセンブリを読み込むと、動作が失敗することがあります。 動作が失敗するのは、Windows 7 のローダーで C++/CLI アセンブリの `mscoree.dll` 以外のエントリ ポイントが考慮されていないためです。 推奨される一連の措置は、エントリ アプリケーションをマネージ コードに変換することです。 スレッド ローカル ストレージ (TLS) が関係するシナリオは、Windows 7 ではすべての場合に明示的にサポート対象外となります。
