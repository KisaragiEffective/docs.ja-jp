---
title: .NET Runtime Identifier (RID) カタログ
description: ランタイム識別子 (RID) と .NET での RID の使用方法について学習します。
ms.date: 01/28/2021
ms.topic: reference
ms.openlocfilehash: ea3881332fe407bdc985d22033a88cb669e360e8
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740303"
---
# <a name="net-rid-catalog"></a>.NET RID カタログ

RID は *ランタイム識別子* の略です。 RID の値は、アプリケーションが実行されている対象プラットフォームの識別に使用されます。
これは NuGet パッケージのプラットフォーム固有のアセットを表すために、.NET パッケージによって使用されます。 RID は、たとえば `linux-x64`、`ubuntu.14.04-x64`、`win7-x64`、または `osx.10.12-x64` などの値です。
ネイティブ依存関係のあるパッケージの場合、パッケージを復元できるプラットフォームが RID によって指定されます。

プロジェクト ファイルの `<RuntimeIdentifier>` 要素では、1 つの RID を設定できます。 複数の RID は、プロジェクト ファイルの `<RuntimeIdentifiers>` 要素でセミコロン区切りのリストとして定義できます。 以下の [.NET CLI コマンド](./tools/index.md)の `--runtime` オプションで使用することもできます。

- [dotnet build](./tools/dotnet-build.md)
- [dotnet clean](./tools/dotnet-clean.md)
- [dotnet pack](./tools/dotnet-pack.md)
- [dotnet publish](./tools/dotnet-publish.md)
- [dotnet restore](./tools/dotnet-restore.md)
- [dotnet run](./tools/dotnet-run.md)
- [dotnet store](./tools/dotnet-store.md)

具体的なオペレーティング システムを表す RID は通常、`[os].[version]-[architecture]-[additional qualifiers]` のようなパターンになります。それぞれ、次のような意味があります。

- `[os]` はオペレーティング システムまたはプラットフォーム システムのモニカーです。 たとえば、`ubuntu` のようにします。

- `[version]` は、オペレーティング システムのバージョンをドット区切りの形式 (`.`) で表したバージョン番号です。 たとえば、`15.10` のようにします。

  - バージョンはマーケティング バージョンに **しないでください**。プラットフォームの API アクセス領域が異なる、オペレーティング システムの複数の別々のバージョンを表すことがあるためです。

- `[architecture]` はプロセッサ アーキテクチャです。 たとえば `x86`、`x64`、`arm` または `arm64` などです。

- `[additional qualifiers]` はさまざまなプラットフォームをさらに区別します。 たとえば、`aot` のように指定します。

## <a name="rid-graph"></a>RID グラフ

RID グラフまたはランタイム フォールバック グラフとは、相互に互換性のある RID の一覧です。 RID は [Microsoft.NETCore.Platforms](https://www.nuget.org/packages/Microsoft.NETCore.Platforms/) パッケージで定義されています。 サポートされている RID および RID グラフの一覧は、`dotnet/runtime` リポジトリにある [*runtime.json*](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/src/runtime.json) ファイルで確認できます。 このファイルでは、基本となる RID を除くすべての RID に `"#import"` ステートメントが記述されていることを確認できます。 このステートメントは互換性のある RID を示しています。

NuGet はパッケージを復元する際、指定されたランタイムと正確に一致するものを見つけようとします。
正確に一致するものが見つからない場合、NuGet は、RID グラフに基づいて最も互換性のあるシステムが見つかるまでグラフを遡ります。

次に示す例は、`osx.10.12-x64` の RID として実際に記述されているものです。

```json
"osx.10.12-x64": {
    "#import": [ "osx.10.12", "osx.10.11-x64" ]
}
```

上の RID は `osx.10.12-x64` が `osx.10.11-x64` をインポートすることを指定します。 そのため、NuGet はパッケージを復元する際、`osx.10.12-x64` と正確に一致するものをパッケージから見つけようとします。 特定のランタイムが見つからなかった場合、NuGet は、たとえば `osx.10.11-x64` ランタイムを指定しているパッケージを復元できます。

次の例は、*runtime.json* ファイルにも定義されている少し大きめの RID グラフです。

```
    win7-x64    win7-x86
       |   \   /    |
       |   win7     |
       |     |      |
    win-x64  |  win-x86
          \  |  /
            win
             |
            any
```

すべての RID は最終的にルート `any` RID にマッピングされます。

RID を使用する際に留意しておく必要のある注意事項があります。

- コンポーネント パーツを取得する目的で RID を解析する行為はお止めください。
- RID をプログラムによって作成しないでください。
- プラットフォームであらかじめ定義されている RID を使用します。
- RID は特定の値である必要があるため、実際の値から推測した値にしないでください。

## <a name="using-rids"></a>RID の使用

RID を使用するには、どのような RID があるのか知る必要があります。 プラットフォームには新しい RID が定期的に追加されます。
最新の完全なバージョンについては、`dotnet/runtime` リポジトリの [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/src/runtime.json) ファイルをご覧ください。

ポータブル RID は、RID グラフに追加された値であり、特定のバージョンや OS のディストリビューションに関連付けられていません。 ほとんどのディストリビューション RID はポータブル RID にマップされるため、複数の Linux ディストリビューションを扱うときは特にこれらを選択することをお勧めします。

次の一覧では、各 OS に使用される最も一般的な RID の小さいサブセットを示します。

## <a name="windows-rids"></a>Windows RID

一般的な値のみを示します。 最新の完全なバージョンについては、`dotnet/runtime` リポジトリの [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/src/runtime.json) ファイルをご覧ください。

- 移植性があります。
  - `win-x64`
  - `win-x86`
  - `win-arm`
  - `win-arm64`
- Windows 7 / Windows Server 2008 R2
  - `win7-x64`
  - `win7-x86`
- Windows 8.1 / Windows Server 2012 R2
  - `win81-x64`
  - `win81-x86`
  - `win81-arm`
- Windows 10 / Windows Server 2016
  - `win10-x64`
  - `win10-x86`
  - `win10-arm`
  - `win10-arm64`

詳細については、[.NET の依存関係と要件](./install/windows.md#dependencies)に関するセクションを参照してください。

## <a name="linux-rids"></a>Linux RID

一般的な値のみを示します。 最新の完全なバージョンについては、`dotnet/runtime` リポジトリの [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/src/runtime.json) ファイルをご覧ください。 以下の一覧にないディストリビューションが実行されているデバイスでも、ポータブル RID のいずれかで動作する可能性があります。 たとえば、一覧にない Linux ディストリビューションが実行されている Raspberry Pi デバイスは、`linux-arm` の対象にできます。

- 移植性があります。
  - `linux-x64` (CentOS、Debian、Fedora、Ubuntu、および派生ディストリビューションなどのほとんどのデスクトップ ディストリビューション)
  - `linux-musl-x64` (Alpine Linux など、[musl](https://wiki.musl-libc.org/projects-using-musl.html) を使用している軽量ディストリビューション)
  - `linux-arm` (Raspberry Pi Model 2+ 上の Raspbian など、ARM で実行されている Linux ディストリビューション)
  - `linux-arm64` (Raspberry Pi Model 3+ 上の Ubuntu Server 64 ビットなど、64 ビット ARM で実行されている Linux ディストリビューション)
- Red Hat Enterprise Linux
  - `rhel-x64` (RHEL バージョン 6 より後では、`linux-x64` に置き換えられます)
  - `rhel.6-x64`
- Tizen
  - `tizen`
  - `tizen.4.0.0`
  - `tizen.5.0.0`

詳細については、[.NET の依存関係と要件](./install/linux.md)に関するセクションを参照してください。

## <a name="macos-rids"></a>macOS RID

macOS RID では、以前の "OSX" ブランドが使用されています。 一般的な値のみを示します。 最新の完全なバージョンについては、`dotnet/runtime` リポジトリの [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/src/runtime.json) ファイルをご覧ください。

- 移植性があります。
  - `osx-x64` (最小 OS バージョンは、macOS 10.12 Sierra です)
- macOS 10.10 Yosemite
  - `osx.10.10-x64`
- macOS 10.11 El Capitan
  - `osx.10.11-x64`
- macOS 10.12 Sierra
  - `osx.10.12-x64`
- macOS 10.13 High Sierra
  - `osx.10.13-x64`
- macOS 10.14 Mojave
  - `osx.10.14-x64`
- macOS 10.15 Catalina
  - `osx.10.15-x64`
- macOS 11.01 Big Sur
  - `osx.11.0-x64`
  - `osx.11.0-arm64`

詳細については、[.NET の依存関係と要件](./install/macos.md#dependencies)に関するセクションを参照してください。

## <a name="see-also"></a>関連項目

- [ランタイム ID](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/readme.md)
