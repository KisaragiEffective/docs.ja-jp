---
title: インターネット インフォメーション サービスでホストされる WCF サービスの配置
ms.date: 03/30/2017
ms.assetid: 04ebd329-3fbd-44c3-b3ab-1de3517e27d7
ms.openlocfilehash: 36bdd22ec838af8b1a6b3be8c5636beced9b9132
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964855"
---
# <a name="deploying-an-internet-information-services-hosted-wcf-service"></a>インターネット インフォメーション サービスでホストされる WCF サービスの配置

インターネットインフォメーションサービス (IIS) でホストされる Windows Communication Foundation (WCF) サービスの開発と展開は、次のタスクで構成されています。

- IIS、ASP.NET、WCF、および WCF アクティブ化コンポーネントが正しくインストールおよび登録されていることを確認します。

- 新しい IIS アプリケーションを作成するか、既存の ASP.NET アプリケーションを再利用します。

- WCF サービスの .svc ファイルを作成します。

- IIS アプリケーションにサービス実装を展開します。

- WCF サービスを構成します。

IIS でホストされる WCF サービスの作成に関する詳細なチュートリアルについては、「 [How to: Host a WCF service IN iis](how-to-host-a-wcf-service-in-iis.md)」を参照してください。

## <a name="ensure-that-iis-aspnet-and-wcf-are-correctly-installed-and-registered"></a>IIS、ASP.NET、および WCF が正しくインストールおよび登録されていることの確認

IIS でホストされる WCF サービスが正常に機能するためには、WCF、IIS、および ASP.NET をインストールする必要があります。 WCF (.NET Framework の一部として)、ASP.NET、および IIS のインストール手順は、オペレーティングシステムによって異なります。 WCF と .NET Framework のインストールの詳細については、「[開発者向けの .NET Framework のインストール](../../install/guide-for-developers.md)」を参照してください。 Windows 10 に IIS をインストールするには、**コントロールパネル**の **[プログラムと機能]** を開き、 **[Windows の機能の有効化または無効化]** を選択します。 **Windows の機能** で、**インターネットインフォメーションサービス**を選択し、 **OK**を選択します。

![IIS が強調表示されている Windows の機能](./media/windows-features-iis.png)

他のオペレーティングシステムに IIS をインストールする手順については、「windows [Vista および windows 7 に](/iis/install/installing-iis-7/installing-iis-on-windows-vista-and-windows-7)Iis をインストールする」および「 [Windows Server 2012 R2 に Iis 8.5 をインストール](/iis/install/installing-iis-85/installing-iis-85-on-windows-server-2012-r2)する」を参照してください。

IIS が既にコンピューターに存在する場合、.NET Framework のインストールプロセスによって WCF が自動的に IIS に登録されます。 .NET Framework の後に IIS がインストールされている場合は、WCF を IIS と ASP.NET に登録するために追加の手順が必要になります。 使用しているオペレーティング システムに応じて、次のように実行します。

- Windows 7 および Windows Server 2003: [ServiceModel 登録ツール (servicemodelreg.exe)](../../../../docs/framework/wcf/servicemodelreg-exe.md)ツールを使用して、WCF を IIS に登録します。 このツールを使用するには、 [Visual Studio の開発者コマンドプロンプト](../../tools/developer-command-prompt-for-vs.md)に「 **servicemodelreg.exe/i/x** 」と入力します。

- Windows 7: 最後に、.NET Framework バージョン4以降を使用するように ASP.NET が構成されていることを確認する必要があります。 これを行うには、[`–i`] オプションを使用して ASPNET_Regiis ツールを実行します。 詳細については、「 [ASP.NET IIS Registration Tool](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/k6h9cz8h(v=vs.90))」を参照してください。

## <a name="create-a-new-iis-application-or-reuse-an-existing-aspnet-application"></a>新しい IIS アプリケーションの作成、または既存の ASP.NET アプリケーションの再利用

IIS でホストされる WCF サービスは、IIS アプリケーションの内部に存在する必要があります。 WCF サービスを排他的にホストする新しい IIS アプリケーションを作成できます。 または、ASP.NET 2.0 コンテンツ (.aspx ページや ASP.NET Web サービス (ASMX) など) を既にホストしている既存のアプリケーションに WCF サービスをデプロイすることもできます。 これらのオプションの詳細については、「wcf[サービスおよび ASP.NET](wcf-services-and-aspnet.md)」の「wcf と ASP.NET の同時ホスト」および「ASP.NET 互換モードでの wcf サービスのホスト」を参照してください。

IIS 6.0 以降のバージョンでは、分離されたオブジェクト指向プログラミングアプリケーションが定期的に再起動されることに注意してください。 既定値は 1740 分です。 サポートされている最大値は 71,582 分です。 この再起動は、無効にできます。 このプロパティの詳細については、「 [Periodicrestarttime](https://docs.microsoft.com/previous-versions/iis/6.0-sdk/ms525914(v=vs.90))」を参照してください。

## <a name="create-an-svc-file-for-the-wcf-service"></a>WCF サービス用の .svc ファイルの作成

IIS でホストされる WCF サービスは、IIS アプリケーション内で特別なコンテンツファイル (.svc ファイル) として表されます。 これは、ASMX ページが、IIS アプリケーションの内部で .asmx ファイルとして表現されるのと同様です。 .Svc ファイルには、wcf ホスティングインフラストラクチャが受信メッセージに応答してホステッドサービスをアクティブ化できるようにする WCF 固有の処理ディレクティブ ([\@ServiceHost](../../../../docs/framework/configure-apps/file-schema/wcf-directive/servicehost.md)) が含まれています。 .svc ファイルの最も一般的な構文を次のステートメントに示します。

`<% @ServiceHost Service="MyNamespace.MyServiceImplementationTypeName" %>`

これは、 [\@ServiceHost](../../../../docs/framework/configure-apps/file-schema/wcf-directive/servicehost.md)ディレクティブと1つの属性 `Service`で構成されます。 `Service` 属性の値は、サービス実装の共通言語ランタイム (CLR: Common Language Runtime) 型名です。 このディレクティブを使用することは、次のコードを使用してサービス ホストを作成することと基本的に同じです。

```csharp
new ServiceHost( typeof( MyNamespace.MyServiceImplementationTypeName ) );
```

サービスのベース アドレスの一覧を作成するなど、追加のホスト構成を実行することもできます。 カスタム <xref:System.ServiceModel.Activation.ServiceHostFactory> を使用して、このディレクティブをカスタム ホスト ソリューション用に拡張することもできます。 WCF サービスをホストする IIS アプリケーションは、<xref:System.ServiceModel.ServiceHost> インスタンスの作成と有効期間の管理を担当しません。 マネージ WCF ホスティングインフラストラクチャは、.svc ファイルに対して最初の要求を受信したときに、必要な <xref:System.ServiceModel.ServiceHost> インスタンスを動的に作成します。 このインスタンスは、コードによって明示的に閉じられるか、アプリケーションがリサイクルされるときまで解放されません。

.Svc ファイルの構文の詳細については、「 [\@ServiceHost](../../../../docs/framework/configure-apps/file-schema/wcf-directive/servicehost.md)」を参照してください。

## <a name="deploy-the-service-implementation-to-the-iis-application"></a>IIS アプリケーションへのサービス実装の展開

IIS でホストされる WCF サービスでは、ASP.NET 2.0 と同じ動的コンパイルモデルが使用されます。 ASP.NET の場合と同様に、IIS でホストされる WCF サービスの実装コードは、さまざまな場所で次のようにいくつかの方法でデプロイできます。

- グローバル アセンブリ キャッシュ (GAC: Global Assembly Cache) またはアプリケーションの \bin ディレクトリに配置される、プリコンパイルされた .dll ファイルとして展開します。 プリコンパイルされたバイナリは、新しいバージョンのクラス ライブラリが展開されるまで更新されません。

- アプリケーションの \ App_Code ディレクトリにある、コンパイルされていないソースファイルとして。 このディレクトリに配置されたソース ファイルは、アプリケーションの最初の要求を処理するときに動的に要求されます。 \App_Code ディレクトリ内のファイルを変更すると、次の要求を受け取ったときにアプリケーション全体がリサイクルされ、再コンパイルされます。

- .Svc ファイルに直接配置される、コンパイルされていないコードとして。 実装コードは、\@ServiceHost ディレクティブの後に、サービスの .svc ファイルにインラインで配置することもできます。 インライン コードを変更すると、次の要求を受け取ったときにアプリケーションがリサイクルされ、再コンパイルされます。

ASP.NET 2.0 コンパイルモデルの詳細については、「 [ASP.NET コンパイルの概要](https://docs.microsoft.com/previous-versions/aspnet/ms178466(v=vs.100))」を参照してください。

## <a name="configure-the-wcf-service"></a>WCF サービスの構成

IIS でホストされる WCF サービスは、アプリケーションの Web.config ファイルに構成を格納します。 IIS でホストされるサービスは、IIS の外部でホストされる WCF サービスと同じ構成要素と構文を使用します。 ただし、次の制約は、IIS ホスト環境に固有です。

- IIS でホストされるサービスのベース アドレス。

- IIS の外部で WCF サービスをホストするアプリケーションは、ベースアドレス Uri のセットを <xref:System.ServiceModel.ServiceHost> コンストラクターに渡すか、サービスの構成に[\<ホスト >](../../../../docs/framework/configure-apps/file-schema/wcf/host.md)要素を指定することによって、ホストするサービスのベースアドレスを制御できます。 IIS でホストされるサービスは、それぞれのベース アドレスを制御できません。IIS でホストされるサービスのベース アドレスは、その .svc ファイルのアドレスです。

### <a name="endpoint-addresses-for-iis-hosted-services"></a>IIS でホストされるサービスのエンドポイント アドレス

IIS でホストされるときのエンドポイント アドレスは、常にサービスを表す .svc ファイルのアドレスを基準にした相対アドレスと見なされます。 たとえば、WCF サービスのベースアドレスが次のエンドポイント構成で `http://localhost/Application1/MyService.svc` 場合は、次のようになります。

```xml
<endpoint address="anotherEndpoint" .../>
```

これにより、`http://localhost/Application1/MyService.svc/anotherEndpoint`に到達できるエンドポイントが提供されます。

同様に、相対アドレスとして空の文字列を使用するエンドポイント構成要素は、ベースアドレスである `http://localhost/Application1/MyService.svc`で到達できるエンドポイントを提供します。

```xml
<endpoint address="" ... />
```

IIS でホストされるサービスのエンドポイントには、常に相対エンドポイント アドレスを使用する必要があります。 完全修飾エンドポイントアドレス (たとえば、`http://localhost/MyService.svc`) を指定すると、エンドポイントアドレスがエンドポイントを公開するサービスをホストする IIS アプリケーションを指していない場合、サービスの展開でエラーが発生する可能性があります。 ホストされるサービスに相対エンドポイント アドレスを使用すると、このような競合が回避されます。

### <a name="available-transports"></a>利用可能なトランスポート

IIS 5.1 および IIS 6.0 でホストされる WCF サービスは、HTTP ベースの通信を使用するように制限されています。 これらの IIS プラットフォームでホストされるサービスで、非 HTTP バインドを使用するように構成すると、サービスをアクティブ化するときにエラーが発生します。 IIS 7.0 の場合、サポートされているトランスポートには、既存の MSMQ アプリケーションとの下位互換性を維持するための HTTP、Net.tcp、Net.pipe、net.tcp、および msmq-mqseries が含まれます。

### <a name="http-transport-security"></a>HTTP トランスポート セキュリティ

IIS でホストされる WCF サービスは、HTTP トランスポートセキュリティ (たとえば、基本認証、ダイジェスト認証、Windows 統合認証などの http 認証スキーム) を使用できます。ただし、サービスを含む IIS 仮想ディレクトリでサポートされている場合は、設定。 ホストされるエンドポイントのバインディングでの HTTP トランスポート セキュリティ設定は、そのエンドポイントを格納する IIS 仮想ディレクトリでのトランスポート セキュリティ設定と一致する必要があります。

たとえば、HTTP ダイジェスト認証を使用するように構成された WCF エンドポイントは、HTTP ダイジェスト認証を許可するように構成された IIS 仮想ディレクトリに存在する必要があります。 IIS 設定と WCF エンドポイント設定の組み合わせが一致しないと、サービスのアクティブ化中にエラーが発生します。

## <a name="see-also"></a>関連項目

- [インターネット インフォメーション サービスでのホスティング](hosting-in-internet-information-services.md)
- [インターネット インフォメーション サービス ホスティングのベスト プラクティス](internet-information-services-hosting-best-practices.md)
- [AppFabric のホスティング機能](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
