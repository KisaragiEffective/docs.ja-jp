---
title: C# での XML ツリーの作成 (LINQ to XML)
ms.date: 08/31/2018
ms.assetid: cc74234a-0bac-4327-9c8c-5a2ead15b595
ms.openlocfilehash: 4794e4fe019b30d8f2acb3eb255bb77ba2f7f290
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79169546"
---
# <a name="creating-xml-trees-in-c-linq-to-xml"></a>C# での XML ツリーの作成 (LINQ to XML)
ここでは、C# での XML ツリーの作成について説明します。  
  
 LINQ クエリの結果を <xref:System.Xml.Linq.XElement> のコンテンツとして使用する方法については、「[関数型構築 (LINQ to XML)](./functional-construction-linq-to-xml.md)」を参照してください。  
  
## <a name="constructing-elements"></a>要素の構築
 <xref:System.Xml.Linq.XElement> コンストラクターと <xref:System.Xml.Linq.XAttribute> コンストラクターのシグネチャを使用すると、要素または属性のコンテンツを引数としてコンストラクターに渡すことができます。 いずれかのコンストラクターは任意の数の引数を受け取るため、任意の数の子要素を渡すことができます。 もちろん、それらの子要素のそれぞれに、さらに子要素を含めることもできます。 いずれの要素にも、任意の数の属性を追加できます。  
  
 <xref:System.Xml.Linq.XNode> オブジェクト (<xref:System.Xml.Linq.XElement> を含む) や <xref:System.Xml.Linq.XAttribute> オブジェクトの追加時に、新しいコンテンツに親がない場合、単にオブジェクトが XML ツリーにアタッチされます。 新しいコンテンツに既に親があり、別の XML ツリーの一部となっている場合は、新しいコンテンツが複製され、新しく複製されたコンテンツが XML ツリーにアタッチされます。 このトピックの最後の例では、この動作について説明します。  
  
 `contacts`<xref:System.Xml.Linq.XElement> を作成するには、次のコードを使用できます。  
  
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
  
 適切にインデントされていれば、<xref:System.Xml.Linq.XElement> オブジェクトを構築するコードは、基になる XML の構造によく似ています。  
  
## <a name="xelement-constructors"></a>XElement コンストラクター  
 <xref:System.Xml.Linq.XElement> クラスは、関数型構築で次のコンストラクターを使用します。 <xref:System.Xml.Linq.XElement> のコンストラクターはこれ以外にも存在しますが、関数型構築に使用されないものはこの一覧に示していません。  
  
|Constructor|[説明]|  
|-----------------|-----------------|  
|`XElement(XName name, object content)`|<xref:System.Xml.Linq.XElement> を作成します。 `name` パラメーターには要素の名前を指定し、`content` には要素のコンテンツを指定します。|  
|`XElement(XName name)`|指定した名前で <xref:System.Xml.Linq.XElement> を初期化して、<xref:System.Xml.Linq.XName> を作成します。|  
|`XElement(XName name, params object[] content)`|指定した名前で <xref:System.Xml.Linq.XElement> を初期化して、<xref:System.Xml.Linq.XName> を作成します。 属性や子要素が、パラメーター リストのコンテンツから作成されます。|  
  
 `content` パラメーターは非常に柔軟です。 <xref:System.Xml.Linq.XElement> の有効な子オブジェクトの型すべてがサポートされています。 このパラメーターで渡されるさまざまな型のオブジェクトには、次の規則が適用されます。  
  
- 文字列はテキスト コンテンツとして追加されます。  
  
- <xref:System.Xml.Linq.XElement> は子要素として追加されます。  
  
- <xref:System.Xml.Linq.XAttribute> は属性として追加されます。  
  
- <xref:System.Xml.Linq.XProcessingInstruction>、<xref:System.Xml.Linq.XComment>、または <xref:System.Xml.Linq.XText> は、子コンテンツとして追加されます。  
  
- <xref:System.Collections.IEnumerable> は列挙され、その結果にこれらの規則が再帰的に適用されます。  
  
- その他の型に対しては `ToString` メソッドが呼び出され、その結果がテキスト コンテンツとして追加されます。  
  
### <a name="creating-an-xelement-with-content"></a>コンテンツを持つ XElement の作成  
 単純コンテンツが含まれる <xref:System.Xml.Linq.XElement> は、1 回のメソッド呼び出しで作成できます。 そのためには、次のように、コンテンツを 2 番目のパラメーターとして指定します。  
  
```csharp  
XElement n = new XElement("Customer", "Adventure Works");  
Console.WriteLine(n);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Customer>Adventure Works</Customer>  
```  
  
 任意の型のオブジェクトをコンテンツとして渡すことができます。 たとえば、次のコードでは、浮動小数点数がコンテンツとして含まれる要素を作成します。  
  
```csharp  
XElement n = new XElement("Cost", 324.50);  
Console.WriteLine(n);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Cost>324.5</Cost>  
```  
  
 浮動小数点数はボックス化されてコンストラクターに渡されます。 ボックス化された数は文字列に変換され、要素のコンテンツとして使用されます。  
  
### <a name="creating-an-xelement-with-a-child-element"></a>子要素を持つ XElement の作成  
 <xref:System.Xml.Linq.XElement> クラスのインスタンスをコンテンツ引数として渡すと、コンストラクターによって、子要素を持つ要素が作成されます。  
  
```csharp  
XElement shippingUnit = new XElement("ShippingUnit",  
    new XElement("Cost", 324.50)  
);  
Console.WriteLine(shippingUnit);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<ShippingUnit>  
  <Cost>324.5</Cost>  
</ShippingUnit>  
```  
  
### <a name="creating-an-xelement-with-multiple-child-elements"></a>複数の子要素を持つ XElement の作成  
 複数の <xref:System.Xml.Linq.XElement> オブジェクトをコンテンツとして渡すことができます。 各 <xref:System.Xml.Linq.XElement> オブジェクトは、子要素として格納されます。  
  
```csharp  
XElement address = new XElement("Address",  
    new XElement("Street1", "123 Main St"),  
    new XElement("City", "Mercer Island"),  
    new XElement("State", "WA"),  
    new XElement("Postal", "68042")  
);  
Console.WriteLine(address);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Address>  
  <Street1>123 Main St</Street1>  
  <City>Mercer Island</City>  
  <State>WA</State>  
  <Postal>68042</Postal>  
</Address>  
```  
  
 上の例を次のように拡張すると、完全な XML ツリーを作成できます。  
  
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
Console.WriteLine(contacts);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Contacts>  
  <Contact>  
    <Name>Patrick Hines</Name>  
    <Phone>206-555-0144</Phone>  
    <Address>  
      <Street1>123 Main St</Street1>  
      <City>Mercer Island</City>  
      <State>WA</State>  
      <Postal>68042</Postal>  
    </Address>  
  </Contact>  
</Contacts>  
```  

### <a name="creating-an-xelement-with-an-xattribute"></a>XAttribute がある XElement を作成する
 <xref:System.Xml.Linq.XAttribute> クラスのインスタンスをコンテンツ引数として渡すと、コンストラクターによって、属性がある要素が作成されます。

```csharp  
XElement phone = new XElement("Phone",  
    new XAttribute("Type", "Home"),  
    "555-555-5555");  
Console.WriteLine(phone);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Phone Type="Home">555-555-5555</Phone>
```

### <a name="creating-an-empty-element"></a>空要素の作成  
 空の <xref:System.Xml.Linq.XElement> を作成する場合は、コンストラクターにコンテンツを渡しません。 次の例では、空要素を作成します。  
  
```csharp  
XElement n = new XElement("Customer");  
Console.WriteLine(n);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Customer />  
```  
  
### <a name="attaching-vs-cloning"></a>アタッチと複製の比較  
 既に説明したように、<xref:System.Xml.Linq.XNode> オブジェクト (<xref:System.Xml.Linq.XElement> を含む) や <xref:System.Xml.Linq.XAttribute> オブジェクトの追加時に、新しいコンテンツに親がない場合、単にオブジェクトが XML ツリーにアタッチされます。 新しいコンテンツに既に親があり、別の XML ツリーの一部となっている場合は、新しいコンテンツが複製され、新しく複製されたコンテンツが XML ツリーにアタッチされます。  

次の例では、親を持つ要素をツリーに追加する場合と親を持たない要素をツリーに追加する場合の動作を示します。

```csharp  
// Create a tree with a child element.  
XElement xmlTree1 = new XElement("Root",  
    new XElement("Child1", 1)  
);  
  
// Create an element that is not parented.  
XElement child2 = new XElement("Child2", 2);  
  
// Create a tree and add Child1 and Child2 to it.  
XElement xmlTree2 = new XElement("Root",  
    xmlTree1.Element("Child1"),  
    child2  
);  
  
// Compare Child1 identity.  
Console.WriteLine("Child1 was {0}",  
    xmlTree1.Element("Child1") == xmlTree2.Element("Child1") ?  
    "attached" : "cloned");  
  
// Compare Child2 identity.  
Console.WriteLine("Child2 was {0}",  
    child2 == xmlTree2.Element("Child2") ?  
    "attached" : "cloned");  

// The example displays the following output:  
//    Child1 was cloned  
//    Child2 was attached  
```

## <a name="see-also"></a>参照

- [XML ツリーの作成 (C#)](./linq-to-xml-overview.md)
