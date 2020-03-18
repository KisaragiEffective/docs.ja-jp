---
title: dotnet run コマンド
description: dotnet run コマンドは、ソース コードからアプリケーションを実行する便利なオプションを提供します。
ms.date: 02/19/2020
ms.openlocfilehash: e442ed56d676ffd189ef6d394d840cea671c2dc6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78157077"
---
# <a name="dotnet-run"></a>dotnet run

**この記事の対象:** ✔️ .NET Core 2.x SDK 以降のバージョン

## <a name="name"></a>名前

`dotnet run` -- 明示的なコンパイルや起動コマンドなしで、ソース コードを実行します。

## <a name="synopsis"></a>構文

```dotnetcli
dotnet run [-c|--configuration] [-f|--framework] [--force] [--interactive] [--launch-profile]
    [--no-build] [--no-dependencies] [--no-launch-profile] [--no-restore] [-p|--project]
    [-r|--runtime] [-v|--verbosity] [[--] [application arguments]]
dotnet run [-h|--help]
```

## <a name="description"></a>説明

`dotnet run` コマンドは、1 つのコマンドを使用して、ソース コードからアプリケーションを実行する便利なオプションを提供します。 コマンド ラインからの短期間の反復開発に便利です。 このコマンドは [`dotnet build`](dotnet-build.md) コマンドに依存し、コードをビルドします。 そのため、プロジェクトを最初に復元する必要があるなど、ビルドに要件があれば、それが `dotnet run` にも適用されます。

出力ファイルは既定の場所である `bin/<configuration>/<target>` に書き込まれます。 たとえば、`netcoreapp2.1` というアプリケーションがあり、`dotnet run` を実行する場合、`bin/Debug/netcoreapp2.1` に出力されます。 必要に応じて、ファイルは上書きされます。 一時ファイルは `obj` ディレクトリに置かれます。

フレームワークを複数指定するプロジェクトの場合、`-f|--framework <FRAMEWORK>` オプションを使用してフレームワークを指定しない限り、`dotnet run` を実行するとエラーが発生します。

`dotnet run` コマンドは、ビルド済みのアセンブリではなく、プロジェクトのコンテキストで使用されます。 代わりにフレームワークに依存するアプリケーション DLL の実行を試みる場合は、コマンドなしで [dotnet](dotnet.md) を実行する必要があります。 たとえば、`myapp.dll` を実行する場合は、次のコードを使用します。

```dotnetcli
dotnet myapp.dll
```

`dotnet` ドライバーの詳細については、[.NET Core コマンド ライン ツール (CLI)](index.md) に関する記事を参照してください。

アプリケーションを実行するため、`dotnet run` コマンドは、NuGet キャッシュから共有ランタイムの外にあるアプリケーションの依存関係を解決します。 このコマンドではキャッシュされた依存関係を使用するため、`dotnet run` を使用してアプリケーションを実稼働環境で実行することは推奨されません。 代わりに、[`dotnet publish`](dotnet-publish.md) コマンドを使用して[展開を作成](../deploying/index.md)し、発行された出力を展開します。

[!INCLUDE[dotnet restore note + options](~/includes/dotnet-restore-note-options.md)]

## <a name="options"></a>オプション

- **`--`**

  実行中のアプリケーションの引数と `dotnet run` の引数を区切ります。 この区切り記号の後の引数はすべて実行中のアプリケーションに渡されます。

- **`-c|--configuration <CONFIGURATION>`**

  ビルド構成を定義します。 ほとんどのプロジェクトの既定値は `Debug` ですが、プロジェクトでビルド構成設定をオーバーライドできます。

- **`-f|--framework <FRAMEWORK>`**

  指定された[フレームワーク](../../standard/frameworks.md)を使用してアプリをビルドし、実行します。 フレームワークはプロジェクト ファイルに指定する必要があります。

- **`--force`**

  最後の復元が成功した場合でも、すべての依存関係が強制的に解決されます。 このフラグを指定することは、*project.assets.json* ファイルを削除することと同じです。

- **`-h|--help`**

  コマンドの短いヘルプを印刷します。

- **`--interactive`**

  コマンドを停止して、ユーザーの入力または操作のために待機させることができます (たとえば、認証を完了する場合)。 .NET Core 3.0 SDK 以降で使用できます。

- **`--launch-profile <NAME>`**

  アプリケーションを起動する場合に使用する起動プロファイル (存在する場合) の名前です。 起動プロファイルは *launchSettings.json* ファイル内に定義されており、通常は、`Development`、`Staging`、`Production` と呼ばれています。 詳細については、[「Working with multiple environments」](/aspnet/core/fundamentals/environments) (複数の環境での使用) をご覧ください。

- **`--no-build`**

  実行前にプロジェクトをビルドしません。 また、`--no-restore` フラグが暗黙的に設定されます。

- **`--no-dependencies`**

  プロジェクト間 (P2P) 参照を含むプロジェクトを復元する場合は、参照ではなく、ルート プロジェクトを復元します。

- **`--no-launch-profile`**

  アプリケーションを構成するための *launchSettings.json* の使用を試みません。

- **`--no-restore`**

  コマンドを実行するときに、暗黙的な復元を実行しません。

- **`-p|--project <PATH>`**

  実行するプロジェクト ファイルのパスを指定します (フォルダー名または完全なパス)。 指定しない場合は、既定で現在のディレクトリに設定されます。

- **`-r|--runtime <RUNTIME_IDENTIFIER>`**

  パッケージを復元するターゲット ランタイムを指定します。 ランタイム ID (RID) の一覧については、[RID カタログ](../rid-catalog.md)に関するページをご覧ください。 .NET Core 3.0 SDK 以降、`-r` の短いオプションを使用できます。

- **`-v|--verbosity <LEVEL>`**

  コマンドの詳細レベルを設定します。 指定できる値は、`q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]`、および `diag[nostic]` です。 既定値は `m` です。 .NET Core 2.1 SDK 以降で利用できます。

## <a name="examples"></a>使用例

- 現在のディレクトリのプロジェクトを実行します。

  ```dotnetcli
  dotnet run
  ```

- 指定されたプロジェクトを実行します。

  ```dotnetcli
  dotnet run --project ./projects/proj1/proj1.csproj
  ```

- 現在のディレクトリのプロジェクトを実行します (この例では、空の `--` オプションが使用されているため、`--help` 引数がアプリケーションに渡されます)。

  ```dotnetcli
  dotnet run --configuration Release -- --help
  ```

- 現在のディレクトリでプロジェクトの依存関係とツールを復元し、最小限の出力のみを表示して、プロジェクトを実行します (.NET Core SDK 2.0 以降のバージョン)。

  ```dotnetcli
  dotnet run --verbosity m
  ```
