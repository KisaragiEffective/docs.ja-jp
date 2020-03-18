---
title: RPC の種類 - WCF 開発者向け gRPC
description: WCF でサポートされているリモート プロシージャコールの種類と、gRPC での対応するリモート プロシージャ コールの種類のレビュー
ms.date: 09/02/2019
ms.openlocfilehash: b9d4ce7cae693ed7904229483cbccfe3b299b640
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79401785"
---
# <a name="types-of-rpc"></a>RPC の種類

Windows 通信基盤 (WCF) 開発者として、次の種類のリモート プロシージャ コール (RPC) を扱うことに慣れている可能性があります。

- リクエスト/返信
- 二重：
  - セッションを使用した一方向の二重化
  - 全二重 (セッションを使用)
- 一方向

これらの RPC 型は、既存の gRPC の概念にかなり自然に対応できます。 この章では、これらの各領域を順番に説明します。 [第5章](migrate-wcf-to-grpc.md)では、同様の例をより詳しく見ていきます。

| WCF | gRPC |
| --- | ---- |
| 通常の要求/応答 | 単項演算子 |
| クライアント コールバック インターフェイスを使用したセッションを使用した双方向サービス | サーバー ストリーミング |
| セッションを使用した全二重サービス | 双方向ストリーミング |
| 一方向操作 | クライアント ストリーミング |

## <a name="requestreply"></a>リクエスト/返信

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

ご覧のとおり、gRPC 単項 RPC サービス メソッドの実装は、WCF 操作の実装と同様です。 違いは、gRPC では、インターフェイスを実装する代わりに基本クラスのメソッドをオーバーライドする点です。 サーバーでは、gRPC 基本メソッドは常<xref:System.Threading.Tasks.Task%601>に 戻りますが、クライアントはサービスを呼び出すために非同期メソッドとブロッキングメソッドの両方を提供します。

## <a name="wcf-duplex-one-way-to-client"></a>WCF 二重方式(クライアントへの 1 つの方法)

WCF アプリケーション (特定のバインディング) は、クライアントとサーバー間の永続的な接続を作成できます。 サーバーは、プロパティで指定された*コールバック インターフェイス*を使用して、接続が閉じられるまで、クライアントに非同期的<xref:System.ServiceModel.ServiceContractAttribute.CallbackContract%2A?displayProperty=nameWithType>にデータを送信できます。

gRPC サービスは、メッセージ ストリームと同様の機能を提供します。 ストリームは、実装の面で WCF 双方向サービスに*正確*にマップされませんが、同じ結果を得ることができます。

### <a name="grpc-streaming"></a>gRPC ストリーミング

gRPC は、クライアントからサーバーへ、およびサーバーからクライアントへの永続ストリームの作成をサポートします。 両方の種類のストリームを同時にアクティブにできます。 この機能は双方向ストリーミングと呼ばれます。

ストリームは、時間の経過と同時に任意の非同期メッセージングに使用できます。 また、1 つの要求または応答で生成および送信するには大きすぎる大きなデータセットを渡すために使用することもできます。

次の例は、サーバー ストリーミング RPC を示しています。

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

このサーバー ストリームは、次のコードに示すように、クライアント アプリケーションから使用できます。

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
> サーバー ストリーミング RPC は、サブスクリプション スタイルのサービスに役立ちます。 また、メモリ内でデータセット全体を構築することが非効率的または不可能な場合に、大規模なデータセットを送信する場合にも役立ちます。 ただし、ストリーミング応答は、単一のメッセージでフィールドを`repeated`送信するほど高速ではありません。 原則として、小規模なデータセットにストリーミングを使用しないでください。

### <a name="differences-from-wcf"></a>WCF との違い

WCF 双方向サービスは、複数のメソッドを持つことができるクライアント コールバック インターフェイスを使用します。 gRPC サーバー ストリーミング サービスは、単一のストリームを介してメッセージを送信することしかできません。 複数のメソッドが必要な場合は[、Any フィールドまたはいずれかのフィールド](protobuf-any-oneof.md)でメッセージの種類を使用して異なるメッセージを送信し、クライアントでメッセージを処理するコードを記述します。

WCF では、セッションを持つ[ServiceContract](xref:System.ServiceModel.ServiceContractAttribute)クラスは、接続が閉じられるまで存続します。 セッション内で複数のメソッドを呼び出すことができます。 gRPC では、`Task`実装メソッドが返すが、接続が閉じられるまで終了しないはずです。

## <a name="wcf-one-way-operations-and-grpc-client-streaming"></a>WCF の一方向操作と gRPC クライアント ストリーミング

WCF には、トランスポート固有の確認を`[OperationContract(IsOneWay = true)]`返す一方向の操作 (でマーク) が用意されています。 gRPC サービスメソッドは、空であっても常に応答を返します。 クライアントは常にその応答を待つ必要があります。 gRPC でのメッセージングの "ファイアアンドフォーゲット" スタイルでは、クライアント ストリーミング サービスを作成できます。

### <a name="thing_logproto"></a>thing_log.プロト

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

前の例に示すように、クライアント ストリーミング RPC を使用して、ファイア アンド フォーゲット メッセージングを行うことができます。 また、非常に大きなデータセットをサーバーに送信するためにも使用できます。 パフォーマンスに関する警告は、小さいデータセットの場合は`repeated`、通常のメッセージでフィールドを使用するという警告が適用されます。

## <a name="wcf-full-duplex-services"></a>WCF 全二重サービス

WCF 双方向バインディングは、サービス インターフェイスとクライアント コールバック インターフェイスの両方で複数の一方向操作をサポートします。 このサポートにより、クライアントとサーバー間の継続的な会話が可能になります。 gRPC は双方向ストリーミング RPC と同様のものをサポートしており、両方のパラメータに修飾子`stream`が付いています。

### <a name="chatproto"></a>チャット.プロト

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

前の例では、実装メソッドが要求ストリーム ( ) と応答ストリーム`IAsyncStreamReader<MessageRequest>`( )`IServerStreamWriter<MessageResponse>`の両方を受け取ることがわかります。 このメソッドは、接続が閉じられるまでメッセージの読み取りと書き込みを行うことができます。

### <a name="chatter-client"></a>チャタクライアント

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
>[前次](wcf-bindings.md)
>[Next](metadata.md)
