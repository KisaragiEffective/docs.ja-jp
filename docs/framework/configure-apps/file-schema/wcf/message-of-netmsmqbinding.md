---
title: <message> の <netMsmqBinding>
ms.date: 03/30/2017
ms.assetid: 6ebf0240-d7be-4493-b0fe-f00fd5989d77
ms.openlocfilehash: 5a4a4e8b645ee2c607988ac3031af537c93ca8c0
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73736751"
---
# <a name="message-of-netmsmqbinding"></a>\<netMsmqBinding の \<メッセージ > >

この `netMsmqBinding` バインディングでの SOAP メッセージ セキュリティ設定を定義します。

[ **\<configuration>** ](../configuration-element.md)\
&nbsp; &nbsp;[ **\<system >** ](system-servicemodel.md) \
&nbsp;&nbsp;&nbsp;&nbsp;\<[**バインド**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**netMsmqBinding >** ](netmsmqbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**バインド >** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**セキュリティ >** ](security-of-netmsmqbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**メッセージ >**  

## <a name="syntax"></a>構文

```xml
<netMsmqBinding>
  <binding>
    <security>
      <message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
               clientCredentialType="None/Windows/UserName/Certificate/CardSpace" />
    </security>
  </binding>
</netMsmqBinding>
```

## <a name="attributes-and-elements"></a>属性および要素

以降のセクションでは、属性、子要素、および親要素について説明します。

### <a name="attributes"></a>属性

|属性|説明|
|---------------|-----------------|
|algorithmSuite|MSMQ トランスポートを介して送信されるメッセージにメッセージ ベースのセキュリティを実現するために使用されるメッセージの暗号化およびキー ラップ アルゴリズムを設定します。<br /><br /> 既定値は `Aes256`です。 この属性は <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> 型です。|
|clientCredentialType|MSMQ トランスポートで送信されるメッセージに対してクライアント認証を実行するときに使用される資格情報の種類を指定します。 以下の値が有効です。<br /><br /> -None: サービスが匿名クライアントと対話できるようにします。 サービスとクライアントはいずれも資格情報を要求しません。<br />-Windows: これにより、Windows 資格情報の認証済みコンテキストで SOAP 交換を行うことができます。 これは、常に Kerberos ベースの認証を実行します。<br />-UserName: これにより、サービスは、ユーザー名資格情報を使用してクライアントの認証を要求できます。 この場合の資格情報は、`clientCredentials` の動作を使用して指定する必要があり**ます。注意:** WINDOWS COMMUNICATION FOUNDATION (WCF) では、パスワードダイジェストの送信や、パスワードを使用したキーの派生、およびメッセージセキュリティのためのこのようなキーの使用はサポートされていません。 そのため、ユーザー名の資格情報を使用する場合、WCF は exchange がセキュリティで保護されるように強制します。 このモードは、サービス証明書が、`clientCredential` 動作および `serviceCertificate` を使用してクライアント側で指定されることを要求します。 <br /><br /> -Certificate: これにより、サービスは、証明書を使用してクライアントを認証するよう要求できます。 この場合のクライアント資格情報は、`clientCredentials` 動作を使用して指定する必要があります。 この場合のサービス資格情報は、`clientCredentials` を指定して `serviceCertificate` 動作を使用することで、指定する必要があります。<br />-CardSpace: サービスでは、クライアントが CardSpace を使用して認証されるように要求できます。 `serviceCertificate` は、`clientCredential` 動作で提供される必要があります。<br /><br /> 既定値は `Windows`です。 この属性は <xref:System.ServiceModel.MessageCredentialType> 型です。|

### <a name="child-elements"></a>子要素

None

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[\< セキュリティ >](security-of-netmsmqbinding.md)|バインディングのセキュリティ設定を定義します。|

## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Configuration.MessageSecurityOverMsmqElement>
- <xref:System.ServiceModel.Configuration.NetMsmqSecurityElement.Message%2A>
- <xref:System.ServiceModel.NetMsmqSecurity.Message%2A>
- <xref:System.ServiceModel.MessageSecurityOverMsmq>
- [WCF のキュー](../../../wcf/feature-details/queues-in-wcf.md)
- [サービスおよびクライアントのセキュリティ保護](../../../wcf/feature-details/securing-services-and-clients.md)
- [バインディング](../../../wcf/bindings.md)
- [システムが提供するバインディングの構成](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [サービスとクライアントを構成するためのバインディングの使用](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding >](bindings.md)
