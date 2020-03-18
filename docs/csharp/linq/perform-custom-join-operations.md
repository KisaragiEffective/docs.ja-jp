---
title: カスタム結合操作の実行 (C# での LINQ)
description: C# でカスタムの LINQ 結合操作を実行する方法について説明します。
ms.date: 12/01/2016
ms.assetid: 56a2a4a5-7299-497d-b3c3-23c848678911
ms.openlocfilehash: 7051007c67bd64cd11ede2f4d5352ce3d497255f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "61659853"
---
# <a name="perform-custom-join-operations"></a>カスタム結合操作の実行

この例では、`join` 句では実現できない結合操作を実行する方法を示しています。 クエリ式の `join` 句は、最も一般的な種類の結合操作である等結合用に限定され、また、それを目的に最適化されています。 等結合の実行時には、`join` 句を使用することで、ほとんどの場合に最適なパフォーマンスが得られます。

ただし、`join` 句は、次の場合には使用できません。

- 結合が非等値の式に基づいている場合 (非等結合)。

- 結合が等値または非等値の複数の式に基づいている場合。

- 結合操作の前に、右辺 (内部) のシーケンスに一時的な範囲変数を導入する必要がある場合。

 等結合ではない結合を実行するには、複数の `from` 句を使用して、各データ ソースを個別に導入することができます。 その後、`where` 句の述語式を各ソースの範囲変数に適用します。 式は、メソッド呼び出しの形式にすることもできます。

> [!NOTE]
> このようなカスタム結合操作を、複数の `from` 句を使用した内部コレクションへのアクセスと混同しないでください。 詳しくは、「[join 句](../language-reference/keywords/join-clause.md)」をご覧ください。

## <a name="example"></a>例

次の例の最初のメソッドは、単純なクロス結合を示しています。 クロス結合は、非常に大きな結果セットを生成することがあるので、注意して使用する必要があります。 ただし、追加のクエリの実行対象となるソース シーケンスを作成するためのいくつかのシナリオでは便利な場合があります。

2 番目のメソッドは、左辺のカテゴリの一覧にカテゴリ ID が含まれているすべての製品のシーケンスを生成します。 `let` 句と `Contains` メソッドを使用して一時配列を作成していることに注意してください。 クエリの前に配列を作成し、最初の `from` 句を削除することもできます。

[!code-csharp[csProgGuideLINQ#64](~/samples/snippets/csharp/concepts/linq/how-to-perform-custom-join-operations_1.cs)]

## <a name="example"></a>例

次の例では、クエリは一致するキーに基づいて 2 つのシーケンスを結合する必要があります。内部 (右辺) シーケンスでは、join 句自体より前にキーを取得することはできません。 この結合が `join` 句を使用して実行された場合は、要素ごとに `Split` メソッドを呼び出す必要があります。 複数の `from` 句を使用すると、クエリは、メソッドを繰り返し呼び出すことのオーバーヘッドを回避することができます。 ただし、`join` は最適化されるため、この特定の場合には、複数の `from` 句を使用するよりも処理が速くなることがあります。 結果は、主に、メソッド呼び出しにかかる負荷に応じて異なります。

[!code-csharp[csProgGuideLINQ#13](~/samples/snippets/csharp/concepts/linq/how-to-perform-custom-join-operations_2.cs)]

## <a name="see-also"></a>参照

- [統合言語クエリ (LINQ)](index.md)
- [join 句](../language-reference/keywords/join-clause.md)
- [join 句の結果の順序指定](order-the-results-of-a-join-clause.md)
