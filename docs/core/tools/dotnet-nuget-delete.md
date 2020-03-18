---
title: dotnet nuget delete コマンド
description: dotnet-nuget-delete コマンドは、サーバーからパッケージを削除または一覧から削除します。
author: karann-msft
ms.date: 06/26/2019
ms.openlocfilehash: 0950f03c0986bde17ae3e2e7170d402ea8222853
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76733122"
---
# <a name="dotnet-nuget-delete"></a>dotnet nuget delete

**この記事の対象:** ✔️ .NET Core 1.x SDK 以降のバージョン

<!-- todo: uncomment when all CLI commands are reviewed
[!INCLUDE [topic-appliesto-net-core-all](../../../includes/topic-appliesto-net-core-all.md)]
-->

## <a name="name"></a>name

`dotnet nuget delete` - サーバーからパッケージを削除または一覧から削除します。

## <a name="synopsis"></a>構文

```dotnetcli
dotnet nuget delete [<PACKAGE_NAME> <PACKAGE_VERSION>] [--force-english-output] [--interactive] [-k|--api-key] [--no-service-endpoint]
    [--non-interactive] [-s|--source]
dotnet nuget delete [-h|--help]
```

## <a name="description"></a>[説明]

`dotnet nuget delete` コマンドは、サーバーからパッケージを削除または一覧から削除します。 [nuget.org](https://www.nuget.org/) の場合、この操作ではパッケージを一覧から削除します。

## <a name="arguments"></a>引数

* **`PACKAGE_NAME`**

  削除するパッケージの名前または ID です。

* **`PACKAGE_VERSION`**

  削除するパッケージのバージョンです。

## <a name="options"></a>オプション

* **`--force-english-output`**

  インバリアントの英語ベースのカルチャを使用して、アプリケーションの実行を強制します。

* **`-h|--help`**

  コマンドの短いヘルプを印刷します。

* **`--interactive`**

  コマンドが認証などの操作をブロックして、手動アクションを要求することを許可します。 .NET Core 2.2 SDK 以降、使用できるオプションです。

* **`-k|--api-key <API_KEY>`**

  サーバーの API キーです。

* **`--no-service-endpoint`**

  ソース URL に "api/v2/package" を追加しません。 .NET Core 2.1 SDK 以降、使用できるオプションです。

* **`--non-interactive`**

  ユーザーに入力や確認のメッセージ画面を表示しません。

* **`-s|--source <SOURCE>`**

  サーバー URL を指定します。 nuget.org でサポートされている URL には、`https://www.nuget.org`、`https://www.nuget.org/api/v3`、および `https://www.nuget.org/api/v2/package` が含まれます。 プライベート フィードの場合、ホスト名を置き換えます (`%hostname%/api/v3` など)。

## <a name="examples"></a>例

* `Microsoft.AspNetCore.Mvc` パッケージのバージョン 1.0 を削除します。

  ```dotnetcli
  dotnet nuget delete Microsoft.AspNetCore.Mvc 1.0
  ```

* ユーザーに資格情報やその他の入力を求めずに、`Microsoft.AspNetCore.Mvc` パッケージのバージョン 1.0 を削除します。

  ```dotnetcli
  dotnet nuget delete Microsoft.AspNetCore.Mvc 1.0 --non-interactive
  ```
