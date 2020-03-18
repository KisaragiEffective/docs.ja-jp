---
title: into - C# リファレンス
ms.date: 07/20/2015
f1_keywords:
- into_CSharpKeyword
- into
helpviewer_keywords:
- into keyword [C#]
ms.assetid: 81ec62c1-f0b1-4755-8a31-959876e77f65
ms.openlocfilehash: f0f5ff1e56a65e83385f814df2fadd957f53561e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713622"
---
# <a name="into-c-reference"></a>into (C# リファレンス)

`into` コンテキスト キーワードは、[group](group-clause.md)、[join](join-clause.md)、または [select](select-clause.md) 句の結果を新しい識別子に保存するための一時的な識別子を作成するために使用できます。 この識別子自体を追加のクエリ コマンドのジェネレーターにすることができます。 `group` または `select` 句で使用する場合、新しい識別子の使用は*継続*と呼ばれることもあります。

## <a name="example"></a>例

次の例では、`into` キーワードを使用して、推定の型 `fruitGroup` を持つ一時的な識別子 `IGrouping` を有効にする方法を示します。 識別子を使用すると、各グループに対して <xref:System.Linq.Enumerable.Count%2A> メソッドを呼び出し、2 つ以上の単語を含むグループのみを選択することができます。

[!code-csharp[cscsrefQueryKeywords#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Into.cs#18)]

`into` 句での `group` の使用は、各グループに対して追加のクエリ操作を実行する場合にのみ必要です。 詳細については、「[group 句](group-clause.md)」を参照してください。

`into` 句での `join` の使用例については、「[join 句](join-clause.md)」を参照してください。

## <a name="see-also"></a>参照

- [クエリ キーワード (LINQ)](query-keywords.md)
- [C# での LINQ](../../linq/index.md)
- [group 句](group-clause.md)
