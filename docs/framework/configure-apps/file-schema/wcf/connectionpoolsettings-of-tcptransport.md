---
description: '詳細情報: <tcpTransport> の <connectionPoolSettings>'
title: <connectionPoolSettings> の <tcpTransport>
ms.date: 03/30/2017
ms.assetid: 2fbc3aa7-fcc9-4193-99a3-85d31d60d3f7
ms.openlocfilehash: 065d3529740714ffd740c2cec71832a7b386b4a3
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99698389"
---
# <a name="connectionpoolsettings-of-tcptransport"></a>\<connectionPoolSettings> の \<tcpTransport>

TCP トランスポートの追加の接続プール設定を指定します。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<tcpTransport>**](tcptransport.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<connectionPoolSettings>**  
  
## <a name="syntax"></a>構文  
  
```xml  
<connectionPoolSettings groupName="String"
                        idleTimeout="TimeSpan"
                        leaseTimeout="TimeSpan"
                        maxOutboundConnectionsPerEndpoint="Integer" />
```  
  
## <a name="attributes-and-elements"></a>属性および要素  

 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|`groupName`|送信チャネルに使用される接続プールの名前を定義する文字列です。 ストリーム配信モードでは、接続が共有されません。したがって、接続プールは無効です。 既定は、"default" 文字列です。 この値を変更して、特定のクライアントの接続を、個別のグループに分離できます。|  
|`idleTimeout`|接続が切断されるまでの最大アイドル時間を指定する正の <xref:System.TimeSpan>。 既定値は 00:02:00 です。|  
|`leaseTimeout`|アクティブな接続が終了されるまでの時間を指定する <xref:System.TimeSpan>。 既定値は 00:05:00 です。<br /><br /> 接続は、接続キャッシュに返された後、アクティブに転送中ではないときに終了します。 TCP トランスポートによって使用される接続キャッシュは、各エンドポイントの必要に応じて、`maxOutboundConnectionsPerEndpoint.` で設定されているキャッシュ制限内で新しい接続を作成します。|  
|`maxOutboundConnectionsPerEndpoint`|そのサービスによって開始されるリモート エンドポイントへの接続の最大数を指定する正の整数。 制限を超えた接続は、制限内に空きができるまでキューに置かれます。 `idleTimeout` は、例外がスローされるまでに接続をキューに入れたままにする期間を制限します。 既定値は 10 です。<br /><br /> この属性は、クライアントから特定のサービス エンドポイントへの接続で、同時にアクティブできる接続数を制限します。 この値よりも多くのアクティブなクライアント接続がある場合、サービスは、クライアントに応答しないように見える可能性があります。 この場合は、この値を調整して、予想される特定のエンドポイントへの同時クライアント接続の最大数より大きくする必要があります。|  
  
### <a name="child-elements"></a>子要素  

 なし。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<namedPipeTransport>](namedpipetransport.md)|チャネルで名前付きパイプを使用してメッセージを転送するトランスポートを定義します。|  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Configuration.TcpConnectionPoolSettingsElement>
- <xref:System.ServiceModel.Channels.TcpTransportBindingElement.ConnectionPoolSettings%2A>
- <xref:System.ServiceModel.Channels.TcpConnectionPoolSettings>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [トランスポート](../../../wcf/feature-details/transports.md)
- [トランスポートの選択](../../../wcf/feature-details/choosing-a-transport.md)
- [バインド](../../../wcf/bindings.md)
- [バインディングの拡張](../../../wcf/extending/extending-bindings.md)
- [カスタム バインディング](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
