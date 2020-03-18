---
title: XDocument のクエリと XElement のクエリ (C#)
ms.date: 07/20/2015
ms.assetid: 46221ff5-62ee-4de8-93ba-66465facb5c1
ms.openlocfilehash: 475c77934ad535bad9ef79ff58bbddf991dc8f5c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "70253140"
---
# <a name="querying-an-xdocument-vs-querying-an-xelement-c"></a>XDocument のクエリと XElement のクエリ (C#)
<xref:System.Xml.Linq.XDocument.Load%2A?displayProperty=nameWithType> によってドキュメントを読み込む場合、<xref:System.Xml.Linq.XElement.Load%2A?displayProperty=nameWithType> によって読み込む場合とは少し異なるクエリを記述する必要があることがわかります。  
  
## <a name="comparison-of-xdocumentload-and-xelementload"></a>XDocument.Load と XElement.Load の比較  
 <xref:System.Xml.Linq.XElement> によって XML ドキュメントを <xref:System.Xml.Linq.XElement.Load%2A?displayProperty=nameWithType> に読み込む場合、XML ツリーのルートの <xref:System.Xml.Linq.XElement> には読み込んだドキュメントのルート要素が含まれます。 一方、<xref:System.Xml.Linq.XDocument> によって同じ XML ドキュメントを <xref:System.Xml.Linq.XDocument.Load%2A?displayProperty=nameWithType> に読み込む場合は、ツリーのルートは <xref:System.Xml.Linq.XDocument> ノードで、読み込んだドキュメントのルート要素は <xref:System.Xml.Linq.XElement> の許可されている 1 つの子 <xref:System.Xml.Linq.XDocument> ノードになります。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 軸は、ルート ノードを基準に動作します。  
  
 この最初の例では、<xref:System.Xml.Linq.XElement.Load%2A> を使用して XML ツリー読み込みます。 次に、ツリーのルートの子要素をクエリします。  
  
```csharp  
// Create a simple document and write it to a file  
File.WriteAllText("Test.xml", @"<Root>  
    <Child1>1</Child1>  
    <Child2>2</Child2>  
    <Child3>3</Child3>  
</Root>");  
  
Console.WriteLine("Querying tree loaded with XElement.Load");  
Console.WriteLine("----");  
XElement doc = XElement.Load("Test.xml");  
IEnumerable<XElement> childList =  
    from el in doc.Elements()  
    select el;  
foreach (XElement e in childList)  
    Console.WriteLine(e);  
```  
  
 この例では、次の出力が生成されることが想定されます。  
  
```output  
Querying tree loaded with XElement.Load  
----  
<Child1>1</Child1>  
<Child2>2</Child2>  
<Child3>3</Child3>  
```  
  
 次の例は上の例と同じですが、<xref:System.Xml.Linq.XDocument> ではなく <xref:System.Xml.Linq.XElement> に XML ツリーが読み込まれる点が異なります。  
  
```csharp  
// Create a simple document and write it to a file  
File.WriteAllText("Test.xml", @"<Root>  
    <Child1>1</Child1>  
    <Child2>2</Child2>  
    <Child3>3</Child3>  
</Root>");  
  
Console.WriteLine("Querying tree loaded with XDocument.Load");  
Console.WriteLine("----");  
XDocument doc = XDocument.Load("Test.xml");  
IEnumerable<XElement> childList =  
    from el in doc.Elements()  
    select el;  
foreach (XElement e in childList)  
    Console.WriteLine(e);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```output  
Querying tree loaded with XDocument.Load  
----  
<Root>  
  <Child1>1</Child1>  
  <Child2>2</Child2>  
  <Child3>3</Child3>  
</Root>  
```  
  
 この同じクエリでは、3 つの子ノードではなく 1 つの `Root` ノードが返されたことがわかります。  
  
 これに対処する 1 つの方法は、次のように、軸メソッドにアクセスする前に <xref:System.Xml.Linq.XDocument.Root%2A> プロパティを使用することです。  
  
```csharp  
// Create a simple document and write it to a file  
File.WriteAllText("Test.xml", @"<Root>  
    <Child1>1</Child1>  
    <Child2>2</Child2>  
    <Child3>3</Child3>  
</Root>");  
  
Console.WriteLine("Querying tree loaded with XDocument.Load");  
Console.WriteLine("----");  
XDocument doc = XDocument.Load("Test.xml");  
IEnumerable<XElement> childList =  
    from el in doc.Root.Elements()  
    select el;  
foreach (XElement e in childList)  
    Console.WriteLine(e);  
```  
  
 このクエリは、<xref:System.Xml.Linq.XElement> をルートとするツリーのクエリと同じように動作します。 この例では次の出力が生成されます。  
  
```output  
Querying tree loaded with XDocument.Load  
----  
<Child1>1</Child1>  
<Child2>2</Child2>  
<Child3>3</Child3>  
```  
  