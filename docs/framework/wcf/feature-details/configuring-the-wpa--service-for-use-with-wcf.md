---
title: Windows Communication Foundation で使用するための Windows プロセス アクティブ化サービスを設定する
ms.date: 03/30/2017
ms.assetid: 1d50712e-53cd-4773-b8bc-a1e1aad66b78
ms.openlocfilehash: 5533393f759408002b83ba8ff485ba8229e921dd
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964628"
---
# <a name="configuring-the-windows-process-activation-service-for-use-with-windows-communication-foundation"></a>Windows Communication Foundation で使用するための Windows プロセス アクティブ化サービスを設定する
このトピックでは、windows Vista で Windows プロセスアクティブ化サービス (WAS) をセットアップして、HTTP ネットワークプロトコルでは通信しない Windows Communication Foundation (WCF) サービスをホストする手順について説明します。 以降の各セクションで、この構成に関する手順について概説します。  
  
- 必要な WCF アクティブ化コンポーネントをインストール (または、のインストールを確認) します。  
  
- 使用するネットワーク プロトコル バインドを含む WAS サイトを作成するか、新しいプロトコル バインドを既存のサイトに追加します。  
  
- サービスをホストするアプリケーションを作成し、必要なネットワーク プロトコルを使用するようにそのアプリケーションを設定します。  
  
- 非 HTTP エンドポイントを公開する WCF サービスを構築します。  
  
## <a name="configuring-a-site-with-non-http-bindings"></a>非 HTTP バインドを使用したサイトの構成  
 WAS で非 HTTP バインドを使用するには、サイト バインドを WAS 構成に追加する必要があります。 WAS の構成ストアは、%windir%\system32\inetsrv\config ディレクトリにある applicationHost.config ファイルです。 この構成ストアは、WAS と IIS 7.0 の両方で共有されます。  
  
 applicationHost.config は、任意の標準テキスト エディター (メモ帳など) で開くことが可能な XML テキスト ファイルです。 ただし、HTTP 以外のサイトバインドを追加するには、IIS 7.0 コマンドライン構成ツール (appcmd.exe) を使用することをお勧めします。  
  
 次のコマンドは、appcmd.exe を使用して、既定の Web サイトに net.tcp サイト バインドを追加します (このコマンドは 1 行で入力します)。  
  
```console  
appcmd.exe set site "Default Web Site" -+bindings.[protocol='net.tcp',bindingInformation='808:*']  
```  
  
 このコマンドは、次に示す行を applicationHost.config ファイルに追加することによって、既定の Web サイトに新しい net.tcp バインドを追加します。  
  
```xml  
<sites>  
    <site name="Default Web Site" id="1">  
        <bindings>  
            <binding protocol="HTTP" bindingInformation="*:80:" />  
            //The following line is added by the command.  
            <binding protocol="net.tcp" bindingInformation="808:*" />  
        </bindings>  
    </site>  
</sites>  
```  
  
## <a name="enabling-an-application-to-use-non-http-protocols"></a>非 HTTP プロトコルを使用するためのアプリケーションの設定  
 個々のネットワークプロトコルをアプリケーションレベルで有効または無効にすることができます。 次のコマンドは、`Default Web Site` で動作するアプリケーションに対して、HTTP プロトコルと net.tcp プロトコルの両方を有効にする方法を示しています。  
  
```console  
appcmd.exe set app "Default Web Site/appOne" /enabledProtocols:net.tcp  
```  
  
 有効なプロトコルの一覧は、Applicationhost.config に格納されているサイトの XML 構成の \<applicationDefaults > 要素で設定することもできます。  
  
 次の applicationHost.config からの XML コードは、HTTP プロトコルと非 HTTP プロトコルの両方にバインドされたサイトを示しています。 非 HTTP プロトコルのサポートに必要な追加の構成は、コメントで付記されています。  
  
```xml  
<sites>  
    <site name="Default Web Site" id="1">  
    <application path="/">  
        <virtualDirectory path="/" physicalPath="D:\inetpub\wwwroot" />  
    </application>  
       <bindings>  
            //The following two lines are added by the command.  
            <binding protocol="HTTP" bindingInformation="*:80:" />  
            <binding protocol="net.tcp" bindingInformation="808:*" />  
       </bindings>  
    </site>  
    <siteDefaults>  
        <logFile   
        customLogPluginClsid="{FF160663-DE82-11CF-BC0A-00AA006111E0}"  
          directory="D:\inetpub\logs\LogFiles" />  
        <traceFailedRequestsLogging   
          directory="D:\inetpub\logs\FailedReqLogFiles" />  
    </siteDefaults>  
    <applicationDefaults   
      applicationPool="DefaultAppPool"   
      //The following line is inserted by the command.  
      enabledProtocols="http, net.tcp" />  
    <virtualDirectoryDefaults allowSubDirConfig="true" />  
</sites>  
```  
  
 非 HTTP アクティブ化の WAS を使用するサービスをアクティブ化しようとしたときに、WAS をインストールおよび構成していない場合、次のエラーが表示されることがあります。  
  
```output  
[InvalidOperationException: The protocol 'net.tcp' does not have an implementation of HostedTransportConfiguration type registered.]   System.ServiceModel.AsyncResult.End(IAsyncResult result) +15778592   System.ServiceModel.Activation.HostedHttpRequestAsyncResult.End(IAsyncResult result) +15698937   System.ServiceModel.Activation.HostedHttpRequestAsyncResult.ExecuteSynchronous(HttpApplication context, Boolean flowContext) +265   System.ServiceModel.Activation.HttpModule.ProcessRequest(Object sender, EventArgs e) +227   System.Web.SyncEventExecutionStep.System.Web.HttpApplication.IExecutionStep.Execute() +80   System.Web.HttpApplication.ExecuteStep(IExecutionStep step, Boolean& completedSynchronously) +171  
```  
  
 このエラーが表示された場合は、非 HTTP アクティブ化の WAS が適切にインストールおよび構成されていることを確認してください。 詳細については、「[方法: WCF アクティブ化コンポーネントをインストールおよび構成](../../../../docs/framework/wcf/feature-details/how-to-install-and-configure-wcf-activation-components.md)する」を参照してください。  
  
## <a name="building-a-wcf-service-that-uses-was-for-non-http-activation"></a>非 HTTP のアクティブ化で WAS を使用する WCF サービスの構築  
 WAS をインストールして構成する手順 (「[方法: WCF アクティブ化コンポーネントをインストールおよび構成](../../../../docs/framework/wcf/feature-details/how-to-install-and-configure-wcf-activation-components.md)する」を参照してください) を実行すると、アクティブ化のために was を使用するようにサービスを構成することは、IIS でホストされるサービスの構成に似ています。  
  
 WAS によってアクティブ化される WCF サービスを構築する方法の詳細については、「 [How to: Host a WCF service IN was](../../../../docs/framework/wcf/feature-details/how-to-host-a-wcf-service-in-was.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [Windows プロセス アクティブ化サービスでのホスティング](../../../../docs/framework/wcf/feature-details/hosting-in-windows-process-activation-service.md)
- [AppFabric のホスティング機能](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
