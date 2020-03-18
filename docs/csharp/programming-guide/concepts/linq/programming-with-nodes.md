---
title: ノードでのプログラミング (C#)
ms.date: 07/20/2015
ms.assetid: c38df0f2-c805-431a-93ff-9103a4284c2f
ms.openlocfilehash: 05c2e95fe97effda7b537a7ac2d8f5780f4e212b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79168315"
---
# <a name="programming-with-nodes-c"></a>ノードでのプログラミング (C#)
XML エディター、変換システム、レポート作成プログラムなどのプログラムを作成する [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の開発者は、要素や属性よりも細かい粒度レベルで動作するプログラムを作成しなければならないことがよくあります。 また場合によっては、ノード レベルで、テキスト ノード、処理命令、およびコメントを操作する必要があります。 このトピックでは、ノード レベルでのプログラミングについて詳しく説明します。  
  
## <a name="node-details"></a>ノードの詳細  
 ノード レベルで作業するプログラマが知っておく必要があるプログラミングの詳細事項がいくつかあります。  
  
### <a name="parent-property-of-children-nodes-of-xdocument-is-set-to-null"></a>XDocument の子ノードの Parent プロパティが Null に設定される  
 <xref:System.Xml.Linq.XObject.Parent%2A> プロパティには、親ノードではなく親 <xref:System.Xml.Linq.XElement> が含まれています。 <xref:System.Xml.Linq.XDocument> の子ノードには、親 <xref:System.Xml.Linq.XElement> がありません。 これらの子ノードの親はドキュメントであるため、子ノードの <xref:System.Xml.Linq.XObject.Parent%2A> プロパティは null に設定されます。  
  
 この動作を次の例で示します。  
  
```csharp  
XDocument doc = XDocument.Parse(@"<!-- a comment --><Root/>");  
Console.WriteLine(doc.Nodes().OfType<XComment>().First().Parent == null);  
Console.WriteLine(doc.Root.Parent == null);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```output  
True  
True  
```  
  
### <a name="adjacent-text-nodes-are-possible"></a>隣接するテキスト ノードが存在する可能性がある  
 多くの XML プログラミング モデルでは、隣接するテキスト ノードが常に連結されます。 これは、テキスト ノードの正規化と呼ばれることがあります。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] ではテキスト ノードは正規化されません。 同じ要素に 2 つのテキスト ノードを追加すると、隣接するテキスト ノードになります。 ただし、<xref:System.Xml.Linq.XText> ノードではなく文字列として指定されたコンテンツを追加すると、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] はその文字列を隣接するテキスト ノードに連結します。  
  
 この動作を次の例で示します。  
  
```csharp  
XElement xmlTree = new XElement("Root", "Content");  
  
Console.WriteLine(xmlTree.Nodes().OfType<XText>().Count());  
  
// this does not add a new text node  
xmlTree.Add("new content");  
Console.WriteLine(xmlTree.Nodes().OfType<XText>().Count());  
  
// this does add a new, adjacent text node  
xmlTree.Add(new XText("more text"));  
Console.WriteLine(xmlTree.Nodes().OfType<XText>().Count());  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```output  
1  
1  
2  
```  
  
### <a name="empty-text-nodes-are-possible"></a>空のテキスト ノードが存在する可能性がある  
 一部の XML プログラミング モデルでは、テキスト ノードに空の文字列が含まれないことが保証されます。 その理由は、テキスト ノードに空の文字列が含まれていなければ、XML のシリアル化に対して影響が生じないためです。 ただし、隣接するテキスト ノードの場合と同じ理由で、テキスト ノードの値を空の文字列に設定することによってテキスト ノードからテキストを削除した場合、テキスト ノード自体は削除されません。  
  
```csharp  
XElement xmlTree = new XElement("Root", "Content");  
XText textNode = xmlTree.Nodes().OfType<XText>().First();  
  
// the following line does not cause the removal of the text node.  
textNode.Value = "";  
  
XText textNode2 = xmlTree.Nodes().OfType<XText>().First();  
Console.WriteLine(">>{0}<<", textNode2);
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```output  
>><<  
```  
  
### <a name="an-empty-text-node-impacts-serialization"></a>空のテキスト ノードがシリアル化に影響する  
 要素に空の子テキスト ノードのみが含まれている場合、その要素は長いタグ構文 `<Child></Child>` でシリアル化されます。 子ノードがまったく含まれていない要素は、短いタグ構文 `<Child />` でシリアル化されます。  
  
```csharp  
XElement child1 = new XElement("Child1",  
    new XText("")  
);  
XElement child2 = new XElement("Child2");  
Console.WriteLine(child1);  
Console.WriteLine(child2);
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Child1></Child1>  
<Child2 />  
```  
  
### <a name="namespaces-are-attributes-in-the-linq-to-xml-tree"></a>LINQ to XML ツリーでは名前空間が属性になる  
 名前空間宣言の構文は属性と同じですが、XSLT や XPath などの一部のプログラミング インターフェイスでは、名前空間宣言が属性と見なされません。 ただし [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、名前空間が <xref:System.Xml.Linq.XAttribute> オブジェクトとして XML ツリーに格納されます。 名前空間宣言を含んでいる要素の属性を反復処理すると、名前空間宣言が、返されるコレクションの項目の 1 つになります。  
  
 <xref:System.Xml.Linq.XAttribute.IsNamespaceDeclaration%2A> プロパティによって、属性が名前空間宣言であるかどうかが示されます。  
  
```csharp  
XElement root = XElement.Parse(  
@"<Root  
    xmlns='http://www.adventure-works.com'  
    xmlns:fc='www.fourthcoffee.com'  
    AnAttribute='abc'/>");  
foreach (XAttribute att in root.Attributes())  
    Console.WriteLine("{0}  IsNamespaceDeclaration:{1}", att, att.IsNamespaceDeclaration);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```output  
xmlns="http://www.adventure-works.com"  IsNamespaceDeclaration:True  
xmlns:fc="www.fourthcoffee.com"  IsNamespaceDeclaration:True  
AnAttribute="abc"  IsNamespaceDeclaration:False  
```  
  
### <a name="xpath-axis-methods-do-not-return-child-white-space-of-xdocument"></a>XPath 軸メソッドからは XDocument の空白の子ノードが返されない  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] で処理できる <xref:System.Xml.Linq.XDocument> の子テキスト ノードは、空白のみを含んでいるものに限られます。 ただし、XPath オブジェクト モデルでは、空白がドキュメントの子ノードとして組み込まれないため、<xref:System.Xml.Linq.XDocument> 軸を使用して <xref:System.Xml.Linq.XContainer.Nodes%2A> の子を反復処理すると、空白のテキスト ノードが返されます。 一方、XPath 軸メソッドを使用して <xref:System.Xml.Linq.XDocument> の子を反復処理すると、空白のテキスト ノードが返されません。  
  
```csharp  
// Create a document with some white-space child nodes of the document.  
XDocument root = XDocument.Parse(  
@"<?xml version='1.0' encoding='utf-8' standalone='yes'?>  
  
<Root/>  
  
<!--a comment-->  
", LoadOptions.PreserveWhitespace);  
  
// count the white-space child nodes using LINQ to XML  
Console.WriteLine(root.Nodes().OfType<XText>().Count());  
  
// count the white-space child nodes using XPathEvaluate  
Console.WriteLine(((IEnumerable)root.XPathEvaluate("text()")).OfType<XText>().Count());
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```output  
3  
0  
```  
  
### <a name="xdeclaration-objects-are-not-nodes"></a>XDeclaration オブジェクトはノードではない  
 <xref:System.Xml.Linq.XDocument> の子ノードを反復処理しても、XML 宣言オブジェクトは生成されません。 これはドキュメントのプロパティであって、ドキュメントの子ノードではありません。  
  
```csharp  
XDocument doc = new XDocument(  
    new XDeclaration("1.0", "utf-8", "yes"),  
    new XElement("Root")  
);  
doc.Save("Temp.xml");  
Console.WriteLine(File.ReadAllText("Temp.xml"));  
  
// this shows that there is only one child node of the document  
Console.WriteLine(doc.Nodes().Count());  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```output  
<?xml version="1.0" encoding="utf-8" standalone="yes"?>  
<Root />  
1  
```  
  