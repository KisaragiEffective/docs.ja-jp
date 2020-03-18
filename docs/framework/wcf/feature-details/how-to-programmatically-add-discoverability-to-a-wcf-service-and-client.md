---
title: プログラムを使用して探索可能性に WCF サービスとクライアントを追加する方法
ms.date: 03/30/2017
ms.assetid: 4f7ae7ab-6fc8-4769-9730-c14d43f7b9b1
ms.openlocfilehash: a139eb4a15486be329bc6853ee6b3a3be06b0619
ms.sourcegitcommit: 9c3a4f2d3babca8919a1e490a159c1500ba7a844
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/12/2019
ms.locfileid: "72291573"
---
# <a name="how-to-programmatically-add-discoverability-to-a-wcf-service-and-client"></a>プログラムを使用して探索可能性に WCF サービスとクライアントを追加する方法
このトピックでは、Windows Communication Foundation (WCF) サービスを探索可能にする方法について説明します。 これは、[自己ホスト](https://go.microsoft.com/fwlink/?LinkId=145523)のサンプルに基づいています。  
  
### <a name="to-configure-the-existing-self-host-service-sample-for-discovery"></a>既存の自己ホスト サービス サンプルを探索用に構成するには  
  
1. Visual Studio 2012 でセルフホストソリューションを開きます。 このサンプルは、TechnologySamples\Basic\Service\Hosting\SelfHost ディレクトリにあります。  
  
2. `System.ServiceModel.Discovery.dll` への参照をサービス プロジェクトに追加します。 "システム" というエラーメッセージが表示される場合があります。 ServiceModel またはその依存関係の1つには、プロジェクトで指定されているものより新しいバージョンの .NET Framework が必要です。 "このメッセージが表示された場合は、ソリューションエクスプローラーでプロジェクトを右クリックし、 **[プロパティ]** を選択します。 プロジェクトの**プロパティ**ウィンドウで、**ターゲットフレームワーク**が [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)]ていることを確認します。  
  
3. Service.cs ファイルを開き、次の `using` ステートメントを追加します。  
  
    ```csharp  
    using System.ServiceModel.Discovery;  
    ```  
  
4. `Main()` メソッドの `using` ステート内で、<xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> インスタンスをサービス ホストに追加します。  
  
    ```csharp  
    public static void Main()  
    {  
        // Create a ServiceHost for the CalculatorService type.  
        using (ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService)))  
        {  
            // Add a ServiceDiscoveryBehavior  
            serviceHost.Description.Behaviors.Add(new ServiceDiscoveryBehavior());                  
  
            // ...  
        }  
    }  
    ```  
  
     <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> は、それが適用されているサービスが探索可能であることを指定します。  
  
5. <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> を、<xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> を追加するコードの直後でサービス ホストに追加します。  
  
    ```csharp  
    // Add ServiceDiscoveryBehavior  
    serviceHost.Description.Behaviors.Add(new ServiceDiscoveryBehavior());  
  
    // Add a UdpDiscoveryEndpoint  
    serviceHost.AddServiceEndpoint(new UdpDiscoveryEndpoint());  
    ```  
  
     このコードは、探索メッセージを標準の UDP 探索エンドポイントに送信する必要があることを指定します。  
  
### <a name="to-create-a-client-application-that-uses-discovery-to-call-the-service"></a>探索を使用してサービスを呼び出すクライアント アプリケーションを作成するには  
  
1. 新しいコンソール アプリケーションを `DiscoveryClientApp` というソリューションに追加します。  
  
2. `System.ServiceModel.dll` および `System.ServiceModel.Discovery.dll` への参照を追加します。  
  
3. GeneratedClient.cs ファイルおよび App.config ファイルを、既存のクライアント プロジェクトから新しい DiscoveryClientApp プロジェクトに追加します。 これを行うには、**ソリューションエクスプローラー**内のファイルを右クリックし、 **[コピー]** を選択します。次に、 **DiscoveryClientApp**プロジェクトを選択して右クリックし、 **[貼り付け]** を選択します。  
  
4. Program.cs を開きます。  
  
5. 次の `using` ステートメントを追加します。  
  
    ```csharp  
    using System.ServiceModel;  
    using System.ServiceModel.Discovery;  
    using Microsoft.ServiceModel.Samples;  
    ```  
  
6. `FindCalculatorServiceAddress()` という静的メソッドを `Program` クラスに追加します。  
  
    ```csharp  
    static EndpointAddress FindCalculatorServiceAddress()  
    {  
    }  
    ```  
  
     このメソッドは、探索を使用して `CalculatorService` サービスを検索します。  
  
7. `FindCalculatorServiceAddress` メソッド内で、新しい <xref:System.ServiceModel.Discovery.DiscoveryClient> インスタンスを作成し、<xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> をコンストラクターに渡します。  
  
    ```csharp  
    static EndpointAddress FindCalculatorServiceAddress()  
    {  
        // Create DiscoveryClient  
        DiscoveryClient discoveryClient = new DiscoveryClient(new UdpDiscoveryEndpoint());  
    }  
    ```  
  
     これは、<xref:System.ServiceModel.Discovery.DiscoveryClient> クラスが、探索メッセージの送受信に標準の UDP 探索エンドポイントを使用する必要があることを WCF に伝えます。  
  
8. 次の行では、<xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A> メソッドを呼び出し、検索対象のサービス コントラクトを含む <xref:System.ServiceModel.Discovery.FindCriteria> インスタンスを指定します。 ここでは、`ICalculator` を指定します。  
  
    ```csharp  
    // Find ICalculatorService endpoints              
    FindResponse findResponse = discoveryClient.Find(new FindCriteria(typeof(ICalculator)));  
    ```  
  
9. <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A> への呼び出しの後で、一致するサービスが少なくとも 1 つあるかどうかを確認し、最初に一致したサービスの <xref:System.ServiceModel.EndpointAddress> を返します。 一致するサービスがない場合は `null` を返します。  
  
    ```csharp  
    if (findResponse.Endpoints.Count > 0)  
    {  
        return findResponse.Endpoints[0].Address;  
    }  
    else  
    {  
        return null;  
    }  
    ```  
  
10. `InvokeCalculatorService` という名前の静的メソッドを `Program` クラスに追加します。  
  
    ```csharp  
    static void InvokeCalculatorService(EndpointAddress endpointAddress)  
    {  
    }  
    ```  
  
     このメソッドは、`FindCalculatorServiceAddress` から返されたエンドポイント アドレスを使用して、電卓サービスを呼び出します。  
  
11. `InvokeCalculatorService` メソッド内で、`CalculatorServiceClient` クラスのインスタンスを作成します。 このクラスは、[自己ホスト](https://go.microsoft.com/fwlink/?LinkId=145523)のサンプルで定義されています。 これは、Svcutil.exe を使用して生成されました。  
  
    ```csharp  
    // Create a client  
    CalculatorClient client = new CalculatorClient();  
    ```  
  
12. 次の行では、クライアントのエンドポイント アドレスを、`FindCalculatorServiceAddress()` から返されたエンドポイント アドレスに設定します。  
  
    ```csharp  
    // Connect to the discovered service endpoint  
    client.Endpoint.Address = endpointAddress;  
    ```  
  
13. 前の手順のコードの直後に、電卓サービスで公開されたメソッドを呼び出します。  
  
    ```csharp  
    Console.WriteLine("Invoking CalculatorService at {0}", endpointAddress);  
  
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
    ```  
  
14. `Main()` クラスの `Program` メソッドにコードを追加して、`FindCalculatorServiceAddress` を呼び出します。  
  
    ```csharp  
    public static void Main()  
    {  
        EndpointAddress endpointAddress = FindCalculatorServiceAddress();  
    }  
    ```  
  
15. 次の行では、`InvokeCalculatorService()` を呼び出し、`FindCalculatorServiceAddress()` から返されたエンドポイント アドレスを渡します。  
  
    ```csharp  
    if (endpointAddress != null)  
    {  
        InvokeCalculatorService(endpointAddress);  
    }  
  
    Console.WriteLine("Press <ENTER> to exit.");  
    Console.ReadLine();  
    ```  
  
### <a name="to-test-the-application"></a>アプリケーションをテストするには  
  
1. 権限のレベルが高いコマンド プロンプトを開き、Service.exe を実行します。  
  
2. コマンド プロンプトを開き、Discoveryclientapp.exe を実行します。  
  
3. Service.exe からの出力は次のようになります。  
  
    ```output  
    Received Add(100,15.99)  
    Return: 115.99  
    Received Subtract(100,15.99)  
    Return: 84.01  
    Received Multiply(100,15.99)  
    Return: 1599  
    Received Divide(100,15.99)  
    Return: 6.25390869293308  
    ```  
  
4. Discoveryclientapp.exe からの出力は次のようになります。  
  
    ```output  
    Invoking CalculatorService at http://localhost:8000/ServiceModelSamples/service  
    Add(100,15.99) = 115.99  
    Subtract(100,15.99) = 84.01  
    Multiply(100,15.99) = 1599  
    Divide(100,15.99) = 6.25390869293308  
  
    Press <ENTER> to exit.  
    ```  
  
## <a name="example"></a>例  
 このサンプルで使用されているコード全体の一覧を次に示します。 このコードは[自己ホスト](https://go.microsoft.com/fwlink/?LinkId=145523)のサンプルに基づいているため、変更されたファイルのみが一覧表示されます。 自己ホストのサンプルの詳細については、「[セットアップ手順](https://go.microsoft.com/fwlink/?LinkId=145522)」を参照してください。  
  
```csharp  
// Service.cs  
using System;  
using System.Configuration;  
using System.ServiceModel;  
using System.ServiceModel.Discovery;  
  
namespace Microsoft.ServiceModel.Samples  
{  
    // See SelfHost sample for service contract and implementation  
    // ...  
  
        // Host the service within this EXE console application.  
        public static void Main()  
        {  
            // Create a ServiceHost for the CalculatorService type.  
            using (ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService)))  
            {  
                // Add the ServiceDiscoveryBehavior to make the service discoverable  
                serviceHost.Description.Behaviors.Add(new ServiceDiscoveryBehavior());  
                serviceHost.AddServiceEndpoint(new UdpDiscoveryEndpoint());  
  
                // Open the ServiceHost to create listeners and start listening for messages.  
                serviceHost.Open();  
  
                // The service can now be accessed.  
                Console.WriteLine("The service is ready.");  
                Console.WriteLine("Press <ENTER> to terminate service.");  
                Console.WriteLine();  
                Console.ReadLine();  
            }  
        }  
    }  
}  
```  
  
```csharp  
// Program.cs  
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.ServiceModel;  
using System.ServiceModel.Discovery;  
using Microsoft.ServiceModel.Samples;  
using System.Text;  
  
namespace DiscoveryClientApp  
{  
    class Program  
    {  
        static EndpointAddress FindCalculatorServiceAddress()  
        {  
            // Create DiscoveryClient  
            DiscoveryClient discoveryClient = new DiscoveryClient(new UdpDiscoveryEndpoint());  
  
            // Find ICalculatorService endpoints              
            FindResponse findResponse = discoveryClient.Find(new FindCriteria(typeof(ICalculator)));  
  
            if (findResponse.Endpoints.Count > 0)  
            {  
                return findResponse.Endpoints[0].Address;  
            }  
            else  
            {  
                return null;  
            }  
        }  
  
        static void InvokeCalculatorService(EndpointAddress endpointAddress)  
        {  
            // Create a client  
            CalculatorClient client = new CalculatorClient();  
  
            // Connect to the discovered service endpoint  
            client.Endpoint.Address = endpointAddress;  
  
            Console.WriteLine("Invoking CalculatorService at {0}", endpointAddress);  
  
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
        static void Main(string[] args)  
        {  
            EndpointAddress endpointAddress = FindCalculatorServiceAddress();  
  
            if (endpointAddress != null)  
            {  
                InvokeCalculatorService(endpointAddress);  
            }  
  
            Console.WriteLine("Press <ENTER> to exit.");  
            Console.ReadLine();  
  
        }  
    }  
}  
```  

## <a name="see-also"></a>参照

- [WCF Discovery の概要](../../../../docs/framework/wcf/feature-details/wcf-discovery-overview.md)
- [WCF Discovery オブジェクト モデル](../../../../docs/framework/wcf/feature-details/wcf-discovery-object-model.md)
