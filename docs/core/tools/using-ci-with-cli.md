---
title: .NET Core SDK とツールを使用した継続的インテグレーション (CI)
description: 継続的インテグレーションで .NET Core SDK とそのツールをビルド サーバー上で使用する方法について説明します。
ms.date: 05/18/2017
ms.openlocfilehash: 6e23a21dd36422a095e56519c9aa28ce2549f7b2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77451039"
---
# <a name="using-net-core-sdk-and-tools-in-continuous-integration-ci"></a>継続的インテグレーション (CI) で .NET Core SDK とツールを使用する

この文書では、.NET Core SDK とそのツールをビルド サーバーで使用する方法について説明します。 .NET Core ツールセットは対話式と自動の両方に対応しています。対話式の場合、開発者はコマンド プロンプトにコマンドを入力します。自動の場合、継続的インテグレーション (CI) サーバーがビルド スクリプトを実行します。 コマンド、オプション、入力、出力は同じです。ユーザーは、ツールの取得方法とアプリを構築するシステムだけを指定します。 このドキュメントでは、CI のツール取得のシナリオと、ビルド スクリプトの設計と構造化の方法に関する推奨事項を取り上げます。

## <a name="installation-options-for-ci-build-servers"></a>CI ビルド サーバーのインストール オプション

### <a name="using-the-native-installers"></a>ネイティブ インストーラーの使用

macOS、Linux、Windows の場合、ネイティブ インストーラーを利用できます。 このインストーラーは、ビルド サーバーへの admin (sudo) アクセスを必要とします。 ネイティブ インストーラーを使用することの利点は、ツールを実行するために必要なあらゆるネイティブ依存性がインストールされることです。 ネイティブ インストーラーはまた、システム全体に SDK をインストールします。

macOS をご利用の場合、PKG インストーラーをお使いください。 Linux の場合、フィードベースのパッケージ マネージャーを利用できます。Ubuntu の apt-get や CentOS の yum などです。あるいは、パッケージ自体、DEB または RPM を利用できます。 Windows の場合、MSI インストーラーをご利用ください。

最新の安定したバイナリは、[.NET ダウンロード](https://dotnet.microsoft.com/download)のページにあります。 最新の (安定性に欠ける可能性がある) プレリリース ツールを使用する場合、[dotnet/core-sdk GitHub リポジトリ](https://github.com/dotnet/core-sdk#installers-and-binaries)のリンクをご利用ください。 Linux ディストリビューションの場合、`tar.gz` アーカイブ (別名、`tarballs`) をご利用いただけます。アーカイブ内のインストール スクリプトを利用して .NET Core をインストールしてください。

### <a name="using-the-installer-script"></a>インストーラー スクリプトの使用

インストーラー スクリプトを使用すると、ビルド サーバーで管理者以外のインストールが可能になり、ツールの取得を簡単に自動化できます。 スクリプトにツールがダウンロードされ、既定の場所か指定された場所に抽出されます。 インストールするツールのバージョンと、SDK 全体をインストールするか、それとも共有ランタイムだけをインストールするかも指定できます。

インストーラー スクリプトは、ビルドの開始時に実行され、必要なバージョンの SDK を取得し、インストールするように自動化されています。 *必要なバージョン*とは、プロジェクトのビルドに必要な SDK のバージョンです。 このスクリプトでは、サーバーのローカル ディレクトリに SDK をインストールし、インストールした場所からツールを実行し、ビルド後にクリーンアップできます (あるいは、CI サービスにクリーンアップさせます)。 これにより、ビルド プロセス全体にカプセル化と分離性が提供されます。 インストール スクリプト参照は [dotnet-install](dotnet-install-script.md) の記事にあります。

> [!NOTE]
> **Azure DevOps Services**
>
> インストーラー スクリプトの使用時、ネイティブ依存性は自動的にはインストールされません。 オペレーティング システムにネイティブ依存性がない場合、それをインストールする必要があります。 詳細については、[.NET Core の依存関係と要件](../install/dependencies.md)に関する記事を参照してください。

## <a name="ci-setup-examples"></a>CI セットアップ例

このセクションでは、PowerShell またはバッシュ スクリプトを利用した手動セットアップについて説明し、SaaS (サービスとしてのソフトウェア) CI (継続的インテグレーション) ソリューションをいくつか紹介します。 対象となる SaaS CI ソリューションは [Travis CI](https://travis-ci.org/)、[AppVeyor](https://www.appveyor.com/)、および [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/index) です。

### <a name="manual-setup"></a>手動セットアップ

各 SaaS サービスには、ビルド プロセスを作成し、構成するための独自の方法があります。 一覧に記載されている以外の SaaS ソリューションを使用する場合、あるいは事前にパッケージに含まれているサポート以上のカスタマイズが必要な場合、少なくとも一部に手動構成が必要になります。

全般的に、手動セットアップでは、あるバージョンのツール (あるいはツールの最新のナイトリー ビルド) を取得し、ビルド スクリプトを実行する必要があります。 PowerShell またはバッシュ スクリプトを使用し、.NET Core コマンドを調整したり、プロジェクト ファイルを使用してビルド プロセスの概要を伝えたりできます。 [オーケストレーション セクション](#orchestrating-the-build)で、これらのオプションについて詳しく説明されています。

手動 CI ビルド サーバー セットアップを実行するスクリプトを作成したら、それを開発マシンで使用し、テスト目的でコードをローカルでビルドします。 スクリプトがローカルで問題なく動作していることを確認したら、CI ビルド サーバーに展開します。 比較的単純な PowerShell スクリプトで、.NET Core SDK を取得し、Windows ビルド サーバーにインストールする方法を実演します。

```powershell
$ErrorActionPreference="Stop"
$ProgressPreference="SilentlyContinue"

# $LocalDotnet is the path to the locally-installed SDK to ensure the
#   correct version of the tools are executed.
$LocalDotnet=""
# $InstallDir and $CliVersion variables can come from options to the
#   script.
$InstallDir = "./cli-tools"
$CliVersion = "1.0.1"

# Test the path provided by $InstallDir to confirm it exists. If it
#   does, it's removed. This is not strictly required, but it's a
#   good way to reset the environment.
if (Test-Path $InstallDir)
{
    rm -Recurse $InstallDir
}
New-Item -Type "directory" -Path $InstallDir

Write-Host "Downloading the CLI installer..."

# Use the Invoke-WebRequest PowerShell cmdlet to obtain the
#   installation script and save it into the installation directory.
Invoke-WebRequest `
    -Uri "https://dot.net/v1/dotnet-install.ps1" `
    -OutFile "$InstallDir/dotnet-install.ps1"

Write-Host "Installing the CLI requested version ($CliVersion) ..."

# Install the SDK of the version specified in $CliVersion into the
#   specified location ($InstallDir).
& $InstallDir/dotnet-install.ps1 -Version $CliVersion `
    -InstallDir $InstallDir

Write-Host "Downloading and installation of the SDK is complete."

# $LocalDotnet holds the path to dotnet.exe for future use by the
#   script.
$LocalDotnet = "$InstallDir/dotnet"

# Run the build process now. Implement your build script here.
```

スクリプトの終わりで、自分のビルド プロセスの実装を指定します。 スクリプトはツールを取得し、ビルド プロセスを実行します。 UNIX マシンの場合、次のバッシュ スクリプトが PowerShell スクリプトに記載されているアクションを同様の方法で実行します。

```bash
#!/bin/bash
INSTALLDIR="cli-tools"
CLI_VERSION=1.0.1
DOWNLOADER=$(which curl)
if [ -d "$INSTALLDIR" ]
then
    rm -rf "$INSTALLDIR"
fi
mkdir -p "$INSTALLDIR"
echo Downloading the CLI installer.
$DOWNLOADER https://dot.net/v1/dotnet-install.sh > "$INSTALLDIR/dotnet-install.sh"
chmod +x "$INSTALLDIR/dotnet-install.sh"
echo Installing the CLI requested version $CLI_VERSION. Please wait, installation may take a few minutes.
"$INSTALLDIR/dotnet-install.sh" --install-dir "$INSTALLDIR" --version $CLI_VERSION
if [ $? -ne 0 ]
then
    echo Download of $CLI_VERSION version of the CLI failed. Exiting now.
    exit 0
fi
echo The CLI has been installed.
LOCALDOTNET="$INSTALLDIR/dotnet"
# Run the build process now. Implement your build script here.
```

### <a name="travis-ci"></a>Travis CI

[ 言語と ](https://travis-ci.org/) キーを使用して .NET Core SDK をインストールするように `csharp`Travis CI`dotnet` を構成できます。 詳細については、公式 Travis CI ドキュメントの「[Building a C#, F#, or Visual Basic Project](https://docs.travis-ci.com/user/languages/csharp/)」(C#、F#、または Visual Basic プロジェクトのビルド) を参照してください。 Travis CI 情報にアクセスするときは、コミュニティが保守管理している `language: csharp` 言語識別子は F# や Mono を含む、あらゆる .NET 言語で機能することにご留意ください。

Travis CI は、*ビルド マトリックス*において、macOS ジョブと Linux ジョブの両方を実行できます。ビルド マトリックスでは、ランタイム、環境、除外/追加の組み合わせを指定し、アプリのビルド組み合わせを範囲に含めます。 詳細については、Travis CI ドキュメントの「[Customizing the Build](https://docs.travis-ci.com/user/customizing-the-build)」 (ビルドをカスタマイズする) の記事を参照してください。 MSBuild ベースのツールのパッケージには、LTS (1.0.x) ランタイムと Current (1.1.x) ランタイムが含まれています。SDK をインストールすることで、ビルドに必要なすべてが与えられます。

### <a name="appveyor"></a>AppVeyor

[AppVeyor](https://www.appveyor.com/) は、`Visual Studio 2017` worker イメージで .NET Core 1.0.1 SDK をインストールします。 異なるバージョンの .NET Core SDK を含む他のビルド イメージも利用できます。 詳細については、[appveyor.yml の例](https://github.com/dotnet/docs/blob/master/appveyor.yml)と AppVoyor ドキュメントの [worker イメージのビルド](https://www.appveyor.com/docs/build-environment/#build-worker-images)に関する記事を参照してください。

.NET Core SDK バイナリがインストール スクリプトを利用してダウンロードされ、解凍され、`PATH` 環境変数に追加されます。 複数バージョンの .NET Core SDK との統合テストを実行するためにビルド マトリックスを追加する:

```yaml
environment:
  matrix:
    - CLI_VERSION: 1.0.1
    - CLI_VERSION: Latest

install:
  # See appveyor.yml example for install script
```

### <a name="azure-devops-services"></a>Azure DevOps Services

以下のいずれかの手法で .NET Core プロジェクトをビルドするように Azure DevOps Services を構成します。

1. コマンドを利用し、[手動セットアップ手順](#manual-setup)からスクリプトを実行します。
1. .NET Core ツールを使用するように構成されたいくつかの Azure DevOps Services 組み込みビルド タスクで構成されるビルドを作成します。

いずれのソリューションも有効です。 ツールはビルドの一部としてダウンロードしているので、手動セットアップ スクリプトを使用し、取得したツールのバージョンを管理します。 ビルドはスクリプトから実行されます。そのスクリプトを作成する必要があります。 この記事では、手動オプションについてのみ説明します。 Azure DevOps Services ビルド タスクを使用したビルドの構築の詳細については、[Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/index) のドキュメントを参照してください。

Azure DevOps Services で手動セットアップ スクリプトを使用するには、新しいビルド定義を作成し、ビルド手順で実行するスクリプトを指定します。 これは Azure DevOps Services ユーザー インターフェイスを使用して実行できます。

1. 最初に新しいビルド定義を作成します。 作成するビルドの種類を定義するためのオプションを指定する画面が表示されたら、 **[空]** オプションを選択します。

   ![ビルド定義で空を選択する](./media/using-ci-with-cli/select-empty-build-definition.png)

1. ビルドするリポジトリを構成すると、ビルド定義が表示されます。 **[ビルド ステップの追加...]** を選択します。

   ![ビルド ステップの追加](./media/using-ci-with-cli/add-build-step.png)

1. **[タスク カタログ]** が表示されます。 このカタログには、ビルドで使用するタスクが含まれています。 スクリプトがあるので、**PowerShell: Run a PowerShell スクリプト**の **[追加]** ボタンを選択します。

   ![PowerShell スクリプトの追加手順](./media/using-ci-with-cli/add-powershell-script.png)

1. ビルド手順を構成します。 ビルドしているリポジトリからスクリプトを追加します。

   ![実行する PowerShell スクリプトを指定する](./media/using-ci-with-cli/powershell-script-path.png)

## <a name="orchestrating-the-build"></a>ビルドの調整

この文書はその大半で .NET Core ツールの取得方法とさまざまな CI サービスの構成方法について説明しています。 .NET Core でコードを調整する (*実際にビルドする*) 方法に関する情報はありません。 ビルド プロセスの構造化方法の選択肢は、ここでは取り上げることができないさまざまな要因に依存します。 [Travis CI](https://travis-ci.org/)、[AppVeyor](https://www.appveyor.com/)、[Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/index) でビルドを調整する方法については、それぞれの文書に記載されている資料とサンプルを参照してください。

.NET Core ツールを利用して .NET Core コードのビルド プロセスを構造化するとき、通常、2 つの手法があります。MSBuild を直接利用するか、.NET Core コマンドライン コマンドを利用します。 いずれの手法を採用するかは、手法と複雑性との兼ね合いで使いやすいものを選択してください。 MSBuild を利用すれば、タスクやターゲットとしてビルド プロセスを表現できますが、MSBuild プロジェクト ファイルの構文は複雑で、学習の難易度が上がります。 .NET Core コマンドライン ツールはおそらく、使い方がより単純です。ただし、`bash` や PowerShell のようなスクリプト記述言語でオーケストレーション ロジックを記述する必要があります。

## <a name="see-also"></a>参照

- [.NET ダウンロード - Linux](https://dotnet.microsoft.com/download?initial-os=linux)
