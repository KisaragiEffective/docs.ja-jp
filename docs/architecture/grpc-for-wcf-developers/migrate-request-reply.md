---
title: Wcf 開発者向けの gRPC-gRPC への WCF 要求/応答サービスの移行
description: 単純な要求/応答サービスを WCF から gRPC に移行する方法について説明します。
ms.date: 09/02/2019
ms.openlocfilehash: 018aa94a15cdcb1e0f559afb7b3a88cd4f915398
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628554"
---
# <a name="migrate-a-wcf-request-reply-service-to-a-grpc-unary-rpc"></a>WCF 要求/応答サービスを gRPC 単項 RPC に移行する

このセクションでは、WCF の基本的な要求/応答サービスを ASP.NET Core gRPC の単項 RPC サービスに移行する方法について説明します。 これらのサービスは、Windows Communication Foundation (WCF) と gRPC の両方で最も単純なサービスの種類であるため、開始するのが優れた場所です。 サービスを移行した後、.NET クライアントアプリケーションからサービスを使用するために、同じ `.proto` ファイルからクライアントライブラリを生成する方法について説明します。

## <a name="the-wcf-solution"></a>WCF ソリューション

[PortfoliosSample ソリューション](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/PortfoliosSample/wcf/TraderSys)には、単一のポートフォリオまたは特定の商人のすべてのポートフォリオをダウンロードする単純な要求/応答ポートフォリオサービスが含まれています。 このサービスは、インターフェイス `IPortfolioService` `ServiceContract` 属性で定義されています。

```csharp
[ServiceContract]
public interface IPortfolioService
{
    [OperationContract]
    Task<Portfolio> Get(Guid traderId, int portfolioId);

    [OperationContract]
    Task<List<Portfolio>> GetAll(Guid traderId);
}
```

`Portfolio` モデルは、 [DataContract](xref:System.Runtime.Serialization.DataContractAttribute)でC#マークされた単純なクラスで、`PortfolioItem` オブジェクトの一覧を含みます。 これらのモデルは、`TraderSys.PortfolioData` プロジェクトで、データアクセスの抽象化を表すリポジトリクラスと共に定義されます。

```csharp
[DataContract]
public class Portfolio
{
    [DataMember]
    public int Id { get; set; }

    [DataMember]
    public Guid TraderId { get; set; }

    [DataMember]
    public List<PortfolioItem> Items { get; set; }
}

[DataContract]
public class PortfolioItem
{
    [DataMember]
    public int Id { get; set; }

    [DataMember]
    public int ShareId { get; set; }

    [DataMember]
    public int Holding { get; set; }

    [DataMember]
    public decimal Cost { get; set; }
}
```

`ServiceContract` の実装では、`DataContract` 型のインスタンスを返す依存関係の挿入によって提供されるリポジトリクラスを使用します。

```csharp
public class PortfolioService : IPortfolioService
{
    private readonly IPortfolioRepository _repository;

    public PortfolioService(IPortfolioRepository repository)
    {
        _repository = repository;
    }

    public async Task<Portfolio> Get(Guid traderId, int portfolioId)
    {
        return await _repository.GetAsync(traderId, portfolioId);
    }

    public async Task<List<Portfolio>> GetAll(Guid traderId)
    {
        return await _repository.GetAllAsync(traderId);
    }
}
```

## <a name="the-portfoliosproto-file"></a>ポートフォリオのプロトコルファイル

前のセクションの手順に従っている場合は、次のような `portfolios.proto` ファイルを含む gRPC プロジェクトが必要です。

```protobuf
syntax = "proto3";

option csharp_namespace = "TraderSys.Portfolios.Protos";

package PortfolioServer;

service Portfolios {
  // RPCs will go here
}
```

最初の手順では、`DataContract` クラスを同等の Protobuf に移行します。

## <a name="convert-the-datacontract-classes-to-grpc-messages"></a>DataContract クラスを gRPC メッセージに変換します

`Portfolio` クラスが依存しているため、`PortfolioItem` クラスは最初に Protobuf メッセージに変換されます。 クラスは単純で、3つのプロパティは gRPC データ型に直接マップされます。 購入時に共有に対して支払われた価格を表す `Cost` プロパティは `decimal` のフィールドです。 gRPC は、`float` または `double` のみをサポートしています。これは、通貨には適していません。 共有価格は最低1セントであるため、コストはセントの `int32` として表すことができます。

> [!NOTE]
> `.proto` ファイル内のフィールド名には、必ずキャメルケースを使用してください。 C#コードジェネレーターはこれらをユーザーに対してパスワード変換します。他の言語のユーザーは、異なるコーディング基準を使用することに感謝します。

```protobuf
message PortfolioItem {
    int32 id = 1;
    int32 share_id = 2;
    int32 holding = 3;
    int32 cost_cents = 4;
}
```

`Portfolio` クラスは少し複雑です。 WCF コードでは、開発者は `TraderId` プロパティに `Guid` を使用し、`List<PortfolioItem>`を格納しています。 ファーストクラスの `UUID` 型を持たない Protobuf では、`traderId` フィールドに `string` を使用し、独自のコードで解析する必要があります。 項目の一覧を表示するには、フィールドで `repeated` キーワードを使用します。

```protobuf
message Portfolio {
    int32 id = 1;
    string trader_id = 2;
    repeated PortfolioItem items = 3;
}
```

データメッセージを取得したので、サービス RPC エンドポイントを宣言できます。

## <a name="convert-servicecontract-to-a-grpc-service"></a>ServiceContract を gRPC サービスに変換する

WCF `Get` メソッドは、`Guid traderId` と `int portfolioId`の2つのパラメーターを受け取ります。 gRPC サービスメソッドは、1つのパラメーターのみを受け取ることができるため、2つの値を格納するメッセージを作成する必要があります。 これらの要求オブジェクトには、メソッドと同じ名前を付け、その後にサフィックス `Request`を付けるのが一般的です。 ここでも、`string` は `Guid`ではなく `traderId` フィールドに使用されています。

サービスは `Portfolio` メッセージを直接返すこともできましたが、今後は旧バージョンとの互換性に影響する可能性があります。 サービスのメソッドごとに個別の `Request` と `Response` メッセージを定義することをお勧めします。これは、その多くが同じであっても同じです。 そのため、1つの `Portfolio` フィールドで `GetResponse` メッセージを宣言します。

次の例では、`GetRequest` メッセージを使用した gRPC サービスメソッドの宣言を示します。

```protobuf
message GetRequest {
    string trader_id = 1;
    int32 portfolio_id = 2;
}

message GetResponse {
    Portfolio portfolio = 1;
}

service Portfolios {
    rpc Get(GetRequest) returns (GetResponse);
}
```

WCF `GetAll` メソッドは、1つのパラメーター (`traderId`) のみを受け取るので、パラメーターの型として `string` を指定できるように思われるかもしれません。 ただし、gRPC には定義済みのメッセージ型が必要です。 この要件は、今後の旧バージョンとの互換性のために、すべての入力と出力にカスタムメッセージを使用する方法を適用するのに役立ちます。

また、WCF メソッドは `List<Portfolio>`も返しますが、単純なパラメーター型を許可しないのと同じ理由で、gRPC は戻り値の型として `repeated Portfolio` を許可しません。 代わりに、`GetAllResponse` の種類を作成してリストをラップします。

> [!WARNING]
> `PortfolioList` メッセージを作成したり、類似したものを使用して複数のサービス方法で使用したりすることがありますが、このようなことは避けてください。 サービスのさまざまなメソッドがどのように進化しているかを知ることはできません。そのため、メッセージを明確にして明確に分離することができます。

```protobuf
message GetAllRequest {
    string trader_id = 1;
}

message PortfolioList {
    repeated Portfolio portfolios = 1;
}

service Portfolios {
    rpc Get(GetRequest) returns (Portfolio);
    rpc GetAll(GetAllRequest) returns (GetAllResponse);
}
```

これらの変更を使用してプロジェクトを保存すると、gRPC ビルドターゲットがバックグラウンドで実行され、すべての Protobuf メッセージ型と、サービスを実装するために継承できる基本クラスが生成されます。

`Services/GreeterService.cs` クラスを開き、コード例を削除します。 これで、ポートフォリオサービスの実装を追加できるようになりました。 生成された基底クラスは `Protos` 名前空間にあり、入れ子になったクラスとして生成されます。 gRPC は、`.proto` ファイルにサービスと同じ名前の静的クラスを作成し、その静的クラス内に `Base` サフィックスを持つ基底クラスを作成します。そのため、基本型の完全な識別子は `TraderSys.Portfolios.Protos.Portfolios.PortfoliosBase`ます。

```csharp
namespace TraderSys.Portfolios.Services
{
    public class PortfolioService : Protos.Portfolios.PortfoliosBase
    {
    }
}
```

基本クラスは、サービスを実装するためにオーバーライドできる `Get` および `GetAll` の `virtual` メソッドを宣言します。 メソッドは、`abstract` ではなく `virtual` ので、実装していない場合は、通常C#のコードで `NotImplementedException` をスローするのと同じように、サービスは明示的な grpc `Unimplemented` 状態コードを返すことができます。

ASP.NET Core 内のすべての gRPC 単項サービスメソッドの署名が一貫しています。 2つのパラメーターがあります。1つ目は `.proto` ファイルで宣言されたメッセージ型、2つ目は ASP.NET Core の `HttpContext` と同様に機能する `ServerCallContext` です。 実際には、基になる `HttpContext`を取得するために使用できる `ServerCallContext` クラスに `GetHttpContext` という拡張メソッドがありますが、頻繁に使用する必要はありません。 この章の後半の `ServerCallContext` と、認証について説明している章を参照してください。

メソッドの戻り値の型は `Task<T>`であり、`T` は応答メッセージの種類です。 すべての gRPC サービスメソッドは非同期です。

## <a name="migrate-the-portfoliodata-library-to-net-core"></a>PortfolioData ライブラリを .NET Core に移行する

この時点で、プロジェクトには、WCF ソリューションの `TraderSys.PortfolioData` クラスライブラリに含まれるポートフォリオリポジトリとモデルが必要です。 新しいクラスライブラリを作成する最も簡単な方法は、Visual Studio の **[新しいプロジェクト]** ダイアログボックスでクラスライブラリ (.NET Standard) テンプレートを使用するか、コマンドラインで .NET Core CLI を使用して、`TraderSys.sln` ファイルが格納されているディレクトリから次のコマンドを実行する方法です。

```dotnetcli
dotnet new classlib -o src/TraderSys.PortfolioData
dotnet sln add src/TraderSys.PortfolioData
```

ライブラリを作成してソリューションに追加したら、生成された `Class1.cs` ファイルを削除し、WCF ソリューションのライブラリのファイルを新しいクラスライブラリのフォルダーにコピーして、フォルダー構造を保持します。

```
Models
  Portfolio.cs
  PortfolioItem.cs
IPortfolioRepository.cs
PortfolioRepository.cs
```

SDK スタイルの .NET プロジェクトでは、任意の `.cs` ファイルが独自のディレクトリに自動的に含まれるため、プロジェクトに明示的に追加する必要はありません。 残りの手順は、`DataContract` と `DataMember` 属性を `Portfolio` クラスと `PortfolioItem` クラスから削除することです。これにC#より、これらは単純な古いクラスになります。

```csharp
public class Portfolio
{
    public int Id { get; set; }
    public Guid TraderId { get; set; }
    public List<PortfolioItem> Items { get; set; }
}

public class PortfolioItem
{
    public int Id { get; set; }
    public int ShareId { get; set; }
    public int Holding { get; set; }
    public decimal Cost { get; set; }
}
```

## <a name="use-aspnet-core-dependency-injection"></a>ASP.NET Core 依存関係の挿入の使用

これで、このライブラリへの参照を gRPC アプリケーションプロジェクトに追加し、gRPC サービス実装で依存関係の挿入を使用して `PortfolioRepository` クラスを使用できるようになりました。 WCF アプリケーションでは、依存関係の注入は Autofac IoC コンテナーによって提供されていました。 ASP.NET Core には依存関係の注入が組み込まれています。 `Startup` クラスの `ConfigureServices` メソッドにリポジトリを登録できます。

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        // Register the repository class as a scoped service (instance per request)
        services.AddScoped<IPortfolioRepository, PortfolioRepository>();

        services.AddGrpc();
    }

    // ...
}
```

`IPortfolioRepository` の実装は、次のように `PortfolioService` クラスのコンストラクターパラメーターとして指定できるようになりました。

```csharp
public class PortfolioService : Protos.Portfolios.PortfoliosBase
{
    private readonly IPortfolioRepository _repository;

    public PortfolioService(IPortfolioRepository repository)
    {
        _repository = repository;
    }
}
```

## <a name="implement-the-grpc-service"></a>GRPC サービスを実装する

メッセージとサービスを `portfolios.proto` ファイルで宣言したので、gRPC によって生成された `Portfolios.PortfoliosBase` クラスを継承する `PortfolioService` クラスにサービスメソッドを実装する必要があります。 メソッドは、基本クラスの `virtual` として宣言されます。 オーバーライドしないと、既定で gRPC "未実装" 状態コードが返されます。

まず、`Get` メソッドを実装します。 既定のオーバーライドは、次の例のようになります。

```csharp
public override Task<GetResponse> Get(GetRequest request, ServerCallContext context)
{
    return base.Get(request, context);
}
```

最初の問題は、`request.TraderId` が文字列であり、サービスに `Guid`が必要であることです。 文字列に対して予期される書式が `UUID`場合でも、呼び出し元が無効な値を送信し、適切に応答する可能性をコードで処理する必要があります。 サービスは、`RpcException` をスローし、標準の `InvalidArgument` ステータスコードを使用して問題を表すことにより、エラーで応答できます。

```csharp
public override Task<GetResponse> Get(GetRequest request, ServerCallContext context)
{
    if (!Guid.TryParse(request.TraderId, out var traderId))
    {
        throw new RpcException(new Status(StatusCode.InvalidArgument, "traderId must be a UUID"));
    }

    return base.Get(request, context);
}
```

`traderId`に適切な `Guid` 値が得られたら、リポジトリを使用してポートフォリオを取得し、それをクライアントに返すことができます。

```csharp
    var response = new GetResponse
    {
        Portfolio = await _repository.GetAsync(request.TraderId, request.PortfolioId)
    };
```

### <a name="map-internal-models-to-grpc-messages"></a>内部モデルを gRPC メッセージにマップする

リポジトリが独自の POCO モデル `Portfolio`を返すため、前のコードは実際には機能しませんが、gRPC は独自の Protobuf メッセージ `Portfolio`を必要とします。 Entity Framework の種類をデータ転送の種類にマップする場合は、2つの間の変換を提供することをお勧めします。 この変換のためのコードを配置するための適切な場所は、Protobuf によって生成されるクラスにあります。これは、拡張できるように `partial` クラスとして宣言されています。

```csharp
namespace TraderSys.Portfolios.Protos
{
    public partial class PortfolioItem
    {
        public static PortfolioItem FromRepositoryModel(PortfolioData.Models.PortfolioItem source)
        {
            if (source is null) return null;

            return new PortfolioItem
            {
                Id = source.Id,
                ShareId = source.ShareId,
                Holding = source.Holding,
                CostCents = (int)(source.Cost * 100)
            };
        }
    }

    public partial class Portfolio
    {
        public static Portfolio FromRepositoryModel(PortfolioData.Models.Portfolio source)
        {
            if (source is null) return null;

            var target = new Portfolio
            {
                Id = source.Id,
                TraderId = source.TraderId.ToString(),
            };

            target.Items.AddRange(source.Items.Select(PortfolioItem.FromRepositoryModel));

            return target;
        }
    }
}
```

> [!NOTE]
> [Automapper](https://automapper.org/)のようなライブラリを使用して、内部モデルクラスから Protobuf 型への変換を処理できます。ただし、`string`/`Guid` や `decimal`/`double` やリストマッピングなどの下位レベルの型変換を構成する場合に限ります。

これで、変換コードを配置したので、`Get` メソッドの実装を完了できます。

```csharp
public override async Task<GetResponse> Get(GetRequest request, ServerCallContext context)
{
    if (!Guid.TryParse(request.TraderId, out var traderId))
    {
        throw new RpcException(new Status(StatusCode.InvalidArgument, "traderId must be a UUID"));
    }

    var portfolio = await _repository.GetAsync(traderId, request.PortfolioId);

    return new GetResponse
    {
        Portfolio = Portfolio.FromRepositoryModel(portfolio)
    };
}

```

`GetAll` メソッドの実装も似ています。 Protobuf メッセージの `repeated` フィールドは `RepeatedField<T>`型の `readonly` プロパティとして生成されるため、次の例のように、`AddRange` メソッドを使用して項目を追加する必要があります。

```csharp
public override async Task<GetAllResponse> GetAll(GetAllRequest request, ServerCallContext context)
{
    if (!Guid.TryParse(request.TraderId, out var traderId))
    {
        throw new RpcException(new Status(StatusCode.InvalidArgument, "traderId must be a UUID"));
    }

    var portfolios = await _repository.GetAllAsync(traderId);

    var response = new GetAllResponse();
    response.Portfolios.AddRange(portfolios.Select(Portfolio.FromRepositoryModel));

    return response;
}
```

WCF 要求-応答サービスを gRPC に正常に移行したところで、`.proto` ファイルからクライアントを作成する方法を見てみましょう。

## <a name="generate-client-code"></a>クライアントコードの生成

クライアントを格納するために、同じソリューションに .NET Standard クラスライブラリを作成します。 これは、主にクライアントコードを作成する例ですが、NuGet を使用してこのようなライブラリをパッケージ化し、他の .NET チームが使用できるように内部リポジトリに配布することもできます。 `TraderSys.Portfolios.Client` という名前の新しい .NET Standard クラスライブラリをソリューションに追加し、`Class1.cs` ファイルを削除します。

> [!CAUTION]
> [Grpc .Net クライアント](https://www.nuget.org/packages/Grpc.Net.Client)の NuGet パッケージには、.net Core 3.0 (または .NET Standard 2.1 に準拠した別のランタイム) が必要です。 以前のバージョンの .NET Framework と .NET Core は、 [Grpc. Core](https://www.nuget.org/packages/Grpc.Core) NuGet パッケージによってサポートされています。

Visual Studio 2019 では、以前のバージョンの Visual Studio で WCF プロジェクトにサービス参照を追加するのと同様の方法で、gRPC サービスへの参照を追加できます。 サービス参照と接続済みサービスはすべて同じ UI で管理されるようになりました。 UI にアクセスするには、ソリューションエクスプローラーで `TraderSys.Portfolios.Client` プロジェクトの **[依存関係]** ノードを右クリックし、 **[接続済みサービスの追加]** を選択します。 表示されたツールウィンドウで、 **[サービス参照]** セクションを選択し、 **[新しい grpc サービス参照の追加]** を選択します。

![Visual Studio 2019 の接続済みサービス UI](media/migrate-request-reply/add-connected-service.png)

`TraderSys.Portfolios` プロジェクトの `portfolios.proto` ファイルを参照し、 **[生成するクラスの種類を選択]** の下の **[クライアント]** のままにして、[ **OK]** を選択します。

![Visual Studio 2019 の [新しい gRPC サービス参照の追加] ダイアログボックス](media/migrate-request-reply/add-new-grpc-service-reference.png)

> [!TIP]
> このダイアログボックスには、[URL] フィールドも表示されることに注意してください。 組織が `.proto` ファイルの web アクセス可能なディレクトリを保持している場合は、この URL アドレスを設定するだけでクライアントを作成できます。

Visual Studio の **[接続済みサービスの追加]** 機能を使用すると、`portfolios.proto` ファイルはコピーされるのではなく、リンクされた*ファイル*としてクラスライブラリプロジェクトに追加されるため、サービスプロジェクトのファイルに対する変更は、クライアントプロジェクトで自動的に適用されます。 `csproj` ファイルの `<Protobuf>` 要素は次のようになります。

```xml
<Protobuf Include="..\TraderSys.Portfolios\Protos\portfolios.proto" GrpcServices="Client">
  <Link>Protos\portfolios.proto</Link>
</Protobuf>
```

> [!TIP]
> Visual Studio を使用していない場合、またはコマンドラインから作業する場合は、`dotnet-grpc` グローバルツールを使用して、.NET gRPC プロジェクトの Protobuf 参照を管理できます。 詳細については、 [`dotnet-grpc` のドキュメント](/aspnet/core/grpc/dotnet-grpc)を参照してください。

### <a name="use-the-portfolios-service-from-a-client-application"></a>クライアントアプリケーションからのポートフォリオサービスの使用

次のコードは、生成されたクライアントをコンソールアプリケーションで使用する方法の簡単な例です。 GRPC クライアントコードのより詳細な探索については、この章の最後にあります。

```csharp
public class Program
{
    public async Task Main(string[] args)
    {
        GetResponse response;

        using (var channel = GrpcChannel.ForAddress("https://localhost:5001"))
        {
            var client = new Protos.Portfolios.PortfoliosClient(channel);

            response = await client.GetAsync(new GetRequest
            {
                TraderId = args[0],
                PortfolioId = int.Parse(args[1])
            });
        }

        foreach (var item in response.Portfolio.Items)
        {
            Console.WriteLine($"Holding {item.Holding} of Share ID {item.ShareId}.");
        }
    }
}
```

これで、基本的な WCF アプリケーションを ASP.NET Core gRPC サービスに移行し、.NET アプリケーションからサービスを使用するクライアントを作成しました。 次のセクションでは、より複雑な双方向サービスについて説明します。

>[!div class="step-by-step"]
>[前へ](create-project.md)
>[次へ](migrate-duplex-services.md)
