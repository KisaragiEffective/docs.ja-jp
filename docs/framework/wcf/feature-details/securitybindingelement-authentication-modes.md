---
title: SecurityBindingElement 認証モード
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 12300bf4-c730-4405-9f65-d286f68b5a43
ms.openlocfilehash: d6831452370b672a6c02ace31dc05ef07ca48154
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74141639"
---
# <a name="securitybindingelement-authentication-modes"></a>SecurityBindingElement 認証モード
Windows Communication Foundation (WCF) では、クライアントとサービスが相互に認証するために使用されるモードがいくつかあります。 <xref:System.ServiceModel.Channels.SecurityBindingElement> クラスの静的メソッドまたは構成を使用して、この認証モード用のセキュリティ バインド要素を作成できます。 このトピックでは、18 の認証モードについて簡単に説明します。  
  
 認証モードの1つに要素を使用する例については、「[方法: 指定された認証モードのための説明を作成](../../../../docs/framework/wcf/feature-details/how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)する」を参照してください。  
  
## <a name="basic-configuration-programming"></a>基本的な構成プログラミング  
 構成ファイルで認証モードを設定する手順を次に示します。  
  
#### <a name="to-set-the-authentication-mode-in-configuration"></a>構成で認証モードを設定するには  
  
1. [\<バインド >](../../../../docs/framework/configure-apps/file-schema/wcf/bindings.md)要素に、 [\<customBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)を追加します。  
  
2. 子要素として、 [\<バインド >](../../configure-apps/file-schema/wcf/bindings.md)要素を `<customBinding>` 要素に追加します。  
  
3. `<security>` 要素を `<binding>` 要素に追加します。  
  
4. `authenticationMode` 属性を、以下で説明する値のいずれかに設定します。 たとえば、次のコードによりモードは `AnonymousForCertificate` に設定されます。  
  
    ```xml  
    <bindings>  
      <customBinding>  
        <binding name="SecureCustomBinding">  
         <security authenticationMode ="AnonymousForCertificate" />  
        </binding>  
      </customBinding>  
    </bindings>  
    ```  
  
#### <a name="to-set-the-mode-programmatically"></a>プログラムでモードを設定するには  
  
1. 戻り値の型を決めます。<xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>、<xref:System.ServiceModel.Channels.TransportSecurityBindingElement>、<xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement>、または <xref:System.ServiceModel.Channels.SecurityBindingElement> のいずれかを指定できます。  
  
2. <xref:System.ServiceModel.Channels.SecurityBindingElement> クラスの適切な静的メソッドを呼び出します。 たとえば、次のコードは <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateAnonymousForCertificateBindingElement%2A> メソッドを呼び出します。  
  
     [!code-csharp[c_CustomBindingsAuthMode#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombindingsauthmode/cs/source.cs#3)]
     [!code-vb[c_CustomBindingsAuthMode#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_custombindingsauthmode/vb/source.vb#3)]  
  
3. カスタム バインドを作成するにはバインド要素を使用します。 詳細については、「[カスタムバインド](../../../../docs/framework/wcf/extending/custom-bindings.md)」を参照してください。  
  
## <a name="mode-descriptions"></a>モードの説明  
  
### <a name="anonymousforcertificate"></a>AnonymousForCertificate  
 この認証モードでは、クライアントは匿名になり、X.509 証明書を使用してサービスが認証されます。 セキュリティ バインド要素は、<xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateAnonymousForCertificateBindingElement%2A> です。 または、<`security`> 要素の `authenticationMode` 属性を `AnonymousForCertificate`に設定します。  
  
### <a name="anonymousforsslnegotiated"></a>AnonymousForSslNegotiated  
 この認証モードでは、クライアントが匿名になり、実行時にネゴシエートされる X.509 証明書を使用してサービスが認証されます。 セキュリティ バインド要素は、最初のパラメーターに <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> の値が渡される場合に <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSslNegotiationBindingElement%2A> メソッドによって返される `false` です。 代わりに、`authenticationMode` 属性に `AnonymousForSslNegotiated` を設定します。  
  
### <a name="certificateovertransport"></a>CertificateOverTransport  
 この認証モードでは、クライアントは、保証サポート トークン、つまりメッセージ署名を行うトークンとして SOAP 層に表示される X.509 証明書を使用して認証を行います。 サービスはトランスポート層で X.509 証明書を使用して認証されます。 セキュリティ バインド要素は、<xref:System.ServiceModel.Channels.TransportSecurityBindingElement> メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateCertificateOverTransportBindingElement%2A> です。 代わりに、`authenticationMode` 属性に `CertificateOverTransport` を設定します。  
  
### <a name="issuedtoken"></a>IssuedToken  
 この認証モードでは、クライアントはサービスに対する認証を行いません。代わりに、クライアントはセキュリティ トークン サービスに対して認証を行い、SAML トークンを受信します。その後、クライアントはサーバーに対してこのトークンを提示し、共有キーの情報を示します。 サービスはクライアントに対して認証されませんが、そのサービスだけがキーを復号化できるように、セキュリティ トークン サービスは発行されたトークンの一部として共有キーを暗号化します。 セキュリティ バインド要素は、<xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenBindingElement%2A> です。 代わりに、`authenticationMode` 属性に `IssuedToken` を設定します。  
  
### <a name="issuedtokenforcertificate"></a>IssuedTokenForCertificate  
 この認証モードでは、クライアントはサービスに対する認証を行いません。代わりに、クライアントはセキュリティ トークン サービスに対して認証を行い、SAML トークンを受信します。その後、クライアントはサーバーに対してこのトークンを提示し、共有キーの情報を示します。 発行済みトークンは、保証サポート トークンまたは所有者トークン (メッセージ署名を行うトークン) として SOAP 層に表示されます。 X.509 証明書を使用したクライアントに対するサービス認証。 セキュリティ バインド要素は、<xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenForCertificateBindingElement%2A> です。 代わりに、`authenticationMode` 属性に `IssuedTokenForCertificate` を設定します。  
  
### <a name="issuedtokenforsslnegotiated"></a>IssuedTokenForSslNegotiated  
 この認証モードでは、クライアントはサービスに対する認証を行いません。代わりに、クライアントはセキュリティ トークン サービスに対して認証を行い、SAML トークンを受信します。その後、クライアントはサーバーに対してこのトークンを提示し、共有キーの情報を示します。 発行済みトークンは、保証サポート トークンまたは所有者トークン (メッセージ署名を行うトークン) として SOAP 層に表示されます。 サービスは X.509 証明書を使用して認証されます。 セキュリティ バインド要素は、<xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenForSslBindingElement%2A> です。 代わりに、`authenticationMode` 属性に `IssuedTokenForSslnegotiated` を設定します。  
  
### <a name="issuedtokenovertransport"></a>IssuedTokenOverTransport  
 この認証モードでは、クライアントはサービスに対する認証を行いません。代わりに、クライアントはセキュリティ トークン サービスに対して認証を行い、SAML トークンを受信します。その後、クライアントはサーバーに対してこのトークンを提示し、共有キーの情報を示します。 発行済みトークンは、保証サポート トークンまたは所有者トークン (メッセージ署名を行うトークン) として SOAP 層に表示されます。 サービスはトランスポート層で X.509 証明書を使用して認証されます。 セキュリティ バインド要素は、`TransportSecurityBindingElement` メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenOverTransportBindingElement%2A> です。 代わりに、`authenticationMode` 属性に `IssuedTokenOverTransport` を設定します。  
  
### <a name="kerberos"></a>Kerberos  
 この認証モードでは、クライアントは Kerberos チケットを使用してサービスに対する認証を行います。 また、その同じチケットによってサーバーが認証されます。 セキュリティ バインド要素は、`SymmetricSecurityBindingElement` メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateKerberosBindingElement%2A> です。 代わりに、`authenticationMode` 属性に `Kerberos` を設定します。  
  
> [!NOTE]
> この認証モードを使用するには、サービス アカウントをサービス プリンシパル名 (SPN: Service Principal Name) に関連付ける必要があります。 この関連付けを行うには、NETWORK SERVICE アカウントまたは LOCAL SYSTEM アカウントでサービスを実行します。 または、SetSpn.exe ツールを使用して、サービス アカウントの SPN を作成します。 どちらの場合も、クライアントは、 [\<servicePrincipalName >](../../../../docs/framework/configure-apps/file-schema/wcf/serviceprincipalname.md)要素で正しい SPN を使用するか、<xref:System.ServiceModel.EndpointAddress> コンストラクターを使用する必要があります。 詳細については、「[サービス id と認証](../../../../docs/framework/wcf/feature-details/service-identity-and-authentication.md)」を参照してください。  
  
> [!NOTE]
> `Kerberos` 認証モードを使用する場合、<xref:System.Security.Principal.TokenImpersonationLevel.Anonymous> および <xref:System.Security.Principal.TokenImpersonationLevel.Delegation> の各偽装レベルはサポートされません。  
  
### <a name="kerberosovertransport"></a>KerberosOverTransport  
 この認証モードでは、クライアントは Kerberos チケットを使用してサービスに対する認証を行います。 Kerberos トークンは、保証サポート トークン、つまりメッセージ署名を行うトークンとして SOAP 層に表示されます。 サービスはトランスポート層で X.509 証明書を使用して認証されます。 セキュリティ バインド要素は、`TransportSecurityBindingElement` メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateKerberosOverTransportBindingElement%2A> です。 代わりに、`authenticationMode` 属性に `KerberosOverTransport` を設定します。  
  
> [!NOTE]
> この認証モードを使用するには、サービス アカウントを SPN に関連付ける必要があります。 この関連付けを行うには、NETWORK SERVICE アカウントまたは LOCAL SYSTEM アカウントでサービスを実行します。 または、SetSpn.exe ツールを使用して、サービス アカウントの SPN を作成します。 どちらの場合も、クライアントは、 [\<servicePrincipalName >](../../../../docs/framework/configure-apps/file-schema/wcf/serviceprincipalname.md)要素で正しい SPN を使用するか、<xref:System.ServiceModel.EndpointAddress> コンストラクターを使用する必要があります。 詳細については、「[サービス id と認証](../../../../docs/framework/wcf/feature-details/service-identity-and-authentication.md)」を参照してください。  
  
### <a name="mutualcertificate"></a>MutualCertificate  
 この認証モードでは、クライアントは、保証サポート トークン、つまりメッセージ署名を行うトークンとして SOAP 層に表示される X.509 証明書を使用して認証を行います。 また、サービスは X.509 証明書を使用して認証されます。 セキュリティ バインド要素は、`SymmetricSecurityBindingElement` メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateMutualCertificateBindingElement%2A> です。 代わりに、`authenticationMode` 属性に `MutualCertificate` を設定します。  
  
### <a name="mutualcertificateduplex"></a>MutualCertificateDuplex  
 この認証モードでは、クライアントは、保証サポート トークン、つまりメッセージ署名を行うトークンとして SOAP 層に表示される X.509 証明書を使用して認証を行います。 また、サービスは X.509 証明書を使用して認証されます。 バインディングは、`AsymmetricSecurityBindingElement` メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateMutualCertificateDuplexBindingElement%2A> です。 代わりに、`authenticationMode` 属性に `MutualCertificateDuplex` を設定します。  
  
### <a name="mutualsslnegotiated"></a>MutualSslNegotiated  
 この認証モードでは、クライアントとサービスは X.509 証明書を使用して認証を行います。 セキュリティ バインド要素は、最初のパラメーターに `SymmetricSecurityBindingElement` の値が渡される場合に <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSslNegotiationBindingElement%2A> メソッドによって返される `true` です。 代わりに、`authenticationMode` 属性に `MutualSslNegotiated` を設定します。  
  
### <a name="secureconversation"></a>SecureConversation  
 セキュリティ バインド要素は、`SymmetricSecurityBindingElement` メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSecureConversationBindingElement%2A> です。 このメソッドは、パラメーターとして <xref:System.ServiceModel.Channels.SecurityBindingElement> を受け取り、初期化時にこれを使用してセキュリティで保護されたセッションを確立します。 代わりに、`authenticationMode` 属性に `SecureConversation` を設定します。  
  
 ブートストラップ バインディングを指定していない場合は、ブートストラップのために `SspiNegotiated` 認証モードが使用されます。  
  
### <a name="sspinegotiation"></a>SspiNegotiation  
 この認証モードでは、ネゴシエーション プロトコルを使用して、クライアントとサーバーの認証を行います。 Kerberos を使用できる場合は Kerberos が使用され、それ以外の場合は、NTLM (NT LanMan) が使用されます。 セキュリティ バインド要素は、`SymmetricSecurityBindingElement` メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSspiNegotiationBindingElement%2A> です。 代わりに、`authenticationMode` 属性に `SspiNegotiated` を設定します。  
  
### <a name="sspinegotiatedovertransport"></a>SspiNegotiatedOverTransport  
 この認証モードでは、ネゴシエーション プロトコルを使用して、クライアントとサーバーの認証を行います。 Kerberos プロトコルを使用できる場合は Kerberos プロトコルが使用され、それ以外の場合は NTLM が使用されます。 発行されたトークンは、保証サポート トークン、つまりメッセージ署名を行うトークンとして SOAP 層に表示されます。 サービスは、トランスポート層で X.509 証明書により追加的に認証されます。 セキュリティ バインド要素は、`TransportSecurityBindingElement` メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSspiNegotiationOverTransportBindingElement%2A> です。 代わりに、`authenticationMode` 属性に `SspiNegotiatedOverTransport` を設定します。  
  
### <a name="usernameforcertificate"></a>UserNameForCertificate  
 この認証モードでは、クライアントは、署名付きサポート トークン、つまりメッセージ署名で署名されたトークンとして SOAP 層に表示されるユーザー名トークンを使用してサービスに対する認証を行います。 X.509 証明書を使用したクライアントに対するサービス認証。 セキュリティ バインド要素は、`SymmetricSecurityBindingElement` メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateUserNameForCertificateBindingElement%2A> です。 代わりに、`authenticationMode` 属性に `UserNameForCertificate` を設定します。  
  
 `UserNameForCertificate` 認証モードでは、クライアントとサービスの両方が WS-Security 1.1 を使用している必要があります。  
  
### <a name="usernameforsslnegotiated"></a>UserNameForSslNegotiated  
 この認証モードでは、クライアントは、署名付きサポート トークン、つまりメッセージ署名で署名されたトークンとして SOAP 層に表示されるユーザー名トークンを使用して認証を行います。 サービスは X.509 証明書を使用して認証されます。 セキュリティ バインド要素は、`SymmetricSecurityBindingElement` メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateUserNameForSslBindingElement%2A> です。 代わりに、`authenticationMode` 属性に `UserNameForSslNegotiated` を設定します。  
  
### <a name="usernameovertransport"></a>UserNameOverTransport  
 この認証モードでは、クライアントは、署名付きサポート トークン、つまりメッセージ署名で署名されたトークンとして SOAP 層に表示されるユーザー名トークンを使用して認証を行います。 サービスはトランスポート層で X.509 証明書を使用して認証されます。 セキュリティ バインド要素は、`TransportSecurityBindingElement` メソッドによって返される <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateUserNameOverTransportBindingElement%2A> です。 代わりに、`authenticationMode` 属性に `UserNameOverTransport` を設定します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Channels.SecurityBindingElement>
- [方法 : 指定した認証モード用の SecurityBindingElement を作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)
