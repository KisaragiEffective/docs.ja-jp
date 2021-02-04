---
title: .NET の配布パッケージ
description: .NET を配布用にパッケージ化、名前付け、およびバージョン管理する方法について学習します。
author: tmds
ms.date: 10/09/2019
ms.openlocfilehash: 93d040064eb739b3bd045fdb16b356732353ddc8
ms.sourcegitcommit: 68c9d9d9a97aab3b59d388914004b5474cf1dbd7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2021
ms.locfileid: "99216305"
---
# <a name="net-distribution-packaging"></a>.NET の配布パッケージ

.NET 5 (および .NET Core) 以降のバージョンはますます多くのプラットフォームで利用できるようになっているため、それを使用するアプリやライブラリをパッケージ化、名前付け、およびバージョン管理する方法を知っていると便利です。 そうすることで、パッケージの管理者は、ユーザーの .NET の実行環境に左右されることなく一貫した体験が保証されるようにサポートできます。 この記事は以下のユーザーに役立ちます。

- ソースから .NET をビルドしようとしている。
- 結果として生じるレイアウトまたは生成されるパッケージに影響する可能性がある .NET CLI に変更を加えたい。

## <a name="disk-layout"></a>ディスク レイアウト

インストールした .NET は複数のコンポーネントで構成されています。これらはファイル システムで次のようにレイアウトされています。

```
{dotnet_root}                                     (*)
├── dotnet                       (1)
├── LICENSE.txt                  (8)
├── ThirdPartyNotices.txt        (8)
├── host                                          (*)
│   └── fxr                                       (*)
│       └── <fxr version>        (2)
├── sdk                                           (*)
│   ├── <sdk version>            (3)
│   └── NuGetFallbackFolder      (4)              (*)
├── packs                                         (*)
│   ├── Microsoft.AspNetCore.App.Ref              (*)
│   │   └── <aspnetcore ref version>     (11)
│   ├── Microsoft.NETCore.App.Ref                 (*)
│   │   └── <netcore ref version>        (12)
│   ├── Microsoft.NETCore.App.Host.<rid>          (*)
│   │   └── <apphost version>            (13)
│   ├── Microsoft.WindowsDesktop.App.Ref          (*)
│   │   └── <desktop ref version>        (14)
│   └── NETStandard.Library.Ref                   (*)
│       └── <netstandard version>        (15)
├── shared                                        (*)
│   ├── Microsoft.NETCore.App                     (*)
│   │   └── <runtime version>     (5)
│   ├── Microsoft.AspNetCore.App                  (*)
│   │   └── <aspnetcore version>  (6)
│   ├── Microsoft.AspNetCore.All                  (*)
│   │   └── <aspnetcore version>  (6)
│   └── Microsoft.WindowsDesktop.App              (*)
│       └── <desktop app version> (7)
└── templates                                     (*)
│   └── <templates version>      (17)
/
├── etc/dotnet
│       └── install_location     (16)
├── usr/share/man/man1
│       └── dotnet.1.gz          (9)
└── usr/bin
        └── dotnet               (10)
```

- (1) **dotnet** ホスト (別名 "muxer (マルチプレクサー)") には 2 つの異なるロールがあります。アプリケーションを起動するランタイムのアクティブ化と、コマンドをディスパッチする SDK のアクティブ化です。 ホストはネイティブの実行可能ファイルです (`dotnet.exe`)。

ホストは 1 つですが、他のコンポーネントのほとんどはバージョン管理されたディレクトリに入っています (2、3、5、6)。 つまり、複数のバージョンが並列インストールされるので、それらをシステム上に置くことができます。

- (2) **host/fxr/\<fxr version>** には、ホストが使用するフレームワーク解決ロジックが含まれます。 ホストでは、インストールされている最新の hostfxr が使用されます。 hostfxr は、.NET アプリケーションの実行時に適切なランタイムを選択する役割があります。 たとえば、.NET Core 2.0.0 用としてビルドされたアプリケーションは、2.0.5 が利用できればそれを利用します。 同様に、hostfxr は開発中、適切な SDK を選択します。

- (3) **sdk/\<sdk version>** SDK ("ツール" ともいう) は、.NET のライブラリやアプリケーションを記述およびビルドするために使用されるマネージド ツールのセットです。 SDK には、.NET CLI、マネージ言語コンパイラ、MSBuild、関連するビルド タスクとターゲット、NuGet、新しいプロジェクト テンプレートなどが含まれています。

- (4) **sdk/NuGetFallbackFolder** には、`dotnet restore` や `dotnet build` の実行時など、復元操作中に SDK によって使用される NuGet パッケージのキャッシュが含まれています。 このフォルダーは、.NET Core 3.0 より前でのみ使用されます。 `nuget.org` からの構築済みのバイナリ アセットを含むため、ソースから作成することはできません。

**共有** フォルダーには、フレームワークが含まれています。 共有フレームワークは、さまざまなアプリケーションで利用できるように、中央の場所で一連のライブラリを提供します。

- (5) **shared/Microsoft.NETCore.App/\<runtime version>** このフレームワークには、.NET のランタイムと補助マネージド ライブラリが含まれています。

- (6) **shared/Microsoft.AspNetCore.{App,All}/\<aspnetcore version>** には、ASP.NET Core ライブラリが含まれます。 `Microsoft.AspNetCore.App` の下にあるライブラリは、.NET プロジェクトの一部として開発され、サポートされています。 `Microsoft.AspNetCore.All` の下にあるライブラリは、サードパーティ製ライブラリも含まれるスーパーセットです。

- (7) **shared/Microsoft.Desktop.App/\<desktop app version>** には、Windows デスクトップ ライブラリが含まれます。 これは、Windows 以外のプラットフォームには含まれていません。

- (8) **LICENSE.txt,ThirdPartyNotices.txt** は、それぞれ .NET ライセンスと、.NET で利用されるサードパーティ ライブラリのライセンスです。

- (9,10) **dotnet.1.gz、dotnet** `dotnet.1.gz` は dotnet のマニュアル ページです。 `dotnet` は dotnet host(1) のシンボリック リンクです。 これらのファイルは、システム統合のために、よく知られている場所にインストールされます。

- (11,12) **Microsoft.NETCore.App.Ref,Microsoft.AspNetCore.App.Ref** には、.NET と ASP.NET Core のそれぞれの `x.y` バージョンの API が記述されています。 これらのパックは、それのターゲット バージョンのコンパイル時に使用されます。

- (13) **Microsoft.NETCore.App.Host.\<rid>** にはプラットフォームのネイティブ バイナリが含まれています`rid`。 このバイナリは、.NET アプリケーションをそのプラットフォームのネイティブ バイナリにコンパイルするときのテンプレートです。

- (14) **Microsoft.WindowsDesktop.App.Ref** には、Windows Desktop アプリケーションの `x.y` バージョンの API が記述されています。 これらのファイルは、そのターゲットに対してコンパイルする場合に使用されます。 これは、Windows 以外のプラットフォームにはありません。

- (15) **NETStandard.Library.Ref** には、netstandard `x.y` の API が記述されています。 これらのファイルは、そのターゲットに対してコンパイルする場合に使用されます。

- (16) **/etc/dotnet/install_location** は、`{dotnet_root}` の完全なパスを含むファイルです。 パスの末尾は改行文字である場合があります。 ルートが `/usr/share/dotnet` の場合は、このファイルを追加する必要はありません。

- (17) **templates** には、SDK で使用されるテンプレートが含まれます。 たとえば、`dotnet new` はここからプロジェクト テンプレートを検索します。

`(*)` でマークされたフォルダーは、複数のパッケージによって使用されます。 一部のパッケージ形式 (たとえば、`rpm`) では、このようなフォルダーを特別に処理する必要があります。 パッケージのメンテナンス担当者は、このことに対処する必要があります。

## <a name="recommended-packages"></a>推奨パッケージ

.NET バージョン管理は、ランタイム コンポーネント `[major].[minor]` バージョン番号に基づきます。
SDK バージョンは同じ `[major].[minor]` を利用し、SDK の機能とパッチ意味論を結合する非依存の `[patch]` が与えられます。
次に例を示します。SDK バージョン 2.2.302 は、2.2 ランタイム対応の SDK の第 3 機能リリースの第 2 パッチ リリースです。 バージョン管理のしくみの詳細については、[.NET のバージョン管理の概要](./versions/index.md)に関するページを参照してください。

パッケージには、その名前にバージョン番号の一部が含まれているものもあります。 それによって、特定のバージョンをインストールすることができます。
バージョンの残りの部分はバージョン名には含まれていません。 それによって、OS パッケージ マネージャーはパッケージを更新できます (セキュリティ修正の自動インストールなど)。 サポートされるパッケージ マネージャーは Linux 固有です。

推奨するパッケージを、以下に示します。

- `dotnet-sdk-[major].[minor]`: 特定のランタイム用に最新の sdk をインストールします
  - **バージョン:** \<sdk version>
  - **例:** dotnet-sdk-2.1
  - **内容:** (3),(4)
  - **依存関係:** `dotnet-runtime-[major].[minor]`、`aspnetcore-runtime-[major].[minor]`、`dotnet-targeting-pack-[major].[minor]`、`aspnetcore-targeting-pack-[major].[minor]`、`netstandard-targeting-pack-[netstandard_major].[netstandard_minor]`、`dotnet-apphost-pack-[major].[minor]`、`dotnet-templates-[major].[minor]`

- `aspnetcore-runtime-[major].[minor]`: 特定の ASP.NET Core ランタイムをインストールします
  - **バージョン:** \<aspnetcore runtime version>
  - **例:** aspnetcore-runtime-2.1
  - **内容:** (6)
  - **依存関係:** `dotnet-runtime-[major].[minor]`

- `dotnet-runtime-deps-[major].[minor]` _(省略可能)_ : 自己完結型アプリケーションを実行する依存関係をインストールします
  - **バージョン:** \<runtime version>
  - **例:** dotnet-runtime-deps-2.1
  - **依存関係:** _ディストリビューション固有の依存関係_

- `dotnet-runtime-[major].[minor]`: 特定のランタイムをインストールします
  - **バージョン:** \<runtime version>
  - **例:** dotnet-runtime-2.1
  - **内容:** (5)
  - **依存関係:** `dotnet-hostfxr-[major].[minor]`, `dotnet-runtime-deps-[major].[minor]`

- `dotnet-hostfxr-[major].[minor]`: 依存関係
  - **バージョン:** \<runtime version>
  - **例:**  dotnet-hostfxr-3.0
  - **内容:** (2)
  - **依存関係:** `dotnet-host`

- `dotnet-host`: 依存関係
  - **バージョン:** \<runtime version>
  - **例:** dotnet-host
  - **内容:** (1)、(8)、(9)、(10)、(16)

- `dotnet-apphost-pack-[major].[minor]`: 依存関係
  - **バージョン:** \<runtime version>
  - **内容:** (13)

- `dotnet-targeting-pack-[major].[minor]`: 最新ではないランタイムを対象にできます
  - **バージョン:** \<runtime version>
  - **内容:** (12)

- `aspnetcore-targeting-pack-[major].[minor]`: 最新ではないランタイムを対象にできます
  - **バージョン:** \<aspnetcore runtime version>
  - **内容:** (11)

- `netstandard-targeting-pack-[netstandard_major].[netstandard_minor]`: netstandard バージョンを対象にできます
  - **バージョン:** \<sdk version>
  - **内容:** (15)

- `dotnet-templates-[major].[minor]`
  - **バージョン:** \<sdk version>
  - **内容:** (15)

`dotnet-runtime-deps-[major].[minor]` では、_ディストリビューション固有の依存関係_ を理解している必要があります。 ディストリビューションのビルド システムは、これを自動的に得ることができる可能性があるため、このパッケージは省略可能です。これらの場合、これらの依存関係は `dotnet-runtime-[major].[minor]` パッケージに直接追加されます。

パッケージの内容がバージョン付きフォルダーにある場合、`[major].[minor]` のパッケージ名はバージョン付きのフォルダー名と同じになります。 `netstandard-targeting-pack-[netstandard_major].[netstandard_minor]` を除くすべてのパッケージでは、これは .NET バージョンとも同じになります。

パッケージ間の依存関係では、バージョン要件 _以上_ を使用する必要があります。 たとえば、`dotnet-sdk-2.2:2.2.401` には `aspnetcore-runtime-2.2 >= 2.2.6` が必要です。 これにより、ユーザーはルート パッケージ (たとえば、`dnf update dotnet-sdk-2.2`) を使用してインストールをアップグレードできます。

ほとんどのディストリビューションで、ソースからビルドするすべての成果物を必要とします。 これはパッケージにいくつかの影響を与えます。

- `shared/Microsoft.AspNetCore.All` の下にあるサードパーティ製ライブラリは、ソースから簡単にビルドできません。 そのため、そのフォルダーは `aspnetcore-runtime` パッケージから除外されます。

- `NuGetFallbackFolder` は `nuget.org` からバイナリ成果物を利用して入力されます。 空のままにしてください。

複数の `dotnet-sdk` パッケージで `NuGetFallbackFolder` に同じファイルが提供されることがあります。 パッケージ マネージャーの問題を回避するには、これらのファイルを同じにします (チェックサム、変更日など)。

## <a name="building-packages"></a>パッケージをビルドする

[dotnet/source-build](https://github.com/dotnet/source-build) リポジトリでは、.NET SDK のソース ターボールとそのすべてのコンポーネントをビルドする方法が提供されます。 この source-build リポジトリの出力は、この記事の最初のセクションで説明したレイアウトに一致します。
