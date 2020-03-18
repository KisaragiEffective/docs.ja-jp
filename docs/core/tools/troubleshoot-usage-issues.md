---
title: .NET Core ツールの使用に関する問題のトラブルシューティング
description: .NET Core ツールを実行するときの一般的な問題と考えられる解決策について説明します。
author: kdollard
ms.date: 02/14/2020
ms.openlocfilehash: ab5d1be8f201ea283f8537f18886feab46157127
ms.sourcegitcommit: 771c554c84ba38cbd4ac0578324ec4cfc979cf2e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2020
ms.locfileid: "77543275"
---
# <a name="troubleshoot-net-core-tool-usage-issues"></a>.NET Core ツールの使用に関する問題のトラブルシューティング

グローバル ツールまたはローカル ツールとして .NET Core ツールをインストールまたは実行しようとすると、問題が発生することがあります。 この記事では、一般的な根本原因と考えられる解決策について説明します。

## <a name="installed-net-core-tool-fails-to-run"></a>インストールされている .NET Core ツールを実行できない

.NET Core ツールの実行が失敗する場合、最も可能性が高いのは、次のいずれかの問題が発生していることです。

* ツールの実行可能ファイルが見つかりませんでした。
* 正しいバージョンの .NET Core ランタイムが見つかりませんでした。

### <a name="executable-file-not-found"></a>実行可能ファイルが見つからない

実行可能ファイルが見つからない場合は、次のようなメッセージが表示されます。

```console
Could not execute because the specified command or file was not found.
Possible reasons for this include:
  * You misspelled a built-in dotnet command.
  * You intended to execute a .NET Core program, but dotnet-xyz does not exist.
  * You intended to run a global tool, but a dotnet-prefixed executable with this name could not be found on the PATH.
```

実行可能ファイルの名前によって、ツールの呼び出し方法が決まります。 次の表ではその形式について説明します。

| 実行可能ファイル名の形式  | 呼び出し方式   |
|-------------------------|---------------------|
| `dotnet-<toolName>.exe` | `dotnet <toolName>` |
| `<toolName>.exe`        | `<toolName>`        |

* グローバル ツール

    グローバル ツールは、既定のディレクトリ内または特定の場所にインストールすることができます。 既定のディレクトリは次のとおりです。

    | OS          | パス                          |
    |-------------|-------------------------------|
    | Linux/macOS | `$HOME/.dotnet/tools`         |
    | Windows     | `%USERPROFILE%\.dotnet\tools` |

    グローバル ツールを実行しようとしている場合は、コンピューター上の `PATH` 環境変数にグローバル ツールをインストールしたパスが含まれること、およびそのパスに実行可能ファイルが存在することを確認してください。

    .NET Core CLI は、初めて使用したときに既定の場所を PATH 環境変数に追加しようとします。 ただし、場所が PATH に自動的に追加されないシナリオがいくつかあるため、次のような場合は PATH を編集して構成する必要があります。

  * Linux を使用していて、apt-get または rpm ではなく *.tar.gz* ファイルを使用して .NET Core SDK をインストールしている場合。
  * macOS 10.15 "Catalina" またはそれ以降のバージョンを使用している場合。
  * macOS 10.14 "Mojave" 以前のバージョンを使用していて、 *.pkg* ではなく *.tar.gz* ファイルを使用して .NET Core SDK をインストールしている場合。
  * .NET Core 3.0 SDK をインストールしてあり、`DOTNET_ADD_GLOBAL_TOOLS_TO_PATH` 環境変数を `false` に設定している場合。
  * .NET Core 2.2 SDK 以前のバージョンをインストールしてあり、`DOTNET_SKIP_FIRST_TIME_EXPERIENCE` 環境変数を `true` に設定している場合。

  詳細については、[.NET Core ツール](global-tools.md)に関する記事を参照してください。

* ローカル ツール

  ローカル ツールを実行しようとしている場合は、*dotnet-tools.json* という名前のマニフェストファイルが、現在のディレクトリまたはそのいずれかの親ディレクトリにあることを確認します。 このファイルは、ルート フォルダーではなく、プロジェクト フォルダー階層のどこかにある *.config* という名前のフォルダーに存在することもあります。 *dotnet-tools.json* が存在する場合は、それを開き、実行しようとしているツールを確認します。 ファイルに `"isRoot": true` のエントリが含まれていない場合は、さらに上のファイル階層で他のツール マニフェスト ファイルを確認します。

  指定されたパスにインストールされている .NET Core ツールを実行しようとしている場合は、ツールを使用するときにそのパスを含める必要があります。 ツールパスにインストールされたツールを使用する例を次に示します。

  ```console
  ..\<toolDirectory>\dotnet-<toolName>
  ```

### <a name="runtime-not-found"></a>ランタイムが見つからない

.NET Core ツールは、[フレームワークに依存するアプリケーション](../deploying/index.md#publish-runtime-dependent)です。つまり、お使いのマシンにインストールされている .NET Core ランタイムに依存します。 予定していたランタイムが見つからない場合、ツールは次のような通常の .NET Core ランタイムのロール フォワード ルールに従います。

* アプリケーションは、指定されたメジャー バージョンおよびマイナー バージョンの最上位の修正プログラム リリースにロール フォワードされます。
* 一致するメジャー バージョン番号とマイナー バージョン番号と一致するランタイムがない場合は、次に高いマイナー バージョンが使用されます。
* ランタイムのプレビュー バージョン間、またはプレビュー バージョンとリリース バージョン間では、ロール フォワードは発生しません。 したがって、プレビュー バージョンを使用して作成された .NET Core ツールは、作成者がリビルドして再パブリッシュしてから再インストールする必要があります。

次の 2 つの一般的なシナリオでは、ロールフォワードは既定では実行されません。

* 使用できるのが古いバージョンのランタイムだけの場合。 ロールフォワードでは、ランタイムの新しいバージョンのみが選択されます。
* 使用できるのが高いメジャー バージョンのランタイムだけの場合。 ロールフォワードでは、メジャー バージョンの境界を越えることはありません。

アプリケーションで適切なランタイムが見つからない場合は、実行が失敗し、エラーが報告されます。

次のいずれかのコマンドを使用して、お使いのコンピューターにインストールされている .NET Core ランタイムを確認することができます。

```dotnetcli
dotnet --list-runtimes
dotnet --info
```

現在インストールされているランタイム バージョンがツールでサポートされている必要があると思われる場合は、ツールの作成者に連絡して、バージョン番号またはマルチターゲットを更新できるかどうかを確認してください。 更新されたバージョン番号でツール パッケージが再コンパイルされて NuGet に再発行された後、コピーを更新することができます。 それまでの間の最も簡単な解決策は、実行しようとしているツールで動作するランタイムのバージョンをインストールすることです。 特定の .NET Core ランタイム バージョンをダウンロードするには、[.NET Core ダウンロード ページ](https://dotnet.microsoft.com/download/dotnet-core)を参照してください。

既定以外の場所に .NET Core SDK をインストールする場合は、`dotnet` 実行可能ファイルが格納されているディレクトリに、環境変数 `DOTNET_ROOT` を設定する必要があります。

## <a name="net-core-tool-installation-fails"></a>.NET Core ツールのインストールが失敗する

いろいろな理由で、.NET Core のグローバル ツールまたはローカル ツールのインストールが失敗する可能性があります。 ツールのインストールが失敗すると、次のようなメッセージが表示されます。

```console
Tool '{0}' failed to install. This failure may have been caused by:

* You are attempting to install a preview release and did not use the --version option to specify the version.
* A package by this name was found, but it was not a .NET Core tool.
* The required NuGet feed cannot be accessed, perhaps because of an Internet connection problem.
* You mistyped the name of the tool.

For more reasons, including package naming enforcement, visit https://aka.ms/failure-installing-tool
```

これらのエラーの診断に役立つよう、上のメッセージと共に、NuGet のメッセージがユーザーに直接表示されます。 NuGet のメッセージは、問題の特定に役立つ場合があります。

### <a name="package-naming-enforcement"></a>パッケージの名前付けの適用

Microsoft では、ツールのパッケージ ID に関するガイダンスを変更したため、いくつかのツールが予測される名前で見つからなくなっています。 新しいガイダンスでは、すべての Microsoft ツールに "Microsoft" というプレフィックスが付いています。 このプレフィックスは予約されており、Microsoft が承認した証明書で署名されたパッケージにのみ使用できます。

移行の間は、古い形式のパッケージ ID を持つ Microsoft ツールと、新しい形式を持つツールが混在します。

```dotnetcli
dotnet tool install -g Microsoft.<toolName>
dotnet tool install -g <toolName>
```

パッケージ ID が更新されると、最新の更新プログラムを取得するには、新しいパッケージ ID に変更する必要があります。 ツール名が簡略化されているパッケージは非推奨となります。

### <a name="preview-releases"></a>プレビュー リリース

* プレビュー リリースをインストールしようとして、`--version` オプションを使用してバージョンを指定しませんでした。

プレビュー段階にある .NET Core ツールは、プレビュー段階であることを、名前の一部で指定する必要があります。 プレビュー全体を含める必要はありません。 バージョン番号が想定される形式であると仮定すると、次の例のようなものを使用できます。

```dotnetcli
dotnet tool install -g --version 1.1.0-pre <toolName>
```

### <a name="package-isnt-a-net-core-tool"></a>パッケージが .NET Core ツールではない

* この名前の NuGet パッケージは見つかりましたが、.NET Core ツールではありませんでした。

.NET Core ツールではなく、通常の NuGet パッケージである NuGet パッケージをインストールしようとすると、次のようなエラーが表示されます。

> NU1212:`<ToolName>` のプロジェクトとパッケージの組み合わせが無効です。 DotnetToolReference プロジェクト形式には、DotnetTool タイプの参照のみ含めることができます。

### <a name="nuget-feed-cant-be-accessed"></a>NuGet フィードにアクセスできない

* おそらくインターネット接続に問題があるため、必要な NuGet フィードにアクセスできません。

ツールをインストールするには、ツール パッケージが含まれている NuGet フィードにアクセスする必要があります。 フィードが使用できない場合は失敗します。 `nuget.config` でフィードを変更するか、特定の `nuget.config` ファイルを要求するか、`--add-source` スイッチを使用して追加のフィードを指定することができます。 既定では、NuGet では接続できないフィードに対してエラーがスローされます。 フラグ `--ignore-failed-sources` を指定すると、これらの到達できないソースをスキップできます。

### <a name="package-id-incorrect"></a>パッケージ ID が正しくない

* ツールの名前が間違って入力されています。

失敗でよくある理由は、ツール名が正しくないことです。 これは、入力ミスや、ツールが移動されたか非推奨になったために、発生する可能性があります。 NuGet.org のツールで、確実に正しい名前を使用する方法の 1 つは、NuGet.org でツールを検索し、インストール コマンドをコピーすることです。

## <a name="see-also"></a>関連項目

* [.NET Core ツール](global-tools.md)
