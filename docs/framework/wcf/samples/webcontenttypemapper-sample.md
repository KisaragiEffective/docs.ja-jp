---
description: '詳細情報: WebContentTypeMapper サンプル'
title: WebContentTypeMapper のサンプル
ms.date: 03/30/2017
ms.assetid: a4fe59e7-44d8-43c6-a1f8-40c45223adca
ms.openlocfilehash: 274672bb8db1dc845b1cc1ced58b4c4a92232e9b
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99798148"
---
# <a name="webcontenttypemapper-sample"></a>WebContentTypeMapper のサンプル

このサンプルでは、新しいコンテンツタイプを Windows Communication Foundation (WCF) メッセージ本文形式にマップする方法を示します。  
  
 <xref:System.ServiceModel.Description.WebHttpEndpoint>要素は Web メッセージエンコーダーにプラグインします。これにより、WCF は、JSON、XML、または生のバイナリメッセージを同じエンドポイントで受信できます。 このエンコーダは、要求の HTTP コンテンツ タイプを調べて、メッセージ本文の書式を決定します。 このサンプルでは、コンテンツ タイプと本文書式との間の割り当てを制御するための <xref:System.ServiceModel.Channels.WebContentTypeMapper> クラスを示します。  
  
 WCF では、コンテンツの種類に対する一連の既定のマッピングが提供されます。 たとえば、`application/json` は JSON に割り当てられ、`text/xml` は XML に割り当てられています。 JSON または XML に割り当てられていないコンテンツ タイプは、生のバイナリ形式に割り当てられます。  
  
 場合によっては (プッシュ スタイルの API など)、クライアントによって返されるコンテンツ タイプがサービス開発者によって制御されないことがあります。 たとえば、クライアントは `text/javascript` としてではなく `application/json` として JSON を返す場合があります。 この場合、サービス開発者は、特定のコンテンツ タイプを正しく処理できるように、次のサンプル コードに示すような <xref:System.ServiceModel.Channels.WebContentTypeMapper> の派生型を指定する必要があります。  
  
```csharp  
public class JsonContentTypeMapper : WebContentTypeMapper  
{  
    public override WebContentFormat  
               GetMessageFormatForContentType(string contentType)  
    {  
        if (contentType == "text/javascript")  
        {  
            return WebContentFormat.Json;  
        }  
        else  
        {  
            return WebContentFormat.Default;  
        }  
    }  
}  
```  
  
 この派生型は、<xref:System.ServiceModel.Channels.WebContentTypeMapper.GetMessageFormatForContentType%28System.String%29> メソッドをオーバーライドする必要があります。 このメソッドは、`contentType` 引数を評価して、<xref:System.ServiceModel.Channels.WebContentFormat.Json>、<xref:System.ServiceModel.Channels.WebContentFormat.Xml>、<xref:System.ServiceModel.Channels.WebContentFormat.Raw>、または <xref:System.ServiceModel.Channels.WebContentFormat.Default> のいずれかの値を返す必要があります。 <xref:System.ServiceModel.Channels.WebContentFormat.Default> の戻り値は、Web メッセージ エンコーダの既定の割り当てによって決まります。 前のサンプル コードでは、`text/javascript` コンテンツ タイプが JSON に割り当てられ、その他すべての割り当ては変わりません。  
  
 `JsonContentTypeMapper` クラスを使用するには、Web.config 内で次のコードを使用します。  
  
```xml  
<system.serviceModel>  
  <standardEndpoints>  
    <webHttpEndpoint>  
      <standardEndpoint name="" contentTypeMapper="Microsoft.Samples.WebContentTypeMapper.JsonContentTypeMapper, JsonContentTypeMapper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />  
    </webHttpEndpoint>  
  </standardEndpoints>  
</system.serviceModel>  
```  
  
 JsonContentTypeMapper の使用要件を検証するには、Web.config ファイルから contentTypeMapper 属性を削除します。 `text/javascript` を使用して JSON コンテンツを送信しようとすると、クライアント ページの読み込みに失敗します。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. 「 [Windows Communication Foundation サンプルのビルド](building-the-samples.md)」の説明に従って、ソリューション WebContentTypeMapperSample をビルドします。  
  
3. に移動 `http://localhost/ServiceModelSamples/JCTMClientPage.htm` します (プロジェクトディレクトリ内からブラウザーで JCTMClientPage.htm を開かないでください)。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) とサンプルをダウンロードして [!INCLUDE[wf1](../../../../includes/wf1-md.md)] ください。 このサンプルは、次のディレクトリに格納されます。  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Ajax\WebContentTypeMapper`  
