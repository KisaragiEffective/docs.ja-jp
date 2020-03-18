---
title: dotnet sln コマンド
description: dotnet-sln コマンドは、ソリューション ファイルでプロジェクトを追加、削除、一覧表示するための便利なオプションを提供します。
ms.date: 02/14/2020
ms.openlocfilehash: b2455c04a46b2a10b8142d8ddc2d8129f2154b27
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77543483"
---
# <a name="dotnet-sln"></a>dotnet sln

**この記事の対象:** ✔️ .NET Core 2.x SDK 以降のバージョン

## <a name="name"></a>name

`dotnet sln` - .NET Core ソリューション ファイル内のプロジェクトを一覧表示または変更します。

## <a name="synopsis"></a>構文

```dotnetcli
dotnet sln [<SOLUTION_FILE>] [command] [-h|--help]
```

## <a name="description"></a>[説明]

`dotnet sln` コマンドでは、ソリューション ファイル内のプロジェクトを一覧表示および変更するための便利な方法を提供します。

`dotnet sln` コマンドを使用するには、ソリューション ファイルが既に存在している必要があります。 作成する必要がある場合、以下の例のように [dotnet new](dotnet-new.md) コマンドを使用します。

```dotnetcli
dotnet new sln
```

## <a name="arguments"></a>引数

- **`SOLUTION_FILE`**

  使用するソリューション ファイル。 この引数が省略された場合、コマンドでは、現在のディレクトリでの検索を行います。 ソリューション ファイルが見つからない場合、または複数のソリューション ファイルが見つかった場合、コマンドは失敗します。

## <a name="options"></a>オプション

- **`-h|--help`**

  コマンドの使用方法を示した説明を出力します。

## <a name="commands"></a>コマンド

### `list`

ソリューション ファイルのすべてのプロジェクトを一覧表示します。

#### <a name="synopsis"></a>構文

```dotnetcli
dotnet sln list [-h|--help]
```

#### <a name="arguments"></a>引数

- **`SOLUTION_FILE`**

  使用するソリューション ファイル。 この引数が省略された場合、コマンドでは、現在のディレクトリでの検索を行います。 ソリューション ファイルが見つからない場合、または複数のソリューション ファイルが見つかった場合、コマンドは失敗します。

#### <a name="options"></a>オプション

- **`-h|--help`**

  コマンドの使用方法を示した説明を出力します。
  
### `add`

1 つ以上のプロジェクトをソリューション ファイルに追加します。

#### <a name="synopsis"></a>構文

```dotnetcli
dotnet sln [<SOLUTION_FILE>] add [--in-root] [-s|--solution-folder] <PROJECT_PATH> [<PROJECT_PATH>...]
dotnet sln add [-h|--help]
```

#### <a name="arguments"></a>引数

- **`SOLUTION_FILE`**

  使用するソリューション ファイル。 指定されていない場合、コマンドでは、現在のディレクトリでの検索を行います。複数のソリューション ファイルがある場合、コマンドは失敗します。

- **`PROJECT_PATH`**

  ソリューションに追加する 1 つ以上のプロジェクトへのパス。 Unix/Linux シェルの[glob パターン](https://en.wikipedia.org/wiki/Glob_(programming))拡張機能は、`dotnet sln`コマンドで正しく処理されます。

#### <a name="options"></a>オプション

- **`-h|--help`**

  コマンドの使用方法を示した説明を出力します。

- **`--in-root`**

  ソリューション フォルダーを作成するのではなく、プロジェクトをソリューションのルートに配置します。 .NET Core 3.0 SDK 以降で使用できます。

- **`-s|--solution-folder`**

  プロジェクトの追加先のソリューション フォルダーのパス。 .NET Core 3.0 SDK 以降で使用できます。

### `remove`

ソリューション ファイルから 1 つまたは複数のプロジェクトを削除します。

#### <a name="synopsis"></a>構文

```dotnetcli
dotnet sln [<SOLUTION_FILE>] remove <PROJECT_PATH> [<PROJECT_PATH>...]
dotnet sln [<SOLUTION_FILE>] remove [-h|--help]
```

#### <a name="arguments"></a>引数

- **`SOLUTION_FILE`**

  使用するソリューション ファイル。 指定されていない場合、コマンドでは、現在のディレクトリでの検索を行います。複数のソリューション ファイルがある場合、コマンドは失敗します。

- **`PROJECT_PATH`**

  ソリューションに追加する 1 つ以上のプロジェクトへのパス。 Unix/Linux シェルの[glob パターン](https://en.wikipedia.org/wiki/Glob_(programming))拡張機能は、`dotnet sln`コマンドで正しく処理されます。

#### <a name="options"></a>オプション

- **`-h|--help`**

  コマンドの使用方法を示した説明を出力します。

## <a name="examples"></a>例

- ソリューション内のプロジェクトを一覧表示する:

  ```dotnetcli
  dotnet sln todo.sln list
  ```

- ソリューションに 1 つの C# プロジェクトを追加する:

  ```dotnetcli
  dotnet sln add todo-app/todo-app.csproj
  ```

- ソリューションから 1 つの C# プロジェクトを削除する:

  ```dotnetcli
  dotnet sln remove todo-app/todo-app.csproj
  ```

- ソリューションのルートに複数の C# プロジェクトを追加する:

  ```dotnetcli
  dotnet sln todo.sln add todo-app/todo-app.csproj back-end/back-end.csproj --in-root
  ```

- ソリューションに複数の C# プロジェクトを追加する:

  ```dotnetcli
  dotnet sln todo.sln add todo-app/todo-app.csproj back-end/back-end.csproj
  ```

- ソリューションから複数の C# プロジェクトを削除する:

  ```dotnetcli
  dotnet sln todo.sln remove todo-app/todo-app.csproj back-end/back-end.csproj
  ```

- glob パターンを使用して、ソリューションに複数の C# プロジェクトを追加する (Unix/Linux のみ):

  ```dotnetcli
  dotnet sln todo.sln add **/*.csproj
  ```

- glob パターンを使用して、ソリューションから複数の C# プロジェクトを削除する (Unix/Linux のみ):

  ```dotnetcli
  dotnet sln todo.sln remove **/*.csproj
  ```
