---
title: RPC の種類 - WCF 開発者向け gRPC
description: WCF でサポートされているリモート プロシージャ コールの種類と、gRPC でそれに対応するものを確認します
ms.date: 09/02/2019
ms.openlocfilehash: 40c0779dc015904e9dabbb448075e3c5aa5dc49a
ms.sourcegitcommit: 267d092663aba36b6b2ea853034470aea493bfae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2020
ms.locfileid: "80111089"
---
# <a name="types-of-rpc"></a>RPC の種類

Windows Communication Foundation (WCF) 開発者は、次の種類のリモート プロシージャ コール (RPC) を使用したことがあるはずです。

- 要求/応答
- 二重:
  - セッション対応の一方向の二重
  - セッション対応の全二重
- 一方向

これらの RPC の種類は、既存の gRPC の概念に自然にマップすることができます。 この章では、これらの各領域について順番に見ていきます。 [第 5 章](migrate-wcf-to-grpc.md)では、同様の例についてさらに詳しく説明します。

| WCF | gRPC |
| --- | ---- |
| 通常の要求/応答 | 単項 |
| クライアント コールバック インターフェイスを使用したセッション対応の二重サービス | サーバー ストリーミング。 |
| セッション対応の全二重サービス | 双方向ストリーミング |
| 一方向の操作 | クライアント ストリーミング |

## <a name="requestreply"></a>要求/応答

少量のデータを取得して返す単純な要求/応答メソッドの場合は、最も単純な gRPC パターンである単項 RPC を使用します。

```protobuf
service Things {
    rpc Get(GetThingRequest) returns (GetThingResponse);
}
```

```csharp
public class ThingService : Things.ThingsBase
{
    public override async Task<GetThingResponse> Get(GetThingRequest request, ServerCallContext context)
    {
        // Get thing from database
        return new GetThingResponse { Thing = thing };
    }
}
```

```csharp
public async Task ShowThing(int thingId)
{
    var thing = await _thingsClient.GetAsync(new GetThingRequest { ThingId = thingId });
    Console.WriteLine($"{thing.Name}");
}
```

ご覧のように、gRPC の単項 RPC サービス メソッドの実装は、WCF 操作の実装に似ています。 gRPC と違うのは、インターフェイスを実装するのではなく、基底クラスのメソッドをオーバーライドすることです。 サーバーでは、gRPC の基本メソッドからは常に <xref:System.Threading.Tasks.Task%601> が返されますが、クライアントではサービスを呼び出すために非同期とブロック両方のメソッドが提供されています。

## <a name="wcf-duplex-one-way-to-client"></a>WCF 二重、クライアントへの一方向

WCF アプリケーション (特定のバインドを使用するもの) では、クライアントとサーバーの間に永続的な接続を作成できます。 接続が閉じられるまで、<xref:System.ServiceModel.ServiceContractAttribute.CallbackContract%2A?displayProperty=nameWithType> プロパティで指定された "*コールバック インターフェイス*" を使用して、サーバーからクライアントに非同期にデータを送信できます。

gRPC のサービスには、メッセージ ストリームで同様の機能が用意されています。 実装という点では、ストリームは WCF の二重サービスに "*厳密*" には対応しませんが、同じ結果を得ることができます。

### <a name="grpc-streaming"></a>gRPC ストリーミング

gRPC により、クライアントからサーバー、およびサーバーからクライアントへの、永続的なストリームの作成がサポートされています。 両方の種類のストリームを、同時にアクティブにすることができます。 この機能は、双方向ストリーミングと呼ばれます。

時間のかかる任意の非同期メッセージングに、ストリームを使用できます。 または、1 つの要求または応答で生成および送信するには大きすぎる大規模なデータセットを渡すために使用できます。

次の例では、サーバー ストリーミング RPC を示します。

```protobuf
service ClockStreamer {
    rpc Subscribe(ClockSubscribeRequest) returns (stream ClockMessage);
}
```

```csharp
public class ClockStreamerService : ClockStreamer.ClockStreamerBase
{
    public override async Task Subscribe(ClockSubscribeRequest request,
        IServerStreamWriter<ClockMessage> responseStream,
        ServerCallContext context)
    {
        while (!context.CancellationToken.IsCancellationRequested)
        {
            var time = DateTimeOffset.UtcNow;
            await responseStream.WriteAsync(new ClockMessage { message = $"The time is {time:t}." });
            await Task.Delay(TimeSpan.FromSeconds(10), context.CancellationToken);
        }
    }
}
```

次のコードで示すように、クライアント アプリケーションからこのサーバー ストリームを使用できます。

```csharp
public async Task TellTheTimeAsync(CancellationToken token)
{
    var channel = GrpcChannel.ForAddress("https://localhost:5001");
    var client = new ClockStreamer.ClockStreamerClient(channel);

    var request = new ClockSubscribeRequest();
    var response = client.Subscribe(request);

    await foreach (var update in response.ResponseStream.ReadAllAsync(token))
    {
        Console.WriteLine(update.Message);
    }
}
```

> [!NOTE]
> サーバー ストリーミング RPC は、サブスクリプション スタイルのサービスに役立ちます。 また、データセット全体をメモリ内で構築するのが非効率または不可能な場合に、大きなデータセットを送信する場合にも役立ちます。 ただし、ストリーミングの応答は、1 つのメッセージで `repeated` フィールドを送信する場合ほど高速ではありません。 ルールとして、小規模なデータセットにはストリーミングを使用しないでください。

### <a name="differences-from-wcf"></a>WCF との相違点

WCF 二重サービスでは、複数のメソッドを使用できるクライアント コールバック インターフェイスが使用されます。 gRPC サーバー ストリーミング サービスの場合、1 つのストリームでのみメッセージを送信できます。 複数のメソッドが必要な場合は、[Any フィールドまたは oneof フィールド](protobuf-any-oneof.md)のいずれかを含むメッセージの種類を使用して、異なるメッセージを送信し、クライアントでそれらを処理するためのコードを記述します。

WCF では、セッションを使用する [ServiceContract](xref:System.ServiceModel.ServiceContractAttribute) クラスは、接続が閉じられるまで維持されます。 セッション内で、複数のメソッドを呼び出すことができます。 gRPC では、実装メソッドによって返される `Task` は、接続が閉じられるまで完了することはできません。

## <a name="wcf-one-way-operations-and-grpc-client-streaming"></a>WCF の一方向操作と gRPC のクライアント ストリーミング

WCF によって提供される一方向の操作 (`[OperationContract(IsOneWay = true)]` でマークされているもの) からは、トランスポート固有の受信確認が返されます。 gRPC のサービス メソッドからは、空の場合でも、常に応答が返されます。 クライアントでは、常にその応答を待機する必要があります。 gRPC でのメッセージングの "ファイア アンド フォーゲット" スタイルの場合は、クライアント ストリーミング サービスを作成できます。

### <a name="thing_logproto"></a>thing_log.proto

```protobuf
service ThingLog {
  rpc OpenConnection(stream Thing) returns (ConnectionClosedResponse);
}
```

### <a name="thinglogservicecs"></a>ThingLogService.cs

```csharp
public class ThingLogService : Protos.ThingLog.ThingLogBase
{
    private static readonly ConnectionClosedResponse EmptyResponse = new ConnectionClosedResponse();
    private readonly ILogger<ThingLogService> _logger;
    public ThingLogService(ILogger<ThingLogService> logger)
    {
        _logger = logger;
    }

    public override async Task<CompletedResponse> OpenConnection(IAsyncStreamReader<Thing> requestStream, ServerCallContext context)
    {
        while (await requestStream.MoveNext(context.CancellationToken))
        {
            _logger.LogInformation(requestStream.Current.Description);
        }
        return EmptyResponse;
    }
}
```

### <a name="thinglog-client-example"></a>ThingLog クライアントの例

```csharp
public class ThingLogger : IAsyncDisposable
{
    private readonly ThingLog.ThingLogClient _client;
    private readonly AsyncClientStreamingCall<ThingLogRequest, CompletedResponse> _stream;

    public ThingLogger(ThingLog.ThingLogClient client)
    {
        _client = client;
        _stream = client.OpenConnection();
    }

    public async Task WriteAsync(string description)
    {
        await _stream.RequestStream.WriteAsync(new Thing
        {
            Description = description,
            Time = Timestamp.FromDateTimeOffset(DateTimeOffset.UtcNow)
        });
    }

    public async ValueTask DisposeAsync()
    {
        await _stream.RequestStream.CompleteAsync();
        _stream.Dispose();
    }
}
```

前の例で示したように、ファイア アンド フォーゲットのメッセージには、クライアント ストリーミング RPC を使用できます。 また、非常に大規模なデータセットをサーバーに送信する場合も、それを使用できます。 パフォーマンスに関しても同じ警告が適用されます。小規模なデータセットの場合は、通常のメッセージで `repeated` フィールドを使用します。

## <a name="wcf-full-duplex-services"></a>WCF の全二重サービス

WCF の二重バインドでは、サービス インターフェイスとクライアント コールバック インターフェイスの両方で、複数の一方向操作がサポートされます。 このサポートにより、クライアントとサーバーの間で持続的な会話が可能になります。 gRPC では、双方向ストリーミング RPC と同様のものがサポートされています。この場合、両方のパラメーターが `stream` 修飾子でマークされます。

### <a name="chatproto"></a>chat.proto

```protobuf
service Chatter {
    rpc Connect(stream IncomingMessage) returns (stream OutgoingMessage);
}
```

### <a name="chatterservicecs"></a>ChatterService.cs

```csharp
public class ChatterService : Chatter.ChatterBase
{
    private readonly IChatHub _hub;

    public ChatterService(IChatHub hub)
    {
        _hub = hub;
    }

    public override async Task Connect(IAsyncStreamReader<MessageRequest> requestStream, IServerStreamWriter<MessageResponse> responseStream, ServerCallContext context)
    {
        _hub.MessageReceived += async (sender, args) =>
            await responseStream.WriteAsync(new MessageResponse {Text = args.Message});

        while (await requestStream.MoveNext(context.CancellationToken))
        {
            await _hub.SendAsync(requestStream.Current.Text);
        }
    }
}
```

前の例では、実装メソッドが要求ストリーム (`IAsyncStreamReader<MessageRequest>`) と応答ストリーム (`IServerStreamWriter<MessageResponse>`) の両方を受け取ることがわかります。 接続が閉じられるまで、メソッドでメッセージの読み取りと書き込みを行うことができます。

### <a name="chatter-client"></a>Chatter クライアント

```csharp
public class Chat : IAsyncDisposable
{
    private readonly Chatter.ChatterClient _client;
    private readonly AsyncDuplexStreamingCall<MessageRequest, MessageResponse> _stream;
    private readonly CancellationTokenSource _cancellationTokenSource;
    private readonly Task _readTask;

    public Chat(Chatter.ChatterClient client)
    {
        _client = client;
        _stream = _client.Connect();
        _cancellationTokenSource = new CancellationTokenSource();
        _readTask = ReadAsync(_cancellationTokenSource.Token);
    }

    public async Task SendAsync(string message)
    {
        await _stream.RequestStream.WriteAsync(new MessageRequest {Text = message});
    }

    private async Task ReadAsync(CancellationToken token)
    {
        while (await _stream.ResponseStream.MoveNext(token))
        {
            Console.WriteLine(_stream.ResponseStream.Current.Text);
        }
    }

    public async ValueTask DisposeAsync()
    {
        await _stream.RequestStream.CompleteAsync();
        await _readTask;
        _stream.Dispose();
    }
}
```

>[!div class="step-by-step"]
>[前へ](wcf-bindings.md)
>[次へ](metadata.md)
