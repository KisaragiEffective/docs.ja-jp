---
title: Net.TCP ポート共有のサンプル
ms.date: 03/30/2017
ms.assetid: 03da5959-0574-4e91-8a53-05854b6c55dc
ms.openlocfilehash: e7a814dcdc027980f70ec90e384f3ec2f74b364d
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74714706"
---
# <a name="nettcp-port-sharing-sample"></a>Net.TCP ポート共有のサンプル
TCP/IP プロトコルはポートと呼ばれる 16 ビットの番号を使用して、同じコンピュータ上で実行されている複数のネットワーク アプリケーションへの接続を区別します。 アプリケーションがポートをリッスンすると、そのポートのすべての TCP トラフィックがそのアプリケーションに送られます。 複数のアプリケーションが 1 つのポートで同時にリッスンすることはできません。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\TCP\PortSharing`  
  
 多くのプロトコルは、使用する標準または既定のポート番号を持っています。 たとえば、HTTP プロトコルは一般的に TCP ポート 80 を使用します。 インターネット インフォメーション サービス (IIS) には、複数の HTTP アプリケーション間でポートを共有するためのリスナーがあります。 IIS はポートを直接リッスンし、メッセージ ストリーム内の情報に基づいて、そのメッセージを適切なアプリケーションに転送します。 これにより、複数の HTTP アプリケーションで、メッセージ受信用のポートの予約が競合することなく同じポート番号を使用できます。  
  
 NetTcp ポート共有は、同じように、複数のネットワークアプリケーションが1つのポートを共有できる Windows Communication Foundation (WCF) 機能です。 NetTcp ポート共有サービスは net.tcp プロトコルを使用して接続を受け入れ、メッセージの送信先アドレスに基づいてメッセージを転送します。  
  
 既定では、NetTcp ポート共有サービスは有効ではありません。 このサンプルを実行する前に、手動でサービスを有効にする必要があります。 詳細については、「[方法: Net.tcp ポート共有サービスを有効](../../../../docs/framework/wcf/feature-details/how-to-enable-the-net-tcp-port-sharing-service.md)にする」を参照してください。 サービスが無効な場合は、サーバー アプリケーションの開始時に例外がスローされます。  
  
```console
Unhandled Exception: System.ServiceModel.CommunicationException: The TransportManager failed to listen on the supplied URI using the NetTcpPortSharing service: failed to start the service because it is disabled. An administrator can enable it by running 'sc.exe config NetTcpPortSharing start= demand'.. ---> System.InvalidOperationException: Cannot start service NetTcpPortSharing on computer '.'. ---> System.ComponentModel.Win32Exception: The service cannot be started, either because it is disabled or because it has no enabled devices associated with it  
```  
  
 ポート共有をサーバー上で有効にするには、<xref:System.ServiceModel.NetTcpBinding.PortSharingEnabled%2A> バインディングまたは <xref:System.ServiceModel.NetTcpBinding> バインディング要素の <xref:System.ServiceModel.Channels.TcpTransportBindingElement> プロパティを設定します。 クライアントは、サーバー上で使用されるポート共有の構成内容を知る必要はありません。  
  
## <a name="enabling-port-sharing"></a>ポート共有の有効化  
 次のコードでは、サーバー上でのポート共有の有効化を示します。 ランダムな URI パスが含まれる固定ポートで、`ICalculator` サービスのインスタンスを開始します。 2 つのサービスは同じポートを共有できますが、NetTcp ポート共有サービスが正しいアプリケーションにメッセージをルーティングできるように、これらのエンドポイント アドレスは全体で一意であることが必要です。  

```csharp
// Configure a binding with TCP port sharing enabled  
NetTcpBinding binding = new NetTcpBinding();  
binding.PortSharingEnabled = true;  
  
// Start a service on a fixed TCP port  
ServiceHost host = new ServiceHost(typeof(CalculatorService));  
ushort salt = (ushort)new Random().Next();  
string address = $"net.tcp://localhost:9000/calculator/{salt}";
host.AddServiceEndpoint(typeof(ICalculator), binding, address);  
host.Open();  
```

 ポート共有を有効にすると、ポート番号の競合を起こすことなく、何度もサービスを実行できます。 コードを変更してポート共有を無効にした場合、サービスを 2 つ開始すると 2 つ目のサービスが失敗し、<xref:System.ServiceModel.AddressAlreadyInUseException> が発生します。  
  
```console  
Unhandled Exception: System.ServiceModel.AddressAlreadyInUseException: There is already a listener on IP endpoint 0.0.0.0:9000.  Make sure that you are not trying to use this endpoint multiple times in your application and that there are no other applications listening on this endpoint. ---> System.Net.Sockets.SocketException: Only one usage of each socket address (protocol/network address/port) is normally permitted  
```  
  
## <a name="running-the-sample"></a>サンプルの実行  
 テスト クライアントを使用して、ポートを共有しているサービスにメッセージが正しくルーティングされていることを確認できます。  

```csharp
class client  
{  
   static void Main(string[] args)  
   {  
      Console.Write("Enter the service number to test: ");  
      ushort salt = ushort.Parse(Console.ReadLine());  
      string address = $"net.tcp://localhost:9000/calculator/{salt}";
      ChannelFactory<ICalculator> factory = new ChannelFactory<ICalculator>(new NetTcpBinding());  
      ICalculator proxy = factory.CreateChannel(new EndpointAddress(address));  
  
      // Call the Add service operation.  
      double value1 = 100.00D;  
      double value2 = 15.99D;  
      double result = proxy.Add(value1, value2);  
      Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
  
      // Call the Subtract service operation.  
      value1 = 145.00D;  
      value2 = 76.54D;  
      result = proxy.Subtract(value1, value2);  
      Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);  
  
      // Call the Multiply service operation.  
      value1 = 9.00D;  
      value2 = 81.25D;  
      result = proxy.Multiply(value1, value2);  
      Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);  
  
      // Call the Divide service operation.  
      value1 = 22.00D;  
      value2 = 7.00D;  
      result = proxy.Divide(value1, value2);  
      Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);  
  
      Console.WriteLine();  
      Console.WriteLine("Press <ENTER> to terminate client.");  
      Console.ReadLine();  
  
      factory.Close();  
   }  
}  
```

 サービスの各インスタンスは、一意の番号とアドレスを出力します。 たとえば、service.exe を実行すると、次のテキストが表示されます。  
  
```console  
Service #4381 listening on net.tcp://localhost:9000/calculator/4381.  
Press <ENTER> to terminate service.  
```  
  
 client.exe を実行する際に、ここに表示されるサービス番号を入力します。  
  
```console  
Enter the service number to test: 4381  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
Press <ENTER> to terminate client.  
```  
  
 このサンプルは、クライアントが使用する生成アドレスを変更することにより、複数コンピュータの構成で実行できます。 Client.cs で、エンドポイント アドレスの書式文字列をサービスの新しいアドレスに合わせます。 "localhost" への参照をすべてサーバー コンピュータの IP アドレスに置き換えます。 この変更を行った後に、サンプルを再コンパイルする必要があります。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. 次のコマンドを使用して、ASP.NET 4.0 をインストールします。  
  
    ```console  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
3. 概要セクションに示したように、NetTcp ポート共有サービスを有効にします。  
  
4. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
5. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。 このサンプルを実行するための詳細情報については、前述の「サンプルの実行」を参照してください。  
