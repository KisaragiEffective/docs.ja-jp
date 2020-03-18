---
title: Wcf 開発者向けの gRPC-gRPC への WCF 双方向サービスの移行
description: さまざまな形式の WCF 双方向サービスを gRPC streaming services に移行する方法について説明します。
ms.date: 09/02/2019
ms.openlocfilehash: 5737f02044ab9e4064f632164db764541a9c4d31
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628541"
---
# <a name="migrate-wcf-duplex-services-to-grpc"></a>WCF 双方向サービスを gRPC に移行する

ここでは、基本的な概念を理解しました。このセクションでは、より複雑な*ストリーミング*grpc サービスについて説明します。

Windows Communication Foundation (WCF) で双方向サービスを使用するには、複数の方法があります。 一部のサービスはクライアントによって開始され、サーバーからデータをストリーミングします。 その他の全二重サービスには、WCF ドキュメントにある従来の電卓の例のように、継続的な双方向通信が含まれる場合があります。 この章では、2つの WCF stock ティッカー実装を実行し、gRPC に移行します。1つはサーバーストリーミング RPC を使用し、もう1つは双方向ストリーミング RPC を使用します。

## <a name="server-streaming-rpc"></a>サーバーストリーミング RPC

SimpleStockPriceTicker の[サンプル SimpleStockTicker WCF ソリューション](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/SimpleStockTickerSample/wcf/SimpleStockTicker)では、クライアントが株価記号の一覧を使用して接続を開始する双方向サービスがあり、サーバーは*コールバックインターフェイス*を使用して更新プログラムが使用可能になったときにそれらを送信します。 クライアントは、サーバーからの呼び出しに応答するために、そのインターフェイスを実装します。

### <a name="the-wcf-solution"></a>WCF ソリューション

WCF ソリューションは、自己ホスト型の Net.tcp サーバーとして .NET Framework 4 に実装されています。*x*コンソールアプリケーション。

#### <a name="servicecontract"></a>ServiceContract

```csharp
[ServiceContract(SessionMode = SessionMode.Required, CallbackContract = typeof(ISimpleStockTickerCallback))]
public interface ISimpleStockTickerService
{
    [OperationContract(IsOneWay = true)]
    void Subscribe(string[] symbols);
}
```

サービスは、コールバックインターフェイス `ISimpleStockTickerCallback` を使用してリアルタイムでクライアントにデータを送信するため、戻り値の型のない1つのメソッドがあります。

#### <a name="the-callback-interface"></a>コールバックインターフェイス

```csharp
[ServiceContract]
public interface ISimpleStockTickerCallback
{
    [OperationContract(IsOneWay = true)]
    void Update(string symbol, decimal price);
}
```

これらのインターフェイスの実装と、フェイク外部依存関係を使用してテストデータを提供できます。

### <a name="grpc-streaming"></a>gRPC ストリーミング

リアルタイムデータを処理するための gRPC プロセスは、WCF プロセスとは異なります。 クライアントからサーバーへの呼び出しでは、永続的なストリームを作成できます。このストリームは、非同期的に到着するメッセージを監視できます。 違いにかかわらず、ストリームは、このデータを処理するためのより直観的な方法であり、LINQ、リアクティブストリーム、関数型プログラミングなどを重視する最新のプログラミングにおいてさらに関連性があります。

サービス定義には2つのメッセージが必要です。1つは要求用で、もう1つはストリーム用です。 サービスは、`return` 宣言内の `stream` キーワードを使用して `StockTickerUpdate` メッセージのストリームを返します。 更新プログラムに `Timestamp` を追加して、価格変更の正確な時間を示すことをお勧めします。

#### <a name="simple_stock_tickerproto"></a>simple_stock_ticker. proto

```protobuf
syntax = "proto3";

option csharp_namespace = "TraderSys.SimpleStockTickerServer.Protos";

import "google/protobuf/timestamp.proto";

package SimpleStockTickerServer;

service SimpleStockTicker {
  rpc Subscribe (SubscribeRequest) returns (stream StockTickerUpdate);
}

message SubscribeRequest {
  repeated string symbols = 1;
}

message StockTickerUpdate {
  string symbol = 1;
  int32 price_cents = 2;
  google.protobuf.Timestamp time = 3;
}
```

### <a name="implement-simplestockticker"></a>SimpleStockTicker を実装する

`TraderSys.StockMarket` クラスライブラリからターゲットソリューションの新しい .NET Standard クラスライブラリに3つのクラスをコピーして、WCF プロジェクトの偽の `StockPriceSubscriber` を再利用します。 ベストプラクティスに従うには、`Factory` の種類を追加してインスタンスを作成し、`IStockPriceSubscriberFactory` を ASP.NET Core 依存関係挿入サービスに登録します。

#### <a name="the-factory-implementation"></a>ファクトリ実装

```csharp
public interface IStockPriceSubscriberFactory
{
    IStockPriceSubscriber GetSubscriber(string[] symbols);
}

public class StockPriceSubscriberFactory : IStockPriceSubscriberFactory
{
    public IStockPriceSubscriber GetSubscriber(string[] symbols)
    {
        return new StockPriceSubscriber(symbols);
    }
}
```

#### <a name="register-the-factory"></a>ファクトリの登録

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton<IStockPriceSubscriberFactory, StockPriceSubscriberFactory>();
}
```

これで、このクラスを使用して gRPC `StockTickerService`を実装できるようになりました。

#### <a name="stocktickerservicecs"></a>StockTickerService.cs

```csharp
public class StockTickerService : Protos.SimpleStockTicker.SimpleStockTickerBase
{
    private readonly IStockPriceSubscriberFactory _subscriberFactory;

    public StockTickerService(IStockPriceSubscriberFactory subscriberFactory)
    {
        _subscriberFactory = subscriberFactory;
    }

    public override async Task Subscribe(SubscribeRequest request, IServerStreamWriter<StockTickerUpdate> responseStream, ServerCallContext context)
    {
        var subscriber = _subscriberFactory.GetSubscriber(request.Symbols.ToArray());

        subscriber.Update += async (sender, args) =>
            await WriteUpdateAsync(responseStream, args.Symbol, args.Price);

        await AwaitCancellation(context.CancellationToken);
    }

    private async Task WriteUpdateAsync(IServerStreamWriter<StockTickerUpdate> stream, string symbol, decimal price)
    {
        try
        {
            await stream.WriteAsync(new StockTickerUpdate
            {
                Symbol = symbol,
                PriceCents = (int)(price * 100),
                Time = Timestamp.FromDateTimeOffset(DateTimeOffset.UtcNow)
            });
        }
        catch (Exception e)
        {
            // Handle any errors caused by broken connection, etc.
            _logger.LogError($"Failed to write message: {e.Message}");
        }
    }

    private static Task AwaitCancellation(CancellationToken token)
    {
        var completion = new TaskCompletionSource<object>();
        token.Register(() => completion.SetResult(null));
        return completion.Task;
    }
}
```

ご覧のように、`.proto` ファイルの宣言では、メソッドが `StockTickerUpdate` メッセージのストリームを返すことが示されていますが、実際には `Task`を返します。 ストリームの作成ジョブは、生成されたコードと、`IServerStreamWriter<StockTickerUpdate>` 応答ストリームを提供する gRPC ランタイムライブラリによって処理され、すぐに使用できるようになります。

接続が開いている間にサービスクラスのインスタンスが保持されている WCF 双方向サービスとは異なり、gRPC サービスは返されたタスクを使用してサービスを維持します。 接続が閉じられるまで、タスクは完了しません。

サービスは、`ServerCallContext`の `CancellationToken` を使用して、クライアントが接続を閉じたことを通知できます。 単純な静的メソッド `AwaitCancellation`は、トークンが取り消されたときに完了するタスクを作成するために使用されます。

`Subscribe` メソッドで、`StockPriceSubscriber` を取得し、応答ストリームに書き込むイベントハンドラーを追加します。 次に、接続が閉じられるのを待ってから、`subscriber` を直ちに破棄して、閉じたストリームにデータを書き込もうとしないようにします。

`WriteUpdateAsync` メソッドには、ストリームにメッセージが書き込まれたときに発生する可能性のあるエラーを処理するための `try`/`catch` ブロックがあります。 この考慮事項は、意図的に、またはどこかの障害が原因で、任意のミリ秒で中断される可能性があるネットワーク経由の永続的な接続において重要です。

### <a name="use-stocktickerservice-from-a-client-application"></a>クライアントアプリケーションからの Stockkerservice の使用

前のセクションと同じ手順に従って、`.proto` ファイルから共有可能なクライアントクラスライブラリを作成します。 このサンプルには、クライアントの使用方法を示す .NET Core 3.0 コンソールアプリケーションが用意されています。

#### <a name="example-programcs"></a>Program.cs の例

```csharp
class Program
{
    static async Task Main(string[] args)
    {
        using var channel = GrpcChannel.ForAddress("https://localhost:5001");
        var client = new SimpleStockTicker.SimpleStockTickerClient(channel);

        var request = new SubscribeRequest();
        request.Symbols.AddRange(args);

        using var stream = client.Subscribe(request);

        var tokenSource = new CancellationTokenSource();

        var task = DisplayAsync(stream.ResponseStream, tokenSource.Token);

        WaitForExitKey();

        tokenSource.Cancel();
        await task;
    }
}
```

この場合、生成されたクライアントの `Subscribe` メソッドは非同期ではありません。 `MoveNext` メソッドは非同期であり、初めて呼び出されたときは、接続が有効になるまで完了しません。そのため、ストリームが作成され、すぐに使用できます。

ストリームは、非同期 `DisplayAsync` メソッドに渡されます。 次に、ユーザーがキーを押すのを待機した後、`DisplayAsync` メソッドをキャンセルし、終了する前にタスクが完了するまで待機します。

> [!NOTE]
> このコードでは、 C#新しい 8 `using` 宣言構文を使用して、`Main` メソッドの終了時にストリームとチャネルを破棄します。 これは小さな変更ですが、インデントや空の線を減らすことができます。

#### <a name="consume-the-stream"></a>ストリームを使用する

WCF では、コールバックインターフェイスを使用して、サーバーがクライアント上でメソッドを直接呼び出すことができます。 gRPC ストリームの動作は異なります。 クライアントは、返されたストリームに対して反復処理を行い、`IEnumerable`を返すローカルメソッドから返された場合と同様にメッセージを処理します。

`IAsyncStreamReader<T>` 型は、`IEnumerator<T>`とよく似ています。 データが多い限り true を返す `MoveNext` メソッドと、最新の値を返す `Current` のプロパティがあります。 唯一の違いは、`MoveNext` メソッドは `bool`だけではなく `Task<bool>` を返すという点です。 `ReadAllAsync` 拡張メソッドは、新しい `await foreach` 構文で使用C#できる標準 8 `IAsyncEnumerable` でストリームをラップします。

```csharp
static async Task DisplayAsync(IAsyncStreamReader<StockTickerUpdate> stream, CancellationToken token)
{
    try
    {
        await foreach (var update in stream.ReadAllAsync(token))
        {
            Console.WriteLine($"{update.Symbol}: {update.Price}");
        }
    }
    catch (RpcException e) when (e.StatusCode == StatusCode.Cancelled)
    {
        return;
    }
    catch (OperationCanceledException)
    {
        Console.WriteLine("Finished.");
    }
}
```

> [!TIP]
> 事後対応型のプログラミングパターンを使用する開発者にとっては、この章の最後にある「[クライアントライブラリ](client-libraries.md#iobservable)」のセクションでは、`IObservable<T>`で `IAsyncStreamReader<T>` をラップする拡張メソッドとクラスを追加する方法を示しています。

ここでも、ネットワークエラーが発生する可能性があるため、ここで例外をキャッチしてください。また、コードが <xref:System.Threading.CancellationToken> を使用してループを中断しているためにスローされる <xref:System.OperationCanceledException> があるため、必ず例外をキャッチしてください。 `RpcException` 型には、`StatusCode`を含む gRPC ランタイムエラーに関する有用な情報が多数含まれています。 詳細については、[第4章の「*エラー処理*](error-handling.md)」を参照してください。

## <a name="bidirectional-streaming"></a>双方向ストリーミング

WCF の全二重サービスを使用すると、非同期のリアルタイムメッセージングを双方向で実現できます。 サーバーストリーミングの例では、クライアントが要求を開始し、更新のストリームを受信します。 適切なバージョンのサービスを使用すると、クライアントは、停止して新しいサブスクリプションを作成することなく、一覧に対して株式の追加や削除を行うことができます。 この機能は、 [Fullstockticker サンプルソリューション](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/FullStockTickerSample/wcf/FullStockTicker)に実装されています。

`IFullStockTickerService` インターフェイスには、次の3つの方法があります。

- `Subscribe` 接続を開始します。
- `AddSymbol` は、ウォッチするストックシンボルを追加します。
- `RemoveSymbol` は、マークされた一覧からシンボルを削除します。

```csharp
[ServiceContract(SessionMode = SessionMode.Required, CallbackContract = typeof(IFullStockTickerCallback))]
public interface IFullStockTickerService
{
    [OperationContract(IsOneWay = true)]
    void Subscribe();

    [OperationContract(IsOneWay = true)]
    void AddSymbol(string symbol);

    [OperationContract(IsOneWay = true)]
    void RemoveSymbol(string symbol);
}
```

コールバックインターフェイスは同じままです。

このパターンを gRPC に実装するのは簡単ではありません。これは、1つはクライアントからサーバー、もう1つはサーバーからクライアントの2つのデータストリームが渡されるためです。 複数のメソッドを使用して追加と削除の操作を実装することはできませんが、1つのストリームに複数の種類のメッセージを渡すことはできません。そのためには、[第3章](protobuf-any-oneof.md)で説明した `Any` 型または `oneof` キーワードを使用します。

許容される型のセットがある場合は、`oneof` を使用することをお勧めします。 `AddSymbolRequest` または `RemoveSymbolRequest`のいずれかを保持できる `ActionMessage` を使用します。

```protobuf
message ActionMessage {
  oneof action {
    AddSymbolRequest add = 1;
    RemoveSymbolRequest remove = 2;
  }
}

message AddSymbolRequest {
  string symbol = 1;
}

message RemoveSymbolRequest {
  string symbol = 1;
}
```

`ActionMessage` メッセージのストリームを受け取る双方向ストリーミングサービスを宣言します。

```protobuf
service FullStockTicker {
  rpc Subscribe (stream ActionMessage) returns (stream StockTickerUpdate);
}
```

このサービスの実装は、前の例と似ていますが、`Subscribe` メソッドの最初のパラメーターが `IAsyncStreamReader<ActionMessage>`になり、`Add` と `Remove` の要求を処理するために使用できるようになりました。

```csharp
public override async Task Subscribe(IAsyncStreamReader<ActionMessage> requestStream, IServerStreamWriter<StockTickerUpdate> responseStream, ServerCallContext context)
{
    using var subscriber = _subscriberFactory.GetSubscriber();

    subscriber.Update += async (sender, args) =>
        await WriteUpdateAsync(responseStream, args.Symbol, args.Price);

    var actionsTask = HandleActions(requestStream, subscriber, context.CancellationToken);

    _logger.LogInformation("Subscription started.");
    await AwaitCancellation(context.CancellationToken);

    try { await actionsTask; } catch { /* Ignored */ }

    _logger.LogInformation("Subscription finished.");
}

private async Task WriteUpdateAsync(IServerStreamWriter<StockTickerUpdate> stream, string symbol, decimal price)
{
    try
    {
        await stream.WriteAsync(new StockTickerUpdate
        {
            Symbol = symbol,
            PriceCents = (int)(price * 100),
            Time = Timestamp.FromDateTimeOffset(DateTimeOffset.UtcNow)
        });
    }
    catch (Exception e)
    {
        // Handle any errors caused by broken connection, etc.
        _logger.LogError($"Failed to write message: {e.Message}");
    }
}

private static Task AwaitCancellation(CancellationToken token)
{
    var completion = new TaskCompletionSource<object>();
    token.Register(() => completion.SetResult(null));
    return completion.Task;
}
```

GRPC が生成した `ActionMessage` クラスは、`Add` プロパティと `Remove` プロパティのどちらか1つだけを設定できることを保証します。 どのような種類のメッセージが使用されているかを判断するには、`null` ていないものを見つける方法が有効です。 また、コード生成によって `ActionMessage` クラスに `enum ActionOneOfCase` が作成されました。これは次のようになります。

```csharp
public enum ActionOneofCase {
    None = 0,
    Add = 1,
    Remove = 2,
}
```

`ActionMessage` オブジェクトのプロパティ `ActionCase` を `switch` ステートメントと共に使用して、どのフィールドが設定されているかを判断できます。

```csharp
private async Task HandleActions(IAsyncStreamReader<ActionMessage> requestStream, IFullStockPriceSubscriber subscriber, CancellationToken token)
{
    await foreach (var action in requestStream.ReadAllAsync(token))
    {
        switch (action.ActionCase)
        {
            case ActionMessage.ActionOneofCase.None:
                _logger.LogWarning("No Action specified.");
                break;
            case ActionMessage.ActionOneofCase.Add:
                subscriber.Add(action.Add.Symbol);
                break;
            case ActionMessage.ActionOneofCase.Remove:
                subscriber.Remove(action.Remove.Symbol);
                break;
            default:
                _logger.LogWarning($"Unknown Action '{action.ActionCase}'.");
                break;
        }
    }
}
```

> [!TIP]
> `switch` ステートメントには、不明な `ActionOneOfCase` 値が検出された場合に警告をログに記録する `default` ケースがあります。 これは、より多くのアクションを追加した、新しいバージョンの `.proto` ファイルをクライアントが使用していることを示す場合に便利です。 これは、既知のフィールドで `null` をテストするよりも `switch` を使用する方がよい理由の1つです。

### <a name="use-fullstocktickerservice-from-a-client-application"></a>クライアントアプリケーションからの FullStockTickerService の使用

この複雑なクライアントの使用方法を示す単純な .NET Core 3.0 WPF アプリケーションがあります。 完全なアプリケーションについては、 [GitHub](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/FullStockTickerSample/grpc/FullStockTicker)を参照してください。

クライアントは `MainWindowViewModel` クラスで使用されます。このクラスは、依存関係の挿入から `FullStockTicker.FullStockTickerClient` 型のインスタンスを取得します。

```csharp
public class MainWindowViewModel : IAsyncDisposable, INotifyPropertyChanged
{
    private readonly FullStockTicker.FullStockTickerClient _client;
    private readonly AsyncDuplexStreamingCall<ActionMessage, StockTickerUpdate> _duplexStream;
    private readonly CancellationTokenSource _cancellationTokenSource;
    private readonly Task _responseTask;
    private string _addSymbol;

    public MainWindowViewModel(FullStockTicker.FullStockTickerClient client)
    {
        _cancellationTokenSource = new CancellationTokenSource();
        _client = client;
        _duplexStream = _client.Subscribe();
        _responseTask = HandleResponsesAsync(_cancellationTokenSource.Token);

        AddCommand = new AsyncCommand(Add, CanAdd);
    }
```

`client.Subscribe()` メソッドによって返されるオブジェクトは、gRPC ライブラリ型 `AsyncDuplexStreamingCall<TRequest, TResponse>`のインスタンスになりました。これにより、サーバーに要求を送信するための `RequestStream` と、応答を処理するための `ResponseStream` が提供されます。

要求ストリームは、いくつかの WPF `ICommand` メソッドから、シンボルを追加および削除するために使用されます。 各操作について、`ActionMessage` オブジェクトの関連フィールドを設定します。

```csharp
private async Task Add()
{
    if (CanAdd())
    {
        await _duplexStream.RequestStream.WriteAsync(new ActionMessage {Add = new AddSymbolRequest {Symbol = AddSymbol}});
    }
}

public async Task Remove(PriceViewModel priceViewModel)
{
    await _duplexStream.RequestStream.WriteAsync(new ActionMessage {Remove = new RemoveSymbolRequest {Symbol = priceViewModel.Symbol}});
    Prices.Remove(priceViewModel);
}
```

> [!IMPORTANT]
> メッセージに `oneof` フィールドの値を設定すると、以前に設定されていたすべてのフィールドが自動的にクリアされます。

応答のストリームは `async` メソッドで処理されます。 返される `Task` は、ウィンドウが閉じられたときに破棄されるように保持されます。

```csharp
private async Task HandleResponsesAsync(CancellationToken token)
{
    var stream = _duplexStream.ResponseStream;

    try
    {
        await foreach (var update in stream.ReadAllAsync(token))
        {
            var price = Prices.FirstOrDefault(p => p.Symbol.Equals(update.Symbol));
            if (price == null)
            {
                price = new PriceViewModel(this) {Symbol = update.Symbol, Price = update.PriceCents / 100m};
                Prices.Add(price);
            }
            else
            {
                price.Price = update.PriceCents / 100m;
            }
        }
    }
    catch (OperationCancelledException) { }
}
```

### <a name="client-cleanup"></a>クライアントのクリーンアップ

ウィンドウが閉じられ、(`MainWindow`の `Closed` イベントから) `MainWindowViewModel` が破棄された場合は、`AsyncDuplexStreamingCall` オブジェクトを適切に破棄することをお勧めします。 特に、サーバー上のストリームを適切に閉じるには、`RequestStream` の `CompleteAsync` メソッドを呼び出す必要があります。 この例は、サンプルビューモデルの `DisposeAsync` メソッドを示しています。

```csharp
public async ValueTask DisposeAsync()
{
    try
    {
        await _duplexStream.RequestStream.CompleteAsync().ConfigureAwait(false);
        await _responseTask.ConfigureAwait(false);
    }
    finally
    {
        _duplexStream.Dispose();
    }
}
```

要求ストリームを閉じると、サーバーは独自のリソースを適時に破棄できます。 これにより、サービスの効率とスケーラビリティが向上し、例外が回避されます。

>[!div class="step-by-step"]
>[前へ](migrate-request-reply.md)
>[次へ](streaming-versus-repeated.md)
