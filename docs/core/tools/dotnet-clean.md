---
title: dotnet clean コマンド
description: dotnet clean コマンドは現在のディレクトリを消去します。
ms.date: 02/14/2020
ms.openlocfilehash: 186f1ea07718a8e178f88c3d079cf6e2f1f8660b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77503752"
---
# <a name="dotnet-clean"></a>dotnet clean

**この記事の対象:** ✔️ .NET Core 2.x SDK 以降のバージョン

## <a name="name"></a>name

`dotnet clean` - プロジェクトの出力を消去します。

## <a name="synopsis"></a>構文

```dotnetcli
dotnet clean [<PROJECT>|<SOLUTION>] [-c|--configuration] [-f|--framework] [--interactive]
    [--nologo] [-o|--output] [-r|--runtime] [-v|--verbosity]
dotnet clean [-h|--help]
```

## <a name="description"></a>[説明]

`dotnet clean` コマンドは、以前のビルドの出力を消去します。 [MSBuild ターゲット](/visualstudio/msbuild/msbuild-targets)として実装されます。そのため、コマンドの実行時にプロジェクトが評価されます。 ビルド中に作成された出力のみが消去されます。 中間 (*obj*) と最終出力 (*bin*) フォルダーの両方が消去されます。

## <a name="arguments"></a>引数

`PROJECT | SOLUTION`

クリーンにする MSBuild プロジェクトまたはソリューション。 プロジェクトまたはソリューションのファイルを指定しない場合、MSBuild は、現在の作業ディレクトリから *proj* または *sln* のどちらかで終わるファイル拡張子を持つファイルを検索して、そのファイルを使います。

## <a name="options"></a>オプション

* **`-c|--configuration <CONFIGURATION>`**

  ビルド構成を定義します。 ほとんどのプロジェクトの既定値は `Debug` ですが、プロジェクトでビルド構成設定をオーバーライドできます。 このオプションは、ビルド時に指定した場合にのみ、消去時にも必要です。

* **`-f|--framework <FRAMEWORK>`**

  ビルド時に指定された[フレームワーク](../../standard/frameworks.md)です。 フレームワークは、[プロジェクト ファイル](csproj.md)で定義する必要があります。 ビルド時にフレームワークを指定した場合は、消去時にフレームワークを指定する必要があります。

* **`-h|--help`**

  コマンドの短いヘルプを印刷します。

* **`--interactive`**

  コマンドを停止して、ユーザーの入力または操作のために待機させることができます。 たとえば、認証を完了する場合があります。 .NET Core 3.0 SDK 以降で使用できます。

* **`--nologo`**

  著作権情報を表示しません。 .NET Core 3.0 SDK 以降で使用できます。

* **`-o|--output <OUTPUT_DIRECTORY>`**

  クリーンにするビルド成果物を含むディレクトリ。 プロジェクトのビルド時にフレームワークを指定した場合、出力ディレクトリ スイッチと共に `-f|--framework <FRAMEWORK>` スイッチを指定します。

* **`-r|--runtime <RUNTIME_IDENTIFIER>`**

  指定したランタイムの出力フォルダーをクリーンアップします。 これは、[自己完結型の展開](../deploying/index.md#publish-self-contained)が作成された場合に使用されます。

* **`-v|--verbosity <LEVEL>`**

  MSBuild の詳細レベルを設定します。 指定できる値は、`q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]`、および `diag[nostic]` です。 既定値は、`normal` です。

## <a name="examples"></a>例

* プロジェクトの既定のビルドを消去します。

  ```dotnetcli
  dotnet clean
  ```

* リリース構成を使用してビルドされたプロジェクトを消去します。

  ```dotnetcli
  dotnet clean --configuration Release
  ```
