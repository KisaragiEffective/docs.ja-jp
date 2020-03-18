---
title: グループ化操作でのサブクエリの実行 (C# での LINQ)
description: C# で LINQ を使用して、グループ化操作でサブクエリを実行する方法について説明します。
ms.date: 12/01/2016
ms.assetid: d75a588e-9b6f-4f37-b195-f99ec8503855
ms.openlocfilehash: fd26f87ad7d5b4892f086bf8c7a34cf19a7f9e02
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173368"
---
# <a name="perform-a-subquery-on-a-grouping-operation"></a>グループ化操作でのサブクエリの実行

この記事では、ソース データを複数のグループに整理し、各グループに対して個別にサブクエリを実行するクエリを作成する方法を 2 つ紹介します。 各例の基本的な手法として、ソース要素のグループ化には、 *という名前の "* 継続`newGroup`" を使用し、`newGroup` に対する新しいサブクエリを生成します。 このサブクエリは、外部クエリによって作成された新しい各グループに対して実行されます。 この例では、最終出力がグループではなく、匿名型のフラットなシーケンスであることに注意してください。  
  
グループ化する方法の詳細については、「[group 句](../language-reference/keywords/group-clause.md)」を参照してください。  
  
継続の詳細については、「[into](../language-reference/keywords/into.md)」を参照してください。 次の例では、インメモリ データ構造をデータ ソースとして使用していますが、どの種類の LINQ データ ソースにも同じ原則が当てはまります。  
  
## <a name="example"></a>例

> [!NOTE]
> この例には、「[オブジェクトのコレクションの照会](query-a-collection-of-objects.md)」のサンプル コードで定義されているオブジェクトへの参照が含まれています。

[!code-csharp[csProgGuideLINQ#23](~/samples/snippets/csharp/concepts/linq/how-to-perform-a-subquery-on-a-grouping-operation_1.cs)]

上記のスニペットのクエリは、メソッド構文を使用して記述することもできます。 次のコード スニペットは、メソッド構文を使用して記述した意味的に同等のクエリです。

[!code-csharp[csProgGuideLINQ#86](~/samples/snippets/csharp/concepts/linq/how-to-perform-a-subquery-on-a-grouping-operation_2.cs)]

## <a name="see-also"></a>参照

- [統合言語クエリ (LINQ)](index.md)
