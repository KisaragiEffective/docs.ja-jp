---
title: XPathNavigator による XML データの削除
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 9f436bca-1b96-494b-a6d2-e102c7551752
ms.openlocfilehash: 062a98a9cc10e6be00f165cf617de2d92d65acbf
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710363"
---
# <a name="remove-xml-data-using-xpathnavigator"></a>XPathNavigator による XML データの削除
<xref:System.Xml.XPath.XPathNavigator> クラスは、XML ドキュメントからノードを削除するためのメソッドのセットを提供します。 これらのメソッドを使用するには、<xref:System.Xml.XPath.XPathNavigator> オブジェクトが編集可能である必要があります。つまり、その <xref:System.Xml.XPath.XPathNavigator.CanEdit%2A> プロパティを `true` にする必要があります。  
  
 XML ドキュメントを編集できる <xref:System.Xml.XPath.XPathNavigator> オブジェクトは、<xref:System.Xml.XmlDocument.CreateNavigator%2A> クラスの <xref:System.Xml.XmlDocument> メソッドによって作成されます。 <xref:System.Xml.XPath.XPathNavigator> クラスによって作成される <xref:System.Xml.XPath.XPathDocument> オブジェクトは読み取り専用であり、<xref:System.Xml.XPath.XPathNavigator> オブジェクトによって作成される <xref:System.Xml.XPath.XPathDocument> オブジェクトの編集メソッドを使用しようとすると、<xref:System.NotSupportedException> が発生します。  
  
 編集可能な <xref:System.Xml.XPath.XPathNavigator> オブジェクトの作成方法については、「[XPathDocument および XmlDocument を使用した XML データの読み取り](../../../../docs/standard/data/xml/reading-xml-data-using-xpathdocument-and-xmldocument.md)」を参照してください。  
  
## <a name="removing-nodes"></a>ノードの削除  
 <xref:System.Xml.XPath.XPathNavigator> クラスは、XML ドキュメントからノードを削除する <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> メソッドを提供します。  
  
### <a name="removing-a-node"></a>1 つのノードの削除  
 <xref:System.Xml.XPath.XPathNavigator> クラスは、XML ドキュメントから、<xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> オブジェクトが現在位置している現在のノードを削除する <xref:System.Xml.XPath.XPathNavigator> メソッドを提供します。  
  
 <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> メソッドを使用してノードを削除した後は、<xref:System.Xml.XmlDocument> オブジェクトのルートからそのノードに到達することはできません。 ノードの削除後、<xref:System.Xml.XPath.XPathNavigator> は削除されたノードの親ノード上に位置します。  
  
 削除操作により、削除されたノード上に位置していた <xref:System.Xml.XPath.XPathNavigator> オブジェクトの位置に影響はありません。 これらの <xref:System.Xml.XPath.XPathNavigator> オブジェクトは削除されたサブツリー内を移動できるという点で有効ですが、<xref:System.Xml.XPath.XPathNavigator> クラスの通常のノード セットの移動メソッドを使用して、主ノード ツリーには移動できません。  
  
> [!NOTE]
> <xref:System.Xml.XPath.XPathNavigator.MoveTo%2A> クラスの <xref:System.Xml.XPath.XPathNavigator> メソッドは、これらの <xref:System.Xml.XPath.XPathNavigator> オブジェクトを主ノード ツリーに戻したり、主ノード ツリーから削除されたサブツリーに移動したりするために使用できます。  
  
 次の例では、`price` ファイルの最初の `book` 要素の `contosoBooks.xml` 要素を、<xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> メソッドを使用して削除します。 <xref:System.Xml.XPath.XPathNavigator> 要素が削除された後の `price` オブジェクトの位置は、親の `book` 要素上です。  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
document.Load("contosoBooks.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books")  
navigator.MoveToChild("book", "http://www.contoso.com/books")  
navigator.MoveToChild("price", "http://www.contoso.com/books")  
  
navigator.DeleteSelf()  
  
Console.WriteLine("Position after delete: {0}", navigator.Name)  
Console.WriteLine(navigator.OuterXml)  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
document.Load("contosoBooks.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books");  
navigator.MoveToChild("book", "http://www.contoso.com/books");  
navigator.MoveToChild("price", "http://www.contoso.com/books");  
  
navigator.DeleteSelf();  
  
Console.WriteLine("Position after delete: {0}", navigator.Name);  
Console.WriteLine(navigator.OuterXml);  
```  
  
 この例は、`contosoBooks.xml` ファイルを入力として使用します。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
### <a name="removing-an-attribute-node"></a>属性ノードの削除  
 属性ノードは <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> メソッドを使用して XML ドキュメントから削除します。  
  
 削除後、<xref:System.Xml.XmlDocument> オブジェクトのルート ノードから属性ノードには到達できません。<xref:System.Xml.XPath.XPathNavigator> オブジェクトの位置は親要素上になります。  
  
#### <a name="default-attributes"></a>既定の属性  
 XML ドキュメントの既定の属性として、DTD または XML スキーマに定義されている属性を削除するときは、使用するメソッドにかかわらず、特別な制限が適用されます。 既定の属性は、その属性が含まれている要素も削除しない限り削除できません。 既定の属性が宣言されている要素に既定の属性は常に存在します。その結果、既定の属性を削除すると、その要素に代替の属性が挿入され、宣言された既定の値に初期化されます。  
  
## <a name="removing-values"></a>値の削除  
 <xref:System.Xml.XPath.XPathNavigator> クラスには、XML ドキュメントから型指定された値と型指定されていない値を削除する <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> メソッドと <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> メソッドがあります。  
  
### <a name="removing-untyped-values"></a>型指定されていない値の削除  
 <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> メソッドは、パラメーターとして渡された型指定されていない `string` 値を挿入するだけです。このパラメーターは、<xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置にあるノードの値です。 <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> メソッドに空の文字列を渡すと、現在のノードの値が削除されます。  
  
 次の例では、`price` メソッドを使用して、`book` ファイル内の最初の `contosoBooks.xml` 要素の <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> 要素の値を削除します。  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
document.Load("contosoBooks.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books")  
navigator.MoveToChild("book", "http://www.contoso.com/books")  
navigator.MoveToChild("price", "http://www.contoso.com/books")  
  
navigator.SetValue("")  
  
navigator.MoveToRoot()  
Console.WriteLine(navigator.OuterXml)  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
document.Load("contosoBooks.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books");  
navigator.MoveToChild("book", "http://www.contoso.com/books");  
navigator.MoveToChild("price", "http://www.contoso.com/books");  
  
navigator.SetValue("");  
  
navigator.MoveToRoot();  
Console.WriteLine(navigator.OuterXml);  
```  
  
 この例は、`contosoBooks.xml` ファイルを入力として使用します。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
### <a name="removing-typed-values"></a>型指定された値の削除  
 ノードの型が W3C XML スキーマの単純型の場合、<xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> メソッドによって挿入される新しい値は、値の設定前に、単純型のファセットに対してチェックされます。 新しい値がノードの型に対して無効な場合 (たとえば、型が `-1` の要素に、値 `xs:positiveInteger` を設定するような場合)、例外が返されます。 <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> メソッドにもパラメーターとして `null` を渡すことはできません。 結果として、型指定されたノードの値の削除は、ノードのスキーマの型に従う必要があります。  
  
 次の例では、`price` メソッドを使用して、値を `book` に設定することにより、`contosoBooks.xml` ファイル内の最初の <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> 要素の `0` 要素の値を削除します。 ノードの値は削除されませんが、本の価格はそのデータ型 `xs:decimal` に従って削除されました。  
  
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
  
navigator.SetTypedValue(0)  
  
navigator.MoveToRoot()  
Console.WriteLine(navigator.OuterXml)  
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
  
navigator.SetTypedValue(0);  
  
navigator.MoveToRoot();  
Console.WriteLine(navigator.OuterXml);  
```  
  
## <a name="namespace-nodes"></a>名前空間ノード  
 名前空間ノードは <xref:System.Xml.XmlDocument> オブジェクトから削除できません。 <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> メソッドを使用して名前空間ノードを削除しようとすると、例外が発生します。  
  
## <a name="the-innerxml-and-outerxml-properties"></a>InnerXml および OuterXml プロパティ  
 <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> クラスの <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> および <xref:System.Xml.XPath.XPathNavigator> プロパティは、<xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置にある XML マークアップを変更します。  
  
 <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> プロパティは、与えられた XML <xref:System.Xml.XPath.XPathNavigator> の解析済みの内容を使用して `string` オブジェクトの現在位置にある子ノードの XML マークアップを変更します。 同様に、<xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> プロパティは、<xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置にある子ノードと現在のノード自体の XML マークアップを変更します。  
  
 このトピックで説明したメソッドに加えて、<xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> プロパティと <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> プロパティは、XML ドキュメントからノードと値を削除するために使用できます。 <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> プロパティと <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> プロパティを使用してノードを変更する方法の詳細については「[XpathNavigator による XML データの変更](../../../../docs/standard/data/xml/modify-xml-data-using-xpathnavigator.md)」のトピックを参照してください。  
  
## <a name="saving-an-xml-document"></a>XML ドキュメントの保存  
 ここに記載されているメソッドによる <xref:System.Xml.XmlDocument> オブジェクトに対する変更の保存は、<xref:System.Xml.XmlDocument> クラスのメソッドを使用して実行されます。 <xref:System.Xml.XmlDocument> オブジェクトに対する変更の保存に関する詳細については、「[ドキュメントの保存と書き込み](../../../../docs/standard/data/xml/saving-and-writing-a-document.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [XPath データ モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-xpath-data-model.md)
- [XPathNavigator による XML データの挿入](../../../../docs/standard/data/xml/insert-xml-data-using-xpathnavigator.md)
- [XpathNavigator による XML データの変更](../../../../docs/standard/data/xml/modify-xml-data-using-xpathnavigator.md)
