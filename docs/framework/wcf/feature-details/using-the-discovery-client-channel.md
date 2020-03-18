---
title: 探索クライアント チャネルの使用
ms.date: 03/30/2017
ms.assetid: 1494242a-1d64-4035-8ecd-eb4f06c8d2ba
ms.openlocfilehash: 05ca54d62179d024e619bc5c9c70a4e08b9dd62f
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73975940"
---
# <a name="using-the-discovery-client-channel"></a>探索クライアント チャネルの使用
WCF クライアント アプリケーションを記述するときには、呼び出すサービスのエンドポイント アドレスを知っている必要があります。 多くの場合、サービスのエンドポイント アドレスがわからなかったり、サービスのアドレスが時間の経過と共に変化したりします。 探索クライアント チャネルでは、WCF クライアント アプリケーションを記述し、呼び出すサービスを示すと、クライアント チャネルが自動的にプローブ要求を送信します。 サービスが応答すると、探索クライアント チャネルは、プローブ応答からサービスのエンドポイント アドレスを受け取り、それを使用してサービスを呼び出します。  
  
## <a name="using-the-discovery-client-channel"></a>探索クライアント チャネルの使用  
 探索クライアント チャネルを使用するには、<xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> のインスタンスをクライアント チャネル スタックに追加します。 または、<xref:System.ServiceModel.Discovery.DynamicEndpoint> を使用すると、まだ追加されていない場合は、<xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> が自動的にバインディングに追加されます。  
  
> [!CAUTION]
> <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> は、クライアント チャネル スタックの最上位要素にすることをお勧めします。 <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> の上に追加されたバインディング要素では、<xref:System.ServiceModel.ChannelFactory>、またはそれによって作成されるチャネルが、エンドポイント アドレスまたは `Via` アドレス (`CreateChannel` メソッドに渡されたアドレス) を使用しないようにする必要があります。これらには、正しいアドレスが格納されていない可能性があります。  
  
 <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> クラスには、2 つのパブリック プロパティが含まれています。  
  
1. 呼び出すサービスを示すのに使用する <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement.FindCriteria%2A>。  
  
2. 探索メッセージの送信先の探索エンドポイントを指定する <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement.DiscoveryEndpointProvider%2A> ます。  
  
 <xref:System.ServiceModel.Discovery.FindCriteria.%23ctor%2A> プロパティでは、探しているサービス コントラクト、必要なスコープ URI、およびチャネルを開く最大試行回数を指定できます。 コントラクト型は <xref:System.ServiceModel.Discovery.FindCriteria>コンストラクターを呼び出すことによって指定されます。 スコープ URI は <xref:System.ServiceModel.Discovery.FindCriteria.Scopes%2A> プロパティに追加できます。 <xref:System.ServiceModel.Discovery.FindCriteria.MaxResults%2A> プロパティでは、クライアントが接続を試行する結果の最大数を指定できます。 プローブ応答を受信すると、クライアントは、プローブ応答で取得したエンドポイント アドレスを使用してチャネルを開きます。 例外が発生した場合、クライアントは次のプローブ応答に進み、必要に応じて、さらに応答の受信を待機します。 チャネルが正常に開かれるか、または結果の最大数に達するまで、この処理が続行されます。 これらの設定の詳細については、「<xref:System.ServiceModel.Discovery.FindCriteria>」を参照してください。  
  
 <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement.DiscoveryEndpointProvider%2A> プロパティでは、使用する探索エンドポイントを指定できます。 通常は <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> ですが、任意の有効なエンドポイントを指定できます。  
  
 サービスとの通信に使用するバインディングを作成するときには、サービスとまったく同じバインディングを使用するように注意する必要があります。 唯一の相違点は、クライアント バインディングの場合は、<xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> がスタックの一番上にあることです。 サービスが、システムによって提供されるバインディングの 1 つを使用している場合は、新しい <xref:System.ServiceModel.Channels.CustomBinding> を作成し、システムによって提供されるバインディングを <xref:System.ServiceModel.Channels.CustomBinding.%23ctor%2A> コンストラクターに渡します。 次に、<xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> を `Insert` プロパティで呼び出して、<xref:System.ServiceModel.Channels.CustomBinding.Elements%2A> を追加できます。  
  
 <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> をバインディングに追加し、構成したら、WCF クライアント クラスのインスタンスを作成し、それを開いて、そのメソッドを呼び出すことができます。 次の例では、探索クライアント チャネルを使用して、`ICalculator` クラス (WCF の入門チュートリアルで使用するクラス) を実装し、その `Add` メソッドを呼び出す、WCF サービスを探索します。  
  
```csharp
// Create the DiscoveryClientBindingElement  
DiscoveryClientBindingElement bindingElement = new DiscoveryClientBindingElement();  
// Search for a service that implements the ICalculator interface, attempting to open  
// the channel a maximum of 2 times  
bindingElement.FindCriteria = new FindCriteria(typeof(ICalculator)) { MaxResults = 2 };  
// Use the UdpDiscoveryEndpoint  
bindingElement.DiscoveryEndpoint = new UdpDiscoveryEndpoint();  
  
// The service uses the BasicHttpBinding, so use that and insert the DiscoveryClientBindingElement at the   
// top of the stack  
CustomBinding binding = new CustomBinding(new BasicHttpBinding());  
binding.Elements.Insert(0,bindingElement);  
  
try  
{  
    // Create the WCF client and call a method  
    CalculatorClient client = new CalculatorClient(binding, new EndpointAddress("http://schemas.microsoft.com/dynamic"));  
    client.Open();  
    client.Add(1, 1);  
}  
catch (EndpointNotFoundException ex)  
{  
    Console.WriteLine("An exception occurred: " + ex.Message);  
}  
```  
  
## <a name="security-and-the-discovery-client-channel"></a>セキュリティおよび探索クライアント チャネル  
 探索クライアント チャネルの使用時には、2 つのエンドポイントが指定されます。 1 つは探索メッセージ用に使用されるエンドポイント (通常は <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint>) で、もう 1 つはアプリケーション エンドポイントです。 セキュリティで保護されたサービスを実装するときには、両方のエンドポイントを保護するように注意する必要があります。 セキュリティの詳細については、「[サービスとクライアント](../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)のセキュリティ保護」を参照してください。
