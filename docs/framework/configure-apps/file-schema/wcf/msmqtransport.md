---
title: <msmqTransport>
ms.date: 03/30/2017
ms.assetid: 19d89f35-76ac-49dc-832b-e8bec2d5e33b
ms.openlocfilehash: fae7c9fbc82dafc0f6be58f5404397d751033b45
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73738855"
---
# <a name="msmqtransport"></a>\<msmqTransport >
チャネルがカスタム バインドに含まれているときに、MSMQ トランスポートでメッセージを転送するようにします。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp; &nbsp;[ **\<system >** ](system-servicemodel.md) \
&nbsp;&nbsp;&nbsp;&nbsp;\<[**バインド**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**customBinding >** ](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**バインド >** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**msmqTransport >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<msmqTransport customDeadLetterQueue="Uri"
               deadLetterQueue="Custom/None/System"
               durable="Boolean"
               exactlyOnce="Boolean"
               manualAddressing="Boolean"
               maxBufferPoolSize="Integer"
               maxImmediateRetries="Integer"
               maxPoolSize="Integer"
               maxReceivedMessageSize="Integer"
               maxRetryCycles="Integer"
               queueTransferProtocol="Native/Srmp/SrmpSecure"
               rejectAfterLastRetry="Boolean"
               retryCycleDelay="TimeSpan"
               timeToLive="TimeSpan"
               useActiveDirectory="Boolean"
               useSourceJournal="Boolean"
               useMsmqTracing="Boolean"
               ...>
  <msmqTransportSecurity>
  </msmqTransportSecurity>
</msmqTransport>
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|customDeadLetterQueue|アプリケーションごとの配信不能キューの場所を示す URI。ここには、期限切れのメッセージや、アプリケーションへの配信に失敗したメッセージが転送されます。<br /><br /> ExactlyOnce 保証が必要なメッセージ (つまり、`exactlyOnce` が `true` に設定される) の場合、この属性は、既定で MSMQ のトランザクション システム全体の配信不能キューになります。<br /><br /> 保証が必要ないメッセージ (つまり、`exactlyOnce` が `false` に設定される) の場合、この属性の既定値は `null` です。<br /><br /> 値は、net.msmq スキームを使用する必要があります。 既定値は、 `null`です。<br /><br /> `deadLetterQueue` が `None` または `System` に設定されている場合は、この属性を `null` に設定する必要があります。 この属性が `null` でない場合は、`deadLetterQueue` を `Custom` に設定する必要があります。|  
|deadLetterQueue|使用する配信不能キューの種類を指定します。<br /><br /> 有効な値を次に示します。<br /><br /> -Custom: Custom 配信不能 queue。<br />-None: 配信不能キューは使用されません。<br />-System: system 配信不能 queue を使用します。<br /><br /> この属性は、DeadLetterQueue 型です。|  
|durable|このバインディングで処理されるメッセージが非揮発性か揮発性かを指定するブール値。 既定値は、 `true`です。<br /><br /> 非揮発性メッセージは、キュー マネージャーがクラッシュしても残り、揮発性メッセージは失われます。 アプリケーションで待ち時間の短縮が要求され、場合によってはメッセージが失われてもかまわない場合は、揮発性メッセージが適しています。<br /><br /> `exactlyOnce` が `true` に設定されている場合、メッセージは非揮発性である必要があります。|  
|exactlyOnce|このバインディングで処理されるメッセージが正確に 1 回だけ受信されるかどうかを指定するブール値。 既定値は、 `true`です。<br /><br /> メッセージは、保証付きまたは保証なしで送信できます。 保証により、アプリケーションは、送信したメッセージが受信メッセージ キューに到達したことを確認できます。到達しなかった場合、アプリケーションは、配信不能キューを読み取ることでそれを判断できます。<br /><br /> `exactlyOnce` は、`true` に設定されている場合、送信されたメッセージが受信メッセージ キューに 1 回だけ配信され、送信に失敗した場合はメッセージが配信不能キューに送信されることを MSMQ が保証することを示します。<br /><br /> `exactlyOnce` を `true` に設定して送信されるメッセージを、トランザクション キューだけに送信する必要があります。|  
|manualAddressing|ユーザーによるメッセージのアドレス指定の管理を有効にするブール値です。 このプロパティは、通常、アプリケーションが複数の宛先のどれにメッセージを送信するかを決定するルーターのシナリオで使用されます。<br /><br /> `true` に設定されている場合、チャネルではメッセージが既にアドレス指定されていると見なされ、他の情報は追加されません。 この場合、ユーザーはすべてのメッセージを別個にアドレス指定できます。<br /><br /> `false` に設定されている場合、既定の Windows Communication Foundation (WCF) アドレス指定機構により、すべてのメッセージのアドレスが自動的に作成されます。<br /><br /> 既定値は、 `false`です。|  
|maxBufferPoolSize|バッファー プールの最大サイズを指定する正の整数です。 既定値は 524288 です。<br /><br /> WCF の多くの部分でバッファーが使用されます。 使用するたびに毎回バッファーを作成および破壊すると負荷が高くなります。バッファーのガベージ コレクションも同様です。 バッファー プールを使用すると、バッファーをプールから取得して使用し、作業が終わったらプールに戻すことができます。 これで、バッファーの作成と破棄のオーバーヘッドを回避できます。|  
|maxImmediateRetries|アプリケーションキューから読み取られるメッセージの即時再試行の最大回数を指定する整数です。 既定値は 5 です。<br /><br /> メッセージの即時再試行を最大回数実行してもアプリケーションでメッセージを処理できない場合、メッセージは、後で再試行するために再試行キューに送信されます。 再試行サイクルが指定されていない場合は、メッセージが有害メッセージ キューに送信されるか、送信者に否定応答が返されます。|  
|maxPoolSize|プールの最大サイズを指定する正の整数です。 既定値は 524288 です。|  
|maxReceivedMessageSize|ヘッダーなどのメッセージの最大サイズ (バイト単位) を指定する正の整数。 受信側にとってメッセージが大きすぎると、メッセージの送信側は SOAP エラーを受け取ります。 メッセージは受信者によってドロップされ、トレース ログにこのイベントのエントリが作成されます。 既定値は 65536 です。|  
|maxRetryCycles|受信側アプリケーションにメッセージを配信する再試行サイクルの最大数を指定する整数。 既定値は、 <xref:System.Int32.MaxValue>です。<br /><br /> 1 回の再試行サイクルで、アプリケーションに、指定された回数のメッセージ配信を試みます。 試行回数は、`maxImmediateRetries` 属性で設定します。 配信を指定された回数試行してもアプリケーションがメッセージを処理できない場合、メッセージは、再試行キューに送信されます。 これに続く再試行サイクルは、`retryCycleDelay` 属性により指定された遅延の後、アプリケーションへの配信を再度試行するアプリケーション キューへの再試行キューから返されるメッセージで構成されています。 `maxRetryCycles` 属性は、アプリケーションがメッセージ配信に使用する再試行サイクル数を指定します。|  
|queueTransferProtocol|このバインドが使用する、キューに置かれている通信チャネルのトランスポートを指定します。 有効な値は、次のとおりです。<br /><br /> -Native: ネイティブ MSMQ プロトコルを使用します。<br />-Srmp: Soap Reliable Messaging プロトコル (SRMP) を使用します。<br />-SrmpSecure: Soap Reliable Messaging Protocol Secure (SRMP) トランスポートを使用します。<br /><br /> この属性は <xref:System.ServiceModel.QueueTransferProtocol> 型です。<br /><br /> `useActiveDirectory` が `true`に設定されている場合、MSMQ は Active Directory アドレス指定をサポートしていないため、この属性を Srmp または Srmps に設定しないでください。|  
|rejectAfterLastRetry|再試行を最大回数実行しても配信できなかったメッセージに対して実行するアクションを指定するブール値。<br /><br /> `true` は、否定応答が送信者に返され、メッセージが削除されることを示します。`false` は、メッセージが有害メッセージ キューに送信されることを示します。 既定値は、 `false`です。<br /><br /> 値が `false` の場合、受信アプリケーションは、有害メッセージ キューを読み取って、有害メッセージ (配信に失敗したメッセージ) を処理できます。<br /><br /> MSMQ 3.0 は送信者への否定応答の返信をサポートしていないので、この属性は、MSMQ 3.0 では無視されます。|  
|retryCycleDelay|すぐに配信できなかったメッセージを配信しようとするときの、再試行サイクルの時間遅延を指定する <xref:System.TimeSpan>。 既定値は 00:10:00 です。<br /><br /> 1 回の再試行サイクルで、受信アプリケーションに、メッセージ配信を指定回試みます。 試行回数は、`maxImmediateRetries` 属性で指定されます。 即時試行を指定された回数実行してもアプリケーションがメッセージを処理できない場合、メッセージは再試行キューに送信されます。 これに続く再試行サイクルは、`retryCycleDelay` 属性により指定された遅延の後、アプリケーションへの配信を再度試行するアプリケーション キューへの再試行キューから返されるメッセージで構成されています。 再試行サイクル数は、`maxRetryCycles` 属性で指定されます。|  
|timeToLive|メッセージの期限が切れて、配信不能キューに入れられるまでのメッセージの有効期間を指定する <xref:System.TimeSpan>。 既定値は 1 日を示す 1.00:00:00 です。<br /><br /> この属性を設定すると、タイムリーなメッセージが受信側アプリケーションで処理される前に古くなることがなくなります。 キュー内のメッセージのうち、指定された期間内に受信アプリケーションで処理されなかったメッセージは、期限切れと呼ばれます。 期限切れのメッセージは、配信不能キューと呼ばれる特別なキューに送信されます。 配信不能キューの場所は、保証の内容に基づいて、`customDeadLetterQueue` 属性を使用して設定されるか、適切な既定値に設定されます。|  
|UseActiveDirectory|キューのアドレスを Active Directory を使用して変換する必要があるかどうかを指定するブール値。<br /><br /> MSMQ キューのアドレスは、パス名または直接形式名で構成できます。 直接形式名を使用する場合、MSMQ は、DNS、NetBIOS、または IP を使用してコンピューター名を解決します。 パス名を使用する場合、MSMQ は、Active Directory を使用してコンピューター名を解決します。 既定では、Windows Communication Framework (WCF) キューに置かれているトランスポートは、メッセージ キューの URI を直接形式名に変換します。 この属性を `true` に設定すると、キューに置かれているトランスポートが DNS、NetBIOS、または IP ではなく Active Directory を使用してコンピューター名を解決する必要があることを、アプリケーションで指定できます。|  
|useMsmqTracing|このバインディングにより処理されるメッセージをトレースするかどうかを指定するブール値です。 既定値は、 `false`です。<br /><br /> トレースが有効な場合、メッセージ キュー コンピューターでメッセージが送受信されるたびに、レポート メッセージが作成され、レポート キューに送信されます。|  
|useSourceJournal|このバインディングにより処理されるメッセージのコピーをソース ジャーナル キューに保存するかどうかを指定するブール値。 既定値は、 `false`です。<br /><br /> キューに置かれたアプリケーションでは、コンピューターの発信キューから送信されたメッセージの記録を残す場合は、メッセージをジャーナル キューにコピーできます。 メッセージが発信キューから送信され、送信先のコンピューターで受信されたという応答を受け取ると、メッセージのコピーが送信元のコンピューターのシステム ジャーナル キューに保持されます。|  
  
### <a name="child-elements"></a>子要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<msmqTransportSecurity >](msmqtransportsecurity.md)|このバインディングのトランスポート セキュリティ設定を指定します。 この要素は <xref:System.ServiceModel.Configuration.MsmqTransportSecurityElement> 型です。|  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<binding >](bindings.md)|カスタム バインドのすべてのバインド機能を定義します。|  
  
## <a name="remarks"></a>Remarks  
 `msmqTransport` 要素は、ユーザーがキューに置かれている通信チャネルのプロパティを設定できるようにします。 キューに置かれている通信チャネルは、そのトランスポートにメッセージ キューを使用します。  
  
 このバインド要素は、メッセージ キューの標準バインディング (`netMsmqBinding`) で使用される既定のバインド要素です。  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Configuration.MsmqTransportElement>
- <xref:System.ServiceModel.Channels.MsmqTransportBindingElement>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [WCF のキュー](../../../wcf/feature-details/queues-in-wcf.md)
- [トランスポート](../../../wcf/feature-details/transports.md)
- [トランスポートの選択](../../../wcf/feature-details/choosing-a-transport.md)
- [バインディング](../../../wcf/bindings.md)
- [バインディングの拡張](../../../wcf/extending/extending-bindings.md)
- [カスタム バインディング](../../../wcf/extending/custom-bindings.md)
- [\<customBinding >](custombinding.md)
