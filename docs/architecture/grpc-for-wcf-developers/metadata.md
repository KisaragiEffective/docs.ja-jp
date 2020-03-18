---
title: WCF 開発者向けのメタデータ-gRPC
description: GRPC でメタデータを使用して、クライアントとサーバーの間に追加のコンテキストを渡す方法。
ms.date: 09/02/2019
ms.openlocfilehash: 64fa94d1e63af480cbc7363631de161c5b8b8fb8
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628580"
---
# <a name="metadata"></a>メタデータ

*メタデータ*とは、要求と応答の処理中に役立つ可能性のある追加データを指しますが、実際のアプリケーションデータには含まれません。 メタデータには、認証トークン、監視用の要求 id とタグ、データセット内のレコードの数などのデータに関する情報が含まれます。

<xref:System.ServiceModel.OperationContextScope> と <xref:System.ServiceModel.OperationContext.OutgoingMessageHeaders?displayProperty=nameWithType> プロパティを使用して Windows Communication Foundation (WCF) メッセージに汎用キー/値ヘッダーを追加し、<xref:System.ServiceModel.Channels.MessageProperties>を使用して処理することができます。

gRPC の呼び出しと応答には、HTTP ヘッダーに似たメタデータを含めることもできます。 このメタデータは、ほとんどの場合 gRPC 自体には見えず、アプリケーションコードまたはミドルウェアによって処理されるために渡されます。 メタデータは、キーと値のペアとして表されます。キーは文字列で、値は文字列またはバイナリデータのいずれかになります。 `.proto` ファイルでメタデータを指定する必要はありません。

メタデータは、 [Grpc. Core. Api](https://www.nuget.org/packages/Grpc.Core.Api/) NuGet パッケージの `Metadata` クラスによって処理されます。 このクラスは、コレクション初期化子構文と共に使用できます。

この例でC#は、クライアントからの呼び出しにメタデータを追加する方法を示します。

```csharp
var metadata = new Metadata
{
    { "Requester", _clientName }
};

var request = new GetPortfolioRequest
{
    Id = portfolioId
};

var response = await client.GetPortfolioAsync(request, metadata);
```

gRPC サービスは、`ServerCallContext` 引数の `RequestHeaders` プロパティからメタデータにアクセスできます。

```csharp
public async Task<GetPortfolioResponse> GetPortfolio(GetPortfolioRequest request, ServerCallContext context)
{
    var requesterHeader = context.RequestHeaders.FirstOrDefault(e => e.Key == "Requester");
    if (requesterHeader != null)
    {
        _logger.LogInformation($"Request from {requesterHeader.Value}");
    }
    // ...
}
```

サービスは、`ServerCallContext`の `ResponseTrailers` プロパティを使用して、クライアントにメタデータを送信できます。

```csharp
public async Task<GetPortfolioResponse> GetPortfolio(GetPortfolioRequest request, ServerCallContext context)
{
    // ...
    context.ResponseTrailers.Add("Responder", _serverName);
    // ...
}
```

>[!div class="step-by-step"]
>[前へ](rpc-types.md)
>[次へ](error-handling.md)
