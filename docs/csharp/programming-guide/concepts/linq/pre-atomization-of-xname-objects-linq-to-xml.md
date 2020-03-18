---
title: XName オブジェクトの事前アトミック化 (LINQ to XML) (C#)
ms.date: 07/20/2015
ms.assetid: e84fbbe7-f072-4771-bfbb-059d18e1ad15
ms.openlocfilehash: 2fd754a352bd2988e52ec9c67a9915a8e587b107
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69591501"
---
# <a name="pre-atomization-of-xname-objects-linq-to-xml-c"></a>XName オブジェクトの事前アトミック化 (LINQ to XML) (C#)
LINQ to XML でパフォーマンスを向上させる方法の 1 つは、<xref:System.Xml.Linq.XName> オブジェクトの事前アトミック化です。 事前アトミック化とは、<xref:System.Xml.Linq.XName> クラスと <xref:System.Xml.Linq.XElement> クラスのコンストラクターを使用して XML ツリーを作成する前に、文字列を <xref:System.Xml.Linq.XAttribute> オブジェクトに割り当てる操作です。 次に、(文字列から <xref:System.Xml.Linq.XName> への暗黙的な変換を使用する) コンストラクターに文字列を渡す代わりに、初期化された <xref:System.Xml.Linq.XName> オブジェクトを渡します。  
  
 これによって、特定の名前が繰り返される大きい XML ツリーを作成するときにパフォーマンスが向上します。 これを行うには、XML ツリーを構築する前に、<xref:System.Xml.Linq.XName> オブジェクトを宣言して初期化し、次に要素名と属性名に文字列を指定する代わりに <xref:System.Xml.Linq.XName> オブジェクトを使用します。 この手法では、同じ名前の多数の要素や属性を作成する場合に、パフォーマンスが大幅に向上します。  
  
 各自のシナリオで事前アトミック化をテストし、使用すべきかどうかを判断してください。  
  
## <a name="example"></a>例  
 例を次に示します。  
  
```csharp  
XName Root = "Root";  
XName Data = "Data";  
XName ID = "ID";  
  
XElement root = new XElement(Root,  
    new XElement(Data,  
        new XAttribute(ID, "1"),  
        "4,100,000"),  
    new XElement(Data,  
        new XAttribute(ID, "2"),  
        "3,700,000"),  
    new XElement(Data,  
        new XAttribute(ID, "3"),  
        "1,150,000")  
);  
  
Console.WriteLine(root);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Root>  
  <Data ID="1">4,100,000</Data>  
  <Data ID="2">3,700,000</Data>  
  <Data ID="3">1,150,000</Data>  
</Root>  
```  
  
 次の例は同じ手法を示し、XML ドキュメントが名前空間にあります。  
  
```csharp  
XNamespace aw = "http://www.adventure-works.com";  
XName Root = aw + "Root";  
XName Data = aw + "Data";  
XName ID = "ID";  
  
XElement root = new XElement(Root,  
    new XAttribute(XNamespace.Xmlns + "aw", aw),  
    new XElement(Data,  
        new XAttribute(ID, "1"),  
        "4,100,000"),  
    new XElement(Data,  
        new XAttribute(ID, "2"),  
        "3,700,000"),  
    new XElement(Data,  
        new XAttribute(ID, "3"),  
        "1,150,000")  
);  
  
Console.WriteLine(root);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<aw:Root xmlns:aw="http://www.adventure-works.com">  
  <aw:Data ID="1">4,100,000</aw:Data>  
  <aw:Data ID="2">3,700,000</aw:Data>  
  <aw:Data ID="3">1,150,000</aw:Data>  
</aw:Root>  
```  
  
 次に示すのは、より実働環境に近い例です。 この例では、要素の内容がクエリによって提供されます。  
  
```csharp  
XName Root = "Root";  
XName Data = "Data";  
XName ID = "ID";  
  
DateTime t1 = DateTime.Now;  
XElement root = new XElement(Root,  
    from i in System.Linq.Enumerable.Range(1, 100000)  
    select new XElement(Data,  
        new XAttribute(ID, i),  
        i * 5)  
);  
DateTime t2 = DateTime.Now;  
  
Console.WriteLine("Time to construct:{0}", t2 - t1);  
```  
  
 前の例では、名前が事前アトミック化されていない次の例に比べて良いパフォーマンスが得られます。  
  
```csharp  
DateTime t1 = DateTime.Now;  
XElement root = new XElement("Root",  
    from i in System.Linq.Enumerable.Range(1, 100000)  
    select new XElement("Data",  
        new XAttribute("ID", i),  
        i * 5)  
);  
DateTime t2 = DateTime.Now;  
  
Console.WriteLine("Time to construct:{0}", t2 - t1);  
```  
  
## <a name="see-also"></a>参照

- [アトミック化された XName および XNamespace オブジェクト (LINQ to XML) (C#)](./atomized-xname-and-xnamespace-objects-linq-to-xml.md)
