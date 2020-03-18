---
title: NuGet および .NET ライブラリ
description: .NET ライブラリ対応の NuGet によるパッケージ化のベスト プラクティスの推奨事項
ms.date: 01/15/2019
ms.openlocfilehash: f1e8d39fe2988f11ce7fd351a4d6bee6d322f2b5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79398533"
---
# <a name="nuget"></a>NuGet

NuGet は .NET エコシステムのパッケージ マネージャーであり、開発者が .NET オープンソース ライブラリを見つけて入手するための主要な方法です。 NuGet パッケージをホストするために Microsoft が提供している無料サービスの [NuGet.org](https://www.nuget.org/) は、パブリック NuGet パッケージのプライマリ ホストですが、[MyGet](https://www.myget.org/) および [Azure Artifacts](https://azure.microsoft.com/services/devops/artifacts/) などのカスタム NuGet サービスに公開できます。

![NuGet](./media/nuget/nuget-logo.png "NuGet")

## <a name="create-a-nuget-package"></a>NuGet パッケージの作成

NuGet パッケージ (`*.nupkg`) は、.NET アセンブリと関連するメタデータを含む zip ファイルです。

NuGet パッケージを作成するには、主な方法が 2 つあります。 より新しいお勧めの方法は、SDK 形式のプロジェクト (コンテンツが `<Project Sdk="Microsoft.NET.Sdk">` から始まるプロジェクト ファイル) からパッケージを作成することです。 アセンブリとターゲットが自動的にパッケージに追加され、パッケージ名やバージョン番号などの残りのメタデータが、MSBuild ファイルに追加されます。 [`dotnet pack`](../../core/tools/dotnet-pack.md) コマンドを使ってコンパイルすると、アセンブリの代わりに `*.nupkg` ファイルが出力されます。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <AssemblyName>Contoso.Api</AssemblyName>
    <PackageVersion>1.1.0</PackageVersion>
    <Authors>John Doe</Authors>
  </PropertyGroup>
</Project>
```

NuGet パッケージを作成する従来からの方法では、`*.nuspec` ファイルと `nuget.exe` コマンドライン ツールを使用します。 nuspec ファイルを使用すると、優れた制御を利用できますが、最終的な NuGet パッケージに含めるアセンブリとターゲットを慎重に指定する必要があります。 間違えやすいうえに、変更を加える際にユーザーは nuspec の更新を忘れやすいです。 nuspec の利点は、まだ SDK 形式のプロジェクト ファイルをサポートしていないフレームワークの NuGet パッケージを作成するために使用できることです。

✔️ SDK 形式のプロジェクト ファイルを使用して NuGet パッケージを作成することを検討してください。

## <a name="package-dependencies"></a>パッケージの依存関係

NuGet パッケージの依存関係については、「[Dependencies](./dependencies.md)」 (依存関係) の記事で説明しています。

## <a name="important-nuget-package-metadata"></a>重要な NuGet パッケージ メタデータ

NuGet パッケージは、多数の[メタデータ プロパティ](/nuget/reference/nuspec)をサポートしています。 次の表に、NuGet.org 上のすべてのパッケージで指定する必要があるコア メタデータを示します。

| MSBuild プロパティの名前              | Nuspec の名前              | [説明]  |
| ---------------------------------- | ------------------------ | ------------ |
| `PackageId`                        | `id`                       | パッケージ ID。 [条件](/nuget/reference/id-prefix-reservation)を満たしている場合、ID のプレフィックスは予約できます。 |
| `PackageVersion`                   | `version`                  | NuGet パッケージ バージョン。 詳細については、「[NuGet package version](./versioning.md#nuget-package-version)」(NuGet package version パッケージ バージョン) をご覧ください。             |
| `Title`                            | `title`                    | わかりやすいパッケージ タイトル。 既定値は `PackageId` です。             |
| `Description`                      | `description`              | UI に表示されるパッケージの長い説明。             |
| `Authors`                          | `authors`                  | パッケージ作成者のコンマで区切りの一覧。nuget.org のプロファイル名と一致します。             |
| `PackageTags`                      | `tags`                     | パッケージを説明するタグとキーワードのスペース区切りの一覧。 タグは、パッケージを検索するときに使用されます。             |
| `PackageIconUrl`                   | `iconUrl`                  | パッケージのアイコンとして使用するイメージの URL。 URL は HTTPS にする必要があり、イメージは 64 x 64 で透明な背景になっている必要があります。             |
| `PackageProjectUrl`                | `projectUrl`               | プロジェクトのホーム ページまたはソース リポジトリの URL。             |
| `PackageLicenseExpression`         | `license`                  | プロジェクト ライセンスの [SPDX 識別子](https://spdx.org/licenses/)。 OSI と FSF によって承認されたライセンスのみが識別子を使用できます。 その他のライセンスでは、`PackageLicenseFile` を使用する必要があります。 [ メタデータの詳細については、`license`こちら](/nuget/reference/nuspec#license)をご覧ください。 |

> [!IMPORTANT]
> ライセンスのないプロジェクトは、[排他的な著作権](https://choosealicense.com/no-permission/)を侵害しているため、他のユーザーが合法的に使用できなくなります。

✔️ NuGet のプレフィックスの予約[条件](/nuget/reference/id-prefix-reservation)を満たしているプレフィックスを持つ NuGet パッケージ名を選択することを検討してください。

✔️ パッケージのアイコンに HTTPS href を使用してください。

> NuGet.org のようなサイトは HTTPS を有効にした状態で実行され、HTTPS ではないイメージを表示すると、コンテンツの混合を示す警告が作成されます。

✔️ 最良な表示結果になるように、64 x 64 で透明な背景を持つパッケージ アイコン イメージを使用してください。

✔️ [ソース リンク](./sourcelink.md)を設定して、お使いのアセンブリと NuGet パッケージにソース管理のメタデータを追加することを検討してください。

> ソース リンクによってメタデータの `RepositoryUrl` と `RepositoryType` が NuGet パッケージに自動的に追加されます。 また、ソース リンクによって、パッケージの作成元のソース コードに関する情報が追加されます。 たとえば、Git リポジトリから作成されたパッケージでは、コミット ハッシュがメタデータとして追加されます。

## <a name="pre-release-packages"></a>プレリリース パッケージ

バージョン サフィックスがある NuGet パッケージは、[プレリリース](/nuget/create-packages/prerelease-packages)と見なされます。 プレリリース パッケージが制限付きユーザーのテスト実行に最適になるように、ユーザーがプレリリース パッケージを選択しない限り、既定では、NuGet パッケージ マネージャーの UI は、安定版リリースを表示します。

```xml
<PackageVersion>1.0.1-beta1</PackageVersion>
```

> [!NOTE]
> 安定版パッケージは、プレリリース パッケージに依存できません。 独自のパッケージをプレリリースにするか、または旧来の安定版バージョンに依存する必要があります。

![NuGet プレリリース パッケージの依存関係](./media/nuget/nuget-prerelease-package.png "NuGet プレリリース パッケージの依存関係")

✔️ テスト、プレビュー、または実験時には、プレリリース パッケージを公開してください。

✔️ 他の安定版パッケージから参照できるように、準備ができ次第、安定版パッケージを公開してください。

## <a name="symbol-packages"></a>シンボル パッケージ

シンボル ファイル (`*.pdb`) は、アセンブリと共に .NET コンパイラによって生成されます。 デバッガ―を使用して実行しながらソース コード全体をステップ実行できるように、シンボル ファイルは、実行場所を元のソース コードにマップします。 NuGet では、.NET アセンブリを含む主要なパッケージと共に、シンボル ファイルを格納している[別個のシンボル パッケージ (`*.snupkg`) の生成](/nuget/create-packages/symbol-packages-snupkg)をサポートしています。 シンボル サーバー上でホストされ、Visual Studio などのツールによってオンデマンドでしかダウンロードできないのが、シンボル パッケージの考え方です。

NuGet.org は独自の[シンボル サーバー リポジトリ](/nuget/create-packages/symbol-packages-snupkg#nugetorg-symbol-server)をホストしています。 開発者は `https://symbols.nuget.org/download/symbols`Visual Studio でシンボル ソース[に ](/visualstudio/debugger/specify-symbol-dot-pdb-and-source-files-in-the-visual-studio-debugger) を追加することで NuGet.org シンボル サーバーに公開されたシンボルを使用できます。

> [!IMPORTANT]
> NuGet.org シンボル サーバーでは、SDK スタイルのプロジェクトで作成された新しい[ポータブル シンボル ファイル](https://github.com/dotnet/core/blob/master/Documentation/diagnostics/portable_pdb.md) (`*.pdb`) のみがサポートされます。
>
> .NET ライブラリのデバッグ時に NuGet.org シンボル サーバーを使用するには、開発者が Visual Studio 2017 バージョン 15.9 以降を持っている必要があります。

シンボル パッケージを作成する代わりに、主要 NuGet パッケージにシンボル ファイルを埋め込むという方法もあります。 主要 NuGet パッケージは大容量になりますが、シンボル ファイルを埋め込む場合、開発者は NuGet.org シンボル サーバーを設定する必要がないことを意味します。 SDK 形式のプロジェクトを使用して NuGet パッケージを構築している場合、`AllowedOutputExtensionsInPackageBuildOutputFolder` プロパティを設定してシンボル ファイルを埋め込むことができます。

```xml
<Project Sdk="Microsoft.NET.Sdk">
 <PropertyGroup>
    <!-- Include symbol files (*.pdb) in the built .nupkg -->
    <AllowedOutputExtensionsInPackageBuildOutputFolder>$(AllowedOutputExtensionsInPackageBuildOutputFolder);.pdb</AllowedOutputExtensionsInPackageBuildOutputFolder>
  </PropertyGroup>
</Project>
```

シンボル ファイルを埋め込むことの短所は、SDK スタイルのプロジェクトでコンパイルした .NET ライブラリの場合、パッケージ サイズが約 30% 増えるということです。 パッケージ サイズが問題であれば、代わりにシンボル パッケージでシンボルを公開してください。

✔️ シンボルをシンボル パッケージ (`*.snupkg`) として NuGet.org に発行することを検討してください。

> シンボル パッケージ (`*.snupkg`) は、メイン パッケージのサイズを肥大化させることなく、また NuGet パッケージをデバッグする予定のない人の復元パフォーマンスに影響を及ぼすことなく、オンデマンドの良好なデバッグ エクスペリエンスを開発者に提供します。
>
> 注意点は、ユーザーが自分の IDE 内で NuGet シンボル サーバーを見つけ、(1 回限りのセットアップとして) 構成し、シンボル ファイルを取得する必要があることです。 Visual Studio 2019 バージョン 16.1 では、既定のシンボル サーバーの一覧に NuGet.org のシンボル サーバーが追加されました。

>[!div class="step-by-step"]
>[前へ](strong-naming.md)
>[次へ](dependencies.md)
