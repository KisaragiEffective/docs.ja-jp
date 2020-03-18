---
title: join 句 - C# リファレンス
ms.date: 07/20/2015
f1_keywords:
- join
- join_CSharpKeyword
helpviewer_keywords:
- join clause [C#]
- join keyword [C#]
ms.assetid: 76e9df84-092c-41a6-9537-c3f1cbd7f0fb
ms.openlocfilehash: 8e52e9db241392b67818b7316767dd97bd38432a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713404"
---
# <a name="join-clause-c-reference"></a>join 句 (C# リファレンス)

`join` 句は、オブジェクト モデル内での直接リレーションシップがない、さまざまなソース シーケンスの要素を関連付ける際に役立ちます。 唯一の要件は、等価性を比較できるいくつかの値が各ソース内の要素間で共有されていることです。 たとえば、食品販売会社には特定の商品についての供給元のリストと購入者のリストがあります。 `join` 句は、たとえば指定された地域のすべての供給元および購入者のリストを作成するために使用できます。

`join` 句では、2 つのソース シーケンスを入力として受け取ります。 各シーケンスの要素は、もう一方のシーケンスの対応するプロパティと比較できるプロパティであるか、そのプロパティを含む要素であることが必要です。 `join` 句では、特殊な `equals` キーワードを使用して、指定されたキーの等価性が比較されます。 `join` 句によって実行される結合はすべてが等結合です。 `join` 句の出力形式は、実行する結合の種類によって異なります。 最も一般的な 3 種類の結合を以下に示します。

- 内部結合

- グループ結合

- 左外部結合

## <a name="inner-join"></a>内部結合

次の例は、単純な内部等結合を示しています。 このクエリによって "商品名/カテゴリ" のペアからなるフラットなシーケンスが生成されます。 複数の要素に同じカテゴリ文字列が含まれます。 `categories` の要素に一致する `products` がない場合、そのカテゴリは結果に含まれません。

[!code-csharp[cscsrefQueryKeywords#24](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Join.cs#24)]

詳細については、「[内部結合の実行](../../linq/perform-inner-joins.md)」を参照してください。

## <a name="group-join"></a>グループ結合

`join` 式を使用した `into` 句はグループ結合と呼ばれます。

[!code-csharp[cscsrefQueryKeywords#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Join.cs#25)]

グループ結合によって生成される階層形式の結果シーケンスでは、左側のソース シーケンスの要素と一致する右側のソース シーケンスの 1 つ以上の要素が関連付けられています。 リレーショナル データベースにおいてグループ結合に相当する用語はありません。グループ結合とは、本質的にはオブジェクト配列のシーケンスです。

右側のソース シーケンスの要素に左側のソースの要素と一致するものが見つからない場合は、その項目に対して `join` 句によって空の配列が生成されます。 つまりグループ結合は、結果シーケンスがグループに整理されることを除けば、基本的には内部等結合です。

グループ結合の結果を選択しただけでは、項目にアクセスすることはできでも、その照合に使用するキーを特定することはできません。 そのため、通常はグループ結合の結果を選択し、前の例に示したようなキー名を含む新しい型にするとさらに便利になります。

また、当然ながらグループ結合の結果を別のサブクエリのジェネレーターとして使用することもできます。

[!code-csharp[cscsrefQueryKeywords#26](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Join.cs#26)]

詳細については、「[グループ結合の実行](../../linq/perform-grouped-joins.md)」を参照してください。

## <a name="left-outer-join"></a>左外部結合

左外部結合では、右側のシーケンスに一致する要素がなくても、左側のソース シーケンスのすべての要素が返されます。 LINQ で左外部結合を実行するには、`DefaultIfEmpty` メソッドとグループ結合を組み合わせて使用し、左側の要素に一致するものがない場合に既定の右側の要素を生成するように指定します。 参照型用の既定値として `null` を使用するか、ユーザー定義の既定の型を指定できます。 次の例では、ユーザー定義の既定の型を示しています。

[!code-csharp[cscsrefQueryKeywords#27](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Join.cs#27)]

詳細については、「[左外部結合の実行](../../linq/perform-left-outer-joins.md)」を参照してください。

## <a name="the-equals-operator"></a>等値演算子

`join` 句は等結合を実行します。 つまり、基準にできるのは 2 つのキーの等価性に関する照合のみです。 "～より大きい" や "等しくない" など、他の種類の比較はサポートされていません。 すべての結合が等結合であることを明確化するために、`join` 句では `equals` 演算子ではなく `==` キーワードを使用します。 `equals` キーワードは `join` 句でしか使用できず、また `==` 演算子とは 1 つの重要な点で異なります。 `equals` を指定すると、左側のキーでは外部のソース シーケンス、右側のキーでは内部のソースが使用されます。 外部のソースは `equals` の左側のスコープ内、内部のソース シーケンスは右側のスコープ内でのみ使用できます。

## <a name="non-equijoins"></a>非等結合

複数の `from` 句を使用して新しいシーケンスをクエリに個別に導入することで、非等結合、クロス結合、およびその他のカスタム結合操作を実行できます。 詳細については、「[カスタム結合操作の実行](../../linq/perform-custom-join-operations.md)」を参照してください。

## <a name="joins-on-object-collections-vs-relational-tables"></a>オブジェクト コレクションとリレーショナル テーブルでの結合の比較

LINQ クエリ式での結合操作は、オブジェクト コレクションに対して実行されます。 2 つのリレーショナル テーブルの "結合" とまったく同じ方法でオブジェクト コレクションを結合することはできません。 LINQ では、2 つのソース シーケンスがリレーションシップによって関連付けられていない場合にのみ明示的な `join` 句が必要になります。 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] を使用する場合、外部キー テーブルはオブジェクト モデル内でプライマリ テーブルのプロパティとして表されます。 たとえば Northwind データベースでは、Customer テーブルに Orders テーブルとの外部キー リレーションシップがあります。 テーブルをオブジェクト モデルに割り当てると、Customer クラスには、その Customer に関連付けられた Orders のコレクションを含む Orders プロパティが含まれます。 実質的には、既に結合が実行されていることになります。

[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] を使用した関連テーブル間でのクエリの詳細については、「[方法: データベース リレーションシップを割り当てる](../../../framework/data/adonet/sql/linq/how-to-map-database-relationships.md)」を参照してください。

## <a name="composite-keys"></a>複合キー

複合キーを使用すると、複数の値の等価性をテストできます。 詳細については、「[複合キーを使用した結合](../../linq/join-by-using-composite-keys.md)」を参照してください。 複合キーは、`group` 句でも使用できます。

## <a name="example"></a>例

次の例では、同じ照合キーを使用し、同じデータ ソースでの内部結合、グループ結合、左外部結合の結果を比較しています。 これらの例には、結果をコンソールにわかりやすく表示するためのコードがいくつか追加されています。

[!code-csharp[cscsrefQueryKeywords#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Join.cs#23)]

## <a name="remarks"></a>解説

`join` 句の後に `into` がない場合は、<xref:System.Linq.Enumerable.Join%2A> メソッド呼び出しに変換されます。 `join` 句の後に `into` がある場合は、<xref:System.Linq.Enumerable.GroupJoin%2A> メソッド呼び出しに変換されます。

## <a name="see-also"></a>参照

- [クエリ キーワード (LINQ)](query-keywords.md)
- [統合言語クエリ (LINQ)](../../linq/index.md)
- [結合演算](../../programming-guide/concepts/linq/join-operations.md)
- [group 句](group-clause.md)
- [左外部結合の実行](../../linq/perform-left-outer-joins.md)
- [内部結合の実行](../../linq/perform-inner-joins.md)
- [グループ化結合の実行](../../linq/perform-grouped-joins.md)
- [join 句の結果の順序指定](../../linq/order-the-results-of-a-join-clause.md)
- [複合キーを使用した結合](../../linq/join-by-using-composite-keys.md)
- [Visual Studio 向けの互換性のあるデータベース システム](/visualstudio/data-tools/installing-database-systems-tools-and-samples)
