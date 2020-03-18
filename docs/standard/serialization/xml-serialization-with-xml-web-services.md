---
title: XML Web サービスを使用した XML シリアル化
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- XML Web services, XML serialization
- XML serialization, XML Web services
- SOAP, XML serialization
- asmx files
- serialization, SOAP
- XML serialization, attributes
- attributes [.NET Framework], XML serialization
- .asmx files
- encoded XML serialization
- literal XML serialization
- serialization, attributes
ms.assetid: a416192f-8102-458e-bc0a-0b8f3f784da9
ms.openlocfilehash: 2301f30a55e136b9a75a414d9325e4cf71c161da
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159534"
---
# <a name="xml-serialization-with-xml-web-services"></a>XML Web サービスを使用した XML シリアル化
XML シリアル化は、XML Web サービス アーキテクチャで使用される基礎的なトランスポート機構であり、<xref:System.Xml.Serialization.XmlSerializer> クラスによって実行されます。 XML Web サービスによって生成される XML を制御するには、「[XML シリアル化を制御する属性](../../../docs/standard/serialization/attributes-that-control-xml-serialization.md)」および「[エンコード済み SOAP シリアル化を制御する属性](../../../docs/standard/serialization/attributes-that-control-encoded-soap-serialization.md)」の一覧に示されている属性を、XML Web サービスの作成に使用するファイル (.asmx) のクラス、戻り値、パラメーター、およびフィールドに適用します。 XML Web サービスの作成の詳細については、「 [ASP.NET を使用した Xml Web サービス](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ba0z6a33(v=vs.100))」を参照してください。  
  
## <a name="literal-and-encoded-styles"></a>リテラル スタイルとエンコード済みスタイル  
 XML Web サービスによって生成される XML は、「 [SOAP メッセージの書式設定のカスタマイズ](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dkwy2d72(v=vs.100))」で説明されているように、リテラルまたはエンコード済みの2つの方法のいずれかで書式設定できます。 このため、XML シリアル化を制御する属性セットは 2 つになります。 「[XML シリアル化を制御する属性](../../../docs/standard/serialization/attributes-that-control-xml-serialization.md)」の一覧に示される属性は、リテラル スタイルの XML を制御するように設計されています。 一方、「[エンコード済み SOAP シリアル化を制御する属性](../../../docs/standard/serialization/attributes-that-control-encoded-soap-serialization.md)」の一覧に示される属性は、エンコード済みスタイルを制御します。 これらの属性を選択的に適用することで、2 つのスタイルのいずれかまたは両方を返すようにアプリケーションを調整できます。 さらに、これらの属性は、必要に応じて戻り値やパラメーターにも適用できます。  
  
### <a name="example-of-using-both-styles"></a>両方のスタイルの使用例  
 XML Web サービスを作成する場合、両方の属性セットをメソッドで使用できます。 次のコード例では、`MyService` という名前のクラスに、`MyLiteralMethod` と `MyEncodedMethod` という 2 つの XML Web サービス メソッドが含まれています。 いずれのメソッドも、`Order` クラスのインスタンスを返すという同じ機能を実行します。 `Order` クラスでは、<xref:System.Xml.Serialization.XmlTypeAttribute> 属性と <xref:System.Xml.Serialization.SoapTypeAttribute> 属性の両方が `OrderID` フィールドに適用され、両方の属性の `ElementName` プロパティには異なる値が設定されます。  
  
 この例を実行するには、拡張子 .asmx のファイルにコードを貼り付け、そのファイルをインターネット インフォメーション サービス (IIS) によって管理される仮想ディレクトリに配置して、 Internet Explorer などの HTML ブラウザーから、コンピューター名、仮想ディレクトリ名、およびファイル名を入力します。  
  
```vb  
<%@ WebService Language="VB" Class="MyService" %>  
Imports System  
Imports System.Web.Services  
Imports System.Web.Services.Protocols  
Imports System.Xml.Serialization  
Public Class Order  
    ' Both types of attributes can be applied. Depending on which type  
    ' the method used, either one will affect the call.  
    <SoapElement(ElementName:= "EncodedOrderID"), _  
    XmlElement(ElementName:= "LiteralOrderID")> _  
    public OrderID As String  
End Class  
  
Public Class MyService  
    <WebMethod, SoapDocumentMethod> _  
    public Function MyLiteralMethod() As Order
        Dim myOrder As Order = New Order()  
        return myOrder  
    End Function  
    <WebMethod, SoapRpcMethod> _  
    public Function MyEncodedMethod() As Order
        Dim myOrder As Order = New Order()  
        return myOrder  
    End Function  
End Class  
```  
  
```csharp  
<%@ WebService Language="C#" Class="MyService" %>  
using System;  
using System.Web.Services;  
using System.Web.Services.Protocols;  
using System.Xml.Serialization;  
public class Order {  
    // Both types of attributes can be applied. Depending on which type  
    // the method used, either one will affect the call.  
    [SoapElement(ElementName = "EncodedOrderID")]  
    [XmlElement(ElementName = "LiteralOrderID")]  
    public String OrderID;  
}  
public class MyService {  
    [WebMethod][SoapDocumentMethod]  
    public Order MyLiteralMethod(){  
        Order myOrder = new Order();  
        return myOrder;  
    }  
    [WebMethod][SoapRpcMethod]  
    public Order MyEncodedMethod(){  
        Order myOrder = new Order();  
        return myOrder;  
    }  
}  
```  
  
 `MyLiteralMethod` を呼び出すコード例を次に示します。 要素名は "LiteralOrderID" に変更されています。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
    <soap:Body>  
        <MyLiteralMethodResponse xmlns="http://tempuri.org/">  
            <MyLiteralMethodResult>  
                <LiteralOrderID>string</LiteralOrderID>  
            </MyLiteralMethodResult>  
        </MyLiteralMethodResponse>  
    </soap:Body>  
</soap:Envelope>  
```  
  
 `MyEncodedMethod` を呼び出すコード例を次に示します。 要素名は "EncodedOrderID" です。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soapenc="http://schemas.xmlsoap.org/soap/encoding/" xmlns:tns="http://tempuri.org/" xmlns:types="http://tempuri.org/encodedTypes" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
    <soap:Body soap:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">  
        <tns:MyEncodedMethodResponse>  
            <MyEncodedMethodResult href="#id1" />  
        </tns:MyEncodedMethodResponse>  
        <types:Order id="id1" xsi:type="types:Order">  
            <EncodedOrderID xsi:type="xsd:string">string</EncodedOrderID>  
        </types:Order>  
    </soap:Body>  
</soap:Envelope>  
```  
  
### <a name="applying-attributes-to-return-values"></a>戻り値への属性の適用  
 属性を戻り値に適用して、名前空間や要素名などを制御することもできます。 次のコード例では、`XmlElementAttribute` 属性を `MyLiteralMethod` メソッドの戻り値に適用しています。 このように属性を適用すると、名前空間や要素名を制御できます。  
  
```vb  
<WebMethod, SoapDocumentMethod> _  
public Function MyLiteralMethod() As _  
<XmlElement(Namespace:="http://www.cohowinery.com", _  
ElementName:= "BookOrder")> _  
Order
    Dim myOrder As Order = New Order()  
    return myOrder  
End Function  
```  
  
```csharp  
[return: XmlElement(Namespace = "http://www.cohowinery.com",  
ElementName = "BookOrder")]  
[WebMethod][SoapDocumentMethod]  
public Order MyLiteralMethod(){  
    Order myOrder = new Order();  
    return myOrder;  
}  
```  
  
 このコードを呼び出すと、次のような XML が返されます。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
    <soap:Body>  
        <MyLiteralMethodResponse xmlns="http://tempuri.org/">  
            <BookOrder xmlns="http://www.cohowinery.com">  
                <LiteralOrderID>string</LiteralOrderID>  
            </BookOrder>  
        </MyLiteralMethodResponse>  
    </soap:Body>  
</soap:Envelope>  
```  
  
### <a name="attributes-applied-to-parameters"></a>パラメーターに適用される属性  
 属性をパラメーターに適用して、名前空間や要素名などを指定することもできます。 次のコード例では、`MyLiteralMethodResponse` メソッドにパラメーターを追加し、そのパラメーターに `XmlAttributeAttribute` 属性を適用しています。 パラメーターに対して、要素名と名前空間の両方が設定されます。  
  
```vb  
<WebMethod, SoapDocumentMethod> _  
public Function MyLiteralMethod(<XmlElement _  
("MyOrderID", Namespace:="http://www.microsoft.com")>ID As String) As _  
<XmlElement(Namespace:="http://www.cohowinery.com", _  
ElementName:= "BookOrder")> _  
Order
    Dim myOrder As Order = New Order()  
    myOrder.OrderID = ID  
    return myOrder  
End Function  
```  
  
```csharp  
[return: XmlElement(Namespace = "http://www.cohowinery.com",  
ElementName = "BookOrder")]  
[WebMethod][SoapDocumentMethod]  
public Order MyLiteralMethod([XmlElement("MyOrderID",
Namespace="http://www.microsoft.com")] string ID){  
    Order myOrder = new Order();  
    myOrder.OrderID = ID;  
    return myOrder;  
}
```  
  
 SOAP 要求は、次のようになります。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
    <soap:Body>  
        <MyLiteralMethod xmlns="http://tempuri.org/">  
            <MyOrderID xmlns="http://www.microsoft.com">string</MyOrderID>  
        </MyLiteralMethod>  
    </soap:Body>  
</soap:Envelope>  
```  
  
### <a name="applying-attributes-to-classes"></a>クラスへの属性の適用  
 クラスと相関関係を持つ要素の名前空間を制御する必要がある場合は、必要に応じて `XmlTypeAttribute`、`XmlRootAttribute`、および `SoapTypeAttribute` を適用します。 次のコード例では、3 つの属性すべてを `Order` クラスに適用しています。  
  
```vb  
<XmlType("BigBookService"), _  
SoapType("SoapBookService"), _  
XmlRoot("BookOrderForm")> _  
Public Class Order  
    ' Both types of attributes can be applied. Depending on which  
    ' the method used, either one will affect the call.  
    <SoapElement(ElementName:= "EncodedOrderID"), _  
    XmlElement(ElementName:= "LiteralOrderID")> _  
    public OrderID As String  
End Class  
```  
  
```csharp  
[XmlType("BigBooksService", Namespace = "http://www.cpandl.com")]  
[SoapType("SoapBookService")]  
[XmlRoot("BookOrderForm")]  
public class Order {  
    // Both types of attributes can be applied. Depending on which  
    // the method used, either one will affect the call.  
    [SoapElement(ElementName = "EncodedOrderID")]  
    [XmlElement(ElementName = "LiteralOrderID")]  
    public String OrderID;  
}  
```  
  
 次のコード例に示すように、`XmlTypeAttribute` と `SoapTypeAttribute` を適用した結果は、サービスの説明を調べると確認できます。  
  
```xml
<s:element name="BookOrderForm" type="s0:BigBookService" />
<s:complexType name="BigBookService">
  <s:sequence>
    <s:element minOccurs="0" maxOccurs="1" name="LiteralOrderID" type="s:string" />
  </s:sequence>

  <s:schema targetNamespace="http://tempuri.org/encodedTypes">
    <s:complexType name="SoapBookService">
      <s:sequence>
        <s:element minOccurs="1" maxOccurs="1" name="EncodedOrderID" type="s:string" />
      </s:sequence>
    </s:complexType>
  </s:schema>
</s:complexType>
```
  
 また、`XmlRootAttribute` を適用した結果も、次に示す HTTP GET と HTTP POST の結果から確認できます。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<BookOrderForm xmlns="http://tempuri.org/">  
    <LiteralOrderID>string</LiteralOrderID>  
</BookOrderForm>  
```  
  
## <a name="see-also"></a>参照

- [XML シリアル化および SOAP シリアル化](xml-and-soap-serialization.md)
- [エンコード済み SOAP シリアル化を制御する属性](attributes-that-control-encoded-soap-serialization.md)
- [方法 : オブジェクトを SOAP エンコード済み XML ストリームとしてシリアル化する](how-to-serialize-an-object-as-a-soap-encoded-xml-stream.md)
- [方法 : SOAP エンコード済み XML シリアル化をオーバーライドする](how-to-override-encoded-soap-xml-serialization.md)
- [XML シリアル化の概要](introducing-xml-serialization.md)
- [方法 : オブジェクトをシリアル化する](how-to-serialize-an-object.md)
- [方法 : オブジェクトを逆シリアル化する](how-to-deserialize-an-object.md)
