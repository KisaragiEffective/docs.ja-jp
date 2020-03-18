---
title: 入れ子になったグループの作成 (C# での LINQ)
description: C# で LINQ クエリ式に入れ子になったグループを作成する方法について説明します。
ms.date: 12/01/2016
ms.assetid: e9f00708-362e-4d13-98c5-d77549347ba0
ms.openlocfilehash: 7d056c9e215ccc7ca24d621b64e2328bed79f22e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "61688619"
---
# <a name="create-a-nested-group"></a>入れ子になったグループの作成

LINQ クエリ式で入れ子になったグループを作成する方法を次の例に示します。 学年レベルに基づいて作成した各グループを、さらに個人の名前に基づくグループに分割します。

## <a name="example"></a>例

> [!NOTE]
> この例には、「[オブジェクトのコレクションの照会](query-a-collection-of-objects.md)」のサンプル コードで定義されているオブジェクトへの参照が含まれています。

[!code-csharp[csProgGuideLINQ#24](~/samples/snippets/csharp/concepts/linq/how-to-create-a-nested-group_1.cs)]

入れ子になったグループの内部要素に対して反復処理を実行するために、入れ子になった `foreach` ループが 3 つ必要であることに注意してください。

## <a name="see-also"></a>参照

- [統合言語クエリ (LINQ)](index.md)
