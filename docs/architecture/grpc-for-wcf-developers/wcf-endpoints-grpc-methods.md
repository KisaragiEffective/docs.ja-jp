---
title: WCF エンドポイントと gRPC メソッド - WCF 開発者向け gRPC
description: ServiceContract および OperationContract 属性で宣言された WCF エンドポイントと、Protobuf で宣言された gRPC メソッドの比較
ms.date: 09/02/2019
ms.openlocfilehash: 1bc6ecbc73bfc0a58393e4c28672b897ed6f2f15
ms.sourcegitcommit: f38e527623883b92010cf4760246203073e12898
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77503428"
---
# <a name="wcf-endpoints-and-grpc-methods"></a>WCF エンドポイントと gRPC メソッド

Windows Communication Foundation (WCF) では、アプリケーションのコードを記述するときに、次のいずれかの方法を使用します。

- クラスでアプリケーション コードを記述し、[OperationContract](xref:System.ServiceModel.OperationContractAttribute) 属性を使用してメソッドを装飾します。
- サービスのインターフェイスを宣言し、[OperationContract](xref:System.ServiceModel.OperationContractAttribute) 属性をインターフェイスに追加します。

たとえば、`greet.proto` Greeter サービスに相当する WCF は、次のように記述できます。

```csharp
[ServiceContract]
public interface IGreeterService
{
    [OperationContract]
    string SayHello(string name);
}
```

第 3 章では、Protobuf メッセージ定義を使用してデータ クラスを生成する方法を示しました。 サービスを実装するための継承元である基底クラスを生成するには、サービスとメソッドの宣言を使用します。 実装するメソッドを `.proto` ファイルで宣言するだけで、コンパイラにより、オーバーライドする必要がある仮想メソッドが含まれる基底クラスが生成されます。

## <a name="operationcontract-properties"></a>OperationContract のプロパティ

[OperationContract](xref:System.ServiceModel.OperationContractAttribute) 属性には、その動作を制御または調整するためのプロパティがあります。 gRPC のメソッドでは、この種の制御は提供されていません。 次の表では、`OperationContract` のプロパティの一覧を示し、それらによって指定される機能が gRPC でどのように処理されるか (または処理されないか) を示します。

| `OperationContract` プロパティ | gRPC                                             |
| ---------------------------- | ------------------------------------------------ |
| <xref:System.ServiceModel.OperationContractAttribute.Action>             | URI で操作を識別します。 gRPC では、`.proto` ファイルの `package`、`service`、`rpc` の名前が使用されます。 |
| <xref:System.ServiceModel.OperationContractAttribute.AsyncPattern>       | gRPC サービスのすべてのメソッドからは、`Task` オブジェクトが返されます。 |
| <xref:System.ServiceModel.OperationContractAttribute.IsInitiating>       | この表の後の段落を参照してください。 |
| <xref:System.ServiceModel.OperationContractAttribute.IsOneWay>           | 一方向の gRPC メソッドでは、`Empty` の結果が返されるか、クライアント ストリーミングが使用されます。 |
| <xref:System.ServiceModel.OperationContractAttribute.IsTerminating>      | この表の後の段落を参照してください。 |
| <xref:System.ServiceModel.OperationContractAttribute.Name>               | このプロパティは SOAP に関連するものであり、gRPC では意味がありません。 |
| <xref:System.ServiceModel.OperationContractAttribute.ProtectionLevel>    | メッセージの暗号化はありません。 ネットワークの暗号化は、トランスポート層 (TLS over HTTP/2) で処理されます。 |
| <xref:System.ServiceModel.OperationContractAttribute.ReplyAction>        | このプロパティは SOAP に関連するものであり、gRPC では意味がありません。 |

`IsInitiating` プロパティを使用すると、[ServiceContract](xref:System.ServiceModel.ServiceContractAttribute) 内のメソッドをセッションの一部として呼び出される最初のメソッドにはできないことを、示すことができます。 `IsTerminating` プロパティを使用すると、操作が呼び出された後でサーバーによりセッションが終了されます (または、コールバック クライアントでプロパティが使用されている場合はクライアント)。 gRPC では、ストリームは 1 つのメソッドによって作成され、明示的に閉じられます。 [gRPC ストリーミング](rpc-types.md#grpc-streaming)に関する記事を参照してください。

gRPC のセキュリティと暗号化の詳細については、[第 6 章](security.md)を参照してください。

>[!div class="step-by-step"]
>[前へ](wcf-services-to-grpc-comparison.md)
>[次へ](wcf-bindings.md)
