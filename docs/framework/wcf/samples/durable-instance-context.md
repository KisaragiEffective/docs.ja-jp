---
title: 永続性インスタンス コンテキスト
ms.date: 03/30/2017
ms.assetid: 97bc2994-5a2c-47c7-927a-c4cd273153df
ms.openlocfilehash: 3ff4cbcf7a6007339d98820384f5e2d4164d1b0b
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74711937"
---
# <a name="durable-instance-context"></a>永続性インスタンス コンテキスト

このサンプルでは、Windows Communication Foundation (WCF) ランタイムをカスタマイズして、永続性インスタンスコンテキストを有効にする方法を示します。 バッキング ストアとして、SQL Server 2005 (この場合は SQL Server 2005 Express) を使用します。 ただし、カスタム ストレージ機構にアクセスする方法も示します。

> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。

このサンプルでは、WCF のチャネル層とサービスモデルレイヤーの両方を拡張します。 したがって、実装の詳細に進む前に基になる概念を理解する必要があります。

永続性インスタンス コンテキストは、現実のケースでも頻繁に起こりうるものです。 たとえば、ショッピング カート アプリケーションには、買い物を中断しても別の日に再開できる機能が用意されています。 そのため、ショッピング カートに翌日アクセスすると、元のコンテキストが復元されます。 接続が切断されている間、ショッピング カート アプリケーション (サーバー上) はショッピング カートのインスタンスを保持しないことに注意してください。 その代わり、状態を永続的なストレージ メディアに保持し、復元されたコンテキストの新しいインスタンスを構築するときにこの状態を使用します。 したがって、同じコンテキストに対してサービスを提供できるサービス インスタンスは、以前のインスタンスと同じではありません (つまり、メモリ アドレスが同じではありません)。

永続性インスタンス コンテキストは、コンテキスト ID をクライアントとサービス間で交換するための簡単なプロトコルによって可能になります。 このコンテキスト ID はクライアント上で作成され、サービスに転送されます。 サービス インスタンスが作成されると、サービス上のランタイムは、永続ストレージ (既定では SQL Server 2005 のデータベース) 上で永続化されている、このコンテキスト ID に対応する状態を読み込もうとします。 利用できる状態がない場合は、新しいインスタンスに既定の状態が設定されます。 サービス実装は、カスタム属性を使用してサービス実装の状態を変更する操作の詳細を指定し、ランタイムがその操作を呼び出した後にサービス インスタンスを保存できるようにします。

前の説明から、目標を達成するための手順は大きく次の 2 つに分けられます。

1. ネットワークに出力されるメッセージを、コンテキスト ID が含まれるように変更します。

2. サービス側のローカル動作を変更して、カスタムのインスタンス化ロジックを実装します。

この一覧の前者の手順は、カスタム チャネルとして実装され、チャネル レイヤにフックされる、ネットワーク上のメッセージに影響します。 後者の手順が影響を及ぼすのは、サービスのローカル動作だけです。したがって、いくつかのサービス拡張ポイントを拡張することによって実装できます。 以降のセクションでは、こうしたそれぞれの拡張について説明します。

## <a name="durable-instancecontext-channel"></a>永続的な InstanceContext チャネル

最初に、チャネル レイヤの拡張について考えます。 カスタム チャネルを記述する最初の手順として、チャネルの通信構造を決定します。 新しいワイヤ プロトコルを導入する際には、チャネルはチャネル スタック内の他のほとんどすべてのチャネルに対応する必要があります。 そのため、すべてのメッセージ交換パターンをサポートする必要があります。 ただし、チャネルの中心的な機能は通信構造に関係なく同じです。 具体的には、コンテキスト ID をクライアント側からメッセージに書き込み、サービス側からこのメッセージのコンテキスト ID を読み取って上位レベルに渡します。 そのため、すべての永続性インスタンス コンテキスト チャネルの実装に対して抽象基本クラスとして動作する、`DurableInstanceContextChannelBase` クラスが作成されます。 このクラスには、共通ステート マシンの管理機能と保護された 2 つのメンバが含まれ、これによってコンテキスト情報をメッセージに適用したり、メッセージからコンテキスト情報を読み取ったりします。

```csharp
class DurableInstanceContextChannelBase
{
  //…
  protected void ApplyContext(Message message)
  {
    //…
  }
  protected string ReadContextId(Message message)
  {
    //…
  }
}
```

これら 2 つのメソッドは、`IContextManager` 実装を使用して、メッセージへのコンテキスト ID の書き込みと、メッセージからのコンテキスト ID の読み取りを行います (`IContextManager` は、すべてのコンテキストマネージャーのコントラクトを定義するために使用されるカスタムインターフェイスです)。チャネルは、カスタム SOAP ヘッダーまたは HTTP クッキーヘッダーにコンテキスト ID を含めることができます。 各コンテキスト マネージャの実装は、`ContextManagerBase` クラスを継承します。このクラスには、すべてのコンテキスト マネージャについての共通機能が含まれています。 このクラスの `GetContextId` メソッドは、コンテキスト ID をクライアント側で生成する際に使用されます。 コンテキスト ID が最初に生成されると、このメソッドはこれをテキスト ファイルに保存します。テキスト ファイルの名前は、リモート エンドポイント アドレスによって作成されます (通常の URI に含まれる、ファイル名として無効な文字は、@ 文字に置き換えられます)。

コンテキスト ID が後で同じリモート エンドポイントで必要になると、このメソッドは適切なファイルが存在するかどうかをチェックします。 ファイルが存在する場合は、コンテキスト ID を読み取って返します。 存在しない場合は、新しく生成されたコンテキスト ID を返してこれをファイルに保存します。 既定の構成では、これらのファイルは現在のユーザーの一時ディレクトリ内にある ContextStore ディレクトリにあります。 ただし、この場所はバインド要素を使用して構成できます。

コンテキスト ID の転送に使用される機構は構成可能です。 HTTP クッキー ヘッダーか、またはカスタム SOAP ヘッダーのどちらかに記述できます。 カスタム SOAP ヘッダーに記述する場合は、このプロトコルを HTTP 以外のプロトコル (TCP や NamedPipes など) で使用できます。 これらの 2 つのオプションは、`MessageHeaderContextManager` と `HttpCookieContextManager` という名前の 2 つのクラスに実装されます。

これらのクラスはどちらも、コンテキスト ID をメッセージに適切に書き込みます。 たとえば `MessageHeaderContextManager` クラスでは、`WriteContext` メソッドにより SOAP ヘッダーにコンテキスト ID が書き込まれます。

```csharp
public override void WriteContext(Message message)
{
  string contextId = this.GetContextId();

  MessageHeader contextHeader =
    MessageHeader.CreateHeader(DurableInstanceContextUtility.HeaderName,
      DurableInstanceContextUtility.HeaderNamespace,
      contextId,
      true);

  message.Headers.Add(contextHeader);
}
```

`ApplyContext` クラス内の `ReadContextId` メソッドと `DurableInstanceContextChannelBase` メソッドは、それぞれ `IContextManager.ReadContext` と `IContextManager.WriteContext` を呼び出します。 ただし、これらのコンテキスト マネージャは `DurableInstanceContextChannelBase` クラスによって直接作成されるわけではありません。 代わりに `ContextManagerFactory` クラスを使用してこの処理が行われます。

```csharp
IContextManager contextManager =
                ContextManagerFactory.CreateContextManager(contextType,
                this.contextStoreLocation,
                this.endpointAddress);
```

`ApplyContext` メソッドは送信チャネルによって呼び出されます。 このメソッドは、コンテキスト ID を送信メッセージに挿入します。 `ReadContextId` メソッドは受信チャネルによって呼び出されます。 このメソッドは、受信メッセージ内でコンテキスト ID を使用できることを確認し、`Properties` クラスの `Message` コレクションにコンテキスト ID を追加します。 さらに、コンテキスト ID の読み取りでエラーが発生した場合は `CommunicationException` をスローし、チャネルを中止します。

```csharp
message.Properties.Add(DurableInstanceContextUtility.ContextIdProperty, contextId);
```

次に進む前に、`Properties` クラスの `Message` コレクションの使用方法について理解することが重要です。 通常、この `Properties` コレクションは、チャネル レイヤのデータを下位レベルから上位レベルに渡すときに使用されます。 この方法により、必要なデータを、プロトコルの詳細に関係なく一貫した方法で上位レベルに渡すことができます。 つまり、チャネル レイヤはコンテキスト ID を、SOAP ヘッダーとしても HTTP クッキー ヘッダーとしても送受信できます。 しかし、上位レベルではこうした詳細を認識する必要はありません。チャネル レイヤにより、この情報が `Properties` コレクションで使用できるためです。

`DurableInstanceContextChannelBase` クラスを配備したら、必要な 10 のインターフェイス (IOutputChannel、IInputChannel、IOutputSessionChannel、IInputSessionChannel、IRequestChannel、IReplyChannel、IRequestSessionChannel、IReplySessionChannel、IDuplexChannel、および IDuplexSessionChannel) のすべてを実装する必要があります。 これらは、使用可能なすべてのメッセージ交換パターン (データグラム、一方向、双方向、およびセッションフル バリエーション) と似ています。 これらの各実装は前記の基本クラスを継承し、`ApplyContext` と `ReadContextId` を適切に呼び出します。 たとえば、IOutputChannel インターフェイスを実装する `DurableInstanceContextOutputChannel` は、メッセージを送信する各メソッドから `ApplyContext` メソッドを呼び出します。

```csharp
public void Send(Message message, TimeSpan timeout)
{
    // Apply the context information before sending the message.
    this.ApplyContext(message);
    //…
}
```

これに対し、`DurableInstanceContextInputChannel` インターフェイスを実装する `IInputChannel` は、メッセージを受信する各メソッドで `ReadContextId` メソッドを呼び出します。

```csharp
public Message Receive(TimeSpan timeout)
{
    //…
      ReadContextId(message);
      return message;
}
```

これらのチャネル実装は、これ以外には、チャネル スタック内でチャネル実装の下にあるチャネルへのメソッド呼び出しを代行します。 ただし、セッションフル バリエーションには、セッション作成の基になる最初のメッセージに対してのみコンテキスト ID の送信と読み取りを許可する、基本的なロジックがあります。

```csharp
if (isFirstMessage)
{
//…
    this.ApplyContext(message);
    isFirstMessage = false;
}
```

これらのチャネル実装は、`DurableInstanceContextBindingElement` クラスと `DurableInstanceContextBindingElementSection` クラスによって WCF チャネルランタイムに適切に追加されます。 バインド要素とバインド要素のセクションの詳細については、 [Httpcookiesession](../../../../docs/framework/wcf/samples/httpcookiesession.md) channel サンプルドキュメントを参照してください。

## <a name="service-model-layer-extensions"></a>サービス モデル レイヤの拡張

コンテキスト ID がチャネル レイヤを通過すると、インスタンス化をカスタマイズするようにサービス側の動作を実装できます。 このサンプルでは、記憶域マネージャを使用して永続ストアからの状態の読み込みと、永続ストアへの状態の保存が行われます。 前述のように、このサンプルにはバッキング ストアとして SQL Server 2005 を使用する記憶域マネージャが用意されています。 ただし、この拡張に対してカスタム ストレージ機構を追加することもできます。 これを行うには、すべての記憶域マネージャが実装する必要のあるパブリック インターフェイスを宣言します。

```csharp
public interface IStorageManager
{
    object GetInstance(string contextId, Type type);
    void SaveInstance(string contextId, object state);
}
```

`SqlServerStorageManager` クラスには、既定の `IStorageManager` 実装が含まれます。 `SaveInstance` メソッドでは、指定されたオブジェクトは XmlSerializer を使用してシリアル化され、SQL Server データベースに保存されます。

```csharp
XmlSerializer serializer = new XmlSerializer(state.GetType());
string data;

using (StringWriter writer = new StringWriter(CultureInfo.InvariantCulture))
{
    serializer.Serialize(writer, state);
    data = writer.ToString();
}

using (SqlConnection connection = new SqlConnection(GetConnectionString()))
{
    connection.Open();

    string update = @"UPDATE Instances SET Instance = @instance WHERE ContextId = @contextId";

    using (SqlCommand command = new SqlCommand(update, connection))
    {
        command.Parameters.Add("@instance", SqlDbType.VarChar, 2147483647).Value = data;
        command.Parameters.Add("@contextId", SqlDbType.VarChar, 256).Value = contextId;

        int rows = command.ExecuteNonQuery();

        if (rows == 0)
        {
            string insert = @"INSERT INTO Instances(ContextId, Instance) VALUES(@contextId, @instance)";
            command.CommandText = insert;
            command.ExecuteNonQuery();
        }
    }
}
```

`GetInstance` メソッドでは、シリアル化されたデータが特定のコンテキスト ID 用に読み取られ、そこから作成されたオブジェクトが呼び出し元に返されます。

```csharp
object data;
using (SqlConnection connection = new SqlConnection(GetConnectionString()))
{
    connection.Open();

    string select = "SELECT Instance FROM Instances WHERE ContextId = @contextId";
    using (SqlCommand command = new SqlCommand(select, connection))
    {
        command.Parameters.Add("@contextId", SqlDbType.VarChar, 256).Value = contextId;
        data = command.ExecuteScalar();
    }
}

if (data != null)
{
    XmlSerializer serializer = new XmlSerializer(type);
    using (StringReader reader = new StringReader((string)data))
    {
        object instance = serializer.Deserialize(reader);
        return instance;
    }
}
```

この記憶域マネージャを直接インスタンス化することはできません。 その代わりに、`StorageManagerFactory` クラスを使用して、記憶域マネージャの作成に関する詳細から抽出します。 このクラスには、指定された型の記憶域マネージャのインスタンスを作成する、1 つの静的メンバ `GetStorageManager` があります。 型パラメーターが `null` の場合、このメソッドは既定の `SqlServerStorageManager` クラスのインスタンスを作成して返します。 さらに指定された型を検証して、それが `IStorageManager` インターフェイスを実装していることを確認します。

```csharp
public static IStorageManager GetStorageManager(Type storageManagerType)
{
IStorageManager storageManager = null;

if (storageManagerType == null)
{
    return new SqlServerStorageManager();
}
else
{
    object obj = Activator.CreateInstance(storageManagerType);

    // Throw if the specified storage manager type does not
    // implement IStorageManager.
    if (obj is IStorageManager)
    {
        storageManager = (IStorageManager)obj;
    }
    else
    {
        throw new InvalidOperationException(
                  ResourceHelper.GetString("ExInvalidStorageManager"));
    }

    return storageManager;
}
}
```

永続ストレージのインスタンスの読み取りや書き込みに必要なインフラストラクチャを実装します。 ここで、サービス側の動作を変更するために必要な手順を行う必要があります。

この処理の最初の手順として、チャネル レイヤを通過したコンテキスト ID を現在の InstanceContext に保存する必要があります。 InstanceContext は、WCF ディスパッチャーとサービスインスタンス間のリンクとして機能するランタイムコンポーネントです。 これを使用すると、追加の状態と動作をサービス インスタンスに提供できます。 セッションの多い通信では、コンテキスト ID は最初のメッセージだけに含まれて送信されるので、これが重要になります。

WCF では、拡張可能なオブジェクトパターンを使用して新しい状態と動作を追加することで、InstanceContext ランタイムコンポーネントを拡張できます。 拡張可能オブジェクトパターンは、既存のランタイムクラスを新しい機能で拡張するか、オブジェクトに新しい状態機能を追加するために WCF で使用されます。 拡張可能オブジェクトパターンには、IExtensibleObject\<T >、IExtension\<T >、IExtensionCollection\<T > という3つのインターフェイスがあります。

- IExtensibleObject\<T > インターフェイスは、機能をカスタマイズする拡張を可能にするオブジェクトによって実装されます。

- IExtension\<T > インターフェイスは、T 型のクラスの拡張であるオブジェクトによって実装されます。

- IExtensionCollection\<T > インターフェイスは IExtensions のコレクションであり、その型を使用して IExtensions を取得できます。

このため、IExtension インターフェイスを実装して、コンテキスト ID を保存するために必要な状態を定義する、InstanceContextExtension クラスを作成する必要があります。 このクラスではさらに、使用される記憶域マネージャを保持する状態も提供されます。 新しい状態が保存された後では、その状態を変更できません。 したがって、インスタンスが作成される際、読み取り専用のプロパティを使用してのみアクセス可能になった時点で、状態がインスタンスに提供され、保存されます。

```csharp
// Constructor
public DurableInstanceContextExtension(string contextId,
            IStorageManager storageManager)
{
    this.contextId = contextId;
    this.storageManager = storageManager;
}

// Read only properties
public string ContextId
{
    get { return this.contextId; }
}

public IStorageManager StorageManager
{
    get { return this.storageManager; }
}
```

InstanceContextInitializer クラスは IInstanceContextInitializer インターフェイスを実装し、作成された InstanceContext の Extensions コレクションにインスタンス コンテキスト拡張を追加します。

```csharp
public void Initialize(InstanceContext instanceContext, Message message)
{
    string contextId =
  (string)message.Properties[DurableInstanceContextUtility.ContextIdProperty];

    DurableInstanceContextExtension extension =
                new DurableInstanceContextExtension(contextId,
                     storageManager);
    instanceContext.Extensions.Add(extension);
}
```

既に説明したように、コンテキスト ID は `Properties` クラスの `Message` コレクションから読み取られ、拡張クラスのコンストラクタに渡されます。 これによって、レイヤ間で情報を交換する場合の一貫性のある方法が示されます。

次の重要な手順は、サービス インスタンスの作成手順をオーバーライドすることです。 WCF を使用すると、カスタムのインスタンス化動作を実装し、IInstanceProvider インターフェイスを使用してランタイムにフックできます。 新しい `InstanceProvider` クラスがこの処理を行うために実装されます。 コンストラクタでは、インスタンス プロバイダから予期されるサービス型が受け入れられます。 これは、後で新しいインスタンスの作成に使用されます。 `GetInstance` 実装では、永続するインスタンスを検索する記憶域マネージャのインスタンスが作成されます。 `null` が返された場合、サービス型の新しいインスタンスがインスタンス化され、呼び出し元に返されます。

```csharp
public object GetInstance(InstanceContext instanceContext, Message message)
{
    object instance = null;

    DurableInstanceContextExtension extension =
    instanceContext.Extensions.Find<DurableInstanceContextExtension>();

    string contextId = extension.ContextId;
    IStorageManager storageManager = extension.StorageManager;

    instance = storageManager.GetInstance(contextId, serviceType);

    instance ??= Activator.CreateInstance(serviceType);
    return instance;
}
```

次の重要な手順は、`InstanceContextExtension` クラス、`InstanceContextInitializer` クラス、および `InstanceProvider` クラスをサービス モデル ランタイムにインストールすることです。 カスタム属性を使用すると、サービス実装クラスの詳細を指定して、動作をインストールできます。 `DurableInstanceContextAttribute` にはこの属性の実装が含まれ、サービス側のランタイム全体を拡張できるようにするために `IServiceBehavior` インターフェイスを実装します。

このクラスには、使用される記憶域マネージャの型を受け入れるプロパティがあります。 このようにして実装を行うことにより、ユーザー独自の `IStorageManager` 実装を、この属性のパラメータとして指定できます。

`ApplyDispatchBehavior` 実装では、現在の `InstanceContextMode` 属性の `ServiceBehavior` が検証されます。 このプロパティが Singleton に設定されている場合、永続性インスタンスを有効化できず、`InvalidOperationException` がスローされてホストに通知されます。

```csharp
ServiceBehaviorAttribute serviceBehavior =
    serviceDescription.Behaviors.Find<ServiceBehaviorAttribute>();

if (serviceBehavior != null &&
     serviceBehavior.InstanceContextMode == InstanceContextMode.Single)
{
    throw new InvalidOperationException(
       ResourceHelper.GetString("ExSingletonInstancingNotSupported"));
}
```

この後、記憶域マネージャのインスタンス、インスタンス コンテキストの初期化子、およびインスタンス プロバイダが作成され、各エンドポイント用に作成された `DispatchRuntime` にインストールされます。

```csharp
IStorageManager storageManager =
    StorageManagerFactory.GetStorageManager(storageManagerType);

InstanceContextInitializer contextInitializer =
    new InstanceContextInitializer(storageManager);

InstanceProvider instanceProvider =
    new InstanceProvider(description.ServiceType);

foreach (ChannelDispatcherBase cdb in serviceHostBase.ChannelDispatchers)
{
    ChannelDispatcher cd = cdb as ChannelDispatcher;

    if (cd != null)
    {
        foreach (EndpointDispatcher ed in cd.Endpoints)
        {
            ed.DispatchRuntime.InstanceContextInitializers.Add(contextInitializer);
            ed.DispatchRuntime.InstanceProvider = instanceProvider;
        }
    }
}
```

ここまでを要約すると、このサンプルでは、カスタム コンテキスト ID を交換するためにカスタム ワイヤ プロトコルを有効化するチャネルを作成しました。また、永続ストレージのインスタンスを読み込むように、既定のインスタンス化動作を上書きしました。

残りの手順は、サービス インスタンスを永続ストレージに保存することです。 既に説明したとおり、`IStorageManager` 実装に状態を保存するには、あらかじめ必要な機能があります。 これを WCF ランタイムと統合する必要があります。 これを行うには、サービス実装クラスのメソッドに適用可能な別の属性が必要です。 つまりこの属性は、サービス インスタンスの状態を変更するメソッドに適用できることが必要です。

この機能は、`SaveStateAttribute` クラスに実装されています。 また、各操作の WCF ランタイムを変更する `IOperationBehavior` クラスも実装しています。 メソッドがこの属性でマークされると、WCF ランタイムは、適切な `DispatchOperation` が構築されている間に、`ApplyBehavior` メソッドを呼び出します。 このメソッドの実装のコードは 1 行です。

```csharp
dispatch.Invoker = new OperationInvoker(dispatch.Invoker);
```

この手順により `OperationInvoker` 型のインスタンスが作成され、作成される `Invoker` の `DispatchOperation` プロパティに割り当てられます。 `OperationInvoker` クラスは、`DispatchOperation` 用に作成された既定の操作呼び出しのラッパーです。 このクラスは、 `IOperationInvoker` インターフェイスを実装します。 `Invoke` メソッドの実装では、実際のメソッド呼び出しは内部の操作呼び出しで代行されます。 ただし、この結果が返される前に `InstanceContext` の記憶域マネージャが使用され、サービス インスタンスが保存されます。

```csharp
object result = innerOperationInvoker.Invoke(instance,
    inputs, out outputs);

// Save the instance using the storage manager saved in the
// current InstanceContext.
InstanceContextExtension extension =
    OperationContext.Current.InstanceContext.Extensions.Find<InstanceContextExtension>();

extension.StorageManager.SaveInstance(extension.ContextId, instance);
return result;
```

## <a name="using-the-extension"></a>拡張機能の使用

チャネル層とサービスモデルの両方の拡張機能が実行され、WCF アプリケーションで使用できるようになりました。 サービスでは、カスタム バインドを使用してチャネルをチャネル スタックに追加し、サービス実装クラスの詳細を適切な属性で指定する必要があります。

```csharp
[DurableInstanceContext]
[ServiceBehavior(InstanceContextMode=InstanceContextMode.PerSession)]
public class ShoppingCart : IShoppingCart
{
//…
     [SaveState]
     public int AddItem(string item)
     {
         //…
     }
//…
 }
```

クライアント アプリケーションは、カスタム バインドを使用して DurableInstanceContextChannel をチャネル スタックに追加する必要があります。 構成ファイル内でチャネルを宣言して構成するには、バインド要素セクションをバインド要素拡張のコレクションに追加する必要があります。

```xml
<system.serviceModel>
 <extensions>
   <bindingElementExtensions>
     <add name="durableInstanceContext"
type="Microsoft.ServiceModel.Samples.DurableInstanceContextBindingElementSection, DurableInstanceContextExtension, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"/>
   </bindingElementExtensions>
 </extensions>
```

これで、他の基本的なバインド要素と同様、このバインド要素をカスタム バインドで使用できるようになりました。

```xml
<bindings>
 <customBinding>
   <binding name="TextOverHttp">
     <durableInstanceContext contextType="HttpCookie"/>
     <reliableSession />
     <textMessageEncoding />
     <httpTransport />
   </binding>
 </customBinding>
</bindings>
```

## <a name="conclusion"></a>結論

このサンプルでは、カスタム プロトコル チャネルを作成する方法と、これを有効にするためにサービス動作をカスタマイズする方法を示しました。

構成セクションを使用して `IStorageManager` 実装を指定することで、この拡張機能をさらに強化することができます。 これにより、サービス コードを再コンパイルせずにバッキング ストアを変更できます。

さらに、インスタンスの状態をカプセル化するクラス (`StateBag` など) を実装できます。 このクラスにより、変更されるたびに状態が保持されます。 この方法により、`SaveState` 属性の使用を回避しながら、永続する処理をより正確に実行できます (たとえば、`SaveState` 属性を含むメソッドを呼び出すたびに状態を保存するのではなく、状態が実際に変更されたときに保持できます)。

このサンプルを実行すると、次の出力が表示されます。 クライアントは、2 つの商品をショッピング カートに追加し、その後ショッピング カートにある商品の一覧をサービスから取得します。 どちらかのコンソールで Enter キーを押すと、サービスとクライアントがどちらもシャットダウンされます。

```console
Enter the name of the product: apples
Enter the name of the product: bananas

Shopping cart currently contains the following items.
apples
bananas
Press ENTER to shut down client
```

> [!NOTE]
> サービスを再ビルドすると、データベース ファイルが上書きされます。 サンプルを複数回実行する間に状態が保持されていることを確認するには、実行のたびにサンプルを再ビルドしないようにしてください。

#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

2. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。

3. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。

> [!NOTE]
> このサンプルを実行するには、SQL Server 2005 または SQL Express 2005 を実行している必要があります。 SQL Server 2005 を実行している場合は、サービスの接続文字列の構成を変更する必要があります 複数コンピューターで実行している場合、SQL Server が必要なのはサーバー コンピューターだけです。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Instancing\Durable`
