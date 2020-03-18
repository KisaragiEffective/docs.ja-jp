---
title: メッセージ レベルのプログラミングによる JSON 形式でのシリアル化
ms.date: 03/30/2017
ms.assetid: 5f940ba2-57ee-4c49-a779-957c5e7e71fa
ms.openlocfilehash: af6c2d726b03fe82f5447bdec25944149b966f2a
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75938067"
---
# <a name="serializing-in-json-with-message-level-programming"></a>メッセージ レベルのプログラミングによる JSON 形式でのシリアル化
WCF は、JSON 形式でのデータのシリアル化をサポートします。 このトピックでは、<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> を使用して型をシリアル化することを WCF に命令する方法について説明します。  
  
## <a name="typed-message-programming"></a>型指定されたメッセージのプログラミング  
 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> は、<xref:System.ServiceModel.Web.WebGetAttribute> または <xref:System.ServiceModel.Web.WebInvokeAttribute> がサービス操作に適用されるときに使用されます。 両方の属性で `RequestFormat` と `ResponseFormat` を指定できます。 要求と応答に JSON を使用する場合。 これらの両方を `WebMessageFormat.Json`に設定します。  JSON を使用するには、<xref:System.ServiceModel.WebHttpBinding>を使用する必要があります。この場合、<xref:System.ServiceModel.Description.WebHttpBehavior>が自動的に構成されます。 WCF シリアル化の詳細については、「[シリアル化と逆シリアル](../../../../docs/framework/wcf/feature-details/serialization-and-deserialization.md)化」を参照してください。 JSON と WCF の詳細については、「[サービスステーション-wcf を使用した RESTful サービスの概要](https://docs.microsoft.com/archive/msdn-magazine/2009/january/service-station-an-introduction-to-restful-services-with-wcf)」を参照してください。  
  
> [!IMPORTANT]
> JSON を使用するには、SOAP 通信をサポートしていない <xref:System.ServiceModel.WebHttpBinding> と <xref:System.ServiceModel.Description.WebHttpBehavior> を使用する必要があります。 <xref:System.ServiceModel.WebHttpBinding> と通信するサービスは、サービスメタデータの公開をサポートしていないため、クライアント側プロキシを生成するために Visual Studio のサービス参照の追加機能または svcutil.exe コマンドラインツールを使用することはできません。 <xref:System.ServiceModel.WebHttpBinding>を使用するサービスをプログラムで呼び出す方法の詳細については、「 [WCF で REST サービスを使用する方法](https://docs.microsoft.com/archive/blogs/pedram/how-to-consume-rest-services-with-wcf)」を参照してください。  
  
## <a name="untyped-message-programming"></a>型指定されていないメッセージのプログラミング  
 型指定されていないメッセージ オブジェクトを直接操作する場合は、型指定されていないメッセージのプロパティを明示的に設定して JSON としてシリアル化する必要があります。 これを行う方法を次のコード スニペットに示します。  
  
```csharp
 Message response = Message.CreateMessage(  
                  MessageVersion.None,    // No SOAP message version  
                             "*",                     // SOAP action, ignored since this is JSON  
                             "Response string: JSON format specified", // Message body  
                             new DataContractJsonSerializer(typeof(string))); // Specify DataContractJsonSerializer  
      response.Properties.Add( WebBodyFormatMessageProperty.Name,   
                    new WebBodyFormatMessageProperty(WebContentFormat.Json)); // Use JSON format  
```  
  
## <a name="see-also"></a>関連項目

- [AJAX の統合と JSON のサポート](../../../../docs/framework/wcf/feature-details/ajax-integration-and-json-support.md)
- [スタンドアロン JSON のシリアル化](../../../../docs/framework/wcf/feature-details/stand-alone-json-serialization.md)
- [JSON シリアル化](../../../../docs/framework/wcf/samples/json-serialization.md)
