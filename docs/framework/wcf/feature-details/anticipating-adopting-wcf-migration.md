---
description: '詳細情報: Windows Communication Foundation 導入の準備:将来の移行の簡略化'
title: Windows Communication Foundation 導入の準備:将来の移行の簡略化
ms.date: 03/30/2017
ms.assetid: f49664d9-e9e0-425c-a259-93f0a569d01b
ms.openlocfilehash: 6ea81b1dd01ed45ff62fa50c1b17442f88fc05af
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643814"
---
# <a name="anticipating-adopting-the-windows-communication-foundation-easing-future-migration"></a>Windows Communication Foundation 導入の準備:将来の移行の簡略化

将来、新しい ASP.NET アプリケーションを WCF へ簡単に移行できるようにするには、前述の推奨事項および次の推奨事項に従います。  
  
## <a name="protocols"></a>プロトコル  

 ASP.NET 2.0 の SOAP 1.2 サポートを無効にします。  
  
```xml  
<configuration>  
     <system.web>  
      <webServices >  
          <protocols>  
           <remove name="HttpSoap12"/>  
          </protocols>
      </webServices>  
     </system.web>
</configuration>  
```  
  
 このような処理を行うのは、WCF ではさまざまなエンドポイントを介してメッセージを交換するため、SOAP 1.1 や SOAP 1.2 など各種プロトコルに対応する必要があるためです。 ASP.NET 2.0 Web サービスが SOAP 1.1 と SOAP 1.2 を両方ともサポートするように構成されている場合 (既定の構成)、その ASP.NET Web サービスの既存の全クライアントと確実に互換性のある、元のアドレスの単一の WCF エンドポイントに移行することはできません。 また、SOAP 1.1 ではなく 1.2 を選択すると、サービスの利用者がさらに厳しく制限されます。  
  
## <a name="service-development"></a>サービスの開発  

 WCF では、<xref:System.ServiceModel.ServiceContractAttribute> をインターフェイスまたはクラスのいずれかに適用することでサービス コントラクトを定義できます。 この属性は、クラスではなくインターフェイスに適用することをお勧めします。これにより、任意の数のクラスでさまざまに実装できるコントラクト定義が作成されます。 ASP.NET 2.0 では、<xref:System.Web.Services.WebService> 属性をクラスだけでなくインターフェイスに適用することもできます。 ただし、既に説明したように ASP.NET 2.0 には不具合があり、<xref:System.Web.Services.WebService> 属性をクラスではなくインターフェイスに適用した場合、この属性の名前空間パラメーターが有効化されません。 一般に、<xref:System.Web.Services.WebService> 属性の名前空間パラメーターを使用して、サービスの名前空間を既定値の `http://tempuri.org` から変更することが望ましので、引き続き <xref:System.ServiceModel.ServiceContractAttribute> 属性をインターフェイスまたはクラスに適用して ASP.NET Web サービスを定義する必要があります。  
  
- これらのインターフェイスを定義するメソッドに含めるコードは、できるだけ少なくします。 これらのメソッドの作業を他のクラスに委任します。 その際、新しい WCF サービスの型も、実質的な作業をそのクラスに委任できます。  
  
- `MessageName` の <xref:System.Web.Services.WebMethodAttribute> パラメーターを使用して、サービスの動作の明示的な名前を指定します。  
  
    ```csharp  
    [WebMethod(MessageName="ExplicitName")]  
    string Echo(string input);  
    ```  
  
     ASP.NET と WCF では既定の動作名が異なるので、この処理は重要です。 明示的な名前を指定することで、既定の名前への依存を避けることができます。  
  
- ASP.NET Web サービスの動作を多様メソッドで実装しないでください。WCF では、多様メソッドを使用した動作の実装がサポートされていません。  
  
- <xref:System.Web.Services.Protocols.SoapDocumentMethodAttribute> を使用して、HTTP 要求をメソッドにルーティングする SOAPAction HTTP ヘッダーの明示的な値を指定します。  
  
    ```csharp  
    [WebMethod]  
    [SoapDocumentMethod(RequestElementName="ExplicitAction")]  
    string Echo(string input);  
    ```  
  
     この方法により、ASP.NET と WCF で SOAPAction の同じ既定値を使用する必要がなくなります。  
  
- SOAP 拡張機能は使用しないでください。 SOAP 拡張機能が必要な場合は、それを検討する目的の機能が WCF で既に提供されていないかどうかを確認します。 目的の機能が備わっている場合は、"すぐには WCF を導入しない" という方針を考え直してください。  
  
## <a name="state-management"></a>状態管理  

 サービスで状態を維持する必要がないようにします。 状態を維持すると、アプリケーションのスケーラビリティが損なわれるだけではありません。WCF の ASP.NET 互換モードを使用すれば ASP.NET のメカニズムに対応できるとはいえ、ASP.NET と WCF とでは状態管理メカニズムが大きく異なります。  
  
## <a name="exception-handling"></a>例外処理  

 サービスで送受信するデータ型の構造を設計するときは、クライアントに伝達する必要があり、サービス内で発生する可能性のあるさまざまな種類の例外を表現する構造も設計します。  
  
```csharp  
[Serializable]  
[XmlRoot(Namespace="ExplicitNamespace", IsNullable=true)]  
public partial class AnticipatedException
{
    private string anticipatedExceptionInformationField;  

    public string AnticipatedExceptionInformation
    {  
        get {
            return this.anticipatedExceptionInformationField;  
        }  
        set {  
            this.anticipatedExceptionInformationField = value;  
        }  
    }  
}  
```  
  
 これらのクラスには、自分自身を XML にシリアル化する機能を与えます。  
  
```csharp  
public XmlNode ToXML()  
{  
     XmlSerializer serializer = new XmlSerializer(  
      typeof(AnticipatedException));  
     MemoryStream memoryStream = new MemoryStream();  
     XmlTextWriter writer = new XmlTextWriter(  
     memoryStream, UnicodeEncoding.UTF8);  
     serializer.Serialize(writer, this);  
     XmlDocument document = new XmlDocument();  
     document.LoadXml(new string(  
     UnicodeEncoding.UTF8.GetChars(  
     memoryStream.GetBuffer())).Trim());  
    return document.DocumentElement;  
}  
```  
  
 これでこのクラスを使用して、明示的にスローされた <xref:System.Web.Services.Protocols.SoapException> インスタンスの詳細を指定できます。  
  
```csharp  
AnticipatedException exception = new AnticipatedException();  
exception.AnticipatedExceptionInformation = "…";  
throw new SoapException(  
     "Fault occurred",  
     SoapException.ClientFaultCode,  
     Context.Request.Url.AbsoluteUri,  
     exception.ToXML());  
```  
  
 この例外クラスは、新しい `FaultException<AnticipatedException>(anticipatedException);` をスローするのに、WCF <xref:System.ServiceModel.FaultException%601> クラスと共に簡単に再利用できます  
  
## <a name="security"></a>セキュリティ  

 セキュリティに関する推奨事項を次にいくつか示します。  
  
- ASP.NET 2.0 のプロファイルは使用しないでください。サービスを WCF へ移行した場合、ASP.NET 統合モードの使用が制限されます。  
  
- サービスへのアクセスを制御する目的で ACL を使用しないでください。ASP.NET Web サービスはインターネット インフォメーション サービス (IIS) を使用して ACL を サポートしますが、WCF はサポートしません。これは、ASP.NET Web サービスはホスティングを IIS に依存しますが、WCF は IIS でホストされる必要がないためです。  
  
- サービスのリソースへのアクセスを承認するには、ASP.NET 2.0 ロール プロバイダーの使用を検討してください。  
  
## <a name="see-also"></a>関連項目

- [Windows Communication Foundation 導入の準備:将来的な統合の容易化](anticipating-adopting-the-wcf-easing-future-integration.md)
