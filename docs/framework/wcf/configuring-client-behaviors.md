---
title: クライアントの動作の構成
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: df5b32fa-e73b-4e8e-b66f-357c748e0173
ms.openlocfilehash: ca466af71f62ef72e021753b132afdc847f75d76
ms.sourcegitcommit: 628e8147ca10187488e6407dab4c4e6ebe0cac47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72320689"
---
# <a name="configuring-client-behaviors"></a>クライアントの動作の構成
Windows Communication Foundation (WCF) では、2つの方法で動作を構成します。動作構成は、クライアントアプリケーション構成ファイルの `<behavior>` セクションで定義するか、呼び出し元アプリケーションでプログラムによって定義されます。 このトピックでは、両方の方法について説明します。  
  
 構成ファイルを使用する場合、動作の構成には、構成設定の名前付きコレクションがあります。 各動作の構成には、一意の名前を指定する必要があります。 この文字列をエンドポイントの構成の `behaviorConfiguration` 属性で使用し、エンドポイントと動作を関連付けます。  
  
## <a name="example"></a>例  
 `myBehavior` という動作を定義する構成コードを次に示します。 クライアント エンドポイントは、この動作を `behaviorConfiguration` 属性で参照します。  
  
```xml  
<configuration>  
    <system.serviceModel>  
        <behaviors>  
            <endpointBehaviors>  
                <behavior name="myBehavior">  
                    <clientVia />  
                </behavior>  
            </endpointBehaviors>  
        </behaviors>  
        <bindings>  
            <basicHttpBinding>  
                <binding name="myBinding" maxReceivedMessageSize="10000" />  
            </basicHttpBinding>  
        </bindings>  
        <client>  
            <endpoint address="myAddress" binding="basicHttpBinding" bindingConfiguration="myBinding" behaviorConfiguration="myBehavior" contract="myContract" />  
        </client>  
    </system.serviceModel>  
</configuration>  
```  
  
## <a name="using-behaviors-programmatically"></a>プログラムによる動作の使用  
 また、クライアントを開く前に、Windows Communication Foundation (WCF) クライアントオブジェクトまたはクライアントチャネルファクトリオブジェクトで適切な `Behaviors` プロパティを見つけることで、動作をプログラムによって構成または挿入することもできます。  
  
## <a name="example"></a>例  
 次のコード例は、チャネル オブジェクトの作成前に、<xref:System.ServiceModel.Description.ServiceEndpoint.Behaviors%2A> プロパティから返される <xref:System.ServiceModel.Description.ServiceEndpoint> 上の <xref:System.ServiceModel.ChannelFactory.Endpoint%2A> プロパティにアクセスすることで、プログラムでクライアント動作が挿入される方法を示します。  
  
 [!code-csharp[ChannelFactoryBehaviors#10](../../../samples/snippets/csharp/VS_Snippets_CFX/channelfactorybehaviors/cs/client.cs#10)]
 [!code-vb[ChannelFactoryBehaviors#10](../../../samples/snippets/visualbasic/VS_Snippets_CFX/channelfactorybehaviors/vb/client.vb#10)]  
  
## <a name="see-also"></a>参照

- [\<behaviors>](../configure-apps/file-schema/wcf/behaviors.md)
