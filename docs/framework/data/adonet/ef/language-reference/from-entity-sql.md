---
title: FROM (Entity SQL)
ms.date: 03/30/2017
ms.assetid: ff3e3048-0d5d-4502-ae5c-9187fcbd0514
ms.openlocfilehash: 2334a30009d6bef9544d2ca1e0ab923a7441d6f2
ms.sourcegitcommit: 8a0fe8a2227af612f8b8941bdb8b19d6268748e7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/03/2019
ms.locfileid: "71833826"
---
# <a name="from-entity-sql"></a>FROM (Entity SQL)
[SELECT](select-entity-sql.md)ステートメントで使用するコレクションを指定します。

## <a name="syntax"></a>構文

```sql
FROM expression [ ,...n ] AS C
```

## <a name="arguments"></a>引数

`expression` \
`SELECT` ステートメントのソースとして使用するコレクションを生成する任意の有効なクエリ式。

## <a name="remarks"></a>コメント

`FROM` 句は、1 つ以上の `FROM` 句の項目をコンマで区切ったリストです。 `FROM` 句を使用して、`SELECT` ステートメントのソースを 1 つ以上指定できます。 `FROM` 句の最も単純な形式は、次の例に示すように、`SELECT` ステートメントのソースとして使用する 1 つのコレクションと 1 つの別名を識別する単一のクエリ式です。

`FROM C as c`

## <a name="from-clause-items"></a>FROM 句の項目

各 `FROM` 句項目は、[!INCLUDE[esql](../../../../../../includes/esql-md.md)] クエリ内のソース コレクションを参照します。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] では、単純 `FROM` 句の項目、`JOIN FROM` 句の項目、および `APPLY FROM` 句の項目のクラス `FROM` 句の項目がサポートされています。 これらの `FROM` 句の各項目については、以下のセクションで詳しく説明します。

### <a name="simple-from-clause-item"></a>単純な FROM 句の項目

最も単純な `FROM` 句の項目は、1 つのコレクションと 1 つの別名を識別する単一の式です。 式には、エンティティ セット、サブクエリ、またはコレクション型のその他の任意の式を使用できます。 次に例を示します。

```sql
LOB.Customers as c
```

別名の指定は省略可能です。 上記のように FROM 句の項目を指定する代わりに、次のように指定することもできます。

```sql
LOB.Customers
```

別名を指定しない場合、[!INCLUDE[esql](../../../../../../includes/esql-md.md)] はコレクション式に基づいて別名の生成を試みます。

### <a name="join-from-clause-item"></a>JOIN FROM 句の項目

`JOIN FROM` 句項目は、2 つの `FROM` 句項目の間の結合を表します。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] は、クロス結合、内部結合、左および右外部結合、および完全外部結合をサポートしています。 これらすべての結合は、Transact-sql でサポートされているのと同様にサポートされています。 Transact-sql と同様に、`JOIN` に含まれる2つの `FROM` 句の項目は独立している必要があります。 つまり、相互に関連付けられた項目は使用できません。 このような場合には、`CROSS APPLY` または `OUTER APPLY` を使用できます。

#### <a name="cross-joins"></a>クロス結合

次の例に示すように、`CROSS JOIN` クエリ式は 2 つのコレクションのデカルト積を生成します。

`FROM C AS c CROSS JOIN D as d`

#### <a name="inner-joins"></a>内部結合

次の例に示すように、`INNER JOIN` は 2 つのコレクションの制約付きデカルト積を生成します。

`FROM C AS c [INNER] JOIN D AS d ON e`

上記のクエリ式では、`ON` 条件が指定されており、右のコレクションの各要素と対になっている左のコレクションの各要素を結合して処理します。 `ON` 条件を指定しない場合、`INNER JOIN` の動作は `CROSS JOIN` と同じになります。

#### <a name="left-outer-joins-and-right-outer-joins"></a>左外部結合と右外部結合

次の例に示すように、`OUTER JOIN` クエリ式は 2 つのコレクションの制約付きデカルト積を生成します。

`FROM C AS c LEFT OUTER JOIN D AS d ON e`

上記のクエリ式では、`ON` 条件が指定されており、右のコレクションの各要素と対になっている左のコレクションの各要素を結合して処理します。 `ON` 条件が指定されていない場合、式は、NULL 値を使用して、右の要素と対になっている左の要素の単一のインスタンスを処理します。

`RIGHT OUTER JOIN` についても同様です。

#### <a name="full-outer-joins"></a>完全外部結合

次の例に示すように、明示的な `FULL OUTER JOIN` は 2 つのコレクションの制約付きデカルト積を生成します。

`FROM C AS c FULL OUTER JOIN D AS d ON e`

上記のクエリ式では、`ON` 条件が指定されており、右のコレクションの各要素と対になっている左のコレクションの各要素を結合して処理します。 `ON` 条件が指定されていない場合、式は、NULL 値を使用して、右の要素と対になっている左の要素のインスタンスを 1 つ処理します。 また、NULL 値を使用して、左の要素と対になっている右の要素のインスタンスも 1 つ処理します。

> [!NOTE]
> SQL-92 との互換性を維持するために、Transact-sql では、OUTER キーワードは省略可能です。 したがって、`LEFT JOIN`、`RIGHT JOIN`、および `FULL JOIN` は、`LEFT OUTER JOIN`、`RIGHT OUTER JOIN`、および `FULL OUTER JOIN` のシノニムです。

### <a name="apply-clause-item"></a>APPLY 句の項目

[!INCLUDE[esql](../../../../../../includes/esql-md.md)] は、`CROSS APPLY` と `OUTER APPLY`の2種類の `APPLY`をサポートしています。

`CROSS APPLY` は、左のコレクションの各要素と右の式の評価によって生成されたコレクションの要素との一意の組み合わせを生成します。 `CROSS APPLY` では、右の式は左の要素に機能的に依存します。このような関連付けられたコレクションの例を次に示します。

`SELECT c, f FROM C AS c CROSS APPLY c.Assoc AS f`

`CROSS APPLY` の動作は結合リストに似ています。 右の式が空のコレクションとして評価される場合、`CROSS APPLY` は、左の要素のそのインスタンスの組み合わせを生成しません。

`OUTER APPLY` は、右の式が空のコレクションとして評価される場合でも組み合わせを生成する以外は、`CROSS APPLY` と同様です。 `OUTER APPLY` の使用例を次に示します。

`SELECT c, f FROM C AS c OUTER APPLY c.Assoc AS f`

> [!NOTE]
> Transact-sql とは異なり、[!INCLUDE[esql](../../../../../../includes/esql-md.md)]に明示的に入れ子になっていない手順は必要ありません。

> [!NOTE]
> `CROSS` および `OUTER APPLY` の演算子は SQL Server 2005 で導入されました。 場合によっては、クエリ パイプラインにより、`CROSS APPLY` 演算子または `OUTER APPLY` 演算子を含む Transact-SQL が生成されることがあります。 SQL Server 2005 より前の SQL Server のバージョンを含む一部のバックエンドプロバイダーは、これらの演算子をサポートしていないため、これらのバックエンドプロバイダーでクエリを実行することはできません。
>
> `CROSS APPLY` 演算子または `OUTER APPLY` 演算子を含むクエリの生成につながる可能性がある一般的なシナリオとしては、ページングを使用した相関サブクエリ、相関サブクエリ全体またはナビゲーションによって生成されたコレクション全体を対象とした AnyElement、要素セレクターを受け取るグループ化メソッドを使用した LINQ クエリ、`CROSS APPLY` 演算子または `OUTER APPLY` 演算子が明示的に指定されたクエリ、`DEREF` コンストラクターを引数に取る `REF` コンストラクターを含むクエリなどがあります。

## <a name="multiple-collections-in-the-from-clause"></a>FROM 句での複数のコレクション

`FROM` 句には、複数のコレクションをコンマで区切って含めることができます。 この場合、これらのコレクションは 1 つに結合されるものと見なされます。 これは、n 方向の CROSS JOIN と考えることができます。

次の例では、`C` と `D` は独立したコレクションですが、`c.Names` は `C`に依存しています。

```sql
FROM C AS c, D AS d, c.Names AS e
```

上記の例は、次の例と論理的に等価です。

`FROM (C AS c JOIN D AS d) CROSS APPLY c.Names AS e`

## <a name="left-correlation"></a>左の相関関係
 `FROM` 句の項目は、前の句で指定された項目を参照できます。 次の例で、`C` と `D` は独立したコレクションですが、`c.Names` は `C` に依存しています。

```sql
from C as c, D as d, c.Names as e
```

上記の例は、次の例と論理的に等価です。

```sql
from (C as c join D as d) cross apply c.Names as e
```

## <a name="semantics"></a>Semantics

論理上、`FROM` 句のコレクションは `n` 方向のクロス結合の一部になると見なされます (1 方向のクロス結合の場合を除く)。 `FROM` 句内の別名は、左から右へと処理され、後で参照できるように現在のスコープに追加されます。 `FROM` 句は、行のマルチセットを生成すると見なされます。 `FROM` 句の項目ごとに 1 つのフィールドが生成され、そのコレクションの項目の 1 つの要素を表します。

`FROM` 句は、c、d、e の各フィールドが `C`、`D`、`c.Names` の要素型になると見なされる Row(c, d, e) 型の行のマルチセットを論理的に生成します。

[!INCLUDE[esql](../../../../../../includes/esql-md.md)] では、スコープ内の各単純 `FROM` 句の項目に別名が導入されています。 たとえば、次の FROM 句の例では、スコープに導入される名前は c、d、e です。

```sql
from (C as c join D as d) cross apply c.Names as e
```

[!INCLUDE[esql](../../../../../../includes/esql-md.md)] (Transact-sql とは異なり) では、`FROM` 句によって、エイリアスのみがスコープに導入されます。 これらのコレクションの列 (プロパティ) への参照は、別名を使用して修飾する必要があります。

## <a name="pulling-up-keys-from-nested-queries"></a>入れ子になったクエリからのキーの抽出

入れ子になったクエリからのキーの抽出を必要とする特定の種類のクエリはサポートされていません。 たとえば、次のクエリは有効です。

```sql
select c.Orders from Customers as c
```

しかし、次のクエリは、入れ子になったクエリにキーがないため無効です。

```sql
select {1} from {2, 3}
```

## <a name="see-also"></a>参照

- [Entity SQL リファレンス](entity-sql-reference.md)
- [クエリ式](query-expressions-entity-sql.md)
- [NULL 値が許容される構造化型](nullable-structured-types-entity-sql.md)
