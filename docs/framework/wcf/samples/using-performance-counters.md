---
title: パフォーマンス カウンターの使用
ms.date: 03/30/2017
ms.assetid: 00a787af-1876-473c-a48d-f52b51e28a3f
ms.openlocfilehash: 6c125ecd0f6cef10b62e7e451eb37bba1ce89b65
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094840"
---
# <a name="using-performance-counters"></a>パフォーマンス カウンターの使用
このサンプルでは、Windows Communication Foundation (WCF) パフォーマンスカウンターにアクセスする方法と、ユーザー定義のパフォーマンスカウンターを作成する方法を示します。 このサンプルは、[はじめに](../../../../docs/framework/wcf/samples/getting-started-sample.md)に基づいています。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
 このサンプルでは、クライアントが `ICalculator` サービスの 4 つのメソッドを呼び出します。 この処理は、ユーザーが中断するまで継続して行われます。 サービスは変更されません。  
  
 パフォーマンス カウンタは、サービスの Web.config ファイルの診断セクションで有効にします。次のサンプル構成を参照してください。  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <diagnostics performanceCounters="All" />   
  </system.serviceModel>  
</configuration>  
```  
  
 このタスクは、[構成エディターツール (svcconfigeditor.exe)](../../../../docs/framework/wcf/configuration-editor-tool-svcconfigeditor-exe.md)を使用して行うこともできます。  
  
 パフォーマンスカウンターを有効にすると、WCF パフォーマンスカウンターのスイート全体がサービスに対して有効になります。 .NET Framework は、`ServiceModelService`、`ServiceModelEndpoint`、および `ServiceModelOperation` の 3 つのレベルで、パフォーマンス データを自動的に保持します。 これらの各レベルには、"呼び出し"、"1 秒あたりの呼び出し回数"、"承認されていないセキュリティ呼び出し" などのパフォーマンス カウンタがあります。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
3. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。  
  
### <a name="to-view-performance-data"></a>パフォーマンス データを表示するには  
  
1. パフォーマンスモニターツールを起動するには、 **[スタート]** ボタンをクリックし、 **[実行]** をクリックして `perfmon` を入力し、[ **OK] を**クリックします。または、コントロールパネルで **[管理ツール]** を選択し、 **[パフォーマンス]** をダブルクリックします。  
  
    > [!NOTE]
    > サンプル コードが実行されるまでは、カウンタを追加することはできません。  
  
2. 一覧表示されているパフォーマンス カウンタを削除するには、削除するパフォーマンス カウンタを選択して Del キーを押します。  
  
3. WCF カウンターを追加するには、グラフペインを右クリックし、 **[カウンターの追加]** を選択します。 **[カウンターの追加]** ダイアログボックスで、パフォーマンスオブジェクト ボックスの一覧の  **[ServiceModelOperation 3.0.0.0、ServiceModelEndpoint 3.0.0.0、または ServiceModelService 3.0.0.0]** を選択します。 表示するカウンタを一覧から選択します。  
  
    > [!NOTE]
    > コンピューターで WCF サービスが実行されていない場合、サービスの WCF パフォーマンスカウンターはありません。  
  
### <a name="to-use-the-configuration-editor-to-enable-counters"></a>構成エディターを使用してカウンターを有効にするには  
  
1. SvcConfigEditor.exe のインスタンスを開きます。  
  
2. ファイル メニューの **開く** をクリックし、**構成ファイル...** をクリックします。  
  
3. サンプル アプリケーションの service フォルダーに移動し、Web.config ファイルを開きます。  
  
4. 構成ツリーで **[診断]** をクリックします。  
  
5. **[診断]** ウィンドウの **[パフォーマンスカウンター]** を [すべて] に切り替えます。  
  
6. 構成ファイルを保存し、エディターを終了します。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\PerfCounters`  
  
## <a name="see-also"></a>参照

- [AppFabric の監視のサンプル](https://docs.microsoft.com/previous-versions/appfabric/ff383407(v=azure.10))
