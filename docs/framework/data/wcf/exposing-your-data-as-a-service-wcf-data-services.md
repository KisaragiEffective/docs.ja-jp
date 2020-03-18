---
title: サービスとしてのデータの公開 (WCF Data Services)
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, configuring
- getting started, WCF Data Services
- WCF Data Services, getting started
ms.assetid: df0bbcee-f66f-4a88-abb4-4e73c8b9c908
ms.openlocfilehash: 71f26d7d41d112e13e6a77f0927c61d51b176b27
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73975308"
---
# <a name="expose-your-data-as-a-service-wcf-data-services"></a>データをサービスとして公開する (WCF Data Services)

WCF Data Services は、データを Open Data Protocol (OData) フィードとして公開するサービスをより簡単に定義できるように、Visual Studio と統合されています。 OData フィードを公開するデータサービスを作成するには、次の基本的な手順を実行する必要があります。

1. **データモデルを定義します。** WCF Data Services は、 [ADO.NET Entity Framework](../adonet/ef/index.md)に基づくデータモデルをネイティブでサポートしています。 詳細については、「[方法: ADO.NET Entity Framework データソースを使用してデータサービスを作成](create-a-data-service-using-an-adonet-ef-data-wcf.md)する」を参照してください。

     WCF Data Services は、<xref:System.Linq.IQueryable%601> インターフェイスのインスタンスを返す共通言語ランタイム (CLR) オブジェクトに基づくデータモデルもサポートしています。 そのため、.NET Framework のリスト、配列、およびコレクションに基づいてデータ サービスを配置できます。 これらのデータ構造での作成、更新、および削除操作を有効にするには、<xref:System.Data.Services.IUpdatable> インターフェイスも実装する必要があります。 詳細については、「[方法: リフレクションプロバイダーを使用してデータサービスを作成](create-a-data-service-using-rp-wcf-data-services.md)する」を参照してください。

     より高度なシナリオでは、WCF Data Services には、遅延バインディングされたデータ型に基づいてデータモデルを定義できるプロバイダーのセットが含まれています。 詳細については、「[カスタムデータサービスプロバイダー](custom-data-service-providers-wcf-data-services.md)」を参照してください。

2. **データサービスを作成します。** 最も基本的なデータ サービスでは、 <xref:System.Data.Services.DataService%601> クラスを継承するクラスを `T` 型 (エンティティ コンテナーの名前空間修飾名) と一緒に公開します。 詳細については、「 [Defining WCF Data Services](defining-wcf-data-services.md)の開発と配置について説明します。

3. **データサービスを構成します。** 既定では、WCF Data Services は、エンティティコンテナーによって公開されているリソースへのアクセスを無効にします。 <xref:System.Data.Services.DataServiceConfiguration> インターフェイスを使用すると、リソースとサービス操作へのアクセスの構成、サポートされている OData のバージョンの指定、およびその他のサービス全体の動作 (バッチ動作や単一の応答で返すことができるエンティティの最大数など) を定義できます。 詳細については、「[データサービスの構成](configuring-the-data-service-wcf-data-services.md)」を参照してください。

Northwind サンプルデータベースに基づく単純なデータサービスを作成する方法の例については、「[クイックスタート](quickstart-wcf-data-services.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [はじめに](getting-started-with-wcf-data-services.md)
- [概要](wcf-data-services-overview.md)
