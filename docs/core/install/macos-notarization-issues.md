---
title: macOS Catalina の公証に対応する
description: macOS で、.NET Core ランタイム、SDK、および .Net Core でビルドされたアプリをインストールするときに、公証と、証明書に関する問題を処理する方法について説明します。
author: thraka
ms.author: adegeo
ms.date: 02/14/2020
ms.openlocfilehash: be39c1ea56699f84736a2b37bc958507b16e826b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79146750"
---
# <a name="macos-catalina-notarization-and-the-impact-on-net-core-downloads-and-projects"></a>macOS Catalina の公証と、.NET Core のダウンロードおよびプロジェクトへの影響

macOS Catalina (バージョン 10.15) 以降では、2019 年 6 月 1 日より後に作成され、Developer ID と共に配布されたすべてのソフトウェアは公証される必要があります。 この要件は、.NET Core ランタイム、.NET Core SDK、および .NET Core を使用して作成されたソフトウェアに適用されます。 この記事では、.NET Core と macOS の公証に関して発生する可能性のある一般的なシナリオについて説明します。

## <a name="installing-net-core"></a>.NET Core のインストール

.NET Core (ランタイムと SDK の両方) バージョン 3.1、3.0、2.1 のインストーラーは、2020 年 2 月 18 日から公証されています。 それより前にリリースされたバージョンは、公証されていません。 公証されていないバージョンの .NET Core は、最初にインストーラーをダウンロードしてから、`sudo installer` コマンドを使用することにより、手動でインストールできます。 詳しくは、[macOS でのダウンロードと手動インストール](sdk.md?pivots=os-macos#download-and-manually-install)に関する記事をご覧ください。

次のバージョン以降では、.NET Core インストーラーは公証されるようになっています。

- .NET Core ランタイム
  - 2.1.16
  - 3.0.3
  - 3.1.2

- .NET Core SDK
  - 2.1.512
  - 3.0.103
  - 3.1.102

## <a name="apphost-is-disabled-by-default"></a>appHost は既定で無効になる

既定では、プロジェクトでコンパイル、発行、または実行を行うと、.NET Core SDK 3.0 以降の公証されていないバージョンで、ネイティブの Mach-O 実行可能ファイル (**appHost** と呼ばれます) が生成されます。 この実行可能ファイルは、アプリを実行するための便利な方法です。 それ以外の場合は、`dotnet <filename.dll>` を実行することによってアプリを起動する必要があります。 appHost が有効になっていると、`dotnet run` コマンドは appHost のコンテキストで呼び出されます。 詳しくは、「[appHost のコンテキスト](#context-of-the-apphost)」をご覧ください。

公証されたバージョンの .NET Core SDK 3.0 以降では、appHost 実行可能ファイルは既定では生成されません。 プロジェクト ファイルの [`UseAppHost`](../project-sdk/msbuild-props.md#useapphost) ブール値の設定を使用して、appHost の生成を有効にできます。 また、コマンド ラインで `-p:UseAppHost` パラメーターを使用することにより、実行する特定の `dotnet` コマンドについて appHost を切り替えることもできます。

- プロジェクト ファイル

  ```xml
  <PropertyGroup>
    <UseAppHost>true</UseAppHost>
  </PropertyGroup>
  ```

- コマンド ライン パラメーター

  ```dotnetcli
  dotnet run -p:UseAppHost=true
  ```

[自己完結型](../deploying/index.md#publish-self-contained)のアプリを発行すると、常に appHost が作成されます。

`UseAppHost` 設定の詳細については、[Microsoft.NET.Sdk の MSBuild プロパティ](../project-sdk/msbuild-props.md#useapphost)に関する記事を参照してください。

### <a name="context-of-the-apphost"></a>appHost のコンテキスト

プロジェクトで appHost が有効になっている場合、`dotnet run` コマンドを使用してアプリを実行すると、アプリは、既定のホスト (既定のホストは `dotnet` コマンドです) ではなく、appHost のコンテキストで呼び出されます。 プロジェクトで appHost が無効になっている場合は、`dotnet run` コマンドを使用すると、既定のホストのコンテキストでアプリが実行されます。 appHost が無効になっている場合でも、自己完結型としてアプリを発行すると、appHost 実行可能ファイルが生成され、ユーザーはその実行可能ファイルを使用してアプリを実行します。 `dotnet <filename.dll>` でアプリを実行すると、共有ランタイムである既定のホストを使用してアプリが呼び出されます。

appHost を使用するアプリが呼び出されるときは、アプリによってアクセスされる証明書パーティションが、公証された既定のホストとは異なります。 既定のホストによってインストールされた証明書にアクセスする必要があるアプリの場合は、`dotnet run` コマンドを使用してプロジェクト ファイルからアプリを実行するか、`dotnet <filename.dll>` コマンドを使用してアプリを直接起動します。

このシナリオについて詳しくは、「[ASP.NET Core と macOS および証明書](#aspnet-core-and-macos-and-certificates)」セクションをご覧ください。

## <a name="aspnet-core-and-macos-and-certificates"></a>ASP.NET Core と macOS および証明書

.NET Core には、<xref:System.Security.Cryptography.X509Certificates> クラスを使用して macOS キーチェーンの証明書を管理する機能が用意されています。 macOS キーチェーンへのアクセスでは、考慮するパーティションを決定するときに、アプリケーション ID が主キーとして使用されます。 たとえば、署名されていないアプリケーションでは、署名されていないパーティションにシークレットが格納されますが、署名されたアプリケーションでは、そのアプリケーションだけがアクセスできるパーティションにシークレットが格納されます。 アプリを呼び出す実行のソースによって、使用するパーティションが決まります。

.NET Core では、実行のソースとして、[appHost](#apphost-is-disabled-by-default)、既定のホスト (`dotnet` コマンド)、カスタム ホストの 3 種類が提供されています。 各実行モデルでは、異なる ID を使用でき (署名あり、または署名なし)、キーチェーン内の異なるパーティションにアクセスできます。 あるモードでインポートされた証明書には、別のモードからはアクセスできない可能性があります。 たとえば、.NET Core の公証されたバージョンには、署名された既定のホストがあります。 証明書は、その ID に基づいて、セキュリティで保護されたパーティションにインポートされます。 appHost が署名されていないため、これらの証明書には、生成された appHost からはアクセスできません。

別の例として、既定では、ASP.NET Core によって既定のホストから既定の SSL 証明書がインポートされます。 appHost を使用する ASP.NET Core アプリケーションでは、この証明書にアクセスできないため、証明書にアクセスできないことが .NET Core で検出されたときにエラーを受け取ります。 エラー メッセージでは、この問題を解決するための方法が示されています。

証明書の共有が必要な場合、macOS では `security` ユーティリティで構成オプションが提供されています。

ASP.NET Core 証明書の問題をトラブルシューティングする方法について詳しくは、「[ASP.NET Core に HTTPS を適用する](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1&tabs=visual-studio#troubleshoot-certificate-problems)」をご覧ください。

## <a name="default-entitlements"></a>既定の権利

.NET Core の既定のホスト (`dotnet` コマンド) には、一連の既定の権利があります。 .NET Core が適切に動作するには、これらの権利が必要です。 アプリケーションでは追加の権利が必要になることがあり、その場合は、[appHost](#apphost-is-disabled-by-default) を生成して使用し、必要な権利をローカルに追加する必要があります。

.NET Core の既定の権利セット:

- `com.apple.security.cs.allow-jit`
- `com.apple.security.cs.allow-unsigned-executable-memory`
- `com.apple.security.cs.allow-dyld-environment-variables`
- `com.apple.security.cs.disable-library-validation`

## <a name="notarize-a-net-core-app"></a>.NET Core アプリを公証する

macOS Catalina (バージョン 10.15) 以降でアプリケーションを実行する場合は、アプリを公証することをお勧めします。 公証のためにアプリケーションと共に送信する appHost は、少なくとも .NET Core と同じ[既定の権利](#default-entitlements)で使用する必要があります。

## <a name="next-steps"></a>次の手順

- [.NET Core の依存関係と要件](dependencies.md)。
- [.NET Core SDK をインストール](sdk.md)します。
- [.NET Core ランタイムをインストールする](runtime.md)
