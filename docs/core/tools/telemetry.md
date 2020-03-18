---
title: .NET Core SDK 製品利用統計情報
description: 利用情報を収集して分析する .NET Core SDK の製品利用統計情報機能や収集されるデータ、およびこの機能を無効にする方法について説明します。
author: KathleenDollard
ms.date: 08/27/2019
ms.openlocfilehash: 9d5d7ff09ade89712f2fbbe35224851bb1c28b4c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78156687"
---
# <a name="net-core-sdk-telemetry"></a>.NET Core SDK 製品利用統計情報

[.NET Core SDK](index.md) には、.NET Core CLI がクラッシュしたとき、利用データと例外情報を収集する製品利用統計情報機能が含まれています。 .NET Core CLI は .NET Core SDK を備える、.NET Core アプリのビルド、テスト、発行を可能にする動詞のセットです。 .NET Team が、ツールの改善に向けて、その使用方法を把握することが重要です。 エラーに関する情報は、チームが問題を解決し、バグを修正するのに役立ちます。

収集されたデータは匿名であり、[Creative Commons Attribution License](https://creativecommons.org/licenses/by/4.0/) の下で全体が公開されます。

## <a name="scope"></a>スコープ

`dotnet` には、アプリを実行し、CLI コマンドを実行するための 2 つの関数があります。 `dotnet` を使用して次の形式でアプリケーションを起動するとき、製品利用統計情報は "*収集されません*"。

- `dotnet [path-to-app].dll`

次のような [.NET Core CLI コマンド](index.md)を使用するとき、製品利用統計情報は "*収集されます*"。

- `dotnet build`
- `dotnet pack`
- `dotnet run`

## <a name="how-to-opt-out"></a>オプトアウトする方法

.NET Core SDK の製品利用統計情報機能は既定では有効になっています。 製品利用統計情報機能をオプトアウトするには、`DOTNET_CLI_TELEMETRY_OPTOUT` 環境変数を `1` または `true` に設定します。

正常にインストールされると、製品利用統計情報のエントリ 1 件も .NET Core SDK インストーラーによって送信されます。 オプトアウトするには、.NET Core SDK をインストールする前に `DOTNET_CLI_TELEMETRY_OPTOUT` 環境変数を設定します。

## <a name="disclosure"></a>開示

いずれかの [.NET Core CLI コマンド](index.md) (`dotnet build` など) を初めて実行するときは、.NET Core SDK では次のテキストと同様のものが表示されます。 テキストは、実行している SDK のバージョンによって多少異なります。 この "最初の実行" の際に、Microsoft がデータ回収に関して通知する方法が示されます。

```console
Telemetry
---------
The .NET Core tools collect usage data in order to help us improve your experience. The data is anonymous. It is collected by Microsoft and shared with the community. You can opt-out of telemetry by setting the DOTNET_CLI_TELEMETRY_OPTOUT environment variable to '1' or 'true' using your favorite shell.

Read more about .NET Core CLI Tools telemetry: https://aka.ms/dotnet-cli-telemetry
```

## <a name="data-points"></a>データ ポイント

製品利用統計情報機能では、ユーザー名やメール アドレスなどの個人データは収集されません。 コードはスキャンされず、名前、リポジトリ、作成者などのプロジェクト レベルのデータは抽出されません。 データは、[Azure Monitor](https://azure.microsoft.com/services/monitor/) テクノロジを使用して Microsoft サーバーに安全に送信され、制限されたアクセスの下で保持され、厳格なセキュリティ コントロールの下で、安全な [Azure Storage](https://azure.microsoft.com/services/storage/) システムから公開されます。

ユーザーのプライバシー保護は Microsoft にとって重要です。 製品利用統計情報で機密データが収集されている、またはデータが安全でないか不適切な方法で処理されていることが疑われる場合、[dotnet/cli](https://github.com/dotnet/cli/issues) リポジトリで問題を提出するか、[dotnet@microsoft.com](mailto:dotnet@microsoft.com) に電子メールを送信し、調査を依頼してください。

製品利用統計情報の機能では次のデータが収集されます。

| SDK バージョン | データ |
|--------------|------|
| すべて          | 呼び出しのタイムスタンプ。 |
| すべて          | 呼び出されたコマンド ("build" など)、2.1 以降はハッシュされます。 |
| すべて          | 地理的な場所を決定するために使用する 3 つのオクテットの IP アドレス。 |
| すべて          | オペレーティング システムとバージョン。 |
| すべて          | SDK が実行されているランタイム ID (RID)。 |
| すべて          | .NET Core SDK バージョン。 |
| すべて          | 製品利用統計情報プロファイル: 明示的なユーザー オプトインでのみ使用され、Microsoft では内部で使用される任意の値。 |
| >=2.0        | コマンドの引数とオプション: (任意の文字列ではなく) いくつかの引数とオプションが収集されます。 [収集されるオプション](#collected-options)に関するページを参照してください。 2\.1.300 以降はハッシュされます。 |
| >=2.0         | SDK がコンテナーで実行されているかどうか。 |
| >=2.0         | ターゲット フレームワーク (`TargetFramework` イベントから)、2.1 以降はハッシュされます。 |
| >=2.0         | ハッシュされたメディア アクセス制御 (MAC) アドレス: マシンの、暗号化された (SHA256) 匿名で一意の ID。 |
| >=2.0         | ハッシュされた現在の作業ディレクトリ。 |
| >=2.0         | インストール成功レポート、インストーラーの実行ファイルの名前がハッシュされます。 |
| >=2.1.300     | カーネル バージョン。 |
| >=2.1.300     | Libc リリース/バージョン。 |
| >=3.0.100     | 出力がリダイレクトされるかどうか (True または False)。 |
| >=3.0.100     | CLI/SDK クラッシュ時の例外の種類とそのスタック トレース (CLI/SDK コードのみ、送信されたスタック トレースに含まれます)。 詳細は、[.NET Core CLI/SDK クラッシュ例外製品利用統計情報の収集](#net-core-clisdk-crash-exception-telemetry-collected)に関するページを参照してください。 |

### <a name="collected-options"></a>収集されるオプション

一部のコマンドでは、追加データが送信されます。 コマンドのサブセットで最初の引数が送信されます。

| コマンド               | 送信される最初の引数データ                |
|-----------------------|-----------------------------------------|
| `dotnet help <arg>`   | コマンドのヘルプが求められます。  |
| `dotnet new <arg>`    | テンプレート名 (ハッシュ済み)。             |
| `dotnet add <arg>`    | `package` または `reference` という単語。      |
| `dotnet remove <arg>` | `package` または `reference` という単語。      |
| `dotnet list <arg>`   | `package` または `reference` という単語。      |
| `dotnet sln <arg>`    | `add`、`list`、または `remove` という単語。    |
| `dotnet nuget <arg>`  | `delete`、`locals`、または `push` という単語。 |

選択されたオプションが使用される場合は、コマンドのサブセットによって、その値と共に送信されます。

| オプション                  | コマンド                                                                                       |
|-------------------------|------------------------------------------------------------------------------------------------|
| `--verbosity`           | すべてのコマンド                                                                                   |
| `--language`            | `dotnet new`                                                                                   |
| `--configuration`       | `dotnet build`, `dotnet clean`, `dotnet publish`, `dotnet run`, `dotnet test`                  |
| `--framework`           | `dotnet build`, `dotnet clean`, `dotnet publish`, `dotnet run`, `dotnet test`, `dotnet vstest` |
| `--runtime`             | `dotnet build`、`dotnet publish`                                                              |
| `--platform`            | `dotnet vstest`                                                                                |
| `--logger`              | `dotnet vstest`                                                                                |
| `--sdk-package-version` | `dotnet migrate`                                                                               |

`--verbosity` と `--sdk-package-version` を除き、他のすべての値は .Net Core 2.1.100 SDK 以降、ハッシュされます。

## <a name="net-core-clisdk-crash-exception-telemetry-collected"></a>収集される .NET Core CLI/SDK クラッシュ例外製品利用統計情報

.NET Core CLI/SDK がクラッシュした場合、例外の名前と CLI/SDK コードのスタック トレースが収集されます。 この情報は、問題を評価し、.NET Core SDK と CLI の品質を向上させる目的で収集されます。 この記事では、Microsoft が収集するデータについて説明します。 また、独自のバージョンの .NET Core SDK をビルドするユーザーが個人情報や機密情報の不注意による漏洩を回避する方法に関するヒントも提供します。

### <a name="types-of-collected-data"></a>収集されるデータの種類

.NET Core CLI では、お使いのアプリケーションの例外についてではなく、CLI/SDK の例外についてのみ、情報が収集されます。 収集されたデータには、例外の名前とスタック トレースが含まれます。 このスタック トレースは CLI/SDK コードです。

次の例では、収集されたデータの種類を確認できます。

```console
System.IO.IOException
at System.ConsolePal.WindowsConsoleStream.Write(Byte[] buffer, Int32 offset, Int32 count)
at System.IO.StreamWriter.Flush(Boolean flushStream, Boolean flushEncoder)
at System.IO.StreamWriter.Write(Char[] buffer)
at System.IO.TextWriter.WriteLine()
at System.IO.TextWriter.SyncTextWriter.WriteLine()
at Microsoft.DotNet.Cli.Utils.Reporter.WriteLine()
at Microsoft.DotNet.Tools.Run.RunCommand.EnsureProjectIsBuilt()
at Microsoft.DotNet.Tools.Run.RunCommand.Execute()
at Microsoft.DotNet.Tools.Run.RunCommand.Run(String[] args)
at Microsoft.DotNet.Cli.Program.ProcessArgs(String[] args, ITelemetry telemetryClient)
at Microsoft.DotNet.Cli.Program.Main(String[] args)
```

### <a name="avoid-inadvertent-disclosure-of-information"></a>不注意による情報の開示を避ける

.NET Core の共同作成者と、自分でビルドした .NET Core SDK のバージョンを実行しているユーザーは、SDK ソース コードのパスを考慮する必要があります。 カスタムのデバッグ ビルドであるか、カスタムのビルド シンボル ファイルで構成されている .NET Core SDK の使用時にクラッシュが発生した場合、ビルド コンピューターからの SDK ソース ファイル パスはスタック トレースの一部としては収集されず、ハッシュされません。

そのため、.NET Core SDK のカスタム ビルドは、パス名によって個人情報や機密情報が公開されるディレクトリには置かないでください。

## <a name="see-also"></a>関連項目

- [.NET Core CLI 製品利用統計情報 - 2019 年第 2 四半期データ](https://dotnet.microsoft.com/platform/telemetry/dotnet-core-cli-2019q2)
- [製品利用統計情報の参照のソース (dotnet/cli リポジトリ)](https://github.com/dotnet/cli/tree/master/src/dotnet/Telemetry)
