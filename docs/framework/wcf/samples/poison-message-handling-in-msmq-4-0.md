---
title: MSMQ 4.0 での有害メッセージ処理
ms.date: 03/30/2017
ms.assetid: ec8d59e3-9937-4391-bb8c-fdaaf2cbb73e
ms.openlocfilehash: 0a9d4ec9657bacdbcb1273791dc7a593a9565c25
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094957"
---
# <a name="poison-message-handling-in-msmq-40"></a>MSMQ 4.0 での有害メッセージ処理
このサンプルでは、サービスで有害メッセージの処理を実行する方法を示します。 このサンプルは、トランザクション処理された[MSMQ バインディング](../../../../docs/framework/wcf/samples/transacted-msmq-binding.md)のサンプルに基づいています。 このサンプルでは、`netMsmqBinding` を使用しています。 サービスは自己ホスト型コンソール アプリケーションであるので、キューに置かれたメッセージをサービスが受信するようすを観察できます。

 キュー通信では、クライアントはサービスとの通信にキューを使用します。 厳密には、クライアントはメッセージをキューに送信します。 サービスは、メッセージをキューから受信します。 したがって、キューを使用する通信では、サービスとクライアントが同時に実行されていなくてもかまいません。

 有害メッセージとは、メッセージを読み取るサービスがメッセージを処理できないためメッセージの読み取りが行われるトランザクションを終了する場合に、キューから繰り返し読み取られるメッセージのことです。 そのような場合、メッセージは再試行されます。 メッセージに問題がある場合、この再試行は理論上、永久に継続します。 これは、トランザクションを使用してキューから読み取り、サービス操作を呼び出す場合にのみ発生します。

 MSMQ のバージョンによって、NetMsmqBinding がサポートする有害メッセージの検出が制限されている場合と、制限されていない場合があります。 メッセージが有害として検出されたら、いくつかの方法で処理できます。 また、MSMQ のバージョンによって、NetMsmqBinding がサポートする有害メッセージの処理が制限されている場合と、制限されていない場合があります。

 このサンプルでは、windows Server 2003 および Windows XP プラットフォームに用意されている制限付きの有害機能、および Windows Vista で提供される完全な有害機能について説明します。 どちらのサンプルでも、有害メッセージをキューから別のキューに移動することを目的としています。 そのキューは、有害メッセージサービスによって提供されます。

## <a name="msmq-v40-poison-handling-sample"></a>MSMQ v4.0 の有害メッセージ処理サンプル
 Windows Vista では、有害メッセージの格納に使用できる有害なサブキュー機能が MSMQ に用意されています。 このサンプルでは、Windows Vista を使用して有害なメッセージを処理するためのベストプラクティスを示します。

 Windows Vista での有害メッセージの検出は洗練されています。 検出に役立つ 3 つのプロパティがあります。 <xref:System.ServiceModel.MsmqBindingBase.ReceiveRetryCount%2A> は、指定したメッセージをキューから再度読み取り、処理するためにアプリケーションにディスパッチする回数です。 メッセージをアプリケーションにディスパッチできないか、またはアプリケーションがサービス操作内のトランザクションをロールバックしたため、メッセージはキューに戻ったときにキューから再度読み取られます。 <xref:System.ServiceModel.MsmqBindingBase.MaxRetryCycles%2A> は、メッセージが再試行キューに移動する回数です。 <xref:System.ServiceModel.MsmqBindingBase.ReceiveRetryCount%2A> に達すると、メッセージは再試行キューに移動されます。 メッセージが再試行キューからメイン キューに移動されると、<xref:System.ServiceModel.MsmqBindingBase.RetryCycleDelay%2A> プロパティは時間遅延となります。 <xref:System.ServiceModel.MsmqBindingBase.ReceiveRetryCount%2A> が 0 にリセットされます。 メッセージは再試行されます。 メッセージを読み取るすべての試行に失敗すると、メッセージが有害としてマークされます。

 メッセージが有害としてマークされると、メッセージは <xref:System.ServiceModel.MsmqBindingBase.ReceiveErrorHandling%2A> 列挙体の設定に従って処理されます。 次にもう一度、使用可能な値を示します。

- Fault (既定値): リスナーとサービス ホストをエラーにします。

- Drop: メッセージを破棄します。

- 移動: メッセージを有害メッセージサブキューに移動します。 この値は、Windows Vista でのみ使用できます。

- Reject: メッセージを送信者の配信不能キューに戻すことにより、メッセージを拒否します。 この値は、Windows Vista でのみ使用できます。

 このサンプルは有害なメッセージに対する `Move` 処置の使用法を示します。 `Move` すると、メッセージが有害サブキューに移動します。

 サービス コントラクトは `IOrderProcessor` です。これは、キューでの使用に適した一方向サービスを定義します。

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true)]
    void SubmitPurchaseOrder(PurchaseOrder po);
}
```

 サービス操作により、注文を処理していることを示すメッセージが表示されます。 有害なメッセージ機能を示すために、サービスのランダム呼び出しでトランザクションをロールバックするために、`SubmitPurchaseOrder` サービス操作は例外をスローします。 これにより、メッセージがキューに戻ります。 最終的に、メッセージは有害とマークされます。 この構成は、有害メッセージを有害サブキューに移動するように設定されています。

```csharp
// Service class that implements the service contract.
// Added code to write output to the console window.
public class OrderProcessorService : IOrderProcessor
{
    static Random r = new Random(137);

    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    public void SubmitPurchaseOrder(PurchaseOrder po)
    {

        int randomNumber = r.Next(10);

        if (randomNumber % 2 == 0)
        {
            Orders.Add(po);
            Console.WriteLine("Processing {0} ", po);
        }
        else
        {
            Console.WriteLine("Aborting transaction, cannot process purchase order: " + po.PONumber);
            Console.WriteLine();
            throw new Exception("Cannot process purchase order: " + po.PONumber);
        }
    }

    public static void OnServiceFaulted(object sender, EventArgs e)
    {
        Console.WriteLine("Service Faulted");
    }

    // Host the service within this EXE console application.
    public static void Main()
    {
        // Get MSMQ queue name from app settings in configuration.
        string queueName = ConfigurationManager.AppSettings["queueName"];

        // Create the transacted MSMQ queue if necessary.
        if (!System.Messaging.MessageQueue.Exists(queueName))
            System.Messaging.MessageQueue.Create(queueName, true);

        // Get the base address that is used to listen for WS-MetaDataExchange requests.
        // This is useful to generate a proxy for the client.
        string baseAddress = ConfigurationManager.AppSettings["baseAddress"];

        // Create a ServiceHost for the OrderProcessorService type.
        ServiceHost serviceHost = new ServiceHost(typeof(OrderProcessorService), new Uri(baseAddress));

        // Hook on to the service host faulted events.
        serviceHost.Faulted += new EventHandler(OnServiceFaulted);

        // Open the ServiceHostBase to create listeners and start listening for messages.
        serviceHost.Open();

        // The service can now be accessed.
        Console.WriteLine("The service is ready.");
        Console.WriteLine("Press <ENTER> to terminate service.");
        Console.WriteLine();
        Console.ReadLine();

        if(serviceHost.State != CommunicationState.Faulted) {
            serviceHost.Close();
        }

    }
}
```

 サービス構成には、`receiveRetryCount`、`maxRetryCycles`、`retryCycleDelay`、および `receiveErrorHandling` という有害メッセージ プロパティが含まれています。次の構成ファイルを参照してください。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <appSettings>
    <!-- Use appSetting to configure MSMQ queue name. -->
    <add key="queueName" value=".\private$\ServiceModelSamplesPoison" />
    <add key="baseAddress" value="http://localhost:8000/orderProcessor/poisonSample"/>
  </appSettings>
  <system.serviceModel>
    <services>
      <service
              name="Microsoft.ServiceModel.Samples.OrderProcessorService">
        <!-- Define NetMsmqEndpoint -->
        <endpoint address="net.msmq://localhost/private/ServiceModelSamplesPoison"
                  binding="netMsmqBinding"
                  bindingConfiguration="PoisonBinding"
                  contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />
      </service>
    </services>

    <bindings>
      <netMsmqBinding>
        <binding name="PoisonBinding"
                 receiveRetryCount="0"
                 maxRetryCycles="1"
                 retryCycleDelay="00:00:05"
                 receiveErrorHandling="Move">
        </binding>
      </netMsmqBinding>
    </bindings>
  </system.serviceModel>
</configuration>
```

## <a name="processing-messages-from-the-poison-message-queue"></a>有害メッセージ キューのメッセージの処理
 有害メッセージ サービスは、最終的な有害メッセージ キューからメッセージを読み取って、それを処理します。

 有害メッセージ キューのメッセージは、メッセージを処理するサービスに対応付けられたメッセージで、有害メッセージ サービスのエンドポイントとは異なる場合があります。 したがって、有害メッセージサービスがキューからメッセージを読み取る場合、WCF チャネル層はエンドポイントで不一致を検出し、メッセージをディスパッチしません。 この場合、メッセージは注文処理サービス宛てになりますが、有害メッセージ サービスが受信します。 別のエンドポイント宛のメッセージも含めてメッセージの受信を続けるには、`ServiceBehavior` を追加して、メッセージの任意の宛先サービス エンドポイントを一致条件としてアドレスをフィルタする必要があります。 これは、有害メッセージ キューから読み込んだメッセージを正常に処理するために必要です。

 有害メッセージ サービス実装そのものは、サービス実装と非常によく似ています。 コントラクトを実装し、注文を処理します。 コード例を次に示します。

```csharp
// Service class that implements the service contract.
// Added code to write output to the console window.
[ServiceBehavior(AddressFilterMode=AddressFilterMode.Any)]
public class OrderProcessorService : IOrderProcessor
{
    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    public void SubmitPurchaseOrder(PurchaseOrder po)
    {
        Orders.Add(po);
        Console.WriteLine("Processing {0} ", po);
    }

    public static void OnServiceFaulted(object sender, EventArgs e)
    {
        Console.WriteLine("Service Faulted...exiting app");
        Environment.Exit(1);
    }

    // Host the service within this EXE console application.
    public static void Main()
    {

        // Create a ServiceHost for the OrderProcessorService type.
        ServiceHost serviceHost = new ServiceHost(typeof(OrderProcessorService));

        // Hook on to the service host faulted events.
        serviceHost.Faulted += new EventHandler(OnServiceFaulted);

        serviceHost.Open();

        // The service can now be accessed.
        Console.WriteLine("The poison message service is ready.");
        Console.WriteLine("Press <ENTER> to terminate service.");
        Console.WriteLine();
        Console.ReadLine();

        // Close the ServiceHostBase to shutdown the service.
        if(serviceHost.State != CommunicationState.Faulted)
        {
            serviceHost.Close();
        }
    }
```

 注文キューからメッセージを読み取る注文処理サービスとは異なり、有害メッセージサービスは有害サブキューからメッセージを読み取ります。 有害キューはメインキューのサブキューであり、"有害" という名前で、MSMQ によって自動的に生成されます。 このファイルにアクセスするには、次のサンプル構成に示すように、メインキュー名の後に ";" とサブキュー名 (この例では "有害") を指定します。

> [!NOTE]
> MSMQ v3.0 用のサンプルでは、有害キュー名はサブキューではなく、メッセージの移動先キューになります。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceModel.Samples.OrderProcessorService">
        <!-- Define NetMsmqEndpoint -->
        <endpoint address="net.msmq://localhost/private/ServiceModelSamplesPoison;poison"
                  binding="netMsmqBinding"
                  contract="Microsoft.ServiceModel.Samples.IOrderProcessor" >
        </endpoint>
      </service>
    </services>

  </system.serviceModel>
</configuration>
```

 サンプルを実行すると、クライアント、サービス、および有害メッセージ サービスのアクティビティがコンソール ウィンドウに表示されます。 サービスがクライアントから受信したメッセージを表示できます。 いずれかのコンソール ウィンドウで Enter キーを押すと、サービスがシャットダウンされます。

 サービスが実行を開始し、注文を処理し、処理の終了をランダムに開始します。 メッセージに注文処理の完了が示されていた場合、クライアントをもう一度実行して、サービスが実際にメッセージを終了したことを確認するまで別のメッセージを送信できます。 構成されている有害設定に基づき、メッセージは 1 度処理を試行されてから、最後の有害キューに移されます。

```console
The service is ready.
Press <ENTER> to terminate service.

Processing Purchase Order: 0f063b71-93e0-42a1-aa3b-bca6c7a89546
        Customer: somecustomer.com
        OrderDetails
                Order LineItem: 54 of Blue Widget @unit price: $29.99
                Order LineItem: 890 of Red Widget @unit price: $45.89
        Total cost of this order: $42461.56
        Order status: Pending

Processing Purchase Order: 5ef9a4fa-5a30-4175-b455-2fb1396095fa
        Customer: somecustomer.com
        OrderDetails
                Order LineItem: 54 of Blue Widget @unit price: $29.99
                Order LineItem: 890 of Red Widget @unit price: $45.89
        Total cost of this order: $42461.56
        Order status: Pending

Aborting transaction, cannot process purchase order: 23e0b991-fbf9-4438-a0e2-20adf93a4f89
```

 有害メッセージ サービスを起動して、有害メッセージを有害キューから読み込みます。 この例では、有害メッセージ サービスはメッセージを読み込んで処理します。 終了され有害指定された発注書が有害メッセージ サービスによって読み込まれるのを確認できます。

```console
The service is ready.
Press <ENTER> to terminate service.

Processing Purchase Order: 23e0b991-fbf9-4438-a0e2-20adf93a4f89
        Customer: somecustomer.com
        OrderDetails
                Order LineItem: 54 of Blue Widget @unit price: $29.99
                Order LineItem: 890 of Red Widget @unit price: $45.89
        Total cost of this order: $42461.56
        Order status: Pending
```

#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

2. サービスを最初に実行すると、サービスはキューが存在するかどうかを確認します。 キューが存在しない場合、サービスによってキューが作成されます。 最初にサービスを実行してキューを作成することも、MSMQ キュー マネージャーでキューを作成することもできます。 Windows 2008 でキューを作成するには、次の手順に従います。

    1. Visual Studio 2012 でサーバーマネージャーを開きます。

    2. **[機能]** タブを展開します。

    3. **[プライベートメッセージキュー]** を右クリックし、 **[新規]** 、 **[プライベートキュー]** の順に選択します。

    4. **[トランザクション]** ボックスをオンにします。

    5. 新しいキューの名前として `ServiceModelSamplesTransacted` を入力します。

3. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。

4. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、キュー名を変更し、localhost ではなく実際のホスト名を反映させ、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。

 `netMsmqBinding` バインディング トランスポートを使用する場合の既定では、セキュリティが有効です。 トランスポート セキュリティの種類は、`MsmqAuthenticationMode` と `MsmqProtectionLevel` の 2 つのプロパティで決まります。 既定では、認証モードは `Windows` に、保護レベルは `Sign` に、それぞれ設定されます。 MSMQ の認証および署名の機能を利用するには、ドメインに属している必要があります。 ドメインに属していないコンピューターでこのサンプルを実行すると、"User's internal message queuing certificate does not exist" というエラーが表示されます。

#### <a name="to-run-the-sample-on-a-computer-joined-to-a-workgroup"></a>ワークグループに参加しているコンピューターでこのサンプルを実行するには

1. ドメインに属していないコンピューターを使用する場合は、トランスポート セキュリティをオフにします。オフにするには、認証モードとセキュリティ レベルを `None` に設定します。サンプル構成を次に示します。

    ```xml
    <bindings>
        <netMsmqBinding>
            <binding name="TransactedBinding">
                <security mode="None"/>
            </binding>
        </netMsmqBinding>
    </bindings>
    ```

     エンドポイントの bindingConfiguration 属性を設定して、エンドポイントがこのバインディングに関連付けられるようにします。

2. サンプルを実行する前に、PoisonMessageServer、サーバー、およびクライアントで構成が変更されていることを確認してください。

    > [!NOTE]
    > `security mode` を `None` に設定することは、`MsmqAuthenticationMode`、`MsmqProtectionLevel`、および `Message` のセキュリティを `None` に設定することに相当します。  
  
3. Metadata Exchange を機能させるため、URL を HTTP バインディングに登録します。 これを行うには、サービスが権限の高いコマンド ウィンドウで実行されている必要があります。 それ以外の場合は、`Unhandled Exception: System.ServiceModel.AddressAccessDeniedException: HTTP could not register URL http://+:8000/ServiceModelSamples/service/. Your process does not have access rights to this namespace (see https://go.microsoft.com/fwlink/?LinkId=70353 for details). ---> System.Net.HttpListenerException: Access is denied`のような例外が発生します。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\MSMQ\Poison\MSMQ4`
