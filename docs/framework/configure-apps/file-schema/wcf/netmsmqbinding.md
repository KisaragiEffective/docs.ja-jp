---
title: <netMsmqBinding>
ms.date: 03/30/2017
ms.assetid: a68b44d7-7799-43a3-9e63-f07c782810a6
ms.openlocfilehash: 7456c6373c64e07b73e15e7e2bb229dce4032121
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74140739"
---
# <a name="netmsmqbinding"></a>\<netMsmqBinding >
複数コンピューターの通信に適しているキューに置かれたバインディングを定義します。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp; &nbsp;[ **\<system >** ](system-servicemodel.md) \
&nbsp;&nbsp;&nbsp;&nbsp;\<[**バインド**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**netMsmqBinding >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<netMsmqBinding>
  <binding closeTimeout="TimeSpan"
           customDeadLetterQueue="Uri"
           deadLetterQueue="Uri"
           durable="Boolean"
           exactlyOnce="Boolean"
           maxBufferPoolSize="Integer"
           maxReceivedMessageSize="Integer"
           maxRetryCycles="Integer"
           name="String"
           openTimeout="TimeSpan"
           poisonMessageHandling="Disabled/EnabledIfSupported"
           queueTransferProtocol="Native/Srmp/SrmpSecure"
           receiveErrorHandling="Drop/Fault/Move/Reject"
           receiveTimeout="TimeSpan"
           receiveRetryCount="Integer"
           rejectAfterLastRetry="Boolean"
           retryCycleDelay="TimeSpan"
           sendTimeout="TimeSpan"
           timeToLive="TimeSpan"
           useActiveDirectory="Boolean"
           useMsmqTracing="Boolean"
           useSourceJournal="Boolean">
    <security>
      <message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
               clientCredentialType="None/Windows/UserName/Certificate/InfoCard" />
      <transport msmqAuthenticationMode="None/WindowsDomain/Certificate"
                 msmqEncryptionAlgorithm="RC4Stream/AES"
                 msmqProtectionLevel="None/Sign/EncryptAndSign"
                 msmqSecureHashAlgorithm="MD5/SHA1/SHA256/SHA512" />
    </security>
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</netMsmqBinding>
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|`closeTimeout`|クローズ操作が完了するまでの期間を指定する <xref:System.TimeSpan> 値。 この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。 既定値は 00:01:00 です。|  
|`customDeadLetterQueue`|アプリケーションごとの配信不能キューの場所が含まれている URI です。ここには、期限切れのメッセージや、転送または配信に失敗したメッセージが配置されます。<br /><br /> 配信不能キューは、送信元アプリケーションのキュー マネージャーにある、配信に失敗した期限切れメッセージのキューです。<br /><br /> <xref:System.ServiceModel.MsmqBindingBase.CustomDeadLetterQueue%2A> によって指定される URI は、net.msmq スキームを使用する必要があります。|  
|`deadLetterQueue`|使用する配信不能キューがある場合にその種類を指定する <xref:System.ServiceModel.MsmqBindingBase.DeadLetterQueue%2A> 値。<br /><br /> 配信不能キューは、アプリケーションへの配信に失敗したメッセージが転送される場所です。<br /><br /> `exactlyOnce` 保証が必要なメッセージ (つまり、`exactlyOnce` 属性が `true` に設定される) の場合、この属性は、既定で MSMQ のトランザクション システム全体の配信不能キューになります。<br /><br /> 保証が必要ないメッセージの場合、この属性の既定値は `null` です。|  
|`durable`|メッセージがキューで非揮発性か揮発性かを示すブール値です。 非揮発性メッセージは、キュー マネージャーがクラッシュしても残り、揮発性メッセージは失われます。 アプリケーションで待ち時間の短縮が要求され、場合によってはメッセージが失われてもかまわない場合は、揮発性メッセージが適しています。 `exactlyOnce` 属性が `true` に設定されている場合、メッセージは永続的にする必要があります。 既定値は、 `true`です。|  
|`exactlyOnce`|このバインディングで処理される各メッセージが 1 回だけ受信されるかどうかを示すブール値です。 その後、送信側に配信エラーが通知されます。 `durable` が `false` の場合、この属性は無視されて、配信が保証されずにメッセージが転送されます。 既定値は、 `true`です。 詳細については、「<xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A>」を参照してください。|  
|`maxBufferPoolSize`|このバインディングに使用するバッファー プール サイズの上限を指定する整数。 既定値は 8 です。|  
|`maxReceivedMessageSize`|このバインディングにより処理される最大メッセージ サイズ (ヘッダーを含む) をバイト単位で定義する正の整数です。 この制限を超えるメッセージの送信者が、SOAP エラーを受信します。 メッセージは受信者によってドロップされ、トレース ログにこのイベントのエントリが作成されます。 既定値は 65536 です。 このメッセージ サイズの制限は、サービス拒否 (DoS) 攻撃への露出を制限するためのものです。|  
|`maxRetryCycles`|有害メッセージ検出機能により使用される再試行サイクルの回数を示す整数です。 すべてのサイクルの配信試行にすべて失敗すると、メッセージは有害メッセージになります。 既定値は 3 です。 詳細については、「<xref:System.ServiceModel.MsmqBindingBase.MaxRetryCycles%2A>」を参照してください。|  
|`name`|必須の属性です。 バインディングの構成名を格納する文字列です。 この値は、バインディングの ID として使用されるため、一意にする必要があります。 .NET Framework 4 以降では、バインドと動作に名前を付ける必要はありません。 既定の構成と無名のバインドおよび動作の詳細については、「 [WCF サービスの](../../../wcf/samples/simplified-configuration-for-wcf-services.md)構成と簡略化された構成の[簡略化](../../../wcf/simplified-configuration.md)」を参照してください。|  
|`openTimeout`|実行中の操作が完了するまでの時間間隔を指定する <xref:System.TimeSpan> 値です。 この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。 既定値は 00:01:00 です。|  
|`QueueTransferProtocol`|このバインディングが使用するキューに置かれた通信チャネルのトランスポートを指定する有効な <xref:System.ServiceModel.QueueTransferProtocol> 値。 MSMQ は、SOAP リライアブル メッセージ プロトコルを使用する場合は Active Directory アドレス指定をサポートしません。 したがって、この属性を `Srmp` に設定したり、`useActiveDirectory` 属性が `true`に設定されているときに `Srmps` したりしないでください。|  
|`receiveErrorHandling`|有害メッセージおよびディスパッチ不能メッセージの処理方法を指定する <xref:System.ServiceModel.ReceiveErrorHandling> 値。|  
|`receiveRetryCount`|キュー マネージャーがメッセージを再試行キューに転送する前にメッセージ送信を試行する最大回数を指定する整数。|  
|`receiveTimeout`|受信操作が完了するまでの時間間隔を指定する <xref:System.TimeSpan> 値です。 この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。 既定値は 00:10:00 です。|  
|`retryCycleDelay`|すぐに配信できなかったメッセージを配信しようとするときの、再試行サイクルの時間遅延を指定する TimeSpan 値です。 実際の待機時間はさらに長くなる場合があるため、この値で定義されるのは最小待機時間だけです。 既定値は、00:10:00 です。 詳細については、「<xref:System.ServiceModel.MsmqBindingBase.RetryCycleDelay%2A>」を参照してください。|  
|`sendTimeout`|送信操作が完了するまでの時間間隔を指定する <xref:System.TimeSpan> 値です。 この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。 既定値は 00:01:00 です。|  
|`timeToLive`|メッセージの期限が切れて、配信不能キューに入れられるまでのメッセージの有効期間を指定する TimeSpan 値です。 既定値は 1.00:00:00 です。<br /><br /> この属性を設定すると、タイムリーなメッセージが受信側アプリケーションで処理される前に古くなることがなくなります。 キュー内のメッセージのうち、指定された期間内に受信アプリケーションで処理されなかったメッセージは、期限切れと呼ばれます。 期限切れのメッセージは、配信不能キューと呼ばれる特別なキューに送信されます。 配信不能キューの場所は、保証の内容に基づいて、`DeadLetterQueue` 属性を使用して設定されるか、適切な既定値に設定されます。|  
|`usingActiveDirectory`|キューのアドレスを Active Directory を使用して変換する必要があるかどうかを指定するブール値。<br /><br /> MSMQ キューのアドレスは、パス名または直接形式名で構成できます。 直接形式名を使用する場合、MSMQ は、DNS、NetBIOS、または IP を使用してコンピューター名を解決します。 パス名を使用する場合、MSMQ は、Active Directory を使用してコンピューター名を解決します。<br /><br /> 既定では、Windows Communication Foundation (WCF) のキューに置かれたトランスポートは、メッセージキューの URI を直接形式名に変換します。 `UseActiveDirectory` プロパティを true に設定すると、キューに置かれているトランスポートが DNS、NetBIOS、または IP ではなく Active Directory を使用してコンピューター名を解決する必要があることを、アプリケーションで指定できます。|  
|`useMsmqTracing`|このバインディングにより処理されるメッセージをトレースするかどうかを指定するブール値です。 既定値は、 `false`です。 トレースが有効な場合、メッセージ キュー コンピューターでメッセージが送受信されるたびに、レポート メッセージが作成され、レポート キューに送信されます。|  
|`useSourceJournal`|このバインディングにより処理されるメッセージのコピーをソース ジャーナルに保存するかどうかを指定するブール値です。 既定値は、 `false`です。<br /><br /> キューに置かれたアプリケーションでは、コンピューターの発信キューから送信されたメッセージの記録を残す場合は、メッセージをジャーナル キューにコピーできます。 メッセージが発信キューから送信され、送信先のコンピューターで受信されたという応答を受け取ると、メッセージのコピーが送信元のコンピューターのシステム ジャーナル キューに保持されます。|  
  
### <a name="child-elements"></a>子要素  
  
|要素|説明|  
|-------------|-----------------|  
|[readerQuotas > の \<](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|このバインドを使用して設定されるエンドポイントにより処理可能な、SOAP メッセージの複雑さに対する制約を定義します。 この要素は <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement> 型です。|  
|[\<セキュリティ >](security-of-netmsmqbinding.md)|バインディングのセキュリティ設定を定義します。 この要素は <xref:System.ServiceModel.Configuration.NetMsmqSecurityElement> 型です。|  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<バインド >](bindings.md)|この要素には、標準バインディングおよびカスタム バインドのコレクションが保持されます。|  
  
## <a name="remarks"></a>Remarks  
 `netMsmqBinding` バインディングは、Microsoft Message Queuing (MSMQ) をトランスポートとして使用したキューのサポートを提供し、疎結合アプリケーション、失敗の切り分け、読み込みの均一化、および切断操作のサポートを有効にします。 これらの機能の詳細については、「 [WCF のキュー](../../../wcf/feature-details/queues-in-wcf.md)」を参照してください。  
  
## <a name="example"></a>例  
  
```xml  
<configuration>
  <system.ServiceModel>
    <bindings>
      <netMsmqBinding>
        <binding closeTimeout="00:00:10"
                 openTimeout="00:00:20"
                 receiveTimeout="00:00:30"
                 sendTimeout="00:00:40"
                 deadLetterQueue="net.msmq://localhost/blah"
                 durable="true"
                 exactlyOnce="true"
                 maxReceivedMessageSize="1000"
                 maxRetries="11"
                 maxRetryCycles="12"
                 poisonMessageHandling="Disabled"
                 rejectAfterLastRetry="false"
                 retryCycleDelay="00:05:55"
                 timeToLive="00:11:11"
                 sourceJournal="true"
                 useMsmqTracing="true"
                 useActiveDirectory="true">
          <security>
            <message clientCredentialType="Windows" />
          </security>
        </binding>
      </netMsmqBinding>
    </bindings>
  </system.ServiceModel>
</configuration>
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.NetMsmqBinding>
- <xref:System.ServiceModel.Configuration.NetMsmqBindingElement>
- [\<バインド >](bindings.md)
- [バインディング](../../../wcf/bindings.md)
- [システムが提供するバインディングの構成](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [サービスとクライアントを構成するためのバインディングの使用](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [WCF のキュー](../../../wcf/feature-details/queues-in-wcf.md)
