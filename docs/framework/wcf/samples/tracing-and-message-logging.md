---
title: トレースとメッセージ ログ
ms.date: 03/30/2017
helpviewer_keywords:
- Tracing and logging
ms.assetid: a4f39bfc-3c5e-4d51-a312-71c5c3ce0afd
ms.openlocfilehash: 448eef6ea147b725600b774026155acc1fca6d36
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094866"
---
# <a name="tracing-and-message-logging"></a>トレースとメッセージ ログ
このサンプルでは、トレースとメッセージ ログを有効にする方法を示します。 結果のトレースとメッセージログは、[サービストレースビューアーツール (svctraceviewer.exe)](../../../../docs/framework/wcf/service-trace-viewer-tool-svctraceviewer-exe.md)を使用して表示されます。 このサンプルは、[はじめに](../../../../docs/framework/wcf/samples/getting-started-sample.md)に基づいています。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
## <a name="tracing"></a>トレース  
 Windows Communication Foundation (WCF) は、<xref:System.Diagnostics> 名前空間で定義されているトレース機構を使用します。 このトレース モデルのトレース データは、アプリケーションが実装するトレース ソースによって作成されます。 各ソースは、名前によって識別されます。 トレース コンシューマでは、情報を取得するトレース ソースのトレース リスナが作成されます。 トレース データを受け取るには、トレース ソースのリスナを作成する必要があります。 WCF では、サービスモデルのトレースソース `switchValue`を設定することによって、サービスまたはクライアントの構成ファイルに次のコードを追加することで、これを行うことができます。  
  
```xml  
<system.diagnostics>  
    <sources>  
      <source name="System.ServiceModel" switchValue="Information,ActivityTracing"  
        propagateActivity="true">  
        <listeners>  
          <add name="xml" />  
        </listeners>  
      </source>  
      <source name="System.ServiceModel.MessageLogging">  
        <listeners>  
          <add name="xml" />  
        </listeners>  
      </source>  
    </sources>  
    <sharedListeners>  
      <add initializeData="C:\logs\TracingAndLogging-service.svclog" type="System.Diagnostics.XmlWriterTraceListener"  
        name="xml" />  
    </sharedListeners>  
    <trace autoflush="true" />  
</system.diagnostics>  
```  
  
 トレースソースの詳細については、「トレースの[構成](../../../../docs/framework/wcf/diagnostics/tracing/configuring-tracing.md)」トピックの「トレースソース」セクションを参照してください。  
  
## <a name="activity-tracing-and-propagation"></a>アクティビティのトレースと伝達  
 クライアントとサービスの両方について `system.ServiceModel` トレースソースの `true` に設定されている `ActivityTracing` 有効にし `propagateActivity` すると、処理の論理単位 (アクティビティ) 内、エンドポイント内のアクティビティ (アクティビティの転送を含む)、および複数のエンドポイントにまたがるアクティビティ (アクティビティ ID の伝達を通じて) の間でトレースが関連付けられます。  
  
 3 つの機構 (アクティビティ、転送、および伝達) により、サービス トレース ビューア ツールを使用してエラーの根本原因をより迅速に見つけることができます。 詳細については、「[サービストレースビューアーを使用した相関トレースの表示」および「トラブルシューティング](../../../../docs/framework/wcf/diagnostics/tracing/using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting.md)」を参照してください。  
  
 ユーザー定義のアクティビティ トレースを作成することにより、サービス モデルによって提供されるトレースを拡張することができます。 ユーザー定義のアクティビティ トレースによって、次の操作を可能にするトレース アクティビティを作成できます。  
  
- 複数のトレースを作業の論理単位ごとにグループ化します。  
  
- 転送や伝達を利用してアクティビティを相互に関連付けます。  
  
- WCF トレース (ログファイルのディスク領域コストなど) のパフォーマンスコストを軽減します。  
  
 ユーザー定義のアクティビティトレースの詳細については、「[トレースの拡張](../../../../docs/framework/wcf/samples/extending-tracing.md)」サンプルを参照してください。  
  
## <a name="message-logging"></a>メッセージ ログ  
 メッセージログは、任意の WCF アプリケーションのクライアントとサービスの両方で有効にすることができます。 メッセージ ログを有効にするには、クライアントとサービスのどちらかに次のコードを追加する必要があります。  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <diagnostics>  
      <!-- Enable Message Logging here. -->  
      <!-- log all messages received or sent at the transport or service model levels -->  
      <messageLogging logEntireMessage="true"  
                      maxMessagesToLog="300"  
                      logMessagesAtServiceLevel="true"  
                      logMalformedMessages="true"  
                      logMessagesAtTransportLevel="true" />  
    </diagnostics>  
  </system.serviceModel>  
</configuration>  
```  
  
 メッセージが記録されるときのトレースの種類は、そのメッセージがクライアントでトレースされるか、またはサーバーでトレースされるかによって異なります。 たとえば、クライアントに送信される "Add" メッセージは、クライアントでは "TransportWrite" カテゴリの下でトレースされるのに対し、サービスでは同じメッセージが "TransportRead" カテゴリの下でトレースされます。  
  
 クライアントの App.config ファイルまたはサービスの Web.config ファイルの <xref:System.Diagnostics> セクションに次のコードを追加して、トレース リスナを構成します。  
  
```xml  
<system.diagnostics>  
    <sources>  
      <source name="System.ServiceModel" switchValue="Information,ActivityTracing"  
        propagateActivity="true">  
        <listeners>  
          <add name="xml" />  
        </listeners>  
      </source>  
      <source name="System.ServiceModel.MessageLogging">  
        <listeners>  
          <add name="xml" />  
        </listeners>  
      </source>  
    </sources>  
    <sharedListeners>  
      <add initializeData="C:\logs\TracingAndLogging-client.svclog" type="System.Diagnostics.XmlWriterTraceListener"  
        name="xml" />  
    </sharedListeners>  
    <trace autoflush="true" />  
  </system.diagnostics>  
```  
  
 メッセージは、構成ファイルで指定された対象ディレクトリ内に XML 形式で記録されます。  
  
> [!NOTE]
> 最初にログ ディレクトリが作成されていないと、トレース ファイルは作成されません。 ディレクトリ C:\logs\ が存在することを確認するか、またはリスナの構成でログ記録用の代替ディレクトリを指定します。 詳細については、このドキュメントの最後にある初期セットアップ手順を参照してください。  
  
 メッセージログの詳細については、「[メッセージログの構成](../../../../docs/framework/wcf/diagnostics/configuring-message-logging.md)」トピックを参照してください。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. トレースとメッセージ ログのサンプルを実行する前に、サービスによって .svclog ファイルが書き込まれるディレクトリ C:\logs\ を作成します。 このディレクトリ名は、トレースとメッセージがログ記録されるパスとして、構成ファイル内で定義されます。この名前は変更可能です。 ユーザー Network Service に、そのログ ディレクトリへの書き込みアクセスを与えます。  
  
3. ソリューションの .net C#エディションC++、、または Visual Basic ビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
4. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\TracingAndLogging`  
  
## <a name="see-also"></a>参照

- [追跡](../../../../docs/framework/wcf/diagnostics/tracing/index.md)
- [AppFabric の監視のサンプル](https://docs.microsoft.com/previous-versions/appfabric/ff383407(v=azure.10))
