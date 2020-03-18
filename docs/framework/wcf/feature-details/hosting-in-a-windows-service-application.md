---
title: Windows サービス アプリケーションのホスト
ms.date: 03/30/2017
ms.assetid: f4199998-27f3-4dd9-aee4-0a4addfa9f24
ms.openlocfilehash: a07aade4619b644dadd1d5acdcb5252b305b94d0
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964491"
---
# <a name="hosting-in-a-windows-service-application"></a>Windows サービス アプリケーションのホスト
Windows サービス (従来 Windows NT サービスと呼ばれていたもの) が提供するプロセス モデルが特に適しているのは、長い期間にわたって動作し続ける必要があり、どのような形式でもユーザー インターフェイスを表示することのないアプリケーションです。 Windows サービス アプリケーションのプロセスの有効期間を管理するのは、サービス コントロール マネージャー (SCM) です。SCM を使用して、Windows サービス アプリケーションを起動、停止、および一時停止できます。 コンピューターの起動時に自動的に開始するように Windows サービスプロセスを構成し、"always on" アプリケーション用の適切なホスティング環境にすることができます。 Windows サービスアプリケーションの詳細については、「 [Windows サービスアプリケーション](https://go.microsoft.com/fwlink/?LinkId=89450)」を参照してください。  
  
 長時間実行される Windows Communication Foundation (WCF) サービスをホストするアプリケーションは、Windows サービスと多くの特性を共有します。 特に、WCF サービスは、ユーザーと直接やり取りしないため、ユーザーインターフェイスの形式を実装しない、実行時間の長いサーバー実行可能ファイルです。 そのため、Windows サービスアプリケーション内で WCF サービスをホストすることは、堅牢で長時間実行される WCF アプリケーションを構築するための1つのオプションです。  
  
 多くの場合、WCF 開発者は、WCF アプリケーションを Windows サービスアプリケーション内、またはインターネットインフォメーションサービス (IIS) または Windows プロセスアクティブ化サービス (WAS) ホスト環境の内部でホストするかどうかを決定する必要があります。 次のような場合、Windows サービス上でホストする方法を使用できないか検討してください。  
  
- アプリケーションを明示的にアクティブ化する必要がある場合。 たとえば、サーバー起動時に自動的にアプリケーションも起動しなければならず、最初に届いたメッセージに応答して動的に起動するのでは困るような場合です。  
  
- アプリケーションをホストするプロセスが、起動されたらそのまま動作し続けなければならない場合。 一度起動した Windows サービスのプロセスは、サーバー管理者がサービス コントロール マネージャーで明示的にシャットダウンしない限り、そのまま動作し続けます。 IIS や WAS 上でホストするアプリケーションは、動的に起動および停止して、システム リソースを効率よく使うことができます。 しかし、ホストするプロセスの有効期間にわたって明示的に制御しなければならない場合、IIS や WAS ではなく Windows サービスを使う必要があります。  
  
- WCF サービスは、Windows Server 2003 で実行し、HTTP 以外のトランスポートを使用する必要があります。 Windows Server 2003 では、IIS 6.0 ホスト環境は HTTP 通信のみに制限されています。 Windows サービスアプリケーションはこの制限の対象ではなく、net.tcp、net.pipe、および net.tcp を含む任意のトランスポート WCF サポートを使用できます。  
  
### <a name="to-host-wcf-inside-of-a-windows-service-application"></a>Windows サービス アプリケーションの内部で WCF をホストするには  
  
1. Windows サービス アプリケーションを作成します。 Windows サービス アプリケーションは、<xref:System.ServiceProcess> 名前空間のクラスを使用して、マネージド コードとして記述することができます。 このアプリケーションには、<xref:System.ServiceProcess.ServiceBase> から派生したクラスを 1 つ含める必要があります。  
  
2. WCF サービスの有効期間を Windows サービスアプリケーションの有効期間にリンクします。 通常、Windows サービスアプリケーションでホストされている WCF サービスは、ホスティングサービスの開始時にアクティブになり、ホスティングサービスが停止したときにメッセージのリッスンを停止し、WCF サービスでエラーが発生したときにホストプロセスをシャットダウンする必要があります。 これは次のようにして実装します。  
  
    - <xref:System.ServiceProcess.ServiceBase.OnStart%28System.String%5B%5D%29> をオーバーライドして、<xref:System.ServiceModel.ServiceHost> のインスタンスを必要な個数開くようにします。 1つの Windows サービスアプリケーションで、1つのグループとして開始および停止する複数の WCF サービスをホストできます。  
  
    - <xref:System.ServiceProcess.ServiceBase.OnStop%2A> をオーバーライドして、<xref:System.ServiceProcess.ServiceBase.OnStart%28System.String%5B%5D%29>中に開始された実行中の WCF サービス <xref:System.ServiceModel.ServiceHost> で <xref:System.ServiceModel.Channels.CommunicationObject.Closed> を呼び出します。  
  
    - <xref:System.ServiceModel.Channels.CommunicationObject.Faulted> の <xref:System.ServiceModel.ServiceHost> イベントを定期受信し、エラー時には、<xref:System.ServiceProcess.ServiceController> クラスを使用して Windows サービス アプリケーションをシャットダウンします。  
  
     Wcf サービスをホストする windows サービスアプリケーションは、WCF を使用しない Windows サービスアプリケーションと同じ方法で展開および管理されます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceProcess>
- [チュートリアル: コンポーネント デザイナーによる Windows サービス アプリケーションの作成](https://go.microsoft.com/fwlink/?LinkId=94875)
- [方法 : マネージド Windows サービスで WCF サービスをホストする](../../../../docs/framework/wcf/feature-details/how-to-host-a-wcf-service-in-a-managed-windows-service.md)
- [Windows サービス ホスト](../../../../docs/framework/wcf/samples/windows-service-host.md)
- [サービス アプリケーションのプログラミング アーキテクチャ](https://go.microsoft.com/fwlink/?LinkId=94876)
- [AppFabric のホスティング機能](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
