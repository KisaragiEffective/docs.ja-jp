---
title: 型指定のない要求-応答
ms.date: 03/30/2017
ms.assetid: 0bf0f9d9-7caf-4d3d-8c9e-2d468cca16a5
ms.openlocfilehash: ba1caddd8f37a37df63e2710883f3096e0989fcd
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715864"
---
# <a name="untyped-requestreply"></a>型指定のない要求/応答
このサンプルは、Message クラスを使用する操作コントラクトを定義する方法を示します。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
 このサンプルは、[はじめに](../../../../docs/framework/wcf/samples/getting-started-sample.md)に基づいています。 サービス コントラクトは、メッセージの種類を引数として取得してメッセージを返すという 1 つの操作を定義します。 この操作は、合計の計算に必要なすべてのデータをメッセージ本文から収集し、その合計を返信メッセージの本文に格納して返送します。  
  
```csharp
[OperationContract(Action = CalculatorService.RequestAction, ReplyAction = CalculatorService.ReplyAction)]  
Message ComputeSum(Message request);  
```  
  
 サービスでは、操作によって入力メッセージ内で渡された整数の配列が取得され、その合計が計算されます。 応答メッセージの送信では、サンプルは適切なメッセージ バージョンと Action を含む新しいメッセージを作成し、計算された合計をメッセージ本文に追加します。 これを実行するサンプル コードを次に示します。  
  
```csharp
public Message ComputeSum(Message request)  
{  
    //The body of the message contains a list of numbers which will be   
    //read as a int[] using GetBody<T>  
    int result = 0;  
  
    int[] inputs = request.GetBody<int[]>();  
    foreach (int i in inputs)  
    {  
        result += i;  
    }  
  
    Message response = Message.CreateMessage(request.Version,   
                                      ReplyAction, result);  
    return response;  
}  
```  
  
 クライアントは、 [ServiceModel メタデータユーティリティツール (svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)によって生成されたコードを使用して、リモートサービスへのプロキシを作成します。 要求メッセージを送信するには、クライアントにメッセージ バージョンが存在する必要があります。これは基になるチャネルによって異なります。 したがって、クライアントは、作成済みのプロキシ チャネルに適用される新しい <xref:System.ServiceModel.OperationContextScope> を作成し、これが、<xref:System.ServiceModel.OperationContext> プロパティで設定されている正しいメッセージ バージョンにより、`OutgoingMessageHeaders.MessageVersion` を作成します。 クライアントは入力配列を本文として要求メッセージに渡し、プロキシの `ComputeSum` を呼び出します。 次にクライアントは、応答メッセージの `GetBody<T>` メソッドにアクセスし、渡した入力の合計を取得します。 これを実行するサンプル コードを次に示します。  
  
```csharp
using (new OperationContextScope(client.InnerChannel))  
{  
    // Call the Sum service operation.  
    int[] values = { 1, 2, 3, 4, 5 };  
    Message request = Message.CreateMessage(  
        OperationContext.Current.OutgoingMessageHeaders.MessageVersion,   
        RequestAction, values);  
    Message reply = client.ComputeSum(request);  
    int response = reply.GetBody<int>();  
  
    Console.WriteLine("Sum of numbers passed (1,2,3,4,5) = {0}",   
                                                       response);  
}  
```  
  
 このサンプルは Web ホストのサンプルです。したがって、クライアント実行可能ファイルのみを実行する必要があります。 クライアントでの出力のサンプルを次に示します。  
  
```console  
Prompt>Client.exe  
Sum of numbers passed (1,2,3,4,5) = 15  
  
Press <ENTER> to terminate client.  
```  
  
 このサンプルは Web ホストのサンプルです。したがって、手順 3. で示したリンクを確認し、サンプルのビルド方法と実行方法を確認してください。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
3. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Message\Untyped`  
