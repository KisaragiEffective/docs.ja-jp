---
title: XPathNavigator を使用したスキーマ検証
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 81fa0e41-d9c9-46f0-b22b-50da839c77f5
ms.openlocfilehash: bfcbf7306e896af54808c49e25f95d0631f5bcc0
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710207"
---
# <a name="schema-validation-using-xpathnavigator"></a>XPathNavigator を使用したスキーマ検証
<xref:System.Xml.XmlDocument> クラスを使用して、<xref:System.Xml.XmlDocument> オブジェクトに含まれる XML コンテンツを 2 つの方法で検証することができます。 最初の方法は、検証型 <xref:System.Xml.XmlReader> オブジェクトを使用して XML コンテンツを検証する方法で、2 番目の方法は、<xref:System.Xml.XmlDocument.Validate%2A> クラスの <xref:System.Xml.XmlDocument> メソッドを使用する方法です。 <xref:System.Xml.XPath.XPathDocument> クラスを使用して XML コンテンツの読み取り専用の検証を行うこともできます。  
  
## <a name="validating-xml-data"></a>XML データの検証  
 <xref:System.Xml.XmlDocument> クラスは、既定では DTD または XML スキーマ定義言語 (XSD) のスキーマ検証を使用した XML ドキュメントの検証は行いません。 XML ドキュメントが整形式であることを確認するだけです。  
  
 XML ドキュメントを検証する最初の方法は、検証型の <xref:System.Xml.XmlDocument> オブジェクトを使用して、ドキュメントが <xref:System.Xml.XmlReader> オブジェクトに読み込まれる際に検証する方法です。 2 番目の方法は、<xref:System.Xml.XmlDocument.Validate%2A> クラスの <xref:System.Xml.XmlDocument> メソッドを使用して、以前に型指定されていない XML ドキュメントを検証する方法です。 いずれの場合も、検証済みの XML ドキュメントに対して行った変更は、<xref:System.Xml.XmlDocument.Validate%2A> クラスの <xref:System.Xml.XmlDocument> メソッドを使用して再度検証することができます。  
  
### <a name="validating-a-document-as-it-is-loaded"></a>読み込み時のドキュメントの検証  
 検証型 <xref:System.Xml.XmlReader> オブジェクトは、<xref:System.Xml.XmlReaderSettings> オブジェクトをパラメーターとして受け取る <xref:System.Xml.XmlReader.Create%2A> クラスの <xref:System.Xml.XmlReader> メソッドに <xref:System.Xml.XmlReaderSettings> オブジェクトを渡すことで作成できます。 パラメーターとして渡された <xref:System.Xml.XmlReaderSettings> オブジェクトには、<xref:System.Xml.XmlReaderSettings.ValidationType%2A> が設定された `Schema` プロパティがあり、<xref:System.Xml.XmlDocument> オブジェクトに含まれる XML ドキュメントの XML スキーマが <xref:System.Xml.XmlReaderSettings.Schemas%2A> プロパティに追加されています。 検証型 <xref:System.Xml.XmlReader> オブジェクトは、次に <xref:System.Xml.XmlDocument> オブジェクトを作成するために使用されます。  
  
 次の例では、検証型 `contosoBooks.xml` オブジェクトを使用して、<xref:System.Xml.XmlDocument> オブジェクトを作成することにより、<xref:System.Xml.XmlDocument> ファイルを <xref:System.Xml.XmlReader> オブジェクトに読み込む際に検証を行います。 この XML ドキュメントはそのスキーマに照らして有効なので、スキーマの検証エラーや警告は何も生成されません。  
  
```vb  
Imports System  
Imports System.Xml  
Imports System.Xml.Schema  
Imports System.Xml.XPath  
  
Class ValidatingReaderExample  
  
    Shared Sub Main(ByVal args() As String)  
  
        Try  
            Dim settings As XmlReaderSettings = New XmlReaderSettings()  
            settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd")  
            settings.ValidationType = ValidationType.Schema  
  
            Dim reader As XmlReader = XmlReader.Create("contosoBooks.xml", settings)  
  
            Dim document As XmlDocument = New XmlDocument()  
            document.Load(reader)  
            Dim navigator As XPathNavigator = document.CreateNavigator()  
  
        Catch e As Exception  
            Console.WriteLine("ValidatingReaderExample.Exception: {0}", e.Message)  
        End Try  
  
    End Sub  
  
    Shared Sub SchemaValidationHandler(ByVal sender As Object, ByVal e As ValidationEventArgs)  
  
        Select Case e.Severity  
            Case XmlSeverityType.Error  
                Console.WriteLine("Schema Validation Error: {0}", e.Message)  
                Exit Sub  
            Case XmlSeverityType.Warning  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message)  
                Exit Sub  
        End Select  
  
    End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using System.Xml;  
using System.Xml.Schema;  
using System.Xml.XPath;  
  
class ValidatingReaderExample  
{  
    static void Main(string[] args)  
    {  
        try  
        {  
            XmlReaderSettings settings = new XmlReaderSettings();  
            settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd");  
            settings.ValidationType = ValidationType.Schema;  
  
            XmlReader reader = XmlReader.Create("contosoBooks.xml", settings);  
  
            XmlDocument document = new XmlDocument();  
            document.Load(reader);  
            XPathNavigator navigator = document.CreateNavigator();  
        }  
        catch (Exception e)  
        {  
            Console.WriteLine("ValidatingReaderExample.Exception: {0}", e.Message);  
        }  
    }  
  
    static void SchemaValidationHandler(object sender, ValidationEventArgs e)  
    {  
        switch (e.Severity)  
        {  
            case XmlSeverityType.Error:  
                Console.WriteLine("Schema Validation Error: {0}", e.Message);  
                break;  
            case XmlSeverityType.Warning:  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message);  
                break;  
        }  
    }  
}  
```  
  
 この例は、`contosoBooks.xml` ファイルを入力として使用します。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 また、`contosoBooks.xsd` ファイルも入力として使用します。  
  
 [!code-xml[XPathXMLExamples#3](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xsd#3)]  
  
 上記の例では、<xref:System.Xml.Schema.XmlSchemaValidationException> を呼び出したときに属性や要素の型が検証スキーマで指定されている型と一致しなかった場合、<xref:System.Xml.XmlDocument.Load%2A> がスローされます。 検証を行う <xref:System.Xml.XmlReaderSettings.ValidationEventHandler> に <xref:System.Xml.XmlReader> が設定されている場合、無効な型が検出されるたびに <xref:System.Xml.XmlReaderSettings.ValidationEventHandler> が呼び出されます。  
  
 <xref:System.Xml.Schema.XmlSchemaException> が <xref:System.Xml.XPath.XPathNavigator.TypedValue%2A> に設定されている属性または要素が `invalid` によりアクセスされると、<xref:System.Xml.XPath.XPathNavigator> がスローされます。  
  
 <xref:System.Xml.Schema.XmlSchemaInfo.Validity%2A> を使用して属性または要素にアクセスするとき、<xref:System.Xml.XPath.XPathNavigator> プロパティを使用して、個々の属性または要素が有効かどうかを判断できます。  
  
> [!NOTE]
> XML ドキュメントが既定値を定義した関連付けられたスキーマと共に <xref:System.Xml.XmlDocument> オブジェクトに読み込まれる場合、<xref:System.Xml.XmlDocument> オブジェクトは、これらの既定値があたかも XML ドキュメント内にあるかのように扱います。 これは、XML ドキュメントで空要素として書かれていても、スキーマから既定値を得た要素に対して <xref:System.Xml.XPath.XPathNavigator.IsEmptyElement%2A> プロパティは常に `false` を返すことを意味します。  
  
### <a name="validating-a-document-using-the-validate-method"></a>Validate メソッドを使用したドキュメントの検証  
 <xref:System.Xml.XmlDocument.Validate%2A> クラスの <xref:System.Xml.XmlDocument> メソッドは、<xref:System.Xml.XmlDocument> オブジェクトの <xref:System.Xml.XmlDocument> プロパティで指定されたスキーマに対して、<xref:System.Xml.XmlDocument.Schemas%2A> オブジェクトに含まれる XML ドキュメントを検証し、情報セットの拡大を実施します。 結果として、<xref:System.Xml.XmlDocument> オブジェクト中の以前は型指定されていない XML ドキュメントは型指定されたドキュメントに置き換わります。  
  
 <xref:System.Xml.XmlDocument> オブジェクトは、<xref:System.Xml.Schema.ValidationEventHandler> メソッドへのパラメーターとして渡された <xref:System.Xml.XmlDocument.Validate%2A> デリゲートを使用してスキーマ検証のエラーと警告を報告します。  
  
 次の例では、`contosoBooks.xml` オブジェクトの <xref:System.Xml.XmlDocument> プロパティに含まれる `contosoBooks.xsd` スキーマに対して、<xref:System.Xml.XmlDocument> オブジェクトに含まれる <xref:System.Xml.XmlDocument.Schemas%2A> ファイルを検証します。  
  
```vb  
Imports System  
Imports System.Xml  
Imports System.Xml.Schema  
Imports System.Xml.XPath  
  
Class ValidateExample  
  
    Shared Sub Main(ByVal args() As String)  
  
        Dim document As XmlDocument = New XmlDocument()  
        document.Load("contosoBooks.xml")  
        Dim navigator As XPathNavigator = document.CreateNavigator()  
  
        document.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd")  
        Dim validation As ValidationEventHandler = New ValidationEventHandler(AddressOf SchemaValidationHandler)  
  
        document.Validate(validation)  
  
    End Sub  
  
    Shared Sub SchemaValidationHandler(ByVal sender As Object, ByVal e As ValidationEventArgs)  
  
        Select Case e.Severity  
            Case XmlSeverityType.Error  
                Console.WriteLine("Schema Validation Error: {0}", e.Message)  
                Exit Sub  
            Case XmlSeverityType.Warning  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message)  
                Exit Sub  
        End Select  
  
    End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using System.Xml;  
using System.Xml.Schema;  
using System.Xml.XPath;  
  
class ValidateExample  
{  
    static void Main(string[] args)  
    {  
        XmlDocument document = new XmlDocument();  
        document.Load("contosoBooks.xml");  
        XPathNavigator navigator = document.CreateNavigator();  
  
        document.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd");  
        ValidationEventHandler validation = new ValidationEventHandler(SchemaValidationHandler);  
  
        document.Validate(validation);  
    }  
  
    static void SchemaValidationHandler(object sender, ValidationEventArgs e)  
    {  
        switch (e.Severity)  
        {  
            case XmlSeverityType.Error:  
                Console.WriteLine("Schema Validation Error: {0}", e.Message);  
                break;  
            case XmlSeverityType.Warning:  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message);  
                break;  
        }  
    }  
}  
```  
  
 この例は、`contosoBooks.xml` ファイルを入力として使用します。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 また、`contosoBooks.xsd` ファイルも入力として使用します。  
  
 [!code-xml[XPathXMLExamples#3](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xsd#3)]  
  
### <a name="validating-modifications"></a>変更点の検証  
 XML ドキュメントに変更が施された後、<xref:System.Xml.XmlDocument.Validate%2A> クラスの <xref:System.Xml.XmlDocument> メソッドを使用して、変更点を XML ドキュメントのスキーマに対して検証できます。  
  
 次の例では、検証型 `contosoBooks.xml` オブジェクトを使用して、<xref:System.Xml.XmlDocument> オブジェクトを作成することにより、<xref:System.Xml.XmlDocument> ファイルを <xref:System.Xml.XmlReader> オブジェクトに読み込む際に検証を行います。 XML ドキュメントは、スキーマ検証のエラーや警告を生成せずに、読み込み時に問題なく検証されます。 次に、この例で `contosoBooks.xsd` スキーマに従うと無効な 2 つの変更が XML ドキュメントに施されます。 最初の変更点は、無効な子要素を挿入してスキーマ検証エラーを引き起こします。2 番目の変更点は、型指定されたノードにノードの型に反して無効な値を設定し、例外が発生します。  
  
```vb  
Imports System  
Imports System.Xml  
Imports System.Xml.Schema  
Imports System.Xml.XPath  
  
Class ValidatingReaderExample  
  
    Shared Sub Main(ByVal args() As String)  
  
        Try  
            Dim settings As XmlReaderSettings = New XmlReaderSettings()  
            settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd")  
            settings.ValidationType = ValidationType.Schema  
  
            Dim reader As XmlReader = XmlReader.Create("contosoBooks.xml", settings)  
  
            Dim document As XmlDocument = New XmlDocument()  
            document.Load(reader)  
            Dim navigator As XPathNavigator = document.CreateNavigator()  
  
            Dim validation As ValidationEventHandler = New ValidationEventHandler(AddressOf SchemaValidationHandler)  
  
            navigator.MoveToChild("bookstore", "http://www.contoso.com/books")  
            navigator.MoveToChild("book", "http://www.contoso.com/books")  
            navigator.MoveToChild("author", "http://www.contoso.com/books")  
  
            navigator.AppendChild("<title>Book Title</title>")  
  
            document.Validate(validation)  
  
            navigator.MoveToParent()  
            navigator.MoveToChild("price", "http://www.contoso.com/books")  
            navigator.SetTypedValue(DateTime.Now)  
        Catch e As Exception  
            Console.WriteLine("ValidatingReaderExample.Exception: {0}", e.Message)  
        End Try  
  
    End Sub  
  
    Shared Sub SchemaValidationHandler(ByVal sender As Object, ByVal e As ValidationEventArgs)  
  
        Select Case e.Severity  
            Case XmlSeverityType.Error  
                Console.WriteLine("Schema Validation Error: {0}", e.Message)  
                Exit Sub  
            Case XmlSeverityType.Warning  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message)  
                Exit Sub  
        End Select  
  
    End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using System.Xml;  
using System.Xml.Schema;  
using System.Xml.XPath;  
  
class ValidatingReaderExample  
{  
    static void Main(string[] args)  
    {  
        try  
        {  
            XmlReaderSettings settings = new XmlReaderSettings();  
            settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd");  
            settings.ValidationType = ValidationType.Schema;  
  
            XmlReader reader = XmlReader.Create("contosoBooks.xml", settings);  
  
            XmlDocument document = new XmlDocument();  
            document.Load(reader);  
            XPathNavigator navigator = document.CreateNavigator();  
  
            ValidationEventHandler validation = new ValidationEventHandler(SchemaValidationHandler);  
  
            navigator.MoveToChild("bookstore", "http://www.contoso.com/books");  
            navigator.MoveToChild("book", "http://www.contoso.com/books");  
            navigator.MoveToChild("author", "http://www.contoso.com/books");  
  
            navigator.AppendChild("<title>Book Title</title>");  
  
            document.Validate(validation);  
  
            navigator.MoveToParent();  
            navigator.MoveToChild("price", "http://www.contoso.com/books");  
            navigator.SetTypedValue(DateTime.Now);  
        }  
        catch (Exception e)  
        {  
            Console.WriteLine("ValidatingReaderExample.Exception: {0}", e.Message);  
        }  
    }  
  
    static void SchemaValidationHandler(object sender, ValidationEventArgs e)  
    {  
        switch (e.Severity)  
        {  
            case XmlSeverityType.Error:  
                Console.WriteLine("Schema Validation Error: {0}", e.Message);  
                break;  
            case XmlSeverityType.Warning:  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message);  
                break;  
        }  
    }  
}  
```  
  
 この例は、`contosoBooks.xml` ファイルを入力として使用します。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 また、`contosoBooks.xsd` ファイルも入力として使用します。  
  
 [!code-xml[XPathXMLExamples#3](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xsd#3)]  
  
 上の例では、<xref:System.Xml.XmlDocument> オブジェクトに含まれる XML ドキュメントに 2 つの変更が施されます。 XML ドキュメントが読み込まれるに従い、発見されたスキーマ検証エラーは検証イベント ハンドラー メソッドで処理され、コンソールに書き出されました。  
  
 この例では、XML ドキュメントが読み込まれた後に検証エラーが挿入され、<xref:System.Xml.XmlDocument.Validate%2A> クラスの <xref:System.Xml.XmlDocument> メソッドを使用して発見されました。  
  
 <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> クラスの <xref:System.Xml.XPath.XPathNavigator> メソッドを使用して行われた変更は、新しい値がノードのスキーマ型に照らして無効だったので、<xref:System.InvalidCastException> が発生しました。  
  
 <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> メソッドを使用して値を変更する方法の詳細については、「[XpathNavigator による XML データの変更](../../../../docs/standard/data/xml/modify-xml-data-using-xpathnavigator.md)」のトピックを参照してください。  
  
### <a name="read-only-validation"></a>読み取り専用の検証  
 <xref:System.Xml.XPath.XPathDocument> クラスは、XML ドキュメントの読み取り専用のメモリ内表現です。 <xref:System.Xml.XPath.XPathDocument> クラスと <xref:System.Xml.XmlDocument> クラスは両方とも、XML ドキュメントを編集しナビゲートするために <xref:System.Xml.XPath.XPathNavigator> オブジェクトを作成します。 <xref:System.Xml.XPath.XPathDocument> クラスは読み取り専用のクラスなので、<xref:System.Xml.XPath.XPathNavigator> オブジェクトから返された <xref:System.Xml.XPath.XPathDocument> オブジェクトは <xref:System.Xml.XPath.XPathDocument> オブジェクトに含まれる XML ドキュメントを編集できません。  
  
 このトピックで前述したように、検証の場合は、検証型 <xref:System.Xml.XPath.XPathDocument> オブジェクトを使用して <xref:System.Xml.XmlDocument> オブジェクトを作成したのと同様にして、<xref:System.Xml.XmlReader> オブジェクトを作成できます。 <xref:System.Xml.XPath.XPathDocument> オブジェクトは読み込みの際に XML ドキュメントを検証しますが、<xref:System.Xml.XPath.XPathDocument> オブジェクト内の XML データは編集できないので、XML ドキュメントの再検証はできません。  
  
 読み取り専用および編集可能な <xref:System.Xml.XPath.XPathNavigator> オブジェクトについては、「[XPathDocument および XmlDocument を使用した XML データの読み取り](../../../../docs/standard/data/xml/reading-xml-data-using-xpathdocument-and-xmldocument.md)」のトピックを参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [XPath データ モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-xpath-data-model.md)
- [XPathDocument および XmlDocument を使用した XML データの読み取り](../../../../docs/standard/data/xml/reading-xml-data-using-xpathdocument-and-xmldocument.md)
- [XPathNavigator を使用した XML データの選択、評価、および照合](../../../../docs/standard/data/xml/selecting-evaluating-and-matching-xml-data-using-xpathnavigator.md)
- [XPathNavigator による XML データへのアクセス](../../../../docs/standard/data/xml/accessing-xml-data-using-xpathnavigator.md)
- [XPathNavigator による XML データの編集](../../../../docs/standard/data/xml/editing-xml-data-using-xpathnavigator.md)
