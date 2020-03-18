---
title: <ws2007FederationHttpBinding>
ms.date: 03/30/2017
ms.assetid: 9af4ec79-cdef-457e-9dca-09d5eb821594
ms.openlocfilehash: 20ba643fddbac8a488e5457f0195cc253d4d23f7
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74139315"
---
# <a name="ws2007federationhttpbinding"></a>\<ws2007FederationHttpBinding >

[\<wsFederationHttpBinding >](wsfederationhttpbinding.md)から派生し、フェデレーションセキュリティをサポートする、セキュリティで保護された相互運用可能なバインド。

[ **\<configuration>** ](../configuration-element.md)\
&nbsp; &nbsp;[ **\<system >** ](system-servicemodel.md) \
&nbsp;&nbsp;&nbsp;&nbsp;\<[**バインド**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**ws2007FederationHttpBinding >**  
  
## <a name="syntax"></a>構文

```xml
<ws2007FederationHttpBinding>
  <binding bypassProxyOnLocal="Boolean"
           closeTimeout="TimeSpan"
           hostNameComparisonMode="StrongWildcard/Exact/WeakWildcard"
           maxBufferPoolSize="integer"
           maxReceivedMessageSize="integer"
           messageEncoding="Text/Mtom"
           name="string"
           openTimeout="TimeSpan"
           privacyNoticeAt="Uri"
           privacyNoticeVersion="Integer"
           proxyAddress="Uri"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan"
           textEncoding="UnicodeFffeTextEncoding/Utf16TextEncoding/Utf8TextEncoding"
           transactionFlow="Boolean"
           useDefaultWebProxy="Boolean">
    <security mode="None/Message/TransportWithMessageCredential">
      <message negotiateServiceCredential="Boolean"
               algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
               issuedTokenType="string"
               issuedKeyType="SymmetricKey/PublicKey">
      </message>
    </security>
    <reliableSession ordered="Boolean"
                     inactivityTimeout="TimeSpan"
                     enabled="Boolean" />
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</ws2007FederationHttpBinding>
```

## <a name="attributes-and-elements"></a>属性および要素

以降のセクションでは、属性、子要素、および親要素について説明します。

### <a name="attributes"></a>属性

|属性|説明|
|---------------|-----------------|
|`bypassProxyOnLocal`|ローカル アドレスでプロキシ サーバーをバイパスするかどうかを示す値。 既定値は、 `false`です。|
|`closeTimeout`|クローズ操作が完了するまでの期間を指定する <xref:System.TimeSpan> 値。 この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。 既定値は 00:01:00 です。|
|`hostNameComparisonMode`|URI の解析に使用する HTTP ホスト名比較モードを指定します。 この属性は <xref:System.ServiceModel.HostNameComparisonMode> 型で、URI が一致したときにサービスへのアクセスにホスト名を使用するかどうかを指定します。 既定値は <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard> で、一致しているホスト名を無視します。|
|`maxBufferPoolSize`|このバインドに使用するバッファー プールの最大サイズ。 既定は 524,288 バイト (512 * 1024) です。 Windows Communication Foundation (WCF) では、多くの部分でバッファーを使用します。 使用するたびに毎回バッファーを作成および破壊すると負荷が高くなります。バッファーのガベージ コレクションも同様です。 バッファー プールを使用すると、バッファーをプールから取得して使用し、作業が終わったらプールに戻すことができます。 これで、バッファーの作成と破棄のオーバーヘッドを回避できます。|
|`maxReceivedMessageSize`|チャネルで受信可能な最大メッセージ サイズ (ヘッダーを含む) が、このバインドを使用してバイト単位で設定されました。 この制限を超えるメッセージの送信者が、SOAP エラーを受信します。 メッセージは受信者によってドロップされ、トレース ログにこのイベントのエントリが作成されます。 既定値は 65536 です。|
|`messageEncoding`|メッセージのエンコードに使用されるエンコーダーを定義します。 以下の値が有効です。<br /><br /> -Text: テキストメッセージエンコーダーを使用します。<br />-Mtom: メッセージ伝送組織機構 1.0 (MTOM) エンコーダーを使用します。<br /><br /> 既定値は Text です。<br /><br /> この属性は <xref:System.ServiceModel.WSMessageEncoding> 型です。|
|`name`|バインドの構成名。 この値は、バインディングの ID として使用されるため、一意にする必要があります。 .NET Framework 4 以降では、バインドと動作に名前を付ける必要はありません。 既定の構成と無名のバインドおよび動作の詳細については、「 [WCF サービスの](../../../wcf/samples/simplified-configuration-for-wcf-services.md)構成と簡略化された構成の[簡略化](../../../wcf/simplified-configuration.md)」を参照してください。|
|`openTimeout`|実行中の操作が完了するまでの時間間隔を指定する <xref:System.TimeSpan> 値です。 この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。 既定値は 00:01:00 です。|
|`privacyNoticeAt`|プライバシーに関する声明の場所を示す URI。|
|`privacyNoticeVersion`|現在のプライバシーに関する声明のバージョン。|
|`proxyAddress`|HTTP プロキシのアドレスを指定する URI。 `useDefaultWebProxy` が `true` の場合、この設定を `null` にする必要があります。 既定値は、 `null`です。|
|`receiveTimeout`|受信操作が完了するまでの時間間隔を指定する <xref:System.TimeSpan> 値です。 この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。 既定値は 00:10:00 です。|
|`sendTimeout`|送信操作が完了するまでの時間間隔を指定する <xref:System.TimeSpan> 値です。 この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。 既定値は 00:01:00 です。|
|`textEncoding`|バインディングでメッセージの発行に使用される文字セット エンコーディングを設定します。 以下の値が有効です。<br /><br /> -BigEndianUnicode: Unicode ビッグエンディアンエンコーディング。<br />-Unicode:16 ビットエンコード。<br />-UTF8: 8 ビットエンコーディング。<br /><br /> 既定値は UTF8 です。 この属性は <xref:System.Text.Encoding> 型です。|
|`transactionFlow`|バインドが WS-Transactions のフローをサポートするかどうかを指定する値。 既定値は、 `false`です。|
|`useDefaultWebProxy`|システムの自動設定 HTTP プロキシを使用するかどうかを示す値。 この属性が `null` の場合、プロキシ アドレスを `true` (つまり、設定しない) にする必要があります。 既定値は、 `true`です。|

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[\<セキュリティ >](security-of-wsfederationhttpbinding.md)|メッセージのセキュリティ設定を定義します。 この要素は <xref:System.ServiceModel.Configuration.WSFederationHttpSecurityElement> 型です。|
|[readerQuotas > の \<](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|このバインドを使用して設定されるエンドポイントにより処理可能な、SOAP メッセージの複雑さに対する制約を定義します。 この要素は <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement> 型です。|
|[\<reliableSession >](https://docs.microsoft.com/previous-versions/ms731375(v=vs.90))|チャネルのエンドポイント間に信頼できるセッションを確立するかどうかを指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[\<バインド >](bindings.md)|この要素には、標準バインディングおよびカスタム バインドのコレクションが保持されます。|

## <a name="remarks"></a>Remarks

フェデレーションは、複数の企業または信頼するドメインで認証と承認用の ID を共有する機能です。 フェデレーションは、WS-Trust プロトコルを使用して、ID 形式を、ある信頼するドメインから別の信頼するドメインにマップします。 フェデレーション HTTP バインドは、SOAP セキュリティと混合モード セキュリティをサポートしますが、トランスポート セキュリティの単独使用はサポートしません。 このバインディングで構成されたサービスは、HTTP トランスポートを使用する必要があります。 詳細については、「 [\<wsFederationHttpBinding >](wsfederationhttpbinding.md)」を参照してください。

## <a name="example"></a>例

```xml
<configuration>
  <system.ServiceModel>
    <bindings>
      <ws2007FederationHttpBinding>
        <binding bypassProxyOnLocal="false"
                 transactionFlow="false"
                 hostNameComparisonMode="WeakWildcard"
                 maxReceivedMessageSize="1000"
                 messageEncoding="Mtom"
                 proxyAddress="http://www.contoso.com"
                 textEncoding="Utf16TextEncoding"
                 useDefaultWebProxy="false">
          <reliableSession ordered="false"
                           inactivityTimeout="00:02:00"
                           enabled="true" />
          <security mode="None">
            <message negotiateServiceCredential="false"
                     algorithmSuite="Aes128"
                     issuedTokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1"
                     issuedKeyType="PublicKey">
              <issuer address="http://localhost/Sts" />
            </message>
          </security>
        </binding>
      </ws2007FederationBinding>
    </bindings>
  </system.ServiceModel>
</configuration>
```

## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.WS2007FederationHttpBinding>
- <xref:System.ServiceModel.Configuration.WS2007FederationHttpBindingElement>
- [\<wsFederationHttpBinding>](wsfederationhttpbinding.md)
- [バインディング](../../../wcf/bindings.md)
- [システムが提供するバインディングの構成](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [サービスとクライアントを構成するためのバインディングの使用](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<バインド >](bindings.md)
