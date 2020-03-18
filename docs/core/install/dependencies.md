---
title: .NET Core SDK とランタイムの依存関係 - .NET Core
description: Windows、Linux、および macOS に .NET Core SDK とランタイムをインストールするためのオペレーティング システムと CPU アーキテクチャの前提条件について説明します。
author: leecow
ms.author: leecow
ms.date: 12/04/2019
zone_pivot_groups: operating-systems-set-one
ms.openlocfilehash: ca86b3c158bb38c1293cd4303dcf4c00ea9175b1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78157812"
---
# <a name="net-core-dependencies-and-requirements"></a>.NET Core の依存関係と要件

この記事では、.NET Core でサポートされているオペレーティング システムと CPU アーキテクチャについて説明します。

## <a name="supported-operating-systems"></a>サポートされるオペレーティング システム

::: zone pivot="os-windows"

<!-- markdownlint-disable MD025 -->
<!-- markdownlint-disable MD024 -->

# <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

.NET Core 3.1 では以下の Windows のバージョンがサポートされます。

> [!NOTE]
> `+` 記号は、最小バージョンを表します。

| OS                            | バージョン                        | アーキテクチャ   |
| ----------------------------- | ------------------------------ | --------------- |
| Windows クライアント                | 7 SP1+、8.1                    | x64、x86        |
| Windows 10 クライアント             | バージョン 1607+                  | x64、x86        |
| Windows Server                | 2012 R2+                       | x64、x86        |
| Nano Server                   | バージョン 1803+                  | x64、ARM32      |

.NET Core 3.1 でサポートされているオペレーティング システム、ディストリビューション、ライフサイクル ポリシーの詳細については、「[.NET Core 3.1 Supported OS Versions](https://github.com/dotnet/core/blob/master/release-notes/3.1/3.1-supported-os.md)」(.NET Core 3.1 でサポートされている OS バージョン) を参照してください。

# <a name="net-core-30"></a>[.NET Core 3.0](#tab/netcore30)

.NET Core 3.0 では以下の Windows のバージョンがサポートされます。

> [!NOTE]
> `+` 記号は、最小バージョンを表します。

| OS                            | バージョン                        | アーキテクチャ   |
| ----------------------------- | ------------------------------ | --------------- |
| Windows クライアント                | 7 SP1+、8.1                    | x64、x86        |
| Windows 10 クライアント             | バージョン 1607+                  | x64、x86        |
| Windows Server                | 2012 R2+                       | x64、x86        |
| Nano Server                   | バージョン 1803+                  | x64、ARM32      |

.NET Core 3.0 でサポートされているオペレーティング システム、ディストリビューション、ライフサイクル ポリシーの詳細については、「[.NET Core 3.0 Supported OS Versions](https://github.com/dotnet/core/blob/master/release-notes/3.0/3.0-supported-os.md)」(.NET Core 3.0 でサポートされている OS バージョン) を参照してください。

# <a name="net-core-22"></a>[.NET Core 2.2](#tab/netcore22)

.NET Core 2.2 では以下の Windows のバージョンがサポートされます。

> [!NOTE]
> `+` 記号は、最小バージョンを表します。

| OS                            | バージョン                        | アーキテクチャ   |
| ----------------------------- | ------------------------------ | --------------- |
| Windows クライアント                | 7 SP1+、8.1                    | x64、x86        |
| Windows 10 クライアント             | バージョン 1607+                  | x64、x86        |
| Windows Server                | 2008 R2 SP1+                   | x64、x86        |
| Nano Server                   | バージョン 1803+                   | x64、ARM32      |

.NET Core 2.2 でサポートされているオペレーティング システム、ディストリビューション、ライフサイクル ポリシーの詳細については、「[.NET Core 2.2 Supported OS Versions](https://github.com/dotnet/core/blob/master/release-notes/2.2/2.2-supported-os.md)」(.NET Core 2.2 でサポートされている OS バージョン) を参照してください。

# <a name="net-core-21"></a>[.NET Core 2.1](#tab/netcore21)

.NET Core 2.1 では以下の Windows のバージョンがサポートされます。

> [!NOTE]
> `+` 記号は、最小バージョンを表します。

| OS                            | バージョン                        | アーキテクチャ   |
| ----------------------------- | ------------------------------ | --------------- |
| Windows クライアント                | 7 SP1+、8.1                    | x64、x86        |
| Windows 10 クライアント             | バージョン 1607+                  | x64、x86        |
| Windows Server                | 2008 R2 SP1+                   | x64、x86        |
| Nano Server                   | バージョン 1803+                  | x64、            |

.NET Core 2.1 でサポートされているオペレーティング システム、ディストリビューション、ライフサイクル ポリシーの詳細については、「[.NET Core 2.1 Supported OS Versions](https://github.com/dotnet/core/blob/master/release-notes/2.1/2.1-supported-os.md)」(.NET Core 2.1 でサポートされている OS バージョン) を参照してください。

---

<!-- markdownlint-disable MD001 -->

### <a name="windows-7--vista--81--server-2008-r2"></a>Windows 7 / Vista / 8.1 / Server 2008 R2

次の Windows のバージョンに .NET SDK またはランタイムをインストールする場合は、追加の依存関係が必要です。

- Windows 7 SP1
- Windows Vista SP 2
- Windows 8.1
- Windows Server 2008 R2
- Windows Server 2012 R2

以下をインストールします。

- [Microsoft Visual C++ 2015 再頒布可能パッケージ Update 3](https://www.microsoft.com/download/details.aspx?id=52685)。
- [KB2533623](https://support.microsoft.com/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)

上記の要件は、次のいずれかのエラーが発生した場合にも必要です。

> お使いのコンピューターに *api-ms-win-crt-runtime-l1-1-0.dll* が見つからず、プログラムを開始できない。 この問題を解決するには、プログラムを再インストールしてください。
>
> \- または
>
> ライブラリ *hostfxr.dll* はあるが、それを *C:\\\<path_to_app>\\hostfxr.dll* から読み込むと失敗する。

::: zone-end

::: zone pivot="os-linux"

# <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

.NET Core 3.1 は、1 つのオペレーティング システムとして Linux を扱います。 サポートされている Linux ディストリビューション用に、1 つの Linux ビルド (チップ アーキテクチャあたり) があります。

.NET Core 3.1 は、次の Linux ディストリビューション/バージョンでサポートされています。

> [!NOTE]
> `+` 記号は、最小バージョンを表します。

| OS                             | バージョン               | アーキテクチャ    |
| ------------------------------ | --------------------- | ---------------- |
| Red Hat Enterprise Linux       | 6、7、8               | X64 |
| CentOS                         | 7+                    | X64 |
| Oracle Linux                   | 7+                    | X64 |
| Fedora                         | 29+                   | X64 |
| Debian                         | 9+                    | x64、ARM32、ARM64 |
| Ubuntu                         | 16.04+                | x64、ARM32、ARM64 |
| Linux Mint                     | 18+                   | X64 |
| openSUSE                       | 15+                   | X64 |
| SUSE Enterprise Linux (SLES)   | 12 SP2+               | X64 |
| Alpine Linux                   | 3.10+                 | x64、ARM64 |

.NET Core 3.1 でサポートされているオペレーティング システム、ディストリビューション、ライフサイクル ポリシーの詳細については、「[.NET Core 3.1 Supported OS Versions](https://github.com/dotnet/core/blob/master/release-notes/3.1/3.1-supported-os.md)」(.NET Core 3.1 でサポートされている OS バージョン) を参照してください。

ARM64 (カーネル 4.14+) 上に .NET Core 3.1 をインストールする方法の詳細については、「[Installing .NET Core 3.0 on Linux ARM64](https://gist.github.com/richlander/467813274cea8abc624553ee72b28213)」(Linux ARM64 での .NET Core 3.0 のインストール) を参照してください。

> [!IMPORTANT]
> ARM64 のサポートには、Linux カーネル 4.14 以降が必要です。 一部の linux ディストリビューションは、この要件を満たしていますが、そうでないものもあります。 たとえば、Ubuntu 18.04 ではサポートされていますが、Ubuntu 16.04 ではされていません。

# <a name="net-core-30"></a>[.NET Core 3.0](#tab/netcore30)

.NET Core 3.0 は、1 つのオペレーティング システムとして Linux を扱います。 サポートされている Linux ディストリビューション用に、1 つの Linux ビルド (チップ アーキテクチャあたり) があります。

.NET Core 3.0 は、次の Linux ディストリビューション/バージョンでサポートされています。

> [!NOTE]
> `+` 記号は、最小バージョンを表します。

| OS                             | バージョン               | アーキテクチャ    |
| ------------------------------ | --------------------- | ---------------- |
| Red Hat Enterprise Linux       | 6、7、8               | X64 |
| CentOS                         | 7+                    | X64 |
| Oracle Linux                   | 7+                    | X64 |
| Fedora                         | 29+                   | X64 |
| Debian                         | 9+                    | x64、ARM32、ARM64 |
| Ubuntu                         | 16.04+                | x64、ARM32、ARM64 |
| Linux Mint                     | 18+                   | X64 |
| openSUSE                       | 15+                   | X64 |
| SUSE Enterprise Linux (SLES)   | 12 SP2+               | X64 |
| Alpine Linux                   | 3.8+                  | x64、ARM64 |

.NET Core 3.0 でサポートされているオペレーティング システム、ディストリビューション、ライフサイクル ポリシーの詳細については、「[.NET Core 3.0 Supported OS Versions](https://github.com/dotnet/core/blob/master/release-notes/3.0/3.0-supported-os.md)」(.NET Core 3.0 でサポートされている OS バージョン) を参照してください。

ARM64 で .NET Core 3.0 をインストールする方法の詳細については、「[Installing .NET Core 3.0 on Linux ARM64](https://gist.github.com/richlander/467813274cea8abc624553ee72b28213)」 (Linux ARM64 での .NET Core 3.0 のインストール) を参照してください。

# <a name="net-core-22"></a>[.NET Core 2.2](#tab/netcore22)

.NET Core 2.2 は、1 つのオペレーティング システムとして Linux を扱います。 サポートされている Linux ディストリビューション用に、1 つの Linux ビルド (チップ アーキテクチャあたり) があります。

.NET Core 2.2 は、次の Linux ディストリビューション/バージョンでサポートされています。

> [!NOTE]
> `+` 記号は、最小バージョンを表します。

| OS                             |  バージョン                |  アーキテクチャ   |
| ------------------------------ | ----------------------- | ---------------- |
| Red Hat Enterprise Linux       |  6、7                   | X64 |
| CentOS                         |  7                      | X64 |
| Oracle Linux                   |  7                      | X64 |
| Fedora                         |  29、30                 | X64 |
| Debian                         |  9                      | x64、ARM32 |
| Ubuntu                         |  16.04、18.04、18.10    | x64、ARM32 |
| Linux Mint                     |  17、18                 | X64 |
| openSUSE                       |  15+                    | X64 |
| SUSE Enterprise Linux (SLES)   |  12 SP2+                | X64 |
| Alpine Linux                   |  3.8+                   | X64 |

.NET Core 2.2 でサポートされているオペレーティング システム、ディストリビューション、ライフサイクル ポリシーの詳細については、「[.NET Core 2.2 Supported OS Versions](https://github.com/dotnet/core/blob/master/release-notes/2.2/2.2-supported-os.md)」(.NET Core 2.2 でサポートされている OS バージョン) を参照してください。

# <a name="net-core-21"></a>[.NET Core 2.1](#tab/netcore21)

.NET Core 2.1 は、1 つのオペレーティング システムとして Linux を扱います。 サポートされている Linux ディストリビューション用に、1 つの Linux ビルド (チップ アーキテクチャあたり) があります。

.NET Core 2.1 は、次の Linux ディストリビューション/バージョンでサポートされています。

> [!NOTE]
> `+` 記号は、最小バージョンを表します。

| OS                             |  バージョン                |  アーキテクチャ   |
| ------------------------------ | ----------------------- | ---------------- |
| Red Hat Enterprise Linux       |  6、7、8                | X64 |
| CentOS                         |  7+                     | X64 |
| Oracle Linux                   |  7+                     | X64 |
| Fedora                         |  29+                    | X64 |
| Debian                         |  9                      | x64、ARM32 |
| Ubuntu                         |  16.04、18.04、19.04、19.10    | x64、ARM32 |
| Linux Mint                     |  17+                    | X64 |
| openSUSE                       |  15+                    | X64 |
| SUSE Enterprise Linux (SLES)   |  12 SP2+                | X64 |
| Alpine Linux                   |  3.8+                   | X64 |

.NET Core 2.1 でサポートされているオペレーティング システム、ディストリビューション、ライフサイクル ポリシーの詳細については、「[.NET Core 2.1 Supported OS Versions](https://github.com/dotnet/core/blob/master/release-notes/2.1/2.1-supported-os.md)」(.NET Core 2.1 でサポートされている OS バージョン) を参照してください。

---

## <a name="linux-distribution-dependencies"></a>Linux ディストリビューションの依存関係

Linux ディストリビューションによっては、追加の依存関係のインストールが必要になる場合があります。

> [!IMPORTANT]
> 選択した Linux ディストリビューションで、バージョンと名前が多少異なる場合があります。

### <a name="ubuntu"></a>Ubuntu

Ubuntu ディストリビューションには、次のライブラリがインストールされている必要があります。

- liblttng-ust0
- libcurl3 (14.x および 16.x 用)
- libcurl4 (18.x 用)
- libssl1.0.0
- libkrb5-3
- zlib1g
- libicu52 (14.x 用)
- libicu55 (16.x 用)
- libicu57 (17.x 用)
- libicu60 (18.x 用)

*System.Drawing.Common* アセンブリを使用する .NET Core アプリの場合は、次の依存関係も必要です。

- libgdiplus (バージョン 6.0.1 以降)

> [!WARNING]
> Ubuntu のほとんどのバージョンには、以前のバージョンの libgdiplus が含まれています。 新しいバージョンの libgdiplus をインストールするには、システムに Mono リポジトリを追加します。 詳細については、「<https://www.mono-project.com/download/stable/>」を参照してください。

### <a name="centos-and-fedora"></a>CentOS と Fedora

CentOS ディストリビューションには、次のライブラリがインストールされている必要があります。

- lttng-ust
- libcurl
- openssl-libs
- krb5-libs
- libicu
- zlib

Fedora ユーザー:ご使用の OpenSSL のバージョンが 1.1 以降の場合は、**compat-openssl10** をインストールする必要があります。

.NET Core 2.0 では、次の依存関係も必要です。

- libunwind
- libuuid

依存関係の詳細については、「[Self-contained Linux applications](https://github.com/dotnet/core/blob/master/Documentation/self-contained-linux-apps.md)」(自己完結型 Linux アプリケーション) をご覧ください。

*System.Drawing.Common* アセンブリを使用する .NET Core アプリの場合は、次の依存関係も必要です。

- libgdiplus (バージョン 6.0.1 以降)

> [!WARNING]
> CentOS と Fedora のほとんどのバージョンには、以前のバージョンの libgdiplus が含まれています。 新しいバージョンの libgdiplus をインストールするには、システムに Mono リポジトリを追加します。 詳細については、「<https://www.mono-project.com/download/stable/>」を参照してください。

::: zone-end

::: zone pivot="os-macos"

.NET Core は、次の macOS のリリースでサポートされています。

> [!NOTE]
> `+` 記号は、最小バージョンを表します。

| .NET Core のバージョン | macOS                 | アーキテクチャ |     |
| ----------------- | --------------------- | --------------| --- |
| 3.1               | High Sierra (10.13+)  | X64 | [詳細情報](https://github.com/dotnet/core/blob/master/release-notes/3.1/3.1-supported-os.md) |
| 3.0               | High Sierra (10.13+)  | X64 | [詳細情報](https://github.com/dotnet/core/blob/master/release-notes/3.0/3.0-supported-os.md) |
| 2.2               | Sierra (10.12+)       | X64 | [詳細情報](https://github.com/dotnet/core/blob/master/release-notes/2.2/2.2-supported-os.md) |
| 2.1               | Sierra (10.12+)       | X64 | [詳細情報](https://github.com/dotnet/core/blob/master/release-notes/2.1/2.1-supported-os.md) |

macOS Catalina (バージョン 10.15) 以降では、2019 年 6 月 1 日より後に作成され、Developer ID と共に配布されたすべてのソフトウェアは公証される必要があります。 この要件は、.NET Core ランタイム、.NET Core SDK、および .NET Core を使用して作成されたソフトウェアに適用されます。

.NET Core (ランタイムと SDK の両方) バージョン 3.1、3.0、2.1 のインストーラーは、2020 年 2 月 18 日から公証されています。 それより前にリリースされたバージョンは、公証されていません。 公証されていないアプリを実行すると、次のイメージのようなエラーが表示されます。

![macOS Catalina の公証に関するアラート](media/dependencies/macos-notarized-pkg-warning.png)

公証の強制が .NET Core (および .NET Core アプリ) に与える影響の詳細については、[macOS Catalina の公証への対応](macos-notarization-issues.md)に関するページを参照してください。

## <a name="libgdiplus"></a>libgdiplus

*System.Drawing.Common* アセンブリを使用する .NET Core アプリケーションでは、libgdiplus をインストールする必要があります。

libgdiplus を取得する簡単な方法は、macOS の [Homebrew ("brew")](https://brew.sh/) パッケージ マネージャーを使用することです。 *brew* をインストールしたら、端末 (コマンド) プロンプトで次のコマンドを実行して libgdiplus をインストールします。

```console
brew update
brew install mono-libgdiplus
```

::: zone-end

## <a name="next-steps"></a>次の手順

- アプリを開発して実行するには、[.NET Core SDK](sdk.md) (ランタイムを含む) をインストールします。
- 他のユーザーが作成したアプリを実行するには、[.NET Core ランタイム](runtime.md)をインストールします。
