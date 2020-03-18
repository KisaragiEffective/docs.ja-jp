---
title: チャネル資格情報 - WCF 開発者向け gRPC
description: ASP.NET Core 3.0 で gRPC チャネル資格情報を実装して使用する方法。
ms.date: 09/02/2019
ms.openlocfilehash: 9ebe0aecb517e4cc2fe280632c4ecb593da9871c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79148206"
---
# <a name="channel-credentials"></a>チャンネルの資格情報

名前が示すように、チャネル資格情報は基になる gRPC チャネルにアタッチされます。 チャネル資格情報の標準形式では、クライアント証明書認証が使用されます。 このプロセスでは、クライアントは接続を行うときに TLS 証明書を提供し、サーバーは呼び出しを許可する前にこれを検証します。

チャネル資格情報と呼び出し資格情報を組み合わせて、gRPC サービスの包括的なセキュリティを提供できます。 チャネル資格情報は、クライアント アプリケーションがサービスにアクセスすることを許可されていることを証明し、呼び出しの資格情報は、クライアント アプリケーションを使用しているユーザーに関する情報を提供します。

クライアント証明書認証は、gRPC で ASP.NETコアで動作するのと同じように機能します。 詳細については、「 [ASP.NET Core で証明書認証を構成する](/aspnet/core/security/authentication/certauth)」を参照してください。

開発目的では自己署名証明書を使用できますが、実稼働環境では、信頼された機関によって署名された適切な HTTPS 証明書を使用する必要があります。

## <a name="add-certificate-authentication-to-the-server"></a>サーバーに証明書認証を追加する

証明書認証は、ホスト レベル (Kestrel サーバーなど) と ASP.NET コア パイプラインの両方で構成します。

### <a name="configure-certificate-validation-on-kestrel"></a>ケストレルで証明書検証を構成する

Kestrel (ASP.NETコア HTTP サーバー) を構成して、クライアント証明書を要求し、必要に応じて、着信接続を受け入れる前に、指定された証明書の検証を実行するように設定できます。 これは、 ではなく`CreateWebHostBuilder`クラスの`Program`メソッドで`Startup`行います。

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            var serverCert = ObtainServerCertificate();
            webBuilder.UseStartup<Startup>()
                .ConfigureKestrel(kestrelServerOptions => {
                    kestrelServerOptions.ConfigureHttpsDefaults(opt =>
                    {
                        opt.ClientCertificateMode = ClientCertificateMode.RequireCertificate;

                        // Verify that client certificate was issued by same CA as server certificate
                        opt.ClientCertificateValidation = (certificate, chain, errors) =>
                            certificate.Issuer == serverCert.Issuer;
                    });
                });
        });

```

この`ClientCertificateMode.RequireCertificate`設定により、Kestrel はクライアント証明書を提供しない接続要求を直ちに拒否しますが、この設定だけでは、提供された証明書は検証されません。 コールバックを`ClientCertificateValidation`追加して、kestrel が接続が確立された時点で、ASP.NET Core パイプラインが関与する前にクライアント証明書を検証できるようにします。 (この場合、コールバックは、サーバー証明書と同じ*認証局*によって発行されたことを確認します)。

### <a name="add-aspnet-core-certificate-authentication"></a>ASP.NETコア証明書認証を追加する

[証明書](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Certificate)の認証を提供する証明書の認証を提供します。

メソッドに証明書認証サービスを`ConfigureServices`追加し、メソッドの ASP.NET Core パイプラインに認証と承認`Configure`を追加します。

```csharp
public class Startup
{
    private readonly bool _isDevelopment;

    public Startup(IWebHostEnvironment env)
    {
        _isDevelopment = env.IsDevelopment();
    }

    public void ConfigureServices(IServiceCollection services)
    {
        services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate(options =>
            {
                if (_isDevelopment)
                {
                    // DO NOT DO THIS IN PRODUCTION!
                    options.RevocationMode = X509RevocationMode.NoCheck;
                }
            });
        services.AddAuthorization();
        services.AddGrpc();
    }

    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        app.UseRouting();

        app.UseAuthentication();
        app.UseAuthorization();

        app.UseEndpoints(endpoints => { endpoints.MapGrpcService<GreeterService>(); });
    }
}
```

## <a name="provide-channel-credentials-in-the-client-application"></a>クライアント アプリケーションでチャネル資格情報を提供する

パッケージでは`Grpc.Net.Client`、接続に`GrpcChannel`使用されるに提供される<xref:System.Net.Http.HttpClient>インスタンスに証明書を構成します。

```csharp
class Program
{
    static async Task Main(string[] args)
    {
        // Assume path to a client .pfx file and password are passed from command line
        // On Windows this would probably be a reference to the Certificate Store
        var cert = new X509Certificate2(args[0], args[1]);

        var handler = new HttpClientHandler();
        handler.ClientCertificates.Add(cert);
        var httpClient = new HttpClient(handler);

        var channel = GrpcChannel.ForAddress("https://localhost:5001/", new GrpcChannelOptions
        {
            HttpClient = httpClient
        });

        var grpc = new Greeter.GreeterClient(channel);
        var response = await grpc.SayHelloAsync(new HelloRequest { Name = "Bob" });
        System.Console.WriteLine(response.Message);
    }
}
```

## <a name="combine-channelcredentials-and-callcredentials"></a>チャネル資格情報と呼び出し資格情報の組み合わせ

証明書とトークン認証の両方を使用するようにサーバーを構成できます。 これを行うには、証明書の変更を Kestrel サーバーに適用し、ASP.NET Core の JWT ベアラー ミドルウェアを使用します。

クライアント`ChannelCredentials``CallCredentials`の両方を提供するには、メソッドを`ChannelCredentials.Create`使用して呼び出しの資格情報を適用します。 インスタンスを使用して証明書認証を適用する<xref:System.Net.Http.HttpClient>必要があります。 `SslCredentials`コンストラクターに引数を渡すと、内部クライアント コードは例外をスローします。 パラメーター`SslCredentials`は、パッケージと`Grpc.Net.Client``Create``Grpc.Core`の互換性を維持するためにパッケージのメソッドにのみ含まれます。

```csharp
var handler = new HttpClientHandler();
handler.ClientCertificates.Add(cert);

var httpClient = new HttpClient(handler);

var callCredentials = CallCredentials.FromInterceptor(((context, metadata) =>
    {
        metadata.Add("Authorization", $"Bearer {_token}");
    }));

var channelCredentials = ChannelCredentials.Create(new SslCredentials(), callCredentials);

var channel = GrpcChannel.ForAddress("https://localhost:5001/", new GrpcChannelOptions
{
    HttpClient = httpClient,
    Credentials = channelCredentials
});

var grpc = new Portfolios.PortfoliosClient(channel);
```

> [!TIP]
> 証明書認証なしで`ChannelCredentials.Create`クライアントに対してこのメソッドを使用できます。 これは、チャネルで行われるすべての呼び出しでトークンの資格情報を渡す便利な方法です。

[証明書認証が追加された FullStockTicker サンプル gRPC アプリケーション](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/FullStockTickerSample/grpc/FullStockTickerAuth/FullStockTicker)のバージョンは GitHub にあります。

>[!div class="step-by-step"]
>[前次](call-credentials.md)
>[Next](encryption.md)
