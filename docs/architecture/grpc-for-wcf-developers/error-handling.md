---
title: エラー処理 - WCF 開発者向け gRPC
description: gRPC でのエラー処理に関するトピック。 最も一般的に使用される状態コードの表が含まれます。
ms.date: 09/02/2019
ms.openlocfilehash: 64a2355a8bd608c074f9bc420312b23aba0c1fb2
ms.sourcegitcommit: e3cbf26d67f7e9286c7108a2752804050762d02d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/09/2020
ms.locfileid: "80988948"
---
# <a name="error-handling"></a>エラー処理

Windows Communication Foundation (WCF) では、SOAP エラー標準のサポートなど、詳細なエラー情報を提供するため、<xref:System.ServiceModel.FaultException%601> と [FaultContract](xref:System.ServiceModel.FaultContractAttribute) が使用されます。

残念ながら、現在のバージョンの gRPC には、WCF での洗練された機能はありません。簡単な状態コードとメタデータに基づく、組み込みのエラー処理だけに限られています。 最も一般的に使用される状態コードの簡単なガイドを次の表に示します。

| 状態コード | 問題 |
| ----------- | ------- |
| `GRPC_STATUS_UNIMPLEMENTED` | メソッドが書き込まれていません。 |
| `GRPC_STATUS_UNAVAILABLE` | サービス全体に問題があります。 |
| `GRPC_STATUS_UNKNOWN` | 無効な応答です。 |
| `GRPC_STATUS_INTERNAL` | エンコードまたはデコードに問題があります。 |
| `GRPC_STATUS_UNAUTHENTICATED` | 認証に失敗しました。 |
| `GRPC_STATUS_PERMISSION_DENIED` | 承認に失敗しました。 |
| `GRPC_STATUS_CANCELLED` | 呼び出しは、通常は呼び出し元によって、キャンセルされました。 |

## <a name="raise-errors-in-aspnet-core-grpc"></a>ASP.NET Core gRPC でエラーを生成する

ASP.NET Core gRPC サービスからは、`RpcException` をスローすることによってエラー応答を送信できます。これは、同じプロセス内であるかのようにクライアントでキャッチできます。 `RpcException` には、状態コードと説明を含める必要があり、必要に応じて、メタデータと長い例外メッセージを含めることができます。 メタデータを使用すると、`FaultContract` オブジェクトで WCF エラーの追加データを伝達できるのと同様に、サポート データを送信できます。

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

## <a name="catch-errors-in-grpc-clients"></a>gRPC クライアントでエラーをキャッチする

WCF クライアントで <xref:System.ServiceModel.FaultException%601> エラーをキャッチできるのと同様に、gRPC クライアントで `RpcException` をキャッチしてエラーを処理できます。 `RpcException` はジェネリック型ではないため、異なるブロックで異なるエラーの種類をキャッチすることはできません。 ただし、次の例に示すように、C# の "*例外フィルター*" 機能を使用して、異なる状態コードごとに個別の `catch` ブロックを宣言できます。

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
> エラーに追加のメタデータを指定する場合は、API のドキュメントまたは `.proto` ファイル内のコメントで、関連するキーと値を必ず文書化してください。

## <a name="grpc-richer-error-model"></a>gRPC の豊富なエラー モデル

Google により、WCF の[FaultContract](xref:System.ServiceModel.FaultContractAttribute) に近い、[より充実したエラー モデル](https://cloud.google.com/apis/design/errors#error_model)が開発されていますが、このモデルは C# ではまだサポートされていません。 現在、それは Go、Java、Python、C++ でのみ使用できます。

>[!div class="step-by-step"]
>[前へ](metadata.md)
>[次へ](ws-protocols.md)
