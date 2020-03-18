---
title: dotnet tool install コマンド
description: dotnet tool install コマンドでは、お使いのコンピューター上に指定された .NET Core ツールをインストールします。
ms.date: 02/14/2020
ms.openlocfilehash: 641e6a2753b1cf3bfc334ba2495342f7c42421fc
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78156975"
---
# <a name="dotnet-tool-install"></a>dotnet tool install

**この記事の対象:** ✔️ .NET Core 2.1 SDK 以降のバージョン

## <a name="name"></a>名前

`dotnet tool install` - お使いのコンピューター上に指定された [.NET Core ツール](global-tools.md)をインストールします。

## <a name="synopsis"></a>構文

```dotnetcli
dotnet tool install <PACKAGE_NAME> <-g|--global> [--add-source] [--configfile] [--framework] [-v|--verbosity] [--version]
dotnet tool install <PACKAGE_NAME> <--tool-path> [--add-source] [--configfile] [--framework] [-v|--verbosity] [--version]
dotnet tool install <PACKAGE_NAME> [--add-source] [--configfile] [--framework] [-v|--verbosity] [--version]
dotnet tool install <-h|--help>
```

## <a name="description"></a>説明

`dotnet tool install` コマンドでは、お使いのコンピューター上に .NET Core ツールをインストールする方法を提供します。 コマンドを使用するには、次のいずれかのインストール オプションを指定します。

* 既定の場所にグローバル ツールをインストールするには、`--global` オプションを使用します。
* カスタムの場所にグローバル ツールをインストールするには、`--tool-path` オプションを使用します。
* ローカル ツールをインストールするには、`--global` および `--tool-path` オプションを省略します。

**ローカル ツールは .NET Core SDK 3.0 以降で使用できます。**

グローバル ツールは、`-g` または `--global` オプションを指定した場合、既定で次のディレクトリにインストールされます。

| OS          | パス                          |
|-------------|-------------------------------|
| Linux/macOS | `$HOME/.dotnet/tools`         |
| Windows     | `%USERPROFILE%\.dotnet\tools` |

ローカル ツールは、現在のディレクトリ下にある *.config*ディレクトリ内の *tool-manifest.json* ファイルに追加されます。 マニフェスト ファイルがまだ存在しない場合は、次のコマンドを実行して作成します。

```dotnetcli
dotnet new tool-manifest
```

詳細については、「[ローカル ツールのインストール](global-tools.md#install-a-local-tool)」を参照してください。

## <a name="arguments"></a>引数

- **`PACKAGE_NAME`**

  インストールする .NET Core ツールが格納されている NuGet パッケージの名前または ID。

## <a name="options"></a>オプション

- **`add-source <SOURCE>`**

  インストール時に使用するために追加の NuGet パッケージ ソースを追加します。

- **`configfile <FILE>`**

  使用する NuGet 構成 (*nuget.config*) ファイル。

- **`framework <FRAMEWORK>`**

  ツールをインストールする[ターゲット フレームワーク](../../standard/frameworks.md)を指定します。 既定では、.NET Core SDK は最適なターゲット フレームワークの選択を試みます。

- **`-g|--global`**

  ユーザー全体のインストールを指定します。 `--tool-path` オプションと組み合わせることはできません。 `--global` と `--tool-path` の両方を省略すると、ローカル ツールのインストールが指定されます。

- **`-h|--help`**

  コマンドの短いヘルプを印刷します。

- **`tool-path <PATH>`**

  グローバル ツールをインストールする場所を指定します。 PATH は絶対パスでも相対パスでもかまいません。 PATH が存在しない場合、コマンドではパスの作成を試みます。 `--global` と `--tool-path` の両方を省略すると、ローカル ツールのインストールが指定されます。

- **`-v|--verbosity <LEVEL>`**

  コマンドの詳細レベルを設定します。 指定できる値は、`q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]`、および `diag[nostic]` です。

- **`--version <VERSION_NUMBER>`**

  インストールするツールのバージョン。 既定では、安定した最新バージョンのパッケージがインストールされます。 このオプションを使用して、プレビューまたは古いバージョンのツールをインストールします。

## <a name="examples"></a>使用例

- **`dotnet tool install -g dotnetsay`**

  既定の場所に [dotnetsay](https://www.nuget.org/packages/dotnetsay/) をグローバル ツールとしてインストールします。

- **`dotnet tool install dotnetsay --tool-path c:\global-tools`**

  特定の Windows ディレクトリに [dotnetsay](https://www.nuget.org/packages/dotnetsay/) をグローバル ツールとしてインストールします。

- **`dotnet tool install dotnetsay --tool-path ~/bin`**

  特定の Linux/macOS ディレクトリに [dotnetsay](https://www.nuget.org/packages/dotnetsay/) をグローバル ツールとしてインストールします。

- **`dotnet tool install -g dotnetsay --version 2.0.0`**

  バージョン 2.0.0 の [dotnetsay](https://www.nuget.org/packages/dotnetsay/) をグローバル ツールとしてインストールします。

- **`dotnet tool install dotnetsay`**

  現在のディレクトリに [dotnetsay](https://www.nuget.org/packages/dotnetsay/) をローカル ツールとしてインストールします。

## <a name="see-also"></a>関連項目

- [.NET Core ツール](global-tools.md)
