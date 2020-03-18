---
title: MSMQ アクティベーション
ms.date: 03/30/2017
ms.assetid: e3834149-7b8c-4a54-806b-b4296720f31d
ms.openlocfilehash: 805ab78908b4d1146cce94cac5357bafbb35c832
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744791"
---
# <a name="msmq-activation"></a>MSMQ アクティベーション

このサンプルでは、メッセージ キューから読み取ったアプリケーションを、Windows プロセス アクティブ化サービス (WAS) でホストする方法を示します。 このサンプルでは、`netMsmqBinding` を使用し、[双方向の通信](../../../../docs/framework/wcf/samples/two-way-communication.md)のサンプルに基づいています。 この場合、サービスは Web ホスト アプリケーションの 1 つであり、クライアントは自己ホスト型です。クライアントはコンソールに出力して、送信された発注書のステータスを確認します。

> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。

> [!NOTE]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> \<InstallDrive >:\ WF_WCF_Samples
>
> このディレクトリが存在しない場合は、 [.NET Framework 4 の Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプル](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WCF と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] のサンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> \<InstallDrive >: \Samples\WCFWFCardSpace\WCF\Basic\Services\Hosting\WASHost\MsmqActivation.

Windows プロセスアクティブ化サービス (WAS) は、Windows Server 2008 用の新しいプロセスアクティブ化機構であり、以前は HTTP ベースのアプリケーションのみが http 以外のプロトコルを使用するアプリケーションに対して使用できるようになった IIS に似た機能を提供しています。 Windows Communication Foundation (WCF) は、リスナーアダプターインターフェイスを使用して、WCF でサポートされている HTTP 以外のプロトコル (TCP、名前付きパイプ、MSMQ など) を介して受信されるアクティブ化要求を伝達します。 HTTP 以外のプロトコルを介して要求を受信する機能は、SMSvcHost.exe で実行されるマネージド Windows サービスによってホストされます。

Net.Msmq リスナ アダプタ サービス (NetMsmqActivator) は、キュー内のメッセージに基づいてキューに置かれたアプリケーションをアクティブ化します。

クライアントはトランザクションのスコープ内から発注書をサービスに送信します。 サービスはトランザクション内で注文を受信して、処理します。 その後、サービスは注文ステータスをクライアントに返信します。 双方向通信を使用するには、クライアントとサービスの両方がキューを使用して、発注書や注文ステータスをキューに追加する必要があります。

`IOrderProcessor` サービス コントラクトは、キューを使用する一方向サービス操作を定義します。 サービス操作は、注文ステータスをクライアントに送信するために応答エンドポイントを使用します。 応答エンドポイントのアドレスは、注文ステータスをクライアントに返信する際に使用されるキューの URI です。 注文処理アプリケーションは、このコントラクトを実装します。

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true)]
    void SubmitPurchaseOrder(PurchaseOrder po,
                                           string reportOrderStatusTo);
}
```

注文ステータスを送信する応答コントラクトは、クライアントが指定します。 クライアントは注文ステータス コントラクトを実装します。 サービスは、このコントラクトについて生成されたクライアントを使用して、注文ステータスをクライアントに返信します。

```csharp
[ServiceContract]
public interface IOrderStatus
{
    [OperationContract(IsOneWay = true)]
    void OrderStatus(string poNumber, string status);
}
```

サービス操作は、送信された発注書を処理します。 <xref:System.ServiceModel.OperationBehaviorAttribute> はサービス操作に適用され、キューからのメッセージの受信に使用されるトランザクション内で登録リストを自動で作成し、サービス操作が完了したときにトランザクションを自動で完了するように指定します。 `Orders` クラスは、注文処理機能をカプセル化します。 この例では、発注書をディクショナリに追加します。 このサービス操作が帰属するしているトランザクションは、`Orders` クラス内の操作で使用できます。

サービス操作は、送信された発注書を処理するだけでなく、注文ステータスをクライアントに戻します。

```csharp
public class OrderProcessorService : IOrderProcessor
{
    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    public void SubmitPurchaseOrder(PurchaseOrder po, string reportOrderStatusTo)
    {
        Orders.Add(po);
        Console.WriteLine("Processing {0} ", po);
        Console.WriteLine("Sending back order status information");
        NetMsmqBinding msmqCallbackBinding = new NetMsmqBinding();
        msmqCallbackBinding.Security.Mode = NetMsmqSecurityMode.None;
        OrderStatusClient client = new OrderStatusClient(msmqCallbackBinding, new EndpointAddress(reportOrderStatusTo));
        // please note that the same transaction that is used to dequeue purchase order is used
        // to send back order status
        using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))
        {
            client.OrderStatus(po.PONumber, po.Status);
            scope.Complete();
        }
    }
}
```

使用するクライアント バインディングの指定には、構成ファイルを使用します。

MSMQ キュー名は、構成ファイルの appSettings セクションで指定されます。 サービスのエンドポイントは、構成ファイルの System.serviceModel セクションで定義されます。

> [!NOTE]
> MSMQ のキュー名とエンドポイント アドレスでは、若干異なるアドレス表記が使用されています。 MSMQ のキュー名では、ドット (.) を使用してローカル コンピューターを表し、バックスラッシュを使用してパスを区切ります。 WCF エンドポイントアドレスは、net.tcp: scheme を指定し、ローカルコンピューターに "localhost" を使用して、パスにスラッシュを使用します。 リモート コンピューターでホストされているキューからの読み出しを行うには、"." や "localhost" をリモート コンピューター名に置き換えます。

クラスの名前が付いた .svc ファイルは、WAS でのサービス コードのホストに使用されます。

Service.svc ファイル自体には、`OrderProcessorService` を作成するディレクティブが含まれます。

`<%@ServiceHost language="c#" Debug="true" Service="Microsoft.ServiceModel.Samples.OrderProcessorService"%>`

さらに、Service.svc ファイルには、System.Transactions.dll を読み込むためのアセンブリ ディレクティブも含まれます。

`<%@Assembly name="System.Transactions, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"%>`

クライアントはトランザクション スコープを作成します。 サービスとの通信はトランザクションのスコープ内で実行され、すべてのメッセージが成功か失敗かを示すアトミックな単位として扱われます。 トランザクションは、トランザクション スコープで `Complete` を呼び出すことでコミットされます。

```csharp
using (ServiceHost serviceHost = new ServiceHost(typeof(OrderStatusService)))
{
    // Open the ServiceHostBase to create listeners and start listening
    // for order status messages.
    serviceHost.Open();

    // Create a proxy with given client endpoint configuration
    OrderProcessorClient client = new OrderProcessorClient();

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
    using (TransactionScope scope = new
        TransactionScope(TransactionScopeOption.Required))
    {
        // Make a queued call to submit the purchase order
        client.SubmitPurchaseOrder(po,
       "net.msmq://localhost/private/ServiceModelSamplesOrder/OrderStatus");
        // Complete the transaction.
        scope.Complete();
    }

    //Closing the client gracefully closes the connection and cleans up
    //resources
    client.Close();

    Console.WriteLine();
    Console.WriteLine("Press <ENTER> to terminate client.");
    Console.ReadLine();

    // Close the ServiceHostBase to shutdown the service.
    serviceHost.Close();
    }
```

クライアント コードは `IOrderStatus` コントラクトを実装して、サービスからの注文ステータスを受信します。 この場合は、注文ステータスを出力します。

```csharp
[ServiceBehavior]
public class OrderStatusService : IOrderStatus
{
    [OperationBehavior(TransactionAutoComplete = true,
                        TransactionScopeRequired = true)]
    public void OrderStatus(string poNumber, string status)
    {
        Console.WriteLine("Status of order {0}:{1} ",
                                         poNumber , status);
    }
}
```

注文ステータス キューは `Main` メソッド内で作成します。 クライアント構成には、注文ステータス サービスをホストするための注文ステータス サービス構成が含まれています。次のサンプル構成を参照してください。

```xml
<appSettings>
    <!-- use appSetting to configure MSMQ queue name -->
    <add key="targetQueueName" value=".\private$\ServiceModelSamples/service.svc" />
    <add key="responseQueueName" value=".\private$\ServiceModelSamples/OrderStatus" />
  </appSettings>

<system.serviceModel>

    <services>
      <service
         name="Microsoft.ServiceModel.Samples.OrderStatusService">
        <!-- Define NetMsmqEndpoint -->
        <endpoint address="net.msmq://localhost/private/ServiceModelSamples/OrderStatus"
                  binding="netMsmqBinding"
                  contract="Microsoft.ServiceModel.Samples.IOrderStatus" />
      </service>
    </services>

    <client>
      <!-- Define NetMsmqEndpoint -->
      <endpoint name="OrderProcessorEndpoint"
                address="net.msmq://localhost/private/ServiceModelSamples/service.svc"
                binding="netMsmqBinding"
                contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />
    </client>

  </system.serviceModel>
```

サンプルを実行すると、クライアントとサービスのアクティビティがサーバーとクライアントの両方のコンソール ウィンドウに表示されます。 サーバーがクライアントから受信したメッセージを表示できます。 どちらかのコンソールで Enter キーを押すと、サーバーとクライアントがどちらもシャットダウンされます。

クライアントには、サーバーから送信された注文ステータス情報が表示されます。

```console
Press <ENTER> to terminate client.
Status of order 70cf9d63-3dfa-4e69-81c2-23aa4478ebed :Pending
```

### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. WAS のアクティブ化に必要な IIS 7.0 がインストールされていることを確認します。

2. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。 さらに、WCF 非 HTTP アクティブ化コンポーネントをインストールする必要があります。

    1. **[スタート]** メニューの **[コントロール パネル]** をクリックします。

    2. **[プログラムと機能]** を選択します。

    3. [ **Windows の機能の有効化または無効化] を**クリックします。

    4. **[機能の概要]** で、 **[機能の追加]** をクリックします。

    5. **[Microsoft .NET Framework 3.0]** ノードを展開し、 **[WINDOWS COMMUNICATION FOUNDATION の非 HTTP アクティブ化]** 機能をオンにします。

3. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。

4. コマンド ウィンドウで client.exe を実行することにより、クライアントを実行します。 これによってキューが作成され、メッセージがそのキューに送信されます。 クライアントを実行しながら、メッセージを読み取るサービスの結果を表示します。

5. MSMQ アクティベーション サービスは、既定では NETWORK SERVICE として動作します。 そのため、アプリケーションのアクティブ化に使用されるキューには、NETWORK SERVICE アカウントによる受信およびピーク権限が必要です。 この権限は、メッセージ キュー MMC を使用して追加できます。

    1. **[スタート]** メニューの **[実行]** をクリックし、「`Compmgmt.msc`」と入力して、enter キーを押します。

    2. **[サービスとアプリケーション]** で、 **[メッセージキュー]** を展開します。

    3. **[専用キュー]** をクリックします。

    4. キュー (servicemodelsamples/Service .svc) を右クリックし、 **[プロパティ]** を選択します。

    5. **セキュリティ** タブで **追加** をクリックし、ネットワークサービスに対して ピーク および 受信 アクセス許可を付与します。

6. MSMQ アクティブ化をサポートするよう Windows プロセス アクティブ化サービス (WAS) を設定します。

    便宜上次の 2 つの手順が、サンプル ディレクトリにある AddMsmqSiteBinding.cmd というバッチ ファイルに実装されています。

    1. net.msmq アクティベーションをサポートするには、既定の Web サイトをあらかじめ net.msmq プロトコルにバインドしておく必要があります。 これは、IIS7.0 管理ツール セットと共にインストールされる appcmd.exe を使用して行います。 権限のレベルが高い (管理者の) コマンド プロンプトで、次のコマンドを実行します。

        ```console
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"
        -+bindings.[protocol='net.msmq',bindingInformation='localhost']
        ```

        > [!NOTE]
        > このコマンドはテキスト 1 行です。

        このコマンドによって、既定の Web サイトに net.msmq サイト バインディングを追加します。

    2. サイト内のすべてのアプリケーションが同じ net.msmq バインディングを共有しますが、net.msmq サポートの有効化はアプリケーションごとに指定できます。 /servicemodelsamples アプリケーションで net.msmq を有効にするには、権限のレベルが高いコマンド プロンプトから、次のコマンドを実行します。

        ```console
        %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples" /enabledProtocols:http,net.msmq
        ```

        > [!NOTE]
        > このコマンドはテキスト 1 行です。

        このコマンドにより、`http://localhost/servicemodelsamples` と `net.msmq://localhost/servicemodelsamples`を使用して/servicemodelsamples アプリケーションにアクセスできるようになります。

7. まだ確認していない場合は、MSMQ アクティベーション サービスが有効になっていることを確認します。 **[スタート]** メニューの **[実行]** をクリックし、「`Services.msc`」と入力します。 サービスの一覧で、 **Net.tcp リスナーアダプター**を検索します。 右クリックし、 **[プロパティ]** を選択します。 **[スタートアップの種類]** を **[自動]** に設定し、 **[適用]** をクリックして、 **[開始]** ボタンをクリックします。 この手順は、Net.Msmq リスナー アダプター サービスを初めて使用する前に 1 回だけ実行する必要があります。

8. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。 さらに、発注書を送信するときのキューの URI 内のコンピューター名を反映するように、発注書を送信するクライアントのコードを変更します。 次のコードを使用します。

    ```csharp
    client.SubmitPurchaseOrder(po, "net.msmq://localhost/private/ServiceModelSamples/OrderStatus");
    ```

9. このサンプル用に追加した net.msmq サイト バインディングを削除します。

    便宜上次の 2 つの手順が、サンプル ディレクトリにある RemoveMsmqSiteBinding.cmd というバッチ ファイルに実装されています。

    1. 権限のレベルが高いコマンド プロンプトから次のコマンドを実行して、有効なプロトコルの一覧から net.msmq を削除します。

        ```console
        %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples" /enabledProtocols:http
        ```

        > [!NOTE]
        > このコマンドはテキスト 1 行です。

    2. 権限のレベルが高いコマンド プロンプトから次のコマンドを実行して、net.msmq サイト バインディングを削除します。

        ```console
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site" --bindings.[protocol='net.msmq',bindingInformation='localhost']
        ```

        > [!NOTE]
        > このコマンドはテキスト 1 行です。

    > [!WARNING]
    > バッチ ファイルを実行すると、DefaultAppPool がリセットされて .NET Framework Version 2.0 で実行されます。

`netMsmqBinding` バインディング トランスポートを使用する場合の既定では、セキュリティが有効です。 トランスポート セキュリティの種類は、`MsmqAuthenticationMode` と `MsmqProtectionLevel` の 2 つのプロパティで決まります。 既定の設定では、認証モードは `Windows`、保護レベルは `Sign` です。 MSMQ の認証および署名の機能を利用するには、ドメインに属している必要があります。 このサンプルをドメインに属していないコンピューターで実行すると、"ユーザーの内部メッセージ キュー認証情報は存在しません" というエラーが表示されます。

### <a name="to-run-the-sample-on-a-computer-joined-to-a-workgroup"></a>ワークグループに参加しているコンピューターでこのサンプルを実行するには

1. ドメインに属していないコンピューターを使用する場合は、トランスポート セキュリティをオフにします。オフにするには、認証モードと保護レベルを "none" に設定します。サンプル構成を次に示します。

    ```xml
    <bindings>
        <netMsmqBinding>
            <binding configurationName="TransactedBinding">
                <security mode="None"/>
            </binding>
        </netMsmqBinding>
    </bindings>
    ```

2. サンプルを実行する前に、サーバーとクライアントの両方の構成を変更します。

    > [!NOTE]
    > `security mode` を `None` に設定することは、`MsmqAuthenticationMode`、`MsmqProtectionLevel`、および `Message` のセキュリティを `None` に設定することに相当します。

3. ワークグループに参加しているコンピューターでアクティブ化を有効にするには、アクティベーション サービスとワーカー プロセスの両方を特定のユーザー アカウントで実行する必要があります (どちらも同じアカウントを使用する必要があります)。さらに、キューはその特定のユーザー アカウントの ACL に設定されている必要があります。

     ワーカー プロセスが実行される ID を変更するには

    1. Inetmgr.exe を実行します。

    2. **[アプリケーションプール]** で、 **AppPool** (通常は**DefaultAppPool**) を右クリックし、 **[アプリケーションプールの既定値の設定]** を選択します。

    3. 特定のユーザー アカウントを使用するように、[ID] プロパティを変更します。

     アクティブ化サービスが実行される ID を変更するには

    1. Services.msc を実行します。

    2. **Net.tcp リスナーアダプター**を右クリックし、 **[プロパティ]** を選択します。

4. **[ログオン]** タブでアカウントを変更します。

5. ワークグループでは、無制限のトークンを使用してサービスを実行する必要もあります。 この操作を行うには、コマンド ウィンドウから次のコマンドを実行します。

    ```console
    sc sidtype netmsmqactivator unrestricted
    ```

## <a name="see-also"></a>参照

- [AppFabric のホスティングと永続化のサンプル](https://docs.microsoft.com/previous-versions/appfabric/ff383418(v=azure.10))
