---
title: '方法: ForEach を使用して BlockingCollection 内の項目を削除する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- thread-safe collections, how to enumerate blocking collection
ms.assetid: 2096103c-22f7-420d-b631-f102bc33a6dd
ms.openlocfilehash: f9a858dea74be63634f887c4204aefa8ac338ad0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75711234"
---
# <a name="how-to-use-foreach-to-remove-items-in-a-blockingcollection"></a>方法: ForEach を使用して BlockingCollection 内の項目を削除する

<xref:System.Collections.Concurrent.BlockingCollection%601> と <xref:System.Collections.Concurrent.BlockingCollection%601.Take%2A> メソッドを使用して <xref:System.Collections.Concurrent.BlockingCollection%601.TryTake%2A> から項目を取得するだけでなく、[foreach](../../../csharp/language-reference/keywords/foreach-in.md) (Visual Basic では [For Each](../../../visual-basic/language-reference/statements/for-each-next-statement.md)) を使用して、追加が完了してコレクションが空になるまで項目を削除することもできます。 これは、通常の  *(* ) ループとは異なり、この列挙子が項目を削除することでソース コレクションを変更するので、*変更列挙*または`foreach`消費列挙`For Each`と呼ばれます。

## <a name="example"></a>例

次の例は、<xref:System.Collections.Concurrent.BlockingCollection%601> (`foreach`) ループを使用して、`For Each` 内のすべての項目を削除する方法を示しています。

[!code-csharp[CDS_BlockingCollection#03](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/example03.cs#03)]
[!code-vb[CDS_BlockingCollection#03](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/enumeratebc.vb#03)]

この例では、消費スレッドの `foreach` メソッドで <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A?displayProperty=nameWithType> ループを使用します。各項目は列挙されるとコレクションから削除されます。 <xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType> はコレクション内の項目の最大数を制限します。 この方法でコレクションを列挙すると、使用できる項目がない場合、またはコレクションが空の場合は、コンシューマー スレッドがブロックされます。 この例では、プロデューサー スレッドによる項目の追加の方が項目の消費より高速なので、ブロックは問題になりません。

プロデューサー スレッドによって追加されたのと同じ順序で、項目が列挙されるという保証はありません。

コレクションを変更せずに列挙するには、`foreach` メソッドを使用せずに `For Each` (<xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A>) のみを使用します。 ただし、この種の列挙は特定時点のコレクションのスナップショットを表すことを理解しておくことが重要です。 ループの実行中に同時に他のスレッドが項目を追加または削除する場合、ループはコレクションの実際の状態を表さない可能性があります。

## <a name="see-also"></a>参照

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [並列プログラミング](../../../../docs/standard/parallel-programming/index.md)
