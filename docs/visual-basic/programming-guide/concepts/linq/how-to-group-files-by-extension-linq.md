---
title: '方法: 拡張子別にファイルをグループ化する (LINQ)'
ms.date: 07/20/2015
ms.assetid: 904dc6d7-7162-4655-a7f4-5785d669bc5a
ms.openlocfilehash: 67beec0d3681bf7f0b04d9a402566076fd27ceba
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347479"
---
# <a name="how-to-group-files-by-extension-linq-visual-basic"></a>方法: 拡張子別にファイルをグループ化する (LINQ) (Visual Basic)
この例では、LINQ を使用して、ファイルまたはフォルダーの一覧に対して、高度なグループ化および並べ替えを実行する方法を示します。 また、<xref:System.Linq.Enumerable.Skip%2A> メソッドと <xref:System.Linq.Enumerable.Take%2A> メソッドを使用して、出力をページごとにコンソール ウィンドウに表示する方法も示します。  
  
## <a name="example"></a>使用例  
 次のクエリは、指定されたディレクトリ ツリーの内容を、ファイル名の拡張子別にグループ化する方法を示しています。  
  
```vb  
Module GroupByExtension  
    Public Sub Main()  
  
        ' Root folder to query, along with all subfolders.  
        Dim startFolder As String = "C:\program files\Microsoft Visual Studio 9.0\VB\"  
  
        ' Used in WriteLine() to skip over startfolder in output lines.  
        Dim rootLength As Integer = startFolder.Length  
  
        'Take a snapshot of the folder contents  
        Dim dir As New System.IO.DirectoryInfo(startFolder)  
        Dim fileList = dir.GetFiles("*.*", System.IO.SearchOption.AllDirectories)  
  
        ' Create the query.  
        Dim queryGroupByExt = From file In fileList _  
                          Group By file.Extension.ToLower() Into fileGroup = Group _  
                          Order By ToLower _  
                          Select fileGroup  
  
        ' Execute the query. By storing the result we can  
        ' page the display with good performance.  
        Dim groupByExtList = queryGroupByExt.ToList()  
  
        ' Display one group at a time. If the number of   
        ' entries is greater than the number of lines  
        ' in the console window, then page the output.  
        Dim trimLength = startFolder.Length  
        PageOutput(groupByExtList, trimLength)  
  
    End Sub  
  
    ' Pages console display for large query results. No more than one group per page.  
    ' This sub specifically works with group queries of FileInfo objects  
    ' but can be modified for any type.  
    Sub PageOutput(ByVal groupQuery, ByVal charsToSkip)  
  
        ' "3" = 1 line for extension key + 1 for "Press any key" + 1 for input cursor.  
        Dim numLines As Integer = Console.WindowHeight - 3  
        ' Flag to indicate whether there are more results to display  
        Dim goAgain As Boolean = True  
  
        For Each fg As IEnumerable(Of System.IO.FileInfo) In groupQuery  
            ' Start a new extension at the top of a page.  
            Dim currentLine As Integer = 0  
  
            Do While (currentLine < fg.Count())  
                Console.Clear()  
                Console.WriteLine(fg(0).Extension)  
  
                ' Get the next page of results  
                ' No more than one filename per page  
                Dim resultPage = From file In fg _  
                                Skip currentLine Take numLines  
  
                ' Execute the query. Trim the display output.  
                For Each line In resultPage  
                    Console.WriteLine(vbTab & line.FullName.Substring(charsToSkip))  
                Next  
  
                ' Advance the current position  
                currentLine = numLines + currentLine  
  
                ' Give the user a chance to break out of the loop  
                Console.WriteLine("Press any key for next page or the 'End' key to exit.")  
                Dim key As ConsoleKey = Console.ReadKey().Key  
                If key = ConsoleKey.End Then  
                    goAgain = False  
                    Exit For  
                End If  
            Loop  
        Next  
    End Sub  
End Module  
```  
  
 このプログラムの出力は、ローカル ファイル システムの詳細と `startFolder` の設定内容に応じて長くなる可能性があります。 すべての結果を確認できるように、次の例では、結果をページごとに出力する方法を示します。 同じ手法を Windows アプリケーションや Web アプリケーションに適用できます。 このコードでは、グループ内の項目をページごとに処理するため、`For Each` ループを入れ子にする必要があります。 また、一覧内での現在位置を計算し、ユーザーがページングを停止してプログラムを終了できるようにするロジックも追加されています。 この場合、ページング クエリは、元のクエリからキャッシュされた結果に対して実行されます。 LINQ to SQL などの他のコンテキストでは、このようなキャッシュは必要ありません。  
  
## <a name="compile-the-code"></a>コードのコンパイル  
System. Linq 名前空間の `Imports` ステートメントを使用して、Visual Basic コンソールアプリケーションプロジェクトを作成します。
  
## <a name="see-also"></a>関連項目

- [LINQ to Objects (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md)
- [LINQ とファイル ディレクトリ (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-and-file-directories.md)
