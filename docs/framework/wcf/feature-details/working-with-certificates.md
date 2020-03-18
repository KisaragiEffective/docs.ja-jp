---
title: 証明書の使用
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- certificates [WCF]
ms.assetid: 6ffb8682-8f07-4a45-afbb-8d2487e9dbc3
ms.openlocfilehash: e38ead0d378092af086218277fd2e85b4a6396c3
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746888"
---
# <a name="working-with-certificates"></a>証明書の使用

Windows Communication Foundation (WCF) のセキュリティをプログラミングする場合、一般に X.509 デジタル証明書を使用して、クライアントとサーバーの認証、暗号化、およびメッセージのデジタル署名を行います。 ここでは、X.509 デジタル証明書の機能および WCF でのそれらの機能の使用方法について簡単に説明します。また、これらの概念の詳細を説明するトピックや、WCF と証明書を使用した一般的なタスクの実行方法が記載されたトピックへのリンクも示します。

簡単に言うと、デジタル証明書は、"*公開キー基盤 (PKI: Public Key Infrastructure)* " の一部です。PKI は、デジタル証明書、証明機関、およびその他の登録機関から成るシステムです。登録機関では、公開キー暗号化を使用して、電子取引に関与する各当事者の有効性の検証と認証を行います。 証明機関は証明書を発行します。各証明書には、"*サブジェクト*" (証明書の発行先のエンティティ)、有効期間 (証明書が有効な場合)、発行者 (証明書を発行したエンティティ)、公開キーなどのデータが含まれた一連のフィールドがあります。 WCF では、これらの各プロパティは <xref:System.IdentityModel.Claims.Claim> (クレーム) として処理されます。各クレームは、さらに ID と権限の 2 種類に分けられます。 X.509 証明書の詳細については、「[X.509 Public Key Certificates](/windows/desktop/SecCertEnroll/about-x-509-public-key-certificates)」(X.509 公開キー証明書) を参照してください。 WCF におけるクレームと承認の詳細については、「[ID モデルを使用したクレームと承認の管理](managing-claims-and-authorization-with-the-identity-model.md)」を参照してください。 PKI の実装の詳細については、「 [Windows Server 2012 R2 でのエンタープライズ pki Active Directory 証明書サービス](https://docs.microsoft.com/archive/blogs/yungchou/enterprise-pki-with-windows-server-2012-r2-active-directory-certificate-services-part-1-of-2)」を参照してください。

証明書の第一の機能は、他者に対して証明書の所有者の ID を認証することです。 証明書は所有者の "*公開キー*" を含んでおり、所有者が秘密キーを保持しています。 公開キーを使用して、証明書の所有者に送信されるメッセージを暗号化できます。 秘密キーにアクセスできるのは所有者だけであるため、所有者だけが暗号化されたメッセージを復号化できます。

証明書は、証明機関によって発行される必要があります。多くの場合、証明機関はサードパーティの証明書発行者です。 Windows ドメインでは、そのドメインのコンピューターに対して証明書を発行する際に使用できる証明機関が含まれています。

## <a name="viewing-certificates"></a>証明書の表示

証明書を使用するには、多くの場合、証明書を表示し、プロパティを確認する必要があります。 Microsoft 管理コンソール (MMC: Microsoft Management Console) スナップイン ツールを使用すると、これを簡単に行うことができます。 詳細については、「 [方法: MMC スナップインを使用して証明書を参照する](how-to-view-certificates-with-the-mmc-snap-in.md)」をご覧ください。

## <a name="certificate-stores"></a>証明書ストア

証明書はストアに格納されています。 2 つの主要なストアがあり、さらにサブストアに分かれています。 コンピューターの管理者は、MMC スナップイン ツールを使用して、両方の主要なストアを表示できます。 管理者以外のユーザーは、現在のユーザー ストアだけを表示できます。

- **ローカル コンピューター ストア**: これには、ASP.NET などのコンピュータープロセスによってアクセスされる証明書が含まれます。 クライアントに対してサーバーを認証するための証明書は、ここに格納します。

- **現在のユーザー ストア**: コンピューターの現在のユーザーの証明書は、通常、対話型アプリケーションによってここに配置されます。 クライアント アプリケーションを作成する場合、サービスに対してユーザーを認証するための証明書は、通常、ここに配置します。

上記の 2 つのストアは、さらにサブストアに分かれています。 サブストアの中で、WCF でプログラミングするときに最も重要なものは次のとおりです。

- **信頼されたルート証明機関**: このストア内の証明書を使用して、証明書チェーンを作成できます。証明書チェーンをさかのぼることで、このストア内の任意の証明機関証明書に到達できます。

  > [!IMPORTANT]
  > 証明書が信頼されたサードパーティ証明機関から発行されたものではない場合でも、ローカル コンピューターは、このストアに配置された証明書を暗黙で信頼します。 したがって、発行者を完全に信頼し、かつ信頼することの結果を理解していない限り、このストアに証明書を配置しないでください。

- **個人**: このストアは、コンピューターのユーザーに関連付けられた証明書に使用されます。 通常、このストアは、信頼されたルート証明機関ストアにある証明機関証明書のいずれかによって発行された証明書に使用されます。 また、自己発行され、アプリケーションから信頼された証明書が格納されている場合もあります。

証明書ストアの詳細については、「[Certificate Stores](/windows/desktop/secauthn/certificate-stores)」(証明書ストア) を参照してください。

### <a name="selecting-a-store"></a>ストアの選択

証明書を格納する場所の選択は、サービスまたはクライアントの実行方法や実行する状況によって異なります。 次の一般規則が適用されます。

- WCF サービスが Windows サービスでホストされる場合は、**ローカル コンピューター** ストアを使用します。 ローカル コンピューター ストアに証明書をインストールするには、管理特権が必要です。

- サービスまたはクライアントがユーザー アカウントで実行されるアプリケーションである場合は、**現在のユーザー** ストアを使用します。

### <a name="accessing-stores"></a>ストアへのアクセス

ストアは、コンピューター上の一種のフォルダーであり、アクセス制御リスト (ACL: Access Control List) によって保護されています。 インターネットインフォメーションサービス (IIS) でホストされるサービスを作成する場合、ASP.NET プロセスは ASP.NET アカウントで実行されます。 このアカウントは、サービスが使用する証明書を格納するストアにアクセス可能である必要があります。 各主要ストアは既定のアクセス リストで保護されていますが、これらのリストは変更できます。 ストアにアクセスする別のロールを作成した場合、そのロールにアクセス許可を付与する必要があります。 WinHttpCertConfig.exe ツールを使用してアクセス リストを変更する方法については、「[方法: 開発中に使用する一時的な証明書を作成する](how-to-create-temporary-certificates-for-use-during-development.md)」を参照してください。

## <a name="chain-trust-and-certificate-authorities"></a>信頼チェーンと証明機関

証明書は、各証明書がその発行元の CA にリンクされる階層構造で作成されます。 このリンクは CA の証明書へのリンクになります。 CA の証明書は、元の CA の証明書を発行した CA にリンクされます。 ルート CA の証明書に到達するまでこのプロセスが繰り返されます。 ルート CA の証明書は本質的に信頼されています。

デジタル証明書を使用する場合、この階層 ("*信頼チェーン*" とも呼ばれます) に依存してエンティティを認証します。 MMC スナップインを使用して証明書のチェーンを表示するには、任意の証明書をダブルクリックし、 **[証明書のパス]** タブをクリックします。証明機関の証明書チェーンをインポートする方法の詳細については、「[方法: 署名の検証に使用する証明機関の証明書チェーンを指定](specify-the-certificate-authority-chain-verify-signatures-wcf.md)する」を参照してください。

> [!NOTE]
> 証明書を "信頼されたルート証明機関" 証明書ストアに配置することにより、その証明書の発行者を信頼されたルート証明機関として指定できます。

### <a name="disabling-chain-trust"></a>信頼チェーンの無効化

新しいサービスの作成時には、信頼されたルート証明書によって発行されていない証明書を使用できます。また、発行する証明書が、信頼されたルート証明機関ストアになくてもかまいません。 開発だけを目的としている場合は、証明書の信頼チェーンをチェックする機構を一時的に無効にできます。 これを行うには、`CertificateValidationMode` プロパティを `PeerTrust` または `PeerOrChainTrust` に設定します。 これらのモードにより、証明書を自己発行するか (ピア信頼)、信頼チェーンに含めるかを指定できます。 このプロパティは、次のどのクラスでも設定できます。

|クラス|プロパティ|
|-----------|--------------|
|<xref:System.ServiceModel.Security.X509ClientCertificateAuthentication>|<xref:System.ServiceModel.Security.X509ClientCertificateAuthentication.CertificateValidationMode%2A?displayProperty=nameWithType>|
|<xref:System.ServiceModel.Security.X509PeerCertificateAuthentication>|<xref:System.ServiceModel.Security.X509PeerCertificateAuthentication.CertificateValidationMode%2A?displayProperty=nameWithType>|
|<xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication>|<xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication.CertificateValidationMode%2A?displayProperty=nameWithType>|
|<xref:System.ServiceModel.Security.IssuedTokenServiceCredential>|<xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A?displayProperty=nameWithType>|

構成を使用して、このプロパティを設定することもできます。 検証モードを指定するには、次の要素を使用します。

- [\<authentication>](../../../../docs/framework/configure-apps/file-schema/wcf/authentication-of-servicecertificate-element.md)

- [\<peerAuthentication>](../../../../docs/framework/configure-apps/file-schema/wcf/peerauthentication-element.md)

- [\<messageSenderAuthentication>](../../../../docs/framework/configure-apps/file-schema/wcf/messagesenderauthentication-element.md)

## <a name="custom-authentication"></a>カスタム認証

`CertificateValidationMode` プロパティを使用すると、証明書の認証方法をカスタマイズすることもできます。 既定では、レベルは `ChainTrust` に設定されています。 <xref:System.ServiceModel.Security.X509CertificateValidationMode.Custom> 値を使用するには、`CustomCertificateValidatorType` 属性を証明書の検証に使用するアセンブリと型に設定することも必要です。 カスタム検証を作成するには、抽象 <xref:System.IdentityModel.Selectors.X509CertificateValidator> クラスから継承する必要があります。

カスタム認証システムを作成する場合、オーバーライドする最も重要なメソッドは <xref:System.IdentityModel.Selectors.X509CertificateValidator.Validate%2A> メソッドです。 カスタム認証の例については、「[X.509 証明書検証](../../../../docs/framework/wcf/samples/x-509-certificate-validator.md)」のサンプルを参照してください。 詳細については、「[カスタム資格情報と資格情報の検証](../../../../docs/framework/wcf/extending/custom-credential-and-credential-validation.md)」を参照してください。

## <a name="using-the-powershell-new-selfsignedcertificate-cmdlet-to-build-a-certificate-chain"></a>PowerShell の新しい SelfSignedCertificate コマンドレットを使用して証明書チェーンを構築する

PowerShell の新しい SelfSignedCertificate コマンドレットは、x.509 証明書と秘密キー/公開キーのペアを作成します。 秘密キーをディスクに保存し、新しい証明書の発行と署名に使用できるため、チェーンになった証明書の階層をシミュレートできます。 コマンドレットは、サービスを開発する際の補助としてのみ使用することを目的としており、実際の展開用の証明書の作成には使用しないでください。 WCF サービスの開発時には、次の手順に従って、新しい-SelfSignedCertificate コマンドレットを使用して信頼チェーンを構築します。

#### <a name="to-build-a-chain-of-trust-with-the-new-selfsignedcertificate-cmdlet"></a>新しい-SelfSignedCertificate コマンドレットを使用して信頼チェーンを構築するには

1. 新しい SelfSignedCertificate コマンドレットを使用して、一時的なルート証明機関 (自己署名) 証明書を作成します。 秘密キーをディスクに保存します。

2. この新しい証明書を使用して、公開キーを含む別の証明書を発行します。

3. 信頼されたルート証明機関ストアに、ルート証明機関証明書をインポートします。

4. 詳しい手順については、「[方法: 開発中に使用する一時的な証明書を作成する](how-to-create-temporary-certificates-for-use-during-development.md)」を参照してください。

## <a name="which-certificate-to-use"></a>使用する証明書の選択

証明書に関する一般的な質問は、どの証明書を使用するかということとその理由に関するものです。 その答えは、クライアントとサービスのどちらをプログラミングするかによって異なります。 以下に一般的なガイドラインを示します (これらの質問に対する包括的な答えではありません)。

### <a name="service-certificates"></a>サービス証明書

サービス証明書の第一の目的は、クライアントに対してサーバーを認証することです。 クライアントがサーバーを認証するときの最初のチェックの 1 つとして、"**サブジェクト**" フィールドの値とサービスへのアクセスに使用する URI (Uniform Resource Identifier) が比較されます。この場合、双方の DNS が一致する必要があります。 たとえば、サービスの URI が `http://www.contoso.com/endpoint/` 場合、 **Subject**フィールドにも `www.contoso.com`値が含まれている必要があります。

このフィールドには複数の値を含めることができますが、各値の先頭にはその値を示す初期化コードが付加されます。 一般的に、初期化は一般的な名前の "CN" です。たとえば、`CN = www.contoso.com`のようになります。 "**サブジェクト**" フィールドを空白にすることもできます。この場合、"**サブジェクト代替名**" フィールドに値として **DNS 名**を含めることができます。

また、証明書の "**目的**" フィールドの値に、適切な値 ("サーバー認証" や "クライアント認証" など) を含める必要があります。

### <a name="client-certificates"></a>クライアント証明書

通常、クライアント証明書はサードパーティ証明機関によって発行されません。 通常は、"クライアント認証" という目的で、ルート証明機関によって、この証明書が現在のユーザー ストアの個人ストアに配置されます。 相互認証が必要な場合、クライアントはこのような証明書を使用できます。

## <a name="online-revocation-and-offline-revocation"></a>オンラインの失効とオフラインの失効

### <a name="certificate-validity"></a>証明書の有効期間

すべての証明書は、"*有効期間*" と呼ばれる一定の期間にのみ有効です。 有効期間は、X.509 証明書の "**有効期間の開始**" フィールドと "**有効期間の終了**" フィールドによって定義されています。 認証時に、証明書がまだ有効期間内かどうかを確認するためのチェックが行われます。

### <a name="certificate-revocation-list"></a>証明書失効リスト

有効期間中であっても、証明機関が証明書を失効させる場合があります。 これは、証明書の秘密キーの侵害など、さまざまな理由によって行われます。

証明書が失効した場合、失効した証明書から発生した下位のチェーンも無効になり、認証手順において信頼されなくなります。 失効した証明書を検出するために、各発行者は日時が記された"*証明書失効リスト*" (CRL) を発行します。 このリストは、オンラインの失効またはオフラインの失効を使用してチェックできます。これを使用するには、`RevocationMode`、`DefaultRevocationMode`、<xref:System.Security.Cryptography.X509Certificates.X509RevocationMode>、および <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication> の各クラスの <xref:System.ServiceModel.Security.X509PeerCertificateAuthentication> プロパティまたは <xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication> プロパティを <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> 列挙値のいずれかに設定します。 すべてのプロパティの既定値は、`Online` です。

(`revocationMode`[serviceBehaviors>\< の) ](../../../../docs/framework/configure-apps/file-schema/wcf/authentication-of-clientcertificate-element.md)[authentication>\< と (](../../../../docs/framework/configure-apps/file-schema/wcf/servicebehaviors.md)[endpointBehaviors>\< の) ](../../../../docs/framework/configure-apps/file-schema/wcf/authentication-of-clientcertificate-element.md)[authentication>\<.の両方の ](../../../../docs/framework/configure-apps/file-schema/wcf/endpointbehaviors.md) 属性を使用することにより、構成でモードを設定することもできます。

## <a name="the-setcertificate-method"></a>SetCertificate メソッド

WCF では、認証、暗号化、またはメッセージのデジタル署名を行うために、多くの場合、サービスまたはクライアントが使用する証明書または証明書のセットを指定する必要があります。 これは、X.509 証明書を表すさまざまなクラスの `SetCertificate` メソッドを使用することで、プログラムによって実行できます。 `SetCertificate` メソッドを使用して証明書を指定するクラスは次のとおりです。

|クラス|方法|
|-----------|------------|
|<xref:System.ServiceModel.Security.PeerCredential>|<xref:System.ServiceModel.Security.PeerCredential.SetCertificate%2A>|
|<xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential>|<xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A>|
|<xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential>|<xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential.SetCertificate%2A>|
|<xref:System.ServiceModel.Security.X509CertificateInitiatorServiceCredential>|
|<xref:System.ServiceModel.Security.X509CertificateInitiatorServiceCredential.SetCertificate%2A>|

`SetCertificate` メソッドを使用するには、ストアの場所とストア、証明書のフィールドを指定する "検索" の種類 (`x509FindType` パラメーター)、および該当のフィールドで検索する値を指定します。 たとえば、次のコードでは、<xref:System.ServiceModel.ServiceHost> インスタンスを作成し、`SetCertificate` メソッドを使用して、クライアントに対してサービスを認証する際に使用するサービス証明書を設定しています。

[!code-csharp[c_WorkingWithCertificates#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_workingwithcertificates/cs/source.cs#1)]
[!code-vb[c_WorkingWithCertificates#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_workingwithcertificates/vb/source.vb#1)]

### <a name="multiple-certificates-with-the-same-value"></a>同じ値が含まれた複数の証明書

ストアには、同じサブジェクト名が含まれた複数の証明書が格納されていることがあります。 つまり、`x509FindType` に <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindBySubjectName> または <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindBySubjectDistinguishedName> を指定したときに、複数の証明書に同じ値が含まれていると、必要な証明書を区別することができないため、例外がスローされます。 この状況は、`x509FindType` を <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindByThumbprint> に設定することによってある程度防ぐことができます。 拇印フィールドには一意の値が含まれるため、このフィールドを使用することで、ストア内の特定の証明書を検索できます。 ただし、この方法には欠点があります。証明書が失効したり、更新されたりした場合、拇印も失われてしまうため、`SetCertificate` メソッドは失敗します。 また、証明書が有効でなくなると、認証が失敗します。 このような状況が発生する可能性を軽減する方法として、`x590FindType` パラメーターを <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindByIssuerName> に設定し、発行者の名前を指定します。 特定の発行者が必要ではない場合は、その他の <xref:System.Security.Cryptography.X509Certificates.X509FindType> 列挙値のいずれか (<xref:System.Security.Cryptography.X509Certificates.X509FindType.FindByTimeValid> など) を設定することもできます。

## <a name="certificates-in-configuration"></a>構成における証明書

証明書は、構成を使用して設定することもできます。 サービスを作成する場合、[\<serviceBehaviors>](../../../../docs/framework/configure-apps/file-schema/wcf/servicebehaviors.md) で証明書をはじめとする資格情報を指定します。 クライアントをプログラミングする場合は、[\<endpointBehaviors>](../../../../docs/framework/configure-apps/file-schema/wcf/endpointbehaviors.md) で証明書を指定します。

## <a name="mapping-a-certificate-to-a-user-account"></a>ユーザー アカウントへの証明書のマッピング

IIS と Active Directory には、証明書を Windows ユーザー アカウントにマップできる機能があります。 この機能の詳細については、「[Map certificates to user accounts](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc736706(v=ws.10))」(ユーザー アカウントへの証明書のマッピング) を参照してください。

Active Directory のマッピングを使用する方法の詳細については、「[Mapping Client Certificates with Directory Service Mapping](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc758484(v=ws.10))」(ディレクトリ サービスのマッピングによるクライアント証明書のマッピング) を参照してください。

この機能が有効になっている場合、<xref:System.ServiceModel.Security.X509ClientCertificateAuthentication.MapClientCertificateToWindowsAccount%2A> クラスの <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication> プロパティを `true` に設定できます。 構成では、次のコードに示すように、`mapClientCertificateToWindowsAccount`[authentication>\< 要素の ](../../../../docs/framework/configure-apps/file-schema/wcf/authentication-of-servicecertificate-element.md) 属性を `true` に設定できます。

```xml
<serviceBehaviors>
 <behavior name="MappingBehavior">
  <serviceCredentials>
   <clientCertificate>
    <authentication certificateValidationMode="None" mapClientCertificateToWindowsAccount="true" />
   </clientCertificate>
  </serviceCredentials>
 </behavior>
</serviceBehaviors>
```

Windows ユーザー アカウントを表すトークンに X.509 証明書をマップすると、Windows トークンを使用して保護されたリソースにアクセスできるため、このマッピングが特権の昇格と見なされます。 したがって、マッピングを行う前に、ドメイン ポリシーにそのポリシーに準拠する X.509 証明書が必要となります。 この要件は、*SChannel* セキュリティ パッケージによって適用されます。

.NET Framework 3.5 以降のバージョンを使用する場合、WCF では、証明書が Windows アカウントにマップされる前にドメインポリシーに準拠していることを確認します。

WCF の最初のリリースでは、ドメイン ポリシーを参照せずにマッピングが実行されます。 そのため、マッピングが有効になっており、X.509 証明書がドメイン ポリシーを満たしていない場合は、最初のリリースの下で実行しているときには動作していた古いアプリケーションが動作しなくなる可能性があります。

## <a name="see-also"></a>参照

- <xref:System.ServiceModel.Channels>
- <xref:System.ServiceModel.Security>
- <xref:System.ServiceModel>
- <xref:System.Security.Cryptography.X509Certificates.X509FindType>
- [サービスおよびクライアントのセキュリティ保護](../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)
