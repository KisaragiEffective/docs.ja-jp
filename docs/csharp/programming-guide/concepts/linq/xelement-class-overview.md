---
title: XElement クラスの概要 (C#)
ms.date: 07/20/2015
ms.assetid: 2b9f0cd8-a1d1-4037-accf-0f38a410fa11
ms.openlocfilehash: 6a93dd4bdaf16fddff800b08b0f3146ecb63f9b7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79167895"
---
# <a name="xelement-class-overview-c"></a>XElement クラスの概要 (C#)
<xref:System.Xml.Linq.XElement> クラスは、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の基礎クラスの 1 つです。 これは XML 要素を表します。 このクラスを使用すると、要素の作成、要素のコンテンツの変更、子要素の追加、変更、削除、要素への属性の追加、および要素のコンテンツのテキスト形式へのシリアル化を行うことができます。 <xref:System.Xml?displayProperty=nameWithType>、<xref:System.Xml.XmlReader>、<xref:System.Xml.XmlWriter> などの、<xref:System.Xml.Xsl.XslCompiledTransform> の他のクラスと相互運用することもできます。  
  
このトピックでは、<xref:System.Xml.Linq.XElement> クラスによって提供される機能について説明します。  
  
## <a name="constructing-xml-trees"></a>XML ツリーの構築  
 次のようなさまざまな方法で XML ツリーを構築できます。  
  
- コードで XML ツリーを構築できます。 詳しくは、「[XML ツリーの作成 (C#)](./linq-to-xml-overview.md)」をご覧ください。  
  
- <xref:System.IO.TextReader>、テキスト ファイル、Web アドレス (URL) など、さまざまなソースから XML を解析できます。 詳しくは、「[XML の解析 (C#)](./how-to-parse-a-string.md)」をご覧ください。  
  
- <xref:System.Xml.XmlReader> を使用してツリーを設定できます。 詳細については、<xref:System.Xml.Linq.XNode.ReadFrom%2A> を参照してください。  
  
- <xref:System.Xml.XmlWriter> にコンテンツを書き込むことができるモジュールがある場合は、<xref:System.Xml.Linq.XContainer.CreateWriter%2A> メソッドを使用してライターを作成し、このライターをモジュールに渡し、<xref:System.Xml.XmlWriter> に書き込まれたコンテンツを使用して XML ツリーを設定できます。  
  
 しかし、XML ツリーを作成する最も一般的な方法は次のとおりです。  
  
```csharp  
XElement contacts =  
    new XElement("Contacts",  
        new XElement("Contact",  
            new XElement("Name", "Patrick Hines"),
            new XElement("Phone", "206-555-0144"),  
            new XElement("Address",  
                new XElement("Street1", "123 Main St"),  
                new XElement("City", "Mercer Island"),  
                new XElement("State", "WA"),  
                new XElement("Postal", "68042")  
            )  
        )  
    );  
```  
  
 XML ツリーを作成するもう 1 つの一般的な方法では、次の例に示すように、LINQ クエリの結果を使用して XML ツリーを設定します。  
  
```csharp  
XElement srcTree = new XElement("Root",  
    new XElement("Element", 1),  
    new XElement("Element", 2),  
    new XElement("Element", 3),  
    new XElement("Element", 4),  
    new XElement("Element", 5)  
);  
XElement xmlTree = new XElement("Root",  
    new XElement("Child", 1),  
    new XElement("Child", 2),  
    from el in srcTree.Elements()  
    where (int)el > 2  
    select el  
);  
Console.WriteLine(xmlTree);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Root>  
  <Child>1</Child>  
  <Child>2</Child>  
  <Element>3</Element>  
  <Element>4</Element>  
  <Element>5</Element>  
</Root>  
```  
  
## <a name="serializing-xml-trees"></a>XML ツリーのシリアル化  
 XML ツリーは、<xref:System.IO.File>、<xref:System.IO.TextWriter>、または <xref:System.Xml.XmlWriter> にシリアル化できます。  
  
 詳しくは、「[XML ツリーのシリアル化 (C#)](./preserving-white-space-while-serializing.md)」をご覧ください。  
  
## <a name="retrieving-xml-data-via-axis-methods"></a>軸メソッドによる XML データの取得  
 軸メソッドを使用すると、属性、子要素、子孫要素、および祖先要素を取得できます。 LINQ クエリは、軸メソッドに対して機能し、XML ツリーを操作して処理するための、柔軟で強力な複数の機能を備えています。  
  
 詳しくは、「[LINQ to XML 軸 (C#)](./linq-to-xml-axes-overview.md)」をご覧ください。  
  
## <a name="querying-xml-trees"></a>XML ツリーのクエリ  
 XML ツリーからデータを抽出する LINQ クエリを記述できます。  
  
 詳しくは、「[XML ツリーのクエリ (C#)](./how-to-find-an-element-with-a-specific-attribute.md)」をご覧ください。  
  
## <a name="modifying-xml-trees"></a>XML ツリーの変更  
 要素を変更するには、そのコンテンツや属性を変更するなど、さまざまな方法があります。 要素を親から削除することもできます。  
  
 詳しくは、「[XML ツリーの変更 (LINQ to XML) (C#)](./in-memory-xml-tree-modification-vs-functional-construction-linq-to-xml.md)」をご覧ください。  
  
## <a name="see-also"></a>参照

- [LINQ to XML プログラミングの概要 (C#)](serializing-to-files-textwriters-and-xmlwriters.md)
