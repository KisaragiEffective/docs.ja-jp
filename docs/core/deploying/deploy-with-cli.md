---
title: .NET Core CLI を使用してアプリを発行する
description: .NET Core CLI コマンドを使用して .NET Core アプリを発行する方法について説明します。
author: thraka
ms.author: adegeo
ms.date: 12/12/2019
dev_langs:
- csharp
- vb
ms.openlocfilehash: f4c2a4ccf551c53e4aa4e125cb5720d6f1cc9601
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79397993"
---
# <a name="publish-net-core-apps-with-the-net-core-cli"></a>.NET Core CLI を使用して .NET Core アプリを発行する

この記事では、コマンド ラインから .NET Core アプリケーションを公開する方法を示します。 .NET Core では、アプリケーションを公開する方法が 3 つ用意されています。 フレームワークに依存する展開では、ローカル環境にインストールされている .NET Core ランタイムを使用するクロス プラットフォームの .dll ファイルが生成されます。 フレームワークに依存する実行可能ファイルでは、ローカル環境にインストールされている .NET Core ランタイムを使用するプラットフォーム固有の実行可能ファイルが生成されます。 自己完結型の実行可能ファイルでは、プラットフォーム固有の実行可能ファイルが生成されて、.NET Core ランタイムのローカル コピーが組み込まれます。

これらの公開モードの概要については、「[.NET Core アプリケーションの展開](index.md)」をご覧ください。

CLI の使用方法について簡単にわかるヘルプをお探しですか。 次の表では、アプリの公開方法についての例を示します。 ターゲット フレームワークは、`-f <TFM>` パラメーターを使用するか、プロジェクト ファイルを編集して、指定することができます。 詳細については、「[公開の基礎](#publishing-basics)」をご覧ください。

| 公開モード | SDK バージョン | command |
| ------------ | ----------- | ------- |
| フレームワークに依存する展開 | 2.x | `dotnet publish -c Release` |
| フレームワークに依存する実行可能ファイル | 2.2 | `dotnet publish -c Release -r <RID> --self-contained false` |
|                                | 3.0 | `dotnet publish -c Release -r <RID> --self-contained false` |
|                                | 3.0* | `dotnet publish -c Release` |
| 自己完結型の展開      | 2.1 | `dotnet publish -c Release -r <RID> --self-contained true` |
|                                | 2.2 | `dotnet publish -c Release -r <RID> --self-contained true` |
|                                | 3.0 | `dotnet publish -c Release -r <RID> --self-contained true` |

\* SDK バージョン 3.0 を使用する場合、基本的な `dotnet publish` コマンドを実行するときは、フレームワークに依存する実行可能ファイルが既定の公開モードです。 これは、プロジェクトのターゲットが **.NET Core 2.1** または **.NET Core 3.0** である場合にのみ適用されます。

## <a name="publishing-basics"></a>公開の基礎

アプリを公開するときは、プロジェクト ファイルの設定 `<TargetFramework>` で既定のターゲット フレームワークを指定します。 任意の有効な[ターゲット フレームワーク モニカー (TFM)](../../standard/frameworks.md) にターゲット フレームワークを変更できます。 たとえば、プロジェクトで `<TargetFramework>netcoreapp2.2</TargetFramework>` を使用している場合は、.NET Core 2.2 をターゲットとするバイナリが作成されます。 この設定で指定されている TFM が、[`dotnet publish`](../tools/dotnet-publish.md) コマンドで使用される既定のターゲットになります。

複数のフレームワークをターゲットにしたい場合は、セミコロンで区切ることにより設定 `<TargetFrameworks>` で複数の TFM 値を設定できます。 `dotnet publish -f <TFM>` コマンドではフレームワークの 1 つを公開できます。 たとえば、`<TargetFrameworks>netcoreapp2.1;netcoreapp2.2</TargetFrameworks>` と設定して `dotnet publish -f netcoreapp2.1` を実行すると、.NET Core 2.1 をターゲットとするバイナリが作成されます。

他の値を設定しない限り、[`dotnet publish`](../tools/dotnet-publish.md) コマンドの出力ディレクトリは `./bin/<BUILD-CONFIGURATION>/<TFM>/publish/` です。 **パラメーターで変更しない限り、** BUILD-CONFIGURATION**の既定のモードは**Debug`-c` です。 たとえば、`dotnet publish -c Release -f netcoreapp2.1` と指定すると、`myfolder/bin/Release/netcoreapp2.1/publish/` に公開されます。

.NET Core SDK 3.0 以降を使用する場合、.NET Core バージョン 2.1、2.2、3.0、またはそれ以降のバージョンをターゲットとするアプリの既定の公開モードは、フレームワークに依存する実行可能ファイルです。

.NET Core SDK 2.1 を使用する場合、.NET Core バージョン 2.1 および 2.2 をターゲットとするアプリの既定の公開モードは、フレームワークに依存する展開です。

### <a name="native-dependencies"></a>ネイティブの依存関係

アプリにネイティブの依存関係がある場合、別のオペレーティング システムではアプリが実行されない可能性があります。 たとえば、ネイティブの Windows API を使用しているアプリは、macOS または Linux では実行されません。 プラットフォーム固有のコードを提供し、プラットフォームごとに実行可能ファイルをコンパイルする必要があります。

また、参照しているライブラリにネイティブの依存関係がある場合、すべてのプラットフォームではアプリを実行できない可能性があることも考慮してください。 ただし、参照している NuGet パッケージには、必要なネイティブの依存関係を処理するためにプラットフォーム固有のバージョンが含まれている可能性があります。

ネイティブの依存関係があるアプリを配布するときは、`dotnet publish -r <RID>` スイッチを使用して、公開対象のターゲット プラットフォームを指定することが必要な場合があります。 ランタイム識別子の一覧については、[ランタイム識別子 (RID) のカタログ](../rid-catalog.md)に関する記事をご覧ください。

プラットフォーム固有のバイナリの詳細については、「[フレームワークに依存する実行可能ファイル](#framework-dependent-executable)」および「[自己完結型の展開](#self-contained-deployment)」セクションをご覧ください。

## <a name="sample-app"></a>サンプル アプリ

次のアプリを使用して、公開コマンドを確認できます。 ターミナルで次のコマンドを実行すると、アプリが作成されます。

```dotnetcli
mkdir apptest1
cd apptest1
dotnet new console
dotnet add package Figgle
```

コンソール テンプレートによって生成される `Program.cs` または `Program.vb` ファイルを、次のように変更する必要があります。

```csharp
using System;

namespace apptest1
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(Figgle.FiggleFonts.Standard.Render("Hello, World!"));
        }
    }
}
```

```vb
Module Program
    Sub Main(args As String())
        Console.WriteLine(Figgle.FiggleFonts.Standard.Render("Hello, World!"))
    End Sub
End Module
```

アプリを実行すると ([`dotnet run`](../tools/dotnet-run.md))、次の出力が表示されます。

```terminal
  _   _      _ _         __        __         _     _ _
 | | | | ___| | | ___    \ \      / /__  _ __| | __| | |
 | |_| |/ _ \ | |/ _ \    \ \ /\ / / _ \| '__| |/ _` | |
 |  _  |  __/ | | (_) |    \ V  V / (_) | |  | | (_| |_|
 |_| |_|\___|_|_|\___( )    \_/\_/ \___/|_|  |_|\__,_(_)
                     |/
```

## <a name="framework-dependent-deployment"></a>フレームワークに依存する展開

.NET Core SDK 2.x の CLI では、フレームワークに依存する展開 (FDD) が、基本的な `dotnet publish` コマンドの既定のモードです。

FDD としてアプリを公開すると、`<PROJECT-NAME>.dll` フォルダーに`./bin/<BUILD-CONFIGURATION>/<TFM>/publish/` ファイルが作成されます。 アプリを実行するには、出力フォルダーに移動して、`dotnet <PROJECT-NAME>.dll` コマンドを使用します。

アプリは、特定のバージョンの .NET Core をターゲットにするように構成されます。 アプリが実行されるすべてのマシンには、そのターゲットの .NET Core ランタイムが存在する必要があります。 たとえば、アプリのターゲットが .NET Core 2.2 である場合、アプリを実行するコンピューターには、.NET Core 2.2 ランタイムがインストールされている必要があります。 「[公開の基礎](#publishing-basics)」セクションで説明したように、プロジェクト ファイルを編集することで、既定のターゲット フレームワークを変更したり、複数のフレームワークをターゲットにしたりできます。

FDD の公開では、アプリが実行されるシステムで使用できる最新の .NET Core セキュリティ更新プログラムまで自動的にロールフォワードするアプリが作成されます。 コンパイル時のバージョンのバインドの詳細については、「[使用する .NET Core のバージョンを選択する](../versions/selection.md#framework-dependent-apps-roll-forward)」をご覧ください。

## <a name="framework-dependent-executable"></a>フレームワークに依存する実行可能ファイル

.NET Core SDK 3.x の CLI では、フレームワークに依存する実行可能ファイル (FDE) が、基本的な `dotnet publish` コマンドの既定のモードです。 現在のオペレーティング システムをターゲットにする限り、他のパラメーターを指定する必要はありません。

このモードでは、クロスプラットフォーム アプリをホストするために、プラットフォームに固有の実行可能なホストが作成されます。 FDD では `dotnet` コマンドの形式でホストを要求するので、このモードは FDD と似ています。 ホストの実行可能ファイル名はプラットフォームごとに異なり、`<PROJECT-FILE>.exe` のような名前になります。 この実行可能ファイルは、`dotnet <PROJECT-FILE>.dll` を呼び出す (アプリの実行手段として引き続き使用可能) 代わりに、直接実行することができます。

アプリは、特定のバージョンの .NET Core をターゲットにするように構成されます。 アプリが実行されるすべてのマシンには、そのターゲットの .NET Core ランタイムが存在する必要があります。 たとえば、アプリのターゲットが .NET Core 2.2 である場合、アプリを実行するコンピューターには、.NET Core 2.2 ランタイムがインストールされている必要があります。 「[公開の基礎](#publishing-basics)」セクションで説明したように、プロジェクト ファイルを編集することで、既定のターゲット フレームワークを変更したり、複数のフレームワークをターゲットにしたりできます。

FDE の公開では、アプリが実行されるシステムで使用できる最新の .NET Core セキュリティ更新プログラムまで自動的にロールフォワードするアプリが作成されます。 コンパイル時のバージョンのバインドの詳細については、「[使用する .NET Core のバージョンを選択する](../versions/selection.md#framework-dependent-apps-roll-forward)」をご覧ください。

.NET Core 2.2 以前では、`dotnet publish` コマンドで次のスイッチを使用して、FDE を公開する必要があります。

- `-r <RID>` このスイッチでは、識別子 (RID) を使用してターゲット プラットフォームを指定します。 ランタイム識別子の一覧については、[ランタイム識別子 (RID) のカタログ](../rid-catalog.md)に関する記事をご覧ください。

- `--self-contained false` このスイッチでは、識別子 (RID) を使用してターゲット プラットフォームを指定します。

`-r` スイッチを使用すると常に、出力フォルダーのパスが `./bin/<BUILD-CONFIGURATION>/<TFM>/<RID>/publish/` に変わります

[アプリの例](#sample-app)を使用する場合は、`dotnet publish -f netcoreapp2.2 -r win10-x64 --self-contained false` を実行します。 このコマンドでは、実行可能ファイル `./bin/Debug/netcoreapp2.2/win10-x64/publish/apptest1.exe` が作成されます

> [!NOTE]
> **グローバリゼーション インバリアント モード**を有効にすることで、展開の合計サイズを小さくすることができます。 このモードは、全世界を意識するものではなく、[インバリアント カルチャ](xref:System.Globalization.CultureInfo.InvariantCulture)の書式設定規則、大文字/小文字の区別規則、文字列比較、並べ替え順序を使用できるアプリケーションにとって便利です。 **グローバリゼーション インバリアント モード**の詳細と、それを有効にする方法については、「[.NET Core Globalization Invariant Mode](https://github.com/dotnet/runtime/blob/master/docs/design/features/globalization-invariant-mode.md)」(.NET Core のグローバリゼーション インバリアント モード) を参照してください。

## <a name="self-contained-deployment"></a>自己完結型の展開

自己完結型の展開 (SCD) を公開すると、.NET Core SDK によってプラットフォーム固有の実行可能ファイルが作成されます。 SCD の公開には、アプリの実行に必要なすべての .NET Core ファイルが含まれますが、[.NET Core のネイティブの依存関係](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)は含まれません。 これらの依存関係は、アプリを実行する前に、システムに存在している必要があります。

SCD の公開で作成されるアプリでは、使用可能な最新の .NET Core セキュリティ更新プログラムへのロールフォワードは行われません。 コンパイル時のバージョンのバインドの詳細については、「[使用する .NET Core のバージョンを選択する](../versions/selection.md#self-contained-deployments-include-the-selected-runtime)」をご覧ください。

`dotnet publish` コマンドで次のスイッチを使用して、SCD を公開する必要があります。

- `-r <RID>` このスイッチでは、識別子 (RID) を使用してターゲット プラットフォームを指定します。 ランタイム識別子の一覧については、[ランタイム識別子 (RID) のカタログ](../rid-catalog.md)に関する記事をご覧ください。

- `--self-contained true` このスイッチでは、SCD として実行可能ファイルを作成するよう .NET Core SDK に指示されます。

> [!NOTE]
> **グローバリゼーション インバリアント モード**を有効にすることで、展開の合計サイズを小さくすることができます。 このモードは、全世界を意識するものではなく、[インバリアント カルチャ](xref:System.Globalization.CultureInfo.InvariantCulture)の書式設定規則、大文字/小文字の区別規則、文字列比較、並べ替え順序を使用できるアプリケーションにとって便利です。 **グローバリゼーション インバリアント モード**の詳細と、それを有効にする方法については、「[.NET Core Globalization Invariant Mode](https://github.com/dotnet/runtime/blob/master/docs/design/features/globalization-invariant-mode.md)」(.NET Core のグローバリゼーション インバリアント モード) を参照してください。

## <a name="see-also"></a>参照

- [.NET Core アプリケーションの展開の概要](index.md)
- [.NET Core のランタイム識別子 (RID) のカタログ](../rid-catalog.md)
