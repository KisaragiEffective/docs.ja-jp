---
title: ETW トレース
ms.date: 03/30/2017
ms.assetid: ac99a063-e2d2-40cc-b659-d23c2f783f92
ms.openlocfilehash: a62403e61e0566d5e7b753ff951bf4b316209b6f
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742117"
---
# <a name="etw-tracing"></a>ETW トレース
このサンプルでは、Event Tracing for Windows (ETW) と、このサンプルに用意されている `ETWTraceListener` を使用して、エンドツーエンド (E2E) のトレースを実装する方法を示します。 このサンプルは[はじめに](../../../../docs/framework/wcf/samples/getting-started-sample.md)に基づいており、ETW トレースが含まれています。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
 このサンプルでは、[トレースとメッセージログ](../../../../docs/framework/wcf/samples/tracing-and-message-logging.md)について理解していることを前提としています。  
  
 <xref:System.Diagnostics> トレース モデル内の各トレース ソースでは、データのトレース場所とトレース方法を決定するトレース リスナーを複数設定できます。 リスナの種類では、トレース データの記録形式が定義されます。 リスナを構成に追加する方法を、次のコード サンプルに示します。  
  
```xml  
<system.diagnostics>  
    <sources>  
        <source name="System.ServiceModel"   
             switchValue="Verbose,ActivityTracing"  
             propagateActivity="true">  
            <listeners>  
                <add type=  
                   "System.Diagnostics.DefaultTraceListener"  
                   name="Default">  
                   <filter type="" />  
                </add>  
                <add name="ETW">  
                    <filter type="" />  
                </add>  
            </listeners>  
        </source>  
    </sources>  
    <sharedListeners>  
        <add type=  
            "Microsoft.ServiceModel.Samples.EtwTraceListener, ETWTraceListener"  
            name="ETW" traceOutputOptions="Timestamp">  
            <filter type="" />  
       </add>  
    </sharedListeners>  
</system.diagnostics>  
```  
  
 このリスナを使用する前に、ETW トレース セッションを開始する必要があります。 このセッションは、Logman.exe または Tracelog.exe を使用して開始できます。 このサンプルには、ETW トレース セッションをセットアップするための SetupETW.bat ファイルと、セッションを閉じてログ ファイルを完了するための CleanupETW.bat ファイルが含まれています。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。 これらのツールの詳細については、「」を参照してください <https://go.microsoft.com/fwlink/?LinkId=56580>  
  
 ETWTraceListener を使用する場合、トレースはバイナリの .etl ファイルにログ記録されます。 ServiceModel トレースが有効な場合、生成されるすべてのトレースは同じファイルに表示されます。 .Etl と .svclog のログファイルを表示するには、[サービストレースビューアーツール (svctraceviewer.exe)](../../../../docs/framework/wcf/service-trace-viewer-tool-svctraceviewer-exe.md)を使用します。 このビューアでは、メッセージを送信側から受信側や使用地点までトレースできる、システムのエンドツーエンドのビューが作成されます。  
  
 ETW トレース リスナは、循環ログをサポートします。 この機能を有効にするには、 **[スタート]** 、 **[実行]** の `cmd` 入力して、コマンドコンソールを起動します。 次のコマンドでは、`<logfilename>` パラメータを使用するログ ファイル名に置き換えます。  
  
```console  
logman create trace Wcf -o <logfilename> -p "{411a0819-c24b-428c-83e2-26b41091702e}" -f bincirc -max 1000  
```  
  
 `-f` スイッチと `-max` スイッチは省略できます。 これらのスイッチでは、それぞれバイナリの循環形式と 1000 MB の最大ログ サイズが指定されます。 `-p` スイッチは、トレース プロバイダーを指定する際に使用します。 この例では、`"{411a0819-c24b-428c-83e2-26b41091702e}"` は "XML ETW サンプル プロバイダー" の GUID です。  
  
 セッションを開始するには、次のコマンドを入力します。  
  
```console  
logman start Wcf  
```  
  
 ログ記録を完了したら、次のコマンドを使用してセッションを停止できます。  
  
```console  
logman stop Wcf  
```  
  
 このプロセスでは、[サービストレースビューアーツール (svctraceviewer.exe)](../../../../docs/framework/wcf/service-trace-viewer-tool-svctraceviewer-exe.md)や Tracerpt など、選択したツールで処理できるバイナリ循環ログが生成されます。  
  
 循環[トレース](../../../../docs/framework/wcf/samples/circular-tracing.md)のサンプルを参照して、循環ログを実行する代替リスナーの詳細情報を確認することもできます。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行していることを確認してください。  
  
2. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
    > [!NOTE]
    > RegisterProvider.bat、SetupETW.bat、および CleanupETW.bat コマンドを使用するには、ローカル管理者アカウントで実行する必要があります。 Windows Vista 以降を使用している場合は、管理者特権でコマンドプロンプトを実行する必要もあります。 これを行うには、コマンドプロンプトのアイコンを右クリックし、 **[管理者として実行]** をクリックします。  
  
3. サンプルを実行する前に、クライアントとサーバーで RegisterProvider.bat を実行します。 この結果、トレースを生成する ETWTracingSampleLog.etl ファイルがセットアップされます。このトレースはサービス トレース ビューアで読み取ることができます。 このファイルは、C:\logs フォルダにあります。 このフォルダーが存在しない場合はフォルダーを作成する必要があります。作成しない場合、トレースは生成されません。 次に、クライアント コンピューターとサーバー コンピューターで SetupETW.bat を実行し、ETW トレース セッションを開始します。 SetupETW.bat ファイルは、CS\Client フォルダーの下にあります。  
  
4. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。  
  
5. サンプルの使用が終わったら、CleanupETW.bat を実行して ETWTracingSampleLog.etl ファイルの作成を完了します。  
  
6. サービス トレース ビューア内で ETWTracingSampleLog.etl ファイルを開きます。 このバイナリ形式のファイルを .svclog ファイルとして保存するかどうかを尋ねるメッセージが表示されます。  
  
7. 新しく作成された .svclog ファイルをサービス トレース ビューアー内で開き、ETW トレースと ServiceModel トレースを表示します。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\AnalyticTrace`  
  
## <a name="see-also"></a>参照

- [AppFabric の監視のサンプル](https://docs.microsoft.com/previous-versions/appfabric/ff383407(v=azure.10))
