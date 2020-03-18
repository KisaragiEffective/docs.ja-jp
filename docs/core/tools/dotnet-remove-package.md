---
title: dotnet remove package コマンド
description: dotnet remove package コマンドは、プロジェクトへの NuGet パッケージ参照を削除する便利なオプションを提供します。
ms.date: 02/14/2020
ms.openlocfilehash: 8eaa311748c5627351ef149012dc4dddd2ab2793
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77503639"
---
# <a name="dotnet-remove-package"></a>dotnet remove package

**この記事の対象:** ✔️ .NET Core 2.x SDK 以降のバージョン

## <a name="name"></a>name

`dotnet remove package` - プロジェクト ファイルからパッケージ参照を削除します。

## <a name="synopsis"></a>構文

```dotnetcli
dotnet remove [<PROJECT>] package <PACKAGE_NAME> [-h|--help]
```

## <a name="description"></a>[説明]

`dotnet remove package` コマンドは、NuGet パッケージ参照をプロジェクトから削除する便利なオプションを提供します。

## <a name="arguments"></a>引数

`PROJECT`

プロジェクト ファイルを指定します。 指定されていない場合、現在のディレクトリで検索されます。

`PACKAGE_NAME`

削除するパッケージ参照。

## <a name="options"></a>オプション

- **`-h|--help`**

  コマンドの短いヘルプを印刷します。

## <a name="examples"></a>例

- 現在のディレクトリにあるプロジェクトから `Newtonsoft.Json` NuGet パッケージを削除する:

  ```dotnetcli
  dotnet remove package Newtonsoft.Json
  ```
