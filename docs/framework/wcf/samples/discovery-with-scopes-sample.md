---
title: スコープを使用した探索のサンプル
ms.date: 03/30/2017
ms.assetid: 6a37a754-6b8c-4ebe-bdf2-d4f0520271d5
ms.openlocfilehash: 8ba5618f472fc8a6e1751776060f99103a67a073
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76728755"
---
# <a name="discovery-with-scopes-sample"></a>スコープを使用した探索のサンプル

このサンプルでは、スコープを使用して、探索可能なエンドポイントを分類する方法、および <xref:System.ServiceModel.Discovery.DiscoveryClient> を使用して、エンドポイントの非同期検索を実行する方法を示します。 サービスに関しては、エンドポイント探索動作を追加し、その動作を使用してエンドポイントにスコープを追加し、さらにエンドポイントの探索可能性を制御することによって、各エンドポイントの探索をカスタマイズする方法を示します。 クライアントに関しては、<xref:System.ServiceModel.Discovery.DiscoveryClient> を作成し、<xref:System.ServiceModel.Discovery.FindCriteria> にスコープを追加してスコープが条件となるように検索パラメーターを最適に調整する方法を示します。 また、終了条件を追加することによってクライアントで応答を制限する方法も示します。

## <a name="service-features"></a>サービス機能

このプロジェクトでは、<xref:System.ServiceModel.ServiceHost> に 2 つのサービス エンドポイントを追加します。 各エンドポイントには <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior> が関連付けられます。 この動作は、両方のエンドポイントの URI スコープを追加するために使用します。 スコープは、クライアントが検索を最適に調整できるように、これらのエンドポイントを区別するために使用します。 2 番目のエンドポイントについては、<xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior.Enabled%2A> プロパティを `false` に設定して探索可能性を無効にすることができます。 これにより、このエンドポイントに関連付けられている探索メタデータが探索メッセージの一部として送信されなくなります。

## <a name="client-features"></a>クライアント機能

`FindCalculatorServiceAddress()` メソッドは、<xref:System.ServiceModel.Discovery.DiscoveryClient> を使用し、2 つの制限が設定された <xref:System.ServiceModel.Discovery.FindCriteria> を渡す方法を示します。 スコープを条件に追加し、<xref:System.ServiceModel.Discovery.FindCriteria.MaxResults%2A> プロパティを 1 に設定します。 このスコープにより、同じスコープを公開するサービスのみに結果が限定されます。 <xref:System.ServiceModel.Discovery.FindCriteria.MaxResults%2A> を 1 に設定すると、<xref:System.ServiceModel.Discovery.DiscoveryClient> が待機する応答が最大で 1 つのエンドポイントに制限されます。 <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A> の呼び出しは、タイムアウトになるかエンドポイントが 1 つ見つかるまでスレッドをブロックする同期操作です。

### <a name="to-use-this-sample"></a>このサンプルを使用するには

1. このサンプルでは HTTP エンドポイントを使用します。このサンプルを実行するには、適切な URL ACL を追加する必要があります。 詳細については、「 [HTTP および HTTPS の構成](../feature-details/configuring-http-and-https.md)」を参照してください。 管理特権で次のコマンドを実行すると、適切な ACL が追加されます。 そのままではコマンドが動作しない場合は、代わりに、お使いのドメインとユーザー名を引数に指定して実行してみてください。`netsh http add urlacl url=http://+:8000/ user=%DOMAIN%\%UserName%`

2. ソリューションをビルドします。

3. ビルド ディレクトリからサービス実行可能ファイルを実行します。

4. クライアント実行可能ファイルを実行します。 クライアントでサービスを検索できることに注意してください。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Discovery\DiscoveryWithScopes`
