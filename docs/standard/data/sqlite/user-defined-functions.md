---
title: ユーザー定義関数
ms.date: 12/13/2019
description: ユーザー定義のスカラー関数と集計関数を作成する方法について説明します。
ms.openlocfilehash: 9ee3fbac73215353c8dc82199203b0d25e580cdb
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450412"
---
# <a name="user-defined-functions"></a>ユーザー定義関数

ほとんどのデータベースには、独自の関数を定義するために使用できる SQL の手続き型の言語が用意されています。 ただし、SQLite はアプリでインプロセスで実行されます。 SQL の新しい言語を習得する代わりに、アプリのプログラミング言語を使用するだけで済みます。

## <a name="scalar-functions"></a>スカラー関数

スカラー関数は、クエリ内の行ごとに1つのスカラー値を返します。 新しいスカラー関数を定義し、<xref:Microsoft.Data.Sqlite.SqliteConnection.CreateFunction%2A>を使用して組み込みの関数をオーバーライドします。

`func` 引数のサポートされているパラメーターと戻り値の型の一覧については、「[データ型](types.md)」を参照してください。

`state` 引数を指定すると、その値が関数のすべての呼び出しに渡されます。 クロージャを回避するには、これを使用します。

クエリをコンパイルするときに SQLite で追加の最適化を使用できるようにするために、関数が決定的である場合は `isDeterministic` を指定します。

次の例では、スカラー関数を追加して、円柱の半径を計算する方法を示します。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/ScalarFunctionSample/Program.cs?name=snippet_CreateFunction)]

## <a name="operators"></a>演算子

次の SQLite 演算子は、対応するスカラー関数によって実装されます。 これらのスカラー関数をアプリに定義すると、これらの演算子の動作が上書きされます。

| 演算子          | 関数      |
| ----------------- | ------------- |
| X GLOB Y          | glob (Y, X)    |
| Y のような X          | like (Y, X)    |
| X のような Y エスケープ Z | like (Y, X, Z) |
| X 一致 Y         | 一致 (Y, X)   |
| X REGEXP (Y)        | regexp (Y, X)  |

次の例では、対応する演算子を有効にするために regexp 関数を定義する方法を示します。 SQLite には、regexp 関数の既定の実装は含まれていません。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/RegularExpressionSample/Program.cs?name=snippet_Regex)]

## <a name="aggregate-functions"></a>集計関数

集計関数は、クエリ内のすべての行に対して1つの集計値を返します。 <xref:Microsoft.Data.Sqlite.SqliteConnection.CreateAggregate%2A>を使用して集計関数を定義およびオーバーライドします。

`seed` 引数は、コンテキストの初期状態を指定します。 クロージャも使用しないようにします。

`func` 引数は、行ごとに1回だけ呼び出されます。 コンテキストを使用して、最終結果を累積します。 コンテキストを返します。 このパターンでは、コンテキストを値型にすることも、変更できないようにすることもできます。

`resultSelector` が指定されていない場合、コンテキストの最終的な状態が結果として使用されます。 これにより、sum や count などの関数の定義が単純化され、行ごとに番号をインクリメントして返すだけで済みます。

すべての行を反復処理した後、コンテキストから最終的な結果を計算するには、`resultSelector` を指定します。

`resultSelector`の `func` の引数と戻り値の型については、「[データ型](types.md)」を参照してください。

関数が決定的である場合は、`isDeterministic` を指定して、SQLite がクエリをコンパイルするときに追加の最適化を使用できるようにします。

次の例では、列の標準偏差を計算する集計関数を定義します。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/AggregateFunctionSample/Program.cs?name=snippet_CreateAggregate)]

## <a name="errors"></a>エラー

ユーザー定義関数が例外をスローした場合、メッセージは SQLite に返されます。 SQLite によってエラーが発生し、SqliteException がスローされます。 詳細については、「[データベースエラー](database-errors.md)」を参照してください。

既定では、エラー SQLite エラーコードは SQLITE_ERROR (または 1) になります。 ただし、必要な <xref:Microsoft.Data.Sqlite.SqliteException.SqliteErrorCode> を指定して関数内の <xref:Microsoft.Data.Sqlite.SqliteException> をスローすることによって変更できます。

## <a name="debugging"></a>デバッグ

SQLite は、実装を直接呼び出します。 これにより、SQLite がクエリを評価している間にトリガーされるブレークポイントを追加できます。 完全な .NET デバッグエクスペリエンスは、ユーザー定義関数の作成に役立ちます。

## <a name="see-also"></a>参照

* [データ型](types.md)
* [SQLite コア関数](https://www.sqlite.org/lang_corefunc.html)
* [SQLite 集計関数](https://www.sqlite.org/lang_aggfunc.html)
