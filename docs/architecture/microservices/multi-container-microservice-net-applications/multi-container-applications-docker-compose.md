---
title: docker-compose.yml で複数のコンテナー アプリケーションを定義する
description: docker-compose.yml を使用して複数コンテナーのアプリケーション用にマイクロサービスの構成を指定する方法。
ms.date: 01/30/2020
ms.openlocfilehash: 86d6feda343df7f4b72374f93fc45b3246780cdf
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77502465"
---
# <a name="defining-your-multi-container-application-with-docker-composeyml"></a>docker-compose.yml で複数のコンテナー アプリケーションを定義する

このガイドでは、[docker-compose.yml](https://docs.docker.com/compose/compose-file/) ファイルは、「[手順 4 複数のコンテナー Docker アプリケーションを構築するときに docker compose.yml で、サービスを定義します](../docker-application-development-process/docker-app-development-workflow.md#step-4-define-your-services-in-docker-composeyml-when-building-a-multi-container-docker-application)」セクションで導入されました。 しかし、docker-compose ファイルには他の使い方もあり、さらに詳しく検討する価値があります。

たとえば、マルチコンテナー アプリケーションの展開方法を、docker-compose.yml ファイルで明示的に記述することができます。 必要に応じて、カスタム Docker イメージのビルド方法も記述できます (カスタム Docker イメージは、Docker CLI を使用してビルドすることもできます)。

基本的には、展開する各コンテナーを定義することに加え、各コンテナー展開に対して特定の特性も定義します。 マルチコンテナー展開の記述ファイルを作成したら、全体のソリューションを、[docker-compose up](https://docs.docker.com/compose/overview/) CLI コマンドで調整された 1 つのアクションで展開することができます。または、Visual Studio から透過的に展開することもできます。 それ以外の場合は、Docker CLI を使用して、コマンドラインから `docker run` コマンドを使用して、複数の手順でコンテナーごとに展開する必要があります。 そのため、docker-compose.yml で定義された各サービスは、イメージまたはビルドを 1 つだけ指定する必要があります。 その他のキーは省略可能で、その対応する `docker run` コマンド ラインと似ています。

次の YAML コードは、グローバルに使用できる定義ですが、eShopOnContainers サンプル用の 1 つの docker-compose.yml ファイルです。 これは eShopOnContainers からの実際の docker-compose ファイルではありません。 むしろ、簡素化され、1 つのファイルに統合されたバージョンです。後述しますが、これは docker-compose ファイルで使用するには最善の方法ではありません。

```yml
version: '3.4'

services:
  webmvc:
    image: eshop/webmvc
    environment:
      - CatalogUrl=http://catalog-api
      - OrderingUrl=http://ordering-api
      - BasketUrl=http://basket-api
    ports:
      - "5100:80"
    depends_on:
      - catalog-api
      - ordering-api
      - basket-api

  catalog-api:
    image: eshop/catalog-api
    environment:
      - ConnectionString=Server=sqldata;Initial Catalog=CatalogData;User Id=sa;Password=your@password
    expose:
      - "80"
    ports:
      - "5101:80"
    #extra hosts can be used for standalone SQL Server or services at the dev PC
    extra_hosts:
      - "CESARDLSURFBOOK:10.0.75.1"
    depends_on:
      - sqldata

  ordering-api:
    image: eshop/ordering-api
    environment:
      - ConnectionString=Server=sqldata;Database=Services.OrderingDb;User Id=sa;Password=your@password
    ports:
      - "5102:80"
    #extra hosts can be used for standalone SQL Server or services at the dev PC
    extra_hosts:
      - "CESARDLSURFBOOK:10.0.75.1"
    depends_on:
      - sqldata

  basket-api:
    image: eshop/basket-api
    environment:
      - ConnectionString=sqldata
    ports:
      - "5103:80"
    depends_on:
      - sqldata

  sqldata:
    environment:
      - SA_PASSWORD=your@password
      - ACCEPT_EULA=Y
    ports:
      - "5434:1433"

  basketdata:
    image: redis
```

このファイルのルート キーはサービスです。 そのキーの下で、`docker-compose up` コマンドを実行するとき、またはこの docker-compose.yml ファイルを使用して Visual Studio から展開するときに、展開および実行するサービスを定義します。 この例では、次の表に示すように、docker-compose.yml ファイルでは複数のサービスが定義されています。

| サービス名 | 説明 |
|--------------|-------------|
| webmvc       | サーバー側 C\# からマイクロサービスを消費する ASP.NET Core MVC アプリケーションを含むコンテナー|
| catalog-api  | Catalog ASP.NET Core Web API マイクロサービスを含むコンテナー |
| ordering-api | Ordering ASP.NET Core Web API マイクロサービスを含むコンテナー |
| sqldata     | マイクロサービス データベースを保持し、SQL Server for Linux が稼働するコンテナー |
| basket-api   | Basket ASP.NET Core Web API マイクロサービスを使用するコンテナー |
| basketdata  | REDIS キャッシュ サービスが稼働し、REDIS キャッシュとしてバスケット データベースを持つコンテナー |

### <a name="a-simple-web-service-api-container"></a>単純な Web サービス API コンテナー

1 つのコンテナーに集中することで、catalog.api container-microservice の定義が単純になります。

```yml
  catalog-api:
    image: eshop/catalog-api
    environment:
      - ConnectionString=Server=sqldata;Initial Catalog=CatalogData;User Id=sa;Password=your@password
    expose:
      - "80"
    ports:
      - "5101:80"
    #extra hosts can be used for standalone SQL Server or services at the dev PC
    extra_hosts:
      - "CESARDLSURFBOOK:10.0.75.1"
    depends_on:
      - sqldata
```

このコンテナー化されたサービスには、次の基本的な構成があります。

- それは、カスタムの **eshop/catalog.api** イメージに基づいています。 わかりやすくするために、ファイル内にビルドやキーの設定はありません。 つまりイメージが、(docker build を使用して) 既にビルドされているか、(docker pull コマンドを使用して) 任意の Docker レジストリからダウンロードされている必要があります。

- 接続文字列を使用して ConnectionString という名前の環境変数を定義します。この変数は、カタログ データ モデルを含む SQL Server インスタンスにアクセスするために Entity Framework によって使用されます。 この例では、同じ SQL Server コンテナーに複数のデータベースが保持されています。 そのため、Docker 用に開発用コンピューターで必要なメモリ量が減ります。 ただし、マイクロサービス データベースごとに 1 つの SQL Server のコンテナーを展開することもできます。

- SQL Server 名は **sqldata** で、Linux 用の SQL Server インスタンスが実行されているコンテナーで使用されているのと同じ名前です。 これは便利な方法です。この名前解決 (内部から Docker ホストへ) を使用できることで、ネットワーク アドレスが解決されるため、他のコンテナーからアクセスするコンテナーの内部 IP アドレスを知る必要はありません。

接続文字列は環境変数によって定義されるため、異なる時点で別のメカニズムを使用して、その変数を設定することができます。 たとえば、最後のホストで運用に展開するときに、または Azure DevOps Services の CI/CD パイプラインから、または優先する DevOps システムから、別の接続文字列を設定できます。

- Docker ホスト内で **catalog-api** サービスへの内部アクセス用に、ポート 80 が公開されます。 ホストは、Linux の Docker イメージに基づいているため、現在は Linux VM ですが、代わりに Windows イメージ上で実行するようにコンテナーを構成することもできます。

- コンテナー上で公開されているポート 80 を、Docker ホスト コンピューター (Linux VM) 上のポート 5101 に転送します。

- Web サービスを **sqldata** サービス (コンテナーで実行されている Linux データベースの SQL Server インスタンス) にリンクします。 この依存関係を指定すると、catalog-api コンテナーは、sqldata コンテナーが起動するまで起動しなくなります。これが重要なのは、catalog-api では、SQL Server データベースが先に起動して実行されている必要があるからです。 ただし、このようなコンテナーの依存関係は、Docker がコンテナー レベルでしかチェックしないため、多くの場合、不十分です。 サービス (この場合は SQL Server) がまだ準備できていない場合もあるため、クライアント マイクロサービスで指数バックオフによる再試行ロジックを実装することをお勧めします。 そうすることで、依存関係のコンテナーが少しの間、準備できない場合でも、アプリケーションが回復力を保つことができます。

- これは外部サーバーへのアクセスを許可するように構成されます: extra\_hosts 設定により、開発用 PC 上のローカル SQL Server インスタンスなど、Docker ホストの外 (つまり、開発用 Docker ホストである既定の Linux VM の外) にある外部のサーバーやマシンにアクセスすることができます。

より高度な他の `docker-compose.yml` 設定もあります。これについては以降のセクションで説明します。

### <a name="using-docker-compose-files-to-target-multiple-environments"></a>docker-compose ファイルを使用して複数の環境をターゲットにする

`docker-compose.*.yml` ファイルは定義ファイルで、その形式を理解する複数のインフラストラクチャで使用できます。 最も簡単なツールは、docker-compose コマンドです。

そのため、docker-compose コマンドを使用することで、次の主なシナリオをターゲットにすることができます。

#### <a name="development-environments"></a>開発環境

アプリケーションを開発するときには、アプリケーションを分離開発環境で実行できることが重要です。 docker-compose CLI コマンドを使用してその環境を作成するか、Visual Studio を使用することができます。そこでは内部的に docker-compose が使用されます。

docker-compose.yml ファイルを使用すると、アプリケーションのすべてのサービスの依存関係 (その他のサービス、キャッシュ、データベース、キューなど) を構成および文書化できます。 docker-compose CLI コマンドを使用すると、1 つのコマンド (docker-compose up) を使用して、依存関係ごとに 1 つ以上のコンテナーを作成して起動することができます。

docker-compose.yml ファイルは、Docker エンジンによって解釈される構成ファイルですが、マルチコンテナー アプリケーションの構成に関する便利なドキュメント ファイルとしても機能します。

#### <a name="testing-environments"></a>テスト環境

継続的配置 (CD) または継続的インテグレーション (CI) プロセスの重要な部分は、単体テストと統合テストです。 これらの自動テストでは、ユーザーまたはアプリケーションのデータ内の他の変更による影響を受けないように、分離環境が必要です。

Docker Compose を使用すると、コマンド プロンプトまたはスクリプトから、次のコマンドのように、いくつかのコマンドで簡単にその分離環境を作成および破棄することができます。

```console
docker-compose -f docker-compose.yml -f docker-compose-test.override.yml up -d
./run_unit_tests
docker-compose -f docker-compose.yml -f docker-compose.test.override.yml down
```

#### <a name="production-deployments"></a>運用展開

Compose を使用してリモート Docker エンジンに展開することもできます。 一般的なケースは、(運用 VM または [Docker マシン](https://docs.docker.com/machine/overview/)でプロビジョニングされたサーバーのように) 1 つの Docker ホスト インスタンスに展開することです。

他の任意のオーケストレーター (Azure Service Fabric、Kubernetes など) を使用している場合は、docker-compose.yml にあるようなセットアップとメタデータの構成設定を、他のオーケストレーターで必要な形式で追加する必要がある場合があります。

いずれの場合も、docker-compose は、開発、テスト、運用ワークフローにとって便利なツールとメタデータ形式ですが、運用ワークフローはお使いのオーケストレーターによって異なる場合があります。

### <a name="using-multiple-docker-compose-files-to-handle-several-environments"></a>複数の docker-compose ファイルを使用して複数の環境を処理する

さまざまな環境をターゲットとする場合は、複数の compose ファイルを使用する必要があります。 これにより、環境に応じて複数の構成バリアントを作成できます。

#### <a name="overriding-the-base-docker-compose-file"></a>基本の docker-compose ファイルをオーバーライドする

前のセクションで示した簡略化された例のように、1 つの docker-compose.yml ファイルを使用できます。 ただし、これはほとんどのアプリケーションにはお勧めできません。

既定では、Compose は docker-compose.yml と省略可能な docker-compose.override.yml の 2 つのファイルを読み取ります。 図 6-11 に示すように、Visual Studio を使用している場合に Docker サポートを有効にすると、アプリケーションをデバッグするための docker compose.vs.debug.g.yml ファイルも Visual Studio によって追加で作成されます。このファイルは、メイン ソリューション フォルダー内の obj\\Docker\\ フォルダーで確認できます。

![docker compose プロジェクト内のファイルのスクリーンショット。](./media/multi-container-applications-docker-compose/docker-compose-file-visual-studio.png)

**図 6-11**。 Visual Studio 2019 の docker-compose ファイル

**docker-compose** プロジェクトのファイル構造:

- *.dockerignore* - ファイルを無視するために使用
- *docker-compose.yml* - マイクロサービスを作成するために使用
- *docker-compose.override.yml* - マイクロサービス環境を構成するために使用

Visual Studio Code や Sublime などの任意のエディターを使用して docker-compose ファイルを編集し、docker-compose up コマンドを使用してアプリケーションを実行することができます。

慣例により、docker-compose.yml ファイルには、基本構成とその他の静的な設定が含まれています。 つまり、ターゲットとしている展開環境に合わせてサービス構成を変更しないでください。

docker-compose.override.yml ファイルには、その名前が示すように、基本構成 (展開環境に依存する構成など) をオーバーライドする構成設定が含まれています。 また、複数の override ファイルを異なる名前で持つことができます。 override ファイルには通常、アプリケーションで必要な、環境または展開に固有の追加情報が含まれています。

#### <a name="targeting-multiple-environments"></a>複数の環境をターゲットにする

一般的なユース ケースは、運用、ステージング、CI、開発などの複数の環境をターゲットにできるように、複数の compose ファイルを定義する場合です。 これらの相違点をサポートするため、図 6-12 に示すように、ご利用の Compose 構成を複数のファイルに分割できます。

![基本ファイルをオーバーライドするように設定された 3 つの docker-compose ファイルの図。](./media/multi-container-applications-docker-compose/multiple-docker-compose-files-override-base.png)

**図 6-12**。 基本の docker-compose.yml ファイル内の値をオーバーライドする複数の docker-compose ファイル

複数の docker-compose*.yml ファイルを組み合わせて、さまざまな環境を処理することができます。 基本の docker-compose.yml ファイルから開始します。 この基本ファイルには、環境に合わせて変更されない基本構成または静的な構成設定が含まれている必要があります。 たとえば、eShopOnContainers には、基本ファイルとして次の docker-compose.yml ファイル (サービスを減らして簡略化したもの) があります。

```yml
#docker-compose.yml (Base)
version: '3.4'
services:
  basket-api:
    image: eshop/basket-api:${TAG:-latest}
    build:
      context: .
      dockerfile: src/Services/Basket/Basket.API/Dockerfile
    depends_on:
      - basketdata
      - identity-api
      - rabbitmq

  catalog-api:
    image: eshop/catalog-api:${TAG:-latest}
    build:
      context: .
      dockerfile: src/Services/Catalog/Catalog.API/Dockerfile
    depends_on:
      - sqldata
      - rabbitmq

  marketing-api:
    image: eshop/marketing-api:${TAG:-latest}
    build:
      context: .
      dockerfile: src/Services/Marketing/Marketing.API/Dockerfile
    depends_on:
      - sqldata
      - nosqldata
      - identity-api
      - rabbitmq

  webmvc:
    image: eshop/webmvc:${TAG:-latest}
    build:
      context: .
      dockerfile: src/Web/WebMVC/Dockerfile
    depends_on:
      - catalog-api
      - ordering-api
      - identity-api
      - basket-api
      - marketing-api

  sqldata:
    image: mcr.microsoft.com/mssql/server:2017-latest

  nosqldata:
    image: mongo

  basketdata:
    image: redis

  rabbitmq:
    image: rabbitmq:3-management

```

基本の docker-compose.yml ファイル内の値は、ターゲット展開環境が異なるため、変更しないでください。

たとえば、webmvc サービス定義に注目すると、ターゲットとする環境が何であれ、その情報がほとんど同じであることがわかります。 次の情報があります。

- サービス名: webmvc

- コンテナーのカスタム イメージ: eshop/webmvc

- 使用する Dockerfile を示す、カスタム Docker イメージをビルドするコマンド

- 他の依存関係コンテナーが起動されるまで、このコンテナーを起動しないようする、他のサービスへの依存関係

追加の構成を持つことはできますが、重要な点は、基本の docker-compose.yml ファイルでは、環境間で共通する情報だけを設定することです。 その後、docker-compose.override.yml または運用またはステージングの同様のファイルで、各環境に固有の構成を配置する必要があります。

通常、docker-compose.override.yml は、eShopOnContainers からの次の例のように、開発環境のために使用されます。

```yml
#docker-compose.override.yml (Extended config for DEVELOPMENT env.)
version: '3.4'

services:
# Simplified number of services here:

  basket-api:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://0.0.0.0:80
      - ConnectionString=${ESHOP_AZURE_REDIS_BASKET_DB:-basketdata}
      - identityUrl=http://identity-api
      - IdentityUrlExternal=http://${ESHOP_EXTERNAL_DNS_NAME_OR_IP}:5105
      - EventBusConnection=${ESHOP_AZURE_SERVICE_BUS:-rabbitmq}
      - EventBusUserName=${ESHOP_SERVICE_BUS_USERNAME}
      - EventBusPassword=${ESHOP_SERVICE_BUS_PASSWORD}
      - AzureServiceBusEnabled=False
      - ApplicationInsights__InstrumentationKey=${INSTRUMENTATION_KEY}
      - OrchestratorType=${ORCHESTRATOR_TYPE}
      - UseLoadTest=${USE_LOADTEST:-False}

    ports:
      - "5103:80"

  catalog-api:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://0.0.0.0:80
      - ConnectionString=${ESHOP_AZURE_CATALOG_DB:-Server=sqldata;Database=Microsoft.eShopOnContainers.Services.CatalogDb;User Id=sa;Password=Pass@word}
      - PicBaseUrl=${ESHOP_AZURE_STORAGE_CATALOG_URL:-http://localhost:5202/api/v1/catalog/items/[0]/pic/}
      - EventBusConnection=${ESHOP_AZURE_SERVICE_BUS:-rabbitmq}
      - EventBusUserName=${ESHOP_SERVICE_BUS_USERNAME}
      - EventBusPassword=${ESHOP_SERVICE_BUS_PASSWORD}
      - AzureStorageAccountName=${ESHOP_AZURE_STORAGE_CATALOG_NAME}
      - AzureStorageAccountKey=${ESHOP_AZURE_STORAGE_CATALOG_KEY}
      - UseCustomizationData=True
      - AzureServiceBusEnabled=False
      - AzureStorageEnabled=False
      - ApplicationInsights__InstrumentationKey=${INSTRUMENTATION_KEY}
      - OrchestratorType=${ORCHESTRATOR_TYPE}
    ports:
      - "5101:80"

  marketing-api:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://0.0.0.0:80
      - ConnectionString=${ESHOP_AZURE_MARKETING_DB:-Server=sqldata;Database=Microsoft.eShopOnContainers.Services.MarketingDb;User Id=sa;Password=Pass@word}
      - MongoConnectionString=${ESHOP_AZURE_COSMOSDB:-mongodb://nosqldata}
      - MongoDatabase=MarketingDb
      - EventBusConnection=${ESHOP_AZURE_SERVICE_BUS:-rabbitmq}
      - EventBusUserName=${ESHOP_SERVICE_BUS_USERNAME}
      - EventBusPassword=${ESHOP_SERVICE_BUS_PASSWORD}
      - identityUrl=http://identity-api
      - IdentityUrlExternal=http://${ESHOP_EXTERNAL_DNS_NAME_OR_IP}:5105
      - CampaignDetailFunctionUri=${ESHOP_AZUREFUNC_CAMPAIGN_DETAILS_URI}
      - PicBaseUrl=${ESHOP_AZURE_STORAGE_MARKETING_URL:-http://localhost:5110/api/v1/campaigns/[0]/pic/}
      - AzureStorageAccountName=${ESHOP_AZURE_STORAGE_MARKETING_NAME}
      - AzureStorageAccountKey=${ESHOP_AZURE_STORAGE_MARKETING_KEY}
      - AzureServiceBusEnabled=False
      - AzureStorageEnabled=False
      - ApplicationInsights__InstrumentationKey=${INSTRUMENTATION_KEY}
      - OrchestratorType=${ORCHESTRATOR_TYPE}
      - UseLoadTest=${USE_LOADTEST:-False}
    ports:
      - "5110:80"

  webmvc:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://0.0.0.0:80
      - PurchaseUrl=http://webshoppingapigw
      - IdentityUrl=http://10.0.75.1:5105
      - MarketingUrl=http://webmarketingapigw
      - CatalogUrlHC=http://catalog-api/hc
      - OrderingUrlHC=http://ordering-api/hc
      - IdentityUrlHC=http://identity-api/hc
      - BasketUrlHC=http://basket-api/hc
      - MarketingUrlHC=http://marketing-api/hc
      - PaymentUrlHC=http://payment-api/hc
      - SignalrHubUrl=http://${ESHOP_EXTERNAL_DNS_NAME_OR_IP}:5202
      - UseCustomizationData=True
      - ApplicationInsights__InstrumentationKey=${INSTRUMENTATION_KEY}
      - OrchestratorType=${ORCHESTRATOR_TYPE}
      - UseLoadTest=${USE_LOADTEST:-False}
    ports:
      - "5100:80"
  sqldata:
    environment:
      - SA_PASSWORD=Pass@word
      - ACCEPT_EULA=Y
    ports:
      - "5433:1433"
  nosqldata:
    ports:
      - "27017:27017"
  basketdata:
    ports:
      - "6379:6379"
  rabbitmq:
    ports:
      - "15672:15672"
      - "5672:5672"

```

この例では、開発オーバーライド構成でいくつかのポートをホストに公開し、リダイレクト URL で環境変数を定義し、開発環境への接続文字列を指定します。 これらの設定はすべて、開発環境のためだけのものです。

`docker-compose up` を実行 (または Visual Studio から起動) すると、コマンドは両方のオーバーライドを、2 つのファイルがマージされているかのように、自動的に読み取ります。

運用環境用に、異なる構成値、ポート、または接続文字列を持つ別の Compose ファイルが必要だとします。 異なる設定と環境変数を使用して、`docker-compose.prod.yml` という名前の別の override ファイルを作成できます。 そのファイルは、別の Git リポジトリに格納したり、別のチームによって管理および保護することができます。

#### <a name="how-to-deploy-with-a-specific-override-file"></a>特定の override ファイルによる展開方法

複数の override ファイルを使用する、または異なる名前の override ファイルを使用するには、docker-compose コマンドで -f オプションを使用してファイルを指定することができます。 Compose によって、コマンドラインで指定された順序でファイルがマージされます。 次の例は、override ファイルを使用した展開方法を示しています。

```console
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

#### <a name="using-environment-variables-in-docker-compose-files"></a>docker-compose ファイルで環境変数を使用する

前の例に示したように、特に運用環境では、環境変数から構成情報を取得できると便利です。 構文 ${MY\_VAR} を使用して、docker-compose ファイル内の環境変数を参照できます。 docker-compose.prod.yml ファイルからの次の行は、環境変数の値の参照方法を示しています。

```yml
IdentityUrl=http://${ESHOP_PROD_EXTERNAL_DNS_NAME_OR_IP}:5105
```

環境変数が作成され、ホスト環境 (Linux、Windows、クラウド クラスターなど) に応じて、さまざまな方法で初期化されます。 しかし、便利な方法は、.env ファイルを使用することです。 docker-compose ファイルは、.env ファイルで既定の環境変数を宣言することをサポートします。 環境変数のこれらの値が既定値です。 ただしこれらは、それぞれの環境 (ホスト OS またはクラスターからの環境変数) で定義した値によってオーバーライドできます。 この .env ファイルを、docker-compose コマンドが実行される場所に配置します。

次の例は、eShopOnContainers アプリケーション用の [.env](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/.env) ファイルと同様の .env ファイルを示しています。

```sh
# .env file

ESHOP_EXTERNAL_DNS_NAME_OR_IP=localhost

ESHOP_PROD_EXTERNAL_DNS_NAME_OR_IP=10.121.122.92
```

Docker-compose は、.env ファイル内の各行が \<変数\>=\<値\> の形式になることを想定しています。

ランタイム環境で設定された値は、.env ファイル内で定義されている値を常にオーバーライドします。 同様に、コマンド ライン引数を介して渡される値も、.env ファイルで設定された既定値をオーバーライドします。

#### <a name="additional-resources"></a>その他の技術情報

- **Docker Compose の概要** \
    <https://docs.docker.com/compose/overview/>

- **複数の Compose ファイル** \
    [https://docs.docker.com/compose/extends/\#multiple-compose-files](https://docs.docker.com/compose/extends/#multiple-compose-files)

### <a name="building-optimized-aspnet-core-docker-images"></a>最適化された ASP.NET Core Docker イメージをビルドする

インターネット上のソースで Docker や .NET Core を検索すると、ソースをコンテナーにコピーして Docker イメージを簡単にビルドする方法を示す Dockerfile が見つかります。 これらの例では、単純な構成を使用することで、ご利用のアプリケーションにパッケージ化された環境で Docker イメージを持つことができます。 次の例は、このような単純な Dockerfile を示しています。

```Dockerfile
FROM mcr.microsoft.com/dotnet/core/sdk:3.1
WORKDIR /app
ENV ASPNETCORE_URLS http://+:80
EXPOSE 80
COPY . .
RUN dotnet restore
ENTRYPOINT ["dotnet", "run"]
```

このような Dockerfile は機能します。 ただし、イメージ、とりわけ運用イメージは大幅に最適化できます。

コンテナーとマイクロサービス モデルでは、常にコンテナーを起動します。 コンテナーは破棄可能なため、コンテナーの一般的な使用方法では、スリープ状態のコンテナーを再起動しません。 オーケストレーター (Kubernetes や Azure Service Fabric など) では、イメージの新しいインスタンスが作成されるだけです。 これは、インスタンス化のプロセスを高速化するため、アプリケーションのビルド時に、アプリケーションをプリコンパイルして最適化する必要があることを意味します。 コンテナーは起動すると、実行できる状態になります。 .NET Core と Docker に関する多くのブログ記事で見られるような、dotnet CLI から `dotnet restore` コマンドおよび `dotnet build` コマンドを使用した、実行時の復元およびコンパイルは行わないでください。

.NET チームは、.NET Core と ASP.NET Core をコンテナー用に最適化されたフレームワークにするための重要な作業を行っています。 .NET Core は、メモリの使用量を抑えた簡易フレームワークというだけではありません。バージョン 2.1 以降、チームでは次の 3 つの主なシナリオに合わせた Docker イメージの最適化に重点を置き、*dotnet/core* にある Docker Hub レジストリに、最適化されたイメージを発行してきました。

1. **開発**: 変更の繰り返しとデバッグを迅速に行う機能が優先され、サイズは 2 番目です。

2. **ビルド:** アプリケーションのコンパイルが優先され、これにはバイナリや、バイナリを最適化するための他の依存関係が含まれています。

3. **実稼働**: コンテナーを迅速に展開し、開始することに重点が置かれます。そのため、これらのイメージは、バイナリと、アプリケーションを稼働させるために必要なコンテンツに限定されます。

これを実現するために、.NET チームは、次の 4 つの基本的なバリエーションを [dotnet/core](https://hub.docker.com/_/microsoft-dotnet-core/) (Docker Hub) に用意しています。

1. **sdk**: 開発シナリオおよびビルド シナリオ向け
1. **aspnet**: ASP.NET 運用シナリオ向け
1. **runtime**: .NET 運用シナリオ向け
1. **runtime-deps**: [自己完結型アプリケーション](../../../core/deploying/index.md#publish-self-contained)の運用シナリオ向け。

迅速に起動するために、ランタイム イメージでも aspnetcore\_urls にポート 80 が自動的に設定され、Ngen を使用してアセンブリのネイティブ イメージ キャッシュが作成されます。

#### <a name="additional-resources"></a>その他の技術情報

- **ASP.NET Core を使用して最適化された Docker イメージをビルドする**  
  <https://docs.microsoft.com/archive/blogs/stevelasker/building-optimized-docker-images-with-asp-net-core>

- **.NET Core アプリケーションの Docker イメージのビルド**  
  [https://docs.microsoft.com/dotnet/core/docker/building-net-docker-images](/aspnet/core/host-and-deploy/docker/building-net-docker-images)

> [!div class="step-by-step"]
> [前へ](data-driven-crud-microservice.md)
> [次へ](database-server-container.md)
