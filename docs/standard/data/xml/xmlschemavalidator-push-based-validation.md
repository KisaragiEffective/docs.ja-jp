---
title: XmlSchemaValidator のプッシュ ベースの検証
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 911d4460-dd91-4958-85b2-2ca3299f9ec6
ms.openlocfilehash: 6a0cc110c2b8bcd97b9f5c16a344db5a63046353
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75709804"
---
# <a name="xmlschemavalidator-push-based-validation"></a>XmlSchemaValidator のプッシュ ベースの検証

<xref:System.Xml.Schema.XmlSchemaValidator> は、プッシュ ベース方式で、XML スキーマを基準として XML データを検証する、効率的かつ高性能なしくみを提供します。 たとえば、<xref:System.Xml.Schema.XmlSchemaValidator> クラスでは、XML 情報セットを XML ドキュメントにシリアル化してから検証型 XML リーダーを使用してドキュメントを再解析する必要なしに、情報セットをそのまま検証することができます。

<xref:System.Xml.Schema.XmlSchemaValidator> クラスは、カスタム XML データ ソースに対する検証エンジンの構築や検証型 XML ライターを構築する 1 つの手段とするなど、上級のシナリオで使用することができます。

次は、<xref:System.Xml.Schema.XmlSchemaValidator> クラスを使用して、`contosoBooks.xml` ファイルを `contosoBooks.xsd` スキーマに対して検証する一例です。 この例では、<xref:System.Xml.Serialization.XmlSerializer> クラスを使用して、`contosoBooks.xml` ファイルを逆シリアル化し、ノードの値を <xref:System.Xml.Schema.XmlSchemaValidator> クラスのメソッドに渡します。

> [!NOTE]
> この例は、このトピックの各セクションを通じて使用されます。

[!code-csharp[XmlSchemaValidatorExamples#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaValidatorExamples/CS/XmlSchemaValidatorExamples.cs#1)]
[!code-vb[XmlSchemaValidatorExamples#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaValidatorExamples/VB/XmlSchemaValidatorExamples.vb#1)]

この例は、`contosoBooks.xml` ファイルを入力として使用します。

[!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]

また、`contosoBooks.xsd` ファイルも入力として使用します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" targetNamespace="http://www.contoso.com/books" xmlns:xs="http://www.w3.org/2001/XMLSchema">
    <xs:element name="bookstore">
        <xs:complexType>
            <xs:sequence>
                <xs:element maxOccurs="unbounded" name="book">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="title" type="xs:string" />
                            <xs:element name="author">
                                <xs:complexType>
                                    <xs:sequence>
                                        <xs:element minOccurs="0" name="name" type="xs:string" />
                                        <xs:element minOccurs="0" name="first-name" type="xs:string" />
                                        <xs:element minOccurs="0" name="last-name" type="xs:string" />
                                    </xs:sequence>
                                </xs:complexType>
                            </xs:element>
                            <xs:element name="price" type="xs:decimal" />
                        </xs:sequence>
                        <xs:attribute name="genre" type="xs:string" use="required" />
                        <xs:attribute name="publicationdate" type="xs:date" use="required" />
                        <xs:attribute name="ISBN" type="xs:string" use="required" />
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

## <a name="validating-xml-data-using-xmlschemavalidator"></a>XmlSchemaValidator を使用した XML データの検証

XML 情報セットの検証を開始するには、最初に <xref:System.Xml.Schema.XmlSchemaValidator> コンストラクターを使用して <xref:System.Xml.Schema.XmlSchemaValidator.%23ctor%2A> クラスの新しいインスタンスを初期化する必要があります。

<xref:System.Xml.Schema.XmlSchemaValidator.%23ctor%2A> コンストラクターはパラメーターとして <xref:System.Xml.XmlNameTable> 値の他に、<xref:System.Xml.Schema.XmlSchemaSet>、<xref:System.Xml.XmlNamespaceManager>、および <xref:System.Xml.Schema.XmlSchemaValidationFlags> オブジェクトをパラメーターとして取ります。 <xref:System.Xml.XmlNameTable> オブジェクトは、スキーマの名前空間や XML 名前空間などの既知の名前空間文字列を分解するために使用され、単純コンテンツの検証中に、<xref:System.Xml.Schema.XmlSchemaDatatype.ParseValue%2A> メソッドに渡されます。 <xref:System.Xml.Schema.XmlSchemaSet> オブジェクトには XML 情報セットを検証するために使用される XML スキーマが含まれています。 <xref:System.Xml.XmlNamespaceManager> オブジェクトは検証中に遭遇する名前空間を解決するために使用されます。 <xref:System.Xml.Schema.XmlSchemaValidationFlags> 値は、検証の特定の機能を無効にするために使用されます。

<xref:System.Xml.Schema.XmlSchemaValidator.%23ctor%2A> コンストラクターの詳細については、<xref:System.Xml.Schema.XmlSchemaValidator> クラスのリファレンス ドキュメントを参照してください。

### <a name="initializing-validation"></a>検証の初期化

<xref:System.Xml.Schema.XmlSchemaValidator> オブジェクトの構築後に、<xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> オブジェクトの状態の初期化に使用される 2 つのオーバーロードされた <xref:System.Xml.Schema.XmlSchemaValidator> メソッドがあります。 次が、その 2 つの <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> メソッドです。

- <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A?displayProperty=nameWithType>

- <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A?displayProperty=nameWithType>

既定の <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A?displayProperty=nameWithType> メソッドは <xref:System.Xml.Schema.XmlSchemaValidator> オブジェクトを開始状態に初期化し、オーバーロードされた <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A?displayProperty=nameWithType> メソッドは <xref:System.Xml.Schema.XmlSchemaObject> をパラメーターとして取り、部分検証のために <xref:System.Xml.Schema.XmlSchemaValidator> オブジェクトを開始状態に初期化します。

両方の <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> メソッドは、<xref:System.Xml.Schema.XmlSchemaValidator> オブジェクトが構築された直後、または <xref:System.Xml.Schema.XmlSchemaValidator.EndValidation%2A> の呼び出し後のみ呼び出し可能です。

<xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A?displayProperty=nameWithType> メソッドの例については、最初の例を参照してください。 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> メソッドの詳細については、<xref:System.Xml.Schema.XmlSchemaValidator> クラスのリファレンス ドキュメントを参照してください。

#### <a name="partial-validation"></a>部分検証

<xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A?displayProperty=nameWithType> をパラメーターとして取る <xref:System.Xml.Schema.XmlSchemaObject> メソッドは、部分検証のために <xref:System.Xml.Schema.XmlSchemaValidator> オブジェクトを開始状態に初期化します。

次の例で <xref:System.Xml.Schema.XmlSchemaObject> は、部分検証のために <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A?displayProperty=nameWithType> メソッドを使用して初期化されます。 `orderNumber` オブジェクトの <xref:System.Xml.XmlQualifiedName> プロパティによって返された <xref:System.Xml.Schema.XmlSchemaObjectTable> コレクション内の <xref:System.Xml.Schema.XmlSchemaSet.GlobalElements%2A> によってスキーマ要素を選択することにより、<xref:System.Xml.Schema.XmlSchemaSet> スキーマ要素が渡されます。 <xref:System.Xml.Schema.XmlSchemaValidator> オブジェクトは、次にこの特定の要素を検証します。

```vb
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet()
schemaSet.Add(Nothing, "schema.xsd")
schemaSet.Compile()
Dim nameTable As NameTable = New NameTable()
Dim manager As XmlNamespaceManager = New XmlNamespaceManager(nameTable)

Dim validator As XmlSchemaValidator = New XmlSchemaValidator(nameTable, schemaSet, manager, XmlSchemaValidationFlags.None)
validator.Initialize(schemaSet.GlobalElements.Item(New XmlQualifiedName("orderNumber")))

validator.ValidateElement("orderNumber", "", Nothing)
validator.ValidateEndOfAttributes(Nothing)
validator.ValidateText("123")
validator.ValidateEndElement(Nothing)
```

```csharp
XmlSchemaSet schemaSet = new XmlSchemaSet();
schemaSet.Add(null, "schema.xsd");
schemaSet.Compile();
NameTable nameTable = new NameTable();
XmlNamespaceManager manager = new XmlNamespaceManager(nameTable);

XmlSchemaValidator validator = new XmlSchemaValidator(nameTable, schemaSet, manager, XmlSchemaValidationFlags.None);
validator.Initialize(schemaSet.GlobalElements[new XmlQualifiedName("orderNumber")]);

validator.ValidateElement("orderNumber", "", null);
validator.ValidateEndOfAttributes(null);
validator.ValidateText("123");
validator.ValidateEndElement(null);
```

この例は、次の XML スキーマを入力として使用します。

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="orderNumber" type="xs:int" />
</xs:schema>
```

<xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> メソッドの詳細については、<xref:System.Xml.Schema.XmlSchemaValidator> クラスのリファレンス ドキュメントを参照してください。

### <a name="adding-additional-schemas"></a>追加のスキーマの追加

<xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A> クラスの <xref:System.Xml.Schema.XmlSchemaValidator> メソッドは、検証中に使用するスキーマ セットに 1 つの XML スキーマを追加するために使用します。 <xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A> メソッドは、XML 情報セットの検証中に、インライン XML スキーマに遭遇した場合の効果をシミュレートするために使用できます。

> [!NOTE]
> <xref:System.Xml.Schema.XmlSchema> パラメーターの対象名前空間は、<xref:System.Xml.Schema.XmlSchemaValidator> オブジェクトが既に遭遇したすべての要素と属性の名前空間と一致してはなりません。
>
> <xref:System.Xml.Schema.XmlSchemaValidationFlags.ProcessInlineSchema?displayProperty=nameWithType> コンストラクターに <xref:System.Xml.Schema.XmlSchemaValidator.%23ctor%2A> 値がパラメーターとして渡されなかった場合、<xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A> メソッドは何も行いません。

<xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A> メソッドの結果は、検証中の現在の XML ノードのコンテキストに依存します。 検証コンテキストの詳細については、このトピックの「検証コンテキスト」を参照してください。

<xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A> メソッドの詳細については、<xref:System.Xml.Schema.XmlSchemaValidator> クラスのリファレンス ドキュメントを参照してください。

### <a name="validating-elements-attributes-and-content"></a>要素、属性、およびコンテンツの検証

<xref:System.Xml.Schema.XmlSchemaValidator> クラスは、XML 情報セット中の要素、属性、およびコンテンツを XML スキーマに対して検証するために使用するいくつかのメソッドを提供します。 各メソッドに関する説明を次の表に示します。

|メソッド|説明|
|------------|-----------------|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A>|現在のコンテキスト中の要素名を検証します。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>|現在の要素コンテキスト中の属性を検証します。または <xref:System.Xml.Schema.XmlSchemaAttribute> メソッドにパラメーターとして渡された <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> オブジェクトに対して属性を検証します。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndOfAttributes%2A>|要素コンテキスト中の必須属性がすべて揃っていることを検証し、要素の子コンテンツを検証するために <xref:System.Xml.Schema.XmlSchemaValidator> オブジェクトを準備します。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A>|現在の要素コンテキスト中でテキストが許されるかどうかを検証し、現在の要素に単純コンテンツがある場合は検証のためにテキストを収集します。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateWhitespace%2A>|現在の要素コンテキスト中で空白が許されるかどうかを検証し、現在の要素に単純コンテンツがある場合は検証のために空白を収集します。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A>|要素のテキスト コンテンツが、単純コンテンツの要素のデータ型に従って有効なことを検証し、現在の要素のコンテンツが複合コンテンツの要素として完全であることを検証します。|
|<xref:System.Xml.Schema.XmlSchemaValidator.SkipToEndElement%2A>|現在の要素コンテンツの検証をスキップし、親要素のコンテキストでコンテンツの検証するために <xref:System.Xml.Schema.XmlSchemaValidator> オブジェクトを準備します。|
|<xref:System.Xml.Schema.XmlSchemaValidator.EndValidation%2A>|検証を終了し、<xref:System.Xml.Schema.XmlSchemaValidationFlags.ProcessIdentityConstraints> 検証オプションが設定されている場合は XML ドキュメント全体について ID 制約をチェックします。|

> [!NOTE]
> <xref:System.Xml.Schema.XmlSchemaValidator> クラスには、上の表で定義された各メソッド呼び出しの発生と順序を強制する、定義済みの状態遷移があります。 <xref:System.Xml.Schema.XmlSchemaValidator> クラスの状態遷移の詳細は、このトピックの「XmlSchemaValidator の状態遷移」セクションで説明されています。

XML 情報セット内の要素、属性、およびコンテンツの検証に使用されるメソッドの例については、前のセクションの例を参照してください。 これらのメソッドの詳細については、<xref:System.Xml.Schema.XmlSchemaValidator> クラスのリファレンス ドキュメントを参照してください。

#### <a name="validating-content-using-an-xmlvaluegetter"></a>XmlValueGetter を使用したコンテンツの検証

<xref:System.Xml.Schema.XmlValueGetter>`delegate` は、属性、テキスト、または空白のノードの値を、属性、テキスト、または空白ノードの XML スキーマ定義言語 (XSD) 型と互換性のある共通言語ランタイム (CLR) 型として渡すために使用できます。 <xref:System.Xml.Schema.XmlValueGetter>`delegate` は、属性、テキスト、または空白のノードの CLR 値が既に使用可能な場合、それを `string` に変換してから検証のために再度解析する手間を省くために役立ちます。

<xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>、<xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A>、および <xref:System.Xml.Schema.XmlSchemaValidator.ValidateWhitespace%2A> メソッドはオーバーロードされ、属性、テキスト、または空白ノードの値を `string` または <xref:System.Xml.Schema.XmlValueGetter>`delegate` として受け入れます。

<xref:System.Xml.Schema.XmlSchemaValidator> クラスの次のメソッドは、<xref:System.Xml.Schema.XmlValueGetter>`delegate` をパラメーターとして受け入れます。

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateWhitespace%2A>

次は、最初の <xref:System.Xml.Schema.XmlValueGetter> クラスの例から取られた `delegate`<xref:System.Xml.Schema.XmlSchemaValidator> の例です。 <xref:System.Xml.Schema.XmlValueGetter>`delegate` は属性の値を <xref:System.DateTime> オブジェクトとして返します。 <xref:System.DateTime> から返された <xref:System.Xml.Schema.XmlValueGetter> オブジェクトを検証するために、<xref:System.Xml.Schema.XmlSchemaValidator> オブジェクトは、最初にそれを属性のデータ型の ValueType (ValueType は XSD 型に対する既定の CLR の割り当て) に変換し、次に変換された値のファセットをチェックします。

```vb
Shared dateTimeGetterContent As Object

Shared Function DateTimeGetterHandle() As Object
    Return dateTimeGetterContent
End Function

Shared Function DateTimeGetter(dateTime As DateTime) As XmlValueGetter
    dateTimeGetterContent = dateTime
    Return New XmlValueGetter(AddressOf DateTimeGetterHandle)
End Function
```

```csharp
static object dateTimeGetterContent;

static object DateTimeGetterHandle()
{
    return dateTimeGetterContent;
}

static XmlValueGetter DateTimeGetter(DateTime dateTime)
{
    dateTimeGetterContent = dateTime;
    return new XmlValueGetter(dateTimeGetterHandle);
}
```

<xref:System.Xml.Schema.XmlValueGetter>`delegate` の例については、最初の例を参照してください。 <xref:System.Xml.Schema.XmlValueGetter>`delegate` の詳細については、<xref:System.Xml.Schema.XmlValueGetter> および <xref:System.Xml.Schema.XmlSchemaValidator> クラスのリファレンス ドキュメントを参照してください。

#### <a name="post-schema-validation-information"></a>スキーマの検証後の情報

<xref:System.Xml.Schema.XmlSchemaInfo> クラスは <xref:System.Xml.Schema.XmlSchemaValidator> クラスによって検証された 1 つの XML ノードのスキーマ検証後情報のいくつかを表します。 <xref:System.Xml.Schema.XmlSchemaValidator> クラスの種々のメソッドは <xref:System.Xml.Schema.XmlSchemaInfo> オブジェクトをオプションの (`null`) `out` パラメーターとして受け入れます。

検証が成功した後、<xref:System.Xml.Schema.XmlSchemaInfo> オブジェクトには検証の結果が設定されています。 たとえば、<xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A> メソッドを使用した検証の成功時には、<xref:System.Xml.Schema.XmlSchemaInfo> オブジェクトの (指定した場合) <xref:System.Xml.Schema.XmlSchemaInfo.SchemaAttribute%2A>、<xref:System.Xml.Schema.XmlSchemaInfo.SchemaType%2A>、<xref:System.Xml.Schema.XmlSchemaInfo.MemberType%2A>、および <xref:System.Xml.Schema.XmlSchemaInfo.Validity%2A> プロパティに検証結果が設定されています。

次の <xref:System.Xml.Schema.XmlSchemaValidator> クラスのメソッドは、<xref:System.Xml.Schema.XmlSchemaInfo> オブジェクトを Out パラメーターとして受け入れます。

- <xref:System.Xml.Schema.XmlSchemaValidator.SkipToEndElement%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndOfAttributes%2A>

<xref:System.Xml.Schema.XmlSchemaInfo> クラスの完全な例については、最初の例を参照してください。 <xref:System.Xml.Schema.XmlSchemaInfo> クラスに関する詳細については、<xref:System.Xml.Schema.XmlSchemaInfo> クラスのリファレンス ドキュメントを参照してください。

### <a name="retrieving-expected-particles-attributes-and-unspecified-default-attributes"></a>期待されるパーティクル、属性、および未指定の既定の属性の取得

<xref:System.Xml.Schema.XmlSchemaValidator> クラスは、現在の検証コンテキスト中で期待されるパーティクル、属性、および未指定の既定の属性を取得するための <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A>、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A>、および <xref:System.Xml.Schema.XmlSchemaValidator.GetUnspecifiedDefaultAttributes%2A> メソッドを提供します。

#### <a name="retrieving-expected-particles"></a>期待されるパーティクルの取得

<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> メソッドは、現在の要素コンテキストで期待されるパーティクルを含む <xref:System.Xml.Schema.XmlSchemaParticle> オブジェクトの配列を返します。 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> メソッドが返せる有効なパーティクルは、<xref:System.Xml.Schema.XmlSchemaElement> クラスと <xref:System.Xml.Schema.XmlSchemaAny> クラスのインスタンスです。

コンテンツ モデルのコンポジターが `xs:sequence` の場合、シーケンス中の次のパーティクルのみが返されます。 コンテンツ モデルのコンポジターが `xs:all` または `xs:choice` の場合、現在の要素コンテキストに続くことができる有効なパーティクルすべてが返されます。

> [!NOTE]
> <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> メソッドが <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> メソッドの呼び出し直後に呼ばれると、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> メソッドはすべてのグローバル要素を返します。

たとえば、XSD (XML スキーマ定義言語) スキーマと続く XML ドキュメントで、`book` 要素の検証後、`book` 要素は現在の要素コンテキストになります。 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> メソッドは、<xref:System.Xml.Schema.XmlSchemaElement> 要素を表す単一の `title` オブジェクトを含む配列を返します。 検証コンテキストが `title` 要素の場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> メソッドは空の配列を返します。 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> メソッドが `title` 要素の検証後で `description` 要素の検証前に呼び出されると、このメソッドは <xref:System.Xml.Schema.XmlSchemaElement> 要素を表す単一の `description` オブジェクトを含む配列を返します。 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> メソッドが `description` 要素の検証後に呼び出されると、メソッドはワイルドカードを表す単一の <xref:System.Xml.Schema.XmlSchemaAny> オブジェクトを含む配列を返します。

```vb
Dim reader As XmlReader =  XmlReader.Create("input.xml")

Dim schemaSet As New XmlSchemaSet()
schemaSet.Add(Nothing, "schema.xsd")
Dim manager As New XmlNamespaceManager(reader.NameTable)

Dim validator As New XmlSchemaValidator(reader.NameTable,schemaSet,manager,XmlSchemaValidationFlags.None)
validator.Initialize()

validator.ValidateElement("book", "", Nothing)

validator.ValidateEndOfAttributes(Nothing)
For Each element As XmlSchemaElement In validator.GetExpectedParticles()
    Console.WriteLine(element.Name)
Next

validator.ValidateElement("title", "", Nothing)
validator.ValidateEndOfAttributes(Nothing)
For Each element As XmlSchemaElement In validator.GetExpectedParticles()
    Console.WriteLine(element.Name)
Next
validator.ValidateEndElement(Nothing)

For Each element As XmlSchemaElement In validator.GetExpectedParticles()
    Console.WriteLine(element.Name)
Next

validator.ValidateElement("description", "", Nothing)
validator.ValidateEndOfAttributes(Nothing)
validator.ValidateEndElement(Nothing)

For Each particle As XmlSchemaParticle In validator.GetExpectedParticles()
    Console.WriteLine(particle.GetType())
Next

validator.ValidateElement("namespace", "", Nothing)
validator.ValidateEndOfAttributes(Nothing)
validator.ValidateEndElement(Nothing)

validator.ValidateEndElement(Nothing)
```

```csharp
XmlReader reader = XmlReader.Create("input.xml");

var schemaSet = new XmlSchemaSet();
schemaSet.Add(null, "schema.xsd");
var manager = new XmlNamespaceManager(reader.NameTable);

var validator = new XmlSchemaValidator(reader.NameTable, schemaSet, manager, XmlSchemaValidationFlags.None);
validator.Initialize();

validator.ValidateElement("book", "", null);

validator.ValidateEndOfAttributes(null);
foreach (XmlSchemaElement element in validator.GetExpectedParticles())
{
    Console.WriteLine(element.Name);
}

validator.ValidateElement("title", "", null);
validator.ValidateEndOfAttributes(null);
foreach (XmlSchemaElement element in validator.GetExpectedParticles())
{
    Console.WriteLine(element.Name);
}
validator.ValidateEndElement(null);

foreach (XmlSchemaElement element in validator.GetExpectedParticles())
{
    Console.WriteLine(element.Name);
}

validator.ValidateElement("description", "", null);
validator.ValidateEndOfAttributes(null);
validator.ValidateEndElement(null);

foreach (XmlSchemaParticle particle in validator.GetExpectedParticles())
{
    Console.WriteLine(particle.GetType());
}

validator.ValidateElement("namespace", "", null);
validator.ValidateEndOfAttributes(null);
validator.ValidateEndElement(null);

validator.ValidateEndElement(null);
```

 この例では、次の XML を入力として使用します。

```xml
<xs:schema xmlns:xs="http://www.w3c.org/2001/XMLSchema">
  <xs:element name="book">
    <xs:sequence>
      <xs:element name="title" type="xs:string" />
      <xs:element name="description" type="xs:string" />
      <xs:any processContent="lax" maxOccurs="unbounded" />
    </xs:sequence>
  </xs:element>
</xs:schema>
```

この例では、入力として次の XSD スキーマを使用します。

```xml
<book>
  <title>My Book</title>
  <description>My Book's Description</description>
  <namespace>System.Xml.Schema</namespace>
</book>
```

> [!NOTE]
> <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> クラスの <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A>、<xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A>、および <xref:System.Xml.Schema.XmlSchemaValidator> メソッドの結果は、検証中の現在のコンテキストに依存します。 詳細については、このトピックの「検証コンテキスト」を参照してください。

<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> メソッドの例については、最初の例を参照してください。 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> メソッドの詳細については、<xref:System.Xml.Schema.XmlSchemaValidator> クラスのリファレンス ドキュメントを参照してください。

#### <a name="retrieving-expected-attributes"></a>期待される属性の取得

<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> メソッドは、現在の要素コンテキストで期待される属性を含む <xref:System.Xml.Schema.XmlSchemaAttribute> オブジェクトの配列を返します。

たとえば最初の例で、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> メソッドは `book` 要素のすべての属性を取得するために使用されます。

<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> メソッドを <xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A> メソッドの直後に呼び出すと、その XML ドキュメントで使用可能なすべての属性が返されます。 ただし、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> メソッドを <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A> メソッドの 1 回以上の呼び出し後に呼び出すと、現在の要素でまだ検証されていない属性が返されます。

> [!NOTE]
> <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> クラスの <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A>、<xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A>、および <xref:System.Xml.Schema.XmlSchemaValidator> メソッドの結果は、検証中の現在のコンテキストに依存します。 詳細については、このトピックの「検証コンテキスト」を参照してください。

<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> メソッドの例については、最初の例を参照してください。 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> メソッドの詳細については、<xref:System.Xml.Schema.XmlSchemaValidator> クラスのリファレンス ドキュメントを参照してください。

#### <a name="retrieving-unspecified-default-attributes"></a>未指定の既定の属性の取得

<xref:System.Xml.Schema.XmlSchemaValidator.GetUnspecifiedDefaultAttributes%2A> メソッドは、要素コンテキスト中でこれまでに <xref:System.Collections.ArrayList> メソッドを使用して検証されていない既定値を持つ属性の <xref:System.Xml.Schema.XmlSchemaAttribute> オブジェクトを指定された <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A> に読み込みます。 <xref:System.Xml.Schema.XmlSchemaValidator.GetUnspecifiedDefaultAttributes%2A> メソッドは、要素コンテキスト中の各属性について <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A> メソッドを呼び出した後で呼び出します。 <xref:System.Xml.Schema.XmlSchemaValidator.GetUnspecifiedDefaultAttributes%2A> メソッドは、検証中の XML ドキュメントに挿入される既定の属性を決定するために使用します。

<xref:System.Xml.Schema.XmlSchemaValidator.GetUnspecifiedDefaultAttributes%2A> メソッドの詳細については、<xref:System.Xml.Schema.XmlSchemaValidator> クラスのリファレンス ドキュメントを参照してください。

### <a name="handling-schema-validation-events"></a>スキーマ検証イベントの処理

検証中に発生したスキーマ検証の警告とエラーは、<xref:System.Xml.Schema.XmlSchemaValidator.ValidationEventHandler> クラスの <xref:System.Xml.Schema.XmlSchemaValidator> イベントで取り扱われます。

スキーマ検証の警告は、<xref:System.Xml.Schema.XmlSeverityType> 値が <xref:System.Xml.Schema.XmlSeverityType.Warning> であり、スキーマ検証エラーは、<xref:System.Xml.Schema.XmlSeverityType> 値が <xref:System.Xml.Schema.XmlSeverityType.Error> です。 <xref:System.Xml.Schema.XmlSchemaValidator.ValidationEventHandler> が何も割り当てられていない場合、すべてのスキーマ検証エラーについて <xref:System.Xml.Schema.XmlSchemaValidationException> が <xref:System.Xml.Schema.XmlSeverityType> 値 <xref:System.Xml.Schema.XmlSeverityType.Error> でスローされます。 ただし、<xref:System.Xml.Schema.XmlSchemaValidationException> 値が <xref:System.Xml.Schema.XmlSeverityType> であるスキーマ検証警告については、<xref:System.Xml.Schema.XmlSeverityType.Warning> はスローされません。

次は最初の例から取られたもので、スキーマ検証中に発見したスキーマ検証警告とエラーを受け取る <xref:System.Xml.Schema.ValidationEventHandler> の一例です。

```vb
Shared Sub SchemaValidationEventHandler(sender As Object, e As ValidationEventArgs)

    Select Case e.Severity
        Case XmlSeverityType.Error
            Console.WriteLine(vbCrLf & "Error: {0}", e.Message)
            Exit Sub
        Case XmlSeverityType.Warning
            Console.WriteLine(vbCrLf & "Warning: {0}", e.Message)
            Exit Sub
    End Select
End Sub
```

```csharp
static void SchemaValidationEventHandler(object sender, ValidationEventArgs e)
{
    switch (e.Severity)
    {
        case XmlSeverityType.Error:
            Console.WriteLine("\nError: {0}", e.Message);
            break;
        case XmlSeverityType.Warning:
            Console.WriteLine("\nWarning: {0}", e.Message);
            break;
    }
}
```

<xref:System.Xml.Schema.ValidationEventHandler> の完全な例については、最初の例を参照してください。 詳細については、<xref:System.Xml.Schema.XmlSchemaInfo> クラスのリファレンス ドキュメントを参照してください。

## <a name="xmlschemavalidator-state-transition"></a>XmlSchemaValidator の状態遷移

<xref:System.Xml.Schema.XmlSchemaValidator> クラスには、XML 情報セット内の要素、属性、およびコンテンツを検証するために、各メソッド呼び出しの発生と順序を強制する定義済みの状態遷移があります。

次の表では、<xref:System.Xml.Schema.XmlSchemaValidator> クラスの状態遷移と、各状態で可能なメソッド呼び出しとその順序について説明します。

|状態|切り替え効果|
|-----------|----------------|
|[検証]|<xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> (<xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A> &#124; TopLevel*) <xref:System.Xml.Schema.XmlSchemaValidator.EndValidation%2A>|
|TopLevel|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateWhitespace%2A> &#124; <xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A> &#124; Element|
|要素|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A> <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>* (<xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndOfAttributes%2A> コンテンツ\*)? <xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A> &#124;<br /><br /> <xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A> <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>\* <xref:System.Xml.Schema.XmlSchemaValidator.SkipToEndElement%2A>&#124;<br /><br /> <xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A> <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>\* <xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndOfAttributes%2A> コンテンツ\* <xref:System.Xml.Schema.XmlSchemaValidator.SkipToEndElement%2A>&#124;|
|Content|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateWhitespace%2A> &#124; <xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A> &#124; Element|

> [!NOTE]
> <xref:System.InvalidOperationException> オブジェクトの現在の状態に従って、メソッドの呼び出しが正しい順序で行われないと、上表の各メソッドによって <xref:System.Xml.Schema.XmlSchemaValidator> がスローされます。

上の状態遷移表では、<xref:System.Xml.Schema.XmlSchemaValidator> クラスの状態遷移の各状態で呼び出せるメソッドと他の状態を説明するために、記号が使われています。 使用される記号は、ドキュメント型定義 (DTD) の XML 標準リファレンスで見られる記号と同一です。

次の表は、上の状態遷移表で見られる記号が <xref:System.Xml.Schema.XmlSchemaValidator> クラスの状態遷移中の各状態について、呼び出せるメソッドと状態にどう影響を与えるかを説明するものです。

|[記号]|説明|
|------------|-----------------|
|&#124;|メソッドと状態 (縦棒の前後の項目) のいずれかを呼び出し可能です。|
|ですか。|疑問符の前のメソッドか状態はオプションです。呼び出す場合は 1 回しか呼べません。|
|*|\* 記号の前のメソッドか状態はオプションです。複数回の呼び出しが可能です。|

## <a name="validation-context"></a>検証コンテキスト

XML 情報セット内の要素、属性、コンテンツの検証に使用される <xref:System.Xml.Schema.XmlSchemaValidator> クラスのメソッドは、<xref:System.Xml.Schema.XmlSchemaValidator> オブジェクトの検証コンテキストを変更します。 たとえば、<xref:System.Xml.Schema.XmlSchemaValidator.SkipToEndElement%2A> メソッドは、現在の要素コンテンツの検証をスキップし、親要素のコンテキスト中のコンテンツを検証するために <xref:System.Xml.Schema.XmlSchemaValidator> オブジェクトを準備します。これは、現在の要素のすべての子要素の検証をスキップして <xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A> メソッドを呼ぶのと同じです。

<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> クラスの <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A>、<xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A>、および <xref:System.Xml.Schema.XmlSchemaValidator> メソッドの結果は、検証中の現在のコンテキストに依存します。

次の表では、XML 情報セット内の要素、属性、およびコンテンツの検証に使用する <xref:System.Xml.Schema.XmlSchemaValidator> クラスのメソッドの 1 つを呼び出した後で、これらのメソッドを呼び出した場合の結果について説明します。

|メソッド|GetExpectedParticles|GetExpectedAttributes|AddSchema|
|------------|--------------------------|---------------------------|---------------|
|<xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A>|既定の <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> メソッドが呼び出されると、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> はすべてのグローバル要素を含む配列を返します。<br /><br /> 1 つの要素の部分検証を初期化するために、<xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> をパラメーターとして取るオーバーロードされた <xref:System.Xml.Schema.XmlSchemaObject> メソッドが呼び出された場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> は、<xref:System.Xml.Schema.XmlSchemaValidator> オブジェクトが初期化された対象の要素だけを返します。|既定の <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> メソッドが呼び出されると、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> は空の配列を返します。<br /><br /> 1 つの属性の部分検証を初期化するために <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> をパラメーターとして取るオーバーロードの <xref:System.Xml.Schema.XmlSchemaObject> メソッドが呼び出された場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> は、<xref:System.Xml.Schema.XmlSchemaValidator> オブジェクトが初期化された対象の属性だけを返します。|プリプロセス エラーがない場合は、スキーマを <xref:System.Xml.Schema.XmlSchemaSet> オブジェクトの <xref:System.Xml.Schema.XmlSchemaValidator> に追加します。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A>|コンテキスト要素が有効な場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> はコンテキスト要素の子として期待される一連の要素を返します。<br /><br /> コンテキスト要素が無効な場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> は空の配列を返します。|コンテキスト要素が有効で、これまで <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A> の呼び出しが行われていない場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> はコンテキスト要素上に定義されたすべての属性のリストを返します。<br /><br /> いくつかの属性が既に検証されている場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> は残りの検証すべき属性のリストを返します。<br /><br /> コンテキスト要素が無効な場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> は空の配列を返します。|上と同じ。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>|コンテキスト属性が最上位の属性である場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> は空の配列を返します。<br /><br /> それ以外の場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> は、コンテキスト要素の最初の子として期待される一連の要素を返します。|コンテキスト属性が最上位の属性である場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> は空の配列を返します。<br /><br /> それ以外の場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> は検証する残りの属性のリストを返します。|上と同じ。|
|<xref:System.Xml.Schema.XmlSchemaValidator.GetUnspecifiedDefaultAttributes%2A>|<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> は、コンテキスト要素の最初の子として期待される一連の要素を返します。|<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> は、コンテキスト要素についてまだ検証されていない必須の属性とオプションの属性の一覧を返します。|上と同じ。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndOfAttributes%2A>|<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> は、コンテキスト要素の最初の子として期待される一連の要素を返します。|<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> は空の配列を返します。|上と同じ。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A>|コンテキスト要素の contentType が Mixed の場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> は、次の位置で期待される一連の要素を返します。<br /><br /> コンテキスト要素の contentType が TextOnly または Empty の場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> は空の配列を返します。<br /><br /> コンテキスト要素の contentType が ElementOnly の場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> は、検証エラーが既に発生しているものを除いて、次の位置で期待される一連の要素を返します。|<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> はコンテキスト要素の検証されていない属性リストを返します。|上と同じ。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateWhitespace%2A>|コンテキストの空白が最上位の空白である場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> は空の配列を返します。<br /><br /> それ以外の場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> メソッドの動作は <xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A> の場合と同じです。|コンテキストの空白が最上位の空白である場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> は空の配列を返します。<br /><br /> それ以外の場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> メソッドの動作は <xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A> の場合と同じです。|上と同じ。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A>|<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> はコンテキスト要素以降に期待される一連の要素を返します (兄弟の可能性)。|<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> はコンテキスト要素の検証されていない属性リストを返します。<br /><br /> コンテキスト要素に親がない場合、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> は空のリストを返します (コンテキスト要素が、<xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A> を呼び出した対象である現在の要素の親)。|上と同じ。|
|<xref:System.Xml.Schema.XmlSchemaValidator.SkipToEndElement%2A>|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A> と同じ。|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A> と同じ。|上と同じ。|
|<xref:System.Xml.Schema.XmlSchemaValidator.EndValidation%2A>|空の配列を返します。|空の配列を返します。|上と同じ。|

> [!NOTE]
> <xref:System.Xml.Schema.XmlSchemaValidator> クラスの各種プロパティで返される値は、上の表のどのメソッドを呼び出しても変わることはありません。

## <a name="see-also"></a>関連項目

- <xref:System.Xml.Schema.XmlSchemaValidator>
