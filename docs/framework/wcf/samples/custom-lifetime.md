---
title: カスタム有効期間
ms.date: 08/20/2018
ms.assetid: 52806c07-b91c-48fe-b992-88a41924f51f
ms.openlocfilehash: 8625877d9b4d05d5cf06af2c9f8ef10f701e98db
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715478"
---
# <a name="custom-lifetime"></a>カスタム有効期間

このサンプルでは、Windows Communication Foundation (WCF) 拡張機能を記述して、共有 WCF サービスインスタンスにカスタムの有効期間サービスを提供する方法を示します。

> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、この記事の最後を参照してください。

## <a name="shared-instancing"></a>共有インスタンス化

WCF には、サービスインスタンスに対して複数のインスタンス化モードが用意されています。 この記事で説明されている共有インスタンス化モードでは、複数のチャネル間でサービスインスタンスを共有する方法を提供します。 クライアントは、サービスのファクトリメソッドに接続して、通信を開始するための新しいチャネルを作成できます。 次のコードスニペットは、クライアントアプリケーションが既存のサービスインスタンスへの新しいチャネルを作成する方法を示しています。

```csharp
// Create a header for the shared instance id
MessageHeader shareableInstanceContextHeader = MessageHeader.CreateHeader(
        CustomHeader.HeaderName,
        CustomHeader.HeaderNamespace,
        Guid.NewGuid().ToString());

// Create the channel factory
ChannelFactory<IEchoService> channelFactory =
    new ChannelFactory<IEchoService>("echoservice");

// Create the first channel
IEchoService proxy = channelFactory.CreateChannel();

// Call an operation to create shared service instance
using (new OperationContextScope((IClientChannel)proxy))
{
    OperationContext.Current.OutgoingMessageHeaders.Add(shareableInstanceContextHeader);
    Console.WriteLine("Service returned: " + proxy.Echo("Apple"));
}

((IChannel)proxy).Close();

// Create the second channel
IEchoService proxy2 = channelFactory.CreateChannel();

// Call an operation using the same header that will reuse the shared service instance
using (new OperationContextScope((IClientChannel)proxy2))
{
    OperationContext.Current.OutgoingMessageHeaders.Add(shareableInstanceContextHeader);
    Console.WriteLine("Service returned: " + proxy2.Echo("Apple"));
}
```

他のインスタンス化モードとは異なり、共有インスタンス化モードでは、独特の方法でサービス インスタンスを解放します。 既定では、すべてのチャネルが <xref:System.ServiceModel.InstanceContext>で閉じられると、WCF サービスランタイムは、サービス <xref:System.ServiceModel.InstanceContextMode> が <xref:System.ServiceModel.InstanceContextMode.PerCall> または <xref:System.ServiceModel.InstanceContextMode.PerSession>するように構成されているかどうかを確認し、インスタンスを解放してリソースを要求するかどうかを確認します。 カスタム <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider> が使用されている場合は、インスタンスを解放する前に、WCF によってプロバイダー実装の <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider.IsIdle%2A> メソッドが呼び出されます。 インスタンスが解放された `true` <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider.IsIdle%2A> が返される場合は。それ以外の場合、<xref:System.ServiceModel.Dispatcher.IInstanceContextProvider> の実装は、コールバックメソッドを使用して、アイドル状態の `Dispatcher` に通知を行います。 これは、プロバイダーの <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider.NotifyIdle%2A> メソッドを呼び出すことによって行われます。

このサンプルでは、アイドルタイムアウトが20秒の <xref:System.ServiceModel.InstanceContext> の解放を遅らせる方法を示します。

## <a name="extending-the-instancecontext"></a>InstanceContext の拡張

WCF では、<xref:System.ServiceModel.InstanceContext> はサービスインスタンスと `Dispatcher`間のリンクです。 WCF では、拡張可能なオブジェクトパターンを使用して新しい状態または動作を追加することで、このランタイムコンポーネントを拡張できます。 拡張可能オブジェクトパターンは、既存のランタイムクラスを新しい機能で拡張するか、オブジェクトに新しい状態機能を追加するために WCF で使用されます。 拡張可能オブジェクト パターンには、<xref:System.ServiceModel.IExtensibleObject%601>、<xref:System.ServiceModel.IExtension%601>、および <xref:System.ServiceModel.IExtensionCollection%601> の 3 つのインターフェイスがあります。

<xref:System.ServiceModel.IExtensibleObject%601> インターフェイスは、機能をカスタマイズする拡張を可能にするために、オブジェクトによって実装されます。

<xref:System.ServiceModel.IExtension%601> インターフェイスは、`T` 型のクラスの拡張が可能なオブジェクトによって実装されます。

最後に、<xref:System.ServiceModel.IExtensionCollection%601> インターフェイスは <xref:System.ServiceModel.IExtension%601> 実装のコレクションであり、<xref:System.ServiceModel.IExtension%601> の実装をその型によって取得できるようにします。

そのため、<xref:System.ServiceModel.InstanceContext>を拡張するには、<xref:System.ServiceModel.IExtension%601> インターフェイスを実装する必要があります。 このサンプルプロジェクトでは、`CustomLeaseExtension` クラスにこの実装が含まれています。

```csharp
class CustomLeaseExtension : IExtension<InstanceContext>
{
}
```

<xref:System.ServiceModel.IExtension%601> インターフェイスには、<xref:System.ServiceModel.IExtension%601.Attach%2A> と <xref:System.ServiceModel.IExtension%601.Detach%2A> の 2 つのメソッドが含まれています。 名前が示すように、これらの 2 つのメソッドは、ランタイムが <xref:System.ServiceModel.InstanceContext> クラスのインスタンスに拡張機能を関連付けるときと関連付けを解除するときに呼び出されます。 このサンプルでは、`Attach` メソッドを使用して、拡張機能の現在のインスタンスに属する <xref:System.ServiceModel.InstanceContext> オブジェクトを追跡します。

```csharp
InstanceContext owner;

public void Attach(InstanceContext owner)
{
    this.owner = owner;
}
```

また、有効期間の延長をサポートするには、必要な実装を拡張機能に追加する必要があります。 したがって、`ICustomLease` インターフェイスは目的のメソッドを使用して宣言され、`CustomLeaseExtension` クラスに実装されます。

```csharp
interface ICustomLease
{
    bool IsIdle { get; }
    InstanceContextIdleCallback Callback { get; set; }
}

class CustomLeaseExtension : IExtension<InstanceContext>, ICustomLease
{
}
```

WCF が <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider> の実装で <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider.IsIdle%2A> メソッドを呼び出すと、この呼び出しは `CustomLeaseExtension`の <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider.IsIdle%2A> メソッドにルーティングされます。 次に、`CustomLeaseExtension` がプライベート状態をチェックし、<xref:System.ServiceModel.InstanceContext> がアイドル状態かどうかを確認します。 アイドル状態の場合は `true`を返します。 それ以外の場合は、延長された指定の有効期間のタイマーが開始されます。

```csharp
public bool IsIdle
{
  get
  {
    lock (thisLock)
    {
      if (isIdle)
      {
        return true;
      }
      else
      {
        StartTimer();
        return false;
      }
    }
  }
}
```

タイマーの `Elapsed` イベントでは、別のクリーンアップサイクルを開始するために、ディスパッチャーのコールバック関数が呼び出されます。

```csharp
void idleTimer_Elapsed(object sender, ElapsedEventArgs args)
{
    lock (thisLock)
    {
        StopTimer();
        isIdle = true;
        Utility.WriteMessageToConsole(
            ResourceHelper.GetString("MsgLeaseExpired"));
        callback(owner);
    }
}
```

インスタンスをアイドル状態に移行するために新しいメッセージが到着したときに、実行中のタイマーを更新する方法はありません。

このサンプルでは、<xref:System.ServiceModel.Dispatcher.IInstanceContextProvider> メソッドの呼び出しを受け取って <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider.IsIdle%2A> にルーティングするために `CustomLeaseExtension` を実装します。 <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider> 実装は、`CustomLifetimeLease` クラスに含まれています。 <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider.IsIdle%2A> メソッドは、WCF がサービスインスタンスを解放しようとしているときに呼び出されます。 ただし、ServiceBehavior の `ISharedSessionInstance` コレクションに存在する特定の <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider> 実装のインスタンスは 1 つだけです。 つまり、WCF が <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider.IsIdle%2A> メソッドをチェックするときに、<xref:System.ServiceModel.InstanceContext> が閉じられているかどうかを知る方法はありません。 したがって、このサンプルでは、スレッドロックを使用して、<xref:System.ServiceModel.Dispatcher.IInstanceContextProvider.IsIdle%2A> メソッドに要求をシリアル化します。

> [!IMPORTANT]
> シリアル化はアプリケーションのパフォーマンスに大きな影響を及ぼす可能性があるので、スレッド ロックは使用しないことをお勧めします。

`CustomLifetimeLease` クラスでは、プライベートメンバーフィールドを使用してアイドル状態を追跡し、<xref:System.ServiceModel.Dispatcher.IInstanceContextProvider.IsIdle%2A> メソッドによって返されます。 <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider.IsIdle%2A> メソッドが呼び出されるたびに、`isIdle` フィールドが返され、`false`にリセットされます。  ディスパッチャーが `false` メソッドを呼び出すようにするには、この値を <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider.NotifyIdle%2A> に設定する必要があります。

```csharp
public bool IsIdle(InstanceContext instanceContext)
{
    get
    {
        lock (thisLock)
        {
            //...
            bool idleCopy = isIdle;
            isIdle = false;
            return idleCopy;
        }
    }
}
```

<xref:System.ServiceModel.Dispatcher.IInstanceContextProvider.IsIdle%2A?displayProperty=nameWithType> メソッドが `false`を返す場合、ディスパッチャーは <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider.NotifyIdle%2A> メソッドを使用してコールバック関数を登録します。 このメソッドは、解放される <xref:System.ServiceModel.InstanceContext> への参照を受け取ります。 このため、サンプルコードでは `ICustomLease` 型拡張機能に対してクエリを実行し、拡張された状態の `ICustomLease.IsIdle` プロパティを確認できます。

```csharp
public void NotifyIdle(InstanceContextIdleCallback callback,
            InstanceContext instanceContext)
{
    lock (thisLock)
    {
       ICustomLease customLease =
           instanceContext.Extensions.Find<ICustomLease>();
       customLease.Callback = callback;
       isIdle = customLease.IsIdle;
       if (isIdle)
       {
             callback(instanceContext);
       }
    }
}
```

`ICustomLease.IsIdle` プロパティをオンにする前に、コールバックプロパティを設定する必要があります。これは、`CustomLeaseExtension` がアイドル状態になったときにディスパッチャーに通知するために必要なためです。 `ICustomLease.IsIdle` が `true` を返す場合は、`isIdle` プライベート メンバーが `CustomLifetimeLease` で単に `true` に設定され、コールバック メソッドが呼び出されます。 コードはロックを保持しているため、他のスレッドがこのプライベートメンバーの値を変更することはできません。 次にディスパッチャーが <xref:System.ServiceModel.Dispatcher.IInstanceContextProvider.IsIdle%2A?displayProperty=nameWithType>を呼び出すときに、`true` を返し、ディスパッチャーがインスタンスを解放できるようにします。

これでカスタム拡張機能の基礎が完成したので、この拡張機能をサービス モデルにフックする必要があります。 <xref:System.ServiceModel.InstanceContext>に `CustomLeaseExtension` 実装をフックするために、WCF には <xref:System.ServiceModel.InstanceContext>のブートストラップを実行するための <xref:System.ServiceModel.Dispatcher.IInstanceContextInitializer> インターフェイスが用意されています。 このサンプルでは、`CustomLeaseInitializer` クラスでこのインターフェイスを実装し、`CustomLeaseExtension` のインスタンスを唯一のメソッドの初期化から得られる <xref:System.ServiceModel.InstanceContext.Extensions%2A> コレクションに追加します。 このメソッドは、<xref:System.ServiceModel.InstanceContext> の初期化中にディスパッチャーによって呼び出されます。

```csharp
public void InitializeInstanceContext(InstanceContext instanceContext,
    System.ServiceModel.Channels.Message message, IContextChannel channel)

    //...

    IExtension<InstanceContext> customLeaseExtension =
        new CustomLeaseExtension(timeout, headerId);
    instanceContext.Extensions.Add(customLeaseExtension);
}
```

 最後に、<xref:System.ServiceModel.Description.IServiceBehavior> の実装を使用して、<xref:System.ServiceModel.Dispatcher.IInstanceContextProvider> の実装をサービスモデルにフックします。 この実装は、`CustomLeaseTimeAttribute` クラスに配置されています。また、<xref:System.Attribute> 基本クラスから派生してこの動作を属性として公開します。

```csharp
public void ApplyDispatchBehavior(ServiceDescription description,
           ServiceHostBase serviceHostBase)
{
    CustomLifetimeLease customLease = new CustomLifetimeLease(timeout);

    foreach (ChannelDispatcherBase cdb in serviceHostBase.ChannelDispatchers)
    {
        ChannelDispatcher cd = cdb as ChannelDispatcher;

        if (cd != null)
        {
            foreach (EndpointDispatcher ed in cd.Endpoints)
            {
                ed.DispatchRuntime.InstanceContextProvider = customLease;
            }
        }
    }
}
```

この動作は、`CustomLeaseTime` 属性を使用して注釈を付けることにより、サンプル サービス クラスに追加できます。

```csharp
[CustomLeaseTime(Timeout = 20000)]
public class EchoService : IEchoService
{
  //…
}
```

このサンプルを実行すると、操作要求と応答がサービスとクライアントの両方のコンソール ウィンドウに表示されます。 どちらかのコンソールで Enter キーを押すと、サービスとクライアントがどちらもシャットダウンされます。

### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

2. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](building-the-samples.md)」の手順に従います。

3. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](running-the-samples.md)」の手順に従います。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Instancing\Lifetime`
