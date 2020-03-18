---
title: 外部マップ
ms.date: 03/30/2017
ms.assetid: 076606b8-d889-4ba0-b5da-ae577b146f23
ms.openlocfilehash: ba5af75ae34b233354fec6e9074f3cc96d924c7f
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72003045"
---
# <a name="external-mapping"></a>外部マップ
[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] では、別の XML ファイルを使用してデータベースのデータモデルとオブジェクトモデルの間のマッピングを指定するプロセスである*外部マッピング*がサポートされています。 外部マッピング ファイルを使用すると、次のような利点があります。  
  
- マッピング コードをアプリケーション コードから分離できます。 この方法により、アプリケーション コードの煩雑さが軽減されます。  
  
- 外部マッピング ファイルは、構成ファイルのような方法で扱うことができます。 たとえば、バイナリを配布した後、外部マッピング ファイルを交換するだけでアプリケーションの動作を更新できます。  
  
## <a name="requirements"></a>要件  
 マッピングファイルは XML ファイルである必要があり、ファイルは [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] スキーマ定義 (.xsd) ファイルに対して検証する必要があります。  
  
 次の規則が適用されます。  
  
- マッピング ファイルは XML ファイルである必要があります。  
  
- XML マッピング ファイルは、XML スキーマ定義ファイルに対して有効である必要があります。 詳細については、「[方法: DBML ファイルおよび外部マッピングファイルを検証する](how-to-validate-dbml-and-external-mapping-files.md)」を参照してください。  
  
- 外部マッピングは、属性ベースのマッピングをオーバーライドします。 言い換えると、外部マッピング ソースを使って <xref:System.Data.Linq.DataContext> を作成した場合、<xref:System.Data.Linq.DataContext> はクラスに作成したすべてのマッピング属性を無視します。 この動作は、クラスが外部マッピング ファイルに含まれるかどうかにかかわらず適用されます。  
  
- [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] では、2つのマッピング方法 (属性ベースおよび外部) のハイブリッド使用はサポートされていません。  
  
## <a name="xml-schema-definition-file"></a>XML スキーマ定義ファイル  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] の外部マッピングは、以下のような XML スキーマ定義に対して有効である必要があります。  
  
 このスキーマ定義ファイルを、DBML ファイルの検証に使われるスキーマ定義ファイルと区別してください。 詳細については、「 [LINQ to SQL でのコード生成](code-generation-in-linq-to-sql.md)」を参照してください。  
  
> [!NOTE]
> また、Visual Studio ユーザーは、この XSD ファイルを [XML スキーマ] ダイアログボックスの "LinqToSqlMapping" として検索します。 外部マッピングファイルを検証するためにこのファイルを正しく使用する方法については、「[方法: DBML ファイルおよび外部マッピングファイルを検証](how-to-validate-dbml-and-external-mapping-files.md)する」を参照してください。  
  
```xml  
<?xml version="1.0" encoding="utf-16"?>  
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" targetNamespace="http://schemas.microsoft.com/linqtosql/mapping/2007" xmlns="http://schemas.microsoft.com/linqtosql/mapping/2007"  
elementFormDefault="qualified" >  
  <xs:element name="Database" type="Database" />  
  <xs:complexType name="Database">  
    <xs:sequence>  
      <xs:element name="Table" type="Table" minOccurs="0" maxOccurs="unbounded" />  
      <xs:element name="Function" type="Function" minOccurs="0" maxOccurs="unbounded" />  
    </xs:sequence>  
    <xs:attribute name="Name" type="xs:string" use="optional" />  
    <xs:attribute name="Provider" type="xs:string" use="optional" />  
  </xs:complexType>  
  <xs:complexType name="Table">  
    <xs:sequence>  
      <xs:element name="Type" type="Type" minOccurs="1" maxOccurs="1" />  
    </xs:sequence>  
    <xs:attribute name="Name" type="xs:string" use="optional" />  
    <xs:attribute name="Member" type="xs:string" use="optional" />  
  </xs:complexType>  
  <xs:complexType name="Type">  
    <xs:sequence>  
      <xs:choice minOccurs="0" maxOccurs="unbounded">  
        <xs:element name="Column" type="Column" minOccurs="0" maxOccurs="unbounded" />  
        <xs:element name="Association" type="Association" minOccurs="0" maxOccurs="unbounded" />  
      </xs:choice>  
      <xs:element name="Type" type="Type" minOccurs="0" maxOccurs="unbounded" />  
    </xs:sequence>  
    <xs:attribute name="Name" type="xs:string" use="required" />  
    <xs:attribute name="InheritanceCode" type="xs:string" use="optional" />  
    <xs:attribute name="IsInheritanceDefault" type="xs:boolean" use="optional" />  
  </xs:complexType>  
  <xs:complexType name="Column">  
    <xs:attribute name="Name" type="xs:string" use="optional" />  
    <xs:attribute name="Member" type="xs:string" use="required" />  
    <xs:attribute name="Storage" type="xs:string" use="optional" />  
    <xs:attribute name="DbType" type="xs:string" use="optional" />  
    <xs:attribute name="IsPrimaryKey" type="xs:boolean" use="optional" />  
    <xs:attribute name="IsDbGenerated" type="xs:boolean" use="optional" />  
    <xs:attribute name="CanBeNull" type="xs:boolean" use="optional" />  
    <xs:attribute name="UpdateCheck" type="UpdateCheck" use="optional" />  
    <xs:attribute name="IsDiscriminator" type="xs:boolean" use="optional" />  
    <xs:attribute name="Expression" type="xs:string" use="optional" />  
    <xs:attribute name="IsVersion" type="xs:boolean" use="optional" />  
    <xs:attribute name="AutoSync" type="AutoSync" use="optional" />  
  </xs:complexType>  
  <xs:complexType name="Association">  
    <xs:attribute name="Name" type="xs:string" use="optional" />  
    <xs:attribute name="Member" type="xs:string" use="required" />  
    <xs:attribute name="Storage" type="xs:string" use="optional" />  
    <xs:attribute name="ThisKey" type="xs:string" use="optional" />  
    <xs:attribute name="OtherKey" type="xs:string" use="optional" />  
    <xs:attribute name="IsForeignKey" type="xs:boolean" use="optional" />  
    <xs:attribute name="IsUnique" type="xs:boolean" use="optional" />  
    <xs:attribute name="DeleteRule" type="xs:string" use="optional" />  
    <xs:attribute name="DeleteOnNull" type="xs:boolean" use="optional" />  
  </xs:complexType>  
  <xs:complexType name="Function">  
    <xs:sequence>  
      <xs:element name="Parameter" type="Parameter" minOccurs="0" maxOccurs="unbounded" />  
      <xs:choice>  
        <xs:element name="ElementType" type="Type" minOccurs="0" maxOccurs="unbounded" />  
        <xs:element name="Return" type="Return" minOccurs="0" maxOccurs="1" />  
      </xs:choice>  
    </xs:sequence>  
    <xs:attribute name="Name" type="xs:string" use="optional" />  
    <xs:attribute name="Method" type="xs:string" use="required" />  
    <xs:attribute name="IsComposable" type="xs:boolean" use="optional" />  
  </xs:complexType>  
  <xs:complexType name="Parameter">  
    <xs:attribute name="Name" type="xs:string" use="optional" />  
    <xs:attribute name="Parameter" type="xs:string" use="required" />  
    <xs:attribute name="DbType" type="xs:string" use="optional" />  
    <xs:attribute name="Direction" type="ParameterDirection" use="optional" />  
  </xs:complexType>  
  <xs:complexType name="Return">  
    <xs:attribute name="DbType" type="xs:string" use="optional" />  
  </xs:complexType>  
  <xs:simpleType name="UpdateCheck">  
    <xs:restriction base="xs:string">  
      <xs:enumeration value="Always" />  
      <xs:enumeration value="Never" />  
      <xs:enumeration value="WhenChanged" />  
    </xs:restriction>  
  </xs:simpleType>  
  <xs:simpleType name="ParameterDirection">  
    <xs:restriction base="xs:string">  
      <xs:enumeration value="In" />  
      <xs:enumeration value="Out" />  
      <xs:enumeration value="InOut" />  
    </xs:restriction>  
  </xs:simpleType>  
  <xs:simpleType name="AutoSync">  
    <xs:restriction base="xs:string">  
      <xs:enumeration value="Never" />  
      <xs:enumeration value="OnInsert" />  
      <xs:enumeration value="OnUpdate" />  
      <xs:enumeration value="Always" />  
      <xs:enumeration value="Default" />  
    </xs:restriction>  
  </xs:simpleType>  
</xs:schema>  
```  
  
## <a name="see-also"></a>参照

- [LINQ to SQL でのコード生成](code-generation-in-linq-to-sql.md)
- [参照](reference.md)
- [方法 : オブジェクト モデルを外部ファイルとして生成する](how-to-generate-the-object-model-as-an-external-file.md)
