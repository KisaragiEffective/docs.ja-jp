---
title: 探索検索と FindCriteria
ms.date: 03/30/2017
ms.assetid: 99016fa4-1778-495b-b4cc-0e22fbec42c6
ms.openlocfilehash: da4c3c4a1d765e4f91b03f4f8fc1a73c3fea1535
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964845"
---
# <a name="discovery-find-and-findcriteria"></a>探索検索と FindCriteria

探索検索操作は、1 つ以上のサービスを探索するためにクライアントによって開始される操作であり、探索における主要なアクションの 1 つです。 検索を実行すると、WS-Discovery Probe メッセージがネットワークを介して送信されます。 指定された条件に一致するサービスは、WS-Discovery ProbeMatch メッセージを使用して応答します。 探索メッセージの詳細については、「 [ws-management 仕様](http://schemas.xmlsoap.org/ws/2004/10/discovery/ws-discovery.pdf)」を参照してください。

## <a name="discoveryclient"></a>DiscoveryClient

<xref:System.ServiceModel.Discovery.DiscoveryClient> クラスは、検索操作を実行するメカニズムを提供し、探索クライアントの操作を簡単に実行できるようにします。 このクラスには、(ブロックする) 同期検索を実行する <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A> メソッドと、ブロックしない非同期検索を実行する <xref:System.ServiceModel.Discovery.DiscoveryClient.FindAsync%2A> メソッドが含まれます。 どちらのメソッドも <xref:System.ServiceModel.Discovery.FindCriteria> パラメーターを使用し、<xref:System.ServiceModel.Discovery.FindResponse> オブジェクトを介してユーザーに結果を提供します。

## <a name="findcriteria"></a>FindCriteria

<xref:System.ServiceModel.Discovery.FindCriteria> にはいくつかのプロパティがあり、検索対象のサービスを指定する検索条件と、検索を続行する期間を指定する検索終了条件に分類できます。 <xref:System.ServiceModel.Discovery.FindCriteria> には、複数の検索条件を指定できます。 既定では、サービスがすべての条件に一致する必要があり、そうでない場合は、サービスがそれ自体を一致サービスと見なしません。 条件の一部にのみ一致するサービスを検索する場合は、サービスにカスタムの検索ロジックを実装するか、複数のクエリを使用します。

検索条件は、次のとおりです。

- <xref:System.ServiceModel.Discovery.Configuration.ContractTypeNameElement> - 省略できます。 検索対象のサービスのコントラクト名、およびサービスの検索に通常使用される条件を指定します。 複数のコントラクト名が指定されると、すべてのコントラクトに一致するサービス エンドポイントのみが応答します。 WCF では、エンドポイントは1つのコントラクトしかサポートできないことに注意してください。

- <xref:System.ServiceModel.Discovery.Configuration.ScopeElement> - 省略できます。 Scopes は、個々のサービス エンドポイントの分類に使用される絶対 URI です。 複数のエンドポイントが同じコントラクトを公開し、これらのエンドポイントのサブセットを検索する手段が必要な場合は、これを使用できます。 複数のスコープが指定されると、すべてのスコープに一致するサービス エンドポイントのみが応答します。

- <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchBy%2A> - Probe メッセージのスコープとエンドポイントのスコープとの一致の判定に使用する、一致アルゴリズムを指定します。 サポートされているスコープ一致規則は、次の 5 つです。

  - <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByExact?displayProperty=nameWithType>: 大文字と小文字が区別される基本の文字列比較を実行します。

  - "/" で区切られたセグメントで一致を <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByPrefix?displayProperty=nameWithType> します。 `http://contoso/building1` の検索は、スコープ `http://contoso/building/floor1`を持つサービスと一致します。 最後の2つのセグメントが一致しないため、`http://contoso/building100` と一致しないことに注意してください。

  - <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByLdap?displayProperty=nameWithType>: LDAP URL を使用してセグメント単位でスコープの一致を判定します。

  - <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByUuid?displayProperty=nameWithType>: UUID 文字列を使用して、スコープが完全に一致するかどうかを判定します。

  - <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByNone?displayProperty=nameWithType>: スコープを指定していないサービスのみを対象に一致を判定します。

  スコープ一致規則が指定されていない場合は、<xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByPrefix> が使用されます。

終了条件は次のとおりです。

1. <xref:System.ServiceModel.Discovery.FindCriteria.Duration%2A> - ネットワーク上でサービスからの応答を待機する最長時間。 既定の時間は 20 秒です。

2. <xref:System.ServiceModel.Discovery.FindCriteria.MaxResults%2A> - 待機する応答の最大件数。 <xref:System.ServiceModel.Discovery.FindCriteria.MaxResults%2A> が経過する前に <xref:System.ServiceModel.Discovery.FindCriteria.Duration%2A> 応答が受信された場合は、検出操作が終了します。

## <a name="findresponse"></a>FindResponse

<xref:System.ServiceModel.Discovery.FindResponse> には、ネットワーク上で一致するサービスから送信された応答を保持する <xref:System.ServiceModel.Discovery.FindResponse.Endpoints%2A> コレクション プロパティがあります。 応答したサービスがない場合、このコレクションは空です。 1 つ以上のサービスが応答した場合、各応答は <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata> オブジェクトに格納されます。これには、アドレスやコントラクトなど、サービスについての追加情報が含まれます。

次の例は、コードで検索操作を実行する方法を示しています。

```csharp
// Create DiscoveryClient
DiscoveryClient discoveryClient = new DiscoveryClient(new UdpDiscoveryEndpoint());

// Create FindCriteria
FindCriteria findCriteria = new FindCriteria(typeof(IPrinterService));
findCriteria.Scopes.Add(new Uri("http://www.contoso.com/building1/floor1"));
findCriteria.Duration = TimeSpan.FromSeconds(10);

// Find ICalculatorService endpoints
FindResponse findResponse = discoveryClient.Find(findCriteria);

Console.WriteLine("Found {0} ICalculatorService endpoint(s).", findResponse.Endpoints.Count)
```

## <a name="see-also"></a>関連項目

- [WCF Discovery の概要](../../../../docs/framework/wcf/feature-details/wcf-discovery-overview.md)
- [探索クライアント チャネルの使用](../../../../docs/framework/wcf/feature-details/using-the-discovery-client-channel.md)
- [スコープを使用した探索](../../../../docs/framework/wcf/samples/discovery-with-scopes-sample.md)
- [基本](../../../../docs/framework/wcf/samples/basic-sample.md)
