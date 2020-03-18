---
title: '方法 : Windows Server AppFabric を使用してワークフロー サービスをホストする'
ms.date: 03/30/2017
ms.assetid: 83b62cce-5fc2-4c6d-b27c-5742ba3bac73
ms.openlocfilehash: ecc7a954b2d88cdbcf934cc836e9ad6e3ef7ed52
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964761"
---
# <a name="how-to-host-a-workflow-service-with-windows-server-app-fabric"></a>方法 : Windows Server AppFabric を使用してワークフロー サービスをホストする

AppFabric でのワークフロー サービスのホスティングは IIS/WAS でのホスティングに似ています。 唯一の違いは、ワークフロー サービスの投入、監視、および管理のために AppFabric に用意されているツールです。 このトピックでは、[実行時間の長いワークフローサービスの作成](../../../../docs/framework/wcf/feature-details/creating-a-long-running-workflow-service.md)によって作成されたワークフローサービスを使用します。 ワークフロー サービスの作成方法はそちらのトピックで説明されています。 このトピックでは、AppFabric を使用したワークフロー サービスのホスティング方法を説明します。 Windows Server App Fabric の詳細については、 [Windows Server App fabric のドキュメント](https://docs.microsoft.com/previous-versions/appfabric/ff384253(v=azure.10))を参照してください。 下の手順を完了する前に、Windows Server AppFabric がインストールされていることを確認してください。  これを行うには、インターネットインフォメーションサービス (inetmgr.exe) を開き、 **[接続]** ビューでサーバー名をクリックし、サイト をクリックして、 **[既定の Web サイト]** をクリックします。 画面の右側には、 **[App Fabric]** というセクションが表示されます。 (右側のペインの一番上に表示される) このセクションが表示されない場合は、AppFabric がインストールされていません。 Windows Server App Fabric のインストールの詳細については、「 [Windows Server App fabric のインストール](https://docs.microsoft.com/previous-versions/appfabric/ee790960(v=azure.10))」を参照してください。  
  
### <a name="creating-a-simple-workflow-service"></a>単純なワークフロー サービスの作成  
  
1. Visual Studio 2012 を開き、[実行時間の長いワークフローサービスの作成](../../../../docs/framework/wcf/feature-details/creating-a-long-running-workflow-service.md)に関するトピックで作成した orderprocessing ソリューションを読み込みます。  
  
2. **Orderservice**プロジェクトを右クリックし、 **[プロパティ]** をクリックして、 **[Web]** タブを選択します。  
  
3. プロパティページの **[開始アクション]** セクションで、**特定のページ**を選択し、編集ボックスに「Service1. .xamlx」と入力します。  
  
4. プロパティ ページの **サーバー** セクションで、**ローカル IIS Web サーバーを使用する** を選択し、次の URL を入力します: `http://localhost/OrderService`。  
  
5. **[仮想ディレクトリの作成]** ボタンをクリックします。 これで新しい仮想ディレクトリが作成され、プロジェクトが作成されるときに必要なファイルが仮想ディレクトリにコピーされるようにプロジェクトが設定されます。  または、仮想ディレクトリに .xamlx、web.config、および必要な DLL を手動でコピーすることもできます。  
  
### <a name="configuring-a-workflow-service-hosted-in-windows-server-app-fabric"></a>Windows Server AppFabric でホストされるワークフロー サービスの構成  
  
1. インターネット インフォメーション サービス マネージャー (inetmgr.exe) を開きます。  
  
2. **[接続]** ウィンドウで、orderservice 仮想ディレクトリに移動します。  
  
3. [Orderservice] を右クリックし、選択**管理の WCF と WF サービス**、**構成しています**. **WCF と WF アプリケーションの構成** ダイアログ ボックスが表示されます。  
  
4. **[全般]** タブを選択すると、次のスクリーンショットに示すように、アプリケーションに関する全般的な情報が表示されます。  
  
     ![App Fabric の構成ダイアログの [全般] タブ](../../../../docs/framework/wcf/feature-details/media/appfabricconfiguration-general.gif "AppFabricConfiguration-全般")  
  
5. **[監視]** タブを選択します。次のスクリーンショットに示すように、さまざまな監視設定が表示されます。  
  
     ![[App Fabric の構成の監視] タブ](../../../../docs/framework/wcf/feature-details/media/appfabricconfiguration-monitoring.gif "AppFabricConfiguration-監視")  
  
     App Fabric でのワークフローサービスの監視の構成の詳細については、「 [App fabric で監視を構成する](https://docs.microsoft.com/previous-versions/appfabric/ee677384(v=azure.10))」を参照してください。  
  
6. [**ワークフローの永続**化] タブを選択します。これにより、次のスクリーンショットに示すように、App Fabric の既定の永続化プロバイダーを使用するようにアプリケーションを構成できます。  
  
     ![App Fabric の&#45;構成の永続化](../../../../docs/framework/wcf/feature-details/media/appfabricconfiguration-persistence.gif "AppFabricConfiguration-永続化")  
  
     Windows Server App Fabric でのワークフロー永続化の構成の詳細については、「 [App fabric でのワークフロー永続化の構成](https://docs.microsoft.com/previous-versions/appfabric/ee677353(v=azure.10))」を参照してください。  
  
7. **[ワークフローホストの管理]** タブを選択します。これにより、次のスクリーンショットに示すように、アイドル状態のワークフローサービスインスタンスをアンロードして永続化する必要があることを指定できます。  
  
     ![App Fabric の構成ワークフローのホスト管理](../../../../docs/framework/wcf/feature-details/media/appfabricconfiguration-management.gif "AppFabricConfiguration-管理")  
  
     ワークフローホスト管理の構成の詳細については、「 [App Fabric でのワークフローホスト管理の構成](https://docs.microsoft.com/previous-versions/appfabric/ff383424(v=azure.10))」を参照してください。  
  
8. **[自動開始]** タブを選択します。これにより、次のスクリーンショットに示すように、アプリケーションのワークフローサービスの自動開始設定を指定できます。  
  
     ![App Fabric の自動&#45;開始構成を示すスクリーンショット。](./media/how-to-host-a-workflow-service-with-windows-server-app-fabric/app-fabric-auto-start-configuration.gif)  
  
     自動開始の構成の詳細については、「 [App Fabric を使用した自動開始の構成](https://docs.microsoft.com/previous-versions/appfabric/ee677261(v=azure.10))」を参照してください。  
  
9. **[調整]** タブを選択します。これにより、次のスクリーンショットに示すように、ワークフローサービスの制限設定を構成できます。  
  
     ![App Fabric の制限の構成を示すスクリーンショット。](./media/how-to-host-a-workflow-service-with-windows-server-app-fabric/app-fabric-throttling-configuration.gif)  
  
     スロットルの構成の詳細については、「 [App Fabric でのスロットルの構成](https://docs.microsoft.com/previous-versions/appfabric/ee677261(v=azure.10))」を参照してください。  
  
10. **[セキュリティ]** タブを選択します。これにより、次のスクリーンショットに示すように、アプリケーションのセキュリティ設定を構成できます。  
  
     ![App Fabric のセキュリティ構成](../../../../docs/framework/wcf/feature-details/media/appfabricconfiguration-security.gif "AppFabricConfiguration-セキュリティ")  
  
     Windows Server App Fabric でセキュリティを構成する方法の詳細については、「 [App fabric でのセキュリティの構成](https://docs.microsoft.com/previous-versions/appfabric/ee677278(v=azure.10))」を参照してください。  
  
### <a name="using-windows-server-app-fabric"></a>Windows Server AppFabric の使用  
  
1. ソリューションを作成して、仮想ディレクトリに必要なファイルをコピーします。  
  
2. OrderClient プロジェクトを右クリックし、 **[デバッグ]** 、 **[新しいインスタンスを開始]** の順に選択して、クライアントアプリケーションを起動します。  
  
3. クライアントが実行され、Visual Studio に **[セキュリティ警告の添付]** ダイアログボックスが表示されます。 **[アタッチしない]** ボタンをクリックします。 これにより、IIS プロセスにアタッチしてデバッグしないように Visual Studio に指示します。  
  
4. クライアント アプリケーションは直ちにワークフロー サービスを呼び出して待機します。 ワークフロー サービスはアイドル状態になり永続化されます。 これは、インターネット インフォメーション サービス (inetmgr.exe) を開始して、[接続] ペインで [OrderService] に移動し、それを選択することによって確認できます。 次に、右側のペインで [App Fabric ダッシュボード] のアイコンをクリックします。 [永続化された WF インスタンス] には、次のスクリーンショットに示すように、永続化されたワークフローサービスインスタンスが1つ表示されます。  
  
     ![App Fabric ダッシュボードを示すスクリーンショット。](./media/how-to-host-a-workflow-service-with-windows-server-app-fabric/app-fabric-dashboard.gif)  
  
     **WF インスタンス履歴**には、ワークフローサービスのアクティブ化の数、ワークフローサービスインスタンスの入力候補の数、失敗したワークフローインスタンスの数など、ワークフローサービスに関する情報が表示されます。 アクティブまたはアイドル状態のインスタンスの下にリンクが表示されます。リンクをクリックすると、次のスクリーンショットに示すように、アイドル状態のワークフローインスタンスに関する詳細情報が表示されます。  
  
     ![永続化されたワークフローインスタンスの詳細を示すスクリーンショット。](./media/how-to-host-a-workflow-service-with-windows-server-app-fabric/persisted-workflow-instance-detail.gif)  
  
     Windows Server App Fabric の機能とその使用方法の詳細については、「 [Windows Server App fabric のホスト機能](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [長時間のワークフロー サービスの作成](../../../../docs/framework/wcf/feature-details/creating-a-long-running-workflow-service.md)
- [AppFabric のホスティング機能](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
- [Windows Server App Fabric のインストール](https://docs.microsoft.com/previous-versions/appfabric/ee790960(v=azure.10))
- [Windows Server App Fabric のドキュメント](https://docs.microsoft.com/previous-versions/appfabric/ff384253(v=azure.10))
