---
title: System.Web.Routing 統合
ms.date: 03/30/2017
ms.assetid: 31fe2a4f-5c47-4e5d-8ee1-84c524609d41
ms.openlocfilehash: fdc355d4560294a16f3e9c488fdaf142d2982c0d
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745340"
---
# <a name="systemwebrouting-integration"></a>System.Web.Routing 統合
インターネットインフォメーションサービス (IIS) で Windows Communication Foundation (WCF) サービスをホストする場合は、.svc ファイルを仮想ディレクトリに配置します。 この .svc ファイルは、使用するサービス ホスト ファクトリと、サービスを実装するクラスを指定します。 サービスに要求を行うときは、URI に .svc ファイルを指定します (例: `http://contoso.com/EmployeeServce.svc`)。 REST サービスを記述するプログラマにとっては、この種類の URI は最適とは言えません。 REST サービス用の URI は、特定のリソースを指定しており、拡張子がないのが普通です。 <xref:System.Web.Routing> 統合機能を使用すると、拡張機能なしで Uri に応答する WCF REST サービスをホストできます。 ルーティングの詳細については、「 [ASP.NET routing](https://docs.microsoft.com/previous-versions/aspnet/cc668201(v=vs.100))」を参照してください。  
  
## <a name="using-systemwebrouting-integration"></a>System.Web.Routing 統合の使用  
 <xref:System.Web.Routing> 統合機能を使用するには、<xref:System.ServiceModel.Activation.ServiceRoute> クラスを使用して 1 つ以上のルートを作成し、Global.asax ファイルでそれらを <xref:System.Web.Routing.RouteTable> に追加します。 これらのルートは、サービスが応答する相対 URI を指定します。 次の例は、その方法を示したものです。  
  
```aspx-csharp  
<%@ Application Language="C#" %>  
<%@ Import Namespace="System.Web.Routing" %>  
<%@ Import Namespace="System.ServiceModel.Activation" %>  
<%@ Import Namespace="System.ServiceModel.Web " %>  
  
<script RunAt="server">  
    void Application_Start(object sender, EventArgs e)  
    {  
        RegisterRoutes(RouteTable.Routes);  
    }  
  
    private void RegisterRoutes(RouteCollection routes)  
    {  
        routes.Add(new ServiceRoute("Customers", new WebServiceHostFactory(), typeof(Service)));   
   }  
</script>  
```  
  
 これは、Customers で始まる相対 URI が指定されたすべての要求を `Service` サービスにルーティングします。  
  
 Web.config ファイルでは、次の例に示すように、`System.Web.Routing.UrlRoutingModule` モジュールを追加し、`runAllManagedModulesForAllRequests` 属性を `true` に設定し、`UrlRoutingHandler` ハンドラーを `<system.webServer>` 要素に追加する必要があります。  
  
```xml  
<system.webServer>  
      <modules runAllManagedModulesForAllRequests="true">  
        <add name="UrlRoutingModule" type="System.Web.Routing.UrlRoutingModule, System.Web, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" />  
      </modules>  
      <handlers>  
        <add name="UrlRoutingHandler" preCondition="integratedMode" verb="*" path="UrlRouting.axd"/>  
      </handlers>  
    </system.webServer>  
```  
  
 これで、ルーティングに必要なモジュールとハンドラーが読み込まれます。 詳細については、[ルーティング](../../../../docs/framework/wcf/feature-details/routing.md)に関するページを参照してください。 また、次の例に示すように、`aspNetCompatibilityEnabled` 要素で `true` 属性を `<serviceHostingEnvironment>` に設定する必要もあります。  
  
```xml  
<system.serviceModel>  
    <serviceHostingEnvironment aspNetCompatibilityEnabled="true"/>  
        <!-- ... -->  
    </system.serviceModel>  
```  
  
 次の例に示すように、このサービスを実装するクラスでは、ASP.NET 互換要件を有効にする必要があります。  
  
```csharp 
[ServiceContract]  
[AspNetCompatibilityRequirements(RequirementsMode=AspNetCompatibilityRequirementsMode.Allowed)]  
    public class Service  
    {  
        // ...  
    }  
```  
  
## <a name="see-also"></a>参照

- [WCF Web HTTP プログラミング モデル](../../../../docs/framework/wcf/feature-details/wcf-web-http-programming-model.md)
- [ASP.NET ルーティング](https://docs.microsoft.com/previous-versions/aspnet/cc668201(v=vs.100))
