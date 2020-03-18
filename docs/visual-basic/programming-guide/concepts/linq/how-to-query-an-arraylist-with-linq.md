---
title: '方法 : LINQ を使用して ArrayList を照会する'
ms.date: 07/20/2015
ms.assetid: 176358a9-d765-4b57-9557-7feb4428138d
ms.openlocfilehash: 94a3c6d4c381f41f9ba87bf3af93261712ad1136
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347756"
---
# <a name="how-to-query-an-arraylist-with-linq-visual-basic"></a>方法: LINQ を使用して ArrayList を照会する (Visual Basic)

LINQ を使用して <xref:System.Collections.IEnumerable> などの非ジェネリックの <xref:System.Collections.ArrayList> コレクションをクエリする場合、範囲変数の型を明示的に宣言して、オブジェクトの特定の型をコレクションに反映させる必要があります。 たとえば、`Student` オブジェクトの <xref:System.Collections.ArrayList> がある場合、 [From 句](../../../../visual-basic/language-reference/queries/from-clause.md)は次のようになります。

```vb
Dim query = From student As Student In arrList
'...
```

範囲変数の型を指定することで、<xref:System.Collections.ArrayList> 内の各項目を `Student` にキャストします。

明示的に型指定された範囲変数をクエリ式で使用すると、<xref:System.Linq.Enumerable.Cast%2A> メソッドを呼び出した場合と同じ結果を得ることができます。 指定したキャストを実行できない場合、<xref:System.Linq.Enumerable.Cast%2A> は例外をスローします。 <xref:System.Linq.Enumerable.Cast%2A> および <xref:System.Linq.Enumerable.OfType%2A> は、非ジェネリックの <xref:System.Collections.IEnumerable> 型で動作する、2 つの標準クエリ演算子メソッドです。 Visual Basic では、データソースの <xref:System.Linq.Enumerable.Cast%2A> メソッドを明示的に呼び出して、特定の範囲変数の型を確認する必要があります。 詳細については、「[クエリ操作での型の関係 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/type-relationships-in-query-operations.md)」を参照してください。

## <a name="example"></a>例

次の例では、<xref:System.Collections.ArrayList> に対して単純なクエリを実行しています。 この例では、コードが <xref:System.Collections.ArrayList.Add%2A> メソッドを呼び出すときにオブジェクト初期化子を使用していますが、これは必須ではありません。

```vb
Imports System.Collections
Imports System.Linq

Module Module1

    Public Class Student
        Public Property FirstName As String
        Public Property LastName As String
        Public Property Scores As Integer()
    End Class

    Sub Main()

        Dim student1 As New Student With {.FirstName = "Svetlana",
                                     .LastName = "Omelchenko",
                                     .Scores = New Integer() {98, 92, 81, 60}}
        Dim student2 As New Student With {.FirstName = "Claire",
                                    .LastName = "O'Donnell",
                                    .Scores = New Integer() {75, 84, 91, 39}}
        Dim student3 As New Student With {.FirstName = "Cesar",
                                    .LastName = "Garcia",
                                    .Scores = New Integer() {97, 89, 85, 82}}
        Dim student4 As New Student With {.FirstName = "Sven",
                                    .LastName = "Mortensen",
                                    .Scores = New Integer() {88, 94, 65, 91}}

        Dim arrList As New ArrayList()
        arrList.Add(student1)
        arrList.Add(student2)
        arrList.Add(student3)
        arrList.Add(student4)

        ' Use an explicit type for non-generic collections
        Dim query = From student As Student In arrList
                    Where student.Scores(0) > 95
                    Select student

        For Each student As Student In query
            Console.WriteLine(student.LastName & ": " & student.Scores(0))
        Next
        ' Keep the console window open in debug mode.
        Console.WriteLine("Press any key to exit.")
        Console.ReadKey()
    End Sub

End Module
' Output:
'   Omelchenko: 98
'   Garcia: 97
```

## <a name="see-also"></a>参照

- [LINQ to Objects (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md)
