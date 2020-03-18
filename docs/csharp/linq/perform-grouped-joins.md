---
title: グループ結合の実行 (C# での LINQ)
description: C# で LINQ を使用して、グループ結合を実行する方法について説明します。
ms.date: 12/01/2016
ms.assetid: 9667daf9-a5fd-4b43-a5c4-a9c2b744000e
ms.openlocfilehash: dfb75b55336d8ca486d5f10b187e955d20cd06fd
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "61689139"
---
# <a name="perform-grouped-joins"></a>グループ結合の実行

グループ結合は、階層データ構造を作成する場合に便利です。 これは、最初のコレクションの各要素と、2 番目のコレクションの相関関係を持つ要素のセットを組み合わせたものです。

たとえば、`Student` という名前のクラスまたはリレーショナル データベース テーブルに、`Id` と `Name` という 2 つのフィールドが含まれているとします。 `Course` という名前の 2 番目のクラスまたはリレーショナル データベース テーブルには、`StudentId` と `CourseTitle` という 2 つのフィールドが含まれているとします。 この 2 つのデータ ソースのグループ結合は、`Student.Id` と `Course.StudentId` の一致に基づいて、`Student` オブジェクトのコレクションを持つ各 `Course` をグループ化します (コレクションは空の場合もあります)。

> [!NOTE]
> 最初のコレクションの各要素は、2 番目のコレクションに相関関係を持つ要素があるかどうかにかかわらず、グループ結合の結果セットに表示されます。 相関関係を持つ要素が見つからない場合、その要素の相関関係を持つ要素のシーケンスは空です。 そのため、結果セレクターは最初のコレクションのすべての要素にアクセスできます。 これは、非グループ結合の結果セレクターとは異なります。非グループ結合の結果セレクターは、2 番目のコレクションに一致するものがない最初のコレクションの要素にアクセスすることはできません。

この記事の最初の例では、グループ結合を実行する方法を示します。 2 つ目の例では、グループ結合を使用して XML 要素を作成する方法を示します。

## <a name="example---group-join"></a>例 - グループ結合

次の例では、`Person` プロパティと一致する `Pet` に基づいて、`Person` 型と `Pet.Owner` 型のオブジェクトのグループ結合を実行します。 一致ごとに要素のペアを生成する非グループ結合と異なり、グループ結合は最初のコレクションの要素ごとに 1 つのオブジェクト (この例では `Person` オブジェクト) のみを作成します。 2 番目のコレクションの対応する要素 (この例では `Pet` オブジェクト) が 1 つのコレクションにグループ化されます。 最後に、結果セレクター機能により、`Person.FirstName` と、`Pet` オブジェクトのコレクションで構成される一致ごとに匿名型が作成されます。

[!code-csharp[CsLINQProgJoining#5](~/samples/snippets/csharp/concepts/linq/how-to-perform-grouped-joins_1.cs)]

## <a name="example---group-join-to-create-xml"></a>例 - XML を作成するグループ結合

グループ結合は、LINQ to XML を使用した XML の作成に適しています。 次の例は前の例に似ていますが、匿名型を作成するのではなく、結果セレクター機能により、結合されたオブジェクトを表す XML 要素を作成する点が異なります。

[!code-csharp[CsLINQProgJoining#6](~/samples/snippets/csharp/concepts/linq/how-to-perform-grouped-joins_2.cs)]

## <a name="see-also"></a>参照

- <xref:System.Linq.Enumerable.Join%2A>
- <xref:System.Linq.Enumerable.GroupJoin%2A>
- [内部結合の実行](perform-inner-joins.md)
- [左外部結合の実行](perform-left-outer-joins.md)
- [匿名型](../programming-guide/classes-and-structs/anonymous-types.md)
