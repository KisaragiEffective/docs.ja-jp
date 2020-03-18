---
title: XPathNavigator による XML データの挿入
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
ms.assetid: 2ed8c28b-b88d-4be7-9c87-92df01f0821f
ms.openlocfilehash: 68c003467d837fe79d5e275968e47fa5dc3490cc
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710727"
---
# <a name="insert-xml-data-using-xpathnavigator"></a>XPathNavigator による XML データの挿入
<xref:System.Xml.XPath.XPathNavigator> クラスは、XML ドキュメント内に兄弟ノード、子ノード、および属性ノードを挿入するためのメソッドのセットを提供します。 これらのメソッドを使用するには、<xref:System.Xml.XPath.XPathNavigator> オブジェクトが編集可能である必要があります。つまり、その <xref:System.Xml.XPath.XPathNavigator.CanEdit%2A> プロパティを `true` にする必要があります。  
  
 XML ドキュメントを編集できる <xref:System.Xml.XPath.XPathNavigator> オブジェクトは、<xref:System.Xml.XmlDocument.CreateNavigator%2A> クラスの <xref:System.Xml.XmlDocument> メソッドによって作成されます。 <xref:System.Xml.XPath.XPathNavigator> クラスによって作成される <xref:System.Xml.XPath.XPathDocument> オブジェクトは読み取り専用であり、<xref:System.Xml.XPath.XPathNavigator> オブジェクトによって作成される <xref:System.Xml.XPath.XPathDocument> オブジェクトの編集メソッドを使用しようとすると、<xref:System.NotSupportedException> が発生します。  
  
 編集可能な <xref:System.Xml.XPath.XPathNavigator> オブジェクトの作成方法については、「[XPathDocument および XmlDocument を使用した XML データの読み取り](../../../../docs/standard/data/xml/reading-xml-data-using-xpathdocument-and-xmldocument.md)」を参照してください。  
  
## <a name="inserting-nodes"></a>ノードの挿入  
 <xref:System.Xml.XPath.XPathNavigator> クラスは、XML ドキュメント内に兄弟ノード、子ノード、および属性ノードを挿入するためのメソッドを提供します。 これらのメソッドによって、<xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置に関連した異なる場所にノードまたは属性を挿入できます。これらのメソッドについて以下に説明します。  
  
### <a name="inserting-sibling-nodes"></a>兄弟ノードの挿入  
 <xref:System.Xml.XPath.XPathNavigator> クラスは、兄弟ノードを挿入する次のメソッドを提供します。  
  
- <xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.InsertElementAfter%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.InsertElementBefore%2A>  
  
 これらのメソッドは <xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置にあるノードの前と後に兄弟ノードを挿入します。  
  
 <xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A> および <xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A> メソッドは、オーバーロードされ、`string`、<xref:System.Xml.XmlReader> オブジェクト、または追加する兄弟ノードをパラメーターとして含む <xref:System.Xml.XPath.XPathNavigator> オブジェクトを受け取ります。 両方のメソッドは、兄弟ノードの挿入に使用される <xref:System.Xml.XmlWriter> オブジェクトも返します。  
  
 <xref:System.Xml.XPath.XPathNavigator.InsertElementAfter%2A> および <xref:System.Xml.XPath.XPathNavigator.InsertElementBefore%2A> メソッドは、パラメーターとして指定された名前空間プレフィックス、ローカル名、名前空間 URI、および値を使用して <xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置にあるノードの前と後に、1 つの兄弟ノードを挿入します。  
  
 次の例では、新しい `pages` 要素が `price` ファイル内の最初の `book` 要素の `contosoBooks.xml` 子要素の前に挿入されます。  
  
 [!code-cpp[XPathNavigatorMethods#19](../../../../samples/snippets/cpp/VS_Snippets_Data/XPathNavigatorMethods/CPP/xpathnavigatormethods.cpp#19)]
 [!code-csharp[XPathNavigatorMethods#19](../../../../samples/snippets/csharp/VS_Snippets_Data/XPathNavigatorMethods/CS/xpathnavigatormethods.cs#19)]
 [!code-vb[XPathNavigatorMethods#19](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XPathNavigatorMethods/VB/xpathnavigatormethods.vb#19)]  
  
 この例は、`contosoBooks.xml` ファイルを入力として使用します。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 <xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A>、<xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A>、<xref:System.Xml.XPath.XPathNavigator.InsertElementAfter%2A>、および <xref:System.Xml.XPath.XPathNavigator.InsertElementBefore%2A> メソッドの詳細については、<xref:System.Xml.XPath.XPathNavigator> クラスのリファレンス ドキュメントを参照してください。  
  
### <a name="inserting-child-nodes"></a>子ノードの挿入  
 <xref:System.Xml.XPath.XPathNavigator> クラスは、子ノードを挿入する次のメソッドを提供します。  
  
- <xref:System.Xml.XPath.XPathNavigator.AppendChild%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.AppendChildElement%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.PrependChildElement%2A>  
  
 これらのメソッドは、<xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置にあるノードの一連の子ノードの最後と最初に、子ノードを追加します。  
  
 「兄弟ノードの挿入」のメソッドと同様、<xref:System.Xml.XPath.XPathNavigator.AppendChild%2A> および <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A> メソッドは、`string`、<xref:System.Xml.XmlReader> オブジェクト、または追加する子ノードをパラメーターとして含む <xref:System.Xml.XPath.XPathNavigator> オブジェクトを受け取ります。 両方のメソッドは、子ノードの挿入に使用される <xref:System.Xml.XmlWriter> オブジェクトも返します。  
  
 また、「兄弟ノードの挿入」のメソッドと同様、<xref:System.Xml.XPath.XPathNavigator.AppendChildElement%2A> および <xref:System.Xml.XPath.XPathNavigator.PrependChildElement%2A> メソッドは、パラメーターとして指定された名前空間プレフィックス、ローカル名、名前空間 URI、および値を使用して <xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置にあるノードの一連の子ノードの最初と最後に 1 つの子ノードを追加します。  
  
 次の例では、新しい `pages` 子要素が `book` ファイル内の最初の `contosoBooks.xml` 要素の一連の子要素に追加されます。  
  
 [!code-cpp[XPathNavigatorMethods#2](../../../../samples/snippets/cpp/VS_Snippets_Data/XPathNavigatorMethods/CPP/xpathnavigatormethods.cpp#2)]
 [!code-csharp[XPathNavigatorMethods#2](../../../../samples/snippets/csharp/VS_Snippets_Data/XPathNavigatorMethods/CS/xpathnavigatormethods.cs#2)]
 [!code-vb[XPathNavigatorMethods#2](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XPathNavigatorMethods/VB/xpathnavigatormethods.vb#2)]  
  
 この例は、`contosoBooks.xml` ファイルを入力として使用します。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 <xref:System.Xml.XPath.XPathNavigator.AppendChild%2A>、<xref:System.Xml.XPath.XPathNavigator.PrependChild%2A>、<xref:System.Xml.XPath.XPathNavigator.AppendChildElement%2A>、および <xref:System.Xml.XPath.XPathNavigator.PrependChildElement%2A> メソッドの詳細については、<xref:System.Xml.XPath.XPathNavigator> クラスのリファレンス ドキュメントを参照してください。  
  
### <a name="inserting-attribute-nodes"></a>属性ノードの挿入  
 <xref:System.Xml.XPath.XPathNavigator> クラスは、属性ノードを挿入する次のメソッドを提供します。  
  
- <xref:System.Xml.XPath.XPathNavigator.CreateAttribute%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.CreateAttributes%2A>  
  
 これらのメソッドは <xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置にある要素ノードに属性ノードを挿入します。 <xref:System.Xml.XPath.XPathNavigator.CreateAttribute%2A> メソッドは、パラメーターとして指定された名前空間プレフィックス、ローカル名、名前空間 URI、および値を使用して <xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置にある要素ノードに、属性ノードを作成します。 <xref:System.Xml.XPath.XPathNavigator.CreateAttributes%2A> メソッドは、属性ノードの挿入に使用される <xref:System.Xml.XmlWriter> オブジェクトも返します。  
  
 次の例では、`discount` メソッドから返された `currency` オブジェクトを使用して、新しい `price` および `book` 属性が `contosoBooks.xml` ファイル内の最初の <xref:System.Xml.XmlWriter> 要素の <xref:System.Xml.XPath.XPathNavigator.CreateAttributes%2A> 子要素に作成されます。  
  
 [!code-cpp[XPathNavigatorMethods#8](../../../../samples/snippets/cpp/VS_Snippets_Data/XPathNavigatorMethods/CPP/xpathnavigatormethods.cpp#8)]
 [!code-csharp[XPathNavigatorMethods#8](../../../../samples/snippets/csharp/VS_Snippets_Data/XPathNavigatorMethods/CS/xpathnavigatormethods.cs#8)]
 [!code-vb[XPathNavigatorMethods#8](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XPathNavigatorMethods/VB/xpathnavigatormethods.vb#8)]  
  
 この例は、`contosoBooks.xml` ファイルを入力として使用します。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 <xref:System.Xml.XPath.XPathNavigator.CreateAttribute%2A> および <xref:System.Xml.XPath.XPathNavigator.CreateAttributes%2A> メソッドの詳細については、<xref:System.Xml.XPath.XPathNavigator> クラスのリファレンス ドキュメントを参照してください。  
  
## <a name="copying-nodes"></a>ノードのコピー  
 別の XML ドキュメントの内容を使用して XML ドキュメントを作成する場合があります。 <xref:System.Xml.XPath.XPathNavigator> クラスおよび <xref:System.Xml.XmlWriter> クラスは、既存の <xref:System.Xml.XmlDocument> オブジェクトまたは <xref:System.Xml.XmlReader> オブジェクトから <xref:System.Xml.XPath.XPathNavigator> オブジェクトへノードをコピーできます。  
  
 <xref:System.Xml.XPath.XPathNavigator.AppendChild%2A> クラスの <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A>、<xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A>、<xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A>、および <xref:System.Xml.XPath.XPathNavigator> メソッドにはすべて、パラメーターとして <xref:System.Xml.XPath.XPathNavigator> オブジェクトまたは <xref:System.Xml.XmlReader> オブジェクトの受け取りが可能なオーバーロードがあります。  
  
 <xref:System.Xml.XmlWriter.WriteNode%2A> クラスの <xref:System.Xml.XmlWriter> メソッドには、<xref:System.Xml.XmlNode>、<xref:System.Xml.XmlReader>、または <xref:System.Xml.XPath.XPathNavigator> オブジェクトの受け取りが可能なオーバーロードがあります。  
  
 次の例では、あるドキュメントからすべての `book` 要素を別のドキュメントにコピーします。  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
document.Load("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", String.Empty)  
  
Dim newBooks As XPathDocument = New XPathDocument("newBooks.xml")  
Dim newBooksNavigator As XPathNavigator = newBooks.CreateNavigator()  
  
Dim nav As XPathNavigator  
For Each nav in newBooksNavigator.SelectDescendants("book", "", false)  
    navigator.AppendChild(nav)  
Next  
  
document.Save("newBooks.xml");  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
document.Load("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
navigator.MoveToChild("bookstore", String.Empty);  
  
XPathDocument newBooks = new XPathDocument("newBooks.xml");  
XPathNavigator newBooksNavigator = newBooks.CreateNavigator();  
  
foreach (XPathNavigator nav in newBooksNavigator.SelectDescendants("book", "", false))  
{  
    navigator.AppendChild(nav);  
}  
  
document.Save("newBooks.xml");  
```  
  
## <a name="inserting-values"></a>値の挿入  
 <xref:System.Xml.XPath.XPathNavigator> クラスは、<xref:System.Xml.XPath.XPathNavigator.SetValue%2A> オブジェクトにノードの値を挿入する <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> および <xref:System.Xml.XmlDocument> メソッドを提供しています。  
  
### <a name="inserting-untyped-values"></a>型指定されていない値の挿入  
 <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> メソッドは、パラメーターとして渡された型指定されていない `string` 値を挿入するだけです。このパラメーターは、<xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置にあるノードの値です。 値に型は設定されず、スキーマ情報が使用可能な場合でも、ノードの型に対して新しい値が有効どうかを検証せずに挿入されます。  
  
 <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> メソッドを使用して `price` フィァイル内のすべての `contosoBooks.xml` 要素を更新する例を次に示します。  
  
 [!code-cpp[XPathNavigatorMethods#47](../../../../samples/snippets/cpp/VS_Snippets_Data/XPathNavigatorMethods/CPP/xpathnavigatormethods.cpp#47)]
 [!code-csharp[XPathNavigatorMethods#47](../../../../samples/snippets/csharp/VS_Snippets_Data/XPathNavigatorMethods/CS/xpathnavigatormethods.cs#47)]
 [!code-vb[XPathNavigatorMethods#47](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XPathNavigatorMethods/VB/xpathnavigatormethods.vb#47)]  
  
 この例は、`contosoBooks.xml` ファイルを入力として使用します。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
### <a name="inserting-typed-values"></a>型指定された値の挿入  
 ノードの型が W3C XML スキーマの単純型の場合、<xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> メソッドによって挿入される新しい値は、値の設定前に、単純型のファセットに対してチェックされます。 新しい値がノードの型に対して無効な場合 (たとえば、型が `-1` の要素に、値 `xs:positiveInteger` を設定するような場合)、例外が返されます。  
  
 次の例では、`price` ファイル内の最初の `book` 要素の `contosoBooks.xml` 要素の値を <xref:System.DateTime> 値に変更しようとしています。 `price` 要素の XML スキーマ型は、`xs:decimal` ファイル内で `contosoBooks.xsd` として定義されているため、結果は例外になります。  
  
```vb  
Dim settings As XmlReaderSettings = New XmlReaderSettings()  
settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd")  
settings.ValidationType = ValidationType.Schema  
  
Dim reader As XmlReader = XmlReader.Create("contosoBooks.xml", settings)  
  
Dim document As XmlDocument = New XmlDocument()  
document.Load(reader)  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books")  
navigator.MoveToChild("book", "http://www.contoso.com/books")  
navigator.MoveToChild("price", "http://www.contoso.com/books")  
  
navigator.SetTypedValue(DateTime.Now)  
```  
  
```csharp  
XmlReaderSettings settings = new XmlReaderSettings();  
settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd");  
settings.ValidationType = ValidationType.Schema;  
  
XmlReader reader = XmlReader.Create("contosoBooks.xml", settings);  
  
XmlDocument document = new XmlDocument();  
document.Load(reader);  
XPathNavigator navigator = document.CreateNavigator();  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books");  
navigator.MoveToChild("book", "http://www.contoso.com/books");  
navigator.MoveToChild("price", "http://www.contoso.com/books");  
  
navigator.SetTypedValue(DateTime.Now);  
```  
  
 この例は、`contosoBooks.xml` ファイルを入力として使用します。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 また、`contosoBooks.xsd` ファイルも入力として使用します。  
  
 [!code-xml[XPathXMLExamples#3](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xsd#3)]  
  
## <a name="the-innerxml-and-outerxml-properties"></a>InnerXml および OuterXml プロパティ  
 <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> クラスの <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> および <xref:System.Xml.XPath.XPathNavigator> プロパティは、<xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置にある XML マークアップを変更します。  
  
 <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> プロパティは、与えられた XML <xref:System.Xml.XPath.XPathNavigator> の解析済みの内容を使用して `string` オブジェクトの現在位置にある子ノードの XML マークアップを変更します。 同様に、<xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> プロパティは、<xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置にある子ノードと現在のノード自体の XML マークアップを変更します。  
  
 このトピックで説明したメソッドに加えて、<xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> プロパティと <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> プロパティは、XML ドキュメントからノードに値を挿入するために使用できます。 <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> プロパティと <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> プロパティを使用してノードと値を挿入する方法の詳細については「[XpathNavigator による XML データの変更](../../../../docs/standard/data/xml/modify-xml-data-using-xpathnavigator.md)」のトピックを参照してください。  
  
## <a name="namespace-and-xmllang-conflicts"></a>名前空間と xml:lang の競合  
 パラメーターとして `xml:lang` オブジェクトを受け取る <xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A> クラスの <xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A>、<xref:System.Xml.XPath.XPathNavigator.AppendChild%2A>、<xref:System.Xml.XPath.XPathNavigator.PrependChild%2A>、および <xref:System.Xml.XPath.XPathNavigator> メソッドを使用して XML データを挿入する場合、名前空間のスコープと <xref:System.Xml.XmlReader> 宣言に関連する競合が発生する可能性があります。  
  
 発生する可能性のある名前空間の競合は次のとおりです。  
  
- <xref:System.Xml.XmlReader> オブジェクトのコンテキスト内のスコープに名前空間があり、プレフィックスから名前空間 URI へのマッピングが <xref:System.Xml.XPath.XPathNavigator> オブジェクトのコンテキスト内にない場合、新たに挿入されたノードに関する新しい名前空間宣言が追加されます。  
  
- <xref:System.Xml.XmlReader> オブジェクトのコンテキスト内と <xref:System.Xml.XPath.XPathNavigator> オブジェクトのコンテキスト内の両方のスコープに同じ名前空間 URI があるが、両方のコンテキスト内で別のプレフィックスにマップされている場合、<xref:System.Xml.XmlReader> オブジェクトからのプレフィックスと名前空間 URI を使用して、新たに挿入されたノードに対する新しい名前空間宣言が追加されます。  
  
- <xref:System.Xml.XmlReader> オブジェクトのコンテキスト内と <xref:System.Xml.XPath.XPathNavigator> オブジェクトのコンテキスト内の両方のスコープに同じ名前空間プレフィックスがあるが、両方のコンテキスト内で別の名前空間 URI にマップされている場合、新たに挿入されたノードに対する新しい名前空間宣言が追加され、<xref:System.Xml.XmlReader> オブジェクトからの名前空間 URI を使用してそのプレフィックスを再宣言します。  
  
- <xref:System.Xml.XmlReader> オブジェクトのコンテキスト内と <xref:System.Xml.XPath.XPathNavigator> オブジェクトのコンテキスト内の両方で、プレフィックスと名前空間 URI が同じ場合、新たに挿入されたノードに対する新しい名前空間宣言は追加されません。  
  
> [!NOTE]
> 上記の説明は、プレフィックスとして空の `string` (たとえば、既定の名前空間宣言) を使用した名前空間宣言にも適用されます。  
  
 発生する可能性のある `xml:lang` の競合は次のとおりです。  
  
- `xml:lang` 属性が <xref:System.Xml.XmlReader> オブジェクトのコンテキスト内のスコープにあるが、<xref:System.Xml.XPath.XPathNavigator> オブジェクトのコンテキスト内にはない場合、`xml:lang` オブジェクトから受け取った値を持つ <xref:System.Xml.XmlReader> 属性が、新たに挿入されたノードに対して追加されます。  
  
- `xml:lang` 属性が <xref:System.Xml.XmlReader> オブジェクトのコンテキスト内と <xref:System.Xml.XPath.XPathNavigator> オブジェクトのコンテキスト内の両方のスコープにあるが、それぞれの値が異なる場合、`xml:lang` オブジェクトから受け取った値を持つ <xref:System.Xml.XmlReader> 属性が、新たに挿入されたノードに対して追加されます。  
  
- `xml:lang` 属性が <xref:System.Xml.XmlReader> オブジェクトのコンテキスト内と <xref:System.Xml.XPath.XPathNavigator> オブジェクトのコンテキスト内の両方のスコープにあるが、それぞれの値が同じ場合、新たに挿入されたノードに対して新しい `xml:lang` 属性は追加されません。  
  
- `xml:lang` 属性が <xref:System.Xml.XPath.XPathNavigator> オブジェクトのコンテキスト内のスコープにあるが、<xref:System.Xml.XmlReader> オブジェクトのコンテキスト内にはない場合、新たに挿入されたノードに対して `xml:lang` 属性は追加されません。  
  
## <a name="inserting-nodes-with-xmlwriter"></a>XmlWriter によるノードの挿入  
 「ノードと値の挿入」に記載されている兄弟ノード、子ノード、および属性ノードの挿入に使用されるメソッドはオーバーロードされます。 <xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A> クラスの <xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A>、<xref:System.Xml.XPath.XPathNavigator.AppendChild%2A>、<xref:System.Xml.XPath.XPathNavigator.PrependChild%2A>、<xref:System.Xml.XPath.XPathNavigator.CreateAttributes%2A>、および <xref:System.Xml.XPath.XPathNavigator> メソッドは、ノードの挿入に使用される <xref:System.Xml.XmlWriter> オブジェクトを返します。  
  
### <a name="unsupported-xmlwriter-methods"></a>サポートされていない XmlWriter メソッド  
 XPath データ モデルとドキュメント オブジェクト モデル (DOM) には相違点があるため、<xref:System.Xml.XmlWriter> クラスによる XML ドキュメントへの情報の書き込みで使用されるメソッドのすべてが <xref:System.Xml.XPath.XPathNavigator> クラスによってサポートされているわけではありません。  
  
 <xref:System.Xml.XmlWriter> クラスによってサポートされない <xref:System.Xml.XPath.XPathNavigator> クラス メソッドについて次の表で説明します。  
  
|メソッド|説明|  
|------------|-----------------|  
|<xref:System.Xml.XmlWriter.WriteEntityRef%2A>|<xref:System.NotSupportedException> 例外をスローします。|  
|<xref:System.Xml.XmlWriter.WriteDocType%2A>|ルート レベルでは無視され、XML ドキュメント内の他のレベルで呼び出された場合は、<xref:System.NotSupportedException> 例外をスローします。|  
|<xref:System.Xml.XmlWriter.WriteCData%2A>|同じ文字または文字列についての <xref:System.Xml.XmlWriter.WriteString%2A> メソッドの呼び出しとして扱われます。|  
|<xref:System.Xml.XmlWriter.WriteCharEntity%2A>|同じ文字または文字列についての <xref:System.Xml.XmlWriter.WriteString%2A> メソッドの呼び出しとして扱われます。|  
|<xref:System.Xml.XmlWriter.WriteSurrogateCharEntity%2A>|同じ文字または文字列についての <xref:System.Xml.XmlWriter.WriteString%2A> メソッドの呼び出しとして扱われます。|  
  
 <xref:System.Xml.XmlWriter> クラスに関する詳細については、<xref:System.Xml.XmlWriter> クラスのリファレンス ドキュメントを参照してください。  
  
### <a name="multiple-xmlwriter-objects"></a>複数の XmlWriter オブジェクト  
 1 つ以上の開いた <xref:System.Xml.XPath.XPathNavigator> オブジェクトを使用して、XML ドキュメントの異なる部分を指す複数の <xref:System.Xml.XmlWriter> オブジェクトを持つことができます。 複数の <xref:System.Xml.XmlWriter> オブジェクトは、単一スレッドのシナリオで許可されサポートされます。  
  
 以下は、複数の <xref:System.Xml.XmlWriter> オブジェクト使用時の重要な注意事項です。  
  
- それぞれの <xref:System.Xml.XmlWriter> オブジェクトの <xref:System.Xml.XmlWriter.Close%2A> メソッドが呼び出されるときに、<xref:System.Xml.XmlWriter> オブジェクトによって書き込まれる XML フラグメントが XML ドキュメントに追加されます。 その時点まで、<xref:System.Xml.XmlWriter> オブジェクトは、まとまっていないフラグメントを書き込んでいます。 XML ドキュメントに対する操作が実行される場合、<xref:System.Xml.XmlWriter> の呼び出し前に <xref:System.Xml.XmlWriter.Close%2A> オブジェクトによって書き込まれているフラグメントは影響を受けません。  
  
- 特定の XML サブツリー上に、開いた <xref:System.Xml.XmlWriter> オブジェクトがあり、そのサブツリーが削除された場合、<xref:System.Xml.XmlWriter> オブジェクトが引き続きそのサブツリーに追加する可能性があります。 サブツリーは単に削除済みフラグメントになります。  
  
- XML ドキュメント内の同じ位置で、複数の <xref:System.Xml.XmlWriter> オブジェクトが開かれた場合、<xref:System.Xml.XmlWriter> オブジェクトが開かれた順序ではなく、それらが閉じられる順序で XML ドキュメントにそれらが追加されます。  
  
 次の例では、<xref:System.Xml.XmlDocument> オブジェクトを作成し、<xref:System.Xml.XPath.XPathNavigator> オブジェクトを作成した後、<xref:System.Xml.XmlWriter> メソッドによって返される <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A> オブジェクトを使用して、`books.xml` ファイル内に最初の book 構造体を作成します。 この例は、それを `book.xml` ファイルとして保存します。  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
Using writer As XmlWriter = navigator.PrependChild()  
  
    writer.WriteStartElement("bookstore")  
    writer.WriteStartElement("book")  
    writer.WriteAttributeString("genre", "autobiography")  
    writer.WriteAttributeString("publicationdate", "1981-03-22")  
    writer.WriteAttributeString("ISBN", "1-861003-11-0")  
    writer.WriteElementString("title", "The Autobiography of Benjamin Franklin")  
    writer.WriteStartElement("author")  
    writer.WriteElementString("first-name", "Benjamin")  
    writer.WriteElementString("last-name", "Franklin")  
    writer.WriteElementString("price", "8.99")  
    writer.WriteEndElement()  
    writer.WriteEndElement()  
    writer.WriteEndElement()  
  
End Using  
  
document.Save("book.xml")  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
XPathNavigator navigator = document.CreateNavigator();  
  
using (XmlWriter writer = navigator.PrependChild())  
{  
    writer.WriteStartElement("bookstore");  
    writer.WriteStartElement("book");  
    writer.WriteAttributeString("genre", "autobiography");  
    writer.WriteAttributeString("publicationdate", "1981-03-22");  
    writer.WriteAttributeString("ISBN", "1-861003-11-0");  
    writer.WriteElementString("title", "The Autobiography of Benjamin Franklin");  
    writer.WriteStartElement("author");  
    writer.WriteElementString("first-name", "Benjamin");  
    writer.WriteElementString("last-name", "Franklin");  
    writer.WriteElementString("price", "8.99");  
    writer.WriteEndElement();  
    writer.WriteEndElement();  
    writer.WriteEndElement();  
}  
document.Save("book.xml");  
```  
  
## <a name="saving-an-xml-document"></a>XML ドキュメントの保存  
 ここに記載されているメソッドによる <xref:System.Xml.XmlDocument> オブジェクトに対する変更の保存は、<xref:System.Xml.XmlDocument> クラスのメソッドを使用して実行されます。 <xref:System.Xml.XmlDocument> オブジェクトに対する変更の保存に関する詳細については、「[ドキュメントの保存と書き込み](../../../../docs/standard/data/xml/saving-and-writing-a-document.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [XPath データ モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-xpath-data-model.md)
- [XpathNavigator による XML データの変更](../../../../docs/standard/data/xml/modify-xml-data-using-xpathnavigator.md)
- [XPathNavigator による XML データの削除](../../../../docs/standard/data/xml/remove-xml-data-using-xpathnavigator.md)
