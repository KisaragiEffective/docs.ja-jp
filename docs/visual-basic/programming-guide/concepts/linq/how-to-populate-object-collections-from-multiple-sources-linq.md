---
title: '方法: 複数のソースからオブジェクト コレクションにデータを設定する (LINQ)'
ms.date: 06/22/2018
ms.assetid: 63062a22-e6a9-42c0-b357-c7c965f58f33
ms.openlocfilehash: 74a2a0f71e575136f1758f72f9a8db72549a9489
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346980"
---
# <a name="how-to-populate-object-collections-from-multiple-sources-linq-visual-basic"></a>方法: 複数のソースからオブジェクトコレクションにデータを設定する (LINQ) (Visual Basic)

この例では、さまざまなソースから一連の新しい型にデータをマージする方法を示します。

> [!NOTE]
> メモリ内データやファイル システム内のデータを、データベース内にあるデータに結合しようとしないでください。 このようなドメイン間結合を行うと、データベース クエリと他の種類のソースとで結合操作の定義方法に違いがあることが原因で、結果が未定義になる可能性があります。 また、データベース内に大量のデータが存在すると、こうした操作によってメモリ不足例外が発生するおそれがあります。 データベースのデータをメモリ内データに結合するには、まずデータベース クエリで `ToList` または `ToArray` を呼び出してから、返されたコレクションで結合を実行します。

## <a name="to-create-the-data-file"></a>データ ファイルを作成するには

- 「[方法: 異種ファイルのコンテンツを結合する (LINQ)」 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-join-content-from-dissimilar-files-linq.md)の説明に従って、.csv ファイルとスコア .csv ファイルをプロジェクトフォルダーにコピーします。

## <a name="example"></a>例

次の例は、2 つのメモリ内文字列コレクションからマージされたデータを、名前付きの型 `Student` を使用して格納する方法を示しています。各コレクションは、.csv 形式のスプレッドシート データをシミュレートしています。 1 つ目の文字列コレクションは学生の名前と ID を表し、2 つ目のコレクションは学生 ID (最初の列) と 4 つの試験の点数を表しています。 外部キーとして ID が使用されます。

```vb
Imports System.Collections.Generic
Imports System.Linq

Class Student
    Public FirstName As String
    Public LastName As String
    Public ID As Integer
    Public ExamScores As List(Of Integer)
End Class

Class PopulateCollection

    Shared Sub Main()

        ' Merge content from spreadsheets into a list of Student objects.

        ' These data files are defined in How to: Join Content from
        ' Dissimilar Files (LINQ).

        ' Each line of names.csv consists of a last name, a first name, and an
        ' ID number, separated by commas. For example, Omelchenko,Svetlana,111
        Dim names As String() = System.IO.File.ReadAllLines("../../../names.csv")

        ' Each line of scores.csv consists of an ID number and four test
        ' scores, separated by commas. For example, 111, 97, 92, 81, 60
        Dim scores As String() = System.IO.File.ReadAllLines("../../../scores.csv")

        ' The following query merges the content of two dissimilar spreadsheets
        ' based on common ID values.
        ' Multiple From clauses are used instead of a Join clause
        ' in order to store the results of scoreLine.Split.
        ' Note the dynamic creation of a list of integers for the
        ' ExamScores member. The first item is skipped in the split string
        ' because it is the student ID, not an exam score.
        Dim queryNamesScores = From nameLine In names
                          Let splitName = nameLine.Split(New Char() {","})
                          From scoreLine In scores
                          Let splitScoreLine = scoreLine.Split(New Char() {","})
                          Where Convert.ToInt32(splitName(2)) = Convert.ToInt32(splitScoreLine(0))
                          Select New Student() With {
                               .FirstName = splitName(1), .LastName = splitName(0), .ID = splitName(2),
                               .ExamScores = (From scoreAsText In splitScoreLine Skip 1
                                             Select Convert.ToInt32(scoreAsText)).ToList()}

        ' Optional. Store the query results for faster access in future
        ' queries. This could be useful with very large data files.
        Dim students As List(Of Student) = queryNamesScores.ToList()

        ' Display each student's name and exam score average.
        For Each s In students
            Console.WriteLine("The average score of " & s.FirstName & " " &
                              s.LastName & " is " & s.ExamScores.Average())
        Next

        ' Keep console window open in debug mode.
        Console.WriteLine("Press any key to exit.")
        Console.ReadKey()
    End Sub
End Class

' Output:
' The average score of Svetlana Omelchenko is 82.5
' The average score of Claire O'Donnell is 72.25
' The average score of Sven Mortensen is 84.5
' The average score of Cesar Garcia is 88.25
' The average score of Debra Garcia is 67
' The average score of Fadi Fakhouri is 92.25
' The average score of Hanying Feng is 88
' The average score of Hugo Garcia is 85.75
' The average score of Lance Tucker is 81.75
' The average score of Terry Adams is 85.25
' The average score of Eugene Zabokritski is 83
' The average score of Michael Tucker is 92
```

[Select 句](../../../../visual-basic/language-reference/queries/select-clause.md)の句では、オブジェクト初期化子を使用して、2つのソースのデータを使用して新しい `Student` オブジェクトをインスタンス化します。

クエリの結果を格納する必要がない場合は、名前付きの型よりも匿名型の方が便利です。 クエリが実行されたメソッドの外部にクエリ結果を渡す場合は、名前付きの型が必要になります。 次の例では、前の例と同じタスクを実行しますが、名前付きの型ではなく匿名型が使用します。

```vb
' Merge the data by using an anonymous type.
' Note the dynamic creation of a list of integers for the
' ExamScores member. We skip 1 because the first string
' in the array is the student ID, not an exam score.
Dim queryNamesScores2 =
    From nameLine In names
    Let splitName = nameLine.Split(New Char() {","})
    From scoreLine In scores
    Let splitScoreLine = scoreLine.Split(New Char() {","})
    Where Convert.ToInt32(splitName(2)) = Convert.ToInt32(splitScoreLine(0))
    Select New With
           {.Last = splitName(0),
            .First = splitName(1),
            .ExamScores = (From scoreAsText In splitScoreLine Skip 1
                           Select Convert.ToInt32(scoreAsText)).ToList()}

' Display each student's name and exam score average.
For Each s In queryNamesScores2
    Console.WriteLine("The average score of " & s.First & " " &
                      s.Last & " is " & s.ExamScores.Average())
Next
```

## <a name="see-also"></a>参照

- [LINQ と文字列 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-and-strings.md)
