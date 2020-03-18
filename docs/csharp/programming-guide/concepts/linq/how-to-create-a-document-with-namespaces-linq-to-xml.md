---
title: 名前空間を持つドキュメントを作成する方法 (C#) (LINQ to XML)
ms.date: 07/20/2015
ms.assetid: 37e63c57-f86d-47ac-88a7-2c2d107def30
ms.openlocfilehash: 429b0b0b41f2201b983f931e469b25ff406b91ac
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74141324"
---
# <a name="how-to-create-a-document-with-namespaces-c-linq-to-xml"></a>名前空間を持つドキュメントを作成する方法 (C#) (LINQ to XML)
このトピックでは、名前空間を持つドキュメントを作成する方法について説明します。  
  
## <a name="example"></a>例  
 名前空間にある要素や属性を作成するには、最初に <xref:System.Xml.Linq.XNamespace> オブジェクトを宣言して初期化します。 次に、加算演算子オーバーロードを使用して名前空間をローカル名に連結し、文字列として表します。  
  
 次の例では、1 つの名前空間を持つドキュメントを作成します。 既定では、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] によって、このドキュメントが既定の名前空間でシリアル化されます。  
  
```csharp  
// Create an XML tree in a namespace.  
XNamespace aw = "http://www.adventure-works.com";  
XElement root = new XElement(aw + "Root",  
    new XElement(aw + "Child", "child content")  
);  
Console.WriteLine(root);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Root xmlns="http://www.adventure-works.com">  
  <Child>child content</Child>  
</Root>  
```  
  
## <a name="example"></a>例  
 次の例では、1 つの名前空間を持つドキュメントを作成します。 名前空間プレフィックスを持つ名前空間を宣言する属性も作成します。 プレフィックス付きの名前空間を宣言する属性を作成するには、属性名が名前空間プレフィックスで、この名前が <xref:System.Xml.Linq.XNamespace.Xmlns%2A> 名前空間にある属性を作成します。 この属性の値は名前空間の URI です。  
  
```csharp  
// Create an XML tree in a namespace, with a specified prefix  
XNamespace aw = "http://www.adventure-works.com";  
XElement root = new XElement(aw + "Root",  
    new XAttribute(XNamespace.Xmlns + "aw", "http://www.adventure-works.com"),  
    new XElement(aw + "Child", "child content")  
);  
Console.WriteLine(root);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<aw:Root xmlns:aw="http://www.adventure-works.com">  
  <aw:Child>child content</aw:Child>  
</aw:Root>  
```  
  
## <a name="example"></a>例  
 次の例では、2 つの名前空間が含まれるドキュメントを作成します。 1 つは既定の名前空間です。 もう 1 つは、プレフィックスを持つ名前空間です。  
  
 ルート要素に名前空間属性を含めることによって名前空間がシリアル化され、`http://www.adventure-works.com` が既定の名前空間となり、`www.fourthcoffee.com` はプレフィックス "fc" でシリアル化されます。 既定の名前空間を宣言する属性を作成するには、"xmlns" という名前の属性を、名前空間を指定せずに作成します。 属性の値は、既定の名前空間 URI です。  
  
```csharp  
// The http://www.adventure-works.com namespace is forced to be the default namespace.  
XNamespace aw = "http://www.adventure-works.com";  
XNamespace fc = "www.fourthcoffee.com";  
XElement root = new XElement(aw + "Root",  
    new XAttribute("xmlns", "http://www.adventure-works.com"),  
    new XAttribute(XNamespace.Xmlns + "fc", "www.fourthcoffee.com"),  
    new XElement(fc + "Child",  
        new XElement(aw + "DifferentChild", "other content")  
    ),  
    new XElement(aw + "Child2", "c2 content"),  
    new XElement(fc + "Child3", "c3 content")  
);  
Console.WriteLine(root);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Root xmlns="http://www.adventure-works.com" xmlns:fc="www.fourthcoffee.com">  
  <fc:Child>  
    <DifferentChild>other content</DifferentChild>  
  </fc:Child>  
  <Child2>c2 content</Child2>  
  <fc:Child3>c3 content</fc:Child3>  
</Root>  
```  
  
## <a name="example"></a>例  
 次の例では、名前空間プレフィックスのある名前空間を 2 つ含むドキュメントを作成します。  
  
```csharp  
XNamespace aw = "http://www.adventure-works.com";  
XNamespace fc = "www.fourthcoffee.com";  
XElement root = new XElement(aw + "Root",  
    new XAttribute(XNamespace.Xmlns + "aw", aw.NamespaceName),  
    new XAttribute(XNamespace.Xmlns + "fc", fc.NamespaceName),  
    new XElement(fc + "Child",  
        new XElement(aw + "DifferentChild", "other content")  
    ),  
    new XElement(aw + "Child2", "c2 content"),  
    new XElement(fc + "Child3", "c3 content")  
);  
Console.WriteLine(root);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<aw:Root xmlns:aw="http://www.adventure-works.com" xmlns:fc="www.fourthcoffee.com">  
  <fc:Child>  
    <aw:DifferentChild>other content</aw:DifferentChild>  
  </fc:Child>  
  <aw:Child2>c2 content</aw:Child2>  
  <fc:Child3>c3 content</fc:Child3>  
</aw:Root>  
```  
  
## <a name="example"></a>例  
 <xref:System.Xml.Linq.XNamespace> オブジェクトを宣言して作成する代わりに、展開名を使用しても同じ結果が得られます。  
  
 ただし、この方法はパフォーマンスに影響を与えます。 拡張名が含まれた文字列を [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] に渡すたびに、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] は名前を解析して、アトミック化された名前空間を検索し、アトミック化された名前を見つける必要があります。 この処理は CPU 時間を消費します。 パフォーマンスが重要な場合には、<xref:System.Xml.Linq.XNamespace> オブジェクトを明示的に宣言して使用することをお勧めします。  
  
 パフォーマンスが重要な場合、詳細については、「[XName オブジェクトの事前アトミック化 (LINQ to XML) (C#)](./pre-atomization-of-xname-objects-linq-to-xml.md)」を参照してください  
  
```csharp  
// Create an XML tree in a namespace, with a specified prefix  
XElement root = new XElement("{http://www.adventure-works.com}Root",  
    new XAttribute(XNamespace.Xmlns + "aw", "http://www.adventure-works.com"),  
    new XElement("{http://www.adventure-works.com}Child", "child content")  
);  
Console.WriteLine(root);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<aw:Root xmlns:aw="http://www.adventure-works.com">  
  <aw:Child>child content</aw:Child>  
</aw:Root>  
```  
  
## <a name="see-also"></a>参照

- [名前空間の概要 (LINQ to XML)](namespaces-overview-linq-to-xml.md)
