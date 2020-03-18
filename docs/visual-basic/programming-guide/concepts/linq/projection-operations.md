---
title: 射影操作
ms.date: 07/20/2015
ms.assetid: b8d38e6d-21cf-4619-8dbb-94476f4badc7
ms.openlocfilehash: d7efb46ccfe3208ae6c58043a64c236171d0c147
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346628"
---
# <a name="projection-operations-visual-basic"></a>プロジェクション操作 (Visual Basic)

射影とは、オブジェクトを、必要なプロパティだけで構成された別の形式に変換する操作のことをいいます。 射影を使用することにより、個々のオブジェクトから構築された新しい型を作成できます。 プロパティを投影し、それに対して数値演算関数を実行できます。 また、元のオブジェクトを変更せずに射影することもできます。

次のセクションでは、射影を実行する標準クエリ演算子メソッドの一覧を示します。

## <a name="methods"></a>メソッド

|メソッド名|説明|Visual Basic クエリ式の構文|詳細|
|-----------------|-----------------|------------------------------------------|----------------------|
|[選択]|変換関数に基づいて値を射影します。|`Select`|<xref:System.Linq.Enumerable.Select%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Select%2A?displayProperty=nameWithType>|
|SelectMany|変換関数に基づいて値のシーケンスを射影し、それを 1 つのシーケンスに平坦化します。|複数の `From` 句を使用|<xref:System.Linq.Enumerable.SelectMany%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.SelectMany%2A?displayProperty=nameWithType>|

## <a name="query-expression-syntax-examples"></a>クエリ式の構文例

### <a name="select"></a>[選択]

次の例では、`Select` 句を使って、文字列リストにある各文字列の最初の文字を射影します。

```vb
Dim words = New List(Of String) From {"an", "apple", "a", "day"}

Dim query = From word In words
            Select word.Substring(0, 1)

Dim sb As New System.Text.StringBuilder()
For Each letter As String In query
    sb.AppendLine(letter)
Next

' Display the output.
MsgBox(sb.ToString())

' This code produces the following output:

' a
' a
' a
' d
```

### <a name="selectmany"></a>SelectMany

次の例では、複数の `From` 句を使用して、文字列のリスト内の各文字列の各単語を射影します。

```vb
Dim phrases = New List(Of String) From {"an apple a day", "the quick brown fox"}

Dim query = From phrase In phrases
            From word In phrase.Split(" "c)
            Select word

Dim sb As New System.Text.StringBuilder()
For Each str As String In query
    sb.AppendLine(str)
Next

' Display the output.
MsgBox(sb.ToString())

' This code produces the following output:

' an
' apple
' a
' day
' the
' quick
' brown
' fox
```

## <a name="select-versus-selectmany"></a>Select と SelectMany の比較

`Select()` と `SelectMany()` の機能はどちらも、ソース値から結果値 (複数も可) を生成することです。 `Select()` は、ソース値ごとに結果値を 1 つ生成します。 そのため、結果全体は、ソース コレクションと同じ数の要素を持つ 1 つのコレクションになります。 これに対し、`SelectMany()` は、各ソース値から、連結されたサブコレクションを含む 1 つの総合的な結果を生成します。 `SelectMany()` に引数として渡される変換関数は、ソース値ごとに列挙可能な値のシーケンスを返す必要があります。 この列挙可能なシーケンスは `SelectMany()` によって連結されて、1 つの大きなシーケンスが作成されます。

これら 2 つのメソッドのアクションの概念的な違いを次の 2 つの図に示します。 どちらも、セレクター (変換) 関数が各ソース値から花の配列を選択することを想定しています。

次の図は、`Select()` がソース コレクションと同じ数の要素を持つコレクションを返すしくみを示しています。

![Select&#40;&#41; のアクションを示すグラフィック。](./media/projection-operations/select-action-graphic.png)

次の図は、`SelectMany()` が中間配列シーケンスを、各中間配列の値を含む最終的な結果値に連結するしくみを示しています。

![SelectMany&#40;&#41; のアクションを示すグラフィック。](./media/projection-operations/select-many-action-graphic.png )

### <a name="code-example"></a>コード例

次の例は、`Select()` と `SelectMany()` の動作を比較しています。 コードは、ソース コレクションの花の名前の各リストから最初の 2 つの項目を取って "花束" を作成します。 この例では、変換関数 <xref:System.Linq.Enumerable.Select%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%29> が使用する "単一の値" 自体が値のコレクションになっています。 各サブ シーケンスで各文字列を列挙するために追加の `For Each` ループを使用しています。

```vb
Class Bouquet
    Public Flowers As List(Of String)
End Class

Sub SelectVsSelectMany()
    Dim bouquets = New List(Of Bouquet) From {
        New Bouquet With {.Flowers = New List(Of String)(New String() {"sunflower", "daisy", "daffodil", "larkspur"})},
        New Bouquet With {.Flowers = New List(Of String)(New String() {"tulip", "rose", "orchid"})},
        New Bouquet With {.Flowers = New List(Of String)(New String() {"gladiolis", "lily", "snapdragon", "aster", "protea"})},
        New Bouquet With {.Flowers = New List(Of String)(New String() {"larkspur", "lilac", "iris", "dahlia"})}}

    Dim output As New System.Text.StringBuilder

    ' Select()
    Dim query1 = bouquets.Select(Function(b) b.Flowers)

    output.AppendLine("Using Select():")
    For Each flowerList In query1
        For Each str As String In flowerList
            output.AppendLine(str)
        Next
    Next

    ' SelectMany()
    Dim query2 = bouquets.SelectMany(Function(b) b.Flowers)

    output.AppendLine(vbCrLf & "Using SelectMany():")
    For Each str As String In query2
        output.AppendLine(str)
    Next

    ' Display the output
    MsgBox(output.ToString())

    ' This code produces the following output:
    '
    ' Using Select():
    ' sunflower
    ' daisy
    ' daffodil
    ' larkspur
    ' tulip
    ' rose
    ' orchid
    ' gladiolis
    ' lily
    ' snapdragon
    ' aster
    ' protea
    ' larkspur
    ' lilac
    ' iris
    ' dahlia

    ' Using SelectMany()
    ' sunflower
    ' daisy
    ' daffodil
    ' larkspur
    ' tulip
    ' rose
    ' orchid
    ' gladiolis
    ' lily
    ' snapdragon
    ' aster
    ' protea
    ' larkspur
    ' lilac
    ' iris
    ' dahlia

End Sub
```

## <a name="see-also"></a>参照

- <xref:System.Linq>
- [標準クエリ演算子の概要 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/standard-query-operators-overview.md)
- [Select 句](../../../../visual-basic/language-reference/queries/select-clause.md)
- [方法 : 結合を使用したデータの結合](../../../../visual-basic/programming-guide/language-features/linq/how-to-combine-data-with-linq-by-using-joins.md)
- [方法: 複数のソースからオブジェクトコレクションにデータを設定する (LINQ) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-populate-object-collections-from-multiple-sources-linq.md)
- [方法 : 特定の型での LINQ クエリ結果の取得](../../../../visual-basic/programming-guide/language-features/linq/how-to-return-a-linq-query-result-as-a-specific-type.md)
- [方法: グループを使用してファイルを複数のファイルに分割する (LINQ) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-split-a-file-into-many-files-by-using-groups-linq.md)
