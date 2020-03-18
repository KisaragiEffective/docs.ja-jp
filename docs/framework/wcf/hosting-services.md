---
title: ホスティング サービス
ms.date: 03/30/2017
helpviewer_keywords:
- hosting services [WCF]
ms.assetid: 192be927-6be2-4fda-98f0-e513c4881acc
ms.openlocfilehash: 9608f9cc55bbba29686440be529659c6606b0eb8
ms.sourcegitcommit: cdf5084648bf5e77970cbfeaa23f1cab3e6e234e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76921045"
---
# <a name="hosting-services"></a>ホスティングサービス

アクティブにするには、サービスを作成してそのコンテキストと有効期間を制御するランタイム環境内で、サービスをホストする必要があります。 Windows Communication Foundation (WCF) サービスは、マネージコードをサポートする任意の Windows プロセスで実行するように設計されています。

WCF は、サービス指向アプリケーションを構築するための統一されたプログラミングモデルを提供します。 このプログラミング モデルには一貫性があり、サービスが展開されるランタイム環境に影響されません。 これは実際には、ホスト オプションにかかわらず、サービスのコードがほぼ同じになることを意味しています。

これらのホスト オプションの範囲は、コンソール アプリケーション内部での実行から、Windows サービスのようなサーバー環境、インターネット インフォメーション サービス (IIS) または Windows プロセス アクティブ化サービス (WAS) で管理されるワーカー プロセス内での実行までさまざまです。 開発者は、サービスの展開要件を満たすホスト環境を選択します。 このような要件は、アプリケーションを展開するプラットフォーム、メッセージの送受信を行うトランスポート、適切な可用性を保証するために必要なプロセスのリサイクルや管理、その他、管理上または信頼性上の要件から導き出されます。 ホスト オプションの情報とガイドラインについて、次のセクションで説明します。

## <a name="hosting-options"></a>ホスティングオプション

### <a name="self-host-in-a-managed-application"></a>マネージアプリケーションでの自己ホスト
 WCF サービスは、任意のマネージアプリケーションでホストできます。 これは、展開に必要なインフラストラクチャが最小限になるため、最も柔軟なオプションです。 マネージド アプリケーション コード内にサービスのコードを埋め込み、続いて <xref:System.ServiceModel.ServiceHost> のインスタンスを作成して開き、サービスを有効にします。 詳細については、「[方法: マネージアプリケーションで WCF サービスをホスト](how-to-host-a-wcf-service-in-a-managed-application.md)する」を参照してください。

 このオプションを使用すると、コンソールアプリケーション内で実行される WCF サービスと、Windows Presentation Foundation (WPF) または Windows フォーム (WinForms) に基づくリッチクライアントアプリケーションなど、2つの一般的なシナリオが有効になります。 通常、コンソールアプリケーション内で WCF サービスをホストすることは、アプリケーションの開発段階で役立ちます。 コンソール アプリケーションにより、アプリケーション内部で起こっている状況を見極めるための情報のデバッグやトレースが容易になり、新しい場所にアプリケーションをコピーして移動することも簡単に行うことができます。 また、このホストオプションを使用すると、WPF や WinForms アプリケーションなどのリッチクライアントアプリケーションで外部との通信を容易に行うことができます。 たとえば、ユーザーインターフェイスに WPF を使用し、他のクライアントがそれに接続して情報を共有できるようにする WCF サービスもホストするピアツーピアコラボレーションクライアントなどです。

### <a name="managed-windows-services"></a>マネージド Windows サービス

このホストオプションは、WCF サービスをホストするアプリケーションドメイン (AppDomain) をマネージ Windows サービス (旧称 NT サービス) として登録することで構成されます。これにより、サービスのプロセスの有効期間がサービスコントロールマネージャー (SCM) によって制御されるようになります。Windows サービス。 自己ホスト オプションと同様、この種類のホスト環境では、ホスト コードをアプリケーションの一部として記述する必要があります。 このサービスは、Windows サービスと WCF サービスの両方として実装されます。これにより、WCF サービスコントラクトインターフェイスから、<xref:System.ServiceProcess.ServiceBase> クラスから継承されるようになります。 次に <xref:System.ServiceModel.ServiceHost> を作成し、オーバーライドされた <xref:System.ServiceProcess.ServiceBase.OnStart%28System.String%5B%5D%29> メソッドで開き、オーバーライドされた <xref:System.ServiceProcess.ServiceBase.OnStop> メソッドで閉じます。 また、 <xref:System.Configuration.Install.Installer> から継承されるインストーラー クラスも実装し、プログラムが Installutil.exe ツールによって Windows サービスとしてインストールされるようにする必要があります。 詳細については、「[方法: マネージ Windows サービスで WCF サービスをホスト](./feature-details/how-to-host-a-wcf-service-in-a-managed-windows-service.md)する」を参照してください。 マネージ Windows サービスのホストオプションによって有効になるシナリオは、メッセージがアクティブ化されていない、セキュリティで保護された環境で IIS の外部でホストされている、長時間実行される WCF サービスの場合です。 サービスの有効期限は代わりにオペレーティング システムによって制御されます。 このホスト オプションは Windows のすべてのバージョンで使用できます。

### <a name="internet-information-services-iis"></a>インターネット インフォメーション サービス (IIS)

IIS ホストオプションは ASP.NET と統合され、プロセスのリサイクル、アイドルシャットダウン、プロセスの正常性の監視、メッセージベースのアクティブ化など、これらのテクノロジによって提供される機能を使用します。 Windows XP および Windows Server 2003 オペレーティングシステムでは、これは高可用性と拡張性を必要とする Web サービスアプリケーションをホストするために推奨されるソリューションです。 IIS では、顧客がエンタープライズ クラスのサーバー製品に求める統合された管理性も提供されます。 このホスト オプションでは、IIS が正しく構成されている必要がありますが、アプリケーションの一部としてホスト コードを書く必要はありません。 WCF サービスの IIS ホストを構成する方法の詳細については、「 [how to: Host a Wcf service IN iis](./feature-details/how-to-host-a-wcf-service-in-iis.md)」を参照してください。

 IIS でホストされるサービスは、HTTP トランスポートのみを使用できます。 IIS 5.1 での実装には、Windows XP でいくつかの制限事項が導入されています。 Windows XP 上の IIS 5.1 によって WCF サービスに対して提供されるメッセージベースのアクティブ化では、同じコンピューター上の他の自己ホスト型 WCF サービスは、ポート80を使用して通信することができません。 WCF サービスは、Windows Server 2003 上の IIS 6.0 でホストされている場合、他のアプリケーションと同じ AppDomain/アプリケーションプール/ワーカープロセスで実行できます。 ただし、WCF と IIS 6.0 はどちらもカーネルモードの HTTP スタック (http.sys) を使用するため、iis 6.0 は、IIS 5.1 とは異なり、同じコンピューターで実行されている他の自己ホスト型 WCF サービスとポート80を共有できます。

### <a name="windows-process-activation-service-was"></a>Windows プロセス アクティブ化サービス (WAS)

Windows プロセスアクティブ化サービス (WAS) は、windows Vista でも使用できる Windows Server 2008 用の新しいプロセスアクティブ化メカニズムです。 使い慣れた IIS 6.0 プロセスモデル (アプリケーションプールとメッセージベースのプロセスアクティベーション) とホスト機能 (迅速な障害保護、正常性の監視、リサイクルなど) が保持されますが、アクティベーションから HTTP への依存関係は削除されます。構造. IIS 7.0 では、HTTP 経由でのメッセージベースのアクティベーションを実行するために WAS が使用されます。 また、wcf でサポートされている他のプロトコル (TCP、MSMQ、名前付きパイプなど) に対してメッセージベースのアクティベーションを提供するために、WAS にもプラグインが追加されました。 これにより、IIS のプロセスのリサイクル、迅速な障害保護、一般的な構成システムなど、これまで HTTP ベースのアプリケーションのみで利用可能だった IIS 機能を、通信プロトコルを使用するアプリケーションでも使用できるようになりました。

 このホスト オプションでは、WAS が正しく構成されている必要がありますが、アプリケーションの一部としてホスト コードを書く必要はありません。 WAS ホストを構成する方法の詳細については、「 [how to: Host a WCF Service IN was](./feature-details/how-to-host-a-wcf-service-in-was.md)」を参照してください。

## <a name="choose-a-hosting-environment"></a>ホスティング環境を選択する
 次の表に、各ホスト オプションに関連する主な利点とシナリオの要点をまとめます。

|ホスト環境|一般的なシナリオ|主な利点と制限|
|-------------------------|----------------------|----------------------------------|
|マネージド アプリケーション ("自己ホスト")|-開発中に使用されるコンソールアプリケーション。<br />-サービスにアクセスする豊富な WinForm および WPF クライアントアプリケーション。|柔軟な.<br />-デプロイが簡単です。<br />-サービスのエンタープライズソリューションではありません。|
|Windows サービス (従来 NT サービスと呼ばれていたもの)|-IIS の外部でホストされる、実行時間の長い WCF サービス。|-オペレーティングシステムによって制御される、メッセージによってアクティブ化されないサービスプロセスの有効期間。<br />-Windows のすべてのバージョンでサポートされています。<br />-セキュリティで保護された環境。|
|IIS 5.1、IIS 6.0|-HTTP プロトコルを使用して、インターネット上の ASP.NET コンテンツとサイドバイサイドで WCF サービスを実行している。|-プロセスのリサイクル。<br />-アイドル状態のシャットダウン。<br />-プロセスの正常性の監視。<br />-メッセージベースのアクティブ化。<br />-HTTP のみ。|
|Windows プロセス アクティブ化サービス (WAS)|-さまざまなトランスポートプロトコルを使用してインターネットに IIS をインストールせずに、WCF サービスを実行する。|-IIS は必要ありません。<br />-プロセスのリサイクル。<br />-アイドル状態のシャットダウン。<br />-プロセスの正常性の監視。<br />-メッセージベースのアクティブ化。<br />-HTTP、TCP、名前付きパイプ、および MSMQ と連携します。|
|IIS 7.0|-ASP.NET コンテンツで WCF サービスを実行しています。<br />-さまざまなトランスポートプロトコルを使用して、インターネット上で WCF サービスを実行しています。|-は利点がありました。<br />-ASP.NET および IIS コンテンツと統合されます。|

 ホスト環境の選択は、その展開先の Windows のバージョン、メッセージの送信に必要なトランスポート、および必要となるプロセスとアプリケーション ドメインのリサイクルの種類に依存します。 次の表に、これらの要件に関連するデータをまとめます。

|ホスト環境|プラットフォームの可用性|サポートされるトランスポート|プロセスと AppDomain のリサイクル|
|-------------------------|---------------------------|--------------------------|-------------------------------------|
|マネージド アプリケーション ("自己ホスト")|Windows XP、Windows Server 2003、Windows Vista、<br /><br /> Windows Server 2008|HTTP、<br /><br /> net.tcp、<br /><br /> net.pipe、<br /><br /> net.msmq|いいえ|
|Windows サービス (従来 NT サービスと呼ばれていたもの)|Windows XP、Windows Server 2003、Windows Vista、<br /><br /> Windows Server 2008|HTTP、<br /><br /> net.tcp、<br /><br /> net.pipe、<br /><br /> net.msmq|いいえ|
|IIS 5.1|Windows XP|HTTP|○|
|IIS 6.0|Windows Server 2003|HTTP|○|
|Windows プロセス アクティブ化サービス (WAS)|Windows Vista、Windows Server 2008|HTTP、<br /><br /> net.tcp、<br /><br /> net.pipe、<br /><br /> net.msmq|○|

 信頼されていないホストからサービスや拡張機能を実行すると、セキュリティが損なわれるので注意してください。 また、権限の借用で <xref:System.ServiceModel.ServiceHost> を開く場合、アプリケーションはユーザーの <xref:System.Security.Principal.WindowsIdentity> をキャッシュするなどして、ユーザーがログオフしていないことを確認する必要があります。

## <a name="see-also"></a>関連項目

- [基本的なプログラミング ライフサイクル](basic-programming-lifecycle.md)
- [サービス コントラクトの実装](implementing-service-contracts.md)
- [方法 : IIS で WCF サービスをホストする](./feature-details/how-to-host-a-wcf-service-in-iis.md)
- [方法 : WAS で WCF サービスをホストする](./feature-details/how-to-host-a-wcf-service-in-was.md)
- [方法 : マネージド Windows サービスで WCF サービスをホストする](./feature-details/how-to-host-a-wcf-service-in-a-managed-windows-service.md)
- [How to: Host a WCF Service in a Managed Application](how-to-host-a-wcf-service-in-a-managed-application.md)
