---
title: テキストから XML へのストリーミング変換を実行する方法 (C#)
ms.date: 07/20/2015
ms.assetid: 9b3bd941-d0ff-4f2d-ae41-7c3b81d8fae6
ms.openlocfilehash: 496535b7f868095a62be2b72b1eea2b082e00a44
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75345797"
---
# <a name="how-to-perform-streaming-transformations-of-text-to-xml-c"></a>テキストから XML へのストリーミング変換を実行する方法 (C#)

テキスト ファイルを処理する方法の 1 つは、`yield return` 構造を使用して、テキスト ファイルを一度に 1 行ずつストリーム出力する拡張メソッドを記述することです。 その後、テキスト ファイルをレイジー遅延方式で処理する LINQ クエリを記述できます。 次に <xref:System.Xml.Linq.XStreamingElement> を使用してストリーム出力すると、ソース テキスト ファイルのサイズにかかわらず、メモリを最小限しか使用しないテキスト ファイルから XML への変換を作成できます。

 ストリーミング変換に関しては、いくつかの注意事項があります。 ストリーミング変換は、ファイル全体の処理を 1 回で行うことが可能で、かつソース ドキュメント内の順番どおりに行を処理できる場合に適しています。 ファイルを 2 回以上処理する必要がある場合、または処理前に行を並べ替える必要がある場合は、ストリーミングの手法が持つ多くの利点を活かすことはできません。

## <a name="example"></a>例

 次のテキスト ファイル (People.txt) は、この例のソースです。

```text
#This is a comment
1,Tai,Yee,Writer
2,Nikolay,Grachev,Programmer
3,David,Wright,Inventor
```

 次のコードには、このテキスト ファイルの行を遅延方式でストリーム出力する拡張メソッドが含まれています。

```csharp
public static class StreamReaderSequence
{
    public static IEnumerable<string> Lines(this StreamReader source)
    {
        if (source == null)
            throw new ArgumentNullException(nameof(source));

        string line;
        while ((line = source.ReadLine()) != null)
        {
            yield return line;
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        var sr = new StreamReader("People.txt");
        var xmlTree = new XStreamingElement("Root",
            from line in sr.Lines()
            let items = line.Split(',')
            where !line.StartsWith("#")
            select new XElement("Person",
                       new XAttribute("ID", items[0]),
                       new XElement("First", items[1]),
                       new XElement("Last", items[2]),
                       new XElement("Occupation", items[3])
                   )
        );
        Console.WriteLine(xmlTree);
        sr.Close();
    }
}
```

 この例を実行すると、次の出力が生成されます。

```xml
<Root>
  <Person ID="1">
    <First>Tai</First>
    <Last>Yee</Last>
    <Occupation>Writer</Occupation>
  </Person>
  <Person ID="2">
    <First>Nikolay</First>
    <Last>Grachev</Last>
    <Occupation>Programmer</Occupation>
  </Person>
  <Person ID="3">
    <First>David</First>
    <Last>Wright</Last>
    <Occupation>Inventor</Occupation>
  </Person>
</Root>
```

## <a name="see-also"></a>参照

- <xref:System.Xml.Linq.XStreamingElement>
