---
title: '方法 : 信頼されたセッション内のメッセージを変換する'
ms.date: 03/30/2017
ms.assetid: 87cd0e75-dd2c-44c1-8da0-7b494bbdeaea
ms.openlocfilehash: 58a392fc6295e82f41e08c80a3343b4059afad7e
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74141680"
---
# <a name="how-to-exchange-messages-within-a-reliable-session"></a>方法 : 信頼されたセッション内のメッセージを変換する

このトピックでは、信頼できるセッションを有効にするために必要な手順について説明します。ここでは、信頼できるセッションを (既定ではなく) オプションでサポートするシステム指定のバインディングを使用します。 信頼できるセッションは、コードまたは構成ファイルで宣言によって強制的に有効にすることができます。 この手順では、クライアントとサービスの構成ファイルを使用して、信頼できるセッションを有効にし、メッセージが送信された順序と同じ順序で到着することを指定します。

この手順の重要な部分は、エンドポイント構成要素に、`Binding1`という名前のバインディング構成を参照する `bindingConfiguration` 属性が含まれていることです。 [ **\<binding >** ](../../configure-apps/file-schema/wcf/bindings.md) configuration 要素は、 [ **\<reliableSession >** ](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731302(v=vs.100))要素の `enabled` 属性を `true`に設定することによって、この名前を参照して信頼できるセッションを有効にします。 信頼できるセッションで順序付き配信の保証を指定するには、`ordered` 属性を `true` に設定します。

この例のソースコピーについては、「 [WS Reliable Session](../../../../docs/framework/wcf/samples/ws-reliable-session.md)」を参照してください。

### <a name="configure-the-service-with-a-wshttpbinding-to-use-a-reliable-session"></a>信頼できるセッションを使用するようにサービスを WSHttpBinding で構成する

1. サービスの種類にサービス コントラクトを定義します。

   [!code-csharp[c_HowTo_UseReliableSession#1121](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/cs/service.cs#1121)]

1. サービス クラスにサービス コントラクトを実装します。 サービスの実装内では、アドレスまたはバインド情報が指定されていないことに注意してください。 構成ファイルからアドレスやバインド情報を取得するためのコードを記述する必要はありません。

   [!code-csharp[c_HowTo_UseReliableSession#1122](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/cs/service.cs#1122)]

1. *Web.config ファイルを*作成して、信頼できるセッションを有効にし、必要なメッセージを順次配信する <xref:System.ServiceModel.WSHttpBinding> を使用する `CalculatorService` のエンドポイントを構成します。

   [!code-xml[c_HowTo_UseReliableSession#2111](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/common/web.config#2111)]

1. 次の行を含む .svc ファイルを作成*し*ます。

   ```
   <%@ServiceHost language=c# Service="CalculatorService" %>
   ```

1. サービスの *.svc*ファイルをインターネットインフォメーションサービス (IIS) 仮想ディレクトリに配置します。

### <a name="configure-the-client-with-a-wshttpbinding-to-use-a-reliable-session"></a>信頼できるセッションを使用するようにクライアントを WSHttpBinding で構成する

1. コマンドラインから[ServiceModel メタデータユーティリティツール (*svcutil.exe*)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)を使用して、サービスメタデータからコードを生成します。

   ```console
   Svcutil.exe <service's Metadata Exchange (MEX) address or HTTP GET address>
   ```

1. 生成されたクライアントには、クライアント実装が満たす必要のあるサービスコントラクトを定義する `ICalculator` インターフェイスが含まれています。

   [!code-csharp[C_HowTo_UseReliableSession#1221](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/cs/client.cs#1221)]

1. 生成されたクライアント アプリケーションは `ClientCalculator` も実装します。 サービスの実装内でアドレスおよびバインド情報が指定されていないことに注意してください。 構成ファイルからアドレスやバインド情報を取得するためのコードを記述する必要はありません。

   [!code-csharp[C_HowTo_UseReliableSession#1222](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/cs/client.cs#1222)]

1. *Svcutil.exe*は、<xref:System.ServiceModel.WSHttpBinding> クラスを使用するクライアントの構成も生成します。 Visual Studio を使用*する場合は*、構成ファイルに app.config という名前を指定します。

   [!code-xml[C_HowTo_UseReliableSession#2211](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/common/app.config#2211)]

1. アプリケーションで `ClientCalculator` のインスタンスを作成し、サービス操作を呼び出します。

   [!code-csharp[C_HowTo_UseReliableSession#1223](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/cs/client.cs#1223)]

1. クライアントをコンパイルして実行します。

## <a name="example"></a>例

システム指定のバインディングの中には、信頼できるセッションを既定でサポートするものがあります。 次の設定があります。

- <xref:System.ServiceModel.WSDualHttpBinding>

- <xref:System.ServiceModel.NetNamedPipeBinding>

- <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>

信頼できるセッションをサポートするカスタムバインディングを作成する方法の例については、「[方法: HTTPS を使用してカスタムの信頼できるセッションのバインディングを作成](../../../../docs/framework/wcf/feature-details/how-to-create-a-custom-reliable-session-binding-with-https.md)する」を参照してください。

## <a name="see-also"></a>関連項目

- [信頼できるセッション](../../../../docs/framework/wcf/feature-details/reliable-sessions.md)
