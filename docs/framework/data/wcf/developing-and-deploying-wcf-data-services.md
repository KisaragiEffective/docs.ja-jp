---
title: WCF Data Services の開発と配置
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, developing
- WCF Data Services, deploying
- deploying [WCF Data Services
- developing applications [WCF Data Services]
ms.assetid: 6557c0e3-5aea-4f6e-bc14-77ad317a168b
ms.openlocfilehash: 1a017267c034fa1d6ea522855b7e0e7f056637ac
ms.sourcegitcommit: f38e527623883b92010cf4760246203073e12898
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77504027"
---
# <a name="develop-and-deploy-wcf-data-services"></a>WCF Data Services の開発とデプロイ

このトピックでは、WCF Data Services の開発と配置について説明します。 WCF Data Services の基本情報については、「[はじめに](getting-started-with-wcf-data-services.md)と[概要](wcf-data-services-overview.md)」を参照してください。

## <a name="develop-wcf-data-services"></a>WCF Data Services の開発

WCF Data Services を使用して、Open Data Protocol (OData) をサポートするデータサービスを作成する場合は、開発時に次の基本的なタスクを実行する必要があります。

1. **データ モデルを定義する**

     WCF Data Services は、リレーショナルデータベースから遅延バインディングデータ型まで、さまざまなデータソースのデータに基づいてデータモデルを定義できるようにする、さまざまなデータサービスプロバイダーをサポートしています。 詳細については、「 [Data Services プロバイダー](data-services-providers-wcf-data-services.md)」を参照してください。

2. **Create the data service**

     最も基本的なデータ サービスでは、 <xref:System.Data.Services.DataService%601> クラスを継承するクラスを `T` 型 (エンティティ コンテナーの名前空間修飾名) と一緒に公開します。 詳細については、「 [Defining WCF Data Services](defining-wcf-data-services.md)の開発と配置について説明します。

3. **データ サービスを構成する**

     既定では、WCF Data Services は、エンティティコンテナーによって公開されているリソースへのアクセスを無効にします。 <xref:System.Data.Services.DataServiceConfiguration> インターフェイスを使用すると、リソースとサービス操作へのアクセスの構成、サポートされている OData のバージョンの指定、および1つの応答フィードで返すことができるエンティティの最大数など、サービス全体のその他の動作を定義できます。 詳細については、「[データサービスの構成](configuring-the-data-service-wcf-data-services.md)」を参照してください。

このトピックでは、主に Visual Studio を使用したデータサービスの開発と配置について説明します。 OData フィードとしてデータを公開するために WCF Data Services によって提供される柔軟性の詳細については、「 [WCF Data Services の定義](defining-wcf-data-services.md)」を参照してください。

### <a name="choose-a-development-web-server"></a>開発 Web サーバーの選択

Visual Studio 2015 を使用して WCF Data Service を ASP.NET アプリケーションまたは ASP.NET Web サイトとして開発する場合、開発時にデータサービスを実行する Web サーバーを選択できます。 次の Web サーバーを Visual Studio と統合すると、ローカルコンピューターでのデータサービスのテストとデバッグが容易になります。

1. **ローカル IIS サーバー**

     インターネットインフォメーションサービス (IIS) で実行される ASP.NET アプリケーションまたは ASP.NET Web サイトであるデータサービスを作成する場合は、ローカルコンピューターで IIS を使用してデータサービスを開発およびテストすることをお勧めします。 IIS でデータ サービスを実行すると、デバッグ時における HTTP 要求のトレースが容易になります。 また、データ サービスに必要なファイルやデータベースなどのリソースにアクセスするために IIS で必要とされる権限を事前に確認することもできます。 IIS でデータサービスを実行するには、IIS と Windows Communication Foundation (WCF) の両方が正しくインストールおよび構成されていることを確認し、ファイルシステムとデータベースの IIS アカウントへのアクセス権を付与する必要があります。 詳細については、「 [How to: Develop a WCF Data Service Running on IIS](how-to-develop-a-wcf-data-service-running-on-iis.md)」を参照してください。

    > [!NOTE]
    > 開発環境でローカル IIS サーバーを構成できるようにするには、管理者権限で Visual Studio を実行する必要があります。

2. **Visual Studio 開発サーバー**

     Visual Studio には、組み込みの Web サーバー、Visual Studio 開発サーバーが含まれています。これは、ASP.NET プロジェクトの既定の Web サーバーです。 この Web サーバーは、開発時にローカルコンピューターで ASP.NET プロジェクトを実行するように設計されています。 [WCF Data Services のクイックスタート](quickstart-wcf-data-services.md)では、Visual Studio 開発サーバーで実行されるデータサービスを作成する方法を示します。

     Visual Studio 開発サーバーを使用してデータサービスを開発する場合は、次の制限事項に注意する必要があります。

    - このサーバーにはローカル コンピューター上でしかアクセスできません。

    - このサーバーは、HTTP メッセージの既定のポートであるポート 80 ではなく、 `localhost` および特定のポートでリッスンします。 詳細については、「 [ASP.NET Web プロジェクト用の Visual Studio の Web サーバー](https://docs.microsoft.com/previous-versions/aspnet/58wxa9w5(v=vs.120))」を参照してください。

    - このサーバーでは、現在のユーザー アカウントのコンテキストでデータ サービスが実行されます。 たとえば、管理者レベルのユーザーとして実行している場合、Visual Studio 開発サーバーで実行されているデータサービスには、管理者レベルの特権が与えられます。 そのため、データ サービスは、IIS サーバーに配置されたときにはアクセスする権限を持たないリソースにも、アクセスできることになります。

    - このサーバーには、認証など、IIS の必要以上の機能は含まれていません。

    - このサーバーは、データサービスから大きなバイナリデータにアクセスするときに、WCF Data Services クライアントによって既定で送信されるチャンク HTTP ストリームを処理できません。 詳細については、「 [Streaming Provider](streaming-provider-wcf-data-services.md)」を参照してください。

    - このサーバーでは、キー値の WCF Data Services でこの文字がサポートされていても、URL のピリオド (`.`) 文字の処理に問題があります。

    > [!TIP]
    > Visual Studio 開発サーバーを使用して開発中にデータサービスをテストすることもできますが、IIS を実行している Web サーバーに配置した後で再テストする必要があります。

3. **Microsoft Azure 開発環境**

     Windows Azure Tools for Visual Studio には、Visual Studio で Windows Azure サービスを開発するためのツールの統合セットが含まれています。 これらのツールでは、Microsoft Azure に配置できるデータ サービスを開発し、配置前にローカル コンピューターでデータ サービスをテストすることができます。 これらのツールは、Visual Studio を使用して、Windows Azure プラットフォームで実行されるデータサービスを開発するときに使用します。 Windows Azure Tools for Visual Studio は、 [Microsoft ダウンロードセンター](https://go.microsoft.com/fwlink/?LinkID=201848)からダウンロードできます。 Windows Azure で実行されるデータサービスの開発の詳細については、「 [Windows azure での OData サービスのデプロイ](https://docs.microsoft.com/archive/blogs/astoriateam/deploying-an-odata-service-in-windows-azure)」を参照してください。

### <a name="development-tips"></a>開発のヒント

データ サービスを開発する際は、次の点を考慮してください。

- ユーザーを認証する場合や、特定のユーザーのアクセスを制限する場合は、データ サービスのセキュリティ要件を決定します。 詳細については、「 [Securing WCF Data Services](securing-wcf-data-services.md)」を参照してください。

- データ サービスをデバッグするときは、HTTP 検査プログラムを使用すると、要求メッセージおよび応答メッセージの内容を検査できるので非常に便利です。 生のパケットを表示できるネットワーク パケット アナライザーを使用すると、データ サービスの HTTP 要求および HTTP 応答を検査できます。

- データサービスをデバッグするときに、通常の操作ではなく、データサービスからエラーに関する詳細情報を取得する必要がある場合があります。 データ サービスから詳細なエラー情報を取得するには、 <xref:System.Data.Services.DataServiceConfiguration.UseVerboseErrors%2A> の <xref:System.Data.Services.DataServiceConfiguration> プロパティを `true` に設定し、データ サービス クラスの <xref:System.ServiceModel.Description.ServiceDebugBehavior.IncludeExceptionDetailInFaults%2A> 属性の <xref:System.ServiceModel.Description.ServiceDebugBehavior> プロパティを `true`に設定します。 詳細については、デバッグ後の[WCF Data Services](https://docs.microsoft.com/archive/blogs/phaniraj/debugging-wcf-data-services)を参照してください。 また、WCF でトレースを有効にして、HTTP メッセージングレイヤーで発生した例外を表示することもできます。 詳細については、「 [Configuring Tracing](../../wcf/diagnostics/tracing/configuring-tracing.md)」を参照してください。

- 通常、データサービスは ASP.NET アプリケーションプロジェクトとして開発されますが、データサービスは、Visual Studio で ASP.NET Web サイトプロジェクトとして作成することもできます。 2種類のプロジェクトの違いについては、「 [Visual Studio での Web アプリケーションプロジェクトと Web サイトプロジェクト](https://docs.microsoft.com/previous-versions/aspnet/dd547590(v=vs.110))」を参照してください。

- Visual Studio の **[新しい項目の追加]** ダイアログボックスを使用してデータサービスを作成する場合、データサービスは IIS の ASP.NET によってホストされます。 ASP.NET と IIS はデータサービスの既定のホストですが、他のホストオプションもサポートされています。 詳細については、「[データサービスのホスト](hosting-the-data-service-wcf-data-services.md)」を参照してください。

## <a name="deploy-wcf-data-services"></a>WCF Data Services のデプロイ

WCF Data Services では、データ サービスをホストするプロセスを柔軟に選択できます。 Visual Studio を使用して、次のプラットフォームにデータサービスを配置できます。

- **IIS でホストされる Web サーバー**

    ASP.NET プロジェクトとして開発されたデータサービスは、標準の ASP.NET デプロイプロセスを使用して IIS Web サーバーに配置できます。  Visual Studio には、配置するデータサービスをホストする ASP.NET プロジェクトの種類に応じて、ASP.NET の次の配置テクノロジが用意されています。

  - **ASP.NET Web アプリケーション用の配置テクノロジ**

    - [方法: Visual Studio で Web 配置パッケージを作成する](https://docs.microsoft.com/previous-versions/aspnet/dd465323(v=vs.110))

    - [方法: Visual Studio でワンクリック発行を使用して Web プロジェクトを配置する](https://docs.microsoft.com/previous-versions/aspnet/dd465337(v=vs.110))

  - **ASP.NET Web サイト用の配置テクノロジ**

    - [方法: web サイトのコピーツールを使用して Web サイトファイルをコピーする](https://docs.microsoft.com/previous-versions/aspnet/c95809c0(v=vs.100))

    - [方法: Web サイトを発行する](https://docs.microsoft.com/previous-versions/aspnet/20yh9f1b(v=vs.100))

    - [チュートリアル: XCOPY を使用した ASP.NET Web アプリケーションのデプロイ](https://docs.microsoft.com/previous-versions/aspnet/f735abw9(v=vs.100))

     ASP.NET アプリケーションの配置オプションの詳細については、「 [Visual Studio と ASP.NET の Web 配置の概要](https://docs.microsoft.com/previous-versions/aspnet/dd394698(v=vs.110))」を参照してください。

    > [!TIP]
    > データ サービスを IIS に配置する前に、IIS を実行している Web サーバーへの配置をテストしておく必要があります。 詳細については、「 [How to: Develop a WCF Data Service Running on IIS](how-to-develop-a-wcf-data-service-running-on-iis.md)」を参照してください。

- **Windows Azure**

     Windows azure Tools for Visual Studio を使用して、データサービスを Windows Azure に配置できます。 Windows Azure Tools for Visual Studio は、 [Microsoft ダウンロードセンター](https://go.microsoft.com/fwlink/?LinkID=201848)からダウンロードできます。 Windows Azure へのデータサービスの配置の詳細については、「 [Windows azure での OData サービスのデプロイ](https://docs.microsoft.com/archive/blogs/astoriateam/deploying-an-odata-service-in-windows-azure)」を参照してください。

### <a name="deployment-considerations"></a>配置に関する考慮事項

データ サービスを配置する際は、次の点を考慮してください。

- Entity Framework プロバイダーを使用して SQL Server データベースにアクセスするデータサービスを配置する場合は、データサービスの配置にデータ構造、データ、またはその両方を伝達することが必要になる場合があります。 Visual Studio では、スクリプト (.sql ファイル) を自動的に作成してコピー先データベースで実行できます。これらのスクリプトは、ASP.NET アプリケーションの Web 配置パッケージに含めることができます。 詳細については、「[方法: Web アプリケーションプロジェクトを使用してデータベースを配置](https://docs.microsoft.com/previous-versions/dd465343(v=vs.100))する」を参照してください。 ASP.NET Web サイトでは、Visual Studio の**データベース発行ウィザード**を使用してこれを行うことができます。 詳細については、「 [SQL Database の発行](https://docs.microsoft.com/previous-versions/aspnet/bb907585(v=vs.100))」を参照してください。

- WCF Data Services には基本的な WCF 実装が含まれているため、windows server AppFabric を使用して、Windows Server 上で実行されている IIS に配置されたデータサービスを監視できます。 Windows Server AppFabric を使用したデータサービスの監視の詳細については、「 [Windows Server appfabric を使用した WCF Data Services の追跡](https://docs.microsoft.com/archive/blogs/rjacobs/tracking-wcf-data-services-with-windows-server-appfabric)」を参照してください。

## <a name="see-also"></a>参照

- [データ サービスのホスティング](hosting-the-data-service-wcf-data-services.md)
- [WCF Data Services のセキュリティ保護](securing-wcf-data-services.md)
- [Defining WCF Data Services](defining-wcf-data-services.md)
