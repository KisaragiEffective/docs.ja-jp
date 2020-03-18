---
title: 指定されたワードのセットを含む文章を照会する方法 (LINQ) (C#)
ms.date: 07/20/2015
ms.assetid: 0724b429-4b87-4d26-a7b1-409358f3fc20
ms.openlocfilehash: df279f57d9965d796397cbcf7a0f3ba05bf9e5c1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79168857"
---
# <a name="how-to-query-for-sentences-that-contain-a-specified-set-of-words-linq-c"></a>指定されたワードのセットを含む文章を照会する方法 (LINQ) (C#)
この例は、指定された一連の単語と一致する文言を含む文をテキスト ファイルから検索する方法を示しています。 この例では検索語句の配列をハードコーディングしていますが、実行時に動的に設定することもできます。 この例のクエリを実行すると、"Historically"、"data"、"integrated" という単語をすべて含んだ文が返されます。  
  
## <a name="example"></a>例  
  
```csharp  
class FindSentences  
{  
    static void Main()  
    {  
        string text = @"Historically, the world of data and the world of objects " +  
        @"have not been well integrated. Programmers work in C# or Visual Basic " +  
        @"and also in SQL or XQuery. On the one side are concepts such as classes, " +  
        @"objects, fields, inheritance, and .NET Framework APIs. On the other side " +  
        @"are tables, columns, rows, nodes, and separate languages for dealing with " +  
        @"them. Data types often require translation between the two worlds; there are " +  
        @"different standard functions. Because the object world has no notion of query, a " +  
        @"query can only be represented as a string without compile-time type checking or " +  
        @"IntelliSense support in the IDE. Transferring data from SQL tables or XML trees to " +  
        @"objects in memory is often tedious and error-prone.";  
  
        // Split the text block into an array of sentences.  
        string[] sentences = text.Split(new char[] { '.', '?', '!' });  
  
        // Define the search terms. This list could also be dynamically populated at runtime.  
        string[] wordsToMatch = { "Historically", "data", "integrated" };  
  
        // Find sentences that contain all the terms in the wordsToMatch array.  
        // Note that the number of terms to match is not specified at compile time.  
        var sentenceQuery = from sentence in sentences  
                            let w = sentence.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' },  
                                                    StringSplitOptions.RemoveEmptyEntries)  
                            where w.Distinct().Intersect(wordsToMatch).Count() == wordsToMatch.Count()  
                            select sentence;  
  
        // Execute the query. Note that you can explicitly type  
        // the iteration variable here even though sentenceQuery  
        // was implicitly typed.
        foreach (string str in sentenceQuery)  
        {  
            Console.WriteLine(str);  
        }  
  
        // Keep the console window open in debug mode.  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
}  
/* Output:  
Historically, the world of data and the world of objects have not been well integrated  
*/  
```  
  
 このクエリではまず、テキストを文単位に分割し、個々の単語を保持する文字列の配列に分割しています。 その各配列について、重複する単語を <xref:System.Linq.Enumerable.Distinct%2A> メソッドですべて削除したうえで、それらの単語の配列と <xref:System.Linq.Enumerable.Intersect%2A> 配列との `wordsToMatch` 演算を実行します。 共通部分のカウントと `wordsToMatch` 配列のカウントとが一致した場合、文を構成する単語内にすべての検索語句が見つかったものとして判断され、該当する文が返されます。  
  
 句読点を文字列から削除するために、<xref:System.String.Split%2A> の呼び出しでは句読点を区切り記号として使用しています。 この処理がないと、たとえば "Historically," という文字列があった場合に、`wordsToMatch` 配列内の "Historically" と一致しません。 ソース テキストに使われている句読点の種類によっては、別の区切り記号を使う必要があります。  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
System.Linq 名前空間と System.IO 名前空間に `using` ディレクティブを使用して、C# コンソール アプリケーション プロジェクトを作成します。

## <a name="see-also"></a>参照

- [LINQ と文字列 (C#)](./linq-and-strings.md)
