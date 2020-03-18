---
title: <tcpTransport>
ms.date: 03/30/2017
ms.assetid: 8fcd18c1-9958-42e7-b442-7903f7bdb563
ms.openlocfilehash: f2c1335795ffd3cb395a7006bfaeb3cf7b39636b
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77448622"
---
# <a name="tcptransport"></a>tcpTransport > の \<
カスタム バインドのメッセージを転送するためにチャネルで使用できる TCP トランスポートを定義します。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<system.servicemodel >** ](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;\<[**バインド**](bindings.md)>\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**customBinding >** ](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**バインド >** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**tcpTransport >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<tcpTransport channelInitializationTimeout="TimeSpan"
              connectionBufferSize="Integer"
              hostNameComparisonMode="StrongWildcard/Exact/WeakWildcard"
              listenBacklog="Integer"
              manualAddressing="Boolean"
              maxBufferPoolSize="Integer"
              maxBufferSize="Integer"
              maxOutputDelay="TimeSpan"
              maxPendingAccepts="Integer"
              maxPendingConnections="Integer"
              maxReceivedMessageSize="Integer"
              portSharingEnabled="Boolean"
              teredoEnabled="Boolean"
              transferMode="Buffered/Streamed/StreamedRequest/StreamedResponse" >
  <connectionPoolSettings groupName="String"
                          idleTimeout="TimeSpan"
                          leaseTimeout="TimeSpan"
                          maxOutboundConnectionsPerEndpoint="Integer" />
</tcpTransport>
```  
  
## <a name="attributes-and-elements"></a>属性と要素  
 次のセクションでは、属性、子要素、親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|channelInitializationTimeout|チャネルの初期化に対して許容される時間制限を取得または設定します。  接続が切断されるまでのチャネルの初期化ステータスの最大時間 (秒単位)。 このクォータには、TCP 接続が .NET メッセージフレーミングプロトコルを使用して自身を認証するのにかかる時間が含まれます。 クライアントは、サーバーが認証を実行するための十分な情報を得る前に初期データを送信する必要があります。 既定値は 30 秒です。|  
|connectionBufferSize|クライアントまたサービスからネットワークでシリアル化されたメッセージのチャンクを転送するために使用されるバッファーのサイズを取得または設定します。|  
|hostNameComparisonMode|URI で一致する場合にサービスに到達するためにホスト名を使用するかどうかを示す値を取得または設定します。|  
|listenBacklog|Web サービスの保留可能なキュー内の接続要求の最大数。 `connectionLeaseTimeout` 属性は、クライアントが接続されるのを待つ時間を制限します。この時間が経過すると接続の例外をスローします。 これは、Web サービスに対して保留可能なキュー内の接続要求の最大数を制御するソケット レベルのプロパティです。 ListenBacklog が低すぎると、WCF は要求の受け入れを停止し、既存のキューに置かれた接続の一部がサーバーによって確認されるまで、新しい接続を破棄します。 既定値は 16 * プロセッサの数です。|  
|manualAddressing|メッセージの手動アドレス指定が必要かどうかを示す値を取得または設定します。|  
|maxBufferPoolSize|トランスポートが使用するバッファー プールの最大サイズを取得または設定します。|  
|maxBufferSize|使用するバッファーの最大サイズを取得または設定します。 ストリーム メッセージの場合、この値は少なくともメッセージ ヘッダーで使用できる最大サイズにする必要があります。これは、バッファー モードで読み取られます。|  
|maxOutputDelay|メッセージのチャンクまたは完全なメッセージを、送信前にメモリ内のバッファーに残したままにできる最長期間を取得または設定します。|  
|maxPendingAccepts|サービスに対する着信接続処理に使用できる保留中の非同期受け入れ操作の最大数を取得または設定します。|  
|maxPendingConnections|サービスでディスパッチを待機している最大接続数を取得または設定します。|  
|maxReceivedMessageSize|受信できる最大メッセージ サイズを取得または設定します。|  
|portSharingEnabled|TCP ポート共有をこの接続で有効にする場合に指定するブール値。 これが `false` の場合、各バインディングは独自の排他ポートを使用します。 既定では、 `false`です。<br /><br /> この設定は、サービスのみに関連します。 クライアントには影響はありません。<br /><br /> この設定を使用するには、[スタートアップの種類] を [手動] または [自動] に変更して、Windows Communication Foundation (WCF) の TCP ポート共有サービスを有効にする必要があります。|  
|teredoEnabled|Teredo (ファイアウォールの内側にあるクライアントをアドレス指定するためのテクノロジ) が有効であるかどうかを指定するブール値。 既定では、 `false`です。<br /><br /> このプロパティは、基になる TCP ソケットで Tredo を有効にします。 詳細については、「 [Teredo の概要](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-xp/bb457011(v=technet.10))」を参照してください。<br /><br /> このプロパティは、Windows XP SP2 および Windows Server 2003 でのみ適用できます。 Windows Vista には、Teredo 用のコンピューター全体の構成オプションがあるため、Vista を実行する場合、このプロパティは無視されます。 Teredo の場合、クライアント コンピューターおよびサービス コンピューターの両方に Microsoft IPv6 スタックをインストールし、Teredo 用に正しく設定する必要があります。|  
|transferMode|接続指向のトランスポートでメッセージをバッファーするか、ストリーム配信するかを示す値を取得または設定します。|  
|connectionPoolSettings|名前付きパイプ バインディングの追加の接続プール設定を指定します。|  
  
### <a name="child-elements"></a>子要素  
 なし  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<バインド >](bindings.md)|カスタム バインドのすべてのバインド機能を定義します。|  
  
## <a name="remarks"></a>コメント  
 このトランスポートは、"net.tcp://hostname:port/path" の形式の URI を使用します。 他の URI コンポーネントは省略可能です。  
  
 `tcpTransport` 要素は、TCP トランスポート プロトコルを実装するカスタム バインディングを作成する場合の開始点となります。 このトランスポートは、WCF 間の通信用に最適化されています。  
  
## <a name="see-also"></a>参照

- <xref:System.ServiceModel.Configuration.TcpTransportElement>
- <xref:System.ServiceModel.Channels.TcpTransportBindingElement>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [トランスポート](../../../wcf/feature-details/transports.md)
- [トランスポートの選択](../../../wcf/feature-details/choosing-a-transport.md)
- [Bindings](../../../wcf/bindings.md)
- [バインディングの拡張](../../../wcf/extending/extending-bindings.md)
- [カスタム バインディング](../../../wcf/extending/custom-bindings.md)
- [\<customBinding >](custombinding.md)
