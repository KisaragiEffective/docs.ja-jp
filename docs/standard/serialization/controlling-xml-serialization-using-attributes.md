---
title: 属性を使用した XML シリアル化の制御
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- classes, serializing
- XML serialization, examples
- derived classes, serializing
- arrays, serializing
- XML serialization, attributes
- preventing serialization
- attributes [.NET Framework], XML serialization
- serialization, examples
- serialization, attributes
ms.assetid: 47d4c39d-30e1-4c7b-8a2e-301325390647
ms.openlocfilehash: d4e30984a232b17d1f40e300655c519ec1a6e191
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159911"
---
# <a name="controlling-xml-serialization-using-attributes"></a>属性を使用した XML シリアル化の制御

属性を使用すると、オブジェクトの XML シリアル化を制御したり、同じ一連のクラスから代替 XML ストリームを作成したりできます。 代替 XML ストリームの作成の詳細については、「[方法 : XML ストリームの代替要素名を指定する](how-to-specify-an-alternate-element-name-for-an-xml-stream.md)」を参照してください。

> [!NOTE]
> 生成された XML が[Simple Object Access Protocol (SOAP) 1.1](https://www.w3.org/TR/2000/NOTE-SOAP-20000508/)という名前の WORLD WIDE WEB コンソーシアム (W3C) ドキュメントのセクション5に準拠している必要がある場合は、「属性」に示されている属性を使用して、エンコードされた[soap シリアル化を制御](attributes-that-control-encoded-soap-serialization.md)します。

既定では、クラス名またはメンバー名によって XML 要素名が決まります。 次の例に示すように、`Book` という名前の単純なクラスでは、`ISBN` という名前のフィールドから \<ISBN> という XML 要素タグが生成されます。

```vb
Public Class Book
    Public ISBN As String
End Class
' When an instance of the Book class is serialized, it might
' produce this XML:
' <ISBN>1234567890</ISBN>.
```

```csharp
public class Book
{
    public string ISBN;
}
// When an instance of the Book class is serialized, it might
// produce this XML:
// <ISBN>1234567890</ISBN>.
```

この既定の動作を変更して、要素に新しい名前を付けることもできます。 次のコードでは、<xref:System.Xml.Serialization.XmlElementAttribute.ElementName%2A> の <xref:System.Xml.Serialization.XmlElementAttribute> プロパティを設定することにより、属性でこの変更を実現する方法を示します。

```vb
Public Class TaxRates
   < XmlElement(ElementName = "TaxRate")> _
    Public ReturnTaxRate As Decimal
End Class
```

```csharp
public class TaxRates {
    [XmlElement(ElementName = "TaxRate")]
    public decimal ReturnTaxRate;
}
```

属性の詳細については、「[属性](../../../docs/standard/attributes/index.md)」を参照してください。 XML シリアル化を制御する属性の一覧については、「[Attributes That Control XML Serialization](attributes-that-control-xml-serialization.md)」(XML シリアル化を制御する属性) を参照してください。

## <a name="controlling-array-serialization"></a>配列のシリアル化の制御

<xref:System.Xml.Serialization.XmlArrayAttribute> 属性および <xref:System.Xml.Serialization.XmlArrayItemAttribute> 属性は、配列のシリアル化を制御できるようにデザインされています。 この 2 つの属性を使用して、要素名、名前空間、および XML スキーマ (XSD) データ型 (W3C (www.w3.org) のドキュメント『XML Schema Part 2: Datatypes』で定義) を制御できます。 また、配列に挿入できる型も指定できます。

<xref:System.Xml.Serialization.XmlArrayAttribute> は、配列をシリアル化すると生成される外側の XML 要素のプロパティを決定します。 たとえば、次に示す配列をシリアル化すると、既定で `Employees` という名前の XML 要素が生成されます。 この `Employees` 要素は、`Employee` 配列型に基づいた名前の一連の要素で構成されます。

```vb
Public Class Group
    Public Employees() As Employee
End Class
Public Class Employee
    Public Name As String
End Class
```

```csharp
public class Group {
    public Employee[] Employees;
}
public class Employee {
    public string Name;
}
```

シリアル化されたインスタンスは次のようになります。

```xml
<Group>
<Employees>
    <Employee>
        <Name>Haley</Name>
    </Employee>
</Employees>
</Group>
```

<xref:System.Xml.Serialization.XmlArrayAttribute> を適用すると、この XML 要素の名前を次のように変更できます。

```vb
Public Class Group
    <XmlArray("TeamMembers")> _
    Public Employees() As Employee
End Class
```

```csharp
public class Group {
    [XmlArray("TeamMembers")]
    public Employee[] Employees;
}
```

その結果、次のような XML が生成されます。

```xml
<Group>
<TeamMembers>
    <Employee>
        <Name>Haley</Name>
    </Employee>
</TeamMembers>
</Group>
```

一方、<xref:System.Xml.Serialization.XmlArrayItemAttribute> は、配列内の項目をシリアル化する方法を制御します。 この属性は、配列を返すフィールドに適用されます。

```vb
Public Class Group
    <XmlArrayItem("MemberName")> _
    Public Employee() As Employees
End Class
```

```csharp
public class Group {
    [XmlArrayItem("MemberName")]
    public Employee[] Employees;
}
```

その結果、次のような XML が生成されます。

```xml
<Group>
<Employees>
    <MemberName>Haley</MemberName>
</Employees>
</Group>
```

## <a name="serializing-derived-classes"></a>派生クラスのシリアル化

<xref:System.Xml.Serialization.XmlArrayItemAttribute> のもう 1 つの用途は、派生クラスをシリアル化することです。 たとえば、`Manager` から派生した `Employee` という名前の別のクラスを、前の例に追加できます。 <xref:System.Xml.Serialization.XmlArrayItemAttribute> を適用しないと、この派生クラス型が認識されないため、実行時にコードが失敗します。 これを回避するには、この属性を 2 回適用し、受け入れ可能な各型 (基本型と派生型) の <xref:System.Xml.Serialization.XmlArrayItemAttribute.Type%2A> プロパティをそれぞれ設定します。

```vb
Public Class Group
    <XmlArrayItem(Type:=GetType(Employee)), _
    XmlArrayItem(Type:=GetType(Manager))> _
    Public Employees() As Employee
End Class
Public Class Employee
    Public Name As String
End Class
Public Class Manager
Inherits Employee
    Public Level As Integer
End Class
```

```csharp
public class Group {
    [XmlArrayItem(Type = typeof(Employee)),
    XmlArrayItem(Type = typeof(Manager))]
    public Employee[] Employees;
}
public class Employee {
    public string Name;
}
public class Manager:Employee {
    public int Level;
}
```

シリアル化されたインスタンスは次のようになります。

```xml
<Group>
<Employees>
    <Employee>
        <Name>Haley</Name>
    </Employee>
    <Employee xsi:type = "Manager">
        <Name>Ann</Name>
        <Level>3</Level>
    </Employee>
</Employees>
</Group>
```

## <a name="serializing-an-array-as-a-sequence-of-elements"></a>要素のシーケンスとしての配列のシリアル化

次の例に示すように、配列を返すフィールドに <xref:System.Xml.Serialization.XmlElementAttribute> を適用して、配列を XML 要素のフラットなシーケンスとしてシリアル化することもできます。

```vb
Public Class Group
    <XmlElement> _
    Public Employees() As Employee
End Class
```

```csharp
public class Group {
    [XmlElement]
    public Employee[] Employees;
}
```

シリアル化されたインスタンスは次のようになります。

```xml
<Group>
<Employees>
    <Name>Haley</Name>
</Employees>
<Employees>
    <Name>Noriko</Name>
</Employees>
<Employees>
    <Name>Marco</Name>
</Employees>
</Group>
```

2 つの XML ストリームを区別するもう 1 つの方法は、XML スキーマ定義ツールを使用して、コンパイル済みのコードから XML スキーマ (XSD) ドキュメント ファイルを生成することです (このツールの使用方法の詳細については、「 [Xml スキーマ定義ツールと Xml シリアル化](the-xml-schema-definition-tool-and-xml-serialization.md)」を参照してください)。フィールドに属性が適用されていない場合、スキーマは次の方法で要素を記述します。

```xml
<xs:element minOccurs="0" maxOccurs ="1" name="Employees" type="ArrayOfEmployee" />
```

フィールドに <xref:System.Xml.Serialization.XmlElementAttribute> を適用すると、生成されたスキーマには次のように要素が記述されます。

```xml
<xs:element minOccurs="0" maxOccurs="unbounded" name="Employees" type="Employee" />
```

## <a name="serializing-an-arraylist"></a>ArrayList のシリアル化

<xref:System.Collections.ArrayList> クラスには、さまざまなオブジェクトのコレクションを含めることができます。 このため、<xref:System.Collections.ArrayList> は配列とほとんど同じように使用できます。 型指定されたオブジェクトの配列を返すフィールドを作成する代わりに、単一の <xref:System.Collections.ArrayList> を返すフィールドを作成することもできます。 ただし、配列の場合と同様に、<xref:System.Xml.Serialization.XmlSerializer> に含まれるオブジェクトの型を <xref:System.Collections.ArrayList> に通知する必要があります。 そのためには、次の例に示すように、<xref:System.Xml.Serialization.XmlElementAttribute> の複数のインスタンスをフィールドに割り当てます。

```vb
Public Class Group
    <XmlElement(Type:=GetType(Employee)), _
    XmlElement(Type:=GetType(Manager))> _
    Public Info As ArrayList
End Class
```

```csharp
public class Group {
    [XmlElement(Type = typeof(Employee)),
    XmlElement(Type = typeof(Manager))]
    public ArrayList Info;
}
```

## <a name="controlling-serialization-of-classes-using-xmlrootattribute-and-xmltypeattribute"></a>XmlRootAttribute と XmlTypeAttribute を使用したクラスのシリアル化の制御

クラスのみに適用できる、<xref:System.Xml.Serialization.XmlRootAttribute> と <xref:System.Xml.Serialization.XmlTypeAttribute> という属性があります。 この 2 つの属性は非常に似ています。 <xref:System.Xml.Serialization.XmlRootAttribute> は、シリアル化されたときに XML ドキュメントの開始要素と終了要素、つまりルート要素を表す 1 つのクラスにのみ適用できます。 一方 <xref:System.Xml.Serialization.XmlTypeAttribute> は、ルート クラスを含む任意のクラスに適用できます。

たとえば、前の例では、`Group` クラスがルート クラスであり、そのすべてのパブリック フィールドとパブリック プロパティは、XML ドキュメント内の XML 要素になります。 したがって、存在するルート クラスは 1 つだけです。 <xref:System.Xml.Serialization.XmlRootAttribute> を適用することで、<xref:System.Xml.Serialization.XmlSerializer> によって生成される XML ストリームを制御できます。 たとえば、要素名や名前空間を変更できます。

<xref:System.Xml.Serialization.XmlTypeAttribute> を使用すると、生成される XML のスキーマを制御できます。 この機能は、XML Web サービスを通じてスキーマを公開する必要がある場合に役立ちます。 次の例では、<xref:System.Xml.Serialization.XmlTypeAttribute> と <xref:System.Xml.Serialization.XmlRootAttribute> の両方を同じクラスに適用しています。

```vb
<XmlRoot("NewGroupName"), _
XmlType("NewTypeName")> _
Public Class Group
    Public Employees() As Employee
End Class
```

```csharp
[XmlRoot("NewGroupName")]
[XmlType("NewTypeName")]
public class Group {
    public Employee[] Employees;
}
```

このクラスをコンパイルし、XML スキーマ定義ツールを使用してそのスキーマを生成すると、`Group` を記述する次の XML が生成されます。

```xml
<xs:element name="NewGroupName" type="NewTypeName">
```

これに対し、クラスのインスタンスをシリアル化した場合は、XML ドキュメントに `NewGroupName` のみが生成されます。

```xml
<NewGroupName>
    . . .
</NewGroupName>
```

## <a name="preventing-serialization-with-the-xmlignoreattribute"></a>XmlIgnoreAttribute によるシリアル化の回避

パブリック プロパティやパブリック フィールドをシリアル化する必要がない場合があります。 たとえば、メタデータの格納に使用しているフィールドまたはプロパティの場合、 <xref:System.Xml.Serialization.XmlIgnoreAttribute> を適用すると、<xref:System.Xml.Serialization.XmlSerializer> がそのフィールドまたはプロパティをスキップします。

## <a name="see-also"></a>参照

- [XML シリアル化を制御する属性](attributes-that-control-xml-serialization.md)
- [エンコード済み SOAP シリアル化を制御する属性](attributes-that-control-encoded-soap-serialization.md)
- [XML シリアル化の概要](introducing-xml-serialization.md)
- [XML シリアル化の例](examples-of-xml-serialization.md)
- [方法 : XML ストリームの代替要素名を指定する](how-to-specify-an-alternate-element-name-for-an-xml-stream.md)
- [方法 : オブジェクトをシリアル化する](how-to-serialize-an-object.md)
- [方法 : オブジェクトを逆シリアル化する](how-to-deserialize-an-object.md)
