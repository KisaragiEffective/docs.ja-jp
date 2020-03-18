---
title: 'チュートリアル: .NET Core ローカル ツールをインストールして使用する'
description: .NET ツールをローカル ツールとしてインストールして使用する方法について説明します。
ms.date: 02/12/2020
ms.openlocfilehash: a4355886513040e2436bdbd87905e5baee2dd7a5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78156700"
---
# <a name="tutorial-install-and-use-a-net-core-local-tool-using-the-net-core-cli"></a>チュートリアル: .NET Core CLI を使って .NET Core ローカル ツールをインストールして使用する

**この記事の対象:** ✔️ .NET Core 3.0 SDK 以降のバージョン

このチュートリアルでは、ローカル ツールをインストールして使用する方法について説明します。 [このシリーズの最初のチュートリアル](global-tools-how-to-create.md)で作成されるツールを使用します。

## <a name="prerequisites"></a>必須コンポーネント

* [このシリーズの最初のチュートリアル](global-tools-how-to-create.md)を完了します。
* .NET Core 2.1 ランタイムをインストールします。

  このチュートリアルでは、.NET Core 2.1 を対象とするツールをインストールして使用するため、お使いのコンピューター上に該当のランタイムがインストールされている必要があります。 2\.1 ランタイムをインストールするには、[.NET Core 2.1 のダウンロード ページ](https://dotnet.microsoft.com/download/dotnet-core/2.1)にアクセスして、 **[Run apps - Runtime]\(アプリの実行 - ランタイム\)** 列でランタイム インストールのリンクを見つけます。

## <a name="create-a-manifest-file"></a>マニフェスト ファイルの作成

ローカル アクセス専用のツール (現在のディレクトリとサブディレクトリ用) をインストールするには、マニフェスト ファイルに追加する必要があります。

*microsoft.botsay* フォルダーから、1 つ上のレベルである *repository* フォルダーに移動します。

```console
cd ..
```

[dotnet new](dotnet-new.md) コマンドを実行して、マニフェスト ファイルを作成します。

```dotnetcli
dotnet new tool-manifest
```

出力によって、ファイルが正常に作成されたことが示されます。

```console
The template "Dotnet local tool manifest file" was created successfully.
```

*.config/dotnet-tools.json* ファイル内には、まだツールはありません。

```json
{
  "version": 1,
  "isRoot": true,
  "tools": {}
}
```

マニフェスト ファイルに示されているツールは、現在のディレクトリとサブディレクトリで使用できます。 現在のディレクトリは、マニフェスト ファイルがある *.config* ディレクトリが含まれているディレクトリです。

ローカル ツールを参照する CLI コマンドを使用すると、SDK では現在のディレクトリおよび親ディレクトリ内でマニフェスト ファイルを検索します。 マニフェスト ファイルが見つかっても、参照されたツールがファイルに含まれていない場合は、親ディレクトリを遡って引き続き検索されます。 検索は、参照されたツールが見つかったとき、または `isRoot` が `true` に設定されているマニフェスト ファイルが見つかったときに終了します。

## <a name="install-botsay-as-a-local-tool"></a>ローカル ツールとして botsay をインストールする

最初のチュートリアルで作成したパッケージからツールをインストールします。

```dotnetcli
dotnet tool install --add-source ./microsoft.botsay/nupkg microsoft.botsay
```

このコマンドでは、前の手順で作成したマニフェスト ファイルにツールを追加します。 コマンドの出力は、新しくインストールされたツールがどのマニフェスト ファイルに含まれているかを示しています。

 ```console
 You can invoke the tool from this directory using the following command:
 'dotnet tool run botsay' or 'dotnet botsay'
 Tool 'microsoft.botsay' (version '1.0.0') was successfully installed.
 Entry is added to the manifest file /home/name/repository/.config/dotnet-tools.json
 ```

*.config/dotnet-tools.json* ファイルには現在、1 つのツールがあります。

```json
{
  "version": 1,
  "isRoot": true,
  "tools": {
    "microsoft.botsay": {
      "version": "1.0.0",
      "commands": [
        "botsay"
      ]
    }
  }
}
```

## <a name="use-the-tool"></a>ツールの使用

*repository* フォルダーから `dotnet tool run` コマンドを実行して、ツールを起動します。

```dotnetcli
dotnet tool run botsay hello from the bot
```

## <a name="restore-a-local-tool-installed-by-others"></a>他のユーザーによってインストールされたローカル ツールを復元する

通常は、リポジトリのルート ディレクトリにローカル ツールをインストールします。 マニフェスト ファイルをリポジトリにチェックインすると、他の開発者が最新のマニフェスト ファイルを取得できるようになります。 マニフェスト ファイルに一覧表示されたすべてのツールをインストールするには、単一の `dotnet tool restore` コマンドを実行できます。

1. *.config/dotnet-tools.json* ファイルを開き、内容を次の JSON に置き換えます。

   ```json
   {
     "version": 1,
     "isRoot": true,
     "tools": {
       "microsoft.botsay": {
         "version": "1.0.0",
         "commands": [
           "botsay"
         ]
       },
       "dotnetsay": {
         "version": "2.1.3",
         "commands": [
           "dotnetsay"
         ]
       }
     }
   }
   ```

1. `<name>` をプロジェクトの作成に使用した名前に置き換えます。

1. 変更内容を保存します。

   この変更を行うことは、他のユーザーがプロジェクト ディレクトリに対してパッケージ `dotnetsay` をインストールした後に、リポジトリから最新バージョンを取得することと同じです。

1. `dotnet tool restore` コマンドを実行します。

   ```dotnetcli
   dotnet tool restore
   ```

   コマンドによって、次の例のような出力が生成されます。

   ```console
   Tool 'microsoft.botsay' (version '1.0.0') was restored. Available commands: botsay
   Tool 'dotnetsay' (version '2.1.3') was restored. Available commands: dotnetsay
   Restore was successful.
   ```

1. ツールが使用可能であることを確認します。

   ```dotnetcli
   dotnet tool list
   ```

   次の例のように、出力はパッケージとコマンドの一覧になります。

   ```console
   Package Id      Version      Commands       Manifest
   --------------------------------------------------------------------------------------------
   microsoft.botsay 1.0.0        botsay         /home/name/repository/.config/dotnet-tools.json
   dotnetsay        2.1.3        dotnetsay      /home/name/repository/.config/dotnet-tools.json
   ```

1. ツールをテストします。

   ```dotnetcli
   dotnet tool run dotnetsay hello from dotnetsay
   dotnet tool run botsay hello from botsay
   ```

## <a name="update-a-local-tool"></a>ローカル ツールを更新する

インストールされているローカルツール `dotnetsay` のバージョンは 2.1.3 です。  最新バージョンは 2.1.4 です。 [dotnet tool update](dotnet-tool-update.md) コマンドを使用して、ツールを最新バージョンに更新します。

```dotnetcli
dotnet tool update dotnetsay
```

出力には、新しいバージョン番号が示されます。

```console
Tool 'dotnetsay' was successfully updated from version '2.1.3' to version '2.1.4'
(manifest file /home/name/repository/.config/dotnet-tools.json).
```

update コマンドでは、パッケージ ID を含む最初のマニフェスト ファイルを検索して、それを更新します。 検索のスコープ内にあるマニフェスト ファイルにこのようなパッケージ ID がない場合、SDK では最も近いマニフェスト ファイルに新しいエントリを追加します。 `isRoot = true` のマニフェスト ファイルが見つかるまで、検索のスコープとして親ディレクトリを遡ります。

## <a name="remove-local-tools"></a>ローカル ツールを削除する

[dotnet tool uninstall](dotnet-tool-uninstall.md) コマンドを実行して、インストールされたツールを削除します。

```dotnetcli
dotnet tool uninstall microsoft.botsay
```

```dotnetcli
dotnet tool uninstall dotnetsay
```

## <a name="troubleshoot"></a>トラブルシューティング

チュートリアルの実行中にエラー メッセージが表示された場合は、「[.NET Core ツールの使用に関する問題のトラブルシューティング](troubleshoot-usage-issues.md)」を参照してください。

## <a name="see-also"></a>関連項目

詳細については、[.NET Core ツール](global-tools.md)に関するページを参照してください。
