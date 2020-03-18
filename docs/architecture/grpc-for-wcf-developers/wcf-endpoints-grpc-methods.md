---
title: WCF エンドポイントと gRPC メソッド-WCF 開発者向け gRPC
description: ServiceContract および OperationContract 属性で宣言された WCF エンドポイントと、Protobuf で宣言されている gRPC メソッドの比較
ms.date: 09/02/2019
ms.openlocfilehash: 1bc6ecbc73bfc0a58393e4c28672b897ed6f2f15
ms.sourcegitcommit: f38e527623883b92010cf4760246203073e12898
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77503428"
---
# <a name="wcf-endpoints-and-grpc-methods"></a>WCF エンドポイントと gRPC メソッド

Windows Communication Foundation (WCF) では、アプリケーションコードを記述するときに、次のいずれかの方法を使用します。

- クラスにアプリケーションコードを記述し、 [Operationcontract](xref:System.ServiceModel.OperationContractAttribute)属性を使用してメソッドを装飾します。
- サービスのインターフェイスを宣言し、 [Operationcontract](xref:System.ServiceModel.OperationContractAttribute)属性をインターフェイスに追加します。

たとえば、WCF の `greet.proto` Greeter service に相当するものは、次のように記述できます。

```csharp
[ServiceContract]
public interface IGreeterService
{
    [OperationContract]
    string SayHello(string name);
}
```

第3章では、Protobuf メッセージ定義を使用してデータクラスを生成する方法を示しました。 サービスとメソッドの宣言は、サービスを実装するために継承する基本クラスを生成するために使用されます。 `.proto` ファイルに実装するメソッドを宣言するだけで、コンパイラは、オーバーライドする必要がある仮想メソッドを含む基本クラスを生成します。

## <a name="operationcontract-properties"></a>OperationContract プロパティ

[Operationcontract](xref:System.ServiceModel.OperationContractAttribute)属性には、その動作を制御または調整するためのプロパティがあります。 gRPC メソッドでは、この種のコントロールは提供されません。 次の表は、これらの `OperationContract` のプロパティを示し、gRPC で処理される機能 (または) を示しています。

| `OperationContract` プロパティ | gRPC                                             |
| ---------------------------- | ------------------------------------------------ |
| <xref:System.ServiceModel.OperationContractAttribute.Action>             | URI は、操作を識別します。 gRPC は、`.proto` ファイルの `package`、`service`、および `rpc` の名前を使用します。 |
| <xref:System.ServiceModel.OperationContractAttribute.AsyncPattern>       | すべての gRPC サービスメソッドは `Task` オブジェクトを返します。 |
| <xref:System.ServiceModel.OperationContractAttribute.IsInitiating>       | この表の後にある段落を参照してください。 |
| <xref:System.ServiceModel.OperationContractAttribute.IsOneWay>           | 一方向の gRPC メソッドは、`Empty` の結果を返すか、クライアントストリーミングを使用します。 |
| <xref:System.ServiceModel.OperationContractAttribute.IsTerminating>      | この表の後にある段落を参照してください。 |
| <xref:System.ServiceModel.OperationContractAttribute.Name>               | このプロパティは SOAP に関連し、gRPC では意味がありません。 |
| <xref:System.ServiceModel.OperationContractAttribute.ProtectionLevel>    | メッセージ暗号化はありません。 ネットワーク暗号化は、トランスポート層 (TLS over HTTP/2) で処理されます。 |
| <xref:System.ServiceModel.OperationContractAttribute.ReplyAction>        | このプロパティは SOAP に関連し、gRPC では意味がありません。 |

`IsInitiating` プロパティを使用すると、 [ServiceContract](xref:System.ServiceModel.ServiceContractAttribute)内のメソッドを、セッションの一部として呼び出される最初のメソッドにすることができないことを示すことができます。 `IsTerminating` プロパティを使用すると、操作が呼び出された後 (または、コールバッククライアントでプロパティが使用されている場合はクライアント)、サーバーによってセッションが閉じられます。 GRPC では、1つのメソッドによってストリームが作成され、明示的に閉じられます。 「 [Grpc streaming](rpc-types.md#grpc-streaming)」を参照してください。

GRPC のセキュリティと暗号化の詳細については、[第6章](security.md)を参照してください。

>[!div class="step-by-step"]
>[前へ](wcf-services-to-grpc-comparison.md)
>[次へ](wcf-bindings.md)
