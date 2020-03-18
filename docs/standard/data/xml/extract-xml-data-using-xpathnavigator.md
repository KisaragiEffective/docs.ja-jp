---
title: XpathNavigator を使用した XML データの抽出
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 095b0987-ee4b-4595-a160-da1c956ad576
ms.openlocfilehash: 627da3c8c45d007e677c4f92f4d5cd602d34ae84
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710857"
---
# <a name="extract-xml-data-using-xpathnavigator"></a>XpathNavigator を使用した XML データの抽出
Microsoft .NET Framework において XML ドキュメントを表現する方法はいくつかあります。 これには、<xref:System.String> を使用する方法、または <xref:System.Xml.XmlReader>、<xref:System.Xml.XmlWriter>、<xref:System.Xml.XmlDocument>、<xref:System.Xml.XPath.XPathDocument> クラスを使用する方法があります。 XML ドキュメントの異なる表現の間での移行を容易にするため、<xref:System.Xml.XPath.XPathNavigator> クラスは、<xref:System.String>, <xref:System.Xml.XmlReader> オブジェクトまたは <xref:System.Xml.XmlWriter> オブジェクトとして XML を抽出するためのメソッドおよびプロパティを多数提供しています。  
  
## <a name="convert-an-xpathnavigator-to-a-string"></a>XPathNavigator から文字列への変換  
 <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> クラスの <xref:System.Xml.XPath.XPathNavigator> プロパティは、XML ドキュメント全体のマークアップ、または 1 つのノードとその子ノードのマークアップだけを取得するために使用されます。  
  
> [!NOTE]
> <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> プロパティは、ノードの子ノードのマークアップだけを取得します。  
  
 以下は、1 つのノードとその子ノードを保存する方法と、<xref:System.Xml.XPath.XPathNavigator> オブジェクトに含まれる XML ドキュメント全体を <xref:System.String> として保存する方法について説明するサンプル コードです。  
  
```vb  
Dim document As XPathDocument = New XPathDocument("input.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
' Save the entire input.xml document to a string.  
Dim xml As String = navigator.OuterXml  
  
' Now save the Root element and its child nodes to a string.  
navigator.MoveToChild(XPathNodeType.Element)  
Dim root As String = navigator.OuterXml  
```  
  
```csharp  
XPathDocument document = new XPathDocument("input.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
// Save the entire input.xml document to a string.  
string xml = navigator.OuterXml;  
  
// Now save the Root element and its child nodes to a string.  
navigator.MoveToChild(XPathNodeType.Element);  
string root = navigator.OuterXml;  
```  
  
## <a name="convert-an-xpathnavigator-to-an-xmlreader"></a>XPathNavigator から XmlReader への変換  
 <xref:System.Xml.XPath.XPathNavigator.ReadSubtree%2A> メソッドは、XML ドキュメントのコンテンツ全体、または 1 つのノードとその子ノードを <xref:System.Xml.XmlReader> オブジェクトにストリーム出力するために使用されます。  
  
 <xref:System.Xml.XmlReader> オブジェクトが現在のノードとその子で作成されている場合、<xref:System.Xml.XmlReader> オブジェクトの <xref:System.Xml.XmlReader.ReadState%2A> プロパティは <xref:System.Xml.ReadState.Initial> に設定されます。 <xref:System.Xml.XmlReader> オブジェクトの <xref:System.Xml.XmlReader.Read%2A> メソッドが初めて呼び出されたときに、<xref:System.Xml.XmlReader> が <xref:System.Xml.XPath.XPathNavigator> の現在のノードに移動されます。 新しい <xref:System.Xml.XmlReader> オブジェクトは、XML ツリーの末尾に到達するまで読み取りを継続します。 この時点で、<xref:System.Xml.XmlReader.Read%2A> メソッドは `false` を返し、<xref:System.Xml.XmlReader> オブジェクトの <xref:System.Xml.XmlReader.ReadState%2A> プロパティは <xref:System.Xml.ReadState.EndOfFile> に設定されます。  
  
 <xref:System.Xml.XPath.XPathNavigator> オブジェクトの位置は、<xref:System.Xml.XmlReader> オブジェクトの作成または移動によって変更されことはありません。 <xref:System.Xml.XPath.XPathNavigator.ReadSubtree%2A> メソッドは、要素ノードまたはルート ノードに位置している場合にだけ有効です。  
  
 1 つのノードとその子ノードを取得する方法と、<xref:System.Xml.XmlReader> オブジェクト内の XML ドキュメント全体を含む <xref:System.Xml.XPath.XPathDocument> オブジェクトを取得する方法の例を次に示します。  
  
```vb  
Dim document As XPathDocument = New XPathDocument("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
' Stream the entire XML document to the XmlReader.  
Dim xml As XmlReader = navigator.ReadSubtree()  
  
While xml.Read()  
    Console.WriteLine(xml.ReadInnerXml())  
End While  
  
xml.Close()  
  
' Stream the book element and its child nodes to the XmlReader.  
navigator.MoveToChild("bookstore", "")  
navigator.MoveToChild("book", "")  
  
Dim book As XmlReader = navigator.ReadSubtree()  
  
While book.Read()  
    Console.WriteLine(book.ReadInnerXml())  
End While  
  
book.Close()  
```  
  
```csharp  
XPathDocument document = new XPathDocument("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
// Stream the entire XML document to the XmlReader.  
XmlReader xml = navigator.ReadSubtree();  
  
while (xml.Read())  
{  
    Console.WriteLine(xml.ReadInnerXml());  
}  
  
xml.Close();  
  
// Stream the book element and its child nodes to the XmlReader.  
navigator.MoveToChild("bookstore", "");  
navigator.MoveToChild("book", "");  
  
XmlReader book = navigator.ReadSubtree();  
  
while (book.Read())  
{  
    Console.WriteLine(book.ReadInnerXml());  
}  
  
book.Close();  
```  
  
 この例は、`books.xml` ファイルを入力として使用します。  
  
 [!code-xml[XPathXMLExamples#1](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/books.xml#1)]  
  
## <a name="converting-an-xpathnavigator-to-an-xmlwriter"></a>XPathNavigator から XmlWriter への変換  
 <xref:System.Xml.XPath.XPathNavigator.WriteSubtree%2A> メソッドは、XML ドキュメントのコンテンツ全体、または 1 つのノードとその子ノードを <xref:System.Xml.XmlWriter> オブジェクトにストリーム出力するために使用されます。  
  
 <xref:System.Xml.XPath.XPathNavigator> オブジェクトの位置は、<xref:System.Xml.XmlWriter> オブジェクトの作成によって変更されことはありません。  
  
 1 つのノードとその子ノードを取得する方法と、<xref:System.Xml.XmlWriter> オブジェクト内の XML ドキュメント全体を含む <xref:System.Xml.XPath.XPathDocument> オブジェクトを取得する方法の例を次に示します。  
  
```vb  
Dim document As XPathDocument = New XPathDocument("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
' Stream the entire XML document to the XmlWriter.  
Dim xml As XmlWriter = XmlWriter.Create("newbooks.xml")  
navigator.WriteSubtree(xml)  
xml.Close()  
  
' Stream the book element and its child nodes to the XmlWriter.  
navigator.MoveToChild("bookstore", "")  
navigator.MoveToChild("book", "")  
  
Dim book As XmlWriter = XmlWriter.Create("book.xml")  
navigator.WriteSubtree(book)  
book.Close()  
```  
  
```csharp  
XPathDocument document = new XPathDocument("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
// Stream the entire XML document to the XmlWriter.  
XmlWriter xml = XmlWriter.Create("newbooks.xml");  
navigator.WriteSubtree(xml);  
xml.Close();  
  
// Stream the book element and its child nodes to the XmlWriter.  
navigator.MoveToChild("bookstore", "");  
navigator.MoveToChild("book", "");  
  
XmlWriter book = XmlWriter.Create("book.xml");  
navigator.WriteSubtree(book);  
book.Close();  
```  
  
 この例は、このトピックの前の方にある `books.xml` ファイルを入力として使用します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [XPath データ モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-xpath-data-model.md)
- [XPathNavigator を使用するノード セットのナビゲーション](../../../../docs/standard/data/xml/node-set-navigation-using-xpathnavigator.md)
- [XPathNavigator を使用する属性と名前空間のナビゲーション](../../../../docs/standard/data/xml/attribute-and-namespace-node-navigation-using-xpathnavigator.md)
- [厳密に型指定された XML データへの XPathNavigator を使用したアクセス](../../../../docs/standard/data/xml/accessing-strongly-typed-xml-data-using-xpathnavigator.md)
