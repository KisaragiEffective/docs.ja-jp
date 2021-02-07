---
description: 詳細については、「WCF 分析トレース」を参照してください。
title: WCF 分析トレース
ms.date: 03/30/2017
ms.assetid: 6029c7c7-3515-4d36-9d43-13e8f4971790
ms.openlocfilehash: 1f5ec26828bba99a127fea6a81f57fed717943a2
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99755695"
---
# <a name="wcf-analytic-tracing"></a>WCF 分析トレース

このサンプルでは、の ETW への書き込みを Windows Communication Foundation (WCF) する分析トレースのストリームに独自のトレースイベントを追加する方法を示し [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] ます。 分析トレースは、パフォーマンスを低下させずに簡単にサービスを確認できるようにするためのものです。 このサンプルでは、api を使用して、 <xref:System.Diagnostics.Eventing?displayProperty=nameWithType> WCF サービスと統合するイベントを作成する方法を示します。  
  
 Api の詳細については <xref:System.Diagnostics.Eventing?displayProperty=nameWithType> 、「」を参照してください <xref:System.Diagnostics.Eventing?displayProperty=nameWithType> 。  
  
 Windows のイベントトレースの詳細については、「 [ETW を使用したデバッグとパフォーマンスチューニングの向上](/archive/msdn-magazine/2007/april/event-tracing-improve-debugging-and-performance-tuning-with-etw)」を参照してください。  
  
## <a name="disposing-eventprovider"></a>EventProvider の破棄  

 このサンプルでは、<xref:System.Diagnostics.Eventing.EventProvider?displayProperty=nameWithType> を実装した <xref:System.IDisposable?displayProperty=nameWithType> クラスを使用します。 WCF サービスのトレースを実装する場合、 <xref:System.Diagnostics.Eventing.EventProvider> サービスの有効期間にリソースを使用する可能性があります。 そのため、読みやすくするためにも、このサンプルでは、ラップされた <xref:System.Diagnostics.Eventing.EventProvider> を破棄しません。 何かの理由で、サービスに対して別のトレースの要件を設定し、このリソースを破棄しなければならない場合は、アンマネージ リソースの破棄に関するベスト プラクティスに従ってこのサンプルを変更してください。 アンマネージリソースの破棄の詳細については、「 [Dispose メソッドの実装](../../../standard/garbage-collection/implementing-dispose.md)」を参照してください。  
  
## <a name="self-hosting-vs-web-hosting"></a>自己ホスト型と Web ホスト型  

 Web ホストサービスの場合、WCF の分析トレースでは、トレースを出力するサービスを識別するために使用される "HostReference" というフィールドが提供されます。 拡張可能なユーザー トレースをこのモデルに加えることができます。このサンプルで、そのためのベスト プラクティスを示します。 結果の文字列にパイプの ' &#124; ' 文字が実際に表示されている場合の Web ホスト参照の形式は、次のいずれかになります。  
  
- アプリケーションがルート以外にある場合  
  
     \<SiteName>\<ApplicationVirtualPath>&#124;\<ServiceVirtualPath>&#124;\<ServiceName>  
  
- アプリケーションがルートにある場合  
  
     \<SiteName>&#124;\<ServiceVirtualPath>&#124;\<ServiceName>  
  
 自己ホスト型サービスの場合、WCF の分析トレースでは、"HostReference" フィールドは設定されません。 このサンプルの `WCFUserEventProvider` クラスは、自己ホスト型サービスで使用した場合も同じように動作します。  
  
## <a name="custom-event-details"></a>カスタム イベントの詳細  

 WCF の ETW イベントプロバイダーマニフェストは、サービスコード内から WCF サービス作成者によって出力されるように設計された3つのイベントを定義します。 次の表に、その 3 つのイベントの概要を示します。  
  
|event|説明|イベント ID|  
|-----------|-----------------|--------------|  
|UserDefinedInformationEventOccurred|このイベントは、問題以外の通知すべき処理がサービスで発生した場合に生成します。 たとえば、データベースの呼び出しに成功した後にイベントを生成します。|301|  
|UserDefinedWarningOccurred|このイベントは、後続の処理でエラーになる可能性がある問題が発生した場合に生成します。 たとえば、データベースの呼び出しが失敗したものの、冗長なデータ ストアを使用して回復できた場合に警告イベントを生成します。|302|  
|UserDefinedErrorOccurred|このイベントは、サービスが想定どおりに動作しなかった場合に生成します。 たとえば、データベースの呼び出しが失敗し、別の場所からもデータを取得できなかった場合にイベントを生成します。|303|  
  
#### <a name="to-use-this-sample"></a>このサンプルを使用するには  
  
1. Visual Studio 2012 を使用して、WCFAnalyticTracingExtensibility ソリューションファイルを開きます。  
  
2. ソリューションをビルドするには、Ctrl キーと Shift キーを押しながら B キーを押します。  
  
3. ソリューションを実行するには、Ctrl キーを押しながら F5 キーを押します。  
  
     Web ブラウザーで、[ **Calculator .svc**] をクリックします。 サービスの WSDL ドキュメントの URI がブラウザーに表示されます。 その URI をコピーします。  
  
4. WCF テストクライアント (WcfTestClient.exe) を実行します。  
  
     WCF テストクライアント (WcfTestClient.exe) は、にあり `\<Visual Studio 2012 Install Dir>\Common7\IDE\WcfTestClient.exe` ます。 既定の Visual Studio 2012 インストールディレクトリは `C:\Program Files\Microsoft Visual Studio 10.0` です。  
  
5. WCF テストクライアント内で、[ **ファイル**] を選択し、[ **サービスの追加**] をクリックしてサービスを追加します。  
  
     入力ボックスにエンドポイントのアドレスを追加します。  
  
6. [**OK**] をクリックしてダイアログ ボックスを閉じます。  
  
     ICalculator サービスは、左側のウィンドウの [ **マイサービスプロジェクト**] の下に追加されます。  
  
7. イベント ビューアー アプリケーションを開きます。  
  
     サービスを呼び出す前に、イベントビューアーを開始し、WCF サービスから生成された追跡イベントをイベントログがリッスンしていることを確認します。  
  
8. [ **スタート** ] メニューから [ **管理ツール**] を選択し、 **イベントビューアー**] をクリックします。 **分析** ログと **デバッグ** ログを有効にします。  
  
9. イベントビューアーのツリービューで、[ **イベントビューアー**]、[ **アプリケーションとサービスログ**]、[ **Microsoft**]、[ **Windows**]、[ **アプリケーションサーバー-アプリケーション**] の順に移動します。 [ **アプリケーションサーバー-アプリケーション**] を右クリックし、[ **表示**] をクリックして、[ **分析およびデバッグログ] を表示** します。  
  
     [ **分析とデバッグログを表示** する] オプションがオンになっていることを確認します。 **分析** ログを有効にします。  
  
     イベントビューアーのツリービューで、[ **イベントビューアー**]、[ **アプリケーションとサービスログ**]、[ **Microsoft**]、[ **Windows**]、[ **アプリケーションサーバー-アプリケーション**]、[ **分析**] の順に移動します。 [ **分析** ] を右クリックし、[ **ログを有効にする**] を選択します。  
  
10. WCF テスト クライアントを使用してサービスをテストします。  
  
    1. WCF テストクライアントで、[ICalculator service] ノードの下の [ **Add ()** ] をダブルクリックします。  
  
         **Add ()** メソッドが、右側のペインに2つのパラメーターと共に表示されます。  
  
    2. 最初のパラメーターに「2」と入力し、2 番目のパラメーターに「3」と入力します。  
  
    3. [ **呼び出し** ] をクリックしてメソッドを呼び出します。  
  
11. 既に開いている **イベントビューアー** ウィンドウにアクセスします。 [ **イベントビューアー**]、[ **アプリケーションとサービスログ**]、[ **Microsoft**]、[ **Windows**]、[ **アプリケーションサーバー-アプリケーション**] の順に移動します。  
  
12. [ **分析** ] ノードを右クリックし、[ **更新**] を選択します。  
  
     右ペインにイベントが表示されます。  
  
13. ID が 303 のイベントを探してダブルクリックして開き、内容を確認します。  
  
     このイベントは、ICalculator サービスのメソッドによって生成され、 `Add()` "2 + 3 = 5" に等しいペイロードがあります。  
  
#### <a name="to-clean-up-optional"></a>クリーンアップするには (省略可能)  
  
1. **イベント ビューアー** を開きます。  
  
2. [ **イベントビューアー**]、[ **アプリケーションとサービスログ**]、[ **Microsoft**]、[ **Windows**]、[ **アプリケーション-サーバー-アプリケーション**] の順に移動します。 [ **分析** ] を右クリックし、[ **ログを無効にする**] を選択します。  
  
3. [ **イベントビューアー**]、[ **アプリケーションとサービスログ**]、[ **Microsoft**]、[ **Windows**]、[ **アプリケーション-サーバー-アプリケーション**]、[ **分析**] の順に移動します。 [ **分析** ] を右クリックし、[ **ログの消去**] を選択します。  
  
4. [ **クリア** ] をクリックすると、イベントがクリアされます。  
  
## <a name="known-issue"></a>既知の問題  

 **イベントビューアー** には、ETW イベントのデコードに失敗する可能性がある既知の問題があります。 次のエラーメッセージが表示されることがあります。 "ソースからのイベント ID の説明" \<id> Server-Applications が見つかりません。 このイベントを発生させるコンポーネントがローカル コンピューターにインストールされていないか、インストールが壊れています。 ローカルコンピューターにコンポーネントをインストールまたは修復できます。 " このエラーが発生した場合は、[**アクション**] メニューの [**更新**] を選択します。 これにより、イベントが正常にデコードされます。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) とサンプルをダウンロードして [!INCLUDE[wf1](../../../../includes/wf1-md.md)] ください。 このサンプルは、次のディレクトリに格納されます。  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\ETWTrace`  
  
## <a name="see-also"></a>関連項目

- [AppFabric の監視のサンプル](/previous-versions/appfabric/ff383407(v=azure.10))
