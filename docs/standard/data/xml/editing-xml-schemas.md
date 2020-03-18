---
title: XML スキーマの編集
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
ms.assetid: fa09c8e5-c2b9-49d2-bb0d-40330cd13e4d
ms.openlocfilehash: d7d9f8e0d4ec2f343b50e68e942bf94e93576f25
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710948"
---
# <a name="editing-xml-schemas"></a>XML スキーマの編集

XML スキーマの編集は、スキーマ オブジェクト モデル (SOM) の最も重要な機能の 1 つです。 XML スキーマの既存の値を変更する場合、SOM のスキーマ コンパイル前のすべてのプロパティを使用できます。 その後、XML スキーマを再コンパイルすると、変更が反映されます。

SOM に読み込まれたスキーマを編集する場合、最初にスキーマの走査を行います。 スキーマを編集する際には、事前に SOM API を使用したスキーマの走査をよく理解しておく必要があります。 また、スキーマのコンパイル後の情報セット (PSCI) のうち、スキーマのコンパイル前のプロパティとコンパイル後のプロパティについてもよく理解しておく必要があります。

## <a name="editing-an-xml-schema"></a>XML スキーマの編集

このセクションには、2 種類のコード サンプルが用意されています。これらはいずれも「[XML スキーマの作成](../../../../docs/standard/data/xml/building-xml-schemas.md)」トピックで作成したカスタム スキーマを編集します。 最初のコード サンプルは、`PhoneNumber` 要素に新しい `Customer` 要素を追加します。2 番目のコード サンプルは、`Title` 要素に新しい `FirstName` 属性を追加します。 また、最初のサンプルは、スキーマのコンパイル前の <xref:System.Xml.Schema.XmlSchema.Elements%2A?displayProperty=nameWithType> コレクションをカスタム スキーマを走査する手段として使用しますが、2 番目のコード サンプルはスキーマのコンパイル後の <xref:System.Xml.Schema.XmlSchema.Items%2A?displayProperty=nameWithType> コレクションを使用します。

### <a name="phonenumber-element-example"></a>PhoneNumber 要素の例

この最初のコード サンプルでは、カスタム スキーマの `PhoneNumber` 要素に新しい `Customer` 要素を追加します。 このコード サンプルでは、次の手順でカスタム スキーマの編集を行います。

1. カスタム スキーマを新しい <xref:System.Xml.Schema.XmlSchemaSet> オブジェクトに追加し、コンパイルします。 スキーマの読み取りまたはコンパイル時に発生するスキーマ検証に関する警告とエラーは、<xref:System.Xml.Schema.ValidationEventHandler> デリゲートで処理されます。

2. <xref:System.Xml.Schema.XmlSchema> プロパティを反復処理して、<xref:System.Xml.Schema.XmlSchemaSet> からコンパイルされた <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> オブジェクトを取得します。 スキーマはコンパイルされているため、スキーマのコンパイル後の情報セット (PSCI) プロパティにアクセスできます。

3. `PhoneNumber` クラスを使用する <xref:System.Xml.Schema.XmlSchemaElement> 要素を作成し、`xs:string` クラスおよび <xref:System.Xml.Schema.XmlSchemaSimpleType> クラスを使用する <xref:System.Xml.Schema.XmlSchemaSimpleTypeRestriction> 単純型の制限を作成し、制限の <xref:System.Xml.Schema.XmlSchemaSimpleTypeRestriction.Facets%2A> プロパティに pattern ファセットを追加し、単純型の <xref:System.Xml.Schema.XmlSchemaSimpleType.Content%2A> プロパティに制限を追加して、<xref:System.Xml.Schema.XmlSchemaElement.SchemaType%2A> 要素の `PhoneNumber` に単純型を追加します。

4. スキーマ コンパイル後の <xref:System.Xml.Schema.XmlSchema.Elements%2A?displayProperty=nameWithType> コレクションの <xref:System.Xml.Schema.XmlSchemaObjectTable.Values%2A> コレクションで、それぞれの <xref:System.Xml.Schema.XmlSchemaElement> を反復処理します。

5. 要素の <xref:System.Xml.Schema.XmlSchemaElement.QualifiedName%2A> が `"Customer"` である場合、`Customer` クラスを使用する複合型の <xref:System.Xml.Schema.XmlSchemaComplexType> 要素と <xref:System.Xml.Schema.XmlSchemaSequence> クラスを使用する複合型の sequence のパーティクルを取得します。

6. シーケンスのスキーマ コンパイル前の <xref:System.Xml.Schema.XmlSchemaSequence.Items%2A> コレクションを使用する既存の `FirstName` 要素および `LastName` 要素を格納するシーケンスに、新しい `PhoneNumber` 要素を追加します。

7. 最後に、<xref:System.Xml.Schema.XmlSchema> クラスの <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> および <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> メソッドを使用して、変更された <xref:System.Xml.Schema.XmlSchemaSet> オブジェクトの再処理とコンパイルを行って、コンソールに出力します。

完全なコード サンプルを次に示します。

[!code-cpp[XmlSchemaEditExample1#1](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaEditExample1/CPP/XmlSchemaEditExample1.cpp#1)]
[!code-csharp[XmlSchemaEditExample1#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaEditExample1/CS/XmlSchemaEditExample1.cs#1)]
[!code-vb[XmlSchemaEditExample1#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaEditExample1/VB/XmlSchemaEditExample1.vb#1)]

変更後のカスタム スキーマ (「[XML スキーマの作成](../../../../docs/standard/data/xml/building-xml-schemas.md)」トピックで作成) を次に示します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema xmlns:tns="http://www.tempuri.org" targetNamespace="http://www.tempuri.org" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="Customer">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="FirstName" type="xs:string" />
        <xs:element name="LastName" type="tns:LastNameType" />
        <xs:element name="PhoneNumber">           <xs:simpleType>             <xs:restriction base="xs:string">               <xs:pattern value="\d{3}-\d{3}-\d(4)" />             </xs:restriction>           </xs:simpleType>         </xs:element>
      </xs:sequence>
      <xs:attribute name="CustomerId" type="xs:positiveInteger" use="required" /
>
    </xs:complexType>
  </xs:element>
  <xs:simpleType name="LastNameType">
    <xs:restriction base="xs:string">
      <xs:maxLength value="20" />
    </xs:restriction>
  </xs:simpleType>
</xs:schema>
```

### <a name="title-attribute-example"></a>Title 属性の例

この 2 番目のコード サンプルでは、カスタム スキーマの `Title` 要素に新しい `FirstName` 属性を追加します。 最初のコード サンプルでは、`FirstName` 要素の型が `xs:string` になっています。 `FirstName` 要素が文字列のコンテンツと同時に属性を持つには、単純なコンテンツの拡張コンテンツ モデルを使用してこの要素の型を複合型に変更する必要があります。

このコード サンプルでは、次の手順でカスタム スキーマの編集を行います。

1. カスタム スキーマを新しい <xref:System.Xml.Schema.XmlSchemaSet> オブジェクトに追加し、コンパイルします。 スキーマの読み取りまたはコンパイル時に発生するスキーマ検証に関する警告とエラーは、<xref:System.Xml.Schema.ValidationEventHandler> デリゲートで処理されます。

2. <xref:System.Xml.Schema.XmlSchema> プロパティを反復処理して、<xref:System.Xml.Schema.XmlSchemaSet> からコンパイルされた <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> オブジェクトを取得します。 スキーマはコンパイルされているため、スキーマのコンパイル後の情報セット (PSCI) プロパティにアクセスできます。

3. `FirstName` クラスを使用して <xref:System.Xml.Schema.XmlSchemaComplexType> 要素に対する複合型を新しく作成します。

4. `xs:string` および <xref:System.Xml.Schema.XmlSchemaSimpleContent> クラスを使用して、<xref:System.Xml.Schema.XmlSchemaSimpleContentExtension> の基本型を使った単純なコンテンツの拡張型を新しく作成します。

5. `Title` の <xref:System.Xml.Schema.XmlSchemaAttribute> を使用して、<xref:System.Xml.Schema.XmlSchemaAttribute.SchemaTypeName%2A> クラスを使用する `xs:string` 属性を新しく作成し、その属性を単純なコンテンツの拡張型に追加します。

6. 単純なコンテンツのコンテンツ モデルを単純なコンテンツの拡張型に設定し、複合型のコンテンツ モデルを単純なコンテンツに設定します。

7. スキーマ コンパイル前の <xref:System.Xml.Schema.XmlSchema.Items%2A?displayProperty=nameWithType> コレクションに、新しい複合型を追加します。

8. スキーマのコンパイル前の <xref:System.Xml.Schema.XmlSchemaObject> コレクションで、それぞれの <xref:System.Xml.Schema.XmlSchema.Items%2A?displayProperty=nameWithType> を反復処理します。

> [!NOTE]
> `FirstName` 要素はスキーマのグローバル要素ではないため、<xref:System.Xml.Schema.XmlSchema.Items%2A?displayProperty=nameWithType> または <xref:System.Xml.Schema.XmlSchema.Elements%2A?displayProperty=nameWithType> コレクションでは使用できません。 コード サンプルでは、`FirstName` 要素を検索するために、最初に `Customer` 要素の検索を行います。
>
> 最初のコード サンプルでは、スキーマ コンパイル後の <xref:System.Xml.Schema.XmlSchema.Elements%2A?displayProperty=nameWithType> コレクションを使用してスキーマを走査しました。 このサンプルでは、スキーマ コンパイル前の <xref:System.Xml.Schema.XmlSchema.Items%2A?displayProperty=nameWithType> コレクションを使用してスキーマを走査します。 どちらのコレクションでもスキーマのグローバル要素にアクセスできますが、<xref:System.Xml.Schema.XmlSchema.Items%2A> コレクションで反復処理を行う場合、スキーマ内のすべてのグローバル要素に対して反復処理を行う必要があり、スキーマに PSCI プロパティが存在しないため、処理時間が長くなります。 PSCI コレクション (<xref:System.Xml.Schema.XmlSchema.Elements%2A?displayProperty=nameWithType>、<xref:System.Xml.Schema.XmlSchema.Attributes%2A?displayProperty=nameWithType>、<xref:System.Xml.Schema.XmlSchema.SchemaTypes%2A?displayProperty=nameWithType> など) では、グローバルな要素、属性、型、および PSCI プロパティに直接アクセスできます。

1. <xref:System.Xml.Schema.XmlSchemaObject> が要素 (その <xref:System.Xml.Schema.XmlSchemaElement.QualifiedName%2A> が `"Customer"`) である場合、`Customer` クラスを使用する複合型の <xref:System.Xml.Schema.XmlSchemaComplexType> 要素と <xref:System.Xml.Schema.XmlSchemaSequence> クラスを使用する複合型の sequence のパーティクルを取得します。

2. スキーマのコンパイル前の <xref:System.Xml.Schema.XmlSchemaParticle> コレクションで、それぞれの <xref:System.Xml.Schema.XmlSchemaSequence.Items%2A?displayProperty=nameWithType> を反復処理します。

3. <xref:System.Xml.Schema.XmlSchemaParticle> が要素 (その <xref:System.Xml.Schema.XmlSchemaElement.QualifiedName%2A> が `"FirstName"`) である場合、<xref:System.Xml.Schema.XmlSchemaElement.SchemaTypeName%2A> 要素の `FirstName` を新しい `FirstName` 複合型に設定します。

4. 最後に、<xref:System.Xml.Schema.XmlSchema> クラスの <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> および <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> メソッドを使用して、変更された <xref:System.Xml.Schema.XmlSchemaSet> オブジェクトの再処理とコンパイルを行って、コンソールに出力します。

完全なコード サンプルを次に示します。

[!code-cpp[XmlSchemaEditExample2#1](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaEditExample2/CPP/XmlSchemaEditExample2.cpp#1)]
[!code-csharp[XmlSchemaEditExample2#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaEditExample2/CS/XmlSchemaEditExample2.cs#1)]
[!code-vb[XmlSchemaEditExample2#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaEditExample2/VB/XmlSchemaEditExample2.vb#1)]

変更後のカスタム スキーマ (「[XML スキーマの作成](../../../../docs/standard/data/xml/building-xml-schemas.md)」トピックで作成) を次に示します。

```xml
<?xml version="1.0" encoding=" utf-8"?>
<xs:schema xmlns:tns="http://www.tempuri.org" targetNamespace="http://www.tempuri.org" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="Customer">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="FirstName" type="tns:FirstNameComplexType" />
        <xs:element name="LastName" type="tns:LastNameType" />
      </xs:sequence>
      <xs:attribute name="CustomerId" type="xs:positiveInteger" use="required" /
>
    </xs:complexType>
  </xs:element>
  <xs:simpleType name="LastNameType">
    <xs:restriction base="xs:string">
      <xs:maxLength value="20" />
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="FirstNameComplexType">     <xs:simpleContent>       <xs:extension base="xs:string">         <xs:attribute name="Title" type="xs:string" />       </xs:extension>     </xs:simpleContent>   </xs:complexType>
</xs:schema>
```

## <a name="see-also"></a>関連項目

- [XML スキーマ オブジェクト モデルの概要](../../../../docs/standard/data/xml/xml-schema-object-model-overview.md)
- [XML スキーマの読み取りと書き込み](../../../../docs/standard/data/xml/reading-and-writing-xml-schemas.md)
- [XML スキーマの作成](../../../../docs/standard/data/xml/building-xml-schemas.md)
- [XML スキーマの走査](../../../../docs/standard/data/xml/traversing-xml-schemas.md)
- [XML スキーマのインクルードまたはインポート](../../../../docs/standard/data/xml/including-or-importing-xml-schemas.md)
- [スキーマをコンパイルするための XmlSchemaSet](../../../../docs/standard/data/xml/xmlschemaset-for-schema-compilation.md)
- [スキーマのコンパイル後の情報セット](../../../../docs/standard/data/xml/post-schema-compilation-infoset.md)
