---
title: SystemWebRouting 統合サンプル
ms.date: 03/30/2017
ms.assetid: f1c94802-95c4-49e4-b1e2-ee9dd126ff93
ms.openlocfilehash: a91763e7dacb04a68cfea1079d55bbc1eda01668
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094892"
---
# <a name="systemwebrouting-integration-sample"></a>SystemWebRouting 統合サンプル
このサンプルでは、<xref:System.Web.Routing> 名前空間のクラスとのホスト層の統合を示します。 <xref:System.Web.Routing> 名前空間のクラスを使用すると、物理リソースに直接対応しない URL をアプリケーションで使用できます。 Web ルーティングを使用すると、開発者は HTTP 用の仮想アドレスを作成して、実際の WCF サービスにマップすることができます。 これは、物理ファイルやリソースを配置せずに WCF サービスをホストする必要がある場合、または .html や .aspx などのファイルがない URL を使用してサービスにアクセスする必要がある場合に役立ちます。 このサンプルでは、<xref:System.Web.Routing.RouteTable> クラスを使用して、global.asax で定義された実行中のサービスにマップされる仮想 URI を作成する方法を示します。 

> [!NOTE]
> <xref:System.Web.Routing> 名前空間のクラスは、HTTP でホストされるサービスに対してのみ機能します。  
  
この例では、WCF を使用して、`movies` フィードと `channels` フィードという2つの RSS フィードを作成します。 サービスをアクティブ化するための Url に拡張子が含まれておらず、<xref:System.Web.HttpApplication> クラスから派生した `Global` クラスの `Application_Start` メソッドに登録されています。  
  
> [!NOTE]
> このサンプルはインターネットインフォメーションサービス (IIS) 7.0 以降でのみ機能します。 IIS 6.0 では、拡張 Url をサポートするために別の方法が使用されているためです。  

#### <a name="to-download-this-sample"></a>このサンプルをダウンロードするには
  
このサンプルは、コンピューターに既にインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
   
`<InstallDrive>:\WF_WCF_Samples`  
   
 このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
   
`<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Hosting\WebRoutingIntegration`  
  
#### <a name="to-use-this-sample"></a>このサンプルを使用するには  
  
1. Visual Studio を使用して、WebRoutingIntegration .sln ファイルを開きます。  
  
2. ソリューションを実行して Web 開発サーバーを起動するには、F5 キーを押します。  
  
     サンプルのディレクトリの一覧が表示されます。 ファイル拡張子が .svc のファイルがないことに注意してください。  
  
3. アドレスバーで、URL に `movies` を追加し、`http://localhost:[port]/movies` を読み、ENTER キーを押します。  
  
     movies フィードがブラウザーに表示されます。  
  
4. アドレスバーで、URL に `channels` を追加して、が `http://localhost:[port]/channels` を読み、enter キーを押します。  
  
     channels フィードがブラウザーに表示されます。  
  
5. Alt キーを押しながら F4 キーを押して Web ブラウザーを閉じます。  
  
     開発サーバーが終了していない場合は、通知領域アイコンを右クリックし、 **[停止]** を選択します。  
  
#### <a name="to-use-this-sample-when-hosted-in-iis"></a>このサンプルを使用するには (IIS でホストする場合)  
  
1. Visual Studio を使用して、WebRoutingIntegration .sln ファイルを開きます。  
  
2. Ctrl キーと Shift キーを押しながら B キーを押して、プロジェクトをビルドします。  
  
3. インターネット インフォメーション サービス (IIS) マネージャーで Web アプリケーションを作成します。  
  
    1. IIS マネージャーで、既定の **[Web サイト]** を右クリックし、 **[アプリケーションの追加]** を選択します。  
  
    2. **エイリアス**として、「`WebRoutingIntegration`」と入力します。  
  
    3. **[物理パス]** で、プロジェクト内のサービスフォルダーを選択します。  
  
    4. **[OK]** をクリックします。  
  
4. アプリケーションを起動します。そのためには、Web アプリケーションを右クリックして **[アプリケーションの管理]** を選択し、 **[参照]** をクリックします。  
  
5. アドレスバーで、URL に `movies` を追加して、が `http://localhost:[port]/movies` を読み、enter キーを押します。  
  
     movies フィードがブラウザーに表示されます。  
  
6. アドレスバーで、URL に `channels` を追加して、が `http://localhost:[port]/channels` を読み、enter キーを押します。  
  
     channels フィードがブラウザーに表示されます。  
  
7. Alt キーを押しながら F4 キーを押して Web ブラウザーを閉じます。  
  
 このサンプルは、HTTP でホストされたサービスの要求をルーティングするために、<xref:System.Web.Routing> 名前空間のクラスを使用してホスト層を構成できることを示しています。  
  
> [!NOTE]
> 既定のアプリケーションプールのバージョンがバージョン2に設定されている場合は、.NET Framework 4 に更新する必要があります。  
  
## <a name="see-also"></a>参照

- [AppFabric のホスティングと永続化のサンプル](https://docs.microsoft.com/previous-versions/appfabric/ff383418(v=azure.10))
