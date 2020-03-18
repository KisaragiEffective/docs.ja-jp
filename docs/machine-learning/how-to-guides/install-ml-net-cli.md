---
title: ML.NET コマンドライン インターフェイス (CLI) ツールをインストールする方法
description: ML.NET コマンド ライン インターフェイス (CLI) ツールをインストール、アップグレード、ダウングレード、およびアンインストールする方法を説明します。
ms.date: 12/18/2019
ms.custom: mlnet-tooling
ms.openlocfilehash: 9f678c7117d32bf817139951db7eef2c3d0f5eb2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78848640"
---
# <a name="how-to-install-the-mlnet-command-line-interface-cli-tool"></a>ML.NET コマンドライン インターフェイス (CLI) ツールをインストールする方法

Windows、Mac、または Linux で ML.NET CLI (コマンド ライン インターフェイス) ツールをインストールする方法を説明します。

ML.NET CLI では、自動機械学習 (AutoML) とトレーニング データセットを使用して、高品質 ML.NET モデルとソース コードが生成されます。

> [!NOTE]
> このトピックは、現在プレビュー段階の ML.NET CLI と ML.NET AutoML について述べており、内容が変更される場合があります。

## <a name="pre-requisites"></a>前提条件

- [.NET Core 2.2 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.2)

- (省略可能) [Visual Studio 2017 または 2019](https://visualstudio.microsoft.com/vs/)

生成された C# コード プロジェクトを Visual Studio で実行するには、`F5` キーを押すか、`dotnet run` (.NET Core CLI) を使用します。

メモ:[.NET Core 2.2 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.2) のインストール後に `dotnet tool` コマンドが機能しない場合は、Windows からサインアウトしてから再度サインインします。

## <a name="install"></a>インストール

ML.NET CLI のインストール方法は他の .NET グローバル ツールと同様です。 `dotnet tool install` .NET Core CLI コマンドを使用します。

次の例は、既定の NuGet フィードの場所に ML.NET CLI をインストールする方法を示しています。

```dotnetcli
dotnet tool install -g mlnet
```

ツールをインストールできない場合 (つまり、既定の NuGet フィードで利用できない場合) は、エラー メッセージが表示されます。 予定していたフィードがチェックされていることを確認します。

インストールが成功すると、ツールの呼び出しに使用したコマンドとインストールされたバージョンを示す、次の例のようなメッセージが表示されます。

```console
You can invoke the tool using the following command: mlnet
Tool 'mlnet' (version 'X.X.X') was successfully installed.
```

インストールが成功したことを確認するには、次のコマンドを入力します。

```console
mlnet
```

"auto-train" コマンドなど、mlnet ツールに利用できるコマンドのヘルプを参照することをお勧めします。

## <a name="install-a-specific-release-version"></a>特定のリリース バージョンをインストールする

ツールのプレリリース バージョンまたは特定のバージョンをインストールしようとしている場合は、次の形式を使用して、[フレームワーク](../../standard/frameworks.md)を指定できます。

```dotnetcli
dotnet tool install -g mlnet --framework <FRAMEWORK>
```

次のコマンドを入力して、パッケージが正しくインストールされているかどうかを確認することもできます。

```dotnetcli
dotnet tool list -g
```

## <a name="uninstall-the-cli-package"></a>CLI パッケージをアンインストールする

ローカル コンピューターからパッケージをアンインストールするには、次のコマンドを入力します。

```dotnetcli
dotnet tool uninstall mlnet -g
```

## <a name="update-the-cli-package"></a>CLI パッケージを更新する

ローカル コンピューターからパッケージを更新するには、次のコマンドを入力します。

```dotnetcli
dotnet tool update -g mlnet
```

## <a name="set-up-cli-suggestions-tab-based-auto-completion"></a>CLI の候補を設定する (タブベースのオートコンプリート)

ML.NET CLI は `System.CommandLine` に基づいているので、タブ補完のサポートが組み込まれています。

次のアニメーションでタブのオートコンプリートのしくみの例を示します。

![イメージ](./media/cli-tab-completion.gif)

"タブベースのオートコンプリート" (パラメーターの候補) は *Windows PowerShell* と *macOS/Linux bash* では機能しますが、*Windows CMD* では機能しません。

これを有効にするには、現在のプレビュー バージョンでは、エンド ユーザーが、シェルごとに 1 回、以下に示すいくつかの手順を実行する必要があります。 これが完了すると、ML.NET CLI などの `System.CommandLine` を使用して作成されるすべてのアプリに対して補完が機能するようになります。

補完を有効にするマシンでは、2 つのことを実行する必要があります。

1. 次のコマンドを実行して、`dotnet-suggest` グローバル ツールをインストールします。

    ```dotnetcli
    dotnet tool install dotnet-suggest -g
    ```

2. 適切な shim スクリプトをシェル プロファイルに追加します。 必要に応じてシェル プロファイル ファイルを作成します。 shim スクリプトによって、シェルからの完了要求が `dotnet-suggest` ツールに転送され、適切な `System.CommandLine` ベースのアプリに委任されます。

    - bash の場合、[dotnet-suggest-shim.bash](https://github.com/dotnet/System.CommandLine/blob/master/src/System.CommandLine.Suggest/dotnet-suggest-shim.bash) の内容を `~/.bash_profile` に追加します。

    - PowerShell の場合は、[dotnet-recommend-shim.ps1](https://github.com/dotnet/System.CommandLine/blob/master/src/System.CommandLine.Suggest/dotnet-suggest-shim.ps1) の内容を PowerShell プロファイルに追加します。 コンソールで次のコマンドを実行して、PowerShell プロファイルへの予想されるパスを見つけることができます。

    ```console
    echo $profile
    ```

(他のシェルについては、[検索する](https://github.com/dotnet/System.CommandLine/issues?q=is%3Aissue+is%3Aopen+label%3A%22shell+suggestion%22)か、[問題](https://github.com/dotnet/System.CommandLine/issues)を開いてください。)

## <a name="installation-directory"></a>インストール ディレクトリ

ML.NET CLI は、既定のディレクトリまたは特定の場所にインストールできます。 既定のディレクトリは次のとおりです。

| OS          | パス                          |
|-------------|-------------------------------|
| Linux/macOS | `$HOME/.dotnet/tools`         |
| Windows     | `%USERPROFILE%\.dotnet\tools` |

これらの場所は、そこにインストールされたグローバル ツールを直接呼び出せるように、SDK が最初に実行されるときにユーザーのパスに追加されます。

注: グローバル ツールは、マシン全体ではなく、ユーザー固有のものです。 ユーザーに固有であることは、そのマシンのすべてのユーザーが使用できるグローバル ツールをインストールできないことを意味します。 このツールは、ツールがインストールされた各ユーザー プロファイルでしか使用できません。

グローバル ツールは、特定のディレクトリにインストールすることもできます。 特定のディレクトリにインストールした場合は、ユーザーはコマンドが使用できることを確認する必要があります。それには、そのディレクトリをパスに含めるか、指定されたディレクトリでコマンドを呼び出すか、指定したディレクトリ内からツールを呼び出します。
この場合、.NET Core CLI によりこの場所が PATH 環境変数に自動的に追加されることはありません。

## <a name="see-also"></a>関連項目

- [ML.NET CLI の概要](../automate-training-with-cli.md)
- [チュートリアル: ML.NET CLI を使用してセンチメントを分析する](../tutorials/sentiment-analysis-cli.md)
- [ML.NET CLI auto-train コマンド リファレンス ガイド](../reference/ml-net-cli-reference.md)
- [ML.NET CLI のテレメトリ](../resources/ml-net-cli-telemetry.md)
