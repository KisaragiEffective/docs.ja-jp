---
title: UDP アクティベーション
ms.date: 03/30/2017
ms.assetid: 4b0ccd10-0dfb-4603-93f9-f0857c581cb7
ms.openlocfilehash: 0f5d07e65abc0b29989834aff496f7c27ea557b5
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715807"
---
# <a name="udp-activation"></a>UDP アクティベーション
このサンプルは、 [Transport: UDP](../../../../docs/framework/wcf/samples/transport-udp.md)サンプルを基にしています。 [Transport: UDP](../../../../docs/framework/wcf/samples/transport-udp.md)サンプルを拡張して、Windows プロセスアクティブ化サービス (WAS) を使用したプロセスのアクティブ化をサポートします。  
  
 サンプルは、次の 3 つの主要素で構成されます。  
  
- UDP プロトコル アクティベータ。アクティベーション対象のアプリケーションの代わりに UDP メッセージを受信するスタンドアロンのプロセスです。  
  
- UDP カスタム トランスポートを使用してメッセージを送信するクライアント。  
  
- UDP カスタム トランスポート経由でメッセージを受信するサービス。WAS によってアクティブ化されるワーカー プロセス内でホストされています。  
  
## <a name="udp-protocol-activator"></a>UDP プロトコル アクティベータ  
 UDP プロトコルアクティベーターは、WCF クライアントと WCF サービスの間のブリッジです。 トランスポート層で UDP プロトコルを介するデータ通信を実現します。 主な機能は、次の 2 つです。  
  
- WAS リスナ アダプタ (LA)。WAS と連携し、受信メッセージに応答してプロセスをアクティブ化します。  
  
- UDP プロトコル リスナ。アクティベーション対象のアプリケーションの代わりに UDP メッセージを受け入れます。  
  
 このアクティベータは、サーバー コンピュータ上でスタンドアロン プログラムとして実行される必要があります。 通常、WAS リスナ アダプタ (NetTcpActivator や NetPipeActivator など) は、長時間にわたって実行される Windows サービスに実装されます。 ただしこのサンプルでは、単純化してわかりやすくするために、このプロトコル アクティベータをスタンドアロン アプリケーションとして実装しています。  
  
### <a name="was-listener-adapter"></a>WAS リスナ アダプタ  
 UDP の WAS リスナ アダプタは `UdpListenerAdapter` クラスに実装されています。 これは WAS とやり取りするモジュールで、UDP プロトコル用アプリケーションのアクティベーションを実行します。 これは、次の Webhost API を呼び出すことによって実行されます。  
  
- `WebhostRegisterProtocol`  
  
- `WebhostUnregisterProtocol`  
  
- `WebhostOpenListenerChannelInstance`  
  
- `WebhostCloseAllListenerChannelInstances`  
  
 このリスナ アダプタは、`WebhostRegisterProtocol` を最初に呼び出した後、applicationHost.config (%windir%\system32\inetsrv 内にあります) に登録されているすべてのアプリケーションの WAS からコールバック `ApplicationCreated` を受信します。 このサンプルで処理するのは、(プロトコル ID が "net.udp" の) UDP プロトコルが有効なアプリケーションだけです。 他の実装でこのアプリケーションへの動的な構成変更 (アプリケーションを無効から有効に移行する場合など) に応答する場合、そうした実装では異なる方法でアプリケーションを処理することがあります。  
  
 コールバック `ConfigManagerInitializationCompleted` が受信される場合、WAS により、プロトコルの初期化に関するすべての通知が完了したことを示します。 その時点で、リスナ アダプタはアクティベーション要求を処理する準備ができています。  
  
 アプリケーションに初めて新しい要求が届くと、このリスナ アダプタは `WebhostOpenListenerChannelInstance` を WAS に呼び出します。ワーカー プロセスがまだ開始されていない場合は、これにより開始されます。 次に、プロトコル ハンドラが読み込まれ、リスナ アダプタと仮想アプリケーション間の通信を開始できるようになります。  
  
 リスナーアダプターは、次のように <`listenerAdapters`> セクションの%SystemRoot%\System32\inetsrv\ApplicationHost.config に登録されます。  
  
```xml  
<add name="net.udp" identity="S-1-5-21-2127521184-1604012920-1887927527-387045" />  
```  
  
### <a name="protocol-listener"></a>プロトコル リスナ  
 UDP プロトコル リスナはプロトコル アクティベータ内部のモジュールで、仮想アプリケーションの代わりに UDP エンドポイントでリッスンします。 これは `UdpSocketListener` クラスに実装されています。 エンドポイントは `IPEndpoint` として表され、このポート番号はサイトのプロトコルのバインディングから抽出されます。  
  
### <a name="control-service"></a>サービスの制御  
 このサンプルでは、WCF を使用して、アクティベーターと WAS ワーカープロセス間の通信を行います。 アクティベータに存在するサービスは、コントロール サービスと呼ばれます。  
  
## <a name="protocol-handlers"></a>プロトコル ハンドラー  
 リスナ アダプタが `WebhostOpenListenerChannelInstance` を呼び出した後、ワーカー プロセスが開始されていない場合は WAS プロセス マネージャによって開始されます。 ワーカー プロセス内部のアプリケーション マネージャは、UDP プロセス プロトコル ハンドラ (PPH) を読み込み、`ListenerChannelId` を要求します。 の PPH は `IAdphManager`を呼び出します。`StartAppDomainProtocolListenerChannel` を実行して、UDP AppDomain プロトコルハンドラー (ADPH) を開始します。  
  
## <a name="hostedudptransportconfiguration"></a>HostedUDPTransportConfiguration  
 この情報は、Web.config で次のように登録されます。  
  
```xml  
<serviceHostingEnvironment>  
<add name="net.udp" transportConfigurationType="Microsoft.ServiceModel.Samples.Hosting.HostedUdpTransportConfiguration, UdpActivation, Version=1.0.0.0, Culture=neutral, PublicKeyToken=6fa904d2da1848d6" />  
</serviceHostingEnvironment>  
```  
  
## <a name="special-setup-for-this-sample"></a>このサンプルの特殊なセットアップ  
 このサンプルは、Windows Vista、Windows Server 2008、または Windows 7 でのみビルドおよび実行可能です。 サンプルを実行するには、最初にすべてのコンポーネントを正しくセットアップする必要があります。 サンプルをインストールするには、次の手順に従います。  
  
#### <a name="to-set-up-this-sample"></a>このサンプルをセットアップするには  
  
1. 次のコマンドを使用して、ASP.NET 4.0 をインストールします。  
  
    ```console  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. Windows Vista でプロジェクトをビルドします。 コンパイル後、ビルド後のフェーズで次の操作を実行します。  
  
    - UDP バインディングを [既定の Web サイト] のサイトにインストールします。  
  
    - 物理パス "%SystemDrive%\inetpub\wwwroot\servicemodelsamples" をポイントする、仮想アプリケーション "ServiceModelSamples" を作成します。  
  
    - さらに、この仮想アプリケーションの "net.udp" プロトコルを有効にします。  
  
3. ユーザー インターフェイス アプリケーション "WasNetActivator.exe" を起動します。 **[セットアップ]** タブをクリックし、次のチェックボックスをオンにして、 **[インストール]** をクリックしてインストールします。  
  
    - UDP リスナ アダプタ  
  
    - UDP プロトコル ハンドラ  
  
4. ユーザーインターフェイスアプリケーション "の**アクティブ化**" タブをクリックします。 **[開始]** ボタンをクリックして、リスナーアダプターを開始します。 これで、プログラムを実行する準備ができました。  
  
    > [!NOTE]
    > このサンプルの実行後は、Cleanup.bat を実行して、[既定の Web サイト] から net.udp バインディングを削除する必要があります。  
  
## <a name="sample-usage"></a>サンプルの使用方法  
 コンパイル後、次の 4 つの異なるバイナリが生成されます。  
  
- Client.exe: クライアント コード。 App.config は、クライアント構成ファイルの Client.exe.config にコンパイルされます。  
  
- UDPActivation.dll: 主要なすべての UDP 実装を含むライブラリ。  
  
- Service.dll: サービス コード。 これは、ServiceModelSamples 仮想アプリケーションの \bin ディレクトリにコピーされます。 サービスファイルは .svc で、構成ファイルは web.config です。コンパイル後、これらは次の場所にコピーされます:%Systemdrive%\inetpub\wwwroot\servicemodelsamples  
  
- WasNetActivator: UDP アクティベータ プログラム。  
  
- 必要なすべての要素が正しくインストールされていることを確認します。 サンプルを実行する方法を、次の手順に示します。  
  
1. 次の Windows サービスが開始されていることを確認します。  
  
    - Windows プロセス アクティブ化サービス (WAS)。  
  
    - インターネット インフォメーション サービス (IIS): W3SVC。  
  
2. 次に、アクティベータ WasNetActivator.exe を起動します。 **[アクティブ化]** タブのドロップダウンリストで、唯一のプロトコルである**UDP**が選択されています。 **[開始]** ボタンをクリックして、アクティベーターを開始します。  
  
3. アクティベータが開始されると、コマンド ウィンドウで Client.exe を実行することにより、クライアント コードを実行できます。 このサンプルの出力は、次のようになります。  
  
    ```console  
    Testing Udp Activation.  
    Start the status service.  
    Sending UDP datagrams.  
    Type a word that you want to say to the server: Hello, world!  
        Sending datagram: Hello, world![0]  
        Sending datagram: Hello, world![1]  
        Sending datagram: Hello, world![2]  
        Sending datagram: Hello, world![3]  
        Sending datagram: Hello, world![4]  
    Calling UDP duplex contract (ICalculatorContract).  
        0 + 0 = 0  
        1 + 2 = 3  
        2 + 4 = 6  
        3 + 6 = 9  
        4 + 8 = 12  
    Getting status and dump server traces:  
        Operation 'Hello' is called: Hello, world![0]  
        Operation 'Hello' is called: Hello, world![1]  
        Operation 'Hello' is called: Hello, world![2]  
        Operation 'Hello' is called: Hello, world![3]  
        Operation 'Hello' is called: Hello, world![4]  
        Operation 'Add' is called: 0 + 0  
        Operation 'Add' is called: 1 + 2  
        Operation 'Add' is called: 2 + 4  
        Operation 'Add' is called: 3 + 6  
        Operation 'Add' is called: 4 + 8  
    Press <ENTER> to complete test.  
    ```  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Transport\UdpActivation`  
