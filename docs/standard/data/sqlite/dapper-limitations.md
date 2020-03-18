---
title: Dapper の制限事項
ms.date: 12/13/2019
description: Dapper を使用する際に発生する制限事項について説明します。
ms.openlocfilehash: 396507f25f591a9ab5c3bb07c0af6fd8d175e4ea
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75901208"
---
# <a name="dapper-limitations"></a>Dapper の制限事項

[Dapper](https://stackexchange.github.io/Dapper/)と共に使用する場合は、いくつかの制限事項に注意する必要があります。

## <a name="parameters"></a>パラメーター

SQLite パラメーター名では大文字と小文字が区別されます。 SQL で使用されるパラメーター名が、匿名オブジェクトのプロパティの大文字と小文字の区別と一致していることを確認します。 [#18861](https://github.com/dotnet/efcore/issues/18861)問題を解決すると、このエクスペリエンスが向上します。

Dapper では、`@` プレフィックスを使用するパラメーターも必要です。 その他のプレフィックスは機能しません。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/DapperSample/Program.cs?name=snippet_Parameter)]

## <a name="data-types"></a>データの種類

Dapper は、SqliteDataReader インデクサーを使用して値を読み取ります。 このインデクサーの戻り値の型は object です。つまり、long、double、string、または byte [] の値のみが返されます。 詳細については、「[データ型](types.md)」を参照してください。 Dapper は、これらのプリミティブ型と他のプリミティブ型とのほとんどの変換を処理します。 残念ながら、`DateTimeOffset`、`Guid`、`TimeSpan`は処理されません。 これらの型を結果で使用する場合は、型ハンドラーを作成します。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/DapperSample/Program.cs?name=snippet_TypeHandlers)]

クエリを実行する前に、必ず型ハンドラーを追加してください。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/DapperSample/Program.cs?name=snippet_AddTypeHandlers)]

## <a name="see-also"></a>関連項目

* [データ型](types.md)
* [非同期の制限事項](async.md)
