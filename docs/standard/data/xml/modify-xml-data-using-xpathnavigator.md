---
title: XpathNavigator による XML データの変更
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
ms.assetid: 03a7c5a1-b296-4af4-b209-043c958dc0a5
ms.openlocfilehash: 906de1ded4961b7c67d48a010555d139df97cded
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710636"
---
# <a name="modify-xml-data-using-xpathnavigator"></a>XpathNavigator による XML データの変更
<xref:System.Xml.XPath.XPathNavigator> クラスは、XML ドキュメント内のノードを変更するためのメソッドのセットを提供します。 これらのメソッドを使用するには、<xref:System.Xml.XPath.XPathNavigator> オブジェクトが編集可能である必要があります。つまり、その <xref:System.Xml.XPath.XPathNavigator.CanEdit%2A> プロパティを `true` にする必要があります。  
  
 XML ドキュメントを編集できる <xref:System.Xml.XPath.XPathNavigator> オブジェクトは、<xref:System.Xml.XmlDocument.CreateNavigator%2A> クラスの <xref:System.Xml.XmlDocument> メソッドによって作成されます。 <xref:System.Xml.XPath.XPathNavigator> クラスによって作成される <xref:System.Xml.XPath.XPathDocument> オブジェクトは読み取り専用で、<xref:System.Xml.XPath.XPathNavigator> オブジェクトによって作成される <xref:System.Xml.XPath.XPathDocument> オブジェクトの編集メソッドを使用しようとすると、<xref:System.NotSupportedException> が発生します。  
  
 編集可能な <xref:System.Xml.XPath.XPathNavigator> オブジェクトの作成方法については、「[XPathDocument および XmlDocument を使用した XML データの読み取り](../../../../docs/standard/data/xml/reading-xml-data-using-xpathdocument-and-xmldocument.md)」を参照してください。  
  
## <a name="modifying-nodes"></a>ノードの変更  
 ノードの値を簡単に変更するには、<xref:System.Xml.XPath.XPathNavigator.SetValue%2A> と <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> クラスの <xref:System.Xml.XPath.XPathNavigator> メソッドを使用します。  
  
 次の表は、異なるノード型に対するこれらメソッドの効果の一覧です。  
  
|<xref:System.Xml.XPath.XPathNodeType>|変更されるデータ。|  
|---------------------------------------------------------------------------------------------------------------------------------------------|------------------|  
|<xref:System.Xml.XPath.XPathNodeType.Root>|サポートされていません。|  
|<xref:System.Xml.XPath.XPathNodeType.Element>|要素のコンテンツ。|  
|<xref:System.Xml.XPath.XPathNodeType.Attribute>|属性の値。|  
|<xref:System.Xml.XPath.XPathNodeType.Text>|テキスト コンテンツ。|  
|<xref:System.Xml.XPath.XPathNodeType.ProcessingInstruction>|ターゲットを除くコンテンツ。|  
|<xref:System.Xml.XPath.XPathNodeType.Comment>|コメントの内容。|  
|<xref:System.Xml.XPath.XPathNodeType.Namespace>|サポート範囲外。|  
  
> [!NOTE]
> <xref:System.Xml.XPath.XPathNodeType.Namespace> ノードまたは <xref:System.Xml.XPath.XPathNodeType.Root> ノードの編集はサポートされません。  
  
 <xref:System.Xml.XPath.XPathNavigator> クラスは、ノードの挿入および削除に使用されるメソッドのセットも提供しています。 XML ドキュメントのノードの挿入と削除の詳細については、「[XPathNavigator による XML データの挿入](../../../../docs/standard/data/xml/insert-xml-data-using-xpathnavigator.md)」と「[XPathNavigator による XML データの削除](../../../../docs/standard/data/xml/remove-xml-data-using-xpathnavigator.md)」のトピックを参照してください。  
  
### <a name="modifying-untyped-values"></a>型指定されていない値の変更  
 <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> メソッドは、パラメーターとして渡された型指定されていない `string` 値を挿入するだけです。このパラメーターは、<xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置にあるノードの値です。 値に型は設定されず、スキーマ情報が使用可能な場合でも、ノードの型に対して新しい値が有効どうかを検証せずに挿入されます。  
  
 <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> メソッドを使用して `price` フィァイル内のすべての `contosoBooks.xml` 要素を更新する例を次に示します。  
  
 [!code-cpp[XPathNavigatorMethods#47](../../../../samples/snippets/cpp/VS_Snippets_Data/XPathNavigatorMethods/CPP/xpathnavigatormethods.cpp#47)]
 [!code-csharp[XPathNavigatorMethods#47](../../../../samples/snippets/csharp/VS_Snippets_Data/XPathNavigatorMethods/CS/xpathnavigatormethods.cs#47)]
 [!code-vb[XPathNavigatorMethods#47](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XPathNavigatorMethods/VB/xpathnavigatormethods.vb#47)]  
  
 この例は、`contosoBooks.xml` ファイルを入力として使用します。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
### <a name="modifying-typed-values"></a>型指定された値の変更  
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
  
#### <a name="the-effects-of-editing-strongly-typed-xml-data"></a>厳密に型指定された XML データの効果  
 <xref:System.Xml.XPath.XPathNavigator> クラスは、厳密に型指定された XML の記述の基本に W3C XML スキーマを使用します。 要素および属性には、W3C XML スキーマ ドキュメントに対する検証に基づいて、型情報を使用して注釈を付けることができます。 他の要素または属性を含めることができる要素は、複合型と呼ばれ、テキストの内容だけを含めることのできる要素は単純型と呼ばれます。  
  
> [!NOTE]
> 属性には単純型しかありません。  
  
 要素または属性は、その型定義に固有のすべての規則に準拠している場合、スキーマ有効と見なされます。 単純型の要素 `xs:int` がスキーマ有効であるためには、-2,147,483,648 ～ 2,147,483,647 の数値を含む必要があります。 複合型の場合、要素のスキーマ有効性は、その子の要素および属性のスキーマ有効性に依存します。 したがって、要素が複合型定義に対して有効な場合、その子の要素および属性はすべて、それらの型定義に対して有効です。 同様に、要素の子の要素または属性のうち 1 つでもその型定義に対して無効か有効性が不明な場合、その要素も無効か有効性が不明になります。  
  
 要素の有効性がその子の要素および属性の有効性に依存することを考えると、要素が以前有効だった場合、どちらを変更しても要素の有効性が変わる結果になります。 具体的には、要素の子要素または属性が挿入、更新または削除されると、その要素の有効性は不明になります。 これは、要素の <xref:System.Xml.Schema.IXmlSchemaInfo.Validity%2A> プロパティの <xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> プロパティが <xref:System.Xml.Schema.XmlSchemaValidity.NotKnown> に設定されることによって表されます。 さらに、要素の親要素 (およびその親、そのまた親と続く) の有効性も不明になるため、この効果は、XML ドキュメント全体で再帰的に上方向にカスケードされます。  
  
 スキーマ検証および <xref:System.Xml.XPath.XPathNavigator> クラスの詳細については、「[XPathNavigator を使用したスキーマ検証](../../../../docs/standard/data/xml/schema-validation-using-xpathnavigator.md)」を参照してください。  
  
### <a name="modifying-attributes"></a>属性の変更  
 <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> および <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> メソッドは、型指定されていない属性ノードと型指定された属性ノード、および「ノードの変更」に記載されているその他のノード型の変更に使用できます。  
  
 次の例では、`genre` ファイル内の最初の `book` 要素の `books.xml` 属性の値を変更しています。  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
document.Load("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", String.Empty)  
navigator.MoveToChild("book", String.Empty)  
navigator.MoveToAttribute("genre", String.Empty)  
  
navigator.SetValue("non-fiction")  
  
navigator.MoveToRoot()  
Console.WriteLine(navigator.OuterXml)  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
document.Load("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
navigator.MoveToChild("bookstore", String.Empty);  
navigator.MoveToChild("book", String.Empty);  
navigator.MoveToAttribute("genre", String.Empty);  
  
navigator.SetValue("non-fiction");  
  
navigator.MoveToRoot();  
Console.WriteLine(navigator.OuterXml);  
```  
  
 <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> および <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> メソッドに関する詳細については、「型指定されていない値の変更」および「型指定された値の変更」を参照してください。  
  
## <a name="innerxml-and-outerxml-properties"></a>InnerXml および OuterXml プロパティ  
 <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> クラスの <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> および <xref:System.Xml.XPath.XPathNavigator> プロパティは、<xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置にある XML マークアップを変更します。  
  
 <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> プロパティは、与えられた XML <xref:System.Xml.XPath.XPathNavigator> の解析済みの内容を使用して `string` オブジェクトの現在位置にある子ノードの XML マークアップを変更します。 同様に、<xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> プロパティは、<xref:System.Xml.XPath.XPathNavigator> オブジェクトの現在位置にある子ノードと現在のノード自体の XML マークアップを変更します。  
  
 次の例では、<xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> プロパティを使用して、`price` ファイル内の最初の `discount` 要素に対して `book` 要素の値を変更し、新しい `contosoBooks.xml` 属性を挿入しています。  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
document.Load("contosoBooks.xml");  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books")  
navigator.MoveToChild("book", "http://www.contoso.com/books")  
navigator.MoveToChild("price", "http://www.contoso.com/books")  
  
navigator.OuterXml = "<price discount=\"0\">10.99</price>"  
  
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
  
navigator.OuterXml = "<price discount=\"0\">10.99</price>";  
  
navigator.MoveToRoot();  
Console.WriteLine(navigator.OuterXml);  
```  
  
 この例は、`contosoBooks.xml` ファイルを入力として使用します。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
## <a name="modifying-namespace-nodes"></a>名前空間ノードの変更  
 ドキュメント オブジェクト モデル (DOM) で、名前空間宣言は挿入、更新、および削除が可能な普通の属性のように扱われます。 <xref:System.Xml.XPath.XPathNavigator> クラスでは、名前空間ノードに対してそのような操作は許可されません。これは、次の例で説明されているように、名前空間ノードの値を変更すると、名前空間ノードのスコープ内の要素および属性の ID が変更される可能性があるためです。  
  
```xml  
<root xmlns="http://www.contoso.com">  
    <child />  
</root>  
```  
  
 上記の XML 例を次のように変更すると、各要素の名前空間 URI が変更されるため、事実上、ドキュメント内のすべての要素の名前が変更されます。  
  
```xml  
<root xmlns="urn:contoso.com">  
    <child />  
</root>  
```  
  
 挿入先のスコープでの名前空間宣言と競合しない名前空間の挿入は、<xref:System.Xml.XPath.XPathNavigator> クラスによって許可されます。 この場合、次の例で説明されているように、名前空間宣言は XML ドキュメント内の下位のスコープで宣言されず、名前の変更が発生しません。  
  
```xml  
<root xmlns:a="http://www.contoso.com">  
    <parent>  
        <a:child />  
    </parent>  
</root>  
```  
  
 上記の XML 例を次のように変更すると、名前空間宣言が XML ドキュメント全体で他の名前空間宣言のスコープより下位に正しく反映されます。  
  
```xml  
<root xmlns:a="http://www.contoso.com">  
    <parent a:parent-id="1234" xmlns:a="http://www.contoso.com/parent-id">  
        <a:child xmlns:a="http://www.contoso.com/">  
    </parent>  
</root>  
```  
  
 上記の XML の例では、属性 `a:parent-id` が `parent` 名前空間名の `http://www.contoso.com/parent-id` 要素に挿入されます。 <xref:System.Xml.XPath.XPathNavigator.CreateAttribute%2A> メソッドは、`parent` 要素上に位置しているときの属性の挿入に使用されます。 XML ドキュメントの残りの部分の一貫性を保持するために、`http://www.contoso.com` 名前空間宣言が <xref:System.Xml.XPath.XPathNavigator> クラスによって自動的に挿入されます。  
  
## <a name="modifying-entity-reference-nodes"></a>エンティティ参照ノードの変更  
 <xref:System.Xml.XmlDocument> オブジェクト内のエンティティ参照ノードは、読み取り専用で、<xref:System.Xml.XPath.XPathNavigator> または <xref:System.Xml.XmlNode> クラスのどちらを使用しても編集できません。 エンティティ参照ノードを変更しようとすると、<xref:System.InvalidOperationException> が発生します。  
  
## <a name="modifying-xsinil-nodes"></a>xsi:nil ノードの変更  
 W3C XML スキーマ勧告では、nillable 状態の要素という概念が導入されました。 要素が nillable の場合、要素は内容を持たなくても有効になります。 nillable 状態の要素という概念は、`null` 状態のオブジェクトという概念に似ています。 主な相違点は、`null` オブジェクトにはアクセスできないのに対して、`xsi:nil` 要素には、内容 (子要素またはテキスト) はなくとも、アクセスできる属性などのプロパティがあることです。 XML ドキュメント内では、要素に内容がないことを示すために、要素に `xsi:nil` の値を持つ `true` 属性が使用されます。  
  
 <xref:System.Xml.XPath.XPathNavigator> オブジェクトを使用して、`xsi:nil` の値の `true` 属性を持つ有効な要素に内容を追加すると、その `xsi:nil` 属性の値は `false` に設定されます。  
  
> [!NOTE]
> `xsi:nil` 属性が `false` に設定された要素のコンテンツが削除されても、その属性の値は `true` に変更されません。  
  
## <a name="saving-an-xml-document"></a>XML ドキュメントの保存  
 ここに記載されている編集メソッドによる <xref:System.Xml.XmlDocument> オブジェクトに対する変更の保存は、<xref:System.Xml.XmlDocument> クラスのメソッドを使用して実行されます。 <xref:System.Xml.XmlDocument> オブジェクトに対する変更の保存に関する詳細については、「[ドキュメントの保存と書き込み](../../../../docs/standard/data/xml/saving-and-writing-a-document.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [XPath データ モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-xpath-data-model.md)
- [XPathNavigator による XML データの挿入](../../../../docs/standard/data/xml/insert-xml-data-using-xpathnavigator.md)
- [XPathNavigator による XML データの削除](../../../../docs/standard/data/xml/remove-xml-data-using-xpathnavigator.md)
