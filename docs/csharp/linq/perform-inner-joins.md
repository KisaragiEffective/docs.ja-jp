---
title: 内部結合の実行 (C# での LINQ)
description: C# で LINQ を使用して、内部結合を実行する方法について説明します。
ms.date: 12/01/2016
ms.assetid: 45bceed6-f549-4114-a9b1-b44feb497742
ms.openlocfilehash: a3e8e9bd97ec630797bc48a3302b27ed45d9103e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "61659840"
---
# <a name="perform-inner-joins"></a>内部結合の実行

リレーショナル データベースでは、"*内部結合*" により、2 番目のコレクション内の一致するすべての要素に対して、最初のコレクションの各要素が一度表示される結果セットが生成されます。 最初のコレクション内の要素に一致する要素が存在しない場合、その要素は結果セットには表示されません。 <xref:System.Linq.Enumerable.Join%2A> メソッドは、C# の `join` 句によって呼び出され、内部結合を実装します。

この記事では、次の 4 種類の内部結合を実行する方法を示します。

- 簡単なキーに基づいて、2 つのデータ ソースの要素を関連付ける単純な内部結合。

- "*複合*" キーに基づいて、2 つのデータ ソースの要素を関連付ける内部結合。 複合キーは複数の値で構成され、複数のプロパティに基づいて要素を関連付けることができます。

- 一連の結合操作が相互に追加された "*複数の結合*"。

- グループ結合を使用して実装された内部結合。

## <a name="example---simple-key-join"></a>例 - 簡単なキーの結合

次の例は、2 つのユーザー定義型オブジェクト、`Person` と `Pet` が含まれた 2 つのコレクションを作成します。 クエリでは、C# の `join` 句を使用して、`Person` オブジェクトを `Pet` がこの `Owner` である `Person` オブジェクトを照合します。 C# の `select` 句では、結果のオブジェクトの表示内容を定義します。 この例では、結果のオブジェクトは、飼い主の姓とペットの名前で構成される匿名型です。

[!code-csharp[CsLINQProgJoining#1](~/samples/snippets/csharp/concepts/linq/how-to-perform-inner-joins_1.cs)]

`Person` が "Huff" の `LastName` オブジェクトは、`Pet` がその `Pet.Owner` に等しい `Person` オブジェクトがないため、結果セットに表示されません。

## <a name="example---composite-key-join"></a>例 - 複合キーの結合

1 つのプロパティだけに基づいて要素を関連付ける代わりに、複合キーを使用して、複数のプロパティに基づいて要素を比較できます。 これを行うには、各コレクションに対してキー セレクター関数を指定し、比較するプロパティで構成された匿名型を返します。 プロパティにラベルを付ける場合は、各キーの匿名型に同じラベルを付ける必要があります。 また、プロパティは、同じ順序で表示する必要があります。

次の例は、`Employee` オブジェクトのリストと `Student` オブジェクトのリストを使用して、学生でもある社員を調べます。 これらの型の両方に、`FirstName` 型の `LastName` プロパティと <xref:System.String> プロパティがあります。 それぞれのリストの要素から結合キーを作成する関数が、各要素の `FirstName` プロパティと `LastName` プロパティで構成された匿名型を返します。 結合操作により、これらの複合キーが等しいかどうか比較され、それぞれのリストの氏名が一致するオブジェクトのペアが返されます。

[!code-csharp[CsLINQProgJoining#2](~/samples/snippets/csharp/concepts/linq/how-to-perform-inner-joins_2.cs)]

## <a name="example---multiple-join"></a>例 - 複数の結合

任意の数の結合操作を相互に追加して、複数の結合を実行できます。 C# の各 `join` 句は、指定されたデータ ソースを前の結合の結果に関連付けます。

次の例は、`Person` オブジェクトのリスト、`Cat` オブジェクトのリスト、`Dog` オブジェクトのリストの 3 つのコレクションを作成します。

C# の最初の `join` 句では、`Person` と一致する `Cat.Owner` オブジェクトに基づいて飼い主と猫を一致させます。 この操作で、`Person` オブジェクトと `Cat.Name` が含まれた匿名型のシーケンスが返されます。

C# の 2 番目の `join` 句では、`Dog` 型の `Owner` プロパティと動物の名前の最初の文字で構成される複合キーに基づいて、最初の結合で返された匿名型を、指定された犬のリストの `Person` オブジェクトに関連付けます。 この操作で、一致するそれぞれのペアの `Cat.Name` プロパティと `Dog.Name` プロパティが含まれた匿名型のシーケンスが返されます。 これは内部結合であるため、2 番目のデータ ソースに一致するものが存在する、最初のデータ ソースのオブジェクトのみが返されます。

[!code-csharp[CsLINQProgJoining#3](~/samples/snippets/csharp/concepts/linq/how-to-perform-inner-joins_3.cs)]

## <a name="example---inner-join-by-using-grouped-join"></a>例 - グループ化結合を使用した内部結合

グループ結合を使用して内部結合を実装する方法を次の例に示します。

`query1` で、`Person` オブジェクトのリストは、`Pet` プロパティと一致する `Person` に基づいて、`Pet.Owner` オブジェクトのリストにグループ結合されます。 グループ結合によって、それぞれのグループが `Person` オブジェクトおよび一致する `Pet` オブジェクトのシーケンスで構成された、中間グループのコレクションが作成されます。

2 番目の `from` 句をクエリに追加すると、シーケンスのシーケンスが 1 つの長いシーケンスに結合 (または平坦化) されます。 最後のシーケンスの要素の型は、`select` 句で指定されます。 この例では、この型は、一致する各ペアの `Person.FirstName` プロパティと `Pet.Name` プロパティで構成された匿名型です。

`query1` の結果は、`join` 句のない `into` 句を使用して内部結合を実行することで得られた結果セットと同じです。 `query2` 変数は、これと同等のクエリを示しています。

[!code-csharp[CsLINQProgJoining#4](~/samples/snippets/csharp/concepts/linq/how-to-perform-inner-joins_4.cs)]

## <a name="see-also"></a>参照

- <xref:System.Linq.Enumerable.Join%2A>
- <xref:System.Linq.Enumerable.GroupJoin%2A>
- [グループ化結合の実行](perform-grouped-joins.md)
- [左外部結合の実行](perform-left-outer-joins.md)
- [匿名型](../programming-guide/classes-and-structs/anonymous-types.md)
