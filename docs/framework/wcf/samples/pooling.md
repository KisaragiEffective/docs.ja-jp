---
title: Pooling
ms.date: 03/30/2017
ms.assetid: 688dfb30-b79a-4cad-a687-8302f8a9ad6a
ms.openlocfilehash: d2962004376cf6f0752067d4e03828cd894efd01
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74716517"
---
# <a name="pooling"></a>Pooling
このサンプルでは、Windows Communication Foundation (WCF) を拡張してオブジェクトプールをサポートする方法を示します。 サンプルでは、エンタープライズ サービスの`ObjectPoolingAttribute` 属性機能と、構文および意味が同じ属性を作成する方法を示します。 オブジェクト プールにより、アプリケーションのパフォーマンスが大幅に向上します。 ただし、適切に使用しないと逆効果になる場合があります。 オブジェクト プールは、負荷のかかる初期化が要求される、使用頻度の高いオブジェクトの再作成によるオーバーヘッドを減少させます。 ただし、プールされたオブジェクト上のメソッドへの呼び出しが完了するのに非常に時間がかかる場合、オブジェクト プールは、最大プール サイズに達するとすぐに追加要求をキューに置きます。 そのため、タイムアウト例外がスローされることによって、いくつかのオブジェクトの作成要求が失敗する場合があります。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
 WCF 拡張機能を作成するための最初の手順は、使用する機能拡張ポイントを決定することです。  
  
 WCF では、*ディスパッチャー*とは、受信メッセージをユーザーのサービスのメソッド呼び出しに変換し、そのメソッドの戻り値を送信メッセージに変換する実行時コンポーネントを指します。 WCF サービスによって、各エンドポイントのディスパッチャーが作成されます。 クライアントに関連付けられたコントラクトが双方向コントラクトの場合、WCF クライアントは dispatcher を使用する必要があります。  
  
 チャネル ディスパッチャとエンドポイント ディスパッチャでは、ディスパッチャの動作を制御するさまざまなプロパティが公開されているため、チャネル全体の拡張とコントラクト全体の拡張を行うことができます。 さらに <xref:System.ServiceModel.Dispatcher.EndpointDispatcher.DispatchRuntime%2A> プロパティにより、ディスパッチ処理を検査、変更、またはカスタマイズすることもできます。 このサンプルでは、サービス クラスのインスタンスを提供するオブジェクトをポイントする <xref:System.ServiceModel.Dispatcher.DispatchRuntime.InstanceProvider%2A> プロパティに焦点を当てています。  
  
## <a name="the-iinstanceprovider"></a>IInstanceProvider  
 WCF では、ディスパッチャーは、<xref:System.ServiceModel.Dispatcher.IInstanceProvider> インターフェイスを実装する <xref:System.ServiceModel.Dispatcher.DispatchRuntime.InstanceProvider%2A>を使用して、サービスクラスのインスタンスを作成します。 このインターフェイスには、次の 3 つのメソッドが含まれています。  
  
- <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%2CSystem.ServiceModel.Channels.Message%29>: メッセージが到着すると、このディスパッチャは <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%2CSystem.ServiceModel.Channels.Message%29> メソッドを呼び出し、メッセージを処理するためのサービス クラスのインスタンスを作成します。 このメソッドの呼び出し頻度は <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> プロパティで決まります。 たとえば <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> プロパティが <xref:System.ServiceModel.InstanceContextMode.PerCall> に設定されている場合、サービス クラスの新しいインスタンスが作成され、到着する各メッセージが処理されます。したがって、<xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%2CSystem.ServiceModel.Channels.Message%29> はメッセージが到着するたびに呼び出されます。  
  
- <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%29>: 前のメソッドと同じです。ただし、このメソッドが呼び出されるのは Message 引数がない場合です。  
  
- <xref:System.ServiceModel.Dispatcher.IInstanceProvider.ReleaseInstance%28System.ServiceModel.InstanceContext%2CSystem.Object%29>: サービス インスタンスの有効期間が経過すると、ディスパッチャは <xref:System.ServiceModel.Dispatcher.IInstanceProvider.ReleaseInstance%28System.ServiceModel.InstanceContext%2CSystem.Object%29> メソッドを呼び出します。 <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%2CSystem.ServiceModel.Channels.Message%29> メソッドと同様、このメソッドへの呼び出し頻度は <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> プロパティで決まります。  
  
## <a name="the-object-pool"></a>オブジェクト プール  
 カスタム <xref:System.ServiceModel.Dispatcher.IInstanceProvider> の実装により、サービスに必要なオブジェクト プールの意味が提供されます。 したがって、このサンプルにはプール用の `ObjectPoolingInstanceProvider` のカスタム実装を提供する <xref:System.ServiceModel.Dispatcher.IInstanceProvider> 型が用意されています。 `Dispatcher` が、新しいインスタンスを作成する代わりに <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%2CSystem.ServiceModel.Channels.Message%29> メソッドを呼び出すと、カスタム実装はメモリ内プールで既存のオブジェクトを検索します。 検索されたオブジェクトが使用可能な場合は、そのオブジェクトが返されます。 それ以外の場合は、新しいオブジェクトが作成されます。 `GetInstance` の実装を次のサンプル コードに示します。  
  
```csharp  
object IInstanceProvider.GetInstance(InstanceContext instanceContext, Message message)  
{  
    object obj = null;  
  
    lock (poolLock)  
    {  
        if (pool.Count > 0)  
        {  
            obj = pool.Pop();  
        }  
        else  
        {  
            obj = CreateNewPoolObject();  
        }  
        activeObjectsCount++;  
    }  
  
    WritePoolMessage(ResourceHelper.GetString("MsgNewObject"));  
  
    idleTimer.Stop();  
  
    return obj;            
}  
```  
  
 カスタム `ReleaseInstance` 実装は、解放されたインスタンスをプールに戻し、`ActiveObjectsCount` 値をデクリメントします。 `Dispatcher` はこれらのメソッドをさまざまなスレッドから呼び出すので、`ObjectPoolingInstanceProvider` クラスのクラス レベル メンバーへの同期アクセスが必要となります。  
  
```csharp  
void IInstanceProvider.ReleaseInstance(InstanceContext instanceContext, object instance)  
{  
    lock (poolLock)  
    {  
        pool.Push(instance);  
        activeObjectsCount--;  
  
        WritePoolMessage(  
        ResourceHelper.GetString("MsgObjectPooled"));  
  
        // When the service goes completely idle (no requests   
        // are being processed), the idle timer is started  
        if (activeObjectsCount == 0)  
            idleTimer.Start();                       
    }  
}  
```  
  
 `ReleaseInstance` メソッドは、"初期化のクリーンアップ" 機能を提供します。 通常は、プールにはその有効期間中に最小限の数のオブジェクトが保持されます。 ただし、使用率が非常に高くなり、プールでオブジェクトを追加作成する必要が生じて、その数が構成に指定されている上限に達する可能性があります。 プールが最終的にアクティブでなくなると、そうした過剰なオブジェクトは余分なオーバーヘッドになります。 したがって、`activeObjectsCount` がゼロに達すると、アイドル タイマが起動し、クリーンアップ サイクルがトリガされて実行されます。  
  
## <a name="adding-the-behavior"></a>動作の追加  
 ディスパッチャのレイヤ拡張は、次の動作を使用してフックされます。  
  
- サービスの動作。 サービス ランタイム全体のカスタマイズを実現します。  
  
- エンドポイントの動作。 サービス エンドポイントのカスタマイズ、特にチャネル ディスパッチャとエンドポイント ディスパッチャのカスタマイズを実現します。  
  
- コントラクトの動作。 クライアント上およびサービス上で、それぞれ <xref:System.ServiceModel.Dispatcher.ClientRuntime> クラスと <xref:System.ServiceModel.Dispatcher.DispatchRuntime> クラスのカスタマイズを実現します。  
  
 オブジェクト プール拡張を行うには、サービスの動作を作成する必要があります。 サービス動作を作成するには、<xref:System.ServiceModel.Description.IServiceBehavior> インターフェイスを実装します。 サービス モデルにカスタム動作を認識させるには、次のようないくつかの方法があります。  
  
- カスタム属性を使用する。  
  
- カスタム動作をサービス説明の動作コレクションに強制的に追加する。  
  
- 構成ファイルを拡張する。  
  
 このサンプルではカスタム属性を使用します。 <xref:System.ServiceModel.ServiceHost> が構築されると、サービスの種類の定義で使用されている属性が調べられ、使用可能な動作がサービス説明の動作コレクションに追加されます。  
  
 インターフェイス <xref:System.ServiceModel.Description.IServiceBehavior> には、<xref:System.ServiceModel.Description.IServiceBehavior.Validate%2A>、<xref:System.ServiceModel.Description.IServiceBehavior.AddBindingParameters%2A>、および <xref:System.ServiceModel.Description.IServiceBehavior.ApplyDispatchBehavior%2A> の 3 つのメソッドがあります。 <xref:System.ServiceModel.Description.IServiceBehavior.Validate%2A> メソッドを使用すると、確実に動作をサービスに適用できます。 このサンプルでは、これを実装することによって、サービスが <xref:System.ServiceModel.InstanceContextMode.Single> を使用して構成されないようにします。 <xref:System.ServiceModel.Description.IServiceBehavior.AddBindingParameters%2A> メソッドは、サービスのバインディングの構成に使用されます。 このシナリオでは、このメソッドは必要ありません。 <xref:System.ServiceModel.Description.IServiceBehavior.ApplyDispatchBehavior%2A> はサービスのディスパッチャの構成に使用されます。 このメソッドは、<xref:System.ServiceModel.ServiceHost> の初期化中に WCF によって呼び出されます。 このメソッドには次のパラメータが渡されます。  
  
- `Description`: この引数は、サービス全体のサービスの説明を提供します。 これを使用すると、サービスのエンドポイント、コントラクト、バインディング、およびその他のデータに関する説明データを検査できます。  
  
- `ServiceHostBase`: この引数は、現在初期化中の <xref:System.ServiceModel.ServiceHostBase> を提供します。  
  
 カスタム <xref:System.ServiceModel.Description.IServiceBehavior> 実装では、`ObjectPoolingInstanceProvider` の新しいインスタンスがインスタンス化され、ServiceHostBase の各 <xref:System.ServiceModel.Dispatcher.DispatchRuntime.InstanceProvider%2A> 内の <xref:System.ServiceModel.Dispatcher.DispatchRuntime> プロパティに割り当てられます。  
  
```csharp  
void IServiceBehavior.ApplyDispatchBehavior(ServiceDescription description, ServiceHostBase serviceHostBase)  
{  
    // Create an instance of the ObjectPoolInstanceProvider.  
    ObjectPoolingInstanceProvider instanceProvider = new  
           ObjectPoolingInstanceProvider(description.ServiceType,   
                                                    minPoolSize);  
  
    // Forward the call if we created a ServiceThrottlingBehavior.  
    if (this.throttlingBehavior != null)  
    {  
        ((IServiceBehavior)this.throttlingBehavior).ApplyDispatchBehavior(description, serviceHostBase);  
    }  
  
    // In case there was already a ServiceThrottlingBehavior   
    // (this.throttlingBehavior==null), it should have initialized   
    // a single ServiceThrottle on all ChannelDispatchers.    
    // As we loop through the ChannelDispatchers, we verify that   
    // and modify the ServiceThrottle to guard MaxPoolSize.  
    ServiceThrottle throttle = null;  
  
    foreach (ChannelDispatcherBase cdb in   
            serviceHostBase.ChannelDispatchers)  
    {  
        ChannelDispatcher cd = cdb as ChannelDispatcher;  
        if (cd != null)  
        {  
            // Make sure there is exactly one throttle used by all   
            // endpoints. If there were others, we could not enforce   
            // MaxPoolSize.  
            if ((this.throttlingBehavior == null) &&   
                        (this.maxPoolSize != Int32.MaxValue))  
            {  
                throttle ??= cd.ServiceThrottle;
                if (cd.ServiceThrottle == null)  
                {  
                    throw new   
InvalidOperationException(ResourceHelper.GetString("ExNullThrottle"));  
                }  
                if (throttle != cd.ServiceThrottle)  
                {  
                    throw new InvalidOperationException(ResourceHelper.GetString("ExDifferentThrottle"));  
                }  
             }  
  
             foreach (EndpointDispatcher ed in cd.Endpoints)  
             {  
                 // Assign it to DispatchBehavior in each endpoint.  
                 ed.DispatchRuntime.InstanceProvider =   
                                      instanceProvider;  
             }  
         }  
     }  
  
     // Set the MaxConcurrentInstances to limit the number of items   
     // that will ever be requested from the pool.  
     if ((throttle != null) && (throttle.MaxConcurrentInstances >   
                                      this.maxPoolSize))  
     {  
         throttle.MaxConcurrentInstances = this.maxPoolSize;  
     }  
}  
```  
  
 <xref:System.ServiceModel.Description.IServiceBehavior> 実装のほかにも、<xref:System.EnterpriseServices.ObjectPoolingAttribute> クラスには属性引数を使用してオブジェクト プールをカスタマイズするいくつかのメンバがあります。 こうしたメンバには <xref:System.EnterpriseServices.ObjectPoolingAttribute.MaxPoolSize%2A>、<xref:System.EnterpriseServices.ObjectPoolingAttribute.MinPoolSize%2A>、<xref:System.EnterpriseServices.ObjectPoolingAttribute.CreationTimeout%2A> などがあり、.NET Enterprise Services で提供されるオブジェクト プール機能のセットに一致します。  
  
 新しく作成されたカスタム `ObjectPooling` 属性を使用してサービス実装に注釈を付けることにより、オブジェクトプール動作を WCF サービスに追加できるようになりました。  
  
```csharp  
[ObjectPooling(MaxPoolSize=1024, MinPoolSize=10, CreationTimeout=30000)]      
public class PoolService : IPoolService  
{  
  // …  
}  
```  
  
## <a name="running-the-sample"></a>サンプルの実行  
 このサンプルでは、特定のシナリオでオブジェクト プールを使用することによって得られるパフォーマンス上の利点を示します。  
  
 サービス アプリケーションは、`WorkService` と `ObjectPooledWorkService` の 2 つのサービスを実装します。 どちらのサービスも同じ実装を共有し、負荷のかかる初期化を必要とします。さらに、比較的負荷の少ない `DoWork()` メソッドを公開します。 両者の唯一の違いは、`ObjectPooledWorkService` ではオブジェクト プールが次のように構成されるという点です。  
  
```csharp  
[ObjectPooling(MinPoolSize = 0, MaxPoolSize = 5)]  
public class ObjectPooledWorkService : IDoWork  
{  
    public ObjectPooledWorkService()  
    {  
        Thread.Sleep(5000);  
        ColorConsole.WriteLine(ConsoleColor.Blue, "ObjectPooledWorkService instance created.");  
    }  
  
    public void DoWork()  
    {  
        ColorConsole.WriteLine(ConsoleColor.Blue, "ObjectPooledWorkService.GetData() completed.");  
    }          
}  
```  
  
 クライアントを実行すると、`WorkService` への呼び出し時間が 5 回計算されます。 次に、`ObjectPooledWorkService` への呼び出し時間が 5 回計算されます。 その後、時間の違いが次のように表示されます。  
  
```console
Press <ENTER> to start the client.  
  
Calling WorkService:  
1 - DoWork() Done  
2 - DoWork() Done  
3 - DoWork() Done  
4 - DoWork() Done  
5 - DoWork() Done  
Calling WorkService took: 26722 ms.  
Calling ObjectPooledWorkService:  
1 - DoWork() Done  
2 - DoWork() Done  
3 - DoWork() Done  
4 - DoWork() Done  
5 - DoWork() Done  
Calling ObjectPooledWorkService took: 5323 ms.  
Press <ENTER> to exit.  
```  
  
> [!NOTE]
> クライアントを最初に実行したときには、両方のサービスにかかった時間は同じように見えます。 サンプルを再実行すると、`ObjectPooledWorkService` の戻り時間の方が早いことがわかります。オブジェクトのインスタンスが既にプール内に存在しているからです。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
3. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。  
  
> [!NOTE]
> Svcutil.exe を使用してこのサンプルの構成を再生成した場合は、クライアント コードに一致するように、クライアント構成内のエンドポイント名を変更してください。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Instancing\Pooling`  
