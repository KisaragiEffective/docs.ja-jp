---
title: 配信不能キュー
ms.date: 03/30/2017
ms.assetid: ff664f33-ad02-422c-9041-bab6d993f9cc
ms.openlocfilehash: 5025aa784817d1189f23918eacfef275abf968e1
ms.sourcegitcommit: cdf5084648bf5e77970cbfeaa23f1cab3e6e234e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76921433"
---
# <a name="dead-letter-queues"></a>配信不能キュー
このサンプルでは、配信できなかったメッセージの処理方法を示します。 これは、トランザクション処理された[MSMQ バインディング](../../../../docs/framework/wcf/samples/transacted-msmq-binding.md)のサンプルに基づいています。 このサンプルでは、`netMsmqBinding` バインディングを使用します。 サービスは自己ホスト型コンソール アプリケーションであるので、キューに置かれたメッセージをサービスが受信するようすを観察できます。

> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。

> [!NOTE]
> このサンプルでは、Windows Vista でのみ使用できるアプリケーション配信不能キューを示します。 このサンプルは、Windows Server 2003 および Windows XP 上の MSMQ 3.0 の既定のシステム全体のキューを使用するように変更できます。

 キュー通信では、クライアントはサービスとの通信にキューを使用します。 厳密には、クライアントはメッセージをキューに送信します。 サービスは、メッセージをキューから受信します。 したがって、キューを使用する通信では、サービスとクライアントが同時に実行されていなくてもかまいません。

 キューを使用する通信では一定の活動停止状態が生じるので、メッセージの有効期間を設定して、有効期間経過後はメッセージをアプリケーションに配信しないようにすることが必要になる場合があります。 また、アプリケーションによっては、メッセージの配信が失敗したかどうかを通知する必要があります。 このような場合に、メッセージの有効期間が経過すると、あるいはメッセージの配信に失敗すると、メッセージは配信不能キューに置かれます。 このとき、送信元のアプリケーションは、配信不能キューに置かれたメッセージを読み取って是正措置を行います。是正措置とは、何も行わない場合から、配信失敗の原因を解消してメッセージを再送信する場合までさまざまです。

 `NetMsmqBinding` バインディングの配信不能キューは、次のプロパティとして表されます。

- <xref:System.ServiceModel.MsmqBindingBase.DeadLetterQueue%2A> プロパティは、クライアントが必要とする配信不能キューの種類を表します。 この列挙体には、次の値があります。

- `None` : クライアントは配信不能キューを必要としません。

- `System`: システムの配信不能キューを使用して配信不能メッセージを保存します。 システムの配信不能キューは、そのコンピューターで実行されているすべてのアプリケーションで共有されます。

- `Custom`: <xref:System.ServiceModel.MsmqBindingBase.CustomDeadLetterQueue%2A> プロパティで指定されるカスタムの配信不能キューを使用して配信不能メッセージを保存します。 この機能は、Windows Vista でのみ使用できます。 同じコンピューターで実行されている他のアプリケーションとキューを共有するのではなく、そのアプリケーション専用の配信不能キューが必要な場合に、この値を使用します。

- <xref:System.ServiceModel.MsmqBindingBase.CustomDeadLetterQueue%2A> プロパティは、配信不能キューとして使用される特定のキューを表します。 これは、Windows Vista でのみ使用できます。

 このサンプルでは、クライアントがトランザクションのスコープ内からメッセージをまとめてサービスに送信しますが、メッセージの "有効期間" には意図的に小さい値 (約 2 秒) を指定しています。 さらに、有効期限切れのメッセージを入れるために使用するカスタムの配信不能キューを指定します。

 クライアント アプリケーションは、配信不能キューからメッセージを読み取った後で、そのメッセージの再送信を試みるか、元のメッセージが配信不能となった原因のエラーを解消してメッセージを送信します。 このサンプルでは、クライアントはエラー メッセージを表示します。

 サービス コントラクトは `IOrderProcessor` です。次のサンプル コードを参照してください。

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true)]
    void SubmitPurchaseOrder(PurchaseOrder po);
}
```

 このサンプルのサービスコードは、トランザクション処理された[MSMQ バインド](../../../../docs/framework/wcf/samples/transacted-msmq-binding.md)のコードです。

 サービスとの通信はトランザクションのスコープ内で実行されます。 サービスはキューからメッセージを読み取って操作を実行し、操作の結果を表示します。 このアプリケーションでは、配信不能メッセージ用の配信不能キューも作成します。

```csharp
//The service contract is defined in generatedClient.cs, generated from the service by the svcutil tool.

//Client implementation code.
class Client
{
    static void Main()
    {
        // Get MSMQ queue name from app settings in configuration
        string deadLetterQueueName = ConfigurationManager.AppSettings["deadLetterQueueName"];

        // Create the transacted MSMQ queue for storing dead message if necessary.
        if (!MessageQueue.Exists(deadLetterQueueName))
            MessageQueue.Create(deadLetterQueueName, true);

        // Create a proxy with given client endpoint configuration
        OrderProcessorClient client = new OrderProcessorClient("OrderProcessorEndpoint");

        // Create the purchase order
        PurchaseOrder po = new PurchaseOrder();
        po.CustomerId = "somecustomer.com";
        po.PONumber = Guid.NewGuid().ToString();

        PurchaseOrderLineItem lineItem1 = new PurchaseOrderLineItem();
        lineItem1.ProductId = "Blue Widget";
        lineItem1.Quantity = 54;
        lineItem1.UnitCost = 29.99F;

        PurchaseOrderLineItem lineItem2 = new PurchaseOrderLineItem();
        lineItem2.ProductId = "Red Widget";
        lineItem2.Quantity = 890;
        lineItem2.UnitCost = 45.89F;

        po.orderLineItems = new PurchaseOrderLineItem[2];
        po.orderLineItems[0] = lineItem1;
        po.orderLineItems[1] = lineItem2;

        //Create a transaction scope.
        using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))
        {
            // Make a queued call to submit the purchase order
            client.SubmitPurchaseOrder(po);
            // Complete the transaction.
            scope.Complete();
        }

        client.Close();

        Console.WriteLine();
        Console.WriteLine("Press <ENTER> to terminate client.");
        Console.ReadLine();
    }
}
```

 クライアントの構成では、メッセージがサービスに到達するまでの時間が短く設定されています。 指定された時間内にメッセージを送信できなかった場合は、メッセージが有効期限切れとなり、配信不能キューに移動します。

> [!NOTE]
> クライアントがメッセージを指定時間内にサービス キューに配信できる場合もあります。 配信不能サービスが動作していることを確認するには、サービスを開始する前にクライアントを実行してみてください。 メッセージはタイムアウトして、配信不能サービスに配信されます。

 アプリケーションでは、どのキューを配信不能キューとして使用するかを定義する必要があります。 キューが指定されていない場合は、既定のシステム全体のトランザクション配信不能キューが使用されます。 この例のクライアント アプリケーションは、アプリケーション専用の配信不能キューを指定します。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <appSettings>
    <!-- use appSetting to configure MSMQ Dead Letter queue name -->
    <add key="deadLetterQueueName" value=".\private$\ServiceModelSamplesOrdersAppDLQ"/>
  </appSettings>

  <system.serviceModel>
    <client>
      <!-- Define NetMsmqEndpoint -->
      <endpoint name="OrderProcessorEndpoint"
                address="net.msmq://localhost/private/ServiceModelSamplesDeadLetter"
                binding="netMsmqBinding"
                bindingConfiguration="PerAppDLQBinding"
                contract="IOrderProcessor" />
    </client>

    <bindings>
      <netMsmqBinding>
        <binding name="PerAppDLQBinding"
                 deadLetterQueue="Custom"
                 customDeadLetterQueue="net.msmq://localhost/private/ServiceModelSamplesOrdersAppDLQ"
                 timeToLive="00:00:02"/>
      </netMsmqBinding>
    </bindings>
  </system.serviceModel>

</configuration>
```

 配信不能メッセージ サービスは、メッセージを配信不能キューから読み取ります。 この配信不能メッセージ サービスは、`IOrderProcessor` コントラクトを実装します。 ただし、この実装は注文を処理するためのものではありません。 配信不能メッセージ サービスはクライアント サービスの 1 つで、注文処理機能は含まれていません。

> [!NOTE]
> 配信不能キューはクライアント キューであり、クライアントのキュー マネージャに対してローカルです。

 配信不能メッセージ サービスの実装の中で、メッセージの配信に失敗した原因を調べて修正処理を実行します。 メッセージ配信失敗の原因は、<xref:System.ServiceModel.Channels.MsmqMessageProperty.DeliveryFailure%2A> と <xref:System.ServiceModel.Channels.MsmqMessageProperty.DeliveryStatus%2A> の 2 つの列挙体に記録されます。 <xref:System.ServiceModel.Channels.MsmqMessageProperty> は <xref:System.ServiceModel.OperationContext> から取得できます。次のサンプル コードを参照してください。

```csharp
public void SubmitPurchaseOrder(PurchaseOrder po)
{
    Console.WriteLine("Submitting purchase order did not succeed ", po);
    MsmqMessageProperty mqProp =
                  OperationContext.Current.IncomingMessageProperties[
                  MsmqMessageProperty.Name] as MsmqMessageProperty;
    Console.WriteLine("Message Delivery Status: {0} ",
                                                mqProp.DeliveryStatus);
    Console.WriteLine("Message Delivery Failure: {0}",
                                               mqProp.DeliveryFailure);
    Console.WriteLine();
    …
}
```

 配信不能キューの中のメッセージは、メッセージを処理するサービス宛てのメッセージです。 このため、配信不能メッセージサービスがキューからメッセージを読み取る場合、Windows Communication Foundation (WCF) チャネル層はエンドポイントで不一致を検出し、メッセージをディスパッチしません。 この場合、メッセージの宛先は注文処理サービスですが、受信するのは配信不能メッセージ サービスです。 別のエンドポイント宛てのメッセージを受信するには、どのアドレスにも一致するアドレス フィルタを `ServiceBehavior` で指定します。 これは、配信不能キューから読み取ったメッセージを正常に処理するために必要です。

 このサンプルでは、エラーの原因がメッセージのタイムアウトである場合に、配信不能メッセージサービスによってメッセージが再送信されます。その他のすべての理由により、次のサンプルコードに示すように、配信エラーが表示されます。

```csharp
// Service class that implements the service contract.
// Added code to write output to the console window.
[ServiceBehavior(InstanceContextMode=InstanceContextMode.Single, ConcurrencyMode=ConcurrencyMode.Single, AddressFilterMode=AddressFilterMode.Any)]
public class PurchaseOrderDLQService : IOrderProcessor
{
    OrderProcessorClient orderProcessorService;
    public PurchaseOrderDLQService()
    {
        orderProcessorService = new OrderProcessorClient("OrderProcessorEndpoint");
    }

    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    public void SubmitPurchaseOrder(PurchaseOrder po)
    {
        Console.WriteLine("Submitting purchase order did not succeed ", po);
        MsmqMessageProperty mqProp = OperationContext.Current.IncomingMessageProperties[MsmqMessageProperty.Name] as MsmqMessageProperty;

        Console.WriteLine("Message Delivery Status: {0} ", mqProp.DeliveryStatus);
        Console.WriteLine("Message Delivery Failure: {0}", mqProp.DeliveryFailure);
        Console.WriteLine();

        // resend the message if timed out
        if (mqProp.DeliveryFailure == DeliveryFailure.ReachQueueTimeout ||
            mqProp.DeliveryFailure == DeliveryFailure.ReceiveTimeout)
        {
            // re-send
            Console.WriteLine("Purchase order Time To Live expired");
            Console.WriteLine("Trying to resend the message");

            // reuse the same transaction used to read the message from dlq to enqueue the message to app. queue
            orderProcessorService.SubmitPurchaseOrder(po);
            Console.WriteLine("Purchase order resent");
        }
    }

    // Host the service within this EXE console application.
    public static void Main()
    {
        // Create a ServiceHost for the PurchaseOrderDLQService type.
        using (ServiceHost serviceHost = new ServiceHost(typeof(PurchaseOrderDLQService)))
        {
            // Open the ServiceHostBase to create listeners and start listening for messages.
            serviceHost.Open();

            // The service can now be accessed.
            Console.WriteLine("The dead letter service is ready.");
            Console.WriteLine("Press <ENTER> to terminate service.");
            Console.WriteLine();
            Console.ReadLine();
        }
    }
}
```

 配信不能メッセージ用の構成を次のサンプルに示します。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service
          name="Microsoft.ServiceModel.Samples.PurchaseOrderDLQService">
        <!-- Define NetMsmqEndpoint in this case, DLQ end point to read messages-->
        <endpoint address="net.msmq://localhost/private/ServiceModelSamplesOrdersAppDLQ"
                  binding="netMsmqBinding"
                  bindingConfiguration="DefaultBinding"
                  contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />
      </service>
    </services>

    <client>
      <!-- Define NetMsmqEndpoint -->
      <endpoint name="OrderProcessorEndpoint"
                 address="net.msmq://localhost/private/ServiceModelSamplesDeadLetter"
                 binding="netMsmqBinding"
                 bindingConfiguration="SystemDLQBinding"
                 contract="IOrderProcessor" />
    </client>

    <bindings>
      <netMsmqBinding>
        <binding name="DefaultBinding" />
        <binding name="SystemDLQBinding"
                 deadLetterQueue="System"/>
      </netMsmqBinding>
    </bindings>
  </system.serviceModel>
</configuration>
```

 このサンプルを実行するときは、3 つの実行可能ファイル (クライアント、サービス、および各アプリケーションの配信不能キューを読み取ってメッセージをサービスに再送信する配信不能サービス) が実行され、各アプリケーションに対して配信不能キューがどのように機能するかがわかります。 これらはすべてコンソール アプリケーションで、コンソール ウィンドウに出力が表示されます。

> [!NOTE]
> キューが使用されているので、クライアントとサービスが同時に実行されている必要はありません。 クライアントを実行してシャットダウンした後にサービスを起動しても、サービスはメッセージを受信します。 キューが作成されるように、サービスを起動してシャットダウンする必要があります。

 クライアントを実行すると、次のメッセージが表示されます。

```console
Press <ENTER> to terminate client.
```

 クライアントはメッセージの送信を試みますが、タイムアウトまでの時間が短いのでメッセージは期限切れとなり、メッセージが各アプリケーションの配信不能キューに置かれます。

 次に、配信不能サービスを実行すると、メッセージが読み取られてエラー コードが表示され、メッセージがサービスに再送信されます。

```console
The dead letter service is ready.
Press <ENTER> to terminate service.

Submitting purchase order did not succeed
Message Delivery Status: InDoubt
Message Delivery Failure: ReachQueueTimeout

Purchase order Time To Live expired
Trying to resend the message
Purchase order resent
```

 サービスを起動すると、再送信されたメッセージが読み取られて処理されます。

```console
The service is ready.
Press <ENTER> to terminate service.

Processing Purchase Order: 97897eff-f926-4057-a32b-af8fb11b9bf9
        Customer: somecustomer.com
        OrderDetails
                Order LineItem: 54 of Blue Widget @unit price: $29.99
                Order LineItem: 890 of Red Widget @unit price: $45.89
        Total cost of this order: $42461.56
        Order status: Pending
```

### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

2. サービスを最初に実行すると、サービスはキューが存在するかどうかを確認します。 キューが存在しない場合、サービスによってキューが作成されます。 最初にサービスを実行してキューを作成することも、MSMQ キュー マネージャーでキューを作成することもできます。 Windows 2008 でキューを作成するには、次の手順に従います。

    1. Visual Studio 2012 でサーバーマネージャーを開きます。

    2. **[機能]** タブを展開します。

    3. **[プライベートメッセージキュー]** を右クリックし、 **[新規]** 、 **[プライベートキュー]** の順に選択します。

    4. **[トランザクション]** ボックスをオンにします。

    5. 新しいキューの名前として `ServiceModelSamplesTransacted` を入力します。

3. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。

4. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、キュー名を適切に変更し、localhost をコンピューターの完全な名前に置き換えて、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。

### <a name="to-run-the-sample-on-a-computer-joined-to-a-workgroup"></a>ワークグループに参加しているコンピューターでこのサンプルを実行するには

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

     エンドポイントの `bindingConfiguration` 属性が設定されて、エンドポイントとバインディングとが関連付けられていることを確認します。

2. DeadLetterService、サーバー、およびクライアントの構成を変更したことを確認してから、サンプルを実行します。

    > [!NOTE]
    > `security mode` を `None` に設定することは、`MsmqAuthenticationMode`、`MsmqProtectionLevel`、および `Message` のセキュリティを `None` に設定することに相当します。

## <a name="comments"></a>コメント
 `netMsmqBinding` バインディング トランスポートを使用する場合の既定では、セキュリティが有効です。 トランスポート セキュリティの種類は、`MsmqAuthenticationMode` と `MsmqProtectionLevel` の 2 つのプロパティで決まります。 既定の設定では、認証モードは `Windows`、保護レベルは `Sign` です。 MSMQ の認証および署名の機能を利用するには、ドメインに属している必要があります。 ドメインに属していないコンピューターでこのサンプルを実行すると、"User's internal message queuing certificate does not exist" というエラーが表示されます。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\MSMQ\DeadLetter`  
