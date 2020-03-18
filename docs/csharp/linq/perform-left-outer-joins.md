---
title: 左外部結合の実行 (C# での LINQ)
description: C# で LINQ を使用して、左外部結合を実行する方法について説明します。
ms.date: 12/01/2016
ms.assetid: f542cee6-3169-4dcf-a631-3a6a79ccd473
ms.openlocfilehash: cc08a1c8670623a10d1e0bf10221d02037a8d7bc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "61688580"
---
# <a name="perform-left-outer-joins"></a>左外部結合の実行

左外部結合は、最初のコレクションの各要素を、2 つ目のコレクション内にある要素との相関関係の有無にかかわらず返す結合です。 LINQ を使用すると、グループ結合の結果に対して <xref:System.Linq.Enumerable.DefaultIfEmpty%2A> メソッドを呼び出すことで、左外部結合を実行できます。

## <a name="example"></a>例

次の例は、グループ結合の結果に対して <xref:System.Linq.Enumerable.DefaultIfEmpty%2A> メソッドを使用し、左外部結合を実行する方法を示しています。

2 つのコレクションの左外部結合を作成するための最初のステップは、グループ結合を使用して内部結合を実行することです。 (このプロセスの詳細については、「[内部結合の実行](perform-inner-joins.md)」参照してください。)この例では、`Pet.Owner` に一致する `Person` オブジェクトに基づいて、`Person` オブジェクトの一覧が `Pet` オブジェクトの一覧に内部結合されています。

2 つ目のステップは、最初 (左側) のコレクションの各要素を結果セットに含めることです。このとき、その要素と一致するものが右のコレクションにあるかどうかは考慮しません。 これを行うには、グループ結合内の一致する要素の各シーケンスに対して、<xref:System.Linq.Enumerable.DefaultIfEmpty%2A> を呼び出します。 この例では、`Pet` オブジェクトに一致する各シーケンスに対して、<xref:System.Linq.Enumerable.DefaultIfEmpty%2A> が呼び出されています。 このメソッドは、`Person` オブジェクトに対して一致する `Pet` オブジェクトのシーケンスが空である場合に、単一の既定値を含んだコレクションを返します。これにより、各 `Person` オブジェクトが結果コレクション内に表されることが保証されます。

> [!NOTE]
> 参照型の既定値は `null` です。そのためこのコード例では、各 `Pet` コレクションの各要素にアクセスする前に Null 参照がチェックされます。

[!code-csharp[CsLINQProgJoining#7](~/samples/snippets/csharp/concepts/linq/how-to-perform-left-outer-joins_1.cs)]

## <a name="see-also"></a>関連項目

- <xref:System.Linq.Enumerable.Join%2A>
- <xref:System.Linq.Enumerable.GroupJoin%2A>
- [内部結合の実行](perform-inner-joins.md)
- [グループ化結合の実行](perform-grouped-joins.md)
- [匿名型](../programming-guide/classes-and-structs/anonymous-types.md)
