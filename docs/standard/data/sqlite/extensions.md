---
title: 拡張
ms.date: 12/13/2019
description: SQLite 拡張機能を読み込む方法について説明します。
ms.openlocfilehash: a85b1227be4274dd20156d2475d6d2d68e250f99
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75901296"
---
# <a name="extensions"></a>拡張

SQLite では、実行時の拡張機能の読み込みがサポートされています。 拡張機能には、追加の SQL 関数、照合順序、仮想テーブルなどが含まれます。

.NET Core には、参照される NuGet パッケージなどの追加の場所でネイティブライブラリを検索するための追加のロジックが含まれています。 残念ながら、SQLite はこのロジックを利用できません。ライブラリを読み込むために、プラットフォーム API を直接呼び出します。 このため、SQLite 拡張機能を読み込む前に、パス、LD_LIBRARY_PATH、または DYLD_LIBRARY_PATH 環境変数の変更が必要になる場合があります。 GitHub に[は](https://github.com/dotnet/samples/blob/master/snippets/standard/data/sqlite/ExtensionsSample/Program.cs)、参照される NuGet パッケージ内の現在のランタイムのバイナリを検索する例があります。

拡張機能を読み込むには、<xref:Microsoft.Data.Sqlite.SqliteConnection.LoadExtension%2A> メソッドを呼び出します。 接続が閉じられてから再び開かれた場合でも、拡張機能は読み込まれたままになります。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/ExtensionsSample/Program.cs?name=snippet_LoadExtension)]

## <a name="see-also"></a>関連項目

* [実行時の読み込み可能な拡張機能](https://www.sqlite.org/loadext.html)
