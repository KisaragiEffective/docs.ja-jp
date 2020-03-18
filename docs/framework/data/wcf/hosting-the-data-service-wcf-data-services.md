---
title: データ サービスのホスティング (WCF Data Services)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, configuring
- WCF Data Services, Windows Communication Foundation
ms.assetid: b48f42ce-22ce-4f8d-8f0d-f7ddac9125ee
ms.openlocfilehash: 3abcd901bcb8a175aa6f30e53b142cbbde56a579
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73975239"
---
# <a name="hosting-the-data-service-wcf-data-services"></a>データ サービスのホスティング (WCF Data Services)
WCF Data Services を使用すると、データを Open Data Protocol (OData) フィードとして公開するサービスを作成できます。 このデータ サービスは、<xref:System.Data.Services.DataService%601> から継承されたクラスとして定義されます。 このクラスは、要求メッセージを処理し、データソースに対して更新を実行し、OData が必要とする応答メッセージを生成するために必要な機能を提供します。 ただし、データサービスは、受信 HTTP 要求のネットワークソケットにバインドしてリッスンすることはできません。 この機能要件のため、データ サービスはホスティング コンポーネントに依存します。

 データ サービス ホストは、データ サービスに代わって次のタスクを実行します。

- 要求をリッスンし、その要求をデータ サービスにルーティングする。

- 要求ごとにデータ サービスのインスタンスを作成する。

- 受信要求を処理するようデータ サービスに要求する。

- データ サービスに代わって応答を送信する。

 データサービスのホスティングを簡単にするために、WCF Data Services は Windows Communication Foundation (WCF) と統合するように設計されています。 データサービスには、ASP.NET アプリケーションでデータサービスホストとして機能する既定の WCF 実装が用意されています。 そのため、次のいずれかの方法でデータ サービスをホストできます。

- ASP.NET アプリケーション。

- 自己ホスト型 WCF サービスをサポートするマネージド アプリケーション

- その他のカスタム データ サービス ホスト

## <a name="hosting-a-data-service-in-an-aspnet-application"></a>ASP.NET アプリケーションでのデータ サービスのホスト

Visual Studio 2015 の **[新しい項目の追加]** ダイアログボックスを使用して ASP.NET アプリケーションでデータサービスを定義すると、このツールによってプロジェクトに2つの新しいファイルが生成されます。 そのうち一方のファイル (拡張子は `.svc`) は、データ サービスのインスタンス化方法を WCF ランタイムに指示します。 [クイックスタート](quickstart-wcf-data-services.md)を完了したときに作成された Northwind サンプルデータサービスのこのファイルの例を次に示します。

```aspx-csharp
<%@ ServiceHost Language="C#"
    Factory="System.Data.Services.DataServiceHostFactory,
            System.Data.Services, Version=4.0.0.0,
            Culture=neutral, PublicKeyToken=b77a5c561934e089"
    Service="NorthwindService.Northwind" %>
```

 このディレクティブは、<xref:System.Data.Services.DataServiceHostFactory> クラスを使用して、名前付きのデータ サービス クラスのサービス ホストを作成するようアプリケーションに指示します。

 `.svc` ファイルの分離コード ページには、データ サービス自体の実装であるクラスが含まれます。Northwind サンプル データ サービスの場合、このクラスは次のように定義されます。

 [!code-csharp[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_service/cs/northwind.svc.cs#servicedefinition)]
 [!code-vb[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_service/vb/northwind.svc.vb#servicedefinition)]

 データサービスは WCF サービスのように動作するため、データサービスは ASP.NET と統合され、WCF Web プログラミングモデルに従います。 詳細については、「 [Wcf サービスと ASP.NET](../../wcf/feature-details/wcf-services-and-aspnet.md) 」および「 [Wcf Web HTTP プログラミングモデル](../../wcf/feature-details/wcf-web-http-programming-model.md)」を参照してください。

## <a name="self-hosted-wcf-services"></a>自己ホスト型 WCF サービス
 WCF 実装が組み込まれているため、WCF Data Services は、WCF サービスとしてのデータサービスの自己ホストをサポートします。 サービスは、コンソールアプリケーションなど、任意の .NET Framework アプリケーションで自己ホストすることができます。 <xref:System.Data.Services.DataServiceHost> を継承する <xref:System.ServiceModel.Web.WebServiceHost> クラスを使用して、特定のアドレスでデータ サービスをインスタンス化します。

 自己ホストを使用するとサービスの配置とトラブルシューティングが容易になるので、開発時やテスト時に役立ちます。 ただし、この種のホストには、ASP.NET またはインターネットインフォメーションサービス (IIS) によって提供される高度なホスト機能や管理機能は用意されていません。 詳細については、「[マネージアプリケーションでのホスティング](../../wcf/feature-details/hosting-in-a-managed-application.md)」を参照してください。

## <a name="defining-a-custom-data-service-host"></a>カスタム データ サービス ホストの定義
 WCF ホストの実装の制限が厳格すぎる場合、データ サービスのカスタム ホストを定義することもできます。 <xref:System.Data.Services.IDataServiceHost> インターフェイスを実装する任意のクラスをデータ サービスのネットワーク ホストとして使用できます。 カスタム ホストは、<xref:System.Data.Services.IDataServiceHost> インターフェイスを実装し、データ サービス ホストの次の基本的な役割を果たすことができるようにする必要があります。

- データ サービスにサービス ルート パスを提供する。

- 適切な <xref:System.Data.Services.IDataServiceHost> メンバーの実装に対する要求ヘッダーおよび応答ヘッダーの情報を処理する。

- データ サービスで発生する例外を処理する。

- クエリ文字列のパラメーターを検証する。

## <a name="see-also"></a>関連項目

- [WCF Data Services の定義](defining-wcf-data-services.md)
- [サービスとしてのデータの公開](exposing-your-data-as-a-service-wcf-data-services.md)
- [データ サービスの構成](configuring-the-data-service-wcf-data-services.md)
