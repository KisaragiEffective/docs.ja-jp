---
title: dotnet tool update コマンド
description: dotnet tool update コマンドでは、お使いのコンピューター上の指定された .NET Core ツールを更新します。
ms.date: 02/14/2020
ms.openlocfilehash: 80e807a0fc06ad762334f888e701f6d9c448369a
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78156947"
---
# <a name="dotnet-tool-update"></a>dotnet tool update

**この記事の対象:** ✔️ .NET Core 2.1 SDK 以降のバージョン

## <a name="name"></a>名前

`dotnet tool update` - お使いのコンピューター上の指定された [.NET Core ツール](global-tools.md)を更新します。

## <a name="synopsis"></a>構文

```dotnetcli
dotnet tool update <PACKAGE_NAME> <-g|--global> [--configfile] [--framework] [-v|--verbosity] [--add-source]
dotnet tool update <PACKAGE_NAME> <--tool-path> [--configfile] [--framework] [-v|--verbosity] [--add-source]
dotnet tool update <PACKAGE_NAME> [--configfile] [--framework] [-v|--verbosity] [--add-source]
dotnet tool update <-h|--help>
```

## <a name="description"></a>説明

`dotnet tool update` コマンドでは、お使いのコンピューター上の .NET Core ツールをパッケージの最新の安定バージョンに更新するための方法を提供します。 このコマンドは、ツールをアンインストールして再インストールして、効果的に更新します。 コマンドを使用するには、次のいずれかのオプションを指定します。

* 既定の場所にインストールされたグローバル ツールを更新するには、`--global` オプションを使用します
* カスタムの場所にインストールされたグローバル ツールを更新するには、`--tool-path` オプションを使用します。
* ローカル ツールを更新するには、`--global` および `--tool-path` オプションを省略します。

**ローカル ツールは .NET Core SDK 3.0 以降で使用できます。**

## <a name="arguments"></a>引数

- **`PACKAGE_NAME`**

  更新する .NET Core グローバル ツールが格納されている NuGet パッケージの名前または ID。 パッケージを見つけるには、[dotnet tool list](dotnet-tool-list.md) コマンドを使用できます。

## <a name="options"></a>オプション

- **`--add-source <SOURCE>`**

  インストール時に使用するために追加の NuGet パッケージ ソースを追加します。

- **`--configfile <FILE>`**

  使用する NuGet 構成 (*nuget.config*) ファイル。

- **`--framework <FRAMEWORK>`**

  ツールを更新する[ターゲット フレームワーク](../../standard/frameworks.md)を指定します。

- **`-g|--global`**

  更新プログラムがユーザー全体のツール用であることを指定します。 `--tool-path` オプションと組み合わせることはできません。 `--global` と `--tool-path` の両方を省略すると、更新されるツールはローカル ツールであると指定されます。

- **`-h|--help`**

  コマンドの短いヘルプを印刷します。

- **`--tool-path <PATH>`**

  グローバル ツールがインストールされている場所を指定します。 PATH は絶対パスでも相対パスでもかまいません。 `--global` オプションと組み合わせることはできません。 `--global` と `--tool-path` の両方を省略すると、更新されるツールはローカル ツールであると指定されます。

- **`-v|--verbosity <LEVEL>`**

  コマンドの詳細レベルを設定します。 指定できる値は、`q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]`、および `diag[nostic]` です。

## <a name="examples"></a>使用例

- **`dotnet tool update -g dotnetsay`**

  [dotnetsay](https://www.nuget.org/packages/dotnetsay/) グローバル ツールを更新します。

- **`dotnet tool update dotnetsay --tool-path c:\global-tools`**

  特定の Windows ディレクトリに配置されている [dotnetsay](https://www.nuget.org/packages/dotnetsay/) グローバル ツールを更新します。

- **`dotnet tool update dotnetsay --tool-path ~/bin`**

  特定の Linux/macOS ディレクトリに配置されている [dotnetsay](https://www.nuget.org/packages/dotnetsay/) グローバル ツールを更新します。

- **`dotnet tool update dotnetsay`**

  現在のディレクトリに対してインストールされている [dotnetsay](https://www.nuget.org/packages/dotnetsay/) ローカル ツールを更新します。

## <a name="see-also"></a>関連項目

- [.NET Core ツール](global-tools.md)
