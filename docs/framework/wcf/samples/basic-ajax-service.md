---
title: 基本的な AJAX サービス
ms.date: 03/30/2017
ms.assetid: d66d0c91-0109-45a0-a901-f3e4667c2465
ms.openlocfilehash: 334cc9e53d7d9746c204abe37e7c30d00baa824b
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74716118"
---
# <a name="basic-ajax-service"></a>基本的な AJAX サービス

このサンプルでは、Windows Communication Foundation (WCF) を使用して、基本的な ASP.NET 非同期 JavaScript and XML (AJAX) サービス (Web ブラウザークライアントから JavaScript コードを使用してアクセスできるサービス) を作成する方法を示します。 このサービスは、<xref:System.ServiceModel.Web.WebGetAttribute> 属性を使用してサービスが HTTP GET 要求に応答し、JSON (JavaScript Object Notation) データ形式を使用して応答するように構成されていることを確認します。

WCF での AJAX サポートは、`ScriptManager` コントロールを介して ASP.NET AJAX で使用できるように最適化されています。 ASP.NET AJAX で WCF を使用する例については、 [ajax のサンプル](ajax.md)を参照してください。

> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。

次のコードでは、サービスが確実に HTTP GET 要求に応答するように <xref:System.ServiceModel.Web.WebGetAttribute> 属性が `Add` 操作に適用されています。 このコードでは、簡単にするために GET を使用します (すべての Web ブラウザーから HTTP GET 要求を構築できます)。 また、GET を使用してキャッシングを有効にすることもできます。 HTTP POST は `WebGetAttribute` 属性を使用しない場合の既定です。

```csharp
[ServiceContract(Namespace = "SimpleAjaxService")]
public interface ICalculator
{
    [WebGet]
    double Add(double n1, double n2);
    //Other operations omitted…
}
```

サンプルの .svc ファイルでは <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory> が使用され、それによって <xref:System.ServiceModel.Description.WebScriptEndpoint> 標準エンドポイントがサービスに追加されます。 エンドポイントは、.svc ファイルに相対する空のアドレス位置に設定されます。 これは、サービスのアドレスが `http://localhost/ServiceModelSamples/service.svc`、操作名以外の追加のサフィックスがないことを意味します。

`<%@ServiceHost language="C#" Debug="true" Service="Microsoft.Samples.SimpleAjaxService.CalculatorService" Factory="System.ServiceModel.Activation.WebScriptServiceHostFactory" %>`

<xref:System.ServiceModel.Description.WebScriptEndpoint> は、ASP.NET AJAX クライアント ページからサービスにアクセスできるように事前に構成されています。 Web.config 内の次のセクションを使用して、エンドポイントに対する構成変更を追加できます。 追加の変更が不要な場合は、このセクションを削除できます。

```xml
<system.serviceModel>
  <standardEndpoints>
    <webScriptEndpoint>
      <!-- Use this element to configure the endpoint -->
      <standardEndpoint name=""  />
    </webScriptEndpoint>
  </standardEndpoints>
</system.serviceModel>
```

<xref:System.ServiceModel.Description.WebScriptEndpoint> は、サービスの既定のデータ形式を XML ではなく JSON に設定します。 サービスを呼び出すには、このトピックで後述するセットアップとビルドの手順を完了した後に `http://localhost/ServiceModelSamples/service.svc/Add?n1=100&n2=200` に移動します。 このテスト機能は、HTTP GET 要求を使用することによって有効になります。

クライアントの Web ページの SimpleAjaxClientPage.aspx には、ユーザーがページ上のいずれかの操作ボタンをクリックするとサービスを呼び出す ASP.NET コードが含まれています。 `ScriptManager` コントロールは、JavaScript を使用してサービスからプロキシにアクセスできるようにする場合に使用します。

```aspx-csharp
<asp:ScriptManager ID="ScriptManager" runat="server">
    <Services>
        <asp:ServiceReference Path="service.svc" />
    </Services>
</asp:ScriptManager>
```

次の JavaScript コードを使用してローカル プロキシをインスタンス化し、操作を呼び出します。

```javascript
// Code for extracting arguments n1 and n2 omitted…
// Instantiate a service proxy
var proxy = new SimpleAjaxService.ICalculator();
// Code for selecting operation omitted…
proxy.Add(parseFloat(n1), parseFloat(n2), onSuccess, onFail, null);
```

サービス呼び出しが成功すると、コードは `onSuccess` ハンドラーを呼び出し、操作の結果がテキスト ボックスに表示されます。

```javascript
function onSuccess(mathResult){
     document.getElementById("result").value = mathResult;
}
```

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Ajax\SimpleAjaxService`
