---
title: 基本的なサンプル
ms.date: 03/30/2017
ms.assetid: c1910bc1-3d0a-4fa6-b12a-4ed6fe759620
ms.openlocfilehash: 2ea5af0a1c05b5632632b2619c0ee4813696d2fc
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76738171"
---
# <a name="basic-sample"></a>基本的なサンプル

このサンプルでは、サービスを探索可能にする方法と、探索可能なサービスの検索方法および呼び出し方法を示します。 このサンプルは、2 つのプロジェクト (サービスとクライアント) で構成されます。

> [!NOTE]
> このサンプルでは、探索をコードで実装しています。  構成で探索を実装するサンプルについては、「[構成](../../../../docs/framework/wcf/samples/configuration-sample.md)」を参照してください。

## <a name="service"></a>サービス

これは簡単な電卓サービスの実装です。 探索関連のコードは `Main` にあります。ここでは、次のコードに示すように、 <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> がサービス ホストに追加され、<xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> が追加されます。

```csharp
using (ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService), baseAddress))
{
    serviceHost.AddServiceEndpoint(typeof(ICalculatorService), new
      WSHttpBinding(), String.Empty);

    // Make the service discoverable over UDP multicast
    serviceHost.Description.Behaviors.Add(new ServiceDiscoveryBehavior());
    serviceHost.AddServiceEndpoint(new UdpDiscoveryEndpoint());

    serviceHost.Open();
    // ...
}
```

## <a name="client"></a>Client

クライアントは、<xref:System.ServiceModel.Discovery.DynamicEndpoint> を使用してサービスを検索します。 <xref:System.ServiceModel.Discovery.DynamicEndpoint> は標準エンドポイントで、クライアントが開いたときにサービスのエンドポイントを解決します。 この場合、<xref:System.ServiceModel.Discovery.DynamicEndpoint> は、サービス コントラクトに基づいてサービスを検索します。 <xref:System.ServiceModel.Discovery.DynamicEndpoint> は、既定で <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> を検索します。 サービス エンドポイントが見つかると、クライアントは指定されたバインディングを介してそのサービスに接続します。

```csharp
public static void Main()
{
   DynamicEndpoint dynamicEndpoint = new DynamicEndpoint( ContractDescription.GetContract(typeof(ICalculatorService)), new WSHttpBinding());
   // ...
}
```

クライアントは、`InvokeCalculatorService` クラスを使用してサービスを検索する <xref:System.ServiceModel.Discovery.DiscoveryClient> という名前のメソッドを定義します。 <xref:System.ServiceModel.Discovery.DynamicEndpoint> は <xref:System.ServiceModel.Description.ServiceEndpoint> を継承しているため、`InvokeCalculatorService` メソッドに渡すことができます。 この例では、次に <xref:System.ServiceModel.Discovery.DynamicEndpoint> を使用して `CalculatorServiceClient` インスタンスを作成し、電卓サービスのさまざまな操作を呼び出します。

```csharp
static void InvokeCalculatorService(ServiceEndpoint serviceEndpoint)
{
   // Create a client
   CalculatorServiceClient client = new CalculatorServiceClient(serviceEndpoint);

   Console.WriteLine("Invoking CalculatorService");
   Console.WriteLine();

   double value1 = 100.00D;
   double value2 = 15.99D;

   // Call the Add service operation.
   double result = client.Add(value1, value2);
   Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);

   // Call the Subtract service operation.
   result = client.Subtract(value1, value2);
   Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);

   // Call the Multiply service operation.
   result = client.Multiply(value1, value2);
   Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);

   // Call the Divide service operation.
   result = client.Divide(value1, value2);
   Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);
   Console.WriteLine();

   //Closing the client gracefully closes the connection and cleans up resources
   client.Close();
}
```

#### <a name="to-use-this-sample"></a>このサンプルを使用するには

1. このサンプルでは HTTP エンドポイントを使用します。このサンプルを実行するには、適切な URL ACL を追加する必要があります。 詳細については、「 [HTTP および HTTPS の構成](../feature-details/configuring-http-and-https.md)」を参照してください。 管理特権で次のコマンドを実行すると、適切な ACL が追加されます。 そのままではコマンドが動作しない場合は、代わりに、ドメインとユーザー名を引数に指定して実行してみてください。 `netsh http add urlacl url=http://+:8000/ user=%DOMAIN%\%UserName%`

2. Visual Studio 2012 を使用して、基本的な .sln を開き、サンプルをビルドします。

3. service.exe アプリケーションを実行します。

4. サービスが開始したら、client.exe を実行します。

5. クライアントがサービスのアドレスを知ることなくサービスを検索できたことを確認します。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Discovery\Basic`
