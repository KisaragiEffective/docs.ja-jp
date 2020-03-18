---
title: dotnet nuget push コマンド
description: dotnet nuget push コマンドでは、パッケージをサーバーにプッシュして発行します。
author: karann-msft
ms.date: 02/14/2020
ms.openlocfilehash: d4ef8e58908fe488c712debff3b313ac0908b43e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77503660"
---
# <a name="dotnet-nuget-push"></a>dotnet nuget push

**この記事の対象:** ✔️ .NET Core 2.x SDK 以降のバージョン

## <a name="name"></a>name

`dotnet nuget push` - パッケージをサーバーにプッシュして発行します。

## <a name="synopsis"></a>構文

```dotnetcli
dotnet nuget push [<ROOT>] [-d|--disable-buffering] [--force-english-output] [--interactive] [-k|--api-key] [-n|--no-symbols]
    [--no-service-endpoint] [-s|--source] [--skip-duplicate] [-sk|--symbol-api-key] [-ss|--symbol-source] [-t|--timeout]
dotnet nuget push [-h|--help]
```

## <a name="description"></a>[説明]

`dotnet nuget push` コマンドは、パッケージをサーバーにプッシュして発行します。 プッシュ コマンドでは、システムの NuGet 構成ファイル、または構成ファイルのチェーンで検出されたサーバーと資格情報の詳細を使用します。 構成ファイルの詳細については、「[Configuring NuGet Behavior](/nuget/consume-packages/configuring-nuget-behavior)」 (NuGet 動作を構成する) をご覧ください。 NuGet の既定の構成は、 *%AppData%\NuGet\NuGet.config* (Windows) または *$HOME/.local/share* (Linux/macOS) を読み込み、次にドライブのルートから開始され、現在のディレクトリで終わる、任意の *nuget.config* または *.nuget\nuget.config* を読み込むことによって取得されます。

## <a name="arguments"></a>引数

- **`ROOT`**

  プッシュされるパッケージのファイル パスを指定します。

## <a name="options"></a>オプション

- **`-d|--disable-buffering`**

  メモリ使用量を削減するために、HTTP(S) サーバーにプッシュするときのバッファリングを無効にします。

- **`--force-english-output`**

  インバリアントの英語ベースのカルチャを使用して、アプリケーションの実行を強制します。

- **`-h|--help`**

  コマンドの短いヘルプを印刷します。

- **`--interactive`**

  コマンドが認証などの操作をブロックして、手動アクションを要求することを許可します。 .NET Core 2.2 SDK 以降、使用できるオプションです。

- **`-k|--api-key <API_KEY>`**

  サーバーの API キーです。

- **`-n|--no-symbols`**

  シンボルをプッシュしません (存在する場合でも)。

- **`--no-service-endpoint`**

  ソース URL に "api/v2/package" を追加しません。 .NET Core 2.1 SDK 以降、使用できるオプションです。

- **`-s|--source <SOURCE>`**

  サーバー URL を指定します。 `DefaultPushSource` 構成値が NuGet 構成ファイルに設定されない限り、このオプションは必須です。

- **`--skip-duplicate`**

  複数のパッケージを HTTP(S) サーバーにプッシュする場合は、すべての 409 競合応答を警告として処理して、プッシュを続行できるようにします。 .NET Core 3.1 SDK 以降で利用できます。

- **`-sk|--symbol-api-key <API_KEY>`**

  シンボル サーバーの API キーです。

- **`-ss|--symbol-source <SOURCE>`**

  シンボル サーバーの URL を指定します。

- **`-t|--timeout <TIMEOUT>`**

  秒単位でサーバーにプッシュする場合のタイムアウトを指定します。 既定値は 300 秒 (5 分) です。 0 (0 秒) を指定すると、既定値が適用されます。

## <a name="examples"></a>例

- API キーを指定して、既定のプッシュ ソースに *foo.nupkg* をプッシュします。

  ```dotnetcli
  dotnet nuget push foo.nupkg -k 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a
  ```

- API キーを指定して、公式 NuGet サーバーに *foo.nupkg* をプッシュします。

  ```dotnetcli
  dotnet nuget push foo.nupkg -k 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -s https://api.nuget.org/v3/index.json
  ```
  
  * API キーを指定して、カスタム プッシュ ソース *に*foo.nupkg`https://customsource` をプッシュします。

  ```dotnetcli
  dotnet nuget push foo.nupkg -k 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -s https://customsource/
  ```

- 既定のプッシュ ソースに *foo.nupkg* をプッシュします。

  ```dotnetcli
  dotnet nuget push foo.nupkg
  ```

- 既定のシンボル ソースに *foo.symbols.nupkg* をプッシュします。

  ```dotnetcli
  dotnet nuget push foo.symbols.nupkg
  ```

- 360 秒のタイムアウトを指定して、既定のプッシュ ソースに *foo.nupkg* をプッシュします。

  ```dotnetcli
  dotnet nuget push foo.nupkg --timeout 360
  ```

- 既定のプッシュ ソースに現在のディレクトリ内のすべての *.nupkg* ファイルをプッシュします。

  ```dotnetcli
  dotnet nuget push *.nupkg
  ```

  > [!NOTE]
  > このコマンドがうまくいかない場合は、古いバージョンの SDK (.NET Core 2.1 SDK 以前のバージョン) に存在したバグが原因である可能性があります。
  > これを解決するには、SDK のバージョンをアップグレードするか、代わりに次のコマンドを実行します: `dotnet nuget push **/*.nupkg`

- HTTP(S) サーバーによって 409 競合応答が返された場合でも、すべての *.nupkg* ファイルをプッシュします。

  ```dotnetcli
  dotnet nuget push *.nupkg --skip-duplicate
  ```
