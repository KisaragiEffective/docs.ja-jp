---
title: .NET Core 2.0 の新機能
description: .NET Core の新機能について。
ms.date: 08/13/2017
ms.openlocfilehash: 115b3adc72b6798c6a7bac9cc18044a8822808a6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79397843"
---
# <a name="whats-new-in-net-core-20"></a>.NET Core 2.0 の新機能

.NET core 2.0 には、次のことの拡張機能や新機能が含まれます。

- [ツール](#tooling)
- [言語サポート](#language-support)
- [プラットフォームの機能強化](#platform-improvements)
- [API の変更](#api-changes-and-library-support)
- [Visual Studio の統合](#visual-studio-integration)
- [ドキュメントの改善](#documentation-improvements)

## <a name="tooling"></a>ツール

### <a name="dotnet-restore-runs-implicitly"></a>dotnet restore の暗黙的な実行

以前のバージョンの .NET Core では、[dotnet new](../tools/dotnet-new.md) コマンドで新しいプロジェクトを作成した直後に、またはプロジェクトに新しい依存関係を追加したときに、[dotnet restore](../tools/dotnet-restore.md) コマンドを実行して、依存関係をダウンロードする必要がありました。

[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]

この `dotnet restore` の自動実行は、`--no-restore` スイッチを `new`、`run`、`build`、`publish`、`pack`、`test` コマンドに渡すことで、無効にすることもできます。

### <a name="retargeting-to-net-core-20"></a>.NET Core 2.0 への再ターゲット

.NET Core 2.0 SDK がインストールされていれば、.NET Core 1.x をターゲットしているプロジェクトを .NET Core 2.0 に再ターゲットすることができます。

.NET Core 2.0 に再ターゲットするには、プロジェクト ファイルを編集して `<TargetFramework>` 要素 (または、プロジェクト ファイルに複数のターゲットがある場合は `<TargetFrameworks>` 要素) の値を 1.x から 2.0 に変更します。

```xml
<PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
 </PropertyGroup>
```

同じ方法で、.NET Standard ライブラリを .NET Standard 2.0 に再ターゲットすることもできます。

```xml
<PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
 </PropertyGroup>
```

.NET Core 2.0 へのプロジェクトの移行の詳細については、「[Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0 (ASP.NET Core 1.x から ASP.NET Core 2.0 への移行)](/aspnet/core/migration/1x-to-2x/index)」をご覧ください。

## <a name="language-support"></a>言語サポート

.NET Core 2.0 では、C# と F# に加えて Visual Basic もサポートされています。

### <a name="visual-basic"></a>Visual Basic

.NET Core のバージョン 2.0 では、Visual Basic 2017 がサポートされています。 Visual Basic を使用して、次の種類のプロジェクトを作成できます。

- .NET Core コンソール アプリ
- .NET Core クラス ライブラリ
- .NET Standard クラス ライブラリ
- .NET Core 単体テスト プロジェクト
- .NET Core xUnit テスト プロジェクト

たとえば、Visual Basic で "Hello World" アプリケーションを作成するには、コマンドラインから次の手順を実行します。

1. コンソール ウィンドウを開き、プロジェクトにディレクトリを作成し、そのディレクトリを現在のディレクトリにします。

1. `dotnet new console -lang vb` コマンドを入力します。

   このコマンドにより、*Program.vb* というファイル名の Visual Basic ソース コードとともに、ファイル拡張子が `.vbproj` のプロジェクト ファイルが作成されます。 このファイル内に、"Hello World!" という文字列を コンソール ウィンドウに表示するためのソース コードが含まれています。

1. `dotnet run` コマンドを入力します。 [.NET Core CLI ](../tools/index.md)によりアプリケーションが自動的にコンパイルされて実行され、"Hello World!" メッセージが表示されます。 コンソール ウィンドウに表示します。

### <a name="support-for-c-71"></a>C# 7.1 のサポート

.NET core 2.0 では、次のような新機能が追加された C# 7.1 がサポートされています。

- アプリケーション エントリ ポイントである `Main` メソッドを、[async](../../csharp/language-reference/keywords/async.md) キーワードでマークできます。
- 推定タプル名。
- 既定の式。

<!-- For more information see [link to C# what's new](url). -->

## <a name="platform-improvements"></a>プラットフォームの機能強化

.NET core 2.0 には、.NET Core のインストールを簡略化し、サポートされているオペレーティング システム上で .NET Core を使用する機能がいくつか含まれています。

### <a name="net-core-for-linux-is-a-single-implementation"></a>.NET Core for Linux は単一実装

.NET core 2.0 では、複数の Linux ディストリビューションで動作する単一の Linux 実装を提供します。 .NET core 1.x では、ディストリビューション固有の Linux 実装をダウンロードする必要がありました。

単一のオペレーティング システムとしての Linux をターゲットするアプリを開発することもできます。 .NET Core 1.x では、ディストリビューションごとに別の Linux をターゲットする必要がありました。

### <a name="support-for-the-apple-cryptographic-libraries"></a>Apple 暗号化ライブラリのサポート

macOS の .NET Core 1.x では、OpenSSL ツールキットの暗号化ライブラリが必要でした。 .NET Core 2.0 では、Apple 暗号化ライブラリを使用し、OpenSSL を必要としないため、OpenSSL をインストールする必要がありません。

## <a name="api-changes-and-library-support"></a>API の変更とライブラリのサポート

### <a name="support-for-net-standard-20"></a>.NET Standard 2.0 のサポート

.NET Standard は、そのバージョンの標準に準拠した .NET 実装で使用する必要がある、バージョン管理された API のセットを定義します。 .NET Standard はライブラリ開発者を対象としています。 .NET Standard の目的は、.NET 実装ごとの .NET Standard のバージョンに対応するライブラリで使用できる機能を保証することです。 .NET Core 1.x では、.NET Standard バージョン 1.6 がサポートされており、.NET Core 2.0 では最新の .NET Standard 2.0 がサポートされています。 詳細については、「[.NET Standard](../../standard/net-standard.md)」をご覧ください。

.NET Standard 2.0 には、.NET Standard 1.6 で使用できた 20,000 個以上の API が含まれています。 この拡張されたアクセス領域の多くは、.NET Framework と Xamarin に共通する API を .NET Standard に組み込んだ結果です。

.NET Standard 2.0 クラス ライブラリは、.NET Framework クラス ライブラリを参照することもできます。ただし、呼び出す API が .NET Standard 2.0 内に存在している必要があります。 .NET Framework ライブラリを再コンパイルする必要はありません。

最新バージョンの .NET Standard 1.6 以降に、.NET Standard に追加された API の一覧については、「[.NET Standard 2.0 vs. 1.6](https://raw.githubusercontent.com/dotnet/standard/master/docs/versions/netstandard2.0_diff.md)」をご覧ください。

### <a name="expanded-surface-area"></a>拡張されたアクセス領域

.NET Core 2.0 で使用できる API の数は、.NET Core 1.1 の 2 倍以上です。

また、[Windows 互換機能パック](../porting/windows-compat-pack.md)により、.NET Framework からの移植がより簡単になりました。

### <a name="support-for-net-framework-libraries"></a>.NET Framework ライブラリのサポート

.NET Core のコードは、既存の NuGet パッケージなど、既存の .NET Framework ライブラリを参照できます。 ライブラリは .NET Standard にある API を使用する必要があることに注意してください。

## <a name="visual-studio-integration"></a>Visual Studio の統合環境

Visual Studio 2017 バージョン 15.3 (場合によって Visual Studio for Mac) では、.NET Core 開発者のために十分な数の拡張機能をご用意しています。

### <a name="retargeting-net-core-apps-and-net-standard-libraries"></a>.NET Core アプリと .NET Standard ライブラリの再ターゲット

.NET Core 2.0 SDK がインストールされていれば、.NET Core 1.x プロジェクトを .NET Core 2.0 に、.NET Standard 1.x ライブラリを .NET Standard 2.0 に再ターゲットすることができます。

Visual Studio のプロジェクトを再ターゲットするには、そのプロジェクトのプロパティ ダイアログの **[アプリケーション]** タブを開き、 **[ターゲット フレームワーク]** の値を「 **.NET Core 2.0**」または「 **.NET Standard 2.0**」に変更します。 プロジェクトを右クリックして**編集\*.csproj ファイル** オプションを選択することで、変更することもできます。 詳細については、前述の「[ツール](#tooling)」セクションをご覧ください。

### <a name="live-unit-testing-support-for-net-core"></a>.NET Core のライブ単体テスト対応

コードを変更すると常に、Live Unit Testing は影響を受ける単体テストをバックグラウンドで自動的に実行して、テスト結果とコード カバレッジをライブで Visual Studio 環境に表示します。 .NET core 2.0 では現在、Live Unit Testing がサポートされています。 以前は、.NET Framework アプリケーションでのみ Live Unit Testing が使用できました。

詳細については、「[Visual Studio での Live Unit Testing](/visualstudio/test/live-unit-testing)」と「[Live Unit Testing に関する FAQ](/visualstudio/test/live-unit-testing-faq)」をご覧ください。

### <a name="better-support-for-multiple-target-frameworks"></a>複数のターゲット フレームワークのサポートを向上

複数のターゲット フレームワークのプロジェクトをビルドする場合は、トップレベルのメニューからターゲット プラットフォームを選択できます。 次の図では、SCD1 という名前のプロジェクトが 64 ビット macOS X 10.11 (`osx.10.11-x64`) と 64 ビット Windows 10/Windows Server 2016 (`win10-x64`) をターゲットにしています。 プロジェクト ボタンを選択する前にターゲット フレームワークを選択できます。今回はデバッグ ビルドを実行するために選択します。

![プロジェクトをビルドするときのターゲット フレームワークの選択を示すスクリーンショット。](./media/dotnet-core-2-0/target-framework-selection.png)

### <a name="side-by-side-support-for-net-core-sdks"></a>.NET Core SDK の side-by-side サポート

.NET Core SDK を Visual Studio とは別にインストールできます。 これにより、異なるバージョンの.NET Core をターゲットしているプロジェクトを 1 つのバージョンの Visual Studio でビルドできます。 以前は、Visual Studio と .NET Core SDK は密接に連携していました (特定のバージョンの Visual Studio に特定のバージョンの SDK が付属していました)。

## <a name="documentation-improvements"></a>ドキュメントの改善

### <a name="net-application-architecture"></a>.NET アプリケーション アーキテクチャ

.NET を使用して次のものをビルドする際は、[.NET アプリケーション アーキテクチャ](https://dotnet.microsoft.com/learn/dotnet/architecture-guides)にある、ガイダンス、ベスト プラクティス、サンプル アプリケーションを掲載した電子ブックをご覧ください。

- [マイクロサービスと Docker コンテナー](../../architecture/microservices/index.md)
- [ASP.NET を使用した Web アプリケーション](../../architecture/modern-web-apps-azure/index.md)
- [Xamarin を使用したモバイル アプリケーション](/xamarin/xamarin-forms/enterprise-application-patterns/index)
- [Azure Cloud にデプロイされたアプリケーション](/azure/architecture/reference-architectures/index)

## <a name="see-also"></a>関連項目

- [ASP.NET Core 2.0 の新機能](/aspnet/core/aspnetcore-2.0)
