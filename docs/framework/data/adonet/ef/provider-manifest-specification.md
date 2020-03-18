---
title: プロバイダー マニフェストの仕様
ms.date: 03/30/2017
ms.assetid: bb450b47-8951-4f99-9350-26f05a4d4e46
ms.openlocfilehash: a9dca140588be26035b235109c48049ce01e9ce1
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73973888"
---
# <a name="provider-manifest-specification"></a>プロバイダー マニフェストの仕様
ここでは、データ ストア プロバイダーでデータ ストアの型および関数がどのようにサポートされているかについて説明します。  
  
 Entity Services は、特定のデータ ストア プロバイダーとは別に動作しますが、データ プロバイダーでモデル、マッピング、およびクエリと基になるデータ ストアの対話方法を明示的に定義できるようにします。 抽象化レイヤーがない場合、Entity Services が対象とするのは、特定のデータ ストアまたはデータ プロバイダーのみです。  
  
 プロバイダーでサポートされる型は、基になるデータベースによって直接的または間接的にサポートされています。 これらの型は必ずしも正確なストア型ではありませんが、プロバイダーが Entity Framework をサポートするために使用する型です。 プロバイダー/ストア型は、Entity Data Model (EDM) 用語で記述されます。  
  
 データ ストアによってサポートされる関数のパラメーターと戻り値の型は、EDM 用語で指定されます。  
  
## <a name="requirements"></a>［要件］  
 Entity Framework とデータストアは、データの損失や切り捨てを発生させることなく、既知の型との間でデータをやり取りできる必要があります。  
  
 プロバイダー マニフェストは、データ ストアへの接続を開くことなく、デザイン時にツールで読み込むことができる必要があります。  
  
 Entity Framework では大文字と小文字が区別されますが、基になるデータストアがではない可能性があります。 マニフェストで EDM 成果物 (識別子と型名など) が定義されて使用されている場合、Entity Framework 大文字と小文字の区別を使用する必要があります。 大文字と小文字が区別されるデータ ストア要素がプロバイダー マニフェストに含まれる場合、その大文字と小文字の区別はプロバイダー マニフェストで保持する必要があります。  
  
 Entity Framework には、すべてのデータプロバイダーのプロバイダーマニフェストが必要です。 Entity Framework のプロバイダーマニフェストがないプロバイダーを使用しようとすると、エラーが発生します。  
  
 次の表では、プロバイダーとの対話によって例外が発生した場合に Entity Framework がスローする例外の種類について説明します。  
  
|懸案事項|例外|  
|-----------|---------------|  
|プロバイダーで DbProviderServices の GetProviderManifest がサポートされていない。|ProviderIncompatibleException|  
|プロバイダー マニフェストが見つからないため、プロバイダー マニフェストを取得しようとすると、プロバイダーから `null` が返される。|ProviderIncompatibleException|  
|無効なプロバイダー マニフェストにより、プロバイダー マニフェストを取得しようとすると、プロバイダーから無効な XML が返される。|ProviderIncompatibleException|  
  
## <a name="scenarios"></a>監視プロセス  
 プロバイダーでは、次のシナリオをサポートしています。  
  
### <a name="writing-a-provider-with-symmetric-type-mapping"></a>対称型マッピングによるプロバイダーの記述  
 マッピングの方向に関係なく、各店舗の種類が1つの EDM 型にマップされる Entity Framework のプロバイダーを作成できます。 EDM 型に対応する非常に単純なマッピングを持つプロバイダー型では、型システムが単純であるか EDM 型と一致するため、対称ソリューションを使用できます。  
  
 ドメインの単純さを利用して、静的な宣言型プロバイダー マニフェストを作成することができます。  
  
 次の 2 つのセクションで構成される XML ファイルを記述します。  
  
- ストア型またはストア関数の "対応する EDM 要素" で表現されるプロバイダー型の一覧。 ストア型には、対応する EDM 型があります。 ストア関数には、対応する EDM 関数があります。 たとえば、varchar は SQL Server の型ですが、対応する EDM 型は文字列になります。  
  
- パラメーターと戻り値の型が EDM 用語で表現される、プロバイダーがサポートする関数の一覧。  
  
### <a name="writing-a-provider-with-asymmetric-type-mapping"></a>非対照型マッピングによるプロバイダーの記述  
 Entity Framework のデータストアプロバイダーを記述する場合、一部の型に対する EDM からプロバイダーへの型のマッピングは、プロバイダーから EDM への型のマッピングとは異なる場合があります。 たとえば、プロバイダー側で unbounded の EDM PrimitiveTypeKind.String が nvarchar(4000) にマップされるのに対して、nvarchar(4000) は EDM PrimitiveTypeKind.String(MaxLength=4000) にマップされる場合があります。  
  
 次の 2 つのセクションで構成される XML ファイルを記述します。  
  
- 両方向 (EDM からプロバイダーおよびプロバイダーから EDM) のマッピングを定義する、EDM 用語で表現されたプロバイダー型の一覧。  
  
- パラメーターと戻り値の型が EDM 用語で表現される、プロバイダーがサポートする関数の一覧。  
  
## <a name="provider-manifest-discoverability"></a>プロバイダー マニフェストの探索可能性  
 マニフェストは、Entity Services の複数のコンポーネントの種類 (Tools や Query など) では間接的に使用されますが、データ ストア メタデータ ローダーを使用してメタデータからより直接的に使用することができます。  
  
 ![dfb3d02b&#45;7a8c&#45;4d51&#45;ac5a&#45;a73d8aa145e6](./media/dfb3d02b-7a8c-4d51-ac5a-a73d8aa145e6.gif "dfb3d02b-7a8c-4d51-ac5a-a73d8aa145e6")  
  
 ただし、指定されたプロバイダーでは、異なるストアや、同じストアの異なるバージョンがサポートされている場合があります。 そのため、プロバイダーは、サポートされるデータ ストアごとに、異なるマニフェストを報告する必要があります。  
  
### <a name="provider-manifest-token"></a>プロバイダー マニフェスト トークン  
 データ ストア接続が開いている場合、プロバイダーは、クエリで情報を取得して、適切なマニフェストを返すことができます。 この動作は、接続情報が利用できない場合やストアに接続できない場合があるオフライン シナリオでは不可能になることもあります。 マニフェストを識別するには、.ssdl ファイルの `ProviderManifestToken` の `Schema` 属性を使用します。 この属性には必須の形式はありません。プロバイダーは、ストアへの接続を開くことなく、マニフェストを特定するために必要最小限の情報を選択します。  
  
 (例:  
  
```xml  
<Schema Namespace="Northwind" Provider="System.Data.SqlClient" ProviderManifestToken="2005" xmlns:edm="http://schemas.microsoft.com/ado/2006/04/edm/ssdl" xmlns="http://schemas.microsoft.com/ado/2006/04/edm/ssdl">  
```  
  
## <a name="provider-manifest-programming-model"></a>プロバイダー マニフェストのプログラミング モデル  
 プロバイダーは <xref:System.Data.Common.DbXmlEnabledProviderManifest> から派生します。これにより、プロバイダーは、そのマニフェストを宣言によって指定できます。 プロバイダーのクラス階層を次の図に示します。  
  
 ![None](./media/d541eba3-2ee6-4cd1-88f5-89d0b2582a6c.gif "d541eba3-2ee6-4cd1-88f5-89d0b2582a6c")  
  
### <a name="discoverability-api"></a>探索可能性の API  
 プロバイダー マニフェストは、データ ストア接続またはプロバイダー マニフェスト トークンのいずれかを使用して、ストア メタデータ ローダー (StoreItemCollection) によって読み込まれます。  
  
#### <a name="using-a-data-store-connection"></a>データ ストア接続を使用する  
 データストア接続が使用可能な場合は、<xref:System.Data.Common.DbProviderServices.GetProviderManifestToken%2A?displayProperty=nameWithType> を呼び出して、<xref:System.Data.Common.DbProviderServices.GetProviderManifest%2A> メソッドに渡されるトークンを返します。このトークンは <xref:System.Data.Common.DbProviderManifest>を返します。 このメソッドは、プロバイダーの `GetDbProviderManifestToken`の実装にデリゲートします。  
  
```csharp
public string GetProviderManifestToken(DbConnection connection);  
public DbProviderManifest GetProviderManifest(string manifestToken);  
```  
  
#### <a name="using-a-provider-manifest-token"></a>プロバイダー マニフェスト トークンを使用する  
 オフライン シナリオの場合、SSDL の表現からトークンが選択されます。 SSDL では、ProviderManifestToken を指定できます (詳細については、「 [Schema 要素 (SSDL)](/ef/ef6/modeling/designer/advanced/edmx/ssdl-spec#schema-element-ssdl) 」を参照してください)。 たとえば、接続を開くことができない場合、SSDL には、マニフェストに関する情報を指定するプロバイダー マニフェスト トークンがあります。  
  
```csharp
public DbProviderManifest GetProviderManifest(string manifestToken);  
```  
  
### <a name="provider-manifest-schema"></a>プロバイダー マニフェスト スキーマ  
 プロバイダーごとに定義された情報のスキーマには、メタデータによって使用される静的な情報が含まれています。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<xs:schema elementFormDefault="qualified"  
   xmlns:xs="http://www.w3.org/2001/XMLSchema"  
   targetNamespace="http://schemas.microsoft.com/ado/2006/04/edm/providermanifest"  
   xmlns:pm="http://schemas.microsoft.com/ado/2006/04/edm/providermanifest">  
  
  <xs:element name="ProviderManifest">  
    <xs:complexType>  
      <xs:sequence>  
        <xs:element name="Types" type="pm:TTypes" minOccurs="1" maxOccurs="1" />  
        <xs:element name="Functions" type="pm:TFunctions" minOccurs="0" maxOccurs="1"/>  
      </xs:sequence>  
      <xs:attribute name="Namespace" type="xs:string" use="required"/>  
    </xs:complexType>  
  </xs:element>  
  <xs:complexType name="TVersion">  
    <xs:attribute name="Major" type="xs:int" use="required" />  
    <xs:attribute name="Minor" type="xs:int" use="required" />  
    <xs:attribute name="Build" type="xs:int" use="required" />  
    <xs:attribute name="Revision" type="xs:int" use="required" />  
  </xs:complexType>  
  
  <xs:complexType name="TIntegerFacetDescription">  
    <xs:attribute name="Minimum" type="xs:int" use="optional" />  
    <xs:attribute name="Maximum" type="xs:int" use="optional" />  
    <xs:attribute name="DefaultValue" type="xs:int" use="optional" />  
    <xs:attribute name="Constant" type="xs:boolean" default="false" />  
  </xs:complexType>  
  
  <xs:complexType name="TBooleanFacetDescription">  
    <xs:attribute name="DefaultValue" type="xs:boolean" use="optional" />  
    <xs:attribute name="Constant" type="xs:boolean" default="true" />  
  </xs:complexType>  
  
  <xs:complexType name="TDateTimeFacetDescription">  
    <xs:attribute name="Constant" type="xs:boolean" default="false" />  
  </xs:complexType>  
  
  <xs:complexType name="TFacetDescriptions">  
    <xs:choice maxOccurs="unbounded">  
      <xs:element name="Precision" minOccurs="0" maxOccurs="1" type="pm:TIntegerFacetDescription"/>  
      <xs:element name="Scale" minOccurs="0" maxOccurs="1" type="pm:TIntegerFacetDescription"/>  
      <xs:element name="MaxLength" minOccurs="0" maxOccurs="1" type="pm:TIntegerFacetDescription"/>  
      <xs:element name="Unicode" minOccurs="0" maxOccurs="1" type="pm:TBooleanFacetDescription"/>  
      <xs:element name="FixedLength" minOccurs="0" maxOccurs="1" type="pm:TBooleanFacetDescription"/>  
    </xs:choice>  
  </xs:complexType>  
  
  <xs:complexType name="TType">  
    <xs:sequence>  
      <xs:element name="FacetDescriptions" type="pm:TFacetDescriptions" minOccurs="0" maxOccurs="1"/>  
    </xs:sequence>  
    <xs:attribute name="Name" type="xs:string" use="required"/>  
    <xs:attribute name="PrimitiveTypeKind" type="pm:TPrimitiveTypeKind" use="required" />  
  </xs:complexType>  
  
  <xs:complexType name="TTypes">  
    <xs:sequence>  
      <xs:element name="Type" type="pm:TType" minOccurs="0" maxOccurs="unbounded"/>  
    </xs:sequence>  
  </xs:complexType>  
  
  <xs:attributeGroup name="TFacetAttribute">  
    <xs:attribute name="Precision" type="xs:int" use="optional"/>  
    <xs:attribute name="Scale" type="xs:int" use="optional"/>  
    <xs:attribute name="MaxLength" type="xs:int" use="optional"/>  
    <xs:attribute name="Unicode" type="xs:boolean" use="optional"/>  
    <xs:attribute name="FixedLength" type="xs:boolean" use="optional"/>  
  </xs:attributeGroup>  
  
  <xs:complexType name="TFunctionParameter">  
    <xs:attribute name="Name" type="xs:string" use="required" />  
    <xs:attribute name="Type" type="xs:string" use="required" />  
    <xs:attributeGroup ref="pm:TFacetAttribute" />  
    <xs:attribute name="Mode" type="pm:TParameterDirection" use="required" />  
  </xs:complexType>  
  
  <xs:complexType name="TReturnType">  
    <xs:attribute name="Type" type="xs:string" use="required" />  
    <xs:attributeGroup ref="pm:TFacetAttribute" />  
  </xs:complexType>  
  
  <xs:complexType name="TFunction">  
    <xs:choice minOccurs="0" maxOccurs ="unbounded">  
      <xs:element name ="ReturnType" type="pm:TReturnType" minOccurs="0" maxOccurs="1" />  
      <xs:element name="Parameter" type="pm:TFunctionParameter" minOccurs="0" maxOccurs="unbounded"/>  
    </xs:choice>  
    <xs:attribute name="Name" type="xs:string" use="required" />  
    <xs:attribute name="Aggregate" type="xs:boolean" use="optional" />  
    <xs:attribute name="BuiltIn" type="xs:boolean" use="optional" />  
    <xs:attribute name="StoreFunctionName" type="xs:string" use="optional" />  
    <xs:attribute name="NiladicFunction" type="xs:boolean" use="optional" />  
    <xs:attribute name="ParameterTypeSemantics" type="pm:TParameterTypeSemantics" use="optional" default="AllowImplicitConversion" />  
  </xs:complexType>  
  
  <xs:complexType name="TFunctions">  
    <xs:sequence>  
      <xs:element name="Function" type="pm:TFunction" minOccurs="0" maxOccurs="unbounded"/>  
    </xs:sequence>  
  </xs:complexType>  
  
  <xs:simpleType name="TPrimitiveTypeKind">  
    <xs:restriction base="xs:string">  
      <xs:enumeration value="Binary"/>  
      <xs:enumeration value="Boolean"/>  
      <xs:enumeration value="Byte"/>  
      <xs:enumeration value="Decimal"/>  
      <xs:enumeration value="DateTime"/>  
      <xs:enumeration value="Time"/>  
      <xs:enumeration value="DateTimeOffset"/>          
      <xs:enumeration value="Double"/>  
      <xs:enumeration value="Guid"/>  
      <xs:enumeration value="Single"/>  
      <xs:enumeration value="SByte"/>  
      <xs:enumeration value="Int16"/>  
      <xs:enumeration value="Int32"/>  
      <xs:enumeration value="Int64"/>  
      <xs:enumeration value="String"/>  
    </xs:restriction>  
  </xs:simpleType>  
  
  <xs:simpleType name="TParameterDirection">  
    <xs:restriction base="xs:string">  
      <xs:enumeration value="In"/>  
      <xs:enumeration value="Out"/>  
      <xs:enumeration value="InOut"/>  
    </xs:restriction>  
  </xs:simpleType>  
  
  <xs:simpleType name="TParameterTypeSemantics">  
    <xs:restriction base="xs:string">  
      <xs:enumeration value="ExactMatchOnly" />  
      <xs:enumeration value="AllowImplicitPromotion" />  
      <xs:enumeration value="AllowImplicitConversion" />  
    </xs:restriction>  
  </xs:simpleType>  
</xs:schema>  
```  
  
#### <a name="types-node"></a>Types ノード  
 プロバイダー マニフェスト内の Types ノードには、データ ストアまたはプロバイダーによってネイティブでサポートされている型に関する情報が含まれています。  
  
##### <a name="type-node"></a>Type ノード  
 各 Type ノードでは、EDM の観点でプロバイダー型が定義されています。 Type ノードには、プロバイダー型の名前、およびマップ先のモデル型とその型マッピングを示すファセットに関する情報が記述されています。  
  
 プロバイダー マニフェストでこの型情報を公開するために、各 TypeInformation の宣言では、各 Type に対応する複数のファセットの説明を定義する必要があります。  
  
|属性名|データの種類|必要|既定値|説明|  
|--------------------|---------------|--------------|-------------------|-----------------|  
|名|文字列型|[はい]|N/A|プロバイダー固有のデータ型の名前|  
|PrimitiveTypeKind|PrimitiveTypeKind|[はい]|N/A|EDM 型の名前|  
  
###### <a name="function-node"></a>Function ノード  
 各 Function では、プロバイダーを介して使用できる 1 つの関数が定義されています。  
  
|属性名|データの種類|必要|既定値|説明|  
|--------------------|---------------|--------------|-------------------|-----------------|  
|名|文字列型|[はい]|N/A|関数の識別子/名前|  
|ReturnType|文字列型|Ｘ|Void|関数の戻り値の EDM 型|  
|Aggregate|ブール型|Ｘ|False|関数が集計関数の場合は True|  
|BuiltIn|ブール型|Ｘ|True|関数がデータ ストアに組み込まれている場合は True|  
|StoreFunctionName|文字列型|Ｘ|\<名 >|データ ストア内の関数名。  関数名のリダイレクト レベルを許可できます。|  
|NiladicFunction|ブール型|Ｘ|False|関数にパラメーターが必要なく、パラメーターなしで呼び出される場合は True|  
|ParameterType<br /><br /> Semantics|ParameterSemantics|Ｘ|AllowImplicit<br /><br /> 変換|クエリ パイプラインによるパラメーター型の置換の処理方法の選択<br /><br /> - ExactMatchOnly<br />- AllowImplicitPromotion<br />- AllowImplicitConversion|  
  
 **パラメーターノード**  
  
 各関数には、1 つ以上の Parameter ノードのコレクションが含まれています。  
  
|属性名|データの種類|必要|既定値|説明|  
|--------------------|---------------|--------------|-------------------|-----------------|  
|名|文字列型|[はい]|N/A|パラメーターの識別子/名前|  
|[種類]|文字列型|[はい]|N/A|パラメーターの EDM 型|  
|モード|パラメーター<br /><br /> 方向|[はい]|N/A|パラメーターの方向<br /><br /> -in<br />-out<br />-inout|  
  
##### <a name="namespace-attribute"></a>Namespace 属性  
 各データ ストア プロバイダーでは、マニフェストで定義された情報に対して 1 つの名前空間または名前空間のグループを定義する必要があります。 この名前空間は、Entity SQL クエリで、関数および型の名前を解決するために使用できます。 たとえば SqlServer の場合、 その名前空間は、標準的な関数が Entity SQL クエリでサポートされるように Entity Services で定義された正規の名前空間 (EDM) とは別にする必要があります。  
  
## <a name="see-also"></a>関連項目

- [Entity Framework データ プロバイダーの作成](writing-an-ef-data-provider.md)
