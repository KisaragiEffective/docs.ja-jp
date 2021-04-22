---
title: メタデータ - WCF 開発者向け gRPC
description: gRPC でメタデータを使用して、クライアントとサーバーの間で追加のコンテキストを渡す方法。
ms.date: 09/02/2019
ms.openlocfilehash: 64fa94d1e63af480cbc7363631de161c5b8b8fb8
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628580"
---
# <a name="metadata"></a>Metadata

"*メタデータ*" とは、要求と応答の処理の間に役立つ可能性がありますが、実際のアプリケーション データには含まれない、追加データのことです。 メタデータには、認証トークン、監視用の要求 ID とタグ、データセット内のレコード数などのデータに関する情報などがあります。

<xref:System.ServiceModel.OperationContextScope> および <xref:System.ServiceModel.OperationContext.OutgoingMessageHeaders?displayProperty=nameWithType> プロパティを使用して Windows Communication Foundation (WCF) メッセージに汎用のキーと値のヘッダーを追加し、<xref:System.ServiceModel.Channels.MessageProperties> を使用してそれらを処理することができます。

gRPC の呼び出しと応答には、HTTP ヘッダーに似たメタデータを含めることもできます。 このメタデータは、gRPC 自体からはほとんど認識されず、アプリケーション コードまたはミドルウェアによって処理されるために渡されます。 メタデータは、キーと値のペアとして表され、キーは文字列、値は文字列またはバイナリ データのいずれかになります。 `.proto` ファイルでメタデータを指定する必要はありません。

メタデータは、[Grpc.Core.Api](https://www.nuget.org/packages/Grpc.Core.Api/) NuGet パッケージの `Metadata` クラスによって処理されます。 このクラスは、コレクション初期化子構文で使用できます。

この例では、C# クライアントからの呼び出しにメタデータを追加する方法を示します。

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

gRPC サービスでは、`ServerCallContext` 引数の `RequestHeaders` プロパティからメタデータにアクセスできます。

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

`ServerCallContext` の `ResponseTrailers` プロパティを使用することにより、サービスでクライアントにメタデータを送信できます。

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
