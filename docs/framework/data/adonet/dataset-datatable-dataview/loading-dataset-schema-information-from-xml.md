---
title: XML の DataSet スキーマ情報の読み込み
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 43dfb23b-5cef-46f2-8d87-78f0fba1eb8c
ms.openlocfilehash: d834f0c4517f4ff9fe8645257d5a947c03893881
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73968397"
---
# <a name="loading-dataset-schema-information-from-xml"></a>XML の DataSet スキーマ情報の読み込み
<xref:System.Data.DataSet> のスキーマ (テーブル、列、リレーション、および制約) は、プログラムによって定義するか、<xref:System.Data.Common.DataAdapter>の**Fill**メソッドまたは**FillSchema**メソッドによって作成するか、または XML ドキュメントから読み込むことができます。 XML ドキュメントから**dataset**スキーマ情報を読み込むには、**データセット**の**readxmlschema**または**InferXmlSchema**メソッドを使用します。 **Readxmlschema**を使用すると、xml スキーマ定義言語 (XSD) スキーマを含むドキュメントまたはインライン xml スキーマを含む xml ドキュメントから、**データセット**スキーマ情報を読み込んだり、推測したりすることができます。 **InferXmlSchema**を使用すると、指定した特定の xml 名前空間を無視しながら、xml ドキュメントからスキーマを推論できます。  
  
> [!NOTE]
> Web サービスまたは XML シリアル化を使用して、XSD コンストラクト (入れ子になったリレーションなど) を使用してメモリ内に作成された**データセット**を転送する場合、**データセット**内のテーブルの順序が保持されないことがあります。 この場合、**データセット**の受信者はテーブルの順序に依存しないようにしてください。 ただし、転送される**データセット**のスキーマが、メモリ内に作成されるのではなく、XSD ファイルから読み取られた場合、テーブルの順序は常に保持されます。  
  
## <a name="readxmlschema"></a>ReadXmlSchema  
 データを読み込まずに XML ドキュメントから**dataset**のスキーマを読み込むには、データ**セット**の**readxmlschema**メソッドを使用します。 **Readxmlschema**は、XML スキーマ定義言語 (XSD) スキーマを使用して定義された**データセット**スキーマを作成します。  
  
 **Readxmlschema**メソッドは、ファイル名、ストリーム、または読み込む XML ドキュメントを含んでいる**XmlReader**の1つの引数を受け取ります。 この XML ドキュメントには、スキーマだけが含まれているか、またはデータのある XML 要素と共にスキーマがインラインで含まれています。 インラインスキーマを XML スキーマとして記述する方法の詳細については、「 [Xml スキーマ (XSD) からの DataSet リレーショナル構造の派生](deriving-dataset-relational-structure-from-xml-schema-xsd.md)」を参照してください。  
  
 **Readxmlschema**に渡された xml ドキュメントにインラインスキーマ情報が含まれていない場合、 **READXMLSCHEMA**は xml ドキュメント内の要素からスキーマを推論します。 **データセット**に既にスキーマが含まれている場合、新しいテーブルが存在しない場合は、新しいテーブルを追加することによって現在のスキーマが拡張されます。 既存のテーブルには新しい列は追加されません。 追加する列が**データセット**に既に存在していても、XML で見つかった列と互換性のない型がある場合は、例外がスローされます。 **Readxmlschema**が xml ドキュメントからスキーマを推論する方法の詳細については、「 [Xml からの DataSet リレーショナル構造の推論](inferring-dataset-relational-structure-from-xml.md)」を参照してください。  
  
 **Readxmlschema**は**データセット**のスキーマのみを読み込むか推論しますが、 **dataset**の**ReadXml**メソッドは、XML ドキュメントに含まれているスキーマとデータの両方の読み込みまたは推論を行います。 詳細については、「 [XML からの DataSet の読み込み](loading-a-dataset-from-xml.md)」を参照してください。  
  
 次のコード例は、XML ドキュメントまたは XML ストリームから**データセット**スキーマを読み込む方法を示しています。 最初の例では、XML スキーマファイル名を**Readxmlschema**メソッドに渡しています。 2番目の例は、 **system.object を示しています**。  
  
```vb  
Dim dataSet As DataSet = New DataSet  
dataSet.ReadXmlSchema("schema.xsd")  
```  
  
```csharp  
DataSet dataSet = new DataSet();  
dataSet.ReadXmlSchema("schema.xsd");  
```  
  
```vb  
Dim xmlStream As New System.IO.StreamReader("schema.xsd")
Dim dataSet As DataSet = New DataSet  
dataSet.ReadXmlSchema(xmlStream)  
xmlStream.Close()  
```  
  
```csharp  
System.IO.StreamReader xmlStream = new System.IO.StreamReader("schema.xsd");  
DataSet dataSet = new DataSet();  
dataSet.ReadXmlSchema(xmlStream);  
xmlStream.Close();  
```  
  
## <a name="inferxmlschema"></a>InferXmlSchema  
 データセットの**InferXmlSchema**メソッドを使用して、XML ドキュメントからスキーマを推論するように**データセット**に指示することもでき**ます。** **InferXmlSchema**は、 **XmlReadMode**の**InferSchema** (データの読み込みとスキーマの推論) を使用した**ReadXml**と、読み取り中のドキュメントにインラインスキーマが含まれてい**ない場合と**同様に機能します。 ただし、 **InferXmlSchema**には、スキーマが推論されるときに無視する特定の XML 名前空間を指定できる追加機能が用意されています。 **InferXmlSchema**は、ファイル名、ストリーム、または**XmlReader**によって指定された XML ドキュメントの場所である2つの必須引数を受け取ります。とは、操作によって無視される XML 名前空間の文字列配列です。  
  
 たとえば、次のような XML があるとします。  
  
```xml  
<NewDataSet xmlns:od="urn:schemas-microsoft-com:officedata">  
<Categories>  
  <CategoryID od:adotype="3">1</CategoryID>   
  <CategoryName od:maxLength="15" od:adotype="130">Beverages</CategoryName>   
  <Description od:adotype="203">Soft drinks and teas</Description>   
</Categories>  
<Products>  
  <ProductID od:adotype="20">1</ProductID>   
  <ReorderLevel od:adotype="3">10</ReorderLevel>   
  <Discontinued od:adotype="11">0</Discontinued>   
</Products>  
</NewDataSet>  
```  
  
 前の XML ドキュメントの要素に対して属性が指定されているため、 **Readxmlschema**メソッドと**ReadXml**メソッドの両方が**InferSchema**の**XmlReadMode**を使用すると、ドキュメント内のすべての要素に対して、 **Categories**、 **CategoryID**、**区分**、 **Description**、 **Products**、 **ProductID**、 **ReorderLevel**、および**廃止**されたテーブルが作成されます。 (詳細については、「 [XML からの DataSet リレーショナル構造の推論](inferring-dataset-relational-structure-from-xml.md)」を参照してください)。ただし、より適切な構造としては、 **categories**テーブルと**Products**テーブルだけを作成してから、 **categories**テーブルの**CategoryID**、 **ReorderLevel** **、および** **廃止** **され** **た列を** **作成します**。 推論されたスキーマで XML 要素に指定されている属性が無視されるようにするには、次の例に示すように、 **InferXmlSchema**メソッドを使用して、 **officedata**の xml 名前空間を無視するように指定します。  
  
```vb  
Dim dataSet As DataSet = New DataSet  
dataSet.InferXmlSchema("input_od.xml", New String() {"urn:schemas-microsoft-com:officedata"})  
```  
  
```csharp  
DataSet dataSet = new DataSet();  
dataSet.InferXmlSchema("input_od.xml", new string[] "urn:schemas-microsoft-com:officedata");  
```  
  
## <a name="see-also"></a>関連項目

- [DataSet での XML の使用](using-xml-in-a-dataset.md)
- [XML スキーマ (XSD) からの DataSet リレーショナル構造の派生](deriving-dataset-relational-structure-from-xml-schema-xsd.md)
- [XML からの DataSet リレーショナル構造の推論](inferring-dataset-relational-structure-from-xml.md)
- [XML からの DataSet の読み込み](loading-a-dataset-from-xml.md)
- [DataSet、DataTable、および DataView](index.md)
- [ADO.NET の概要](../ado-net-overview.md)
