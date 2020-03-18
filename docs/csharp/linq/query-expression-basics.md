---
title: クエリ式の基本 (C# での LINQ)
description: クエリ式に関連する概念について説明します
ms.date: 11/30/2016
ms.assetid: 027db1f8-346f-44d2-a16e-043fcea3a4e0
ms.openlocfilehash: 83beaa82d4b4b42ff9da5230edddd391b33a0717
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173355"
---
# <a name="query-expression-basics"></a>クエリ式の基本

この記事では、C# でのクエリ式に関連する基本概念について説明します。

## <a name="what-is-a-query-and-what-does-it-do"></a>クエリとは何か。またどのような働きをするのか

*クエリ*とは、指定したデータ ソース (単一または複数) からどのようなデータを取得し、それらのデータをどのような形式と編成で返すかを説明した、命令のセットです。 クエリは、それが生成する結果とは区別されます。

一般に、ソース データは、同じ種類の要素のシーケンスとして論理的に編成されます。 たとえば、SQL データベース テーブルには、行のシーケンスが含まれています。 XML ファイルには、XML 要素のシーケンスがあります (ただし、これらはツリー構造で階層化されています)。 メモリ内コレクションには、オブジェクトのシーケンスが含まれています。

アプリケーションの観点から言うと、元のソース データの特定の型や構造体はは重要ではありません。 アプリケーションは常に、ソース データを <xref:System.Collections.Generic.IEnumerable%601> または <xref:System.Linq.IQueryable%601> コレクションとして認識します。 たとえば、LINQ to XML では、ソース データは `IEnumerable`\<<xref:System.Xml.Linq.XElement>> として表示されます。

クエリは、このソース シーケンスに対して、次の 3 つのうち、いずれかの操作を行います。

- 個々 の要素を変更することなく、要素のサブセットを取得して新しいシーケンスを生成する。 クエリはその後、次の例のように、返されたシーケンスをさまざまな方法で並べ替えたり、グループ化する場合があります (例では `scores` を `int[]` と想定)。

    [!code-csharp[csrefQueryExpBasics#45](~/samples/snippets/csharp/concepts/linq/query-expression-basics_1.cs)]

- 上記の例のように要素のシーケンスを取得するが、それらを新しい型のオブジェクトに変換する。 たとえば、クエリでは、データ ソース内の特定の顧客レコードから姓だけを取得することがあります。 また、完全なレコードを取得し、それを使用して別のメモリ内オブジェクト型や XML データを構築した後、最終的な結果シーケンスを生成することもあります。 次の例では、`int` から `string` へのプロジェクションを行っています。 `highScoresQuery` の新しい型があることに注目してください。

    [!code-csharp[csrefQueryExpBasics#46](~/samples/snippets/csharp/concepts/linq/query-expression-basics_2.cs)]

- ソース データに関するシングルトン値を取得します。次に例を示します。

  - 特定の条件に一致する要素の数。

  - 最大値または最小値を持つ要素。

  - 条件に一致する最初の要素や、指定された要素セット内の特定の値の合計。 たとえば、次のクエリは、整数配列 `scores` から、80 より大きいスコアの数を返します。

    [!code-csharp[csrefQueryExpBasics#47](~/samples/snippets/csharp/concepts/linq/query-expression-basics_3.cs)]

    上記の例では、`Count` メソッドに対する呼び出しの前に、クエリ式を囲むかっこが使用されています。 これは、具体的な結果を格納する新しい変数を使用しても表現できます。 この手法では、クエリを格納する変数が、結果を格納するクエリとは別に保持されるので、コードがより読みやすくなります。

    [!code-csharp[csrefQueryExpBasics#48](~/samples/snippets/csharp/concepts/linq/query-expression-basics_4.cs)]

上記の例では、`Count` に対する呼び出しの前でクエリが実行されています。これは、`highScoresQuery` によって返された要素の数を確認するために、`Count` が結果を反復処理する必要があるためです。

## <a name="what-is-a-query-expression"></a>クエリ式とは何か

*クエリ式*とは、クエリ構文で表されたクエリのことです。 クエリ式は、ファーストクラスの言語コンストラクトです。 他の式とよく似ていて、C# 式が有効である任意のコンテキストで使用できます。 クエリ式の構文は、SQL や XQuery などのような宣言型の構文で記述された、一連の句で構成されます。 各句には 1 つ以上の C# 式が含まれていて、それらの式は、それ自体がクエリ式である場合もあれば、クエリ式を含んでいる場合もあります。

クエリ式は [from](../language-reference/keywords/from-clause.md) 句で始まり、[select](../language-reference/keywords/select-clause.md) または [group](../language-reference/keywords/group-clause.md) 句で終わる必要があります。 最初の `from` 句と最後の `select` または `group` 句の間には、次の省略可能句を 1 つ以上含めることができます: [where](../language-reference/keywords/where-clause.md)、[orderby](../language-reference/keywords/orderby-clause.md)、[join](../language-reference/keywords/join-clause.md)、[let](../language-reference/keywords/let-clause.md)、および追加の [from](../language-reference/keywords/from-clause.md) 句。 また、[into](../language-reference/keywords/into.md) キーワードを使用して、`join` 句や `group` 句の結果を、同じクエリ式内の追加のクエリ句のソースとして使用することもできます。

### <a name="query-variable"></a>クエリ変数

LINQ では、クエリの*結果*ではなく、*クエリ*を格納する変数を、クエリ変数と呼びます。 より具体的に言うと、クエリ変数は常に列挙可能な型であり、`foreach` ステートメントか、または `IEnumerator.MoveNext` メソッドに対する直接呼び出しで反復処理された場合に、要素のシーケンスを生成します。

次のコード例は、データ ソース、フィルター句、および並べ替え句がそれぞれ 1 つずつあり、ソース要素の変換がない、簡単なクエリ式を示したものです。 `select` 句でクエリが終わっています。

[!code-csharp[csrefQueryExpBasics#49](~/samples/snippets/csharp/concepts/linq/query-expression-basics_5.cs)]

上記の例では、`scoreQuery` が *クエリ変数*です。クエリ変数は単に*クエリ*と呼ばれることもあります。 クエリ変数には、`foreach` ループで生成された実際の結果データは格納されません。 また、`foreach` ステートメントが実行されたとき、クエリ結果はクエリ変数 `scoreQuery` を通じては返されません。 結果は反復変数 `testScore` を通じて返されます。 `scoreQuery` 変数は 2 番目の `foreach` ループで反復処理できます。 この変数とデータ ソースのどちらかが変更されないかぎり、同じ結果が生成されます。

クエリ変数には、クエリ構文、メソッド構文、またはそれら 2 つの組合せで表現されたクエリが格納される場合があります。 次の例では、`queryMajorCities` と `queryMajorCities2` の両方がクエリ変数です。

[!code-csharp[csrefQueryExpBasics#50](~/samples/snippets/csharp/concepts/linq/query-expression-basics_6.cs)]

これに対し、次の 2 つの例は、クエリで初期化されてはいるものの、クエリ変数ではない変数を示しています。 これらは結果を格納するので、クエリ変数ではありません。

[!code-csharp[csrefQueryExpBasics#51](~/samples/snippets/csharp/concepts/linq/query-expression-basics_7.cs)]

クエリのさまざまな表現方法については、「[LINQ でのクエリ構文とメソッド構文](../programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq.md)」をご覧ください。

#### <a name="explicit-and-implicit-typing-of-query-variables"></a>クエリ変数の明示的型指定と暗黙的型指定

このドキュメントでは通常、明示的な型のクエリ変数で説明を行います。これは、クエリ変数と [select 句](../language-reference/keywords/select-clause.md)の関係を示すためです。 ただし、 [var](../language-reference/keywords/var.md) キーワードを使用すれば、コンパイル時にクエリ変数 (またはその他のローカル変数) の型を推論するようにコンパイラに指示することもできます。 たとえば、このトピックで先に示したクエリの例は、暗黙的な型指定を使用しても表現できます。

[!code-csharp[csrefQueryExpBasics#52](~/samples/snippets/csharp/concepts/linq/query-expression-basics_8.cs)]

詳しくは、「[暗黙的に型指定されるローカル変数](../programming-guide/classes-and-structs/implicitly-typed-local-variables.md)」および「[LINQ クエリ操作での型の関係](../programming-guide/concepts/linq/type-relationships-in-linq-query-operations.md)」をご覧ください。

### <a name="starting-a-query-expression"></a>クエリ式の開始

クエリ式は、`from` 句で始める必要があります。 この句では、データ ソースと範囲変数を指定します。 範囲変数は、ソース シーケンスが走査されるときの、ソース シーケンス内の連続する各要素を表します。 範囲変数は、データ ソース内の要素の型に基づいて厳密に型指定されます。 次の例では、`countries` が `Country` オブジェクトの配列であるため、範囲変数も `Country` として型指定されています。 範囲変数は厳密に型指定されるので、ドット演算子を使用して、その型の利用可能なメンバーにアクセスすることができます。

[!code-csharp[csrefQueryExpBasics#53](~/samples/snippets/csharp/concepts/linq/query-expression-basics_9.cs)]

範囲変数は、クエリがセミコロンまたは *continuation* 句で終了するまでスコープ内に維持されます。

クエリ式には、複数の `from` 句を含めることができます。 ソース シーケンス内の各要素がそれ自体コレクションであるか、またはコレクションを格納している場合には、追加の `from` 句を使用します。 たとえば、`Country` オブジェクトのコレクションがあり、各オブジェクトに、`Cities` という名前の `City` オブジェクトのコレクションが格納されているとします。 その場合、各 `Country` 内の `City` オブジェクトを照会するには、次のように 2 つの `from` 句を使用します。

[!code-csharp[csrefQueryExpBasics#54](~/samples/snippets/csharp/concepts/linq/query-expression-basics_10.cs)]

詳しくは、「[from 句](../language-reference/keywords/from-clause.md)」をご覧ください。

### <a name="ending-a-query-expression"></a>クエリ式の終了

クエリ式は、`group` 句または `select` 句のいずれかで終わる必要があります。

#### <a name="group-clause"></a>group 句

`group` 句は、指定したキーによって編成されたグループのシーケンスを生成するために使用します。 キーには、任意のデータ型を指定できます。 たとえば、次のクエリでは、1 つ以上の `Country` オブジェクトを含み、キーが `char` 値であるグループのシーケンスが作成されます。

[!code-csharp[csrefQueryExpBasics#55](~/samples/snippets/csharp/concepts/linq/query-expression-basics_11.cs)]

グループ化について詳しくは、「[group 句](../language-reference/keywords/group-clause.md)」をご覧ください。

#### <a name="select-clause"></a>select 句

`select` 句は、その他すべての型のシーケンスを生成するために使用します。 シンプルな `select` 句は、データ ソース内に含まれるオブジェクトと同じ型のオブジェクトのシーケンスを生成します。 この例では、データ ソースに `Country` オブジェクトが含まれています。 `orderby` 句は要素を新しい順序に並べ替え、`select` 句は並べ替えられた `Country` オブジェクトのシーケンスを生成します。

[!code-csharp[csrefQueryExpBasics#56](~/samples/snippets/csharp/concepts/linq/query-expression-basics_12.cs)]

`select` 句は、ソース データを新しい型のシーケンスに変換するために使用できます。 この変換は、*プロジェクション*とも呼ばれます。 次の例では、`select` 句は元の要素内にあるフィールドのサブセットのみを含んだ、匿名型のシーケンスを*プロジェクト*します。 新しいオブジェクトはオブジェクト初期化子を使用して初期化されています。

[!code-csharp[csrefQueryExpBasics#57](~/samples/snippets/csharp/concepts/linq/query-expression-basics_13.cs)]

`select` 句を使用してソース データを変換する方法について詳しくは、「[select 句](../language-reference/keywords/select-clause.md)」をご覧ください。

#### <a name="continuations-with-into"></a>"into" を使用した継続

`select` 句または `group` 句で `into` キーワードを使用すると、クエリを格納する一時的な識別子を作成できます。 これは、grouping 操作や select 操作の後、クエリに対する追加のクエリ操作を実行する必要がある場合に便利です。 次の例では、1 千万という範囲の人口で `countries` をグループ化しています。 これらのグループが作成された後、追加の句で一部のグループを除外し、その後、グループを昇順で並べ替えようとしています。 これらの追加操作を実行するには、`countryGroup` によって継続を表す必要があります。

[!code-csharp[csrefQueryExpBasics#58](~/samples/snippets/csharp/concepts/linq/query-expression-basics_14.cs)]

詳しくは、「[into](../language-reference/keywords/into.md)」をご覧ください。

### <a name="filtering-ordering-and-joining"></a>フィルター処理、並べ替え、および結合

開始の `from` 句と、終了の `select` または`group`句の間には、その他のすべての省略可能句 (`where`、`join`、`orderby`、`from`、`let`) を必要に応じて使用できます。 省略可能句は、クエリ本文で任意の回数 (0 回～複数回) 使用できます。

#### <a name="where-clause"></a>where 句

`where` 句は、1 つ以上の述語式に基づいて、ソース データから要素を除外するために使用します。 次の例では、`where` 句に 1 つの述語と 2 つの条件があります。

[!code-csharp[csrefQueryExpBasics#59](~/samples/snippets/csharp/concepts/linq/query-expression-basics_15.cs)]

詳しくは、「[where 句](../language-reference/keywords/where-clause.md)」をご覧ください。

#### <a name="orderby-clause"></a>orderby 句

`orderby` 句は、結果を昇順または降順で並べ替えるために使用します。 第 2 の並べ替え順序を指定することもできます。 次の例では、`Area` プロパティを使用して、`country` オブジェクトに対する 第 1 の並べ替えを実行しています。 その後、`Population` プロパティを使用して第 2 の並べ替えを実行しています。

[!code-csharp[csrefQueryExpBasics#60](~/samples/snippets/csharp/concepts/linq/query-expression-basics_16.cs)]

`ascending` キーワードは省略可能です。順序が指定されていない場合は、これが既定の並べ替え順序になります。 詳しくは、「[orderby 句](../language-reference/keywords/orderby-clause.md)」をご覧ください。

#### <a name="join-clause"></a>join 句

`join` 句は、各要素内の指定したキー間での等値比較に基づいて、1 つのデータ ソースの要素を別のデータ ソースの要素と関連付けたり、組み合わせたりするために使用します。 LINQ では、join 操作は要素の型が異なるオブジェクトのシーケンスに対して実行されます。 2 つのシーケンスを結合した後には、`select` または `group` ステートメント使用して、出力シーケンスに格納する要素を指定する必要があります。 また、匿名型を使用して、関連付けられた各要素セットのプロパティを、出力シーケンス用の新しい型に結合することもできます。 次の例では、`Category` プロパティが `categories` 文字列配列内のいずれかのカテゴリと一致する `prod` オブジェクトを関連付けています。 `Category` が `categories` 内の文字列に一致しない製品は除外されます。`select` ステートメントは、`cat` と `prod` の両方からプロパティを取った新しい型をプロジェクトしています。

[!code-csharp[csrefQueryExpBasics#61](~/samples/snippets/csharp/concepts/linq/query-expression-basics_17.cs)]

[into](../language-reference/keywords/into.md) キーワードを使用して `join` 操作の結果を一時変数に格納することにより、グループ結合を実行することもできます。 詳しくは、「[join 句](../language-reference/keywords/join-clause.md)」をご覧ください。

#### <a name="let-clause"></a>let 句

`let` 句は、式の結果 (メソッド呼び出しなど) を新しい範囲変数に格納するために使用します。 次の例では、`Split` よって返された文字列配列の最初の要素を、範囲変数 `firstName` に格納しています。

[!code-csharp[csrefQueryExpBasics#62](~/samples/snippets/csharp/concepts/linq/query-expression-basics_18.cs)]

詳しくは、「[let 句](../language-reference/keywords/let-clause.md)」をご覧ください。

### <a name="subqueries-in-a-query-expression"></a>クエリ式内のサブクエリ

クエリ句には、それ自体にクエリ式が含まれることがあります。これは、*サブクエリ*とも呼ばれます。 各サブクエリは、独自の `from` で始まります。この句は、最初の `from` 句と必ずしも同じデータ ソースを指している必要はありません。 たとえば、次のクエリは、select ステートメントで grouping 操作の結果を取得するために使用されるクエリ式を示しています。

[!code-csharp[csrefQueryExpBasics#63](~/samples/snippets/csharp/concepts/linq/query-expression-basics_19.cs)]

詳細については、「[グループ化操作でのサブクエリの実行](perform-a-subquery-on-a-grouping-operation.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../programming-guide/index.md)
- [統合言語クエリ (LINQ)](index.md)
- [クエリ キーワード (LINQ)](../language-reference/keywords/query-keywords.md)
- [標準クエリ演算子の概要](../programming-guide/concepts/linq/standard-query-operators-overview.md)
