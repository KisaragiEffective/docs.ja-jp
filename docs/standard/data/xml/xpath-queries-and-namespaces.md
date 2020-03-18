---
title: XPath クエリおよび名前空間
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: ef6402be-2f8e-4be2-8d3e-a80891cdef8b
ms.openlocfilehash: 91503ce0bffa1a9390432a51bff1ef10d80f563a
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75709778"
---
# <a name="xpath-queries-and-namespaces"></a>XPath クエリおよび名前空間
XPath クエリは XML ドキュメント中の名前空間を認識し、名前空間プレフィックスを使用して要素と属性名を修飾することができます。 名前空間プレフィックスで要素や属性の名前を修飾すると、XPath クエリで返されるノードを特定の名前空間に属するノードだけに限定することができます。  
  
 たとえば、プレフィックス `books` が名前空間 `http://www.contoso.com/books` に割り当てられると、XPath クエリ `/books:books/books:book` は名前空間 `book` 内の `http://www.contoso.com/books` 要素だけを選択します。  
  
## <a name="the-xmlnamespacemanager"></a>XmlNamespaceManager  
 XPath クエリで名前空間を使用するために、<xref:System.Xml.IXmlNamespaceResolver> クラスに似た <xref:System.Xml.XmlNamespaceManager> インターフェイスから派生したオブジェクトが、名前空間 URI と XPath クエリに含まれるプレフィックスを使用して作成されます。  
  
 <xref:System.Xml.XmlNamespaceManager> オブジェクトは次の方法でクエリで使用することができます。  
  
- <xref:System.Xml.XmlNamespaceManager> オブジェクトの <xref:System.Xml.XPath.XPathExpression> メソッドを使用して、<xref:System.Xml.XPath.XPathExpression.SetContext%2A> オブジェクトを既存の <xref:System.Xml.XPath.XPathExpression> オブジェクトに関連付ける。 静的 <xref:System.Xml.XPath.XPathExpression> メソッドを使用して、新しい <xref:System.Xml.XPath.XPathExpression.Compile%2A> オブジェクトをコンパイルすることもできます。このメソッドは XPath 式を表す文字列と <xref:System.Xml.XmlNamespaceManager> オブジェクトをパラメーターとして取り、新しい <xref:System.Xml.XPath.XPathExpression> オブジェクトを返します。  
  
- XPath 式を表す文字列と共に <xref:System.Xml.XmlNamespaceManager> オブジェクト自体をパラメーターとして受け入れ側の <xref:System.Xml.XPath.XPathNavigator> クラス メソッドに渡す。  
  
 次は、<xref:System.Xml.XPath.XPathNavigator> インターフェイスから派生したオブジェクトをパラメーターとして受け付ける <xref:System.Xml.IXmlNamespaceResolver> クラスのメソッドです。  
  
- <xref:System.Xml.XPath.XPathNavigator.Evaluate%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.Select%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.SelectSingleNode%2A>  
  
### <a name="the-default-namespace"></a>既定の名前空間  
 次の XML ドキュメントでは、`http://www.contoso.com/books` 名前空間を宣言するために、空のプレフィックスの既定の名前空間が使用されています。  
  
```xml  
<books xmlns="http://www.contoso.com/books">  
    <book>  
        <title>Title</title>  
        <author>Author Name</author>  
        <price>5.50</price>  
    </book>  
</books>  
```  
  
 XPath は空のプレフィックスを `null` 名前空間として取り扱います。 つまり、XPath クエリでは、名前空間に割り当てられたプレフィックスだけを使用できます。 このことは、XML ドキュメント内の名前空間に対してクエリする場合、たとえそれが既定の名前空間であっても、そのプレフィックスを定義する必要があることを意味します。  
  
 たとえば、上記の XML ドキュメントでプレフィックスを定義しなければ、XPath クエリ `/books/book` は何も結果を返しません。  
  
 ドキュメントで、名前空間内にないノードと既定の名前空間内のノードに対してクエリを行う場合には、あいまいさを避けるために、プレフィックスを付加しなければなりません。  
  
 次のコードは、既定の名前空間のプレフィックスを定義し、`book` 名前空間からすべての `http://www.contoso.com/books` 要素を選択します。  
  
```vb  
Dim document As XPathDocument = New XPathDocument("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
Dim query As XPathExpression = navigator.Compile("/books:books/books:book")  
Dim manager As XmlNamespaceManager = New XmlNamespaceManager(navigator.NameTable)  
manager.AddNamespace("books", "http://www.contoso.com/books")  
query.SetContext(manager)  
Dim nodes As XPathNodeIterator = navigator.Select(query)  
```  
  
```csharp  
XPathDocument document = new XPathDocument("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
XPathExpression query = navigator.Compile("/books:books/books:book");  
XmlNamespaceManager manager = new XmlNamespaceManager(navigator.NameTable);  
manager.AddNamespace("books", "http://www.contoso.com/books");  
query.SetContext(manager);  
XPathNodeIterator nodes = navigator.Select(query);  
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [XPath データ モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-xpath-data-model.md)
- [XPathNavigator を使用した XML データの選択](../../../../docs/standard/data/xml/select-xml-data-using-xpathnavigator.md)
- [XPathNavigator による XPath 式の評価](../../../../docs/standard/data/xml/evaluate-xpath-expressions-using-xpathnavigator.md)
- [XPathNavigator によるノードの一致](../../../../docs/standard/data/xml/matching-nodes-using-xpathnavigator.md)
- [XPath クエリで認識されるノード型](../../../../docs/standard/data/xml/node-types-recognized-with-xpath-queries.md)
- [コンパイルされた XPath 式](../../../../docs/standard/data/xml/compiled-xpath-expressions.md)
