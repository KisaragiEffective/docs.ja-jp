---
title: <httpTransport>
ms.date: 03/30/2017
ms.assetid: 8b30c065-b32a-4fa3-8eb4-5537a9c6b897
ms.openlocfilehash: b3558db6018d79f0fad27ff28657bfadb5637467
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73736765"
---
# <a name="httptransport"></a>\<httpTransport >
カスタム バインディングの SOAP メッセージを送信する HTTP トランスポートを指定します。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp; &nbsp;[ **\<system >** ](system-servicemodel.md) \
&nbsp;&nbsp;&nbsp;&nbsp;\<[**バインド**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**customBinding >** ](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**バインド >** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**httpTransport >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<httpTransport allowCookies="Boolean"
               authenticationScheme="Digest/Negotiate/Ntlm/Basic/Anonymous"
               bypassProxyOnLocal="Boolean"
               hostnameComparisonMode="StrongWildcard/Exact/WeakWildcard"
               keepAliveEnabled="Boolean"
               maxBufferSize="Integer"
               proxyAddress="Uri"
               proxyAuthenticationScheme="None/Digest/Negotiate/Ntlm/Basic/Anonymous"
               realm="String"
               transferMode="Buffered/Streamed/StreamedRequest/StreamedResponse"
               unsafeConnectionNtlmAuthentication="Boolean"
               useDefaultWebProxy="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|allowCookies|クライアントがクッキーを受け入れて、それらを今後の要求に反映させるかどうかを指定するブール値です。 既定値は、 `false`です。<br /><br /> この属性はクッキーを使用する ASMX Web サービスと対話する場合に使用できます。 この方法で、サーバーから返されるクッキーを、それ以降のサービスに対するすべてのクライアント要求に自動的にコピーできます。|  
|authenticationScheme|HTTP リスナーにより処理されているクライアント要求の認証に使用するプロトコルを指定します。 以下の値が有効です。<br /><br /> -Digest: ダイジェスト認証を指定します。<br />-Negotiate: クライアントとネゴシエートし、認証方式を決定します。 クライアントとサーバーの両方が Kerberos をサポートする場合は、この方式が使用されます。それ以外の場合は NTLM が使用されます。<br />-Ntlm: NTLM 認証を指定します。<br />-Basic: 基本認証を指定します。<br />-Anonymous: 匿名認証を指定します。<br /><br /> 既定は Anonymous です。 この属性は <xref:System.Net.AuthenticationSchemes> 型です。 この属性は 1 回だけ設定できます。|  
|bypassProxyOnLocal|ローカル アドレスでプロキシ サーバーをバイパスするかどうかを示すブール値。 既定値は、 `false`です。<br /><br /> ローカル アドレスは、ローカル LAN またはイントラネット上にあるアドレスです。<br /><br /> サービスアドレスが `http://localhost`で始まる場合、Windows Communication Foundation (WCF) は常にプロキシを無視します。<br /><br /> クライアントが同じマシン上のサービスと対話するときにプロキシを経由させる場合は、localhost ではなくホスト名を使用する必要があります。|  
|hostnameComparisonMode|URI の解析に使用する HTTP ホスト名比較モードを指定します。 有効な値は次のとおりです。<br /><br /> -StrongWildcard: ("+") は、指定されたスキーム、ポート、および相対 URI のコンテキストで使用可能なすべてのホスト名と一致します。<br />-Exact: ワイルドカードなし<br />-値のワイルドカード: ("\*") は、明示的に、または厳密なワイルドカード機構を使用して一致していない、指定されたスキーム、ポート、および相対 UIR のコンテキストで使用可能なすべてのホスト名と一致します。<br /><br /> この属性は <xref:System.ServiceModel.HostNameComparisonMode> 型です。 既定値は、 <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard>です。|  
|keepAliveEnabled|インターネット リソースへの永続的な接続を行うかどうかを示すブール値。|  
|maxBufferSize|バッファーの最大サイズを指定する正の整数です。 既定値は 524288 です|  
|proxyAddress|HTTP プロキシのアドレスを指定する URI。 `useSystemWebProxy` が `true` の場合、この設定を `null` にする必要があります。 既定値は、 `null`です。|  
|proxyAuthenticationScheme|HTTP プロキシにより処理されているクライアント要求の認証に使用するプロトコルを指定します。 以下の値が有効です。<br /><br /> -None: 認証は実行されません。<br />-Digest: ダイジェスト認証を指定します。<br />-Negotiate: クライアントとネゴシエートし、認証方式を決定します。 クライアントとサーバーの両方が Kerberos をサポートする場合は、この方式が使用されます。それ以外の場合は NTLM が使用されます。<br />-Ntlm: NTLM 認証を指定します。<br />-Basic: 基本認証を指定します。<br />-Anonymous: 匿名認証を指定します。<br /><br /> 既定は Anonymous です。 この属性は <xref:System.Net.AuthenticationSchemes> 型です。 <xref:System.Net.AuthenticationSchemes.IntegratedWindowsAuthentication?displayProperty=nameWithType> はサポートされていないことに注意してください。|  
|realm|プロキシおよびサーバーで使用するレルムを指定する文字列です。 既定値は空の文字列です。<br /><br /> サーバーは、レルムを使用して、保護されたリソースをパーティションに分割します。 パーティションごとに、独自の認証方式と承認データベースの両方、またはそのいずれかを指定できます。 レルムは、基本認証およびダイジェスト認証だけに使用されます。 クライアントが正常に認証されると、その認証は特定のレルムのすべてのリソースに対して有効となります。 領域の詳細については、 [IETF web サイト](https://www.ietf.org)の RFC 2617 を参照してください。|  
|transferMode|メッセージが要求や応答をバッファーするか、ストリーミングするかを指定します。 以下の値が有効です。<br /><br /> -バッファー: 要求メッセージと応答メッセージがバッファーされます。<br />-ストリーミング: 要求メッセージと応答メッセージがストリーミングされます。<br />-StreamedRequest: 要求メッセージをストリーミングし、応答メッセージをバッファーします。<br />-StreamedResponse: 要求メッセージがバッファーされ、応答メッセージがストリーミングされます。<br /><br /> 既定値はバッファーです。 この属性は <xref:System.ServiceModel.TransferMode> 型です。|  
|unsafeConnectionNtlmAuthentication|サーバー上で安全ではない接続共有を有効にするかどうかを指定するブール値です。 既定値は、 `false`です。 有効な場合、NTLM 認証は、TCP 接続ごとに 1 回実行されます。|  
|useDefaultWebProxy|ユーザー固有の設定ではなく、コンピューター全体のプロキシ設定を使用するかどうかを指定するブール値です。 既定値は、 `true`です。|  
  
### <a name="child-elements"></a>子要素  
 None  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<binding >](bindings.md)|カスタム バインドのすべてのバインド機能を定義します。|  
  
## <a name="remarks"></a>Remarks  
 `httpTransport` 要素は、HTTP トランスポート プロトコルを実装するカスタム バインドを作成する場合の開始点となります。 HTTP は、相互運用性のために使用される主要なトランスポートです。 このトランスポートは、他の WCF 以外の Web サービススタックとの相互運用性を確保するために、Windows Communication Foundation (WCF) によってサポートされています。  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Configuration.HttpTransportElement>
- <xref:System.ServiceModel.Channels.HttpTransportBindingElement>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [トランスポート](../../../wcf/feature-details/transports.md)
- [トランスポートの選択](../../../wcf/feature-details/choosing-a-transport.md)
- [バインディング](../../../wcf/bindings.md)
- [バインディングの拡張](../../../wcf/extending/extending-bindings.md)
- [カスタム バインディング](../../../wcf/extending/custom-bindings.md)
- [\<customBinding >](custombinding.md)
