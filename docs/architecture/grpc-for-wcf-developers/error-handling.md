---
title: エラー処理-WCF 開発者向け gRPC
description: GRPC でのエラー処理に関するトピック。 最も一般的に使用されるステータスコードの表が含まれています。
ms.date: 09/02/2019
ms.openlocfilehash: c380c651f854adc97e8b2ead36d30c3b83662aac
ms.sourcegitcommit: 771c554c84ba38cbd4ac0578324ec4cfc979cf2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2020
ms.locfileid: "77542794"
---
# <a name="error-handling"></a>エラー処理

Windows Communication Foundation (WCF) は <xref:System.ServiceModel.FaultException%601> と[Faultcontract](xref:System.ServiceModel.FaultContractAttribute)を使用して、SOAP Fault standard のサポートなど、詳細なエラー情報を提供します。

残念ながら、現在のバージョンの gRPC には、WCF での洗練された機能がありません。単純なステータスコードとメタデータに基づいて組み込まれたエラー処理は限られています。 次の表に、最も一般的に使用されるステータスコードの簡単なガイドを示します。

| status code | 問題 |
| ----------- | ------- |
| `GRPC_STATUS_UNIMPLEMENTED` | メソッドが書き込まれていません。 |
| `GRPC_STATUS_UNAVAILABLE` | サービス全体に問題があります。 |
| `GRPC_STATUS_UNKNOWN` | 無効な応答です。 |
| `GRPC_STATUS_INTERNAL` | エンコード/デコードに問題があります。 |
| `GRPC_STATUS_UNAUTHENTICATED` | 認証に失敗しました。 |
| `GRPC_STATUS_PERMISSION_DENIED` | 承認に失敗しました。 |
| `GRPC_STATUS_CANCELLED` | 呼び出しはキャンセルされました。通常は呼び出し元によって行われます。 |

## <a name="raise-errors-in-aspnet-core-grpc"></a>ASP.NET Core gRPC でエラーが発生する

ASP.NET Core gRPC サービスは、`RpcException`をスローすることによってエラー応答を送信することができます。これは、同じプロセスにあるかのようにクライアントからキャッチできます。 `RpcException` には、状態コードと説明を含める必要があります。また、必要に応じて、メタデータと長い例外メッセージを含めることができます。 メタデータを使用して、サポートデータを送信することができます。これは、`FaultContract` オブジェクトが WCF エラーに対して追加データを伝達する方法と似ています。

```csharp
public async Task<GetPortfolioResponse> GetPortfolio(GetPortfolioRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;
    if (!ValidateUser(user))
    {
        var metadata = new Metadata
        {
            { "User", user.Identity.Name }
        };
        throw new RpcException(new Status(StatusCode.PermissionDenied, "Permission denied"), metadata);
    }
}
```

## <a name="catch-errors-in-grpc-clients"></a>GRPC クライアントでエラーをキャッチする

WCF クライアントが <xref:System.ServiceModel.FaultException%601> エラーをキャッチできるのと同様に、gRPC クライアントは、エラーを処理するための `RpcException` をキャッチできます。 `RpcException` はジェネリック型ではないため、異なるブロックでさまざまなエラーの種類をキャッチすることはできません。 ただし、次のC#例に示すように、の*例外フィルター*機能を使用して、状態コードごとに個別の `catch` ブロックを宣言できます。

```csharp
try
{
    var portfolio = await client.GetPortfolioAsync(new GetPortfolioRequest { Id = id });
}
catch (RpcException ex) when (ex.StatusCode == StatusCode.PermissionDenied)
{
    var userEntry = ex.Trailers.FirstOrDefault(e => e.Key == "User");
    Console.WriteLine($"User '{userEntry.Value}' does not have permission to view this portfolio.");
}
catch (RpcException)
{
    // Handle any other error type ...
}
```

> [!IMPORTANT]
> エラーのために追加のメタデータを指定する場合は、API のドキュメント、または `.proto` ファイルのコメントで、関連するキーと値を必ず文書化してください。

## <a name="grpc-richer-error-model"></a>gRPC の豊富なエラーモデル

Google は、WCF の[Faultcontract](xref:System.ServiceModel.FaultContractAttribute)のようなより[豊富なエラーモデル](https://cloud.google.com/apis/design/errors#error_model)を開発しましたが、 C#このモデルはまだではサポートされていません。 現在、このファイルは、ゴー、Java、Python、およびC++でのみ使用できます。

>[!div class="step-by-step"]
>[前へ](metadata.md)
>[次へ](ws-protocols.md)
