---
title: <reliableSession>
ms.date: 03/30/2017
ms.assetid: 129b4a59-37f0-4030-b664-03795d257d29
ms.openlocfilehash: 95f6646041dc2dd7bae7691a0a9f748c844f50b6
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73738754"
---
# <a name="reliablesession"></a>\<reliableSession >
WS-ReliableMessaging の設定を定義します。 この要素がカスタム バインディングに追加される場合、その結果となるチャネルにより、正確に 1 回の配信保証をサポートできます。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp; &nbsp;[ **\<system >** ](system-servicemodel.md) \
&nbsp;&nbsp;&nbsp;&nbsp;\<[**バインド**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**customBinding >** ](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**バインド >** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**reliableSession >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<reliableSession acknowledgementInterval="TimeSpan"
                 flowControlEnabled="Boolean"
                 inactivityTimeout="TimeSpan"
                 maxPendingChannels="Integer"
                 maxRetryCount="Integer"
                 maxTransferWindowSize="Integer"
                 reliableMessagingVersion="Default/WSReliableMessagingFebruary2005/WSReliableMessaging11"
                 ordered="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|acknowledgementInterval|そのポイントまで受信したメッセージの受信確認の送信をチャネルが待機する最大時間間隔を含む <xref:System.TimeSpan>。 既定値は 00:00:0.2 です。|  
|flowControlEnabled|高度なフロー制御を有効にするかどうかを示すブール値。WS-ReliableMessaging のフロー制御の Microsoft 固有の実装です。 既定値は、 `true`です。|  
|inactivityTimeout|他の通信相手がチャネルにメッセージを送信せずにいられる最長期間を指定する <xref:System.TimeSpan>。他の通信相手がメッセージを送信しない期間がこの値を超えると、チャネルでエラーが発生します。 既定値は 00:10:00 です。<br /><br /> チャネルでのアクティビティは、アプリケーションまたはインフラストラクチャのメッセージを受信するように定義されています。 このプロパティは、アクティブでないセッションを維持する最長期間を制御します。 反応のない時間がこれより長く経過すると、インフラストラクチャによってセッションは中止され、チャネルはエラーとなります。 **注:** アプリケーションが定期的にメッセージを送信して接続を維持する必要はありません。|  
|maxPendingChannels|受け入れるリスナーを待機できるチャネルの最大数を指定する整数。 この値は、1 ～ 16384 (1 と 16384を含む) のいずれかです。 既定値は 4 です。<br /><br /> チャネルは、受け入れを待っている間は保留状態になります。 制限に達すると、チャネルは作成されなくなります。 正確には、この数が (保留状態のチャネルの受け入れによって) 減少するまで、保留モードにされます。 これはファクトリごとの制限です。<br /><br /> しきい値に達した後に、リモート アプリケーションが新しい信頼できるセッションを確立しようとした場合、要求は拒否され、この原因となる open 操作でエラーが発生します。 この制限は、保留状態の送信チャネルの数には適用されません。|  
|maxRetryCount|基になるチャネルで Send を呼び出すことで、信頼できるチャネルが受信確認を受信していないメッセージの再転送を試みる最大回数を指定する整数。<br /><br /> この値は、ゼロより大きい値である必要があります。 既定値は 8 です。<br /><br /> この値は、0 を超えた整数にする必要があります。 最後の再転送後に受信確認が受信されない場合、チャネルでエラーが発生します。<br /><br /> メッセージは、受信側でその配信が確認されている場合、転送されたと見なされます。<br /><br /> 転送済みのメッセージの受信確認が一定時間内に受信されない場合、インフラストラクチャは、自動的にそのメッセージを再転送します。 インフラストラクチャは、このプロパティで指定された回数まで、メッセージを再送信しようとします。 最後の再転送後に受信確認が受信されない場合、チャネルでエラーが発生します。<br /><br /> インフラストラクチャは、指数バックオフ アルゴリズムを使用し、計算された平均ラウンドトリップ時間に基づいて、いつ再転送するかを判断します。 時間は、まず再転送の 1 秒前に開始され、試行ごとに遅延が 2 倍にされます。この結果、最初の転送試行から最後の再転送試行までに約 8.5 分かかります。 最初の再転送試行のタイミングは、計算されたラウンドトリップ時間に従って調整されるので、試行にかかる時間延長はさまざまです。 これにより、再転送のタイミングをさまざまなネットワーク条件に動的に適用できます。|  
|maxTransferWindowSize|バッファーの最大サイズを指定する整数。 有効値は 1 ～ 4096 の範囲です。<br /><br /> クライアントでは、この属性は、まだ受信側で受信が確認されていないメッセージを保持するために信頼できるチャネルが使用するバッファーの最大サイズを定義します。 クォータの単位はメッセージです。 バッファーがいっぱいになると、それ以降の SEND 操作がブロックされます。<br /><br /> 受信側では、この属性は、まだアプリケーションにディスパッチされていない受信メッセージを格納するためにチャネルが使用するバッファーの最大サイズを定義します。 バッファーがいっぱいになると、それ以降のメッセージは、受信側によって通知なしに自動的に削除されるので、クライアントによる再転送が必要になります。|  
|ordered|メッセージが送信された順序で到着されることを保証するかどうかを指定するブール値。 この設定が `false` の場合、メッセージは送信された順序で到着しません。 既定値は、 `true`です。|  
|reliableMessagingVersion|使用する WS-ReliableMessaging バージョンを指定する <xref:System.ServiceModel.ReliableMessagingVersion> の有効な値。|  
  
### <a name="child-elements"></a>子要素  
 None  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<binding >](bindings.md)|カスタム バインドのすべてのバインド機能を定義します。|  
  
## <a name="remarks"></a>Remarks  
 信頼できるセッションは、信頼できるメッセージとセッションに関する機能を提供します。 信頼できるメッセージは、エラー時に通信を再試行するほか、メッセージの順次到着などの配信の保証を指定できるようにします。 セッションでは、呼び出し間でクライアントの状態が保持されます。 この要素には、オプションとして、順序付けされたメッセージ配信を行う機能も用意されています。 この実装されたセッションは、SOAP およびトランスポート中継局を通過できます。  
  
 各バインド要素は、メッセージの送信または受信時の処理手順を表します。 実行時に、バインド要素は、メッセージの送受信に求められる送信および受信チャネル スタックを作成するために必要なチャネル ファクトリとリスナーを作成します。 `reliableSession` が提供するスタック内のオプションの層は、エンドポイント間に信頼できるセッションを確立し、このセッションの動作を構成することができます。  
  
 詳細については、「[信頼できるセッション](../../../wcf/feature-details/reliable-sessions.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の例では、さまざまなトランスポートとメッセージ エンコーディング要素を使用し、特に、クライアントの状態を保持し、配信順序を保証することを指定する、信頼できるセッションを有効化することによって、カスタム バインドを構成する方法を示します。 この機能は、クライアントとサービスのアプリケーション構成ファイルで構成されます。 サービスの構成の例を次に示します。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceModel.Samples.CalculatorService"
               behaviorConfiguration="CalculatorServiceBehavior">
        <!-- This endpoint is exposed at the base address provided by host: http://localhost/servicemodelsamples/service.svc -->
        <!-- specify customBinding binding and a binding configuration to use -->
        <endpoint address=""
                  binding="customBinding"
                  bindingConfiguration="Binding1"
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />
        <!-- The mex endpoint is exposed at http://localhost/servicemodelsamples/service.svc/mex -->
        <endpoint address="mex"
                  binding="mexHttpBinding"
                  contract="IMetadataExchange" />
      </service>
    </services>
    <!-- custom binding configuration - configures HTTP transport, reliable sessions -->
    <bindings>
      <customBinding>
        <binding name="Binding1">
          <reliableSession />
          <security authenticationMode="SecureConversation"
                    requireSecurityContextCancellation="true">
          </security>
          <compositeDuplex />
          <oneWay />
          <textMessageEncoding messageVersion="Soap12WSAddressing10"
                               writeEncoding="utf-8" />
          <httpTransport authenticationScheme="Anonymous"
                         bypassProxyOnLocal="false"
                         hostNameComparisonMode="StrongWildcard"
                         proxyAuthenticationScheme="Anonymous"
                         realm=""
                         useDefaultWebProxy="true" />
        </binding>
      </customBinding>
    </bindings>
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->
    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <serviceMetadata httpGetEnabled="True" />
          <serviceDebug includeExceptionDetailInFaults="False" />
        </behavior>
      </serviceBehaviors>
    </behaviors>
  </system.serviceModel>
</configuration>
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Configuration.ReliableSessionElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- <xref:System.ServiceModel.Channels.ReliableSessionBindingElement>
- [信頼できるセッション](../../../wcf/feature-details/reliable-sessions.md)
- [バインディング](../../../wcf/bindings.md)
- [バインディングの拡張](../../../wcf/extending/extending-bindings.md)
- [カスタム バインディング](../../../wcf/extending/custom-bindings.md)
- [\<customBinding >](custombinding.md)
