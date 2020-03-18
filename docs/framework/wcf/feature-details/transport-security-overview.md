---
title: トランスポート セキュリティの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 00959326-aa9d-44d0-af61-54933d4adc7f
ms.openlocfilehash: ba5f963ebb33da0aaf2c33c776fee7f226e52e85
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742711"
---
# <a name="transport-security-overview"></a>トランスポート セキュリティの概要
Windows Communication Foundation (WCF) のトランスポートセキュリティ機構は、使用されているバインディングとトランスポートによって異なります。 たとえば、<xref:System.ServiceModel.WSHttpBinding> クラスを使用する場合、トランスポートは HTTP であり、トランスポートをセキュリティで保護するための主要機構は SSL (Secure Sockets Layer) over HTTP (一般に HTTPS と呼ばれます) です。 このトピックでは、WCF システム指定のバインディングで使用される主要なトランスポートセキュリティ機構について説明します。  
  
> [!NOTE]
> .NET Framework 3.5 以降で SSL セキュリティが使用されている場合、WCF クライアントは、証明書ストア内の中間証明書と SSL ネゴシエーション中に受信した中間証明書の両方を使用して、サービスの証明書チェーンの検証を実行します。証明. .NET Framework 3.0 では、ローカルの証明書ストアにインストールされている中間証明書のみが使用されます。  
  
> [!WARNING]
> トランスポート セキュリティを使用した場合、<xref:System.Threading.Thread.CurrentPrincipal%2A?displayProperty=nameWithType> プロパティが上書きされることがあります。 この問題が発生しないようにするには、<xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.PrincipalPermissionMode%2A?displayProperty=nameWithType> を <xref:System.ServiceModel.Description.PrincipalPermissionMode.None?displayProperty=nameWithType>に設定します。 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> は、サービスの説明で設定できる、サービスの動作です。  
  
## <a name="basichttpbinding"></a>BasicHttpBinding  
 既定では、<xref:System.ServiceModel.BasicHttpBinding> クラスはセキュリティを提供しません。 このバインディングは、セキュリティを実装していない Web サービス プロバイダーとの相互運用性のためにデザインされています。 ただし、<xref:System.ServiceModel.BasicHttpSecurity.Mode%2A> プロパティを <xref:System.ServiceModel.BasicHttpSecurityMode.None> 以外の値に設定することにより、セキュリティを有効にすることができます。 トランスポート セキュリティを有効にするには、このプロパティを <xref:System.ServiceModel.BasicHttpSecurityMode.Transport> に設定します。  
  
### <a name="interoperation-with-iis"></a>IIS との相互運用性  
 <xref:System.ServiceModel.BasicHttpBinding> クラスは、主に既存の Web サービスと相互運用するために使用されます。これらのサービスの多くは、インターネット インフォメーション サービス (IIS) によってホストされます。 そのため、このバインディングのトランスポート セキュリティは、IIS サイトとシームレスに相互運用できるようにデザインされています。 IIS サイトと相互運用するには、セキュリティ モードを <xref:System.ServiceModel.BasicHttpSecurityMode.Transport> に設定した後、クライアント資格情報の種類を設定します。 資格情報の種類の値は、IIS ディレクトリのセキュリティ機構に対応しています。 モードを設定し、資格情報の種類を Windows に設定するコードを次に示します。 この構成は、クライアントとサーバーが同じ Windows ドメインに存在する場合に使用できます。  
  
 [!code-csharp[c_ProgrammingSecurity#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_programmingsecurity/cs/source.cs#10)] 
 [!code-vb[c_ProgrammingSecurity#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_programmingsecurity/vb/source.vb#10)]  
  
 または、次のように構成します。  
  
```xml  
<bindings>  
  <basicHttpBinding>  
    <binding name="SecurityByTransport">  
      <security mode="Transport">  
        <transport clientCredentialType="Windows" />  
       </security>  
     </binding>  
  </basicHttpBinding>  
</bindings>  
```  
  
 以下のセクションでは、その他のクライアント資格情報の種類について説明します。  
  
#### <a name="basic"></a>Basic  
 これは、IIS の基本認証方法に対応しています。 このモードを使用する場合は、Windows ユーザー アカウントと、適切な NTFS ファイル システムのアクセス許可を使用して IIS サーバーを構成する必要があります。 IIS 6.0 の詳細については、「[基本認証を有効にする」および「領域名を構成](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc785293(v=ws.10))する」を参照してください。 IIS 7.0 の詳細については、「[基本認証を構成する (iis 7)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772009(v=ws.10))」を参照してください。  
  
#### <a name="certificate"></a>Certificate  
 IIS には、クライアントに証明書を使用してログオンすることを要求するオプションがあります。 この機能により、IIS はクライアント証明書を Windows アカウントにマップすることもできます。 IIS 6.0 の詳細については、「 [iis 6.0 でクライアント証明書を有効にする](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc727994(v=ws.10))」を参照してください。 IIS 7.0 の詳細については、「 [iis 7 でサーバー証明書を構成する](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732230(v=ws.10))」を参照してください。  
  
#### <a name="digest"></a>ダイジェスト  
 ダイジェスト認証は基本認証と似ていますが、資格情報をクリア テキストではなくハッシュとして送信できるという利点があります。 IIS 6.0 の詳細については、「 [iis 6.0 でのダイジェスト認証](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc782661(v=ws.10))」を参照してください。 IIS 7.0 の詳細については、「[ダイジェスト認証を構成する (iis 7)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754104(v=ws.10))」を参照してください。  
  
#### <a name="windows"></a>Windows  
 これは、IIS の統合 Windows 認証に対応しています。 この値に設定する場合、サーバーは、Kerberos プロトコルを使用する Windows ドメインにドメイン コントローラーとして存在することにもなっています。 サーバーが Kerberos ベースのドメインに存在しない場合、または Kerberos システムに障害が発生した場合は、次のセクションで説明する NTLM (NT LAN Manager) 値を使用できます。 IIS 6.0 の詳細については、「 [iis 6.0 での統合 Windows 認証](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc738016(v=ws.10))」を参照してください。 IIS 7.0 の詳細については、「 [iis 7 でサーバー証明書を構成する](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732230(v=ws.10))」を参照してください。
  
#### <a name="ntlm"></a>NTLM  
 この値を使用すると、Kerberos プロトコルが失敗した場合に、サーバーは、NTLM を使用して認証を実行できます。 Iis 6.0 での IIS の構成の詳細については、「 [NTLM 認証を強制](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786486(v=ws.10))する」を参照してください。 IIS 7.0 では、Windows 認証に NTLM 認証が含まれています。 詳細については、「 [IIS 7 でサーバー証明書を構成する](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732230(v=ws.10))」を参照してください。
  
## <a name="wshttpbinding"></a>WsHttpBinding  
 <xref:System.ServiceModel.WSHttpBinding> クラスは、WS-* 仕様を実装するサービスと共に相互運用するようにデザインされています。 このバインディングのトランスポート セキュリティは、SSL (Secure Sockets Layer) over HTTP または HTTPS です。 SSL を使用する WCF アプリケーションを作成するには、IIS を使用してアプリケーションをホストします。 自己ホスト型アプリケーションを作成する場合は、HttpCfg.exe ツールを使用して、X.509 証明書をコンピューターの特定のポートにバインドします。 ポート番号は、WCF アプリケーションの一部としてエンドポイントアドレスとして指定されます。 トランスポート モードを使用する場合は、エンドポイント アドレスに HTTPS プロトコルを含める必要があります。そうしないと、実行時に例外が発生します。 詳細については、「 [HTTP トランスポートセキュリティ](../../../../docs/framework/wcf/feature-details/http-transport-security.md)」を参照してください。  
  
 クライアント認証の場合、<xref:System.ServiceModel.HttpTransportSecurity.ClientCredentialType%2A> クラスの <xref:System.ServiceModel.HttpTransportSecurity> プロパティを <xref:System.ServiceModel.HttpClientCredentialType> 列挙値のいずれかに設定します。 この列挙値は、<xref:System.ServiceModel.BasicHttpBinding> のクライアント資格情報の種類と同一であり、IIS サービスを使用してホストされるようにデザインされています。  
  
 クライアント資格情報の種類が Windows の場合に使用するバインディングの例を次に示します。  
  
 [!code-csharp[c_ProgrammingSecurity#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_programmingsecurity/cs/source.cs#11)]
 [!code-vb[c_ProgrammingSecurity#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_programmingsecurity/vb/source.vb#11)]  
  
## <a name="wsdualhttpbinding"></a>WSDualHttpBinding  
 このバインディングが提供するのは、トランスポート レベルのセキュリティではなく、メッセージ レベルのセキュリティだけです。  
  
## <a name="nettcpbinding"></a>NetTcpBinding  
 <xref:System.ServiceModel.NetTcpBinding> クラスは、メッセージ トランスポートに TCP を使用します。 トランスポート モードのセキュリティは、TLS (Transport Layer Security) over TCP を実装することによって実現されます。 TLS 実装は、オペレーティング システムによって提供されます。  
  
 次のコードに示すように、<xref:System.ServiceModel.TcpTransportSecurity.ClientCredentialType%2A> クラスの <xref:System.ServiceModel.TcpTransportSecurity> プロパティを <xref:System.ServiceModel.TcpClientCredentialType> 値のいずれかに設定することで、クライアントの資格情報の種類を指定することもできます。  
  
 [!code-csharp[c_ProgrammingSecurity#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_programmingsecurity/cs/source.cs#12)]
 [!code-vb[c_ProgrammingSecurity#12](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_programmingsecurity/vb/source.vb#12)]  
  
#### <a name="client"></a>Client  
 クライアントでは、<xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A> クラスの <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential> メソッドを使用して証明書を指定する必要があります。  
  
> [!NOTE]
> Windows セキュリティを使用している場合には、証明書は不要です。  
  
 証明書の拇印を使用するコードを次に示します。拇印により、証明書が一意に識別されます。 証明書の詳細については、「[証明書の使用](../../../../docs/framework/wcf/feature-details/working-with-certificates.md)」を参照してください。  
  
 [!code-csharp[c_ProgrammingSecurity#13](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_programmingsecurity/cs/source.cs#13)]
 [!code-vb[c_ProgrammingSecurity#13](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_programmingsecurity/vb/source.vb#13)]  
  
 または、[動作] セクションで[\<clientCredentials >](../../../../docs/framework/configure-apps/file-schema/wcf/clientcredentials.md)要素を使用して、クライアントの構成で証明書を指定します。  
  
```xml  
<behaviors>  
  <behavior>  
   <clientCredentials>  
     <clientCertificate findValue= "101010101010101010101010101010000000000"   
      storeLocation="LocalMachine" storeName="My"   
      X509FindType="FindByThumbPrint"/>  
     </clientCertificate>  
   </clientCredentials>  
 </behavior>  
</behaviors>    
```  
  
## <a name="netnamedpipebinding"></a>NetNamedPipeBinding  
 同じネットワーク上の 2 台のコンピューター間に名前付きパイプ チャネルを作成できますが、<xref:System.ServiceModel.NetNamedPipeBinding> クラスは、コンピューター内通信 (つまり、同じコンピューター上で実行されるプロセス) を効率的に行うことができるようにデザインされています。 このバインディングが提供するのは、トランスポート レベルのセキュリティだけです。 このバインディングを使用してアプリケーションを作成する場合は、エンドポイント アドレスにプロトコルとして "net.pipe" を含める必要があります。  
  
## <a name="wsfederationhttpbinding"></a>WSFederationHttpBinding  
 トランスポート セキュリティを使用する場合、このバインディングでは SSL over HTTP を使用します。SSL over HTTP は HTTPS とも呼ばれ、発行済みトークン (<xref:System.ServiceModel.WSFederationHttpSecurityMode.TransportWithMessageCredential>) を含みます。 フェデレーションアプリケーションの詳細については、「[フェデレーションと発行済みトークン](../../../../docs/framework/wcf/feature-details/federation-and-issued-tokens.md)」を参照してください。  
  
## <a name="netpeertcpbinding"></a>NetPeerTcpBinding  
 <xref:System.ServiceModel.NetPeerTcpBinding> クラスは、ピアツーピア ネットワーク機能を使用して効率的に通信できるようにデザインされた、セキュリティ保護されたトランスポートです。 クラスとバインディングの名前が示すように、プロトコルは TCP です。 セキュリティ モードが Transport に設定されている場合、このバインディングは TLS over TCP を実装します。 ピアツーピア機能の詳細については、「[ピアツーピアネットワーク](../../../../docs/framework/wcf/feature-details/peer-to-peer-networking.md)」を参照してください。  
  
## <a name="msmqintegrationbinding-and-netmsmqbinding"></a>MsmqIntegrationBinding と NetMsmqBinding  
 メッセージキュー (旧称 MSMQ) を使用したトランスポートセキュリティの詳細については、「[トランスポートセキュリティを使用したメッセージのセキュリティ保護](../../../../docs/framework/wcf/feature-details/securing-messages-using-transport-security.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [WCF セキュリティのプログラミング](../../../../docs/framework/wcf/feature-details/programming-wcf-security.md)
