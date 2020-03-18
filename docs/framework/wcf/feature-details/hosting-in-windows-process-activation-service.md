---
title: Windows プロセス アクティブ化サービスでのホスティング
ms.date: 03/30/2017
helpviewer_keywords:
- hosting services [WCF], WAS
ms.assetid: d2b9d226-15b7-41fc-8c9a-cb651ac20ecd
ms.openlocfilehash: aa782c46d6530bb30055c536dd10d78f9ab9f79f
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75963773"
---
# <a name="hosting-in-windows-process-activation-service"></a>Windows プロセス アクティブ化サービスでのホスティング
Windows プロセス アクティブ化サービス (WAS) は、Windows Communication Foundation (WCF) サービスをホストするアプリケーションが含まれているワーカー プロセスのアクティブ化と有効期間を管理します。 WAS プロセス モデルは、HTTP への依存性を取り除くことで、HTTP サーバーの IIS 6.0 プロセス モデルを一般化します。 これにより、WCF サービスは、メッセージベースのアクティブ化をサポートするホスティング環境で HTTP プロトコルと非 HTTP プロトコルの両方を使用できるようになり、特定のコンピューターで多数のアプリケーションをホストできるようになります。  
  
 WAS ホスティング環境で実行される WCF サービスの構築の詳細については、「[方法: was で Wcf サービスをホスト](../../../../docs/framework/wcf/feature-details/how-to-host-a-wcf-service-in-was.md)する」を参照してください。  
  
 WAS プロセスモデルは、信頼性が高く管理も容易でリソースを効果的に使用する方法でアプリケーションのホストを実現するいくつかの機能を提供します。  
  
- HTTP と非 HTTP ネットワーク プロトコルを使用して到着する作業項目に応答して、アプリケーションやワーカー プロセス アプリケーションを動的に起動、停止する、メッセージに基づくアクティベーション。  
  
- 実行中のアプリケーションの状態を維持するための、信頼性の高いアプリケーションとワーカー プロセスのリサイクル。  
  
- 集中化されたアプリケーション設定と管理。  
  
- 完全な IIS インストールの配置スペースを必要とせずに、アプリケーションで IIS プロセス モデルを利用可能。  
[Windows Server AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ff384253(v=azure.10))は、IIS 7.0 および Windows プロセスアクティブ化サービス (WAS) と連携して、NET4 WCF および WF サービスのための豊富なアプリケーションホスティング環境を提供します。 この利点には、プロセス ライフサイクル管理、プロセス リサイクル、共有ホスティング、迅速な障害保護、プロセスの孤立化、オンデマンド アクティブ化、状態監視などがあります。 詳細については、「 [appfabric のホスティング機能](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))」と「 [appfabric のホスティングの概念](https://docs.microsoft.com/previous-versions/appfabric/ee677371(v=azure.10))」を参照してください。  
  
## <a name="elements-of-the-was-addressing-model"></a>WAS アドレス指定モデルの要素  
 アプリケーションには、サーバーによって有効期間と実行環境が管理されているコード単位である URI (Uniform Resource Identifier) アドレスがあります。 1 つの WAS サーバー インスタンスを多数の異なるアプリケーションでホームとすることができます。 サーバーは、アプリケーションを*サイト*と呼ばれるグループに編成します。 サイト内でアプリケーションは階層で整理されます。この階層は URI の構造に反映されてアプリケーションの外部アドレスとして提供されます。  
  
 アプリケーション アドレスは、ベース URI プレフィックスとアプリケーション固有の相対アドレス (パス) の 2 つの部分に分かれます。この 2 つの部分が結合されアプリケーションの外部アドレスが提供されます。 ベース URI プレフィックスは、サイト バインドで構築され、サイト内のすべてのアプリケーションで使用されます。 アプリケーションアドレスは、アプリケーション固有のパスフラグメント ("/applicationone" など) を取得し、それらをベース URI プレフィックス (例: "net.tcp:/localhost") に追加することによって作成され、完全なアプリケーション URI に到達します。  
  
 HTTP と 非 HTTP サイト バインディングの両方の WAS サイト用に考えられるアドレス シナリオを次の表に示します。  
  
|通信の種類|サイトのバインド|アプリケーション パス|ベース アプリケーション URI|  
|--------------|-------------------|----------------------|---------------------------|  
|HTTP のみ|http: *:80:\*|/appTwo|http://localhost/appTwo/|  
|HTTP と 非 HTTP の混在|http: *:80:\*<br /><br /> net.tcp: 808:\*|/appTwo|http://localhost/appTwo/<br />net.tcp://localhost/appTwo/|  
|非 HTTP のみ|net.pipe: *|/appThree|net.pipe://appThree/|  
  
 アプリケーション内のサービスとリソースにもアドレスを指定できます。 アプリケーション内では、アプリケーション リソースにベース アプリケーション パスに対する相対アドレスが指定されます。 たとえば、コンピューター名 contoso.com のサイトに HTTP と Net.TCP プロトコルの両方のサイト バインドがあるとします。 さらに、そのサイトには 1 つのアプリケーションが /Billing に格納されており、GetOrders.svc でサービスを公開しているとします。 このとき、GetOrders.svc サービスで SecureEndpoint の相対アドレスを持つエンドポイントが公開されている場合、サービスのエンドポイントは次の 2 つの URI で公開されることになります。  
  
- `http://contoso.com/Billing/GetOrders.svc/SecureEndpoint`
- `net.tcp://contoso.com/Billing/GetOrders.svc/SecureEndpoint`
  
## <a name="the-was-runtime"></a>WAS ランタイム  
 アプリケーションは、アドレス指定と管理の目的でサイトに編成されます。 実行時にもアプリケーションはアプリケーション プールにグループ化されます。 アプリケーション プールには、多数の異なるサイトからの多数の異なるアプリケーションを格納できます。 アプリケーション プール内のすべてのアプリケーションで、一連の共通の実行時特性を共有します。 たとえば、すべてのアプリケーションは同じバージョンの共通言語ランタイム (CLR) 下で実行され、またすべてのアプリケーションで共通のプロセス ID を共有します。 各アプリケーション プールはワーカー プロセス (w3wp.exe) のインスタンスに対応します。 共有アプリケーション プール内で実行される各マネージド アプリケーションは、CLR AppDomain により他のアプリケーションから分離されます。  
  
## <a name="see-also"></a>関連項目

- [WAS アクティベーション アーキテクチャ](../../../../docs/framework/wcf/feature-details/was-activation-architecture.md)
- [WCF で使用するための WAS を設定する](../../../../docs/framework/wcf/feature-details/configuring-the-wpa--service-for-use-with-wcf.md)
- [方法 : WCF アクティブ化コンポーネントをインストールして設定する](../../../../docs/framework/wcf/feature-details/how-to-install-and-configure-wcf-activation-components.md)
- [方法 : WAS で WCF サービスをホストする](../../../../docs/framework/wcf/feature-details/how-to-host-a-wcf-service-in-was.md)
- [AppFabric のホスティング機能](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
