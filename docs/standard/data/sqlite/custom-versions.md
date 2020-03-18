---
title: カスタム SQLite のバージョン
ms.date: 12/13/2019
description: ネイティブ SQLite ライブラリのカスタムバージョンを使用する方法について説明します。
ms.openlocfilehash: dd27278c1dbe17b12e5067d04d19043bf259b1e8
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746995"
---
# <a name="custom-sqlite-versions"></a>カスタム SQLite のバージョン

SQLitePCLRaw は、上に構築されています。 バンドルを使用するか、SQLitePCLRaw プロバイダーを構成することによって、ネイティブ SQLite ライブラリのカスタムバージョンを使用できます。

## <a name="bundles"></a>同梱

SQLitePCLRaw にはバンドルパッケージが用意されているため、さまざまなプラットフォーム間で適切な依存関係を簡単に取り込むことができます。

メインの SQLitePCLRaw パッケージは、既定で bundle_e_sqlite3 になります。

別のバンドルを使用するには、使用するバンドルパッケージと共に `Microsoft.Data.Sqlite.Core` パッケージをインストールします。 バンドルは、自動的に Microsoft. Data. Sqlite によって初期化されます。

| バンドル | [説明] |
| --- | --- |
| Bundle_e_sqlite3 SQLitePCLRaw | すべてのプラットフォームで SQLite の一貫性のあるバージョンを提供します。 には、FTS4、FTS5、JSON1、および R * ツリー拡張が含まれています。 これは既定値です。 |
| Bundle_green SQLitePCLRaw | Bundle_e_sqlite3 と同じですが、システム SQLite ライブラリを使用する iOS を除きます。 |
| Bundle_zetetic SQLitePCLRaw | では、Zetetic (含まれていません) からの公式の SQLCipher ビルドが使用されます。 |
| Bundle_winsqlite3 SQLitePCLRaw | Windows 10 のシステム SQLite ライブラリである winsqlite3 を使用します。 |
| Bundle_e_sqlcipher SQLitePCLRaw | SQLCipher の非公式なオープンソースビルドを提供します。 |

たとえば、非公式のオープンソースの SQLCipher ビルドを使用するには、次のコマンドを使用します。

### <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.Data.Sqlite.Core
dotnet add package SQLitePCLRaw.bundle_e_sqlcipher
```

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

``` PowerShell
Install-Package Microsoft.Data.Sqlite.Core
Install-Package SQLitePCLRaw.bundle_e_sqlcipher
```

---

## <a name="sqlitepclraw-providers"></a>SQLitePCLRaw プロバイダー

`SQLitePCLRaw.provider.dynamic_cdecl` パッケージを利用することで、独自の SQLite ビルドを使用できます。 この場合、アプリと共にネイティブライブラリをデプロイする必要があります。 アプリでのネイティブライブラリのデプロイの詳細は、使用している .NET プラットフォームとランタイムによって大きく異なります。

まず、IGetFunctionPointer を実装する必要があります。 .NET Core では、実装は非常に簡単です。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/SystemLibrarySample/Program.cs?name=snippet_NativeLibraryAdapter)]

次に、SQLitePCLRaw プロバイダーを構成します。 これは、アプリで使用される前に実行されていることを確認してください。 また、プロバイダーをオーバーライドする可能性のある SQLitePCLRaw バンドルパッケージを使用しないようにしてください。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/SystemLibrarySample/Program.cs?name=snippet_SetProvider)]
