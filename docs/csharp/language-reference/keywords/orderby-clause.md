---
title: orderby 句 - C# リファレンス
ms.date: 07/20/2015
f1_keywords:
- orderby
- orderby_CSharpKeyword
helpviewer_keywords:
- orderby clause [C#]
- orderby keyword [C#]
ms.assetid: 21f87f48-d69d-4e95-9a52-6fec47b37e1f
ms.openlocfilehash: cd76b2c33fe1a1a986bc05e3c3ed5f22809686ed
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173576"
---
# <a name="orderby-clause-c-reference"></a>orderby 句 (C# リファレンス)

クエリ式では、`orderby` 句によって返されるシーケンスまたはサブシーケンス (グループ) が昇順または降順で並べ替えられます。 2 番目の並べ替え操作を 1 つ以上実行するために、複数のキーを指定できます。 並べ替えは、要素の型の既定の比較演算子によって実行されます。 既定の並べ替え順序は昇順です。 カスタムの比較演算子を指定することもできます。 ただしこれは、メソッド ベースの構文を使用する場合にのみ使用できます。 詳細については、「[Sorting Data](../../programming-guide/concepts/linq/sorting-data.md)」(データの並べ替え) を参照してください。

## <a name="example"></a>例

次の例では、最初のクエリが A から始まるアルファベット順で単語を並べ替え、2 番目のクエリが同じ単語を降順で並べ替えます (`ascending` キーワードは、既定の並べ替え値で、省略可能です)。

[!code-csharp[cscsrefQueryKeywords#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Orderby.cs#20)]

## <a name="example"></a>例

次の例では、学生の姓で最初の並べ替えを実行し、学生の名で 2 番目の並べ替えを実行します。

[!code-csharp[cscsrefQueryKeywords#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Orderby.cs#22)]

## <a name="remarks"></a>解説

コンパイル時に、`orderby` 句は <xref:System.Linq.Enumerable.OrderBy%2A> メソッドの呼び出しに変換されます。 `orderby` 句内の複数のキーは、<xref:System.Linq.Enumerable.ThenBy%2A> メソッドの呼び出しに変換されます。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [クエリ キーワード (LINQ)](query-keywords.md)
- [C# での LINQ](../../linq/index.md)
- [group 句](group-clause.md)
- [統合言語クエリ (LINQ)](../../programming-guide/concepts/linq/index.md)
