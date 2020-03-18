---
title: JSON および XML 形式の AJAX サービスのサンプル
ms.date: 03/30/2017
ms.assetid: 8ea5860d-0c42-4ae9-941a-e07efdd8e29c
ms.openlocfilehash: ca9bdbfa135ac7dc0b69589d4f8fce07bc4c4afe
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74716214"
---
# <a name="ajax-service-with-json-and-xml-sample"></a>JSON および XML 形式の AJAX サービスのサンプル

このサンプルでは、Windows Communication Foundation (WCF) を使用して、JavaScript Object Notation (JSON) データまたは XML データを返す非同期 JavaScript and XML (AJAX) サービスを作成する方法を示します。 AJAX サービスには、Web ブラウザー クライアントから JavaScript コードを使用してアクセスできます。 このサンプルは、[基本的な AJAX サービス](../../../../docs/framework/wcf/samples/basic-ajax-service.md)のサンプルに基づいています。

他の AJAX サンプルとは異なり、このサンプルでは ASP.NET AJAX および <xref:System.Web.UI.ScriptManager> コントロールを使用しません。 いくつかの追加構成では、JavaScript を通じて任意の HTML ページから WCF AJAX サービスにアクセスできます。このシナリオを次に示します。 ASP.NET AJAX で WCF を使用する例については、「 [ajax のサンプル](ajax.md)」を参照してください。

このサンプルでは、JSON と XML 間で操作の応答のタイプを切り替える方法を示します。 この機能は、サービスが ASP.NET AJAX または HTML/JavaScript クライアント ページでアクセスできるように構成されているかどうかにかかわらず使用できます。

> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。

ASP.NET AJAX 以外のクライアントを使用するには、.svc ファイルで <xref:System.ServiceModel.Activation.WebServiceHostFactory> (<xref:System.ServiceModel.Activation.WebScriptServiceHostFactory> ではありません) を使用します。 <xref:System.ServiceModel.Activation.WebServiceHostFactory> によって、<xref:System.ServiceModel.Description.WebHttpEndpoint> 標準エンドポイントがサービスに追加されます。 エンドポイントは、.svc ファイルを基準とした空のアドレスで構成されます。これは、サービスのアドレスが `http://localhost/ServiceModelSamples/service.svc`、操作名以外の追加のサフィックスがないことを意味します。

`<%@ServiceHost language="c#" Debug="true" Service="Microsoft.Samples.XmlAjaxService.CalculatorService" Factory="System.ServiceModel.Activation.WebServiceHostFactory" %>`

Web.config 内の次のセクションを使用して、エンドポイントに対する構成変更を追加できます。 追加の変更が不要な場合は、このセクションを削除できます。

```xml
<system.serviceModel>
  <standardEndpoints>
    <webHttpEndpoint>
      <!-- Use this element to configure the endpoint -->
      <standardEndpoint name="" />
    </webHttpEndpoint>
  </standardEndpoints>
</system.serviceModel>
```

<xref:System.ServiceModel.Description.WebHttpEndpoint> の既定のデータ形式は XML ですが、<xref:System.ServiceModel.Description.WebScriptEndpoint> の既定のデータ形式は JSON です。 詳細については、「 [ASP.NET を使用せずに WCF AJAX サービスを作成する](../../../../docs/framework/wcf/feature-details/creating-wcf-ajax-services-without-aspnet.md)」を参照してください。

次のサンプルのサービスは、2つの操作を持つ標準の WCF サービスです。 どちらの操作でも <xref:System.ServiceModel.Web.WebMessageBodyStyle.Wrapped> または <xref:System.ServiceModel.Web.WebGetAttribute> 属性に <xref:System.ServiceModel.Web.WebInvokeAttribute> の本文スタイルが必要です。この本文スタイルは、`webHttp` 動作に固有で、JSON/XML データ形式の切り替えに影響しません。

```csharp
[OperationContract]
[WebInvoke(ResponseFormat = WebMessageFormat.Xml, BodyStyle = WebMessageBodyStyle.Wrapped)]
MathResult DoMathXml(double n1, double n2);
```

操作の応答形式は XML として指定されます。これは[\<webHttp >](../../../../docs/framework/configure-apps/file-schema/wcf/webhttp.md)動作の既定の設定です。 ただし、応答形式を明示的に指定することをお勧めします。

その他の操作では `WebInvokeAttribute` 属性を使用し、応答に XML ではなく JSON を明示的に指定します。

```csharp
[OperationContract]
[WebInvoke(ResponseFormat = WebMessageFormat.Json, BodyStyle = WebMessageBodyStyle.Wrapped)]
MathResult DoMathJson(double n1, double n2);
```

どちらの場合も、操作は、標準の WCF データコントラクト型である複合型 `MathResult`を返します。

クライアント Web ページ Xmlajaxclientpage.htm には、ユーザーが [**計算の実行] (JSON を返す)** または [**計算の実行] (XML を返す)** ボタンをクリックしたときに、上記の2つの操作のいずれかを呼び出す JavaScript コードが含まれています。 サービスを呼び出すコードによって JSON 本文が作成され、HTTP POST を使用して送信されます。 [基本的な Ajax サービス](../../../../docs/framework/wcf/samples/basic-ajax-service.md)サンプルと ASP.NET ajax を使用したその他のサンプルとは異なり、要求は JavaScript で手動で作成されます。

```csharp
// Create HTTP request
var xmlHttp;
// Request instantiation code omitted…
// Result handler code omitted…

// Build the operation URL
var url = "service.svc/ajaxEndpoint/";
url = url + operation;

// Build the body of the JSON message
var body = '{"n1":';
body = body + document.getElementById("num1").value + ',"n2":';
body = body + document.getElementById("num2").value + '}';

// Send the HTTP request
xmlHttp.open("POST", url, true);
xmlHttp.setRequestHeader("Content-type", "application/json");
xmlHttp.send(body);
```

サービスが応答すると、応答はこれ以上の処理を行わずにページ上のテキスト ボックスに表示されます。 これは、使用される XML および JSON データ形式を直接確認できるように、デモンストレーションの目的で実装されています。

```javascript
// Create result handler
xmlHttp.onreadystatechange=function(){
     if(xmlHttp.readyState == 4){
          document.getElementById("result").value = xmlHttp.responseText;
     }
}
```

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\AJAX\XmlAjaxService`

#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

2. 「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の説明に従って、ソリューション XmlAjaxService をビルドします。

3. `http://localhost/ServiceModelSamples/XmlAjaxClientPage.htm` に移動します (プロジェクトディレクトリからブラウザーで Xmlajaxclientpage.htm を開かないでください)。

## <a name="see-also"></a>参照

- [HTTP POST を使用する AJAX サービス](../../../../docs/framework/wcf/samples/ajax-service-using-http-post.md)
