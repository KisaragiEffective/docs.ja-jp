---
title: dotnet tool run コマンド
description: dotnet tool run コマンドでは、ローカル ツールを起動します。
ms.date: 02/14/2020
ms.openlocfilehash: 76830b8a8088fbf21f14ab0722b9547eabde7ba4
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78156960"
---
# <a name="dotnet-tool-run"></a>dotnet tool run

**この記事の対象:** ✔️ .NET Core 3.0 SDK 以降のバージョン

## <a name="name"></a>名前

`dotnet tool run` - ローカル ツールを起動します。

## <a name="synopsis"></a>構文

```dotnetcli
dotnet tool run <COMMAND NAME>
dotnet tool run <-h|--help>
```

## <a name="description"></a>説明

`dotnet tool run` コマンドでは、現在のディレクトリのスコープ内にあるツール マニフェスト ファイルを検索します。 指定されたツールへの参照が見つかると、そのツールが実行されます。 詳細については、「[ローカル ツールの起動](global-tools.md#invoke-a-local-tool)」を参照してください。

## <a name="arguments"></a>引数

- **`COMMAND_NAME`**

  実行するツールのコマンド名。

## <a name="options"></a>オプション

- **`-h|--help`**

  コマンドの短いヘルプを印刷します。

## <a name="example"></a>例

- **`dotnet tool run dotnetsay`**

  `dotnetsay` ローカル ツールを実行します。

## <a name="see-also"></a>関連項目

- [.NET Core ツール](global-tools.md)
