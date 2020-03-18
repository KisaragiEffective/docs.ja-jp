---
title: dotnet store コマンド
description: "'dotnet store' コマンドは、指定したアセンブリをランタイム パッケージ ストアに格納します。"
ms.date: 02/14/2020
ms.openlocfilehash: da1d132b2b873ff55ec104b5bb092d0194889bdc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77503584"
---
# <a name="dotnet-store"></a>dotnet store

**この記事の対象:** ✔️ .NET Core 2.x SDK 以降のバージョン

## <a name="name"></a>name

`dotnet store` - 指定したアセンブリを[ランタイム パッケージ ストア](../deploying/runtime-store.md)に格納します。

## <a name="synopsis"></a>構文

```dotnetcli
dotnet store -m|--manifest -f|--framework -r|--runtime  [--framework-version] [-h|--help] [--output] [--skip-optimization] [--skip-symbols] [-v|--verbosity] [--working-dir]
```

## <a name="description"></a>[説明]

`dotnet store` - 指定したアセンブリを[ランタイム パッケージ ストア](../deploying/runtime-store.md)に格納します。 既定では、アセンブリは、ターゲット ランタイムとフレームワークに合わせて最適化されます。 詳細については、[「ランタイム パッケージ ストア」](../deploying/runtime-store.md) を参照してください。

## <a name="required-options"></a>必須オプション

- **`-f|--framework <FRAMEWORK>`**

  [ターゲット フレームワーク](../../standard/frameworks.md)を指定します。 ターゲット フレームワークはプロジェクト ファイルに指定する必要があります。

- **`-m|--manifest <PATH_TO_MANIFEST_FILE>`**

  "*パッケージ ストア マニフェスト ファイル*" は、格納するパッケージの一覧を含む XML ファイルです。 マニフェスト ファイルの形式は、SDK スタイル プロジェクト形式と互換性があります。 そのため、必要なパッケージを参照するプロジェクト ファイルを `-m|--manifest` オプションと使用することで、ランタイム パッケージ ストアにアセンブリを格納することができます。 複数のマニフェスト ファイルを指定するには、ファイルごとにオプションとパスを繰り返します。 たとえば、`--manifest packages1.csproj --manifest packages2.csproj` のように指定します。

- **`-r|--runtime <RUNTIME_IDENTIFIER>`**

  ターゲットとする[ランタイム識別子](../rid-catalog.md)。

## <a name="optional-options"></a>省略可能なオプション

- **`--framework-version <FRAMEWORK_VERSION>`**

  .NET Core SDK のバージョンを指定します。 このオプションでは、`-f|--framework` オプションで指定したフレームワーク以降の特定のフレームワーク バージョンを選択することができます。

- **`-h|--help`**

  ヘルプ情報を表示します。

- **`-o|--output <OUTPUT_DIRECTORY>`**

  ランタイム パッケージ ストアへのパスを指定します。 指定しない場合、ユーザー プロファイル .NET Core インストール ディレクトリのサブディレクトリ *store* が既定値となります。

- **`--skip-optimization`**

  最適化フェーズをスキップします。

- **`--skip-symbols`**

  シンボルの生成をスキップします。 現時点では、Windows と Linux 上でのみシンボルを生成することができます。

- **`-v|--verbosity <LEVEL>`**

  コマンドの詳細レベルを設定します。 指定できる値は、`q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]`、および `diag[nostic]` です。

- **`-w|--working-dir <WORKING_DIRECTORY>`**

  コマンドで使用される作業ディレクトリです。 指定しない場合、現在のディレクトリの *obj* サブディレクトリが使用されます。

## <a name="examples"></a>例

- .NET Core 2.0.0 の *packages.csproj* プロジェクト ファイルで指定されたパッケージを格納します。

  ```dotnetcli
  dotnet store --manifest packages.csproj --framework-version 2.0.0
  ```

- *packages.csproj* で指定されたパッケージを最適化なしで格納します。

  ```dotnetcli
  dotnet store --manifest packages.csproj --skip-optimization
  ```

## <a name="see-also"></a>参照

- [ランタイム パッケージ ストア](../deploying/runtime-store.md)
