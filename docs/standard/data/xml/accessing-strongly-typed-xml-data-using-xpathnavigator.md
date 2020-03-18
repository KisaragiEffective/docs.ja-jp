---
title: 厳密に型指定された XML データへの XPathNavigator を使用したアクセス
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 898e0f52-8a7c-4d1f-afcd-6ffb28b050b4
ms.openlocfilehash: e6ec30e3c7c2318b199122cd63c7f56584707a98
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78158052"
---
# <a name="accessing-strongly-typed-xml-data-using-xpathnavigator"></a>厳密に型指定された XML データへの XPathNavigator を使用したアクセス
XPath 2.0 データ モデルの一例として、<xref:System.Xml.XPath.XPathNavigator> クラスは、共通言語ランタイム (CLR) 型に対応した厳密に型指定されたデータを含むことができます。 XPath 2.0 のデータ モデルに従い、要素と属性のみが厳密に型指定されたデータを含むことができます。 <xref:System.Xml.XPath.XPathNavigator> クラスは、データ型を変換する機構に加えて、厳密に型指定されたデータとして <xref:System.Xml.XPath.XPathDocument> または <xref:System.Xml.XmlDocument> オブジェクト内のデータにアクセスする機構を提供します。  
  
## <a name="type-information-exposed-by-xpathnavigator"></a>XPathNavigator が公開する型情報  
 DTD、XML スキーマ定義言語 (XSD) のスキーマ、または他の機構を使用して処理しない限り、XML 1.0 データに型はありません。 XML 要素や属性と関連付けられる型情報のカテゴリは多数あります。  
  
- 単純 CLR 型 : XML スキーマ言語で共通言語ランタイム (CLR) 型を直接サポートするものはありません。 要素や属性の単純コンテンツを最適な CLR 型として見ることができると便利なので、コンテンツをさらに適切な型に細分化する追加のスキーマ情報がない場合、すべての単純コンテンツは <xref:System.String> として型指定できます。 <xref:System.Xml.XPath.XPathNavigator.ValueType%2A> プロパティを使用することにより、要素と属性の単純コンテンツに最も一致する CLR 型を見つけることができます。 スキーマの組み込み型から CLR 型への対応の詳細については、「[System.Xml クラスでの型のサポート](../../../../docs/standard/data/xml/type-support-in-the-system-xml-classes.md)」を参照してください。  
  
- 単純 (CLR) 型のリスト : 単純コンテンツを持つ要素と属性には、空白で区切られた値のリストを含めることができます。 値は XML スキーマで "リスト型" として指定します。 XML スキーマがない場合、こうした単純型は 1 つのテキスト ノードとして取り扱われます。 XML スキーマが使用可能な場合、この単純コンテンツは、各項が CLR オブジェクトの 1 つのコレクションに対応した単純型を持つ、これ以上分割不可能な一連の値として取り扱うことができます。 スキーマの組み込み型から CLR 型への対応の詳細については、「[System.Xml クラスでの型のサポート](../../../../docs/standard/data/xml/type-support-in-the-system-xml-classes.md)」を参照してください。  
  
- 型指定された値 : スキーマにより検証された単純型の属性と要素は、型指定された値を持ちます。 この値は、数値型、文字列型、または日付型などのプリミティブ型です。 XSD のすべての組み込みの単純型は、単に <xref:System.String> としてでなく、さらに適当な型としてノードの値にアクセスできる CLR 型と対応をとることができます。 属性や子要素を持つ要素は複合型と考えられます。 単純コンテンツ (子としてテキスト ノードのみ) を持つ複合型の型指定された値は、そのコンテンツの単純型のそれと同じです。 複合コンテンツ (1 つ以上の子要素) を持つ複合型の型指定された値は、<xref:System.String> として返されたすべての子テキスト ノードの連結された文字列値です。 スキーマの組み込み型から CLR 型への対応の詳細については、「[System.Xml クラスでの型のサポート](../../../../docs/standard/data/xml/type-support-in-the-system-xml-classes.md)」を参照してください。  
  
- スキーマ言語固有の型名 : 多くの場合、ノードの値へのアクセスに使用されるのは (外部スキーマを適用したため設定された) CLR 型です。 しかし、XML ドキュメントに適用された特定のスキーマに関連付けられた型を調べる必要がある場合もあります。 たとえば、XML ドキュメントを検索して、添付されたスキーマに従って "PurchaseOrder" 型の内容を持つすべての要素を抽出したいが場合があります。 このような型情報はスキーマ検証の結果としてのみ設定され、この情報には <xref:System.Xml.XPath.XPathNavigator.XmlType%2A> クラスの <xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> プロパティと <xref:System.Xml.XPath.XPathNavigator> プロパティを通じてアクセスできます。 詳細については、下の「スキーマ検証後の情報セット (PSVI)」のセクションを参照してください。  
  
- スキーマ言語固有の型のリフレクション : また、XML ドキュメントに適用されたスキーマ固有の型の詳細をさらに取得する必要が生じる場合があります。 たとえば、何か特別な計算をするために、XML ファイルの読み込み中に XML ドキュメント中の有効な各ノードについて `maxOccurs` 属性を抽出する必要がある場合があります。 この情報はスキーマ検証を通じてのみ設定されるので、<xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> クラスの <xref:System.Xml.XPath.XPathNavigator> プロパティを通じてアクセスされます。 詳細については、下の「スキーマ検証後の情報セット (PSVI)」のセクションを参照してください。  
  
## <a name="xpathnavigator-typed-accessors"></a>XPathNavigator の型指定されたアクセサー  
 次の表に、ノードの型情報へのアクセスに使用できる <xref:System.Xml.XPath.XPathNavigator> クラスの各種プロパティとメソッドを示します。  
  
|プロパティ|Description|  
|--------------|-----------------|  
|<xref:System.Xml.XPath.XPathNavigator.XmlType%2A>|有効な場合、ノードの XML スキーマ型の情報を含みます。|  
|<xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A>|検証後に追加されたノードのスキーマ検証後の情報セットを含みます。 これには、検証情報に加えて、XML スキーマ型情報も含まれます。|  
|<xref:System.Xml.XPath.XPathNavigator.ValueType%2A>|ノードの型指定された値の CLR 型。|  
|<xref:System.Xml.XPath.XPathNavigator.TypedValue%2A>|型がノードの XML スキーマ型に最も一致する 1 つ以上の CLR 値としてのノードの内容。|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAsBoolean%2A>|XPath 2.0 の <xref:System.String> のキャスト規則に従って、<xref:System.Boolean> 値にキャストされた現在のノードの `xs:boolean` 値。|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAsDateTime%2A>|XPath 2.0 の <xref:System.String> のキャスト規則に従って、<xref:System.DateTime> 値にキャストされた現在のノードの `xs:datetime` 値。|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAsDouble%2A>|XPath 2.0 の <xref:System.String> のキャスト規則に従って、<xref:System.Double> 値にキャストされた現在のノードの `xsd:double` 値。|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAsInt%2A>|XPath 2.0 の <xref:System.String> のキャスト規則に従って、<xref:System.Int32> 値にキャストされた現在のノードの `xs:integer` 値。|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAsLong%2A>|XPath 2.0 の <xref:System.String> のキャスト規則に従って、<xref:System.Int64> 値にキャストされた現在のノードの `xs:integer` 値。|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAs%2A>|XPath 2.0 のキャスト規則に従って、変換先の型にキャストされたノードのコンテンツ。|  
  
 スキーマの組み込み型から CLR 型への対応の詳細については、「[System.Xml クラスでの型のサポート](../../../../docs/standard/data/xml/type-support-in-the-system-xml-classes.md)」を参照してください。  
  
## <a name="the-post-schema-validation-infoset-psvi"></a>スキーマ検証後の情報セット (PSVI)  
 XML スキーマ プロセッサは、XML 情報セットを入力として受け入れ、それをスキーマ検証後の情報セット (PSVI) に変換します。 PSVI は、元の入力 XML 情報セットに新しい情報項目を追加し、新しいプロパティを追加したものです。 <xref:System.Xml.XPath.XPathNavigator> によって公開される PSVI 中の XML 情報セットに追加される情報には 3 つの広範なクラスがあります。  
  
1. 検証結果 : 要素または属性が問題なく検証されたかどうかの情報。 これは、<xref:System.Xml.Schema.IXmlSchemaInfo.Validity%2A> クラスの <xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> プロパティの <xref:System.Xml.XPath.XPathNavigator> プロパティによって公開されます。  
  
2. 既定の情報 : 要素または属性の値が、スキーマに定義された既定値により得られたものかどうかを示します。 これは、<xref:System.Xml.Schema.IXmlSchemaInfo.IsDefault%2A> クラスの <xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> プロパティの <xref:System.Xml.XPath.XPathNavigator> プロパティによって公開されます。  
  
3. 型のコメント : スキーマ コンポーネントへの参照。これは型の定義や、要素と属性の宣言である場合があります。 ノードが有効な場合、<xref:System.Xml.XPath.XPathNavigator.XmlType%2A> の <xref:System.Xml.XPath.XPathNavigator> プロパティは、ノード固有の型情報を含みます。 検証された後に編集されているときなど、ノードの有効性が不明な場合は、 <xref:System.Xml.XPath.XPathNavigator.XmlType%2A> プロパティが `null` に設定されますが、この場合でも、<xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> クラスの <xref:System.Xml.XPath.XPathNavigator> プロパティの各種プロパティから型情報を入手できます。  
  
 次の例は、<xref:System.Xml.XPath.XPathNavigator> によって公開されるスキーマ検証後の情報セット内の情報の使用方法を示します。  
  
```vb  
Dim settings As XmlReaderSettings = New XmlReaderSettings()  
settings.Schemas.Add("http://www.contoso.com/books", "books.xsd")  
settings.ValidationType = ValidationType.Schema  
  
Dim reader As XmlReader = XmlReader.Create("books.xml", settings)  
  
Dim document As XmlDocument = New XmlDocument()  
document.Load(reader)  
Dim navigator As XPathNavigator = document.CreateNavigator()  
navigator.MoveToChild("books", "http://www.contoso.com/books")  
navigator.MoveToChild("book", "http://www.contoso.com/books")  
navigator.MoveToChild("published", "http://www.contoso.com/books")  
  
Console.WriteLine(navigator.SchemaInfo.SchemaType.Name)  
Console.WriteLine(navigator.SchemaInfo.Validity)  
Console.WriteLine(navigator.SchemaInfo.SchemaElement.MinOccurs)  
```  
  
```csharp  
XmlReaderSettings settings = new XmlReaderSettings();  
settings.Schemas.Add("http://www.contoso.com/books", "books.xsd");  
settings.ValidationType = ValidationType.Schema;  
  
XmlReader reader = XmlReader.Create("books.xml", settings);  
  
XmlDocument document = new XmlDocument();  
document.Load(reader);  
XPathNavigator navigator = document.CreateNavigator();  
navigator.MoveToChild("books", "http://www.contoso.com/books");  
navigator.MoveToChild("book", "http://www.contoso.com/books");  
navigator.MoveToChild("published", "http://www.contoso.com/books");  
  
Console.WriteLine(navigator.SchemaInfo.SchemaType.Name);  
Console.WriteLine(navigator.SchemaInfo.Validity);  
Console.WriteLine(navigator.SchemaInfo.SchemaElement.MinOccurs);  
```  
  
 この例は、`books.xml` ファイルを入力として使用します。  
  
```xml  
<books xmlns="http://www.contoso.com/books">  
    <book>  
        <title>Title</title>  
        <price>10.00</price>  
        <published>2003-12-31</published>  
    </book>  
</books>  
```  
  
 また、`books.xsd` スキーマも入力として使用します。  
  
```xml  
<xs:schema xmlns="http://www.contoso.com/books"
attributeFormDefault="unqualified" elementFormDefault="qualified"
targetNamespace="http://www.contoso.com/books"
xmlns:xs="http://www.w3.org/2001/XMLSchema">  
    <xs:simpleType name="publishedType">  
        <xs:restriction base="xs:date">  
            <xs:minInclusive value="2003-01-01" />  
            <xs:maxInclusive value="2003-12-31" />  
        </xs:restriction>  
    </xs:simpleType>  
    <xs:complexType name="bookType">  
        <xs:sequence>  
            <xs:element name="title" type="xs:string"/>  
            <xs:element name="price" type="xs:decimal"/>  
            <xs:element name="published" type="publishedType"/>  
        </xs:sequence>  
    </xs:complexType>  
    <xs:complexType name="booksType">  
        <xs:sequence>  
            <xs:element name="book" type="bookType" />  
        </xs:sequence>  
    </xs:complexType>  
    <xs:element name="books" type="booksType" />  
</xs:schema>  
```  
  
## <a name="obtain-typed-values-using-valueas-properties"></a>ValueAs プロパティによる型指定された値の取得  
 ノードの型指定された値は、<xref:System.Xml.XPath.XPathNavigator.TypedValue%2A> の <xref:System.Xml.XPath.XPathNavigator> プロパティにアクセスして取得することができます。 場合により、ノードの型指定された値を別の型に変換する必要がある場合があります。 一般的な例として、XML ノードから数値を取得する場合があります。 たとえば、次のような未検証で型指定されていない XML ドキュメントがあるとします。  
  
```xml  
<books>  
    <book>  
        <title>Title</title>  
        <price>10.00</price>  
        <published>2003-12-31</published>  
    </book>  
</books>  
```  
  
 <xref:System.Xml.XPath.XPathNavigator> が `price` 要素上に位置している場合、<xref:System.Xml.XPath.XPathNavigator.XmlType%2A> プロパティは `null` となり、<xref:System.Xml.XPath.XPathNavigator.ValueType%2A> プロパティは <xref:System.String> であり、<xref:System.Xml.XPath.XPathNavigator.TypedValue%2A> プロパティは文字列 `10.00` です。  
  
 しかし、<xref:System.Xml.XPath.XPathItem.ValueAs%2A>、<xref:System.Xml.XPath.XPathNavigator.ValueAsDouble%2A>、<xref:System.Xml.XPath.XPathNavigator.ValueAsInt%2A>、または <xref:System.Xml.XPath.XPathNavigator.ValueAsLong%2A> のメソッドとプロパティを使用して、値を数値として抽出することは可能です。 次の例では、<xref:System.Xml.XPath.XPathItem.ValueAs%2A> メソッドを使用してキャストを実行します。  
  
```vb  
Dim document As New XmlDocument()  
document.Load("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
navigator.MoveToChild("books", "")  
navigator.MoveToChild("book", "")  
navigator.MoveToChild("price", "")  
  
Dim price = navigator.ValueAs(GetType(Decimal))  
Dim discount As Decimal = 0.2  
  
Console.WriteLine("The price of the book has been dropped 20% from {0:C} to {1:C}", navigator.Value, (price - price * discount))  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
document.Load("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
navigator.MoveToChild("books", "");  
navigator.MoveToChild("book", "");  
navigator.MoveToChild("price", "");  
  
Decimal price = (decimal)navigator.ValueAs(typeof(decimal));  
  
Console.WriteLine("The price of the book has been dropped 20% from {0:C} to {1:C}", navigator.Value, (price - price * (decimal)0.20));  
```  
  
 スキーマの組み込み型から CLR 型への対応の詳細については、「[System.Xml クラスでの型のサポート](../../../../docs/standard/data/xml/type-support-in-the-system-xml-classes.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [System.Xml クラスでの型のサポート](../../../../docs/standard/data/xml/type-support-in-the-system-xml-classes.md)
- [XPath データ モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-xpath-data-model.md)
- [XPathNavigator を使用するノード セットのナビゲーション](../../../../docs/standard/data/xml/node-set-navigation-using-xpathnavigator.md)
- [XPathNavigator を使用する属性と名前空間のナビゲーション](../../../../docs/standard/data/xml/attribute-and-namespace-node-navigation-using-xpathnavigator.md)
- [XpathNavigator を使用した XML データの抽出](../../../../docs/standard/data/xml/extract-xml-data-using-xpathnavigator.md)
