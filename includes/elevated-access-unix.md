---
ms.openlocfilehash: 85f50b221e7ecb1ebd6fa539894ab7aabed8d362
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "67540111"
---
### <a name="install-the-global-tool"></a>グローバル ツールをインストールする

パッケージ アセットは `--tool-path` オプションを使用して保護された場所にインストールする必要があります。 このように分けることにより、制限付きユーザー環境が特権環境と共有されることを回避できす。

```bash
sudo dotnet tool install PACKAGEID --tool-path /usr/local/share/dotnet-tools
```

`/usr/local/share/dotnet-tools` は `drwxr-xr-x` アクセス許可で作成されます。 ディレクトリが既に存在する場合は、`ls -l` コマンドを使用して、制限付きユーザーにディレクトリを編集するアクセス許可がないことを確認します。 ある場合は、`sudo chmod o-w -R /usr/share/dotnet-tools` コマンドを使用してそのアクセス許可を削除します。

### <a name="run-the-global-tool"></a>グローバル ツールを実行する

**オプション 1** sudo で完全なパスを使用します。

```bash
sudo /usr/local/share/dotnet-tools/TOOLCOMMAND
```

**オプション 2** ツールごとに 1 回、ツールのシンボルのリンクを追加します。

```bash
sudo ln -s /usr/local/share/dotnet-tools/TOOLCOMMAND /usr/local/bin/TOOLCOMMAND
```

次で実行します。

```bash
sudo TOOLCOMMAND
```

### <a name="uninstall-the-global-tool"></a>グローバル ツールをアンインストールする

```bash
sudo dotnet tool uninstall PACKAGEID --tool-path /usr/local/share/dotnet-tools
```

シンボルのリンクを作成した場合は、それも削除する必要があります。

```bash
sudo rm /usr/local/bin/TOOLCOMMAND
```
