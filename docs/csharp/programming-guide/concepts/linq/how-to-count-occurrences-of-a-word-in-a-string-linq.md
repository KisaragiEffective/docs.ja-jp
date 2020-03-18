---
title: 文字列での単語の出現回数をカウントする方法 (LINQ) (C#)
ms.date: 07/20/2015
ms.assetid: f8e6f546-7c14-4aa1-8a75-e8d09f3b8ccd
ms.openlocfilehash: 9c3ac2e0d44d52e437586a4d105a022f75c1dc54
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79169326"
---
# <a name="how-to-count-occurrences-of-a-word-in-a-string-linq-c"></a>文字列での単語の出現回数をカウントする方法 (LINQ) (C#)
この例では、LINQ クエリを使用して、指定された単語が文字列内に出現する回数をカウントする方法を示します。 カウントを実行するには、まず <xref:System.String.Split%2A> メソッドを呼び出して単語の配列を作成します。 <xref:System.String.Split%2A> メソッドを呼び出すと、パフォーマンスが低下します。 文字列に対する操作が単語のカウントのみである場合は、<xref:System.Text.RegularExpressions.Regex.Matches%2A> または <xref:System.String.IndexOf%2A> メソッドの使用を検討してください。 ただし、パフォーマンスが重要でない場合や、他の種類のクエリを実行する目的で事前に文章を分割している場合は、LINQ を使用して単語や語句をカウントすることにも意味があります。  
  
## <a name="example"></a>例  
  
```csharp  
class CountWords  
{  
    static void Main()  
    {  
        string text = @"Historically, the world of data and the world of objects" +  
          @" have not been well integrated. Programmers work in C# or Visual Basic" +  
          @" and also in SQL or XQuery. On the one side are concepts such as classes," +  
          @" objects, fields, inheritance, and .NET Framework APIs. On the other side" +  
          @" are tables, columns, rows, nodes, and separate languages for dealing with" +  
          @" them. Data types often require translation between the two worlds; there are" +  
          @" different standard functions. Because the object world has no notion of query, a" +  
          @" query can only be represented as a string without compile-time type checking or" +  
          @" IntelliSense support in the IDE. Transferring data from SQL tables or XML trees to" +  
          @" objects in memory is often tedious and error-prone.";  
  
        string searchTerm = "data";  
  
        //Convert the string into an array of words  
        string[] source = text.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);  
  
        // Create the query.  Use ToLowerInvariant to match "data" and "Data"
        var matchQuery = from word in source  
                         where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()  
                         select word;  
  
        // Count the matches, which executes the query.  
        int wordCount = matchQuery.Count();  
        Console.WriteLine("{0} occurrences(s) of the search term \"{1}\" were found.", wordCount, searchTerm);  
  
        // Keep console window open in debug mode  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
}  
/* Output:  
   3 occurrences(s) of the search term "data" were found.  
*/  
```  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 System.Linq 名前空間と System.IO 名前空間に `using` ディレクティブを使用して、C# コンソール アプリケーション プロジェクトを作成します。  
  
## <a name="see-also"></a>参照

- [LINQ と文字列 (C#)](./linq-and-strings.md)
