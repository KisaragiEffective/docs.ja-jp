---
title: Ocelot を使った API ゲートウェイの実装
description: Ocelot を使用して API ゲートウェイを実装する方法と、コンテナー ベースの環境で Ocelot を使用する方法について説明します。
ms.date: 01/30/2020
ms.openlocfilehash: 0eb834829a418cfa1ccdf13c5fc8849f6855c4ba
ms.sourcegitcommit: f38e527623883b92010cf4760246203073e12898
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77502414"
---
# <a name="implement-api-gateways-with-ocelot"></a>Ocelot を使った API ゲートウェイの実装

> [!IMPORTANT]
> 現在、参照マイクロサービス アプリケーション [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) では、[Envoy](https://www.envoyproxy.io/) が提供する機能を使用して、前に参照された [Ocelot](https://github.com/ThreeMammals/Ocelot) ではなく、API ゲートウェイを実装しています。
> この設計が選択されたのは、eShopOnContainers に実装される新しい gRPC サービス間通信で必要な WebSocket プロトコルのサポートが Envoy に組み込まれているためです。
> ただし、ガイドにこのセクションが含まれているのは、Ocelot を実稼働レベルのシナリオに適したシンプルで有能な軽量の API ゲートウェイとして 検討できるようにするためです。

## <a name="architect-and-design-your-api-gateways"></a>API ゲートウェイのアーキテクチャとデザイン

次のアーキテクチャ図は、eShopOnContainers で Ocelot を使用して API ゲートウェイがどのように実装されたかを示しています。

![eShopOnContainers アーキテクチャを示す図。](./media/implement-api-gateways-with-ocelot/eshoponcontainers-architecture.png)

**図 6-28**。 API ゲートウェイを使用した eShopOnContainers アーキテクチャ

この図では、アプリケーション全体を 1 つの Docker ホスト、または "Docker for Windows" または "Docker for Mac" を搭載した開発 PC に配置する方法を示しています。 ただし、どのオーケストレーターでもそれに配置する方法は似ていますが、図のコンテナーはいずれも、オーケストレーターでスケールアウトする可能性があります。

さらに、データベース、キャッシュ、メッセージ ブローカーなどのインフラストラクチャ資産は、オーケストレーターからオフロードして、Azure SQL Database、Azure Cosmos DB、Azure Redis、Azure Service Bus、またはオンプレミスの任意の HA クラスタリング ソリューションのような、インフラストラクチャの可用性が高いシステムに配置する必要があります。

また、図からわかるように、複数の API ゲートウェイを持つことで、複数の開発チームがマイクロサービスと独自の関連する API ゲートウェイを開発して配置するときに、自律的に行うことができるようになります (この例では、マーケティング機能とショッピング機能)。

1 つのポイントが複数の開発チームによって更新されることを意味するモノリシック API ゲートウェイがあれば、アプリケーションの 1 つの部分にすべてのマイクロサービスを組み合わせることができたかもしれません。

デザインにさらに踏み込むと、きめ細かい API ゲートウェイを、選択したアーキテクチャに応じて、1 つのビジネス マイクロサービスに制限できる場合があります。 ビジネスまたはドメインで API ゲートウェイの境界を示すとデザインが良くなります。

たとえば、API ゲートウェイ層を細分化することは、マイクロサービスに基づくより高度な複合 UI アプリケーションで特に役立つ場合があります。これは、きめ細かい API ゲートウェイの概念が UI コンポジション サービスと似ているためです。

詳細は前のセクション「[マイクロサービスを基にしている複合 UI を作成する](../architect-microservice-container-applications/microservice-based-composite-ui-shape-layout.md)」で取り上げています。

重要な点は、多くの中規模および大規模なアプリケーションの場合、カスタム ビルドの API ゲートウェイ製品の使用は、通常は有効なアプローチですが、API ゲートウェイで自立型マイクロサービスを作成している複数の開発チームに対して、複数の独立した構成領域を許可している場合を除き、単一のモノシリック アグリゲーターや一意の中央カスタム API ゲートウェイとしては適切なアプローチではないことです。

### <a name="sample-microservicescontainers-to-reroute-through-the-api-gateways"></a>API ゲートウェイ経由で経路変更するマイクロサービス/コンテナーのサンプル

例として、eShopOnContainers には、次の図に示すように、API ゲートウェイ経由で公開する必要がある約 6 種類の内部のマイクロサービスがあります。

![Services フォルダーのスクリーンショット。そのサブフォルダーを確認できます。](./media/implement-api-gateways-with-ocelot/eshoponcontainers-microservice-folders.png)

**図 6-29**。 Visual Studio の eShopOnContainers ソリューション内のマイクロサービス フォルダー

ID サービスについては、システム内の唯一の横断的関心事のため、デザインでは API ゲートウェイのルーティングから除外されていますが、Ocelot を使用すると、経路変更リストの一部として含めることができます。

コードからわかるように、これらのサービスはすべて、ASP.NET Core Web API サービスとして現在実装されています。 マイクロサービスの 1 つに注目してみましょう。Catalog マイクロサービス コードなどがあります。

![Catalog.API プロジェクトのコンテンツを示すソリューション エクスプローラーのスクリーンショット。](./media/implement-api-gateways-with-ocelot/catalog-api-microservice-folders.png)

**図 6-30**。 サンプル Web API マイクロサービス (Catalog マイクロサービス)

Catalog マイクロサービスは、複数のコントローラーと次のコードにあるようなメソッドを持つ一般的な ASP.NET Core Web API プロジェクトであることがわかります。

```csharp
[HttpGet]
[Route("items/{id:int}")]
[ProducesResponseType((int)HttpStatusCode.BadRequest)]
[ProducesResponseType((int)HttpStatusCode.NotFound)]
[ProducesResponseType(typeof(CatalogItem),(int)HttpStatusCode.OK)]
public async Task<IActionResult> GetItemById(int id)
{
    if (id <= 0)
    {
        return BadRequest();
    }
    var item = await _catalogContext.CatalogItems.
                                          SingleOrDefaultAsync(ci => ci.Id == id);
    //…

    if (item != null)
    {
        return Ok(item);
    }
    return NotFound();
}
```

HTTP 要求は、マイクロサービス データベースと追加の必須アクションにアクセスするような C# コードの実行で終了します。

マイクロサービス URL については、コンテナーがローカル開発用 PC (ローカル Docker ホスト) に配置されると、各マイクロサービスのコンテナーには、次の dockerfile にあるように、その dockerfile で指定された内部ポート (通常はポート 80) が常に与えられます。

```Dockerfile
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
```

コードに示されているポート 80 は、Docker ホスト内の内部ポートであるため、クライアント アプリが到達することはできません。

クライアント アプリがアクセスできるのは、`docker-compose` を使用して配置したときに公開された外部ポートだけです (このようなポートがある場合に)。

運用環境に配置するときに、このような外部ポートを公開しないでください。 クライアント アプリとマイクロサービス間の直接通信を回避するために、API ゲートウェイを使用する理由がここにあります。

ただし、開発時には、マイクロサービス/コンテナーに直接アクセスして、Swagger を経由して実行できる必要があります。 そのため、eShopOnContainers では、API ゲートウェイまたはクライアント アプリで使われなくなっても、外部ポートは指定されたままになります。

Catalog マイクロサービスの `docker-compose.override.yml` ファイルの例を次に示します。

```yml
catalog-api:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - ASPNETCORE_URLS=http://0.0.0.0:80
    - ConnectionString=YOUR_VALUE
    - ... Other Environment Variables
  ports:
    - "5101:80"   # Important: In a production environment you should remove the external port (5101) kept here for microservice debugging purposes.
                  # The API Gateway redirects and access through the internal port (80).
```

docker-compose.override.yml の構成では、Catalog コンテナーの内部ポートがポート 80 になっていますが、外部アクセスのポートは 5101 になっていることがわかります。 ただし、このポートは、API ゲートウェイを使用する場合には、アプリケーションでは使用せずに、Catalog マイクロサービスのデバッグ、実行、テストのためにだけ使用します。

マイクロサービスの適切な運用配置環境は、Kubernetes や Service Fabric などのオーケストレーターであるため、通常は docker-compose を使用して運用環境に配置することはありません。 これらの環境に配置する場合は、マイクロサービスに対してどの外部ポートも直接公開しない別の構成ファイルを使用しますが、API ゲートウェイからのリバース プロキシは常に使用します。

ローカルの Docker ホストで Catalog マイクロサービスを実行します。 Visual Studio から完全な eShopOnContainers ソリューションを実行する (docker-compose ファイル内のすべてのサービスを実行する) か、`docker-compose.yml` と `docker-compose.override.yml` が配置されているフォルダーにある CMD または PowerShell で次の docker-compose コマンドを使用して Catalog マイクロサービスを起動します。

```console
docker-compose run --service-ports catalog-api
```

このコマンドでは、catalog.api サービス コンテナーと docker-compose.yml に指定されている依存関係のみが実行されます。 この例では、SQL Server コンテナーと RabbitMQ コンテナーです。

こうすると、"外部" ポート (この例では `http://localhost:5101/swagger`) を経由して直接アクセスする Swagger UI を使用して、Catalog マイクロサービスに直接アクセスして、そのメソッドを表示できるようになります。

![Catalog.API REST API を表示している Swagger UI のスクリーンショット。](./media/implement-api-gateways-with-ocelot/test-catalog-microservice.png)

**図 6-31**。 Swagger UI を使用した Catalog マイクロサービスのテスト

この時点で、Visual Studio で C# コードにブレークポイントを設定し、Swagger UI で公開されるメソッドを使用してマイクロサービスをテストし、最後に `docker-compose down` コマンドを使用してすべてをクリーンアップすることは可能です。

しかしながら、マイクロサービスへの直接アクセス通信 (この例では外部ポート 5101 を経由) こそが、アプリケーションで回避したいことなのです。 また、API ゲートウェイ (この例では Ocelot ) の間接参照の追加レベルを設定することで、これを回避できます。 これにより、クライアント アプリがマイクロサービスに直接アクセスすることはなくなります。

## <a name="implementing-your-api-gateways-with-ocelot"></a>Ocelot を使った API ゲートウェイの実装

Ocelot は、基本的に特定の順序で適用できるミドルウェアのセットです。

Ocelot は、ASP.NET Core でのみ動作するように設計されています。 これは `netstandard2.0` をターゲットにしているため、.NET Standard 2.0 がサポートされていれば、どこでも使用できます。これには、.NET Core 2.0 ランタイムと .NET Framework 4.6.1 ランタイム以降が含まれます。

[Ocelot の NuGet パッケージ](https://www.nuget.org/packages/Ocelot/)を使用して、Visual Studio から ASP.NET Core プロジェクトに Ocelot とその依存関係をインストールします。

```powershell
Install-Package Ocelot
```

eShopOnContainers では、その API ゲートウェイの実装はシンプルな ASP.NET Core WebHost プロジェクトであり、次の図に示すように、Ocelot のミドルウェアによってすべての API ゲートウェイ機能が処理されます。

![Ocelot API ゲートウェイ プロジェクトを表示しているソリューション エクスプローラーのスクリーンショット。](./media/implement-api-gateways-with-ocelot/ocelotapigw-base-project.png)

**図 6-32**。 eShopOnContainers の OcelotApiGw 基本プロジェクト

この ASP.NET Core WebHost プロジェクトは基本的に、`Program.cs` と `Startup.cs` の 2 つのシンプルなファイルで構築されます。

Program.cs では、一般的な ASP.NET Core BuildWebHost を作成して構成する必要があります。

```csharp
namespace OcelotApiGw
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args)
        {
            var builder = WebHost.CreateDefaultBuilder(args);

            builder.ConfigureServices(s => s.AddSingleton(builder))
                    .ConfigureAppConfiguration(
                          ic => ic.AddJsonFile(Path.Combine("configuration",
                                                            "configuration.json")))
                    .UseStartup<Startup>();
            var host = builder.Build();
            return host;
        }
    }
}
```

ここで Ocelot について重要な点は、`AddJsonFile()` メソッドを通じてビルダーに提供する必要がある `configuration.json` ファイルです。 その `configuration.json` で、すべての API ゲートウェイの ReRoute を指定します。つまり、特定のポートを持つ外部エンドポイントと、通常は別のポートを使用する相関内部エンドポイントを指定します。

```json
{
    "ReRoutes": [],
    "GlobalConfiguration": {}
}
```

構成には 2 つのセクションがあります。 ReRoute の配列と GlobalConfiguration です。 ReRoute は、Ocelot にアップストリーム要求の処理方法を指示するオブジェクトです。 グローバル構成を使用すると、ReRoute 固有の設定をオーバーライドできます。 ReRoute 固有の設定をたくさん管理しない場合に便利です。

eShopOnContainers のうちの 1 つの API ゲートウェイから、[ReRoute 構成ファイル](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/ApiGateways/Web.Bff.Shopping/apigw/configuration.json)の単純な例を次に示します。

```json
{
  "ReRoutes": [
    {
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "catalog-api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/c/{everything}",
      "UpstreamHttpMethod": [ "POST", "PUT", "GET" ]
    },
    {
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "basket-api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/b/{everything}",
      "UpstreamHttpMethod": [ "POST", "PUT", "GET" ],
      "AuthenticationOptions": {
        "AuthenticationProviderKey": "IdentityApiKey",
        "AllowedScopes": []
      }
    }

  ],
    "GlobalConfiguration": {
      "RequestIdKey": "OcRequestId",
      "AdministrationPath": "/administration"
    }
  }
```

Ocelot API ゲートウェイの主な機能は、HTTP 要求を受け取り、それらを (現在は別の HTTP 要求として) ダウンストリーム サービスに転送することです。 Ocelot では、ある要求を別の場所にルーティングすることを ReRoute と説明しています。

例として、Basket マイクロサービスの構成である、上記の configuration.json の ReRoute の 1 つに注目してみましょう。

```json
{
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "basket-api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/b/{everything}",
      "UpstreamHttpMethod": [ "POST", "PUT", "GET" ],
      "AuthenticationOptions": {
        "AuthenticationProviderKey": "IdentityApiKey",
        "AllowedScopes": []
      }
}
```

DownstreamPathTemplate、Scheme、および DownstreamHostAndPorts が、この要求の転送先となる内部マイクロサービスの URL を構成しています。

ポートは、サービスで使用される内部ポートです。 コンテナーを使用している場合、ポートはその dockerfile で指定されます。

`Host` は、使用するサービスの名前解決に依存するサービス名です。 docker-compose を使用している場合、サービス名は、docker-compose ファイルで提供されるサービス名を使用する、Docker ホストにより提供されます。 Kubernetes や Service Fabric などのオーケストレーターを使用している場合、その名前は、DNS または各オーケストレーターで提供される名前解決で解決される必要があります。

DownstreamHostAndPorts は、要求の転送先となるすべてのダウンストリーム サービスのホストとポートを含む配列です。 通常、これには 1 つのエントリだけが含まれますが、要求をダウンストリーム サービスに負荷分散したい場合には、Ocelot を使用することで、複数のエントリを追加してロード バランサーを選択することができます。 ただし、Azure と任意のオーケストレーターを使用している場合は、クラウドとオーケストレーター インフラストラクチャを使用して負荷分散した方が良い場合があります。

UpstreamPathTemplate は、クライアントから特定の要求に対し、どの DownstreamPathTemplate を使用するかを識別するために Ocelot によって使用される URL です。 最後に、Ocelot が異なる要求 (GET、POST、PUT) を同じ URL に区別できるように、UpstreamHttpMethod が使用されます。

この時点で、1 つまたは[複数のマージされた configuration.json ファイル](https://ocelot.readthedocs.io/en/latest/features/configuration.html#merging-configuration-files)を使用して 1 つの Ocelot API ゲートウェイ (ASP.NET Core WebHost) を持つことも、[Consul KV ストアに構成を格納](https://ocelot.readthedocs.io/en/latest/features/configuration.html#store-configuration-in-consul)することもできます。

ただし、アーキテクチャとデザインのセクションで紹介したように、自律型のマイクロサービスが本当に必要な場合は、その 1 つのモノリシック API ゲートウェイを複数の API ゲートウェイや Backend for Frontend (BFF) に分割した方がよい場合があります。 そのために、Docker コンテナーを使用してこのアプローチを実装する方法を見てみましょう。

### <a name="using-a-single-docker-container-image-to-run-multiple-different-api-gateway--bff-container-types"></a>1 つの Docker コンテナー イメージを使用して、複数の異なる API ゲートウェイ/BFF コンテナーの種類を実行する

eShopOnContainers では、Ocelot API ゲートウェイと共に 1 つの Docker コンテナー イメージを使用していますが、その後、実行時、サービスごとに異なる PC フォルダーにアクセスする Docker ボリュームを使用し、configuration.json ファイルを提供することで、API ゲートウェイ/BFF コンテナーの種類ごとに異なるサービス/コンテナーを作成します。

![すべての API ゲートウェイに対する単一 Ocelot ゲートウェイの Docker イメージの図。](./media/implement-api-gateways-with-ocelot/reusing-single-ocelot-docker-image.png)

**図 6-33**。 複数の API ゲートウェイの種類で 1 つの Ocelot Docker イメージを再利用する

eShopOnContainers では、"OcelotApiGw" という名前のプロジェクトと、docker-compose.yml ファイルで指定されたイメージ名 "eshop/ocelotapigw" を使用して、"汎用 Ocelot API ゲートウェイ Docker イメージ" が作成されます。 次に、Docker に配置すると、次の docker-compose.yml ファイルからの抜粋に示されているように、同じ Docker イメージから作成された 4 つの API ゲートウェイ コンテナーができます。

```yml
  mobileshoppingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile

  mobilemarketingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile

  webshoppingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile

  webmarketingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile
```

さらに、次の docker-compose.override.yml ファイルでわかるように、API ゲートウェイのこれらのコンテナーの唯一の違いは Ocelot 構成ファイルです。これはサービス コンテナーごとに異なり、Docker ボリュームにより実行時に指定されます。

```yml
mobileshoppingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity-api
  ports:
    - "5200:80"
  volumes:
    - ./src/ApiGateways/Mobile.Bff.Shopping/apigw:/app/configuration

mobilemarketingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity-api
  ports:
    - "5201:80"
  volumes:
    - ./src/ApiGateways/Mobile.Bff.Marketing/apigw:/app/configuration

webshoppingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity-api
  ports:
    - "5202:80"
  volumes:
    - ./src/ApiGateways/Web.Bff.Shopping/apigw:/app/configuration

webmarketingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity-api
  ports:
    - "5203:80"
  volumes:
    - ./src/ApiGateways/Web.Bff.Marketing/apigw:/app/configuration
```

次の Visual Studio エクスプローラーに示されているように、前のコードにより、特定のビジネス/BFF API ゲートウェイをそれぞれ定義するために必要な唯一のファイルは、configuration.json ファイルだけです。これは、4 つの API ゲートウェイが同じ Docker イメージに基づいているためです。

![configuration.json ファイルを含むすべての API ゲートウェイを示すスクリーンショット。](./media/implement-api-gateways-with-ocelot/ocelot-configuration-files.png)

**図 6-34**。 Ocelot で各 API ゲートウェイ/BFF を定義するために必要な唯一のファイル

API ゲートウェイを複数の API ゲートウェイに分割することで、マイクロサービスの異なるサブセットに重点を置いているさまざまな開発チームが、独立した Ocelot 構成ファイルを使用して、独自の API ゲートウェイを管理できます。 くわえて、同時に同じ Ocelot Docker イメージを再利用することができます。

ここで、(eShopOnContainers ServicesAndWebApps.sln ソリューションを開くとき、または "docker-compose up" を実行している場合に、VS に既定で含まれる) API ゲートウェイを使用して eShopOnContainers を実行すると、次のサンプル ルートが実行されます。

たとえば、webshoppingapigw API ゲートウェイによって提供されるアップストリーム URL `http://localhost:5202/api/v1/c/catalog/items/2/` にアクセスすると、次のブラウザーにあるように、Docker ホスト内の内部ダウンストリーム URL `http://catalog-api/api/v1/2` からの同じ結果が得られます。

![API ゲートウェイ経由の応答を表示しているブラウザーのスクリーンショット。](./media/implement-api-gateways-with-ocelot/access-microservice-through-url.png)

**図 6-35**。 API ゲートウェイによって提供される URL からマイクロサービスにアクセスする

テストまたはデバッグの理由から、API ゲートウェイを通過せずに Catalog Docker コンテナーに直接アクセスしたい場合 (開発環境でのみ)、'catalog.api' は Docker ホスト (docker-compose サービス名により処理されるサービス検索) 内部の DNS 解決であるため、コンテナーに直接アクセスする唯一の方法は、次のブラウザーにある `http://localhost:5101/api/v1/Catalog/items/1` のように、開発テスト用にのみ提供される docker-compose.override.yml で公開されている外部ポートを経由することです。

![Catalog.api への直接応答を表示しているブラウザーのスクリーンショット。](./media/implement-api-gateways-with-ocelot/direct-access-microservice-testing.png)

**図 6-36**。 テスト目的でのマイクロサービスへの直接アクセス

ただし、アプリケーションは、ダイレクト ポートの "ショートカット" を経由してではなく、API ゲートウェイを経由してすべてのマイクロサービスにアクセスするように構成されます。

### <a name="the-gateway-aggregation-pattern-in-eshoponcontainers"></a>eShopOnContainers でのゲートウェイ集計パターン

前述のとおり、要求の集計を実装する柔軟な方法は、コードでカスタム サービスを使用することです。 [Ocelot で要求の集計機能](https://ocelot.readthedocs.io/en/latest/features/requestaggregation.html#request-aggregation)を使用して、要求の集計を実装することもできますが、十分な柔軟性が得られない場合があります。 そのため、eShopOnContainers で集計を実装するために選択した方法は、各アグリゲーターに明示的な ASP.NET Core Web API サービスを使用することです。

このアプローチに従うと、前出の簡略化されたグローバル アーキテクチャの図に示されていないアグリゲーター サービスを考慮に入れた場合、API ゲートウェイ コンポジション図は、実際にはもう少し拡張されます。

次の図では、アグリゲーター サービスがその関連する API ゲートウェイとどのように連携するかも確認できます。

![アグリゲーター サービスを示している eShopOnContainers アーキテクチャの図。](./media/implement-api-gateways-with-ocelot/eshoponcontainers-architecture-aggregator-services.png)

**図 6-37**。 アグリゲーター サービスを使用した eShopOnContainers アーキテクチャ

さらに拡大してみると、次の画像の "Shopping" 事業領域では、API ゲートウェイでアグリゲーター サービスを使用するとき、クライアント アプリとマイクロサービスの間の通信が減ることを確認できます。

![eShopOnContainers アーキテクチャの拡大を示す図。](./media/implement-api-gateways-with-ocelot/zoom-in-vision-aggregator-services.png)

**図 6-38**。 アグリゲーター サービスのビジョンの拡大表示

API ゲートウェイから送信される可能性がある要求を図に示すと、複雑になる可能性があることがわかります。 しかし、クライアント アプリの視点かラ見て、青の矢印がどのように簡略化されるかがわかります。アグリゲーター パターンを使用すると、頻繁な通信と通信の待機時間を減らすことで、最終的にユーザー エクスペリエンス、とりわけリモート アプリ (モバイルおよび SPA アプリ) のユーザー エクスペリエンスが大幅に向上します。

"マーケティング" ビジネス領域とマイクロサービスの場合は、シンプルなユース ケースであるため、アグリゲーターを使用する必要はありませんでしたが、必要に応じて使用することもできます。

### <a name="authentication-and-authorization-in-ocelot-api-gateways"></a>Ocelot API ゲートウェイでの認証と承認

Ocelot API ゲートウェイでは、認証トークンを提供する [IdentityServer](https://identityserver.io/) を使用して、ASP.NET Core Web API サービスなどの認証サービスを API ゲートウェイの内側または外側に配置することができます。

eShopOnContainers では、BFF に基づく境界とビジネス領域を持つ複数の API ゲートウェイを使用しているため、ID/認証サービス (次の図に黄色で強調表示) は、API ゲートウェイから除外されています。

![API ゲートウェイの下にある ID マイクロサービスを示す図。](./media/implement-api-gateways-with-ocelot/eshoponcontainers-identity-service-position.png)

**図 6-39**。 eShopOnContainers 内の ID サービスの位置

ただし、Ocelot では、この別の図に示されているように、ID/認証マイクロサービスを API ゲートウェイの境界内に配置することもサポートしています。

![Ocelot API ゲートウェイの認証を示す図。](./media/implement-api-gateways-with-ocelot/ocelot-authentication.png)

**図 6-40**。 Ocelot での認証

前の図に示したように、ID マイクロサービスが API ゲートウェイ (AG) の下にある場合は、次のようになります。1) AG で ID マイクロサービスに認証トークンを要求し、2) ID マイクロサービスによって AG にトークンが返され、3-4) AG で認証トークンを使用してマイクロサービスに要求します。 eShopOnContainers アプリケーションによって API ゲートウェイが複数の BFF (Backend for Frontend) とビジネス領域の API ゲートウェイに分割されているため、もう 1 つのオプションは、横断的な問題に対して追加の API ゲートウェイを作成することでした。 その選択は、複数の横断的関心事のマイクロサービスを持つより複雑なマイクロサービス ベースのアーキテクチャでは適切でしょう。 eShopOnContainers には横断的関心事は 1 つしかないので、簡素化するため、API ゲートウェイ領域外のセキュリティ サービスだけを処理すると判断されました。

いずれの場合も、アプリが API ゲートウェイ レベルでセキュリティ保護されている場合は、任意のセキュリティで保護されたマイクロサービスを使用するときに、最初にアクセスされるのは、Ocelot API ゲートウェイの認証モジュールです。 これは HTTP 要求をリダイレクトして、ID/認証マイクロサービスにアクセスして、access_token で保護されたサービスにアクセスできるように、アクセス トークンを取得します。

認証を使用して API ゲートウェイ レベルで任意のサービスをセキュリティで保護する方法は、configuration.json で、その関連する設定に AuthenticationProviderKey を設定することで行います。

```json
    {
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "basket-api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/b/{everything}",
      "UpstreamHttpMethod": [],
      "AuthenticationOptions": {
        "AuthenticationProviderKey": "IdentityApiKey",
        "AllowedScopes": []
      }
    }
```

Ocelot により実行時に Re-Routes AuthenticationOptions.AuthenticationProviderKey が調べられ、指定したキーに登録されている認証プロバイダーがあることが確認されます。 ない場合は、Ocelot が起動しません。 ある場合、ReRoute の実行時にそのプロバイダーが使用されます。

Ocelot WebHost は `authenticationProviderKey = "IdentityApiKey"` で構成されているため、そのサービスに認証トークンがない任意の要求がある場合には常に、認証が必要になります。

```csharp
namespace OcelotApiGw
{
    public class Startup
    {
        private readonly IConfiguration _cfg;

        public Startup(IConfiguration configuration) => _cfg = configuration;

        public void ConfigureServices(IServiceCollection services)
        {
            var identityUrl = _cfg.GetValue<string>("IdentityUrl");
            var authenticationProviderKey = "IdentityApiKey";
                         //…
            services.AddAuthentication()
                .AddJwtBearer(authenticationProviderKey, x =>
                {
                    x.Authority = identityUrl;
                    x.RequireHttpsMetadata = false;
                    x.TokenValidationParameters = new Microsoft.IdentityModel.Tokens.TokenValidationParameters()
                    {
                        ValidAudiences = new[] { "orders", "basket", "locations", "marketing", "mobileshoppingagg", "webshoppingagg" }
                    };
                });
            //...
        }
    }
}
```

次に、次の Basket マイクロサービス コントローラーのように、マイクロサービスのようにアクセスされる任意のリソースで、[Authorize] 属性を使用して承認を設定する必要があります。

```csharp
namespace Microsoft.eShopOnContainers.Services.Basket.API.Controllers
{
    [Route("api/v1/[controller]")]
    [Authorize]
    public class BasketController : Controller
    {
      //...
    }
}
```

"basket" などの ValidAudiences は、次のコードのように、Startup クラスの ConfigureServices() で、`AddJwtBearer()` を使用して各マイクロサービスで定義されている対象ユーザーと関連付けられます。

```csharp
// prevent from mapping "sub" claim to nameidentifier.
JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Clear();

var identityUrl = Configuration.GetValue<string>("IdentityUrl");

services.AddAuthentication(options =>
{
    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;

}).AddJwtBearer(options =>
{
    options.Authority = identityUrl;
    options.RequireHttpsMetadata = false;
    options.Audience = "basket";
});
```

`http://localhost:5202/api/v1/b/basket/1` のような API ゲートウェイに基づく ReRoute URL を使用して、セキュリティで保護された任意のマイクロサービスにアクセスしようしたときに、有効なトークンを提供しないと、401 未承認エラーが表示されます。 その一方で、ReRoute URL が認証されると、(内部マイクロサービス URL に) 関連付けられているダウンストリーム スキーマが何であれ、Ocelot によって呼び出されます。

**Ocelot の ReRoute 層での承認。**  Ocelot では、認証後に評価されたクレーム ベースの承認をサポートします。 次の行を ReRoute 構成に追加することで、ルート レベルで承認を設定します。

```json
"RouteClaimsRequirement": {
    "UserType": "employee"
}
```

この例では、承認ミドルウェアが呼び出されると、ユーザーがトークンに要求の種類 'UserType' を持っているかどうか、および要求の値が 'employee' かどうかが Ocelot によって確認されます。 そうでない場合、ユーザーは承認されず、403 アクセス不可の応答が返されます。

## <a name="using-kubernetes-ingress-plus-ocelot-api-gateways"></a>Kubernetes イングレスと Ocelot API ゲートウェイを使用する

(Azure Kubernetes Service クラスターで使用されているように) Kubernetes を使用する場合、通常は、*Nginx* に基づく [Kubernetes イングレス層](https://kubernetes.io/docs/concepts/services-networking/ingress/)を通じて、すべての HTTP 要求を統合します。

Kubernetes では、どのイングレス アプローチも使用しない場合、サービスとポッドには、クラスター ネットワークによるルーティングのみが可能な IP が与えられます。

ただし、イングレス アプローチを使用する場合は、リバース プロキシとして機能する、インターネットとサービス (API ゲートウェイを含む) の間の中間層が与えられます。

定義としては、イングレスはクラスター サービスに到達する受信接続を許可するルールのコレクションです。 イングレスは通常、サービスに外部から到達可能な URL の提供、トラフィックの負荷分散、SSL 終了などを提供するために構成されます。 ユーザーは、イングレス リソースを API サーバーに POST することで、イングレスを要求します。

eShopOnContainers では、ローカルで開発し、開発用コンピューターだけを Docker ホストとして使用する場合は、どのイングレスも使用せず、複数の API ゲートウェイのみを使用します。

ただし、Kubernetes に基づいて "運用" 環境をターゲットにする場合は、eShopOnContainers では、API ゲートウェイの前にイングレスが使用されます。 これにより、クライアントは引き続き同じベース URL を呼び出しますが、要求は複数の API ゲートウェイまたは BFF にルーティングされます。

API ゲートウェイはサービスのみに接するフロント エンドまたはファサードであり、通常はその範囲外となる Web アプリケーションではありません。 さらに、API ゲートウェイは、特定の内部マイクロサービスを非表示にすることがあります。

ただし、イングレスは、HTTP 要求をリダイレクトするだけですが、どのマイクロサービスまたは Web アプリも非表示にはしません。

次の図に示すように、Web アプリケーションの前の Kubernetes にイングレス Nginx 層と、複数の Ocelot API ゲートウェイ/BFF を配置するのが理想的なアーキテクチャです。

![イングレス層が AKS 環境にどのように適合しているかを示す図。](./media/implement-api-gateways-with-ocelot/eshoponcontainer-ingress-tier.png)

**図 6-41**。 Kubernetes に配置するときの eShopOnContainers のイングレス層

Kubernetes イングレスは、Web アプリケーションなど、通常は API ゲートウェイの範囲外となるアプリに宛てられるすべてのトラフィックのリバース プロキシとして機能します。 Kubernetes に eShopOnContainers を配置すると、少数のサービスまたはエンドポイントだけが_イングレス_経由で公開されます。基本的に、URL の接尾辞は次のとおりです。

- `/`: クライアント SPA Web アプリケーション用
- `/webmvc`: クライアント MVC Web アプリケーション用
- `/webstatus`: 状態/正常性チェックを示すクライアント Web アプリ用
- `/webshoppingapigw`: Web BFF とショッピング ビジネス プロセス用
- `/webmarketingapigw`: Web BFF とマーケティング ビジネス プロセス用
- `/mobileshoppingapigw`: モバイル BFF とショッピング ビジネス プロセス用
- `/mobilemarketingapigw`: モバイル BFF とマーケティング ビジネス プロセス用

Kubernetes に配置するときに、各 Ocelot API ゲートウェイは、API ゲートウェイを実行している_ポッド_ごとに異なる "configuration.json" ファイルを使用します。 これらの "configuration.json" ファイルは、"ocelot" という名前の Kubernetes _構成マップ_に基づいて作成されたボリュームを (もともとは deploy.ps1 スクリプトを使用して) マウントすることによって提供されます。 各コンテナーにより、関連する構成ファイルが `/app/configuration` という名前のコンテナーのフォルダーにマウントされます。

eShopOnContainers のソース コード ファイルでは、元の "configuration.json" ファイルは `k8s/ocelot/` フォルダー内にあります。 BFF/API ゲートウェイごとに 1 つのファイルがあります。

## <a name="additional-cross-cutting-features-in-an-ocelot-api-gateway"></a>Ocelot API ゲートウェイで追加された横断機能

Ocelot API ゲートウェイを使用する場合に、調査および使用するための重要な機能は他にもあり、次のリンクで説明されています。

- **Ocelot と Consul または Eureka を統合するクライアント側でのサービス検出** \
  <https://ocelot.readthedocs.io/en/latest/features/servicediscovery.html>

- **API ゲートウェイ層でのキャッシュ** \
  <https://ocelot.readthedocs.io/en/latest/features/caching.html>

- **API ゲートウェイ層でのログ記録** \
  <https://ocelot.readthedocs.io/en/latest/features/logging.html>

- **API ゲートウェイ層でのサービスの品質 (再試行、サーキット ブレーカー)**  \
  <https://ocelot.readthedocs.io/en/latest/features/qualityofservice.html>

- **速度の制限** \
  [https://ocelot.readthedocs.io/en/latest/features/ratelimiting.html](https://ocelot.readthedocs.io/en/latest/features/ratelimiting.html )

> [!div class="step-by-step"]
> [前へ](background-tasks-with-ihostedservice.md)
> [次へ](../microservice-ddd-cqrs-patterns/index.md)
