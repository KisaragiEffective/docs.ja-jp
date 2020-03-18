---
title: '方法: 文字列内の文字を照会する (LINQ)'
ms.date: 07/20/2015
ms.assetid: 499ebbe0-746c-4235-9dba-ce722c12b50e
ms.openlocfilehash: 2f306a488610aaa5775210eba3d7312b092545a7
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75345522"
---
# <a name="how-to-query-for-characters-in-a-string-linq-visual-basic"></a>方法: 文字列内の文字を照会する (LINQ) (Visual Basic)

<xref:System.String> クラスはジェネリック <xref:System.Collections.Generic.IEnumerable%601> インターフェイスを実装しているため、任意の文字列を文字のシーケンスとしてクエリできます。 ただし、これは LINQ の一般的な使用方法ではありません。 複雑なパターン一致操作には、<xref:System.Text.RegularExpressions.Regex> クラスを使用してください。

## <a name="example"></a>使用例

次の例では、文字列を対象にクエリを実行して、その文字列に含まれる数字の数を特定します。 クエリは、最初に使用された後も "再利用" されます。 これができるのは、クエリ自体には実際の結果が格納されないためです。

```vb
Class QueryAString

    Shared Sub Main()

        ' A string is an IEnumerable data source.
        Dim aString As String = "ABCDE99F-J74-12-89A"

        ' Select only those characters that are numbers
        Dim stringQuery = From ch In aString
                          Where Char.IsDigit(ch)
                          Select ch
        ' Execute the query
        For Each c As Char In stringQuery
            Console.Write(c & " ")
        Next

        ' Call the Count method on the existing query.
        Dim count As Integer = stringQuery.Count()
        Console.WriteLine(System.Environment.NewLine & "Count = " & count)

        ' Select all characters before the first '-'
        Dim stringQuery2 = aString.TakeWhile(Function(c) c <> "-")

        ' Execute the second query
        For Each ch In stringQuery2
            Console.Write(ch)
        Next

        Console.WriteLine(System.Environment.NewLine & "Press any key to exit")
        Console.ReadKey()
    End Sub
End Class
' Output:
' 9 9 7 4 1 2 8 9
' Count = 8
' ABCDE99F
```

## <a name="compile-the-code"></a>コードのコンパイル

System. Linq 名前空間の `Imports` ステートメントを使用して、Visual Basic コンソールアプリケーションプロジェクトを作成します。

## <a name="see-also"></a>関連項目

- [LINQ と文字列 (Visual Basic)](linq-and-strings.md)
- [LINQ クエリと正規表現を組み合わせる方法 (Visual Basic)](how-to-combine-linq-queries-with-regular-expressions.md)
