---
title: dotnet nuget verify コマンド
description: dotnet nuget verify コマンドによって、署名されたパッケージが検証されます。
author: kartheekp-ms
ms.date: 10/08/2020
ms.openlocfilehash: 1c300e5a09b4049a9895b9b3f6c742f701dc2200
ms.sourcegitcommit: 985c603cb21a085f8a8105f34ff5b87a44b76ab4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2021
ms.locfileid: "107564845"
---
# <a name="dotnet-nuget-verify"></a>dotnet nuget verify

**この記事の対象:** ✔️ .NET 5.0.100-rc.2.x SDK 以降のバージョン

## <a name="name"></a>名前

`dotnet nuget verify` - 署名済みの NuGet パッケージを検証します。

## <a name="synopsis"></a>構文

```dotnetcli
dotnet nuget verify [<package-path(s)>]
    [--all]
    [--certificate-fingerprint <FINGERPRINT>]
    [-v|--verbosity <LEVEL>]

dotnet nuget verify -h|--help
```

## <a name="description"></a>説明

`dotnet nuget verify` コマンドによって、署名済みの NuGet パッケージが検証されます。

## <a name="arguments"></a>引数

- **`package-path(s)`**

  検証されるパッケージのファイル パスを指定します。 複数のパッケージを検証するために、複数の位置引数を渡すことができます。

## <a name="options"></a>オプション

- **`--all`**

  可能なすべての検証をパッケージに対して実行する必要があることを指定します。 既定では、`signatures` のみ検証されます。

> [!NOTE]
> このコマンドで現在サポートされているのは、`signature` 検証のみとなります。

- **`--certificate-fingerprint <FINGERPRINT>`**

  署名者の証明書が、指定されたいずれかの `SHA256` 指紋と一致していることを検証します。 このオプションを複数回指定することで複数のフィンガープリントを提供できます。

* **`-v|--verbosity <LEVEL>`**

  [MSBuild の詳細レベル](/visualstudio/msbuild/obtaining-build-logs-with-msbuild#verbosity-settings)を設定します。 指定できる値は、`q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]`、および `diag[nostic]` です。 既定値は、`minimal` です。

    次の表は、各詳細レベルに表示される内容を示しています。

                                      | `q[uiet]` | `m[inimal]` | `n[ormal]` | `d[etailed]` | `diag[nostic]`
    ----------------------------------| --------- | ----------- | ---------- | -----------| --------------
    `Certificate chain Information`   | ❌       | ❌          | ❌         | ✔️         | ✔️
    `Path to package being verified`  | ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Hashing algorithm used for signature`        | ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Author/Repository Certificate -> SHA1 hash`| ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Author/Repository Certificate -> Issued By`| ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Timestamp Certificate -> Issued By`| ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Timestamp Certificate -> SHA-256 hash`| ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Timestamp Certificate -> Validity period`| ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Timestamp Certificate -> SHA1 hash`| ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Timestamp Certificate -> Subject name`| ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Author/Repository Certificate -> Subject name`| ❌       | ✔️          | ✔️         | ✔️         | ✔️
    `Author/Repository Certificate -> SHA-256 hash`| ❌       | ✔️          | ✔️         | ✔️         | ✔️
    `Author/Repository Certificate -> Validity period`| ❌       | ✔️          | ✔️         | ✔️         | ✔️
    `Author/Repository Certificate -> Service index URL (If applicable)`| ❌       | ✔️          | ✔️         | ✔️         | ✔️
    `Package name being verified`                    | ❌       | ✔️          | ✔️         | ✔️         | ✔️
    `Type of signature (author or repository)`| ❌       | ✔️          | ✔️         | ✔️         | ✔️

    ❌は、表示 "**されない**" 詳細を示します。 ✔️は、表示される詳細を示します。

* **`-h|--help`**

  コマンドの短いヘルプを印刷します。

## <a name="examples"></a>使用例

- *foo.nupkg* を検証します。

  ```dotnetcli
  dotnet nuget verify foo.nupkg
  ```

- 複数の NuGet パッケージを検証します - *foo. nupkg* および "*指定されたディレクトリ内のすべての .nupkg ファイル*"。

  ```dotnetcli
  dotnet nuget verify foo.nupkg c:\mydir\*.nupkg
  ```

- *foo. nupkg* の署名が、指定された証明書のフィンガープリントと一致していることを検証します。

  ```dotnetcli
  dotnet nuget verify foo.nupkg --certificate-fingerprint CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039
  ```

- *foo. nupkg* の署名が、指定された証明書のフィンガープリントのいずれかと一致していることを検証します。

  ```dotnetcli
  dotnet nuget verify foo.nupkg --certificate-fingerprint CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039 --certificate-fingerprint EC10992GG5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E027
  ```
