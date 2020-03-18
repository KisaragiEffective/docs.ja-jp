---
title: スキーマをコンパイルするための XmlSchemaSet
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 55c4b175-3170-4071-9d60-dd5a42f79b54
ms.openlocfilehash: 55347de81c65b7390584415dd29044f4ca4ba02a
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75709817"
---
# <a name="xmlschemaset-for-schema-compilation"></a>スキーマをコンパイルするための XmlSchemaSet
XML スキーマ定義言語 (XSD) スキーマの格納と検証が可能なキャッシュである <xref:System.Xml.Schema.XmlSchemaSet> について説明します。  
  
## <a name="the-xmlschemaset-class"></a>XmlSchemaSet クラス  
 <xref:System.Xml.Schema.XmlSchemaSet> は、XML スキーマ定義言語 (XSD) スキーマの格納と検証が可能なキャッシュです。  
  
 <xref:System.Xml?displayProperty=nameWithType> Version 1.0 では、XML スキーマはスキーマのライブラリとして <xref:System.Xml.Schema.XmlSchemaCollection> クラスに読み込まれました。 <xref:System.Xml?displayProperty=nameWithType> Version 2.0 では、<xref:System.Xml.XmlValidatingReader> クラスおよび <xref:System.Xml.Schema.XmlSchemaCollection> クラスは廃止され、それぞれ <xref:System.Xml.XmlReader.Create%2A> メソッドおよび <xref:System.Xml.Schema.XmlSchemaSet> クラスによって置き換えられています。  
  
 <xref:System.Xml.Schema.XmlSchemaSet> は、標準との互換性、パフォーマンス、廃止された Microsoft XDR (XML-Data Reduced) スキーマ形式など、多くの問題を解決するために導入されました。  
  
 以下は、<xref:System.Xml.Schema.XmlSchemaCollection> クラスと <xref:System.Xml.Schema.XmlSchemaSet> クラスの比較です。  
  
|XmlSchemaCollection|XmlSchemaSet|  
|-------------------------|------------------|  
|Microsoft XDR と W3C XML スキーマをサポート。|W3C XML スキーマだけをサポート。|  
|スキーマは、<xref:System.Xml.Schema.XmlSchemaCollection.Add%2A> メソッドの呼び出し時にコンパイルされます。|スキーマは、<xref:System.Xml.Schema.XmlSchemaSet.Add%2A> メソッドの呼び出し時にコンパイルされません。 これによって、スキーマ ライブラリ作成時のパフォーマンスが向上します。|  
|各スキーマが個別のコンパイル済みバージョンを生成するため、"スキーマ アイランド" が発生する可能性があります。 結果として、すべての include および import はそのスキーマ内だけにスコープ設定されます。|コンパイル済みスキーマは、1 つの論理スキーマ (スキーマの集合を 1 つ) を生成します。 1 つのスキーマ内の、その集合にインポートされ追加されるすべてのスキーマは、集合自体に直接追加されます。 これは、すべてのスキーマに対して、すべての型を使用できることを意味しています。|  
|コレクション内では、特定の対象名前空間に対して 1 つのスキーマだけが存在可能です。|型の競合がない限り、同じ対象名前空間に対して複数のスキーマを追加できます。|  
  
## <a name="migrating-to-the-xmlschemaset"></a>XmlSchemaSet への移行  
 次のサンプル コードでは、廃止された <xref:System.Xml.Schema.XmlSchemaSet> クラスから新しい <xref:System.Xml.Schema.XmlSchemaCollection> クラスへの移行のガイドを提供します。 このコード サンプルは、次に挙げるこれら 2 つのクラスの主な相違点を説明するものです。  
  
- <xref:System.Xml.Schema.XmlSchemaCollection.Add%2A> クラスの <xref:System.Xml.Schema.XmlSchemaCollection> メソッドとは異なり、スキーマは <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> の <xref:System.Xml.Schema.XmlSchemaSet> メソッド呼び出し時にはコンパイルされません。 サンプル コード内では、<xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> の <xref:System.Xml.Schema.XmlSchemaSet> メソッドが明示的に呼び出されます。  
  
- <xref:System.Xml.Schema.XmlSchemaSet> を繰り返すには、<xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> の <xref:System.Xml.Schema.XmlSchemaSet> プロパティを使用する必要があります。  
  
 廃止された <xref:System.Xml.Schema.XmlSchemaCollection> のコード サンプルを次に示します。  
  
```vb  
Dim schemaCollection As XmlSchemaCollection = New XmlSchemaCollection()  
schemaCollection.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd")  
schemaCollection.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd")  
  
Dim schema As XmlSchema  
  
For Each schema in schemaCollection  
  
   Console.WriteLine(schema.TargetNamespace)  
  
Next  
```  
  
```csharp  
XmlSchemaCollection schemaCollection = new XmlSchemaCollection();  
schemaCollection.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd");  
schemaCollection.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd");  
  
foreach(XmlSchema schema in schemaCollection)  
{  
   Console.WriteLine(schema.TargetNamespace);  
}  
```  
  
 対応する <xref:System.Xml.Schema.XmlSchemaSet> のコード サンプルを次に示します。  
  
```vb  
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet()  
schemaSet.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd")  
schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd")  
schemaSet.Compile()  
  
Dim schema As XmlSchema  
  
For Each schema in schemaSet.Schemas()  
  
   Console.WriteLine(schema.TargetNamespace)  
  
Next  
```  
  
```csharp  
XmlSchemaSet schemaSet = new XmlSchemaSet();  
schemaSet.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd");  
schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd");  
schemaSet.Compile();  
  
foreach(XmlSchema schema in schemaSet.Schemas())  
{  
   Console.WriteLine(schema.TargetNamespace);  
}  
```  
  
## <a name="adding-and-retrieving-schemas"></a>スキーマの追加と取り出し  
 スキーマは <xref:System.Xml.Schema.XmlSchemaSet> の <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> メソッドを使用して <xref:System.Xml.Schema.XmlSchemaSet> に追加されます。 スキーマが <xref:System.Xml.Schema.XmlSchemaSet> に追加される場合、そのスキーマは対象名前空間の URI に関連付けられます。 対象名前空間の URI は、<xref:System.Xml.Schema.XmlSchemaSet.Add%2A> メソッドへのパラメーターとして指定するか、名前空間が指定されていない場合、<xref:System.Xml.Schema.XmlSchemaSet> がスキーマ内に定義されている名前空間を使用するかのどちらかです。  
  
 スキーマは <xref:System.Xml.Schema.XmlSchemaSet> の <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> プロパティを使用して <xref:System.Xml.Schema.XmlSchemaSet> から取り出されます。 <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> の <xref:System.Xml.Schema.XmlSchemaSet> プロパティでは、<xref:System.Xml.Schema.XmlSchema> に含まれる <xref:System.Xml.Schema.XmlSchemaSet> オブジェクトに対して繰り返すことができます。 <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> プロパティは、<xref:System.Xml.Schema.XmlSchema> に含まれるすべての <xref:System.Xml.Schema.XmlSchemaSet> オブジェクトを返すか、または対象名前空間のパラメーターが指定された場合、その名前空間に属するすべての <xref:System.Xml.Schema.XmlSchema> オブジェクトを返すかのどちらかです。 対象名前空間として `null` が指定された場合、<xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> プロパティは名前空間のないスキーマをすべて返します。  
  
 次の例では、`books.xsd` 名前空間内の `http://www.contoso.com/books` スキーマを <xref:System.Xml.Schema.XmlSchemaSet> に追加し、`http://www.contoso.com/books` 名前空間に属しているすべてのスキーマを <xref:System.Xml.Schema.XmlSchemaSet> から取り出した後、それらのスキーマを <xref:System.Console> に書き出します。  
  
```vb  
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet  
schemaSet.Add("http://www.contoso.com/books", "books.xsd")  
  
Dim schema As XmlSchema  
  
For Each schema In schemaSet.Schemas("http://www.contoso.com/books")  
  
   schema.Write(Console.Out)  
  
Next  
```  
  
```csharp  
XmlSchemaSet schemaSet = new XmlSchemaSet();  
schemaSet.Add("http://www.contoso.com/books", "books.xsd");  
  
foreach (XmlSchema schema in schemaSet.Schemas("http://www.contoso.com/books"))  
{  
   schema.Write(Console.Out);  
}  
```  
  
 <xref:System.Xml.Schema.XmlSchemaSet> オブジェクトのスキーマの追加と取り出しに関する詳細については、<xref:System.Xml.Schema.XmlSchemaSet.Add%2A> メソッドおよび <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> プロパティのリファレンス ドキュメントを参照してください。  
  
## <a name="compiling-schemas"></a>スキーマのコンパイル  
 <xref:System.Xml.Schema.XmlSchemaSet> 内のスキーマは、<xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> の <xref:System.Xml.Schema.XmlSchemaSet> メソッドによって、1 つの論理スキーマにコンパイルされます。  
  
> [!NOTE]
> 廃止された <xref:System.Xml.Schema.XmlSchemaCollection> クラスとは異なり、スキーマは <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> メソッドの呼び出し時にはコンパイルされません。  
  
 <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> メソッドの実行が成功した場合、<xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> の <xref:System.Xml.Schema.XmlSchemaSet> プロパティは `true` に設定されます。  
  
> [!NOTE]
> <xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> プロパティは、スキーマが <xref:System.Xml.Schema.XmlSchemaSet> 内にあるときに編集されても、影響を受けません。 <xref:System.Xml.Schema.XmlSchemaSet> 内の個別のスキーマの更新は追跡されません。 その結果、<xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> プロパティは、`true` のスキーマが追加または削除されない限り、<xref:System.Xml.Schema.XmlSchemaSet> に含まれるスキーマの 1 つが変更されていても、<xref:System.Xml.Schema.XmlSchemaSet> になる可能性があります。  
  
 次の例では、`books.xsd` ファイルを <xref:System.Xml.Schema.XmlSchemaSet> に追加した後、<xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> メソッドを呼び出します。  
  
```vb  
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet()  
schemaSet.Add("http://www.contoso.com/books", "books.xsd")  
schemaSet.Compile()  
```  
  
```csharp  
XmlSchemaSet schemaSet = new XmlSchemaSet();  
schemaSet.Add("http://www.contoso.com/books", "books.xsd");  
schemaSet.Compile();  
```  
  
 <xref:System.Xml.Schema.XmlSchemaSet> 内のスキーマのコンパイルに関する詳細については、<xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> メソッドのリファレンス ドキュメントを参照してください。  
  
## <a name="reprocessing-schemas"></a>スキーマの再処理  
 <xref:System.Xml.Schema.XmlSchemaSet> 内のスキーマの再処理は、<xref:System.Xml.Schema.XmlSchemaSet.Add%2A> の <xref:System.Xml.Schema.XmlSchemaSet> メソッド呼び出し時にスキーマに対して実行されるすべての前処理手順を実行します。 <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> メソッドの呼び出しが成功した場合、<xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> の <xref:System.Xml.Schema.XmlSchemaSet> プロパティは `false` に設定されます。  
  
 <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> のコンパイル後、<xref:System.Xml.Schema.XmlSchemaSet> 内のスキーマが変更された場合は、<xref:System.Xml.Schema.XmlSchemaSet> メソッドを使用する必要があります。  
  
 次の例は、<xref:System.Xml.Schema.XmlSchemaSet> メソッドを使用して <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> に追加されたスキーマの再処理を示しています。 <xref:System.Xml.Schema.XmlSchemaSet> メソッドを使用して <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> がコンパイルされた後、<xref:System.Xml.Schema.XmlSchemaSet> に追加されたスキーマが変更された場合、<xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> 内のスキーマは変更されているのに、`true` プロパティは <xref:System.Xml.Schema.XmlSchemaSet> に設定されています。 <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> メソッドの呼び出しは、<xref:System.Xml.Schema.XmlSchemaSet.Add%2A> メソッドによって実行されるすべての前処理を実行し、<xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> プロパティを `false` に設定します。  
  
```vb  
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet()  
Dim schema As XmlSchema = schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd")  
schemaSet.Compile()  
  
Dim element As XmlSchemaElement = New XmlSchemaElement()  
schema.Items.Add(element)  
element.Name = "book"  
element.SchemaTypeName = New XmlQualifiedName("string", "http://www.w3.org/2001/XMLSchema")  
  
schemaSet.Reprocess(schema)  
```  
  
```csharp  
XmlSchemaSet schemaSet = new XmlSchemaSet();  
XmlSchema schema = schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd");  
schemaSet.Compile();  
  
XmlSchemaElement element = new XmlSchemaElement();  
schema.Items.Add(element);  
element.Name = "book";  
element.SchemaTypeName = new XmlQualifiedName("string", "http://www.w3.org/2001/XMLSchema");  
  
schemaSet.Reprocess(schema);  
```  
  
 <xref:System.Xml.Schema.XmlSchemaSet> 内のスキーマの前処理に関する詳細については、<xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> メソッドのリファレンス ドキュメントを参照してください。  
  
## <a name="checking-for-a-schema"></a>スキーマの確認  
 <xref:System.Xml.Schema.XmlSchemaSet.Contains%2A> の <xref:System.Xml.Schema.XmlSchemaSet> メソッドを使用して、<xref:System.Xml.Schema.XmlSchemaSet> 内にスキーマが含まれているかどうかを確認できます。 <xref:System.Xml.Schema.XmlSchemaSet.Contains%2A> メソッドは、確認対象に対象名前空間または <xref:System.Xml.Schema.XmlSchema> オブジェクトのどちらかを受け取ります。 どちらの場合も、<xref:System.Xml.Schema.XmlSchemaSet.Contains%2A> メソッドは、`true` 内にスキーマが含まれている場合 <xref:System.Xml.Schema.XmlSchemaSet> を、それ以外の場合 `false` を返します。  
  
 スキーマの確認に関する詳細については、<xref:System.Xml.Schema.XmlSchemaSet.Contains%2A> メソッドのリファレンス ドキュメントを参照してださい。  
  
## <a name="removing-schemas"></a>スキーマの削除  
 スキーマは <xref:System.Xml.Schema.XmlSchemaSet> の <xref:System.Xml.Schema.XmlSchemaSet.Remove%2A> および <xref:System.Xml.Schema.XmlSchemaSet.RemoveRecursive%2A> メソッドを使用して <xref:System.Xml.Schema.XmlSchemaSet> から削除されます。 <xref:System.Xml.Schema.XmlSchemaSet.Remove%2A> メソッドは、指定されたスキーマを <xref:System.Xml.Schema.XmlSchemaSet> から削除し、<xref:System.Xml.Schema.XmlSchemaSet.RemoveRecursive%2A> メソッドは、指定されたスキーマとそのスキーマがインポートしているすべてのスキーマを <xref:System.Xml.Schema.XmlSchemaSet> から削除します。  
  
 以下は、複数のスキーマを <xref:System.Xml.Schema.XmlSchemaSet> に追加してから、<xref:System.Xml.Schema.XmlSchemaSet.RemoveRecursive%2A> メソッドを使用して、それらのスキーマの 1 つとそのスキーマがインポートしているすべてのスキーマを削除する例を示しています。  
  
```vb  
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet()  
schemaSet.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd")  
schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd")  
schemaSet.Add("http://www.contoso.com/music", "http://www.contoso.com/music.xsd")  
  
Dim schema As XmlSchema  
  
For Each schema In schemaSet.Schemas()  
  
   If schema.TargetNamespace = "http://www.contoso.com/music" Then  
      schemaSet.RemoveRecursive(schema)  
   End If  
  
Next  
```  
  
```csharp  
XmlSchemaSet schemaSet = new XmlSchemaSet();  
schemaSet.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd");  
schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd");  
schemaSet.Add("http://www.contoso.com/music", "http://www.contoso.com/music.xsd");  
  
foreach (XmlSchema schema in schemaSet.Schemas())  
{  
   if (schema.TargetNamespace == "http://www.contoso.com/music")  
   {  
      schemaSet.RemoveRecursive(schema);  
   }  
}  
```  
  
 <xref:System.Xml.Schema.XmlSchemaSet> からのスキーマの削除に関する詳細については、<xref:System.Xml.Schema.XmlSchemaSet.Remove%2A> メソッドおよび <xref:System.Xml.Schema.XmlSchemaSet.RemoveRecursive%2A> メソッドのリファレンス ドキュメントを参照してください。  
  
## <a name="schema-resolution-and-xsimport"></a>スキーマの解決と xs:import  
 次の例は、<xref:System.Xml.Schema.XmlSchemaSet> 内で、指定された名前空間に対して複数のスキーマが存在するときに、スキーマをインポートする場合の <xref:System.Xml.Schema.XmlSchemaSet> の動作を示しています。  
  
 例として、<xref:System.Xml.Schema.XmlSchemaSet> 名前空間に関する複数のスキーマを含む `http://www.contoso.com` を考えます。 次の `xs:import` ディレクティブを持つスキーマが <xref:System.Xml.Schema.XmlSchemaSet> に追加されます。  
  
```xml  
<xs:import namespace="http://www.contoso.com" schemaLocation="http://www.contoso.com/schema.xsd" />  
```  
  
 <xref:System.Xml.Schema.XmlSchemaSet> は、`http://www.contoso.com` URL から読み込むことによって、`http://www.contoso.com/schema.xsd` 名前空間に関するスキーマをインポートしようとします。 `http://www.contoso.com` 名前空間に関する他のスキーマ ドキュメントが <xref:System.Xml.Schema.XmlSchemaSet> 内にある場合でも、インポート元のスキーマでは、スキーマ宣言とそのスキーマ ドキュメント内で定義した型だけしか使用できません。 `schema.xsd` ファイルが `http://www.contoso.com/schema.xsd` URL で見つからない場合、`http://www.contoso.com` 名前空間に関するスキーマは、インポート元のスキーマにインポートされません。  
  
## <a name="validating-xml-documents"></a>XML ドキュメントの検証  
 XML ドキュメントは、<xref:System.Xml.Schema.XmlSchemaSet> 内のスキーマに対して検証できます。 スキーマを <xref:System.Xml.XmlReaderSettings> オブジェクトの <xref:System.Xml.Schema.XmlSchemaSet><xref:System.Xml.XmlReaderSettings.Schemas%2A> プロパティに追加するか、<xref:System.Xml.Schema.XmlSchemaSet> を <xref:System.Xml.XmlReaderSettings> オブジェクトの <xref:System.Xml.XmlReaderSettings.Schemas%2A> プロパティに追加することによって、XML ドキュメントを検証します。 次に、<xref:System.Xml.XmlReaderSettings> オブジェクトを作成して、XML ドキュメントを検証するために、<xref:System.Xml.XmlReader.Create%2A> オブジェクトが <xref:System.Xml.XmlReader> クラスの <xref:System.Xml.XmlReader> メソッドによって使用されます。  
  
 <xref:System.Xml.Schema.XmlSchemaSet> を利用して XML 文書を検証する方法については、「[XmlSchemaSet による XML スキーマ (XSD) 検証](../../../../docs/standard/data/xml/xml-schema-xsd-validation-with-xmlschemaset.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.Schema.XmlSchemaSet.Add%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.Contains%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.Remove%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.RemoveRecursive%2A>
- [スキーマ キャッシュとしての XmlSchemaSet](../../../../docs/standard/data/xml/xmlschemaset-for-schema-compilation.md)
- [XmlSchemaSet による XML スキーマ (XSD) 検証](../../../../docs/standard/data/xml/xml-schema-xsd-validation-with-xmlschemaset.md)
