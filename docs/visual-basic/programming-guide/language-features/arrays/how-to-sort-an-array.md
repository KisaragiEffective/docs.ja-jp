---
title: '方法: 配列を並べ替える'
ms.date: 07/20/2015
f1_keywords:
- Array.Sort
helpviewer_keywords:
- arrays [Visual Basic], sorting
- examples [Visual Basic], arrays
ms.assetid: 9289aeaa-9626-4698-94a7-1d1fd3702b87
ms.openlocfilehash: 3fb9af8de0fc86075fdccd64506c855c1c720660
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351852"
---
# <a name="how-to-sort-an-array-in-visual-basic"></a>方法: Visual Basic で配列を並べ替える

この記事では、Visual Basic で文字列の配列を並べ替える方法の例を示します。

## <a name="example"></a>例

この例では、`zooAnimals`という名前の `String` オブジェクトの配列を宣言し、それを設定して、アルファベット順に並べ替えます。
  
```vb
Private Sub SortAnimals()
    Dim zooAnimals(2) As String
    zooAnimals(0) = "lion"
    zooAnimals(1) = "turtle"
    zooAnimals(2) = "ostrich"
    Array.Sort(zooAnimals)
End Sub
```

## <a name="robust-programming"></a>信頼性の高いプログラミング

次の条件を満たす場合は、例外が発生する可能性があります。

- 配列が空です (<xref:System.ArgumentNullException> クラス)。
- 配列は多次元 (<xref:System.RankException> クラス) です。
- 配列の1つ以上の要素が <xref:System.IComparable> インターフェイス (<xref:System.InvalidOperationException> クラス) を実装していません。

## <a name="see-also"></a>参照

- <xref:System.Array.Sort%2A?displayProperty=nameWithType>
- [配列](index.md)
- [配列のトラブルシューティング](troubleshooting-arrays.md)
- [コレクション](../../concepts/collections.md)
- [For Each...Next ステートメント](../../../language-reference/statements/for-each-next-statement.md)
