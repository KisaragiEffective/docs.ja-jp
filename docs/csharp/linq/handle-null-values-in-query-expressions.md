---
title: クエリ式の null 値の処理 (C# での LINQ)
description: C# の LINQ クエリ式で null 値を処理する方法について説明します。
ms.date: 12/01/2016
ms.assetid: ac63ae8b-724d-4251-9334-528f4e884ae7
ms.openlocfilehash: c9a3aaec05fa029a8db66826bdcb4a1d106176e3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73736858"
---
# <a name="handle-null-values-in-query-expressions"></a>クエリ式の null 値の処理

この例では、ソース コレクション内の可能な null 値を処理する方法を示します。 <xref:System.Collections.Generic.IEnumerable%601> のようなオブジェクト コレクションには、値が [null](../language-reference/keywords/null.md) の要素を含めることができます。 ソース コレクションが null であるか null の値を持つ要素を含み、クエリで null 値を処理しない場合、クエリを実行すると <xref:System.NullReferenceException> がスローされます。

## <a name="example"></a>例

次の例に示すように、null 参照の例外を回避する防御的なコーディングをすることができます。

[!code-csharp[csProgGuideLINQ#82](~/samples/snippets/csharp/concepts/linq/how-to-handle-null-values-in-query-expressions_1.cs)]

前の例では、`where` 句によって、カテゴリ シーケンス内のすべての null 要素が除外されます。 この手法は、join 句での null チェックに依存しません。 この例の null 条件式が機能する理由は、`Products.CategoryID` が `int?` 型 (`Nullable<int>` の短縮形) であるためです。

## <a name="example"></a>例

join 句で、比較キーの一方だけが null 許容値型である場合、クエリ式でもう一方のキーを null 許容型にキャストできます。 次の例では、`EmployeeID` は `int?` 型の値を含む列であるとします。

[!code-csharp[csProgGuideLINQ#83](~/samples/snippets/csharp/concepts/linq/how-to-handle-null-values-in-query-expressions_2.cs)]

## <a name="see-also"></a>参照

- <xref:System.Nullable%601>
- [統合言語クエリ (LINQ)](index.md)
- [null 許容値型](../language-reference/builtin-types/nullable-value-types.md)
