---
title: '方法 : WAS で WCF サービスをホストする'
ms.date: 03/30/2017
ms.assetid: 9e3e213e-2dce-4f98-81a3-f62f44caeb54
ms.openlocfilehash: 9945e398bbd33776cce808b44388a4415da297a1
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964768"
---
# <a name="how-to-host-a-wcf-service-in-was"></a>方法 : WAS で WCF サービスをホストする
このトピックでは、Windows プロセスアクティブ化サービス (WAS) でホストされる Windows Communication Foundation (WCF) サービスを作成するために必要な基本的な手順について説明します。 WAS は、HTTP 以外のトランスポート プロトコルで動作するインターネット インフォメーション サービス (IIS) 機能を一般化した新しいプロセス アクティブ化サービスです。 WCF では、リスナーアダプターインターフェイスを使用して、WCF でサポートされている HTTP 以外のプロトコル (TCP、名前付きパイプ、メッセージキューなど) を介して受信されるアクティブ化要求を伝達します。  
  
 このホスト オプションでは、WAS アクティブ化コンポーネントのインストールと構成が正しく行われている必要がありますが、アプリケーションの一部としてホスト コードを記述する必要はありません。 WAS のインストールと構成の詳細については、「[方法: WCF アクティブ化コンポーネントをインストールおよび構成](../../../../docs/framework/wcf/feature-details/how-to-install-and-configure-wcf-activation-components.md)する」を参照してください。  
  
> [!WARNING]
> Web サーバーの要求処理パイプラインをクラシック モードに設定すると、WAS のアクティブ化がサポートされません。 WAS のアクティブ化を使用する場合は、Web サーバーの要求処理パイプラインを統合モードに設定する必要があります。  
  
 WCF サービスが WAS でホストされている場合、標準のバインドが通常の方法で使用されます。 ただし、WAS でホストされるサービスを <xref:System.ServiceModel.NetTcpBinding> や <xref:System.ServiceModel.NetNamedPipeBinding> を使用して構成する場合は、制限を遵守する必要があります。 つまり、異なるエンドポイントが同じトランスポートを使用する場合、次の 7 つのプロパティでバインディング設定が一致する必要があります。  
  
- ConnectionBufferSize  
  
- ChannelInitializationTimeout  
  
- MaxPendingConnections  
  
- MaxOutputDelay  
  
- MaxPendingAccepts  
  
- ConnectionPoolSettings.IdleTimeout  
  
- ConnectionPoolSettings.MaxOutboundConnectionsPerEndpoint  
  
 バインディングの設定が一致しない場合、これらのプロパティの値は常に最初に初期化されたエンドポイントによって決定されるため、後で追加されるエンドポイントは <xref:System.ServiceModel.ServiceActivationException> をスローします。  
  
 この例のソースコピーについては、「 [TCP Activation](../../../../docs/framework/wcf/samples/tcp-activation.md)」を参照してください。  
  
### <a name="to-create-a-basic-service-hosted-by-was"></a>WAS でホストされる基本サービスを作成するには  
  
1. サービスの種類にサービス コントラクトを定義します。  
  
     [!code-csharp[C_HowTo_HostInWAS#1121](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/cs/service.cs#1121)]  
  
2. サービス クラスにサービス コントラクトを実装します。 アドレス情報とバインディング情報はサービスの実装内では指定されないことに注意してください。 同様に、コードは構成ファイルから情報を取得する必要はありません。  
  
     [!code-csharp[C_HowTo_HostInWAS#1122](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/cs/service.cs#1122)]  
  
3. <xref:System.ServiceModel.NetTcpBinding> エンドポイントによって使用される `CalculatorService` バインディングを定義する Web.config ファイルを作成します。  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <configuration>  
      <system.serviceModel>  
        <bindings>  
          <netTcpBinding>  
            <binding portSharingEnabled="true">  
              <security mode="None" />  
            </binding>  
          </netTcpBinding>  
        </bindings>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
4. 次のコードを含む Service.svc ファイルを作成します。  
  
   ```
   <%@ServiceHost language=c# Service="CalculatorService" %>
   ```
  
5. IIS 仮想ディレクトリに Service.svc ファイルを配置します。  
  
### <a name="to-create-a-client-to-use-the-service"></a>サービスを使用するクライアントを作成するには  
  
1. コマンドラインから[ServiceModel メタデータユーティリティツール (svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)を使用して、サービスメタデータからコードを生成します。  
  
    ```console
    Svcutil.exe <service's Metadata Exchange (MEX) address or HTTP GET address>
    ```  
  
2. 生成されたクライアントには、クライアントの実装時に満たされなければならないサービス コントラクトを定義する `ICalculator` インターフェイスが含まれます。  
  
     [!code-csharp[C_HowTo_HostInWAS#1221](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/cs/client.cs#1221)]  
  
3. 生成されたクライアント アプリケーションは `ClientCalculator` も実装します。 このサービスの実装では、アドレス情報とバインディング情報が指定されないことに注意してください。 同様に、コードは構成ファイルから情報を取得する必要はありません。  
  
     [!code-csharp[C_HowTo_HostInWAS#1222](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/cs/client.cs#1222)]  
  
4. <xref:System.ServiceModel.NetTcpBinding> を使用するクライアントの構成は、Svcutil.exe で生成することもできます。 Visual Studio を使用する場合は、このファイルの名前は App.config ファイル内で指定する必要があります。  
  
     [!code-xml[C_HowTo_HostInWAS#2211](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/common/app.config#2211)]   
  
5. アプリケーションで `ClientCalculator` のインスタンスを作成し、サービス操作を呼び出します。  
  
     [!code-csharp[C_HowTo_HostInWAS#1223](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/cs/client.cs#1223)]  
  
6. クライアントをコンパイルして実行します。  
  
## <a name="see-also"></a>関連項目

- [TCP アクティベーション](../../../../docs/framework/wcf/samples/tcp-activation.md)
- [AppFabric のホスティング機能](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
