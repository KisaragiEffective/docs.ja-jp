---
title: dotnet-symbol 診断ツール - .NET CLI
description: dotnet-symbol CLI ツールをインストールして使用し、.NET ダンプとミニダンプのデバッグに必要なファイルをダウンロードする方法について学習します。
ms.date: 11/17/2020
ms.topic: reference
ms.openlocfilehash: 95f806ea0cc5d1c7dc552418adb054c34e92fc66
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740775"
---
# <a name="symbol-downloader-dotnet-symbol"></a>シンボル ダウンローダー (dotnet-symbol)

**この記事の対象:** ✔️ .NET Core 2.1 SDK 以降のバージョン

## <a name="install"></a>インストール

`dotnet-symbol` [NuGet パッケージ](https://www.nuget.org/packages/dotnet-symbol)の最新のリリース バージョンをインストールするには、次のように [dotnet tool install](../tools/dotnet-tool-install.md) コマンドを使用します。

```dotnetcli
dotnet tool install --global dotnet-symbol
```

## <a name="synopsis"></a>構文

```console
dotnet-symbol [-h|--help] [options] <FILES>
```

## <a name="description"></a>説明

`dotnet-symbol` グローバル ツールによって、コア ダンプとミニダンプのデバッグに必要なファイル (シンボル、DAC、モジュールなど) をダウンロードできます。 これは、別のコンピューターでキャプチャされたダンプをデバッグするときに便利です。 `dotnet-symbol` を使用すると、ダンプを分析するために必要なモジュールとシンボルをダウンロードできます。

## <a name="options"></a>オプション

- **`--microsoft-symbol-server`**

  'http://msdl.microsoft.com/download/symbols ' シンボル サーバー パス (既定) を追加します。

- **`--server-path <symbol server path>`**

  サーバー パスにシンボル サーバーを追加します。

- **`authenticated-server-path <pat> <server path>`**

  個人用アクセス トークン (PAT) を使用して、認証済みのシンボル サーバーをサーバー パスに追加します。

- **`--cache-directory <file cache directory>`**

  キャッシュ ディレクトリを追加します。

- **`--recurse-subdirectories`**

  すべてのサブディレクトリの入力ファイルを処理します。

- **`--host-only`**

  lldb でコア ダンプを読み込むために必要なホスト プログラム (つまり dotnet) のみをダウンロードします。

- **`--symbols`**

  シンボル ファイル (.pdb、.dbg、.dwarf) をダウンロードします。

- **`--modules`**

  モジュール ファイル (.dll、.so、.dylib) をダウンロードします。

- **`--debugging`**

  特別なデバッグ モジュール (DAC、DBI、SOS) をダウンロードします。

- **`--windows-pdbs`**

  ポータブル PDB も使用できる場合は、Windows PDB を強制的にダウンロードします。

- **`-o, --output <output directory>`**

  出力ディレクトリを設定します。 それ以外の場合は、入力ファイルの次に書き込みます (既定)。

- **`-d, --diagnostics`**

  診断出力を有効にします。

- **`-h|--help`**

  コマンド ライン ヘルプを表示します。

## <a name="download-symbols"></a>シンボルをダウンロードする

ダンプ ファイルに対して `dotnet-symbol` を実行すると、既定では、マネージド アセンブリを含むダンプのデバッグに必要なすべてのモジュール、シンボル、および DAC (または DBI) ファイルがダウンロードされます。 SOS で必要に応じてシンボルをダウンロードできるようになったため、ほとんどの Linux コア ダンプは、ホスト (dotnet) モジュールとデバッグ モジュールのみを含む lldb を使用して分析できます。 lldb を使用してコア ダンプを診断するために必要なこれらのファイルを取得するには、次のように実行します。

```console
dotnet-symbol --host-only --debugging <dump file path>
```

## <a name="troubleshoot"></a>トラブルシューティング

- シンボルのダウンロード中の 404 Not Found。

   シンボルのダウンロードは、[公式 Web サイト](https://dotnet.microsoft.com/download/dotnet)などの公式チャネルを通じて取得された公式の .NET Core ランタイム バージョンと、[dotnet インストール スクリプト内の既定のソース](../tools/dotnet-install-script.md)でのみサポートされています。 デバッグ ファイルのダウンロード中に 404 エラーが発生した場合は、ダンプが別のソースの .NET Core ランタイムを使って作成されたことが示されている可能性があります。たとえば、ソースからローカルに構築されたものや、特定の Linux ディストリビューション用のもの、または archlinux のようなコミュニティ サイトから作成されたものです。 このような場合は、デバッグに必要なファイル (dotnet、libcoreclr.so、libmscordaccore.so) を、それらのソースから、またはダンプ ファイルが作成された環境からコピーする必要があります。

## <a name="see-also"></a>関連項目

* [シンボルでデバッグする](/windows/win32/dxtecharts/debugging-with-symbols)
* [シンボルとポータブル PDB](./symbols.md)
