---
title: Windows Communication Foundation のセキュリティ動作
ms.date: 03/30/2017
ms.assetid: 513232c0-39fd-4409-bda6-5ebd5e0ea7b0
ms.openlocfilehash: 12ae9bb90752fe3ee76404948693c501fc42efe6
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76730943"
---
# <a name="security-behaviors-in-wcf"></a>Windows Communication Foundation のセキュリティ動作
Windows Communication Foundation (WCF) では、動作によって、サービスレベルまたはエンドポイントレベルで実行時の動作が変更されます。 (一般的な動作の詳細については、「[サービスの実行時の動作の指定](../../../../docs/framework/wcf/specifying-service-run-time-behavior.md)」を参照してください)。*セキュリティの動作*により、資格情報、認証、承認、および監査ログの制御が可能になります。 動作は、プログラムまたは構成を通じて使用できます。 ここでは、セキュリティ機能に関連する以下の動作の構成について説明します。  
  
- [serviceCredentials > を\<](../../../../docs/framework/configure-apps/file-schema/wcf/servicecredentials.md)します。  
  
- [clientCredentials >\<](../../../../docs/framework/configure-apps/file-schema/wcf/clientcredentials.md)ます。  
  
- [\<serviceAuthorization >](../../../../docs/framework/configure-apps/file-schema/wcf/serviceauthorization-element.md)。  
  
- [serviceSecurityAudit > を\<](../../../../docs/framework/configure-apps/file-schema/wcf/servicesecurityaudit.md)します。  
  
- [\<serviceMetadata >](../../../../docs/framework/configure-apps/file-schema/wcf/servicemetadata.md)。これにより、クライアントがメタデータにアクセスできるセキュリティで保護されたエンドポイントを指定することもできます。  
  
## <a name="setting-credentials-with-behaviors"></a>動作を使用した資格情報の設定  
 [\<serviceCredentials >](../../../../docs/framework/configure-apps/file-schema/wcf/servicecredentials.md)と[\<clientCredentials >](../../../../docs/framework/configure-apps/file-schema/wcf/clientcredentials.md)を使用して、サービスまたはクライアントの資格情報の値を設定します。 基になるバインド構成によって、資格情報を設定する必要があるかどうかが決まります。 たとえば、セキュリティ モードが `None` に設定されている場合、クライアントとサービスは相互に認証を行わないため、どの種類の資格情報も必要ありません。  
  
 一方、サービス バインディングでは、クライアント資格情報の種類が必要になることがあります。 その場合は、動作を使用して資格情報の値を設定することが必要になる場合があります (使用できる資格情報の種類の詳細については、「資格情報の[種類の選択](../../../../docs/framework/wcf/feature-details/selecting-a-credential-type.md)」を参照してください)。場合によっては、Windows 資格情報を使用して認証するときに、環境によって実際の資格情報の値が自動的に設定されます。資格情報の別のセットを指定しない限り、資格情報の値を明示的に設定する必要はありません。  
  
 すべてのサービス資格情報は、 [\<serviceBehaviors >](../../../../docs/framework/configure-apps/file-schema/wcf/servicebehaviors.md)の子要素として検出されます。 サービス資格情報として使用される証明書を次の例に示します。  
  
```xml  
<configuration>  
 <system.serviceModel>  
  <behaviors>  
   <serviceBehaviors>  
    <behavior name="ServiceBehavior1">  
     <serviceCredentials>  
      <serviceCertificate findValue="000000000000000000000000000"   
                          storeLocation="CurrentUser"  
      storeName="Root" x509FindType="FindByThumbprint" />  
     </serviceCredentials>  
    </behavior>  
   </serviceBehaviors>  
  </behaviors>  
 </system.serviceModel>  
</configuration>  
```  
  
## <a name="service-credentials"></a>サービス資格情報  
 [\<serviceCredentials >](../../../../docs/framework/configure-apps/file-schema/wcf/servicecredentials.md)には、4つの子要素が含まれています。 以下のセクションでは、これらの要素とそれぞれの使い方について説明します。  
  
### <a name="servicecertificate-element"></a>\<serviceCertificate > 要素  
 メッセージ セキュリティ モードを使用しているクライアントへのサービスの認証に使用する X.509 証明書を指定するには、この要素を使用します。 定期的に更新される証明書を使用している場合は、証明書のサムプリントが変更されます。 その場合、証明書を同じサブジェクト名で再発行できるため、`X509FindType` としてサブジェクト名を使用します。  
  
 要素の使用方法の詳細については、「[方法: クライアント資格情報の値を指定する](../../../../docs/framework/wcf/how-to-specify-client-credential-values.md)」を参照してください。  
  
### <a name="certificate-of-clientcertificate-element"></a>\<clientCertificate > 要素の \<証明書 >  
 サービスがクライアントと安全に通信するためにクライアントの証明書を事前に持っている必要がある場合は、 [\<certificate >](../../../../docs/framework/configure-apps/file-schema/wcf/certificate-of-clientcertificate-element.md)要素を使用します。 このような状況は、双方向通信パターンを使用する場合に生じます。 より一般的な要求/応答パターンでは、クライアントが自身の証明書を要求に含め、サービスはこの証明書を使用して、クライアントへの応答をセキュリティで保護します。 ただし、双方向通信パターンには要求も応答もありません。 サービスは、クライアントの証明書を通信から推測できないため、クライアントへのメッセージをセキュリティで保護するためにクライアントの証明書が前もって必要になります。 クライアントの証明書を帯域外方式で取得し、この要素を使用して証明書を指定する必要があります。 双方向サービスの詳細については、「[方法: 双方向コントラクトを作成](../../../../docs/framework/wcf/feature-details/how-to-create-a-duplex-contract.md)する」を参照してください。  
  
### <a name="authentication-of-clientcertificate-element"></a>\<clientCertificate > 要素の認証 > を \<  
 [\<authentication >](../../../../docs/framework/configure-apps/file-schema/wcf/authentication-of-clientcertificate-element.md)要素を使用すると、クライアントの認証方法をカスタマイズできます。 `CertificateValidationMode` 属性は、`None`、`ChainTrust`、`PeerOrChainTrust`、`PeerTrust`、または `Custom` に設定できます。 既定では、レベルは `ChainTrust`に設定されます。これにより、各証明書は、チェーンの最上位にある*ルート証明機関*で終了する証明書の階層構造で見つかる必要があることを指定します。 これは最もセキュリティで保護されているモードです。 また、値を `PeerOrChainTrust` に設定することもできます。これは、信頼されたチェーン内の証明書と共に、自己発行された証明書 (ピア信頼) も受け入れるよう指定します。 自己発行の資格情報は信頼された証明機関から購入したものである必要はないため、この値はクライアントとサービスの開発およびデバッグに使用されます。 クライアントを展開するときは、代わりに `ChainTrust` 値を使用します。 また、値を `Custom` に設定することもできます。 `Custom` 値に設定する場合は、`CustomCertificateValidatorType` 属性を、証明書の検証に使用するアセンブリと型に設定する必要もあります。 独自のカスタム検証を作成するには、抽象 <xref:System.IdentityModel.Selectors.X509CertificateValidator> クラスを継承する必要があります。  
  
### <a name="issuedtokenauthentication-element"></a>\<issuedTokenAuthentication > 要素  
 発行されるトークンのシナリオには、3 つの段階があります。 最初の段階では、サービスにアクセスしようとしているクライアントは、*セキュリティトークンサービス*(STS) と呼ばれます。 次に、STS がクライアントを認証し、その後、クライアントにトークン (通常は、SAML (Security Assertions Markup Language) トークン) を発行します。 最後に、クライアントがトークンを持ってサービスに戻ります。 サービスはトークンを調べ、トークンを認証することでクライアントの認証を可能にするデータを確認します。 トークンを認証するには、セキュリティ トークン サービスで使用される証明書がサービスによって認識されている必要があります。 [\<issuedTokenAuthentication >](../../../../docs/framework/configure-apps/file-schema/wcf/issuedtokenauthentication-of-servicecredentials.md)要素は、このようなセキュリティトークンサービス証明書のリポジトリです。 証明書を追加するには、 [\<knowncertificates >](../../../../docs/framework/configure-apps/file-schema/wcf/knowncertificates.md)を使用します。 次の例に示すように、証明書ごとに[> を追加\<](../../../../docs/framework/configure-apps/file-schema/wcf/add-of-knowncertificates.md)を挿入します。  
  
```xml  
<issuedTokenAuthentication>  
   <knownCertificates>  
      <add findValue="www.contoso.com"   
           storeLocation="LocalMachine" storeName="My"   
           X509FindType="FindBySubjectName" />  
    </knownCertificates>  
</issuedTokenAuthentication>  
```  
  
 既定では、証明書はセキュリティ トークン サービスから取得する必要があります。 このような "既知" の証明書により、正当なクライアントのみがサービスにアクセスできるようになります。  
  
 `SamlSecurityToken` セキュリティトークンを発行する*セキュリティトークンサービス*(STS) を利用するフェデレーションアプリケーションでは、 [\<allowedAudienceUris >](../../../../docs/framework/configure-apps/file-schema/wcf/allowedaudienceuris.md)コレクションを使用する必要があります。 STS がセキュリティ トークンを発行する場合、このセキュリティ トークンに `SamlAudienceRestrictionCondition` を追加して、セキュリティ トークンの提供先となる Web サービスの URI を指定できます。 これにより、受け取り側 `SamlSecurityTokenAuthenticator` Web サービスは、次のようにしてこのチェックが行われるように指定することで、発行されたセキュリティ トークンがこの Web サービスを対象としていることを確認できます。  
  
- [\<issuedTokenAuthentication >](../../../../docs/framework/configure-apps/file-schema/wcf/issuedtokenauthentication-of-servicecredentials.md)の `audienceUriMode` 属性を `Always` または `BearerKeyOnly`に設定します。  
  
- このコレクションに URI を追加して、有効な URI のセットを指定します。 これを行うには、URI ごとに[> を追加\<](../../../../docs/framework/configure-apps/file-schema/wcf/add-of-allowedaudienceuris.md)を挿入します。  
  
 詳細については、<xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator> を参照してください。  
  
 この構成要素の使用方法の詳細については、「[方法: フェデレーションサービスで資格情報を構成する](../../../../docs/framework/wcf/feature-details/how-to-configure-credentials-on-a-federation-service.md)」を参照してください。  
  
#### <a name="allowing-anonymous-cardspace-users"></a>匿名の CardSpace ユーザーの許可  
 `AllowUntrustedRsaIssuers` 要素の `<IssuedTokenAuthentication>` 属性を `true` に設定すると、任意の RSA キー ペアで署名された自己発行トークンを提示することがすべてのクライアントに明示的に許可されます。 発行者は、キーに発行者データが関連付けられていないため、*信頼*されていません。 CardSpace ユーザーは、id の自己提供のクレームを含む自己発行のカードを作成できます。 この機能を使用するときは十分に注意してください。 この機能を使用する場合は、RSA 公開キーを、ユーザー名と一緒にデータベースに格納する必要のある比較的安全なパスワードとして考えます。 サービスへのクライアント アクセスを許可する前に、クライアントから提示された RSA 公開キーを、提示されたユーザー名に対応する格納済みの公開キーと比較して検証します。 これは、ユーザーが各自のユーザー名を登録し、自己発行の RSA 公開キーに関連付けることができる登録プロセスが確立されていることが前提となります。  
  
## <a name="client-credentials"></a>クライアントの資格情報  
 クライアント資格情報は、相互認証が必要な場合にサービスに対するクライアントの認証に使用されます。 また、このセクションを使用して、クライアントがサービスの証明書によってサービスへのメッセージをセキュリティで保護する必要がある場合に使用するサービス証明書を指定することもできます。  
  
 セキュリティ トークン サービスまたはローカル発行者から発行されたトークンを使用するフェデレーション シナリオの一部として、クライアントを構成することもできます。 フェデレーションシナリオの詳細については、「[フェデレーションと発行済みトークン](../../../../docs/framework/wcf/feature-details/federation-and-issued-tokens.md)」を参照してください。 すべてのクライアント資格情報は、次のコードに示すように、 [\<endpointBehaviors >](../../../../docs/framework/configure-apps/file-schema/wcf/endpointbehaviors.md)の下にあります。  
  
```xml  
<behaviors>  
 <endpointBehaviors>  
  <behavior name="EndpointBehavior1">  
   <clientCredentials>  
    <clientCertificate findValue="cn=www.contoso.com"     
        storeLocation="LocalMachine"  
        storeName="AuthRoot" x509FindType="FindBySubjectName" />  
    <serviceCertificate>  
     <defaultCertificate findValue="www.cohowinery.com"   
                    storeLocation="LocalMachine"  
                    storeName="Root" x509FindType="FindByIssuerName" />  
    <authentication revocationMode="Online"  
                    trustedStoreLocation="LocalMachine" />  
    </serviceCertificate>  
   </clientCredentials>  
  </behavior>  
 </endpointBehaviors>  
```  
  
#### <a name="clientcertificate-element"></a>\<clientCertificate > 要素  
 この要素で、クライアントの認証に使用する証明書を設定します。 詳細については、「[方法: クライアント資格情報の値を指定する](../../../../docs/framework/wcf/how-to-specify-client-credential-values.md)」を参照してください。  
  
#### <a name="httpdigest"></a>\<httpDigest >  
 この機能は、Windows の Active Directory およびインターネット インフォメーション サービス (IIS) と共に有効にする必要があります。 詳細については、「 [IIS 6.0 でのダイジェスト認証](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc782661(v=ws.10))」を参照してください。  
  
#### <a name="issuedtoken-element"></a>\<issuedToken > 要素  
 [\<issuedToken >](../../../../docs/framework/configure-apps/file-schema/wcf/issuedtoken.md)には、トークンのローカル発行者、または Security Token Service で使用される動作を構成するために使用される要素が含まれています。 ローカル発行者を使用するようにクライアントを構成する方法については、「[方法: ローカル発行者を構成](../../../../docs/framework/wcf/feature-details/how-to-configure-a-local-issuer.md)する」を参照してください。  
  
#### <a name="localissueraddress"></a>\<localIssuerAddress >  
 既定のセキュリティ トークン サービス アドレスを指定します。 これは、<xref:System.ServiceModel.WSFederationHttpBinding> が Security Token Service の URL を指定しない場合、またはフェデレーションバインディングの発行者アドレスが `http://schemas.microsoft.com/2005/12/ServiceModel/Addressing/Anonymous` または `null`場合に使用されます。 そのような場合、<xref:System.ServiceModel.Description.ClientCredentials> は、ローカルの発行者およびバインディングのアドレスと共に構成し、その発行者と通信するために使用する必要があります。  
  
#### <a name="issuerchannelbehaviors"></a>\<issuerChannelBehaviors >  
 Security Token Service との通信時に使用される WCF クライアントの動作を追加するには、 [\<issuerChannelBehaviors >](../../../../docs/framework/configure-apps/file-schema/wcf/issuerchannelbehaviors-element.md)を使用します。 [\<endpointbehaviors >](../../../../docs/framework/configure-apps/file-schema/wcf/endpointbehaviors.md)セクションでクライアントの動作を定義します。 定義された動作を使用するには、2つの属性を持つ `<issuerChannelBehaviors>` 要素に <`add`> 要素を追加します。 次の例に示すように、`issuerAddress` をセキュリティ トークン サービスの URL に設定し、`behaviorConfiguration` 属性を定義済みのエンドポイントの動作の名前に設定します。  
  
```xml  
<clientCredentials>  
   <issuedToken>  
      <issuerChannelBehaviors>  
         <add issuerAddress="http://www.contoso.com"  
               behaviorConfiguration="clientBehavior1" />     
```  
  
#### <a name="servicecertificate-element"></a>\<serviceCertificate > 要素  
 サービス証明書の認証を制御するには、この要素を使用します。  
  
 [\<defaultCertificate >](../../../../docs/framework/configure-apps/file-schema/wcf/defaultcertificate-element.md)要素には、サービスで証明書が指定されていない場合に使用する1つの証明書を格納できます。  
  
 [\<scopedCertificates >](../../../../docs/framework/configure-apps/file-schema/wcf/scopedcertificates-element.md)を使用し、 [> を追加\<](../../../../docs/framework/configure-apps/file-schema/wcf/add-of-scopedcertificates-element.md)て、特定のサービスに関連付けられているサービス証明書を設定します。 `<add>` 要素には、証明書をサービスに関連付けるために使用する `targetUri` 属性があります。  
  
 [\<authentication >](../../../../docs/framework/configure-apps/file-schema/wcf/authentication-of-servicecertificate-element.md)要素は、証明書の認証に使用される信頼レベルを指定します。 既定のレベルは "ChainTrust" に設定され、チェーンの最上位の信頼された証明機関で終了する証明書の階層構造で各証明書を検索するよう指定します。 これは最もセキュリティで保護されているモードです。 また、値を "PeerOrChainTrust" に設定することもできます。これは、信頼されたチェーン内の証明書と共に、自己発行された証明書 (ピア信頼) も受け入れることを指定します。 自己発行の資格情報は信頼された証明機関から購入したものである必要はないため、この値はクライアントとサービスの開発およびデバッグに使用されます。 クライアントを展開するときは、代わりに "ChainTrust" 値を使用します。 値を "Custom" または "None" に設定できます。 "Custom" 値を使用するには、`CustomCertificateValidatorType` 属性も証明書の検証に使用するアセンブリと型に設定する必要があります。 独自のカスタム検証を作成するには、抽象 <xref:System.IdentityModel.Selectors.X509CertificateValidator> クラスを継承する必要があります。 詳細については、「[方法: カスタム証明書検証を使用するサービスを作成](../../../../docs/framework/wcf/extending/how-to-create-a-service-that-employs-a-custom-certificate-validator.md)する」を参照してください。  
  
 [\<authentication >](../../../../docs/framework/configure-apps/file-schema/wcf/authentication-of-servicecertificate-element.md)要素には、証明書の失効を確認する方法を指定する `RevocationMode` 属性が含まれています。 既定値は "online" です。この場合、証明書が失効していないかどうかが自動的にチェックされます。 詳細については、「[証明書の使用](../../../../docs/framework/wcf/feature-details/working-with-certificates.md)」を参照してください。  
  
## <a name="serviceauthorization"></a>ServiceAuthorization  
 [\<serviceAuthorization >](../../../../docs/framework/configure-apps/file-schema/wcf/serviceauthorization-element.md)要素には、承認、カスタムロールプロバイダー、および偽装に影響する要素が含まれています。  
  
 <xref:System.Security.Permissions.PrincipalPermissionAttribute> クラスは、サービス メソッドに適用されます。 この属性は、保護メソッドの使用を承認するときに使用するユーザー グループを指定します。 既定値は "UseWindowsGroups" で、リソースにアクセスしようとしている ID を Windows グループ ("管理者" や "ユーザー" など) で検索するよう指定します。 次のコードに示すように、"UseAspNetRoles" を指定して、<`system.web` > 要素の下に構成されているカスタムロールプロバイダーを使用することもできます。  
  
```xml  
<system.web>  
  <membership defaultProvider="SqlProvider"   
   userIsOnlineTimeWindow="15">  
     <providers>  
       <clear />  
       <add   
          name="SqlProvider"   
          type="System.Web.Security.SqlMembershipProvider"   
          connectionStringName="SqlConn"  
          applicationName="MembershipProvider"  
          enablePasswordRetrieval="false"  
          enablePasswordReset="false"  
          requiresQuestionAndAnswer="false"  
          requiresUniqueEmail="true"  
          passwordFormat="Hashed" />  
     </providers>  
   </membership>  
  <!-- Other configuration code not shown.-->  
</system.web>  
```  
  
 `roleProviderName` 属性で `principalPermissionMode` を使用する方法を次のコードに示します。  
  
```xml  
<behaviors>  
 <behavior name="ServiceBehaviour">          
  <serviceAuthorization principalPermissionMode ="UseAspNetRoles"   
                        roleProviderName ="SqlProvider" />  
</behavior>   
   <!-- Other configuration code not shown. -->  
</behaviors>  
```  
  
## <a name="configuring-security-audits"></a>セキュリティ監査の構成  
 [\<serviceSecurityAudit >](../../../../docs/framework/configure-apps/file-schema/wcf/servicesecurityaudit.md)を使用して、書き込まれるログとログに記録するイベントの種類を指定します。 詳細については、「[監査](../../../../docs/framework/wcf/feature-details/auditing-security-events.md)」を参照してください。  
  
```xml  
<system.serviceModel>  
<serviceBehaviors>  
  <behavior name="NewBehavior">  
    <serviceSecurityAudit auditLogLocation="Application"   
             suppressAuditFailure="true"  
             serviceAuthorizationAuditLevel="Success"   
             messageAuthenticationAuditLevel="Success" />  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
## <a name="secure-metadata-exchange"></a>セキュリティで保護されたメタデータ交換  
 メタデータをクライアントにエクスポートすると、構成やクライアント コードのダウンロードが可能になるため、サービスとクライアントの開発者にとって便利です。 サービスが悪意のあるユーザーに公開されないようにするために、SSL over HTTP (HTTPS) 機構を使用して転送をセキュリティで保護できます。 転送を保護するには、まず、サービスをホストしているコンピューターの特定のポートに適切な X.509 証明書をバインドする必要があります。 (詳細については、「[証明書の使用](../../../../docs/framework/wcf/feature-details/working-with-certificates.md)」を参照してください)。次に、サービス構成に[\<serviceMetadata >](../../../../docs/framework/configure-apps/file-schema/wcf/servicemetadata.md)を追加し、`HttpsGetEnabled` 属性を `true`に設定します。 最後に、次の例に示すように、`HttpsGetUrl` 属性をサービス メタデータ エンドポイントの URL に設定します。  
  
```xml  
<behaviors>  
 <serviceBehaviors>  
  <behavior name="NewBehavior">  
    <serviceMetadata httpsGetEnabled="true"   
     httpsGetUrl="https://myComputerName/myEndpoint" />  
  </behavior>  
 </serviceBehaviors>  
</behaviors>  
```  
  
## <a name="see-also"></a>参照

- [監査](../../../../docs/framework/wcf/feature-details/auditing-security-events.md)
- [Windows Server App Fabric のセキュリティモデル](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
