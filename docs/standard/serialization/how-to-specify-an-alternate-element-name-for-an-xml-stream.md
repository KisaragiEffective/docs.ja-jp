---
title: '方法 : XML ストリームの代替要素名を指定する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- XML serialization, overriding
- serialization, overriding
- XML stream, specifying alternate element name for
- overriding XML serialization
- classes, overriding
- overriding classes
ms.assetid: 5cc1c0b0-f94b-4525-9a41-88a582cd6668
ms.openlocfilehash: 6aaff20e2955fc9f121b3e60b14c0bbcf7515660
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159859"
---
# <a name="how-to-specify-an-alternate-element-name-for-an-xml-stream"></a>方法 : XML ストリームの代替要素名を指定する
  
<xref:System.Xml.Serialization.XmlSerializer> を使用して、クラスのセットが同じである XML ストリームを複数生成できます。 このような作業が必要になるのは、2 つの異なる XML Web サービスが、若干の違いのある同じ基本情報を要求するような場合です。 たとえば、書籍の注文を処理する 2 つの XML Web サービスがあり、どちらにも ISBN 番号が必要であるとします。 一方のサービスは \<ISBN> タグを使用しますが、もう一方は \<BookID> タグを使用します。 `Book` という名前のフィールドを含む、`ISBN` という名前のクラスがあります。 `Book` クラスのインスタンスがシリアル化されると、既定では、メンバー名 (ISBN) がタグ要素名として使用されます。 最初の XML Web サービスの場合、この既定どおりで問題ありません。 一方、2 つ目の XML Web サービスに XML ストリームを送信するには、タグの要素名が `BookID` になるようにシリアル化をオーバーライドする必要があります。  
  
## <a name="to-create-an-xml-stream-with-an-alternate-element-name"></a>代替要素名で XML ストリームを作成するには  
  
1. <xref:System.Xml.Serialization.XmlElementAttribute> クラスのインスタンスを作成します。  
  
2. <xref:System.Xml.Serialization.XmlElementAttribute.ElementName%2A> の <xref:System.Xml.Serialization.XmlElementAttribute> を "BookID" に設定します。  
  
3. <xref:System.Xml.Serialization.XmlAttributes> クラスのインスタンスを作成します。  
  
4. `XmlElementAttribute` オブジェクトを <xref:System.Xml.Serialization.XmlAttributes.XmlElements%2A> の <xref:System.Xml.Serialization.XmlAttributes> プロパティによってアクセスされるコレクションに追加します。  
  
5. <xref:System.Xml.Serialization.XmlAttributeOverrides> クラスのインスタンスを作成します。  
  
6. `XmlAttributes` を <xref:System.Xml.Serialization.XmlAttributeOverrides> に追加し、オーバーライドするオブジェクトの型とオーバーライドされるメンバーの名前を渡します。  
  
7. `XmlSerializer` を使用して、`XmlAttributeOverrides` クラスのインスタンスを作成します。  
  
8. `Book` クラスのインスタンスを作成し、シリアル化または逆シリアル化します。  
  
## <a name="example"></a>例  
  
```vb  
Public Class SerializeOverride()  
    ' Creates an XmlElementAttribute with the alternate name.  
    Dim myElementAttribute As XmlElementAttribute = _  
    New XmlElementAttribute()  
    myElementAttribute.ElementName = "BookID"  
    Dim myAttributes As XmlAttributes = New XmlAttributes()  
    myAttributes.XmlElements.Add(myElementAttribute)  
    Dim myOverrides As XmlAttributeOverrides = New XmlAttributeOverrides()  
    myOverrides.Add(typeof(Book), "ISBN", myAttributes)  
    Dim mySerializer As XmlSerializer = _  
    New XmlSerializer(GetType(Book), myOverrides)  
    Dim b As Book = New Book()  
    b.ISBN = "123456789"  
    ' Creates a StreamWriter to write the XML stream to.  
    Dim writer As StreamWriter = New StreamWriter("Book.xml")  
    mySerializer.Serialize(writer, b);  
End Class  
```  
  
```csharp  
public class SerializeOverride()  
{  
    // Creates an XmlElementAttribute with the alternate name.  
    XmlElementAttribute myElementAttribute = new XmlElementAttribute();  
    myElementAttribute.ElementName = "BookID";  
    XmlAttributes myAttributes = new XmlAttributes();  
    myAttributes.XmlElements.Add(myElementAttribute);  
    XmlAttributeOverrides myOverrides = new XmlAttributeOverrides();  
    myOverrides.Add(typeof(Book), "ISBN", myAttributes);  
    XmlSerializer mySerializer =
    new XmlSerializer(typeof(Book), myOverrides)  
    Book b = new Book();  
    b.ISBN = "123456789"  
    // Creates a StreamWriter to write the XML stream to.  
    StreamWriter writer = new StreamWriter("Book.xml");  
    mySerializer.Serialize(writer, b);  
}  
```  
  
 XML ストリームは、次のようになります。  
  
```xml  
<Book>  
    <BookID>123456789</BookID>  
</Book>  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Xml.Serialization.XmlElementAttribute>
- <xref:System.Xml.Serialization.XmlAttributes>
- <xref:System.Xml.Serialization.XmlAttributeOverrides>
- [XML シリアル化および SOAP シリアル化](../../../docs/standard/serialization/xml-and-soap-serialization.md)
- <xref:System.Xml.Serialization.XmlSerializer>
- [方法 : オブジェクトをシリアル化する](../../../docs/standard/serialization/how-to-serialize-an-object.md)
- [方法 : オブジェクトを逆シリアル化する](../../../docs/standard/serialization/how-to-deserialize-an-object.md)
