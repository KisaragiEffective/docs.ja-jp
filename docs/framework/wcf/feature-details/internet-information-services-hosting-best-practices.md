---
title: インターネット インフォメーション サービス ホスティングのベスト プラクティス
ms.date: 03/30/2017
ms.assetid: 0834768e-9665-46bf-86eb-d4b09ab91af5
ms.openlocfilehash: 092e6ab675cf807db44c2085f8b0e7bbf67d7b28
ms.sourcegitcommit: 09b4090b78f52fd09b0e430cd4b26576f1fdf96e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/17/2020
ms.locfileid: "76211917"
---
# <a name="internet-information-services-hosting-best-practices"></a>インターネット インフォメーション サービス ホスティングのベスト プラクティス
このトピックでは、Windows Communication Foundation (WCF) サービスをホストするためのいくつかのベストプラクティスについて説明します。  
  
## <a name="implementing-wcf-services-as-dlls"></a>WCF サービスの DLL としての実装  
 WCF サービスを Web アプリケーションの \bin ディレクトリに配置されている DLL として実装すると、Web アプリケーションモデルの外部でサービスを再利用できます。たとえば、インターネットインフォメーションサービス (IIS) が配置されていない可能性のあるテスト環境などです。  
  
## <a name="service-hosts-in-iis-hosted-applications"></a>IIS でホストされるアプリケーションでのサービス ホスト  
 強制的な自己ホスト Api を使用して、iis ホスト環境でネイティブにサポートされていないネットワークトランスポートをリッスンする新しいサービスホストを作成しないでください (TCP 通信は IIS 6.0 ではネイティブにサポートされていないため、iis 6.0 では TCP サービスをホストします)。 この方法はお勧めしません。 強制的に作成されたサービス ホストは、IIS ホスト環境内で認識されないからです。 重要な点は、IIS がホスト アプリケーション プールがアイドル状態であるかどうかを判断するときに、強制的に作成されたサービスによって実行される処理が IIS によって考慮されないことです。 この結果、強制的に作成されたサービス ホストを持つアプリケーションは、IIS ホスト プロセスを積極的に廃棄する IIS ホスト環境を持つことになります。  
  
## <a name="uris-and-iis-hosted-endpoints"></a>URI と IIS でホストされるエンドポイント  
 IIS でホストされるサービスのエンドポイントは、絶対アドレスではなく、相対 URI (Uniform Resource Identifier) を使用して構成する必要があります。 これにより、エンドポイント アドレスが、ホスト アプリケーションに属する URI アドレスのセット内に確実に含まれ、メッセージに基づくアクティベーションが正常に行われるようになります。  
  
## <a name="state-management-and-process-recycling"></a>状態管理とプロセスのリサイクル  
 IIS ホスト環境は、メモリにローカル状態を保持しないサービスに最適化されています。 IIS は、さまざまな外部および内部イベントに応答してホスト プロセスをリサイクルするため、メモリのみに格納される揮発性の状態はすべて失われます。 IIS でホストされるサービスは、それぞれの状態をプロセスの外部 (データベースなど)、またはアプリケーションのリサイクル イベントが発生した場合に簡単に再作成できるメモリ内キャッシュに格納する必要があります。  
  
> [!NOTE]
> WCF がメッセージ層の信頼性とセキュリティのために使用するプロトコルによって、揮発性のメモリ内状態が使用されます。 WCF の信頼できるセッションとセキュリティセッションは、アプリケーションのリサイクルによって予期せず終了することがあります。 これらのプロトコルを使用する IIS でホストされるアプリケーションは、アプリケーション層の状態を関連付けるために WCF によって提供されるセッションキー以外のもの (アプリケーションレイヤーコンストラクトやカスタム関連付けヘッダーなど) に依存するか、無効にする必要があります。ホストされるアプリケーションの IIS プロセスのリサイクル。  
  
## <a name="optimizing-performance-in-middle-tier-scenarios"></a>中間層シナリオでのパフォーマンスの最適化  
 *中間層シナリオ*で最適なパフォーマンスを実現するために、受信メッセージへの応答として他のサービスを呼び出すサービスは、WCF サービスクライアントをリモートサービスに1回インスタンス化して、複数の受信要求にわたって再利用します。 WCF サービスクライアントのインスタンス化は、既存のクライアントインスタンスでのサービス呼び出しを行う場合に比べてコストのかかる操作であり、中間層シナリオでは、複数の要求にわたってリモートクライアントをキャッシュすることによって、パフォーマンスが個別に向上します。 WCF サービスクライアントはスレッドセーフであるため、複数のスレッドにわたってクライアントへのアクセスを同期する必要はありません。  
  
 また、中間層シナリオでは、`svcutil /a` オプションによって生成された非同期 API を使用してパフォーマンスを向上させます。 `/a` オプションを指定すると、 [ServiceModel メタデータユーティリティツール (svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)によって各サービス操作の `BeginXXX/EndXXX` メソッドが生成されます。これにより、バックグラウンドスレッドでリモートサービスへの実行に時間のかかる可能性がある呼び出しを行うことができます。  
  
## <a name="wcf-in-multi-homed-or-multi-named-scenarios"></a>マルチホーム シナリオまたはマルチネーム シナリオでの WCF  
 IIS Web ファーム内に WCF サービスを展開することができます。この場合、一連のコンピューターが共通の外部名 (`http://www.contoso.com`など) を共有していても、異なるホスト名で個別にアドレス指定されます (たとえば、`http://www.contoso.com` は `http://machine1.internal.contoso.com` `http://machine2.internal.contoso.com`とという名前の2つの異なるコンピューターにトラフィックを転送する場合があります)。 この展開シナリオは WCF で完全にサポートされていますが、サービスのメタデータ (Web サービス記述言語) に正しい (外部の) ホスト名を表示するには、WCF サービスをホストする IIS Web サイトの特別な構成が必要です。  
  
 WCF によって生成されるサービスメタデータに正しいホスト名が表示されるようにするには、明示的なホスト名を使用するように WCF サービスをホストする IIS Web サイトの既定の id を構成します。 たとえば、`www.contoso.com` ファーム内に存在するコンピューターでは、HTTP に対して *:80: www. contoso .com の IIS サイトバインドを使用し、\*: 443: www. contoso .com を HTTPS に使用する必要があります。  
  
 Microsoft 管理コンソール (MMC) スナップインを使用して、IIS Web サイト バインディングを構成できます。  
  
## <a name="application-pools-running-in-different-user-contexts-overwrite-assemblies-from-other-accounts-in-the-temporary-folder"></a>異なるユーザー コンテキストで実行されるアプリケーション プールが、一時フォルダー内の他のアカウントのアセンブリを上書きする  
 異なるユーザーコンテキストで実行されているアプリケーションプールが、一時的な ASP.NET files フォルダー内の他のアカウントのアセンブリを上書きできないようにするには、アプリケーションごとに異なる id と一時フォルダーを使用します。 たとえば、/Application1 と /Application2 という 2 つの仮想アプリケーションがある場合は、2 つの異なる ID を使用して、A と B の 2 つのアプリケーション プールを作成できます。 アプリケーション プール A は、一方のユーザー ID (user1) の下で、アプリケーション プール B は、もう一方のユーザー ID (user2) の下で実行でき、/Application1 が A を、/Application2 が B を使用するように構成します。  
  
 Web.config では、\<system.web/compilation/@tempFolder> を使用して一時フォルダーを構成できます。 /Application1 の場合、"c:\tempForUser1" にすることができ、アプリケーション2起動には "c:\tempForUser2" を指定できます。 これらのフォルダーに対応する書き込みアクセス許可を 2 つの ID に与えます。  
  
 これで、user2 は、(c:\tempForUser1 の下にある) /Application2 のコード生成フォルダーを変更できなくなります。  
  
## <a name="enabling-asynchronous-processing"></a>非同期処理の有効化  
 既定では、IIS 6.0 以前でホストされている WCF サービスに送信されるメッセージは、同期方式で処理されます。 ASP.NET は、独自のスレッド (ASP.NET ワーカースレッド) で WCF を呼び出し、WCF は別のスレッドを使用して要求を処理します。 WCF は、その処理が完了するまで ASP.NET のワーカー スレッドに保持されます。 このため、要求は同期的に処理されます。 要求を非同期で処理することで、要求の処理に必要なスレッド数を減らすことができるため、スケーラビリティが向上します。 WCF は、要求の処理中に ASP.NET スレッドに対してを保持しません。 IIS 6.0 を実行しているコンピューターでは、非同期動作の使用は推奨されません。これは、サーバーが*サービス拒否*(DOS) 攻撃を仕掛けてくる受信要求を調整する方法がないためです。 IIS 7.0 以降では、同時要求スロットルが導入されています`[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\2.0.50727.0]"MaxConcurrentRequestsPerCpu`。 この新しいスロットルにより、非同期処理を安全に使用することができます。  IIS 7.0 の既定では、非同期のハンドラーとモジュールが登録されます。 この機能が無効になっている場合は、アプリケーションの Web.config ファイルで要求の非同期処理を手動で有効にすることができます。 使用する設定は、`aspNetCompatibilityEnabled` 設定によって異なります。 `aspNetCompatibilityEnabled` を `false` に設定している場合は、次の構成スニペットに示すように、`System.ServiceModel.Activation.ServiceHttpModule` を構成します。  
  
```xml  
<system.serviceModel>  
    <serviceHostingEnvironment aspNetCompatibilityEnabled="false" />      
  </system.serviceModel>  
  <system.webServer>  
    <modules>  
      <remove name="ServiceModel"/>  
      <add name="ServiceModel"   
           preCondition="integratedMode,runtimeVersionv2.0"   
           type="System.ServiceModel.Activation.ServiceHttpModule, System.ServiceModel,Version=3.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>  
    </modules>  
    </system.webServer>  
```  
  
 `aspNetCompatibilityEnabled` を `true` に設定している場合は、次の構成スニペットに示すように、`System.ServiceModel.Activation.ServiceHttpHandlerFactory` を構成します。  
  
```xml  
<system.serviceModel>  
    <serviceHostingEnvironment aspNetCompatibilityEnabled="true" />      
  </system.serviceModel>  
  <system.webServer>  
    <handlers>  
          <clear/>  
          <add name="TestAsyncHttpHandler"   
               path="*.svc"   
               verb="*"   
               type="System.ServiceModel.Activation.ServiceHttpHandlerFactory, System.ServiceModel, Version=3.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"           
               />  
    </handlers>      
  </system.webServer>  
```  
  
## <a name="see-also"></a>関連項目

- [サービスホスティングのサンプル](../samples/hosting.md)
- [AppFabric のホスティング機能](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
