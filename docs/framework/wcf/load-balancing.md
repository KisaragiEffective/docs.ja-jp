---
title: 負荷分散
ms.date: 03/30/2017
helpviewer_keywords:
- load balancing [WCF]
ms.assetid: 148e0168-c08d-4886-8769-776d0953b80f
ms.openlocfilehash: c9a1e889ab5adcb8f0eb5ea851c81a4f9ee56e95
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74138547"
---
# <a name="load-balancing"></a>負荷分散
Windows Communication Foundation (WCF) アプリケーションの容量を増やす方法の1つは、負荷分散されたサーバーファームに配置することによってスケールアウトすることです。 WCF アプリケーションは、Windows ネットワーク負荷分散やハードウェアベースの負荷分散アプライアンスなどのソフトウェアロードバランサーを含む、標準的な負荷分散手法を使用して負荷分散することができます。  
  
 以下のセクションでは、さまざまなシステム指定のバインディングを使用して構築された WCF アプリケーションの負荷分散に関する考慮事項について説明します。  
  
## <a name="load-balancing-with-the-basic-http-binding"></a>基本 HTTP バインディングによる負荷分散  
 負荷分散の観点からは、<xref:System.ServiceModel.BasicHttpBinding> を使用して通信する WCF アプリケーションは、他の一般的な種類の HTTP ネットワークトラフィック (静的な HTML コンテンツ、ASP.NET ページ、または ASMX Web サービス) とまったく同じではありません。 このバインディングを使用する WCF チャネルは本質的にステートレスであり、チャネルが閉じられると接続を終了します。 したがって、<xref:System.ServiceModel.BasicHttpBinding> には既存の HTTP 負荷分散の手法で十分に対応できます。  
  
 既定では、<xref:System.ServiceModel.BasicHttpBinding> は、メッセージの接続 HTTP ヘッダーで `Keep-Alive` 値を送信することで、サポートするサービスにクライアントが永続的な接続を確立できるようにします。 この構成では、以前に確立した接続を再使用して同じサーバーへの後続するメッセージを送信できるため、スループットが向上します。 ただし、接続を再使用すると、クライアントが負荷分散ファーム内の特定サーバーと強く関連付けられてしまうため、ラウンドロビン方式の負荷分散の効果を損ねることがあります。 これを回避するには、`Keep-Alive` またはユーザー定義の <xref:System.ServiceModel.Channels.HttpTransportBindingElement.KeepAliveEnabled%2A> で <xref:System.ServiceModel.Channels.CustomBinding> プロパティを使用して、サーバーの HTTP <xref:System.ServiceModel.Channels.Binding> を無効にできます。 構成を使用してこれを行う例を次に示します。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
  
 <system.serviceModel>  
  <services>  
   <service   
     name="Microsoft.ServiceModel.Samples.CalculatorService"  
     behaviorConfiguration="CalculatorServiceBehavior">  
     <host>  
      <baseAddresses>  
       <add baseAddress="http://localhost:8000/servicemodelsamples/service"/>  
      </baseAddresses>  
     </host>  
    <!-- configure http endpoint, use base address provided by host  
         And the customBinding -->  
     <endpoint address=""  
           binding="customBinding"  
           bindingConfiguration="HttpBinding"   
           contract="Microsoft.ServiceModel.Samples.ICalculator" />  
   </service>  
  </services>  
  
  <bindings>  
    <customBinding>  
  
    <!-- Configure a CustomBinding that disables keepAliveEnabled-->  
      <binding name="HttpBinding" keepAliveEnabled="False"/>  
  
    </customBinding>  
  </bindings>  
 </system.serviceModel>  
</configuration>  
```  
  
 .NET Framework 4 で導入された簡略化された構成を使用すると、次の簡略化された構成を使用して同じ動作を実現できます。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
 <system.serviceModel>  
    <protocolMapping>  
      <add scheme="http" binding="customBinding" />  
    </protocolMapping>  
    <bindings>  
      <customBinding>  
  
      <!-- Configure a CustomBinding that disables keepAliveEnabled-->  
        <binding keepAliveEnabled="False"/>  
  
      </customBinding>  
    </bindings>  
 </system.serviceModel>  
</configuration>  
```  
  
 既定のエンドポイントについては、「[Simplified Configuration](simplified-configuration.md)」 (簡易構成) と「[Simplified Configuration for WCF Services](./samples/simplified-configuration-for-wcf-services.md)」 (WCF サービスの簡易構成) を参照してください。  
  
## <a name="load-balancing-with-the-wshttp-binding-and-the-wsdualhttp-binding"></a>WSHttp バインディングおよび WSDualHttp バインディングによる負荷分散  
 <xref:System.ServiceModel.WSHttpBinding> と <xref:System.ServiceModel.WSDualHttpBinding> はどちらも、既定のバインド構成をいくつか変更すれば、HTTP の負荷分散手法を使用して負荷分散できます。  
  
- セキュリティ コンテキストの確立を無効にします。これは、<xref:System.ServiceModel.NonDualMessageSecurityOverHttp.EstablishSecurityContext%2A> の <xref:System.ServiceModel.WSHttpBinding> プロパティを `false` に設定することで実現されます。 また、セキュリティセッションが必要な場合は、「セキュリティ[で保護さ](./feature-details/secure-sessions.md)れたセッション」のトピックの説明に従って、ステートフルなセキュリティセッションを使用することもできます。 ステートフルなセキュリティ セッションは、セキュリティ セッションのすべての状態 (ステート) を保護セキュリティ トークンの一部として要求ごとに転送するため、サービスをステートレスな状態に保つことができます。 ステートフルなセキュリティ セッションを有効にする場合、システムによって提供される <xref:System.ServiceModel.Channels.CustomBinding> と <xref:System.ServiceModel.Channels.Binding> では、必要な構成設定が公開されないため、<xref:System.ServiceModel.WSHttpBinding> またはユーザー定義の <xref:System.ServiceModel.WSDualHttpBinding> を使用する必要があります。  
  
- 信頼できるセッションを使用しないでください。 この機能は既定で無効になっています。  
  
## <a name="load-balancing-the-nettcp-binding"></a>Net.TCP バインディングの負荷分散  
 <xref:System.ServiceModel.NetTcpBinding> は、IP レイヤー負荷分散の手法を使用して負荷分散できます。 ただし、<xref:System.ServiceModel.NetTcpBinding> は接続の待ち時間を減らすために、既定で TCP 接続のプールを作成します。 この最適化は、負荷分散の基本的なメカニズムに干渉します。 <xref:System.ServiceModel.NetTcpBinding> を最適化するための主な構成値はリース タイムアウトで、これは接続プール設定の一部です。 接続プールによって、クライアントの接続がファーム内の特定サーバーと関連付けられます。 このような接続の有効期間 (これはリース タイムアウトの設定で制御される要素です) が長くなるにつれて、ファーム内の各サーバーの負荷分散がうまくいかなくなります。 その結果、平均呼び出し時間が増加します。 したがって、<xref:System.ServiceModel.NetTcpBinding> を負荷分散シナリオで使用する場合は、バインディングによって使用される既定のリース タイムアウトを少なくすることを検討してください。 負荷分散のシナリオでは、30 秒のリース タイムアウトから始めるのが合理的ですが、最適値はアプリケーションによって異なります。 チャネルのリースタイムアウトおよびその他のトランスポートクォータの詳細については、「[トランスポートクォータ](./feature-details/transport-quotas.md)」を参照してください。  
  
 負荷分散のシナリオで最適なパフォーマンスを実現するには、<xref:System.ServiceModel.NetTcpSecurity> (<xref:System.ServiceModel.SecurityMode.Transport> または <xref:System.ServiceModel.SecurityMode.TransportWithMessageCredential>) を使用することを検討してください。  
  
## <a name="see-also"></a>関連項目

- [インターネット インフォメーション サービス ホスティングのベスト プラクティス](./feature-details/internet-information-services-hosting-best-practices.md)
