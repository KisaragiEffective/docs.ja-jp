---
title: XML スキーマのインクルードまたはインポート
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
ms.assetid: fe1b4a11-37f4-4e1a-93c9-239f4fe736c0
ms.openlocfilehash: d9a18876d8a5ba3067aa35c617b1e20fce0411f5
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710779"
---
# <a name="including-or-importing-xml-schemas"></a>XML スキーマのインクルードまたはインポート
XML スキーマには、`<xs:import />` 要素、`<xs:include />` 要素、および `<xs:redefine />` 要素を含めることができます。 これらのスキーマ要素は、インクルードまたはインポートするスキーマの構造を補足するために使用できる他の XML スキーマを参照します。 <xref:System.Xml.Schema.XmlSchemaImport> クラス、<xref:System.Xml.Schema.XmlSchemaInclude> クラス、および <xref:System.Xml.Schema.XmlSchemaRedefine> クラスは、スキーマ オブジェクト モデル (SOM) API でこれらの要素にマップされます。  
  
## <a name="including-or-importing-an-xml-schema"></a>XML スキーマのインクルードまたはインポート  
 次のコード サンプルは、アドレス スキーマを使用して「[XML スキーマの作成](../../../../docs/standard/data/xml/building-xml-schemas.md)」で作成したカスタム スキーマを補足します。 アドレス スキーマを使用してカスタム スキーマを補足すると、カスタム スキーマでアドレス型が使用できるようになります。  
  
 アドレス スキーマを統合するには、`<xs:include />` 要素または `<xs:import />` 要素を使用して、アドレス スキーマのコンポーネントをそのまま使用するか、または `<xs:redefine />` 要素を使用して、カスタム スキーマのニーズに合わせてコンポーネントを変更します。 アドレス スキーマの `targetNamespace` はカスタム スキーマのものとは異なっているため、`<xs:import />` 要素 (インポートのセマンティクス) が使用されます。  
  
 このコード サンプルでは、アドレス スキーマのインクルードを次の手順で行います。  
  
1. カスタム スキーマとアドレス スキーマを新しい <xref:System.Xml.Schema.XmlSchemaSet> オブジェクトに追加し、コンパイルします。 スキーマの読み込みまたはコンパイル時に発生するスキーマ検証に関する警告とエラーは、<xref:System.Xml.Schema.ValidationEventHandler> デリゲートによって処理されます。  
  
2. <xref:System.Xml.Schema.XmlSchema> プロパティを反復処理して、<xref:System.Xml.Schema.XmlSchemaSet> からカスタム スキーマとアドレス スキーマの両方に対するコンパイルされた <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> オブジェクトを取得します。 これらのスキーマはコンパイルされているため、スキーマのコンパイル後の情報セット (PSCI) プロパティにアクセスできます。  
  
3. <xref:System.Xml.Schema.XmlSchemaImport> オブジェクトを作成し、インポートの <xref:System.Xml.Schema.XmlSchemaImport.Namespace%2A> プロパティをアドレス スキーマの名前空間に設定し、インポートの <xref:System.Xml.Schema.XmlSchemaExternal.Schema%2A> プロパティをアドレス スキーマの <xref:System.Xml.Schema.XmlSchema> オブジェクトに設定して、カスタム スキーマの <xref:System.Xml.Schema.XmlSchema.Includes%2A> プロパティにインポートを追加します。  
  
4. <xref:System.Xml.Schema.XmlSchema> クラスの <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> メソッドおよび <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> メソッドを使用して、カスタム スキーマの変更された <xref:System.Xml.Schema.XmlSchemaSet> オブジェクトの再処理とコンパイルを行って、コンソールに出力します。  
  
5. 最後に、カスタム スキーマの <xref:System.Xml.Schema.XmlSchema.Includes%2A> プロパティを使用して、カスタム スキーマにインポートされたすべてのスキーマを再帰的にコンソールに出力します。 <xref:System.Xml.Schema.XmlSchema.Includes%2A> プロパティを使用すると、スキーマに追加されたすべてのインクルード、インポート、および再定義にアクセスできます。  
  
 以下に、完全なコード サンプルおよびコンソールに出力されるカスタム スキーマとアドレス スキーマを示します。  
  
 [!code-cpp[XmlSchemaImportExample#1](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaImportExample/CPP/XmlSchemaImportExample.cpp#1)]
 [!code-csharp[XmlSchemaImportExample#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaImportExample/CS/XmlSchemaImportExample.cs#1)]
 [!code-vb[XmlSchemaImportExample#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaImportExample/VB/XmlSchemaImportExample.vb#1)]  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<xs:schema xmlns:tns="http://www.tempuri.org" targetNamespace="http://www.tempuri.org" xmlns:xs="http://www.w3.org/2001/XMLSchema">  
  <xs:import namespace="http://www.example.com/IPO" />  
  <xs:element name="Customer">  
    <xs:complexType>  
      <xs:sequence>  
        <xs:element name="FirstName" type="xs:string" />  
        <xs:element name="LastName" type="tns:LastNameType" />  
      </xs:sequence>  
      <xs:attribute name="CustomerId" type="xs:positiveInteger" use="required" />  
    </xs:complexType>  
  </xs:element>  
  <xs:simpleType name="LastNameType">  
    <xs:restriction base="xs:string">  
      <xs:maxLength value="20" />  
    </xs:restriction>  
  </xs:simpleType>  
</xs:schema>  
<schema targetNamespace="http://www.example.com/IPO" xmlns="http://www.w3.org/2001/XMLSchema" xmlns:ipo="http://www.example.com/IPO">  
  <annotation>  
    <documentation xml:lang="en">  
      Addresses for International Purchase order schema  
      Copyright 2000 Example.com. All rights reserved.  
    </documentation>  
  </annotation>  
  <complexType name="Address">  
    <sequence>  
      <element name="name"   type="string"/>  
      <element name="street" type="string"/>  
      <element name="city"   type="string"/>  
    </sequence>  
  </complexType>  
  <complexType name="USAddress">  
    <complexContent>  
      <extension base="ipo:Address">  
        <sequence>  
          <element name="state" type="ipo:USState"/>  
          <element name="zip"   type="positiveInteger"/>  
        </sequence>  
      </extension>  
    </complexContent>  
  </complexType>  
  <!-- other Address derivations for more countries or regions -->  
  <simpleType name="USState">  
    <restriction base="string">  
      <enumeration value="AK"/>  
      <enumeration value="AL"/>  
      <enumeration value="AR"/>  
      <!-- and so on ... -->  
    </restriction>  
  </simpleType>  
</schema>  
```  
  
 `<xs:import />` 要素、`<xs:include />` 要素、`<xs:redefine />` 要素、<xref:System.Xml.Schema.XmlSchemaImport> クラス、<xref:System.Xml.Schema.XmlSchemaInclude> クラス、<xref:System.Xml.Schema.XmlSchemaRedefine> クラスの詳細については、[W3C XML スキーマ](https://www.w3.org/XML/Schema)および <xref:System.Xml.Schema?displayProperty=nameWithType> 名前空間クラスのリファレンス ドキュメントを参照してください。  
  
## <a name="see-also"></a>関連項目

- [XML スキーマ オブジェクト モデルの概要](../../../../docs/standard/data/xml/xml-schema-object-model-overview.md)
- [XML スキーマの読み取りと書き込み](../../../../docs/standard/data/xml/reading-and-writing-xml-schemas.md)
- [XML スキーマの作成](../../../../docs/standard/data/xml/building-xml-schemas.md)
- [XML スキーマの走査](../../../../docs/standard/data/xml/traversing-xml-schemas.md)
- [XML スキーマの編集](../../../../docs/standard/data/xml/editing-xml-schemas.md)
- [スキーマをコンパイルするための XmlSchemaSet](../../../../docs/standard/data/xml/xmlschemaset-for-schema-compilation.md)
