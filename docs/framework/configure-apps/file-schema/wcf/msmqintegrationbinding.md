---
title: <msmqIntegrationBinding>
ms.date: 03/30/2017
helpviewer_keywords:
- msmqIntegrationBinding Element
ms.assetid: edf277f3-e3bf-4ed8-9f55-83b5788430a7
ms.openlocfilehash: ba28a81dd2ea0684ed863821afd3a8f31c0fb064
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74140770"
---
# <a name="msmqintegrationbinding"></a>\<msmqIntegrationBinding >
MSMQ を介してメッセージをルーティングすることでキューのサポートを提供するバインディングを定義します。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp; &nbsp;[ **\<system >** ](system-servicemodel.md) \
&nbsp;&nbsp;&nbsp;&nbsp;\<[**バインド**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**customBinding >** ](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**バインド >** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**msmqIntegrationBinding >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<msmqIntegrationBinding>
  <binding closeTimeout="TimeSpan"
           customDeadLetterQueue="Uri"
           deadLetterQueue="Uri"
           durable="Boolean"
           exactlyOnce="Boolean"
           maxReceivedMessageSize="Integer"
           maxRetryCycles="Integer"
           name="String"
           openTimeout="TimeSpan"
           receiveContextEnabled="Boolean"
           receiveErrorHandling="Drop/Fault/Move/Reject"
           receiveTimeout="TimeSpan"
           receiveRetryCount="Integer"
           retryCycleDelay="TimeSpan"
           sendTimeout="TimeSpan"
           serializationFormat="XML/Binary/ActiveX/ByteArray/Stream"
           timeToLive="TimeSpan"
           useMsmqTracing="Boolean"
           useSourceJournal="Boolean">
  </binding>
</msmqIntegrationBinding>
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|closeTimeout|クローズ操作が完了するまでの期間を指定する <xref:System.TimeSpan> 値。 この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。 既定値は 00:01:00 です。|  
|customDeadLetterQueue|アプリケーションごとの配信不能キューの場所が含まれている URI です。ここには、期限切れのメッセージや、転送または配信に失敗したメッセージが配置されます。<br /><br /> 配信不能キューは、送信元アプリケーションのキュー マネージャーにある、配信に失敗した期限切れメッセージのキューです。<br /><br /> <xref:System.ServiceModel.MsmqBindingBase.CustomDeadLetterQueue%2A> によって指定される URI は、net.msmq スキームを使用する必要があります。|  
|deadLetterQueue|使用する配信不能キューがある場合にその種類を指定する <xref:System.ServiceModel.MsmqBindingBase.DeadLetterQueue%2A> 値。<br /><br /> 配信不能キューは、アプリケーションへの配信に失敗したメッセージが転送される場所です。<br /><br /> exactlyOnce 保証が必要なメッセージ (つまり、`exactlyOnce` 属性が `true` に設定される) の場合、この属性は、既定で MSMQ のトランザクション システム全体の配信不能キューになります。<br /><br /> 保証が必要ないメッセージの場合、この属性の既定値は `null` です。|  
|durable|メッセージがキューで非揮発性か揮発性かを示すブール値です。 非揮発性メッセージは、キュー マネージャーがクラッシュしても残り、揮発性メッセージは失われます。 アプリケーションで待ち時間の短縮が要求され、場合によってはメッセージが失われてもかまわない場合は、揮発性メッセージが適しています。 `exactlyOnce` 属性が `true` に設定されている場合、メッセージは永続的にする必要があります。 既定値は、 `true`です。|  
|exactlyOnce|各メッセージが 1 回だけ受信されるかどうかを示すブール値です。 その後、送信側に配信エラーが通知されます。 `durable` が `false` の場合、この属性は無視されて、配信が保証されずにメッセージが転送されます。 既定値は、 `true`です。 詳細については、「<xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A>」を参照してください。|  
|maxReceivedMessageSize|このバインディングにより処理される最大メッセージ サイズ (ヘッダーを含む) をバイト単位で定義する正の整数です。 この制限を超えるメッセージの送信者が、SOAP エラーを受信します。 メッセージは受信者によってドロップされ、トレース ログにこのイベントのエントリが作成されます。 既定値は 65536 です。 このメッセージ サイズの制限は、サービス拒否 (DoS) 攻撃への露出を制限するためのものです。|  
|maxRetryCycles|有害メッセージ検出機能により使用される再試行サイクルの回数を示す整数です。 すべてのサイクルの配信試行にすべて失敗すると、メッセージは有害メッセージになります。 既定値は 2 です。 詳細については、「<xref:System.ServiceModel.MsmqBindingBase.MaxRetryCycles%2A>」を参照してください。|  
|name|バインディングの構成名を格納する文字列です。 この値は、バインディングの ID として使用されるため、一意にする必要があります。 .NET Framework 4 以降では、バインドと動作に名前を付ける必要はありません。 既定の構成と無名のバインドおよび動作の詳細については、「 [WCF サービスの](../../../wcf/samples/simplified-configuration-for-wcf-services.md)構成と簡略化された構成の[簡略化](../../../wcf/simplified-configuration.md)」を参照してください。|  
|openTimeout|実行中の操作が完了するまでの時間間隔を指定する <xref:System.TimeSpan> 値です。 この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。 既定値は 00:01:00 です。|  
|receiveErrorHandling|有害メッセージおよびディスパッチ不能メッセージの処理方法を指定する <xref:System.ServiceModel.ReceiveErrorHandling> 値。|  
|receiveRetryCount|アプリケーション キューからアプリケーションへのメッセージの転送が失敗した場合に、キュー マネージャーが即時再試行を行う最大回数を指定する整数。<br /><br /> 配信試行を最大回数実行してもアプリケーションがメッセージにアクセスできない場合、メッセージは、後で再配信するために再試行キューに送信されます。 メッセージが送信キューに戻されるまでの時間は、`retryCycleDelay` で制御されます。 再試行サイクルが `maxRetryCycles` 値に達した場合は、メッセージが有害メッセージ キューに送信されるか、送信者に否定応答が返されます。|  
|receiveTimeout|受信操作が完了するまでの時間間隔を指定する <xref:System.TimeSpan> 値です。 この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。 既定値は 00:10:00 です。|  
|receiveContextEnabled|キュー内のメッセージを処理するための受信コンテキストが有効になっているかどうかを指定するブール値。 これが `true`に設定されている場合、サービスはキュー上のメッセージを "ピーク" して処理を開始することができます。問題が発生して例外がスローされた場合は、キューに残ります。 また、サービスは、後で処理を再試行するためにメッセージを "ロック" することもできます。 ReceiveContext には、処理されたメッセージを "完了" してキューから削除できるようにするメカニズムが用意されています。ネットワーク上のキューへのメッセージの読み取りと再書き込みは行われなくなり、個々のメッセージは処理中に異なるサービスインスタンス間でバウンスされることはありません。|  
|retryCycleDelay|すぐに配信できなかったメッセージを配信しようとするときの、再試行サイクルの時間遅延を指定する TimeSpan 値です。 実際の待機時間はさらに長くなる場合があるため、この値で定義されるのは最小待機時間だけです。 既定値は、00:30:00 です。 詳細については、「<xref:System.ServiceModel.MsmqBindingBase.RetryCycleDelay%2A>」を参照してください。|  
|sendTimeout|送信操作が完了するまでの時間間隔を指定する <xref:System.TimeSpan> 値です。 この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。 既定値は 00:01:00 です。|  
|serializationFormat|メッセージ本文のシリアル化に使用される形式を定義します。 この属性は <xref:System.ServiceModel.MsmqIntegration.MsmqMessageSerializationFormat> 型です。|  
|timeToLive|メッセージの期限が切れて、配信不能キューに入れられるまでのメッセージの有効期間を指定する TimeSpan 値です。 既定値は 1.00:00:00 です。<br /><br /> この属性を設定すると、タイムリーなメッセージが受信側アプリケーションで処理される前に古くなることがなくなります。 キュー内のメッセージのうち、指定された期間内に受信アプリケーションで処理されなかったメッセージは、期限切れと呼ばれます。 期限切れのメッセージは、配信不能キューと呼ばれる特別なキューに送信されます。 配信不能キューの場所は、保証の内容に基づいて、`DeadLetterQueue` 属性を使用して設定されるか、適切な既定値に設定されます。|  
|useMsmqTracing|このバインディングにより処理されるメッセージをトレースするかどうかを指定するブール値です。 既定値は、 `false`です。 トレースが有効な場合、メッセージ キュー コンピューターでメッセージが送受信されるたびに、レポート メッセージが作成され、レポート キューに送信されます。|  
|useSourceJournal|このバインディングにより処理されるメッセージのコピーをソース ジャーナルに保存するかどうかを指定するブール値です。 既定値は、 `false`です。<br /><br /> キューに置かれたアプリケーションでは、コンピューターの発信キューから送信されたメッセージの記録を残す場合は、メッセージをジャーナル キューにコピーできます。 メッセージが発信キューから送信され、送信先のコンピューターで受信されたという応答を受け取ると、メッセージのコピーが送信元のコンピューターのシステム ジャーナル キューに保持されます。|  
  
## <a name="serializationformat-attribute"></a>{SerializationFormat} 属性  
  
|[値]|説明|  
|-----------|-----------------|  
|Xml|XML 形式|  
|2 項|バイナリ形式|  
|ActiveX|ActiveX 形式|  
|ByteArray|オブジェクトをバイト配列にシリアル化します。|  
|ストリーム|ストリームとして書式設定された本文。|  
  
### <a name="child-elements"></a>子要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<セキュリティ >](security-of-msmqintegrationbinding.md)|バインディングのセキュリティ設定を定義します。 この要素は <xref:System.ServiceModel.Configuration.MsmqIntegrationSecurityElement> 型です。|  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<バインド >](bindings.md)|この要素には、標準バインディングおよびカスタム バインドのコレクションが保持されます。|  
  
## <a name="remarks"></a>Remarks  
 このバインド要素を使用すると、Windows Communication Foundation (WCF) アプリケーションが、COM、MSMQ ネイティブ Api、または使用可能な <xref:System.Messaging?displayProperty=nameWithType> 名前空間で定義されている型を使用する既存の MSMQ アプリケーションとメッセージを送受信できるようになります。この構成要素を使用して、キューのアドレス指定方法、転送の保証、メッセージを永続的にに保存する必要があるかどうか、およびメッセージを保護および認証する方法を指定します。 詳細については、「[方法: WCF エンドポイントおよびメッセージキューアプリケーションを使用](../../../wcf/feature-details/how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications.md)してメッセージを交換する」を参照してください。  
  
## <a name="example"></a>例  
  
```xml  
<configuration>
  <system.ServiceModel>
    <bindings>
      <msmqIntegrationBinding>
        <binding closeTimeout="00:00:10"
                 openTimeout="00:00:20"
                 receiveTimeout="00:00:30"
                 sendTimeout="00:00:40"
                 deadLetterQueue="net.msmq://localhost/blah"
                 durable="true"
                 exactlyOnce="true"
                 maxReceivedMessageSize="1000"
                 maxImmediateRetries="11"
                 maxRetryCycles="12"
                 poisonMessageHandling="Disabled"
                 rejectAfterLastRetry="false"
                 retryCycleDelay="00:05:55"
                 timeToLive="00:11:11"
                 useSourceJournal="true"
                 useMsmqTracing="true"
                 serializationFormat="Binary">
          <security mode="None" />
        </binding>
      </msmqIntegrationBinding>
    </bindings>
  </system.ServiceModel>
</configuration>
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Configuration.MsmqIntegrationBindingElement>
- <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>
- <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBindingElement>
- [\<バインド >](bindings.md)
- [バインディング](../../../wcf/bindings.md)
- [システムが提供するバインディングの構成](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [サービスとクライアントを構成するためのバインディングの使用](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [WCF のキュー](../../../wcf/feature-details/queues-in-wcf.md)
