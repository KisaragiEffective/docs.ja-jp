---
title: クエリ結果をメモリに格納する
description: 結果の格納方法。
ms.date: 11/30/2016
ms.assetid: 5b863961-1750-4cf9-9607-acea5054d15a
ms.openlocfilehash: 66a7a95c74db4062e76c54d4339ccb7343f44067
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "65633563"
---
# <a name="store-the-results-of-a-query-in-memory"></a>クエリ結果をメモリに格納する

クエリは、基本的に、データの取得方法と編成方法を指示するための一連の命令です。 結果の各項目は順次要求されるため、クエリは遅延実行されます。 `foreach` を使用して結果を反復すると、項目はアクセスのたびに返されます。 クエリを評価し、`foreach` のループを実行せずに結果を格納するには、クエリ変数で次のメソッドのいずれかを呼び出します。

- <xref:System.Linq.Enumerable.ToList%2A>

- <xref:System.Linq.Enumerable.ToArray%2A>

- <xref:System.Linq.Enumerable.ToDictionary%2A>

- <xref:System.Linq.Enumerable.ToLookup%2A>

 クエリ結果を格納するときは、次の例に示すように、返されたコレクション オブジェクトを新しい変数に割り当てることをお勧めします。

## <a name="example"></a>例

[!code-csharp[csProgGuideLINQ#25](~/samples/snippets/csharp/concepts/linq/how-to-store-the-results-of-a-query-in-memory_1.cs)]

## <a name="see-also"></a>参照

- [統合言語クエリ (LINQ)](index.md)
