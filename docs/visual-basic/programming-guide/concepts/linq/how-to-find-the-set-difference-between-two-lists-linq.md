---
title: '方法 : 2 つのリストの差集合を見つける (LINQ)'
ms.date: 07/20/2015
ms.assetid: b5b25474-10a8-4df6-aab5-75621bb6b68e
ms.openlocfilehash: 42c5b5ee8a6af1323cb5eee12694fb94063eb877
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347507"
---
# <a name="how-to-find-the-set-difference-between-two-lists-linq-visual-basic"></a>方法: 2 つのリストの差集合を検索する (LINQ) (Visual Basic)
この例では、LINQ を使用して、2 つの文字列リストを比較し、names2.txt ではなく names1.txt でそれらの行を出力する方法を示します。  
  
### <a name="to-create-the-data-files"></a>データ ファイルを作成するには  
  
1. [「方法: 文字列コレクションを結合および比較する (LINQ) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-combine-and-compare-string-collections-linq.md)」に示されているように、ソリューションフォルダーに names1.txt と names2.txt をコピーします。  
  
## <a name="example"></a>使用例  
  
```vb  
Class CompareLists  
  
    Shared Sub Main()  
  
        ' Create the IEnumerable data sources.  
        Dim names1 As String() = System.IO.File.ReadAllLines("../../../names1.txt")  
        Dim names2 As String() = System.IO.File.ReadAllLines("../../../names2.txt")  
  
        ' Create the query. Note that method syntax must be used here.  
        Dim differenceQuery = names1.Except(names2)  
        Console.WriteLine("The following lines are in names1.txt but not names2.txt")  
  
        ' Execute the query.  
        For Each name As String In differenceQuery  
            Console.WriteLine(name)  
        Next  
  
        ' Keep console window open in debug mode.  
        Console.WriteLine("Press any key to exit.")  
        Console.ReadKey()  
    End Sub  
End Class  
' Output:  
' The following lines are in names1.txt but not names2.txt  
' Potra, Cristina  
' Noriega, Fabricio  
' Aw, Kam Foo  
' Toyoshima, Tim  
' Guy, Wey Yuan  
' Garcia, Debra  
```  
  
 <xref:System.Linq.Enumerable.Except%2A>、<xref:System.Linq.Enumerable.Distinct%2A>、<xref:System.Linq.Enumerable.Union%2A>、<xref:System.Linq.Enumerable.Concat%2A>など、Visual Basic の一部の種類のクエリ操作は、メソッドベースの構文でのみ表すことができます。  
  
## <a name="compile-the-code"></a>コードのコンパイル  
System. Linq 名前空間の `Imports` ステートメントを使用して、Visual Basic コンソールアプリケーションプロジェクトを作成します。
  
## <a name="see-also"></a>関連項目

- [LINQ と文字列 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-and-strings.md)
