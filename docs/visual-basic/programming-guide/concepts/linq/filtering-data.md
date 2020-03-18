---
title: データのフィルター処理
ms.date: 07/20/2015
ms.assetid: 7749519a-7edc-49fe-aef9-6a353864af6c
ms.openlocfilehash: 81e207e451055fb2952e4bf393db067f0851afb4
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353494"
---
# <a name="filtering-data-visual-basic"></a>データのフィルター処理 (Visual Basic)

フィルター処理とは、特定の条件を満たす要素のみが含まれるように結果セットを限定する操作のことです。 選択とも呼ばれます。

次の図は、文字のシーケンスをフィルター処理した結果を示したものです。 フィルター処理操作の述語では、文字が "A" でなければならないことが指定されています。

![LINQ のフィルター操作を示す図。](./media/filtering-data/linq-filter-operation.png)

次のセクションでは、選択を実行する標準クエリ演算子メソッドの一覧を示します。

## <a name="methods"></a>メソッド

|メソッド名|説明|Visual Basic クエリ式の構文|詳細|
|-----------------|-----------------|------------------------------------------|----------------------|
|OfType|指定した型にキャストできるかどうかにより、値を選択します。|該当しない。|<xref:System.Linq.Enumerable.OfType%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.OfType%2A?displayProperty=nameWithType>|
|場所|述語関数に基づいて値を選択します。|`Where`|<xref:System.Linq.Enumerable.Where%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Where%2A?displayProperty=nameWithType>|

## <a name="query-expression-syntax-example"></a>クエリ式の構文例

次の例では、`Where` を使用して、特定の長さを持つ文字列を配列からフィルター処理します。

```vb
Dim words() As String = {"the", "quick", "brown", "fox", "jumps"}

Dim query = From word In words
            Where word.Length = 3
            Select word

Dim sb As New System.Text.StringBuilder()
For Each str As String In query
    sb.AppendLine(str)
Next

' Display the results.
MsgBox(sb.ToString())

' This code produces the following output:

' the
' fox
```

## <a name="see-also"></a>参照

- <xref:System.Linq>
- [標準クエリ演算子の概要 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/standard-query-operators-overview.md)
- [WHERE 句](../../../../visual-basic/language-reference/queries/where-clause.md)
- [方法 : クエリ結果のフィルター処理](../../../../visual-basic/programming-guide/language-features/linq/how-to-filter-query-results-by-using-linq.md)
- [方法: リフレクションを使用してアセンブリのメタデータを照会する (LINQ) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-query-an-assembly-s-metadata-with-reflection-linq.md)
- [方法: 指定された属性または名前のファイルを照会する (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-query-for-files-with-a-specified-attribute-or-name.md)
- [方法: 任意の単語またはフィールドを基準にテキストデータの並べ替えまたはフィルター処理を実行する (LINQ) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-sort-or-filter-text-data-by-any-word-or-field-linq.md)
