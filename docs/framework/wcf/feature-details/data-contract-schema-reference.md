---
title: データ コントラクト スキーマの参照
ms.date: 03/30/2017
helpviewer_keywords:
- data contracts [WCF], schema reference
ms.assetid: 9ebb0ebe-8166-4c93-980a-7c8f1f38f7c0
ms.openlocfilehash: af183fa02ea3ec98f316979198624351d9b25f21
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75963367"
---
# <a name="data-contract-schema-reference"></a>データ コントラクト スキーマの参照

ここでは、XML シリアル化用の共通言語ランタイム (CLR) 型を表すために <xref:System.Runtime.Serialization.DataContractSerializer> が使用する XML スキーマ (XSD) のサブセットについて説明します。

## <a name="datacontractserializer-mappings"></a>DataContractSerializer のマッピング

`DataContractSerializer` は、メタデータエンドポイントまたは[ServiceModel メタデータユーティリティツール (svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)を使用して、WINDOWS COMMUNICATION FOUNDATION (WCF) サービスからメタデータをエクスポートするときに、CLR 型を XSD にマップします。 詳細については、「[データコントラクトシリアライザー](../../../../docs/framework/wcf/feature-details/data-contract-serializer.md)」を参照してください。

また、 `DataContractSerializer` は、Svcutil.exe を使用して Web サービス記述言語 (WSDL) や XSD ドキュメントにアクセスし、サービスまたはクライアントのデータ コントラクトを生成するときに、XSD を CLR 型にマッピングします。

`DataContractSerializer`を使用して CLR 型にマッピングできるのは、この文書に記載されている要件に適合する XML スキーマ インスタンスに限られます。

### <a name="support-levels"></a>サポート レベル

`DataContractSerializer` は、特定の XML スキーマ機能に次のサポート レベルを提供します。

- **サポートあり**。 この機能から CLR 型または属性の一方または両方への、 `DataContractSerializer`を使用する明示的なマッピングが存在します。

- **無視**。 この機能は、 `DataContractSerializer`によってインポートされたスキーマで使用できますが、コードの生成に影響しません。

- **禁止**。 `DataContractSerializer` は、この機能を使用するスキーマのインポートをサポートしません。 たとえば、Svcutil.exe は、この機能を使用するスキーマに基づいて WSDL にアクセスする場合、代わりに <xref:System.Xml.Serialization.XmlSerializer> を使用します。 これが既定値です。

## <a name="general-information"></a>一般情報

- スキーマ名前空間については、「 [XML Schema (XML スキーマ)](https://www.w3.org/2001/XMLSchema)」を参照してください。 このドキュメントでは、プレフィックス "xs" を使用しています。

- スキーマ以外の名前空間を含む属性は無視されます。

- 注釈 (このドキュメントで説明しているものを除きます) はすべて無視されます。

### <a name="xsschema-attributes"></a>\<xs: schema >: attributes

|属性|DataContract|
|---------------|------------------|
|`attributeFormDefault`|無視。|
|`blockDefault`|無視。|
|`elementFormDefault`|修飾する必要があります。 `DataContractSerializer`でスキーマをサポートするには、すべての要素を修飾する必要があります。 これを行うには、xs:schema/@elementFormDefault を "qualified" に設定するか、個々の要素宣言で xs:element/@form を "qualified" に設定します。|
|`finalDefault`|無視。|
|`Id`|無視。|
|`targetNamespace`|サポートされます。データ コントラクト名前空間にマッピングされます。 この属性を指定しなかった場合は、空の名前空間が使用されます。 予約された名前空間をすることはできません `http://schemas.microsoft.com/2003/10/Serialization/` です。|
|`version`|無視。|

### <a name="xsschema-contents"></a>\<xs: schema >: コンテンツ

|目次|Schema|
|--------------|------------|
|`include`|サポートあり。 `DataContractSerializer` では xs:include と xs:import がサポートされています。 ただし、メタデータをローカル ファイルから読み込む場合、Svcutil.exe では、後続の `xs:include/@schemaLocation` 参照と `xs:import/@location` 参照が制限されます。 この場合、 `include` ではなく帯域外機構を通じてスキーマ ファイルの一覧を渡す必要があります。 `include`されたスキーマ ドキュメントは無視されます。|
|`redefine`|禁止。 セキュリティ上の理由により、 `xs:redefine` では、 `DataContractSerializer` の使用が禁止されています。 `x:redefine` では、 `schemaLocation` を後続させる必要があります。 状況によっては、DataContract を使用する Svcutil.exe では、 `schemaLocation`の使用が制限されます。|
|`import`|サポートあり。 `DataContractSerializer` では、 `xs:include` と `xs:import`がサポートされています。 ただし、メタデータをローカル ファイルから読み込む場合、Svcutil.exe では、後続の `xs:include/@schemaLocation` 参照と `xs:import/@location` 参照が制限されます。 この場合、 `include` ではなく帯域外機構を通じてスキーマ ファイルの一覧を渡す必要があります。 `include`されたスキーマ ドキュメントは無視されます。|
|`simpleType`|サポートあり。 `xs:simpleType` のセクションを参照してください。|
|`complexType`|サポートされます。データ コントラクトにマッピングされます。 `xs:complexType` のセクションを参照してください。|
|`group`|無視。 `DataContractSerializer` では、 `xs:group`、 `xs:attributeGroup`、および `xs:attribute`の使用はサポートされていません。 これらの宣言は `xs:schema`の子として無視され、 `complexType` やその他のサポートされている構文内から参照できません。|
|`attributeGroup`|無視。 `DataContractSerializer` では、 `xs:group`、 `xs:attributeGroup`、および `xs:attribute`の使用はサポートされていません。 これらの宣言は `xs:schema`の子として無視され、 `complexType` やその他のサポートされている構文内から参照できません。|
|`element`|サポートあり。 グローバル要素宣言 (GED) を参照してください。|
|`attribute`|無視。 `DataContractSerializer` では、 `xs:group`、 `xs:attributeGroup`、および `xs:attribute`の使用はサポートされていません。 これらの宣言は `xs:schema`の子として無視され、 `complexType` やその他のサポートされている構文内から参照できません。|
|`notation`|無視。|

## <a name="complex-types--xscomplextype"></a>複合型– \<xs: complexType >

### <a name="general-information"></a>一般情報

各複合型 \<xs: complexType >、データコントラクトにマップされます。

### <a name="xscomplextype-attributes"></a>\<xs: complexType >: attributes

|属性|Schema|
|---------------|------------|
|`abstract`|false (既定) のみ有効です。|
|`block`|禁止。|
|`final`|無視。|
|`id`|無視。|
|`mixed`|false (既定) のみ有効です。|
|`name`|サポートされています。データ コントラクト名にマッピングされます。 名前にピリオドが含まれている場合は、内部型への型のマッピングが実行されます。 たとえば、 `A.B` という名前の複合型は、データ コントラクト名 `A`を持つ型の内部型であるデータ コントラクト型にマッピングされますが、このようなデータ コントラクト型が存在する場合に限られます。 複数レベルの入れ子が可能です。たとえば、 `A.B.C` という内部型も可能ですが、これは、 `A` と `A.B` の両方が存在する場合に限られます。|

### <a name="xscomplextype-contents"></a>\<xs:complexType>: contents

|目次|Schema|
|--------------|------------|
|`simpleContent`|拡張は禁止です。<br /><br /> 制限は、 `anySimpleType`からのみ許可されます。|
|`complexContent`|サポートあり。 「継承」を参照してください。|
|`group`|禁止。|
|`all`|禁止。|
|`choice`|許可されていません|
|`sequence`|サポートされます。データ コントラクトのデータ メンバーにマッピングされます。|
|`attribute`|use="prohibited" の場合でも禁止です (ただし、例外が 1 つあります)。 標準シリアル化スキーマ名前空間のオプションの属性のみがサポートされます。 これらは、データ コントラクト プログラミング モデルのデータ メンバーにマッピングされません。 現在、これらの属性で意味のあるものは 1 つだけです。詳細については、ISerializable のセクションを参照してください。 他の属性はすべて無視されます。|
|`attributeGroup`|禁止。 WCF v1 リリースでは、`DataContractSerializer` は `xs:complexType`内の `attributeGroup` の存在を無視します。|
|`anyAttribute`|禁止。|
|(空)|データ メンバーを持たないデータ コントラクトにマッピングされます。|

### <a name="xssequence-in-a-complex-type-attributes"></a>\<xs: sequence > 複合型: attributes

|属性|Schema|
|---------------|------------|
|`id`|無視。|
|`maxOccurs`|1 (既定) のみ有効です。|
|`minOccurs`|1 (既定) のみ有効です。|

### <a name="xssequence-in-a-complex-type-contents"></a>\<xs: sequence > 複合型: コンテンツ

|目次|Schema|
|--------------|------------|
|`element`|各インスタンスがデータ メンバーにマッピングされます。|
|`group`|禁止。|
|`choice`|禁止。|
|`sequence`|禁止。|
|`any`|禁止。|
|(空)|データ メンバーを持たないデータ コントラクトにマッピングされます。|

## <a name="elements--xselement"></a>要素– \<xs: element >

### <a name="general-information"></a>一般情報

`<xs:element>` は、次の構文で発生します。

- `<xs:sequence>`内で発生し、通常 (コレクション以外) のデータ コントラクトのデータ メンバーを表すことができます。 この場合、 `maxOccurs` 属性は 1 にする必要があります (値 0 は使用できません)。

- `<xs:sequence>`内で発生し、コレクション データ コントラクトのデータ メンバーを表すことができます。 この場合、 `maxOccurs` 属性は 2 以上の値か、"unbounded" にする必要があります。

- `<xs:schema>` 内で GED (グローバル要素宣言) として発生します。

### <a name="xselement-with-maxoccurs1-within-an-xssequence-data-members"></a>\<xs: sequence > (データメンバー) 内で maxOccurs = 1 を持つ xs: element > を \<します。

|属性|Schema|
|---------------|------------|
|`ref`|禁止。|
|`name`|サポートされます。データ メンバー名にマッピングされます。|
|`type`|サポートされます。データ メンバー型にマッピングされます。 詳細については、「型/プリミティブのマッピング」を参照してください。 指定しない場合 (および要素に匿名型が含まれていない場合) は、 `xs:anyType` が使用されます。|
|`block`|無視。|
|`default`|禁止。|
|`fixed`|禁止。|
|`form`|修飾する必要があります。 この属性は、 `elementFormDefault` の `xs:schema`を通じて設定できます。|
|`id`|無視。|
|`maxOccurs`|1|
|`minOccurs`|データ メンバーの <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> プロパティにマッピングされます (`IsRequired` が 1 の場合、 `minOccurs` は true です)。|
|`nillable`|型のマッピングに影響します。 「型/プリミティブのマッピング」を参照してください。|

### <a name="xselement-with-maxoccurs1-within-an-xssequence-collections"></a>\<xs: sequence > (Collections) 内の maxOccurs > 1 で、xs: element > を \<します。

- <xref:System.Runtime.Serialization.CollectionDataContractAttribute>にマッピングされます。

- コレクションの型では、xs:sequence 内で xs:element を 1 つしか使用できません。

 コレクションは次のいずれかになります。

- 標準コレクション (配列など)。

- ディクショナリ コレクション (1 つの値を別の値にマッピングする、 <xref:System.Collections.Hashtable>など)。

- ディクショナリとキー/値ペアの配列の種類との唯一の相違は、生成されるプログラミング モデルにあります。 特定の種類がディクショナリ コレクションであることを示すには、スキーマ注釈機構を使用できます。

`ref`、 `block`、 `default`、 `fixed`、 `form`、および `id` の各属性に対するルールは、コレクション以外の場合と同じです。 その他に、次の表に示す属性があります。

|属性|Schema|
|---------------|------------|
|`name`|サポートされます。 <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ItemName%2A> 属性の `CollectionDataContractAttribute` プロパティにマッピングされます。|
|`type`|サポートされます。コレクションに格納されている型にマッピングされます。|
|`maxOccurs`|2 以上または "unbounded"。 DC スキーマでは、"unbounded" を使用する必要があります。|
|`minOccurs`|無視。|
|`nillable`|型のマッピングに影響します。 この属性は、ディクショナリ コレクションでは無視されます。|

### <a name="xselement-within-an-xsschema-global-element-declaration"></a>xs: schema > グローバル要素宣言の \<内で xs: element > を \<

- スキーマ内の型と同じ名前および名前空間を持つか、それ自体の内部で匿名型を定義するグローバル要素宣言 (GED) は、型に関連付けられていると言います。

- スキーマのエクスポート : 単純型と複合型の両方で、生成された型ごとに関連 GED が生成されます。

- 逆シリアル化/シリアル化 : 関連 GED が、該当する型のルート要素として使用されます。

- スキーマのインポート : 次のルールに従う場合、関連 GED は不要となり無視されます (ただし、それが型を定義する場合を除きます)。

|属性|Schema|
|---------------|------------|
|`abstract`|関連 GED では false のみ有効です。|
|`block`|関連 GED では禁止です。|
|`default`|関連 GED では禁止です。|
|`final`|関連 GED では false のみ有効です。|
|`fixed`|関連 GED では禁止です。|
|`id`|無視。|
|`name`|サポートあり。 関連 GED の定義を参照してください。|
|`nillable`|関連 GED では true のみ有効です。|
|`substitutionGroup`|関連 GED では禁止です。|
|`type`|サポートされます。関連 GED に関連付けられた型に一致させる必要があります (ただし、要素に匿名型が含まれている場合を除きます)。|

### <a name="xselement-contents"></a>\<xs: element >: contents

|目次|Schema|
|--------------|------------|
|`simpleType`|サポートされています。*|
|`complexType`|サポートされています。*|
|`unique`|無視。|
|`key`|無視。|
|`keyref`|無視。|
|(空白)|サポートあり。|

匿名型に対して `simpleType` と `complexType,` のマッピングを使用する場合は、匿名型のデータコントラクトが存在しない点を除き、匿名型の場合と同じように \* します。これにより、名前付きデータコントラクトが作成され、要素名から派生した名前が生成されます。 匿名型のルールは、次のとおりです。

- WCF 実装の詳細: `xs:element` 名にピリオドが含まれていない場合、匿名型は外部データコントラクト型の内部型にマップされます。 名前にピリオドが含まれている場合、結果のデータ コントラクト型は、内部型ではなく、独立した型になります。

- 内部型の生成されたデータ コントラクト名は、外部型のデータ コントラクト名の後にピリオド、要素の名前、および文字列 "Type" が付いたものになります。

- そのような名前を持つデータ コントラクトが既に存在する場合は、"1"、"2"、"3" などの番号が付加されて一意の名前になります。

## <a name="simple-types---xssimpletype"></a>Simple Types - \<xs:simpleType>

### <a name="xssimpletype-attributes"></a>\<xs: simpleType >: attributes

|属性|Schema|
|---------------|------------|
|`final`|無視。|
|`id`|無視。|
|`name`|サポートされます。データ コントラクト名にマッピングされます。|

### <a name="xssimpletype-contents"></a>\<xs: simpleType >: contents

|目次|Schema|
|--------------|------------|
|`restriction`|サポートあり。 列挙データ コントラクトにマッピングされます。 列挙パターンに一致しない場合、この属性は無視されます。 `xs:simpleType` の制限のセクションを参照してください。|
|`list`|サポートあり。 フラグ列挙データ コントラクトにマッピングされます。 `xs:simpleType` の一覧のセクションを参照してください。|
|`union`|禁止。|

### <a name="xsrestriction"></a>\<xs:restriction>

- 複合型制限は、base="`xs:anyType`" の場合のみサポートされます。

- `xs:string` 以外の制限ファセットを持たない `xs:enumeration` の単純型制限は、列挙データ コントラクトにマッピングされます。

- その他すべての単純型制限は、それぞれが制限する型にマッピングされます。 たとえば、 `xs:int` の制限は、 `xs:int` 自体と同様に整数にマッピングされます。 プリミティブ型のマッピングの詳細については、「型/プリミティブのマッピング」を参照してください。

### <a name="xsrestriction-attributes"></a>\<xs: restriction >: attributes

|属性|Schema|
|---------------|------------|
|`base`|サポートされている単純型または `xs:anyType`のみ有効です。|
|`id`|無視。|

### <a name="xsrestriction-for-all-other-cases-contents"></a>\<xs: restriction > (その他すべての場合): 内容

|目次|Schema|
|--------------|------------|
|`simpleType`|存在する場合、サポートされているプリミティブ型から派生する必要があります。|
|`minExclusive`|無視。|
|`minInclusive`|無視。|
|`maxExclusive`|無視。|
|`maxInclusive`|無視。|
|`totalDigits`|無視。|
|`fractionDigits`|無視。|
|`length`|無視。|
|`minLength`|無視。|
|`maxLength`|無視。|
|`enumeration`|無視。|
|`whiteSpace`|無視。|
|`pattern`|無視。|
|(空白)|サポートあり。|

## <a name="enumeration"></a>列挙体

### <a name="xsrestriction-for-enumerations-attributes"></a>列挙型の xs: restriction > の \<: 属性

|属性|Schema|
|---------------|------------|
|`base`|存在する場合、 `xs:string`のみ有効です。|
|`id`|無視。|

### <a name="xsrestriction-for-enumerations-contents"></a>列挙型の \<xs: restriction >: 内容

|目次|Schema|
|--------------|------------|
|`simpleType`|存在する場合、データ コントラクトによってサポートされている列挙体制限 (このセクション) のみ有効です。|
|`minExclusive`|無視。|
|`minInclusive`|無視。|
|`maxExclusive`|無視。|
|`maxInclusive`|無視。|
|`totalDigits`|無視。|
|`fractionDigits`|無視。|
|`length`|禁止。|
|`minLength`|禁止。|
|`maxLength`|禁止。|
|`enumeration`|サポートあり。 列挙体 "id" は無視され、"値" が列挙データ コントラクトの値の名前にマッピングされます。|
|`whiteSpace`|禁止。|
|`pattern`|禁止。|
|(空)|サポートされます。空の列挙型にマッピングされます。|

 次のコードは、C# の列挙クラスを示しています。

```csharp
public enum MyEnum
{
  first = 3,
  second = 4,
  third =5
}
```

このクラスは `DataContractSerializer`により、次のスキーマにマッピングされます。 列挙値が 1 から始まる場合、 `xs:annotation` ブロックは生成されません。

```xml
<xs:simpleType name="MyEnum">
  <xs:restriction base="xs:string">
    <xs:enumeration value="first">
      <xs:annotation>
        <xs:appinfo>
          <EnumerationValue xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
          3
          </EnumerationValue>
        </xs:appinfo>
      </xs:annotation>
    </xs:enumeration>
    <xs:enumeration value="second">
      <xs:annotation>
        <xs:appinfo>
          <EnumerationValue xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
          4
          </EnumerationValue>
        </xs:appinfo>
      </xs:annotation>
    </xs:enumeration>
  </xs:restriction>
</xs:simpleType>
```

### <a name="xslist"></a>\<xs:list>

`DataContractSerializer` は、 `System.FlagsAttribute` によってマークされた列挙型を、 `xs:list` から派生した `xs:string`にマッピングします。 これ以外の `xs:list` のバリエーションはサポートされません。

### <a name="xslist-attributes"></a>\<xs: list >: attributes

|属性|Schema|
|---------------|------------|
|`itemType`|禁止。|
|`id`|無視。|

### <a name="xslist-contents"></a>\<xs: list >: contents

|目次|Schema|
|--------------|------------|
|`simpleType`|`xs:string` ファセットを使用する `xs:enumeration` からの制限のみ有効です。|

列挙値が 2 の累乗の数列 (フラグの既定) に従わない場合、値は `xs:annotation/xs:appInfo/ser:EnumerationValue` 要素に格納されます。

たとえば、次のコードは列挙型をフラグとして処理します。

```csharp
[Flags]
public enum AuthFlags
{
  AuthAnonymous = 1,
  AuthBasic = 2,
  AuthNTLM = 4,
  AuthMD5 = 16,
  AuthWindowsLiveID = 64,
}
```

この列挙型は次のスキーマにマッピングされます。

```xml
<xs:simpleType name="AuthFlags">
    <xs:list>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:enumeration value="AuthAnonymous" />
          <xs:enumeration value="AuthBasic" />
          <xs:enumeration value="AuthNTLM" />
          <xs:enumeration value="AuthMD5">
            <xs:annotation>
              <xs:appinfo>
                <EnumerationValue xmlns="http://schemas.microsoft.com/2003/10/Serialization/">16</EnumerationValue>
              </xs:appinfo>
            </xs:annotation>
          </xs:enumeration>
          <xs:enumeration value="AuthWindowsLiveID">
            <xs:annotation>
              <xs:appinfo>
                <EnumerationValue xmlns="http://schemas.microsoft.com/2003/10/Serialization/">64</EnumerationValue>
              </xs:appinfo>
            </xs:annotation>
          </xs:enumeration>
        </xs:restriction>
      </xs:simpleType>
    </xs:list>
  </xs:simpleType>
```

## <a name="inheritance"></a>継承

### <a name="general-rules"></a>一般ルール

データ コントラクトは、別のデータ コントラクトを継承できます。 このようなデータ コントラクトは base にマッピングされ、 `<xs:extension>` XML スキーマ コントラクトを使用する拡張型によって派生されます。

データ コントラクトは、コレクション データ コントラクトを継承できません。

データ コントラクトの例を次のコードに示します。

```csharp
[DataContract]
public class Person
{
  [DataMember]
  public string Name;
}
[DataContract]
public class Employee : Person
{
  [DataMember]
  public int ID;
}
```

このデータ コントラクトは次の XML スキーマ型の宣言にマッピングされます。

```xml
<xs:complexType name="Employee">
 <xs:complexContent mixed="false">
  <xs:extension base="tns:Person">
   <xs:sequence>
    <xs:element minOccurs="0" name="ID" type="xs:int"/>
   </xs:sequence>
  </xs:extension>
 </xs:complexContent>
</xs:complexType>
<xs:complexType name="Person">
 <xs:sequence>
  <xs:element minOccurs="0" name="Name"
    nillable="true" type="xs:string"/>
 </xs:sequence>
</xs:complexType>
```

### <a name="xscomplexcontent-attributes"></a>\<xs: complexContent >: attributes

|属性|Schema|
|---------------|------------|
|`id`|無視。|
|`mixed`|false のみ有効です。|

### <a name="xscomplexcontent-contents"></a>\<xs: complexContent >: コンテンツ

|目次|Schema|
|--------------|------------|
|`restriction`|禁止ですが、base="`xs:anyType`" の場合を除きます。 後者は、 `xs:restriction` のコンテナーの下に `xs:complexContent`のコンテンツを直接配置するのと同じです。|
|`extension`|サポートあり。 データ コントラクトの継承にマッピングされます。|

### <a name="xsextension-in-xscomplexcontent-attributes"></a>xs: complexContent >: 属性での xs: extension > \<の \<

|属性|Schema|
|---------------|------------|
|`id`|無視。|
|`base`|サポートあり。 この型が継承する基本データ コントラクト型にマッピングされます。|

### <a name="xsextension-in-xscomplexcontent-contents"></a>xs: complexContent >: contents \<の \<xs: extension >

`<xs:complexType>` コンテンツと同じルールです。

`<xs:sequence>` を指定した場合は、そのメンバー要素が、派生データ コントラクトに存在する追加のデータ メンバーにマッピングされます。

基本型の要素と同じ名前を持つ要素が派生型に含まれている場合は、重複する要素宣言が、一意のものとして生成される名前を持つデータ メンバーにマッピングされます。 データ メンバーの名前には、一意の名前が見つかるまで正の整数が付加されます ("member1"、"member2" など)。 これに対して、次のようになります。

- 基本データ コントラクトのデータ メンバーと同じ名前および型を持つデータ メンバーが派生データ コントラクトに存在する場合、 `DataContractSerializer` は、これに対応する要素を派生型で生成します。

- 基本データ コントラクトのデータ メンバーと名前は同じでも型が異なるデータ メンバーが派生データ コントラクトに存在する場合、 `DataContractSerializer` は、 `xs:anyType` 型の要素を含むスキーマを、基本型と派生型の両方の宣言にインポートします。 元の型名は、 `xs:annotations/xs:appInfo/ser:ActualType/@Name`に保持されます。

それぞれのデータ メンバーの順序によっては、これら両方のバリエーションにより、あいまいなコンテンツ モデルを含むスキーマが生じる場合があります。

## <a name="typeprimitive-mapping"></a>型/プリミティブのマッピング

`DataContractSerializer` は、XML スキーマのプリミティブ型で次のマッピングを使用します。

|XSD 型|.NET 型|
|--------------|---------------|
|`anyType`|<xref:System.Object>.|
|`anySimpleType`|<xref:System.String>.|
|`duration`|<xref:System.TimeSpan>.|
|`dateTime`|<xref:System.DateTime>.|
|`dateTimeOffset`|オフセットの<xref:System.DateTime> および <xref:System.TimeSpan> 。 後の「DateTimeOffset のシリアル化」を参照してください。|
|`time`|<xref:System.String>.|
|`date`|<xref:System.String>.|
|`gYearMonth`|<xref:System.String>.|
|`gYear`|<xref:System.String>.|
|`gMonthDay`|<xref:System.String>.|
|`gDay`|<xref:System.String>.|
|`gMonth`|<xref:System.String>.|
|`boolean`|<xref:System.Boolean>|
|`base64Binary`|<xref:System.Byte> 配列|
|`hexBinary`|<xref:System.String>.|
|`float`|<xref:System.Single>.|
|`double`|<xref:System.Double>.|
|`anyURI`|<xref:System.Uri>.|
|`QName`|<xref:System.Xml.XmlQualifiedName>.|
|`string`|<xref:System.String>.|
|`normalizedString`|<xref:System.String>.|
|`token`|<xref:System.String>.|
|`language`|<xref:System.String>.|
|`Name`|<xref:System.String>.|
|`NCName`|<xref:System.String>.|
|`ID`|<xref:System.String>.|
|`IDREF`|<xref:System.String>.|
|`IDREFS`|<xref:System.String>.|
|`ENTITY`|<xref:System.String>.|
|`ENTITIES`|<xref:System.String>.|
|`NMTOKEN`|<xref:System.String>.|
|`NMTOKENS`|<xref:System.String>.|
|`decimal`|<xref:System.Decimal>.|
|`integer`|<xref:System.Int64>.|
|`nonPositiveInteger`|<xref:System.Int64>.|
|`negativeInteger`|<xref:System.Int64>.|
|`long`|<xref:System.Int64>.|
|`int`|<xref:System.Int32>.|
|`short`|<xref:System.Int16>.|
|`Byte`|<xref:System.SByte>.|
|`nonNegativeInteger`|<xref:System.Int64>.|
|`unsignedLong`|<xref:System.UInt64>.|
|`unsignedInt`|<xref:System.UInt32>.|
|`unsignedShort`|<xref:System.UInt16>.|
|`unsignedByte`|<xref:System.Byte>.|
|`positiveInteger`|<xref:System.Int64>.|

## <a name="iserializable-types-mapping"></a>ISerializable 型のマッピング

.NET Framework バージョン1.0 では、永続化またはデータ転送のためにオブジェクトをシリアル化するための一般的なメカニズムとして <xref:System.Runtime.Serialization.ISerializable> が導入されました。 `ISerializable` を実装し、アプリケーション間で渡される .NET Framework 型は多数あります。 <xref:System.Runtime.Serialization.DataContractSerializer> は、当然ながら `ISerializable` クラスをサポートします。 `DataContractSerializer` は、型の QName (修飾名) のみが異なり、事実上プロパティ コレクションである `ISerializable` 実装スキーマ型をマッピングします。 たとえば、`DataContractSerializer`マップ<xref:System.Exception>で次の XSD 型を `http://schemas.datacontract.org/2004/07/System` 名前空間。

```xml
<xs:complexType name="Exception">
 <xs:sequence>
  <xs:any minOccurs="0" maxOccurs="unbounded"
      namespace="##local" processContents="skip"/>
 </xs:sequence>
 <xs:attribute ref="ser:FactoryType"/>
</xs:complexType>
```

データ コントラクトのシリアル化スキーマで宣言されたオプションの属性 `ser:FactoryType` は、型を逆シリアル化できるファクトリ クラスを参照します。 ファクトリ クラスは、使用する `DataContractSerializer` インスタンスの既知の型コレクションの一部である必要があります。 既知の型の詳細については、「[データコントラクトの既知の型](../../../../docs/framework/wcf/feature-details/data-contract-known-types.md)」を参照してください。

## <a name="datacontract-serialization-schema"></a>DataContract のシリアル化スキーマ

`DataContractSerializer` によってエクスポートされたいくつかのスキーマでは、次の特別なデータ コントラクト シリアル化名前空間の型、要素、および属性を使用します。

`http://schemas.microsoft.com/2003/10/Serialization`

データ コントラクト シリアル化スキーマの完全な宣言を次に示します。

```xml
<xs:schema attributeFormDefault="qualified"
   elementFormDefault="qualified"
   targetNamespace =
    "http://schemas.microsoft.com/2003/10/Serialization/"
   xmlns:xs="http://www.w3.org/2001/XMLSchema"
   xmlns:tns="http://schemas.microsoft.com/2003/10/Serialization/">

 <!-- Top-level elements for primitive types. -->
 <xs:element name="anyType" nillable="true" type="xs:anyType"/>
 <xs:element name="anyURI" nillable="true" type="xs:anyURI"/>
 <xs:element name="base64Binary"
       nillable="true" type="xs:base64Binary"/>
 <xs:element name="boolean" nillable="true" type="xs:boolean"/>
 <xs:element name="byte" nillable="true" type="xs:byte"/>
 <xs:element name="dateTime" nillable="true" type="xs:dateTime"/>
 <xs:element name="decimal" nillable="true" type="xs:decimal"/>
 <xs:element name="double" nillable="true" type="xs:double"/>
 <xs:element name="float" nillable="true" type="xs:float"/>
 <xs:element name="int" nillable="true" type="xs:int"/>
 <xs:element name="long" nillable="true" type="xs:long"/>
 <xs:element name="QName" nillable="true" type="xs:QName"/>
 <xs:element name="short" nillable="true" type="xs:short"/>
 <xs:element name="string" nillable="true" type="xs:string"/>
 <xs:element name="unsignedByte"
       nillable="true" type="xs:unsignedByte"/>
 <xs:element name="unsignedInt"
       nillable="true" type="xs:unsignedInt"/>
 <xs:element name="unsignedLong"
       nillable="true" type="xs:unsignedLong"/>
 <xs:element name="unsignedShort"
       nillable="true" type="xs:unsignedShort"/>

 <!-- Primitive types introduced for certain .NET simple types. -->
 <xs:element name="char" nillable="true" type="tns:char"/>
 <xs:simpleType name="char">
  <xs:restriction base="xs:int"/>
 </xs:simpleType>

 <!-- xs:duration is restricted to an ordered value space,
    to map to System.TimeSpan -->
 <xs:element name="duration" nillable="true" type="tns:duration"/>
 <xs:simpleType name="duration">
  <xs:restriction base="xs:duration">
   <xs:pattern
     value="\-?P(\d*D)?(T(\d*H)?(\d*M)?(\d*(\.\d*)?S)?)?"/>
   <xs:minInclusive value="-P10675199DT2H48M5.4775808S"/>
   <xs:maxInclusive value="P10675199DT2H48M5.4775807S"/>
  </xs:restriction>
 </xs:simpleType>

 <xs:element name="guid" nillable="true" type="tns:guid"/>
 <xs:simpleType name="guid">
  <xs:restriction base="xs:string">
   <xs:pattern value="[\da-fA-F]{8}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{12}"/>
  </xs:restriction>
 </xs:simpleType>

 <!-- This is used for schemas exported from ISerializable type. -->
 <xs:attribute name="FactoryType" type="xs:QName"/>
</xs:schema>
```

ここで次の点に注意します。

- `ser:char` を導入して、型 <xref:System.Char>の Unicode 文字を表現します。

- `valuespace` の `xs:duration` を順序付きセットに縮小し、 <xref:System.TimeSpan>にマッピングできるようにします。

- `FactoryType` から派生した型からエクスポートされたスキーマで <xref:System.Runtime.Serialization.ISerializable>を使用します。

## <a name="importing-non-datacontract-schemas"></a>非 DataContract スキーマのインポート

`DataContractSerializer` には、 `ImportXmlTypes` XSD プロファイルに準拠しないスキーマのインポートを可能にする `DataContractSerializer` オプションがあります (「 <xref:System.Runtime.Serialization.XsdDataContractImporter.Options%2A> プロパティ」を参照してください)。 このオプションを `true` に設定すると、非準拠スキーマ型を受け入れ、それを次の実装 ( <xref:System.Xml.Serialization.IXmlSerializable> の配列をラップする <xref:System.Xml.XmlNode> ) にマッピングできるようになります (クラス名のみ異なります)。

```csharp
[GeneratedCodeAttribute("System.Runtime.Serialization", "3.0.0.0")]
[System.Xml.Serialization.XmlSchemaProviderAttribute("ExportSchema")]
[System.Xml.Serialization.XmlRootAttribute(IsNullable=false)]
public partial class Person : object, IXmlSerializable
{
  private XmlNode[] nodesField;
  private static XmlQualifiedName typeName =
new XmlQualifiedName("Person","http://Microsoft.ServiceModel.Samples");
  public XmlNode[] Nodes
  {
    get {return this.nodesField;}
    set {this.nodesField = value;}
  }
  public void ReadXml(XmlReader reader)
  {
    this.nodesField = XmlSerializableServices.ReadNodes(reader);
  }
  public void WriteXml(XmlWriter writer)
  {
    XmlSerializableServices.WriteNodes(writer, this.Nodes);
  }
  public System.Xml.Schema.XmlSchema GetSchema()
  {
    return null;
  }
  public static XmlQualifiedName ExportSchema(XmlSchemaSet schemas)
  {
    XmlSerializableServices.AddDefaultSchema(schemas, typeName);
    return typeName;
  }
}
```

## <a name="datetimeoffset-serialization"></a>DateTimeOffset のシリアル化

<xref:System.DateTimeOffset> はプリミティブ型としては扱われません。 その代わりに、2 つの部分から成る複合型要素としてシリアル化されます。 最初の部分は日時を表し、2 番目の部分は日時からのオフセットを表します。 次のコード例に、シリアル化された DateTimeOffset 値を示します。

```xml
<OffSet xmlns:a="http://schemas.datacontract.org/2004/07/System">
  <DateTime i:type="b:dateTime" xmlns=""
    xmlns:b="http://www.w3.org/2001/XMLSchema">2008-08-28T08:00:00
  </DateTime>
  <OffsetMinutes i:type="b:short" xmlns=""
   xmlns:b="http://www.w3.org/2001/XMLSchema">-480
   </OffsetMinutes>
</OffSet>
```

スキーマは次のとおりです。

```xml
<xs:schema targetNamespace="http://schemas.datacontract.org/2004/07/System">
   <xs:complexType name="DateTimeOffset">
      <xs:sequence minOccurs="1" maxOccurs="1">
         <xs:element name="DateTime" type="xs:dateTime"
         minOccurs="1" maxOccurs="1" />
         <xs:element name="OffsetMinutes" type="xs:short"
         minOccurs="1" maxOccurs="1" />
      </xs:sequence>
   </xs:complexType>
</xs:schema>
```

## <a name="see-also"></a>関連項目

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- <xref:System.Runtime.Serialization.XsdDataContractImporter>
- [データ コントラクトの使用](../../../../docs/framework/wcf/feature-details/using-data-contracts.md)
