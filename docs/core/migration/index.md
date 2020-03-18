---
title: project.json からの .NET Core の移行
description: project.json を使って以前の .NET Core プロジェクトを移行する方法について説明します
ms.date: 07/19/2017
ms.openlocfilehash: 8a9dc05c82fd5476a70ee36a294a287abbfb68c4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "77450688"
---
# <a name="migrating-net-core-projects-from-projectjson"></a>project.json からの .NET Core プロジェクトの移行

このドキュメントでは、.NET Core プロジェクトの次の 3 つの移行シナリオについて説明します。

1. [有効な最新スキーマの *project.json* から *csproj* への移行](#migration-from-projectjson-to-csproj)
2. [DNX から csproj への移行](#migration-from-dnx-to-csproj)
3. [RC3 と以前の .NET Core csproj プロジェクトから最終形式への移行](#migration-from-earlier-net-core-csproj-formats-to-rtm-csproj)

このドキュメントは、project.json を使っている、以前の .NET Core プロジェクトのみに適用されます。 .NET Framework から .NET Core への移行には適用されません。

## <a name="migration-from-projectjson-to-csproj"></a>project.json から csproj への移行

*project.json* から *.csproj* への移行は、次のいずれかの方法で実行できます。

- [Visual Studio](#visual-studio)
- [dotnet 移行コマンドライン ツール](#dotnet-migrate)

いずれの方法も、同じ基本エンジンを使用してプロジェクトを移行するので、両方の結果は同じです。 ほとんどの場合、2 つの方法のいずれかを使用して *project.json* を *csproj* に移行するだけで完了します。プロジェクト ファイルをさらに手動で編集する必要はありません。 結果の *.csproj* ファイルには、格納しているディレクトリ名と同じ名前が付けられます。

### <a name="visual-studio"></a>Visual Studio

Visual Studio 2017 または Visual Studio 2019 バージョン 16.2 以前で *.xproj* ファイル、または *.xproj* ファイルを参照するソリューション ファイルを開くと、 **[一方向のアップグレード]** ダイアログが表示されます。 このダイアログには、移行されるプロジェクトが表示されます。 ソリューション ファイルを開くと、ソリューション ファイルに指定されているすべてのプロジェクトが表示されます。 移行されるプロジェクトの一覧を確認し、 **[OK]** を選択します。

![移行されるプロジェクトの一覧が表示された [一方向のアップグレード] ダイアログ](media/one-way-upgrade.jpg)

選択したプロジェクトは、Visual Studio によって自動的に移行されます。 すべてのプロジェクトを選択していない状態でソリューションを移行すると、同じダイアログが開き、そのソリューションの残りのプロジェクトをアップグレードすることを確認するメッセージが表示されます。 プロジェクトが移行されたら、**ソリューション エクスプローラー** ウィンドウでプロジェクトを右クリックして、 **[編集 \<プロジェクト名.csproj]** を選択し、そのコンテンツを表示して変更することができます。

移行されたファイル (*project.json*、*global.json*、 *.xproj*、およびソリューション ファイル) は *Backup* フォルダーに移動されます。 移行されるソリューション ファイルは Visual Studio 2017 または Visual Studio 2019 にアップグレードされ、Visual Studio 2015 以前のバージョンではそのソリューション ファイルを開くことができなくなります。 移行レポートを含む *UpgradeLog.htm* というファイルも保存され、自動的に開かれます。

> [!IMPORTANT]
> Visual Studio 2019 バージョン 16.3 以降では、 *.xproj* ファイルの読み込みまたは移行はできません。 また、Visual Studio 2015 では、 *.xproj* ファイルを移行する機能は提供されていません。 これらの Visual Studio バージョンのいずれかを使用している場合は、適切なバージョンの Visual Studio をインストールするか、次に説明するコマンド ライン移行ツールを使用します。

### <a name="dotnet-migrate"></a>dotnet migrate

コマンドラインのシナリオでは、[`dotnet migrate`](../tools/dotnet-migrate.md) コマンドを使用できます。 検出されたものに応じて、プロジェクト、ソリューション、または一連のフォルダーの順に移行されます。 プロジェクトを移行すると、プロジェクトとそのすべての依存ファイルが移行されます。

移行されたファイル (*project.json*、*global.json*、および *.xproj*) は *Backup* フォルダーに移動されます。

> [!NOTE]
> Visual Studio Code を使用している場合、`dotnet migrate` コマンドを実行しても、*tasks.json* などの Visual Studio Code 固有のファイルは変更されません。 これらのファイルは手動で変更する必要があります。
> これは、Visual Studio ではないエディターまたは統合開発環境 (IDE) を使用している場合にも該当します。

[project.json](../tools/project-json-to-csproj.md) および *.csproj* 形式の比較については、「*project.json プロパティと csproj プロパティの間のマッピング*」を参照してください。

次のエラーが発生する場合:

> No executable found matching command dotnet-migrate (コマンド dotnet-migrate と一致する実行可能ファイルが見つかりません)

`dotnet --version` を実行して使用しているバージョンを確認します。 [`dotnet migrate`](../tools/dotnet-migrate.md) は、.NET Core SDK 1.0.0 で導入され、バージョン 3.0.100 で削除されました。
現在のディレクトリまたは親ディレクトリに *global.json* ファイルがあり、それに指定されている `sdk` バージョンがこの範囲外である場合に、このエラーが発生します。

## <a name="migration-from-dnx-to-csproj"></a>DNX から csproj への移行

.NET Core 開発にまだ DNX を使用している場合、移行プロセスは次の 2 段階で実行する必要があります。

1. [既存の DNX 移行ガイダンス](from-dnx.md)を使用して DNX から project-json 対応の CLI に移行します。
2. 前のセクションの手順に従って、*project.json* から *.csproj* に移行します。

> [!NOTE]
> Preview 1 リリースの .NET Core CLI で、DNX は公式に非推奨になりました。

## <a name="migration-from-earlier-net-core-csproj-formats-to-rtm-csproj"></a>以前の .NET Core csproj 形式から RTM csproj への移行

.NET Core csproj 形式は、ツールの新しいプレリリース バージョンごとに変化し、進化しています。 以前のバージョンの csproj から最新バージョンにプロジェクト ファイルを移行するツールはないため、プロジェクト ファイルを手動で編集する必要があります。 実際の手順は、移行するプロジェクト ファイルのバージョンによって異なります。 バージョン間で加えられた変更内容に基づいて、考慮する必要があるガイダンスの一部を次に示します。

- `<Project>` 要素からツールのバージョン プロパティを削除します (存在する場合)。
- `xmlns` 要素から XML 名前空間 (`<Project>`) を削除します。
- 存在しない場合は、`Sdk` 要素に `<Project>` 属性を追加し、`Microsoft.NET.Sdk` または `Microsoft.NET.Sdk.Web` に設定します。 この属性は、プロジェクトが SDK を使用することを指定するために使用します。 `Microsoft.NET.Sdk.Web` は Web アプリに使用されます。
- プロジェクトの一番上と一番下から `<Import Project="$(MSBuildExtensionsPath)\$(MSBuildToolsVersion)\Microsoft.Common.props" />` および `<Import Project="$(MSBuildToolsPath)\Microsoft.CSharp.targets" />` ステートメントを削除します。 これらの import ステートメントは SDK で暗黙的に指定されているので、プロジェクトに含める必要はありません。
- プロジェクト内に `Microsoft.NETCore.App` または `NETStandard.Library` `<PackageReference>` アイテムがある場合は、削除することをお勧めします。 これらのパッケージ参照は、[SDK で暗黙的に指定されています](https://aka.ms/sdkimplicitrefs)。
- `Microsoft.NET.Sdk` `<PackageReference>` 要素がある場合は削除します。 SDK の参照は、`Sdk` 要素の `<Project>` 属性によって行われます。
- [SDK で暗黙的に指定](https://en.wikipedia.org/wiki/Glob_(programming))されている [glob](../project-sdk/overview.md#default-compilation-includes) を削除します。 プロジェクトにこれらの glob を残すと、コンパイル アイテムが重複するため、ビルド時にエラーが発生します。

これらの手順を実行すると、RTM .NET Core csproj 形式と完全に互換性のあるプロジェクトになります。

古い csproj 形式から新しい形式に移行する前と後の例については、.NET ブログの記事「[Updating Visual Studio 2017 RC – .NET Core Tooling improvements](https://devblogs.microsoft.com/dotnet/updating-visual-studio-2017-rc-net-core-tooling-improvements/)」 (Visual Studio 2017 RC の更新 - .NET Core ツールの改善) を参照してください。

## <a name="see-also"></a>参照

- [Visual Studio プロジェクトのポート、移行、アップグレード](/visualstudio/porting/port-migrate-and-upgrade-visual-studio-projects)
