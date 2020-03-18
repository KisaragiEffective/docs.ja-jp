---
title: 一方向
ms.date: 03/30/2017
ms.assetid: 74e3e03d-cd15-4191-a6a5-1efa2dcb9e73
ms.openlocfilehash: 91bdc09e374b3a1c6d407d4bd95428fafaf3ecc1
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74714649"
---
# <a name="one-way"></a>一方向
このサンプルでは、一方向サービス操作へのサービスのアクセスを示します。 クライアントは、双方向サービス操作の場合と同様、サービス操作の完了を待機しません。 このサンプルは[はじめに](../../../../docs/framework/wcf/samples/getting-started-sample.md)に基づいており、`wsHttpBinding` バインディングを使用します。 このサンプルでは、サービスは自己ホスト型コンソール アプリケーションであり、サービスが要求を受信して処理するかどうかを監視できます。 また、クライアントもコンソール アプリケーションです。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
 一方向サービス コントラクトを作成するには、サービス コントラクトを定義し、<xref:System.ServiceModel.OperationContractAttribute> クラスを各操作に適用し、<xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> を `true` に設定します。次のコードを参照してください。  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface IOneWayCalculator  
{  
    [OperationContract(IsOneWay=true)]  
    void Add(double n1, double n2);  
    [OperationContract(IsOneWay = true)]  
    void Subtract(double n1, double n2);  
    [OperationContract(IsOneWay = true)]  
    void Multiply(double n1, double n2);  
    [OperationContract(IsOneWay = true)]  
    void Divide(double n1, double n2);  
}  
```  
  
 クライアントがサービス操作の完了を待機しないことを示すため、このサンプルではサービス コードに 5 秒の遅延を実装します。次のサンプル コードを参照してください。  
  
```csharp
// This service class implements the service contract.  
// This code writes output to the console window.  
 [ServiceBehavior(ConcurrencyMode = ConcurrencyMode.Multiple,  
    InstanceContextMode = InstanceContextMode.PerCall)]  
public class CalculatorService : IOneWayCalculator  
{  
    public void Add(double n1, double n2)  
    {  
        Console.WriteLine("Received Add({0},{1}) - sleeping", n1, n2);  
        System.Threading.Thread.Sleep(1000 * 5);  
        double result = n1 + n2;  
        Console.WriteLine("Processing Add({0},{1}) - result: {2}", n1, n2, result);  
    }  
    ...  
}  
```  
  
 クライアントがサービスを呼び出すと、サービス操作の完了を待たずに呼び出しが返されます。  
  
 サンプルを実行すると、クライアントとサービスのアクティビティがサービスとクライアントの両方のコンソール ウィンドウに表示されます。 サービスがクライアントから受信したメッセージを表示できます。 どちらかのコンソールで Enter キーを押すと、サービスとクライアントがどちらもシャットダウンされます。  
  
 先にクライアントが完了してその次にサービスが完了し、クライアントが一方向サービス操作の完了を待たないことが示されます。 クライアントからの出力を次に示します。  
  
```console  
Add(100,15.99)  
Subtract(145,76.54)  
Multiply(9,81.25)  
Divide(22,7)  
  
Press <ENTER> to terminate client.  
```  
  
 サービスからの出力が次のように表示されます。  
  
```console  
The service is ready.  
Press <ENTER> to terminate service.  
  
Received Add(100,15.99) - sleeping  
Received Subtract(145,76.54) - sleeping  
Received Multiply(9,81.25) - sleeping  
Received Divide(22,7) - sleeping  
Processing Add(100,15.99) - result: 115.99  
Processing Subtract(145,76.54) - result: 68.46  
Processing Multiply(9,81.25) - result: 731.25  
Processing Divide(22,7) - result: 3.14285714285714  
```  
  
> [!NOTE]
> HTTP は本来、要求/応答プロトコルです。つまり、要求が行われると応答が返されます。 これは、HTTP を介して公開される一方向サービス操作にも当てはまります。 この操作が呼び出されると、サービスは HTTP ステータス コード 202 を返し、その後サービス操作が実行されます。 このステータス コードは、要求は処理用に受け入れられたが、処理はまだ完了していないことを示します。 操作を呼び出したクライアントは、サービスから 202 応答を受信するまでブロックします。 これにより、セッションを使用するように構成されているバインディングを使用して複数の一方向メッセージが送信されたときに、いくつかの予期しない動作が発生する場合があります。 このサンプルで使用されている `wsHttpBinding` バインディングは、既定ではセッションを使用してセキュリティ コンテキストを確立するように構成されています。 既定では、セッション内のメッセージは、送信される順序で到着することが保証されています。 このため、セッション内の 2 番目のメッセージが送信されても、最初のメッセージが処理されるまでは処理されません。 この結果、クライアントは、前のメッセージの処理が完了するまではメッセージの 202 応答を受信しません。 このため、クライアントは以降のそれぞれの操作呼び出しでブロックしているように見えます。 この動作を回避するため、サンプルでは、メッセージを処理用の各インスタンスに同時にディスパッチするようにランタイムを構成します。 サンプルでは、各メッセージが異なるインスタンスによって処理できるように <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> を `PerCall` に設定します。 複数のスレッドがメッセージを同時にディスパッチするように、<xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A> が `Multiple` に設定されています。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
3. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。  
  
> [!NOTE]
> サービスを先に実行してからクライアントを実行してください。また、クライアントをシャットダウンした後でサービスをシャットダウンしてください。 これにより、サービスが終了したことによりクライアントでセキュリティ セッションが正常に終了できない場合に発生する、クライアント側の例外が回避できます。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Service\Oneway`  
