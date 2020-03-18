---
title: 資格情報ネゴシエーションを使用しない Windows クライアントを使用するメッセージ セキュリティ
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: fc07a26c-cbee-41c5-8fb0-329085fef749
ms.openlocfilehash: d3b05a1786131a119d516edeba0d6e8e24289f87
ms.sourcegitcommit: 09b4090b78f52fd09b0e430cd4b26576f1fdf96e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/17/2020
ms.locfileid: "76212025"
---
# <a name="message-security-with-a-windows-client-without-credential-negotiation"></a>資格情報ネゴシエーションを使用しない Windows クライアントを使用するメッセージ セキュリティ

次のシナリオは、Kerberos プロトコルによって保護された Windows Communication Foundation (WCF) クライアントとサービスを示しています。

サービスとクライアントは、いずれも同じドメインまたは信頼できるドメインに配置されています。

> [!NOTE]
> このシナリオと[Windows クライアントを使用したメッセージセキュリティ](../../../../docs/framework/wcf/feature-details/message-security-with-a-windows-client.md)の違いは、このシナリオでは、アプリケーションメッセージを送信する前にサービスとのサービス資格情報がネゴシエートされないことです。 また、このシナリオでは Kerberos プロトコルを使用するので、Windows ドメイン環境が必要になります。

![資格情報ネゴシエーションを使用しないメッセージセキュリティ](../../../../docs/framework/wcf/feature-details/media/0c9f9baa-2439-4ef9-92f4-43c242d85d0d.gif "0c9f9baa-2439-4ef9-92f4-43c242d85d0d")

|特徴|説明|
|--------------------|-----------------|
|セキュリティ モード|[メッセージ]|
|相互運用性|○ Kerberos トークン プロファイル互換クライアントを使用する WS-Security|
|認証 (サーバー)|サーバーとクライアントの相互認証|
|認証 (クライアント)|サーバーとクライアントの相互認証|
|整合性|○|
|機密性|○|
|Transport|HTTP|
|バインディング|<xref:System.ServiceModel.WSHttpBinding>|

## <a name="service"></a>サービス

次のコードと構成は、別々に実行します。 以下のいずれかを実行します。

- 構成を使用せずに、コードを使用してスタンドアロン サービスを作成します。

- 提供された構成を使用してサービスを作成しますが、エンドポイントを定義しません。

### <a name="code"></a>コード

次のコードは、メッセージ セキュリティを使用するサービス エンドポイントを作成します。 このコードは、サービス資格情報のネゴシエーションとセキュリティ コンテキスト トークン (SCT) の確立を無効にします。

> [!NOTE]
> ネゴシエートせずに Windows の資格情報を使用するには、サービスのユーザー アカウントが、Active Directory ドメインを使用して登録されたサービス プリンシパル名 (SPN) にアクセスする必要があります。 この操作を行う方法は 2 とおりあります。

1. `NetworkService` アカウントまたは `LocalSystem` アカウントを使用してサービスを実行します。 これらのアカウントは、コンピューターが Active Directory ドメインに参加したときに確立されたコンピューターの SPN にアクセスできるため、WCF はサービスのメタデータ内のサービスのエンドポイント内に適切な SPN 要素を自動的に生成します (Web サービスの説明言語 (WSDL))。

2. 任意の Active Directory ドメイン アカウントを使用してサービスを実行します。 この場合、そのドメイン アカウント用の SPN を確立する必要があります。 これを行うには、Setspn.exe ユーティリティ ツールを使用する方法があります。 サービスのアカウントの SPN を作成したら、その SPN をメタデータ (WSDL) を通じてサービスのクライアントに発行するように WCF を構成します。 これを行うには、アプリケーション構成ファイルまたはコードのどちらかを使用して、公開されるエンドポイントのエンドポイント ID を設定します。 プログラムで ID を公開する方法を次の例に示します。

Spn、Kerberos プロトコル、および Active Directory の詳細については、「 [Windows 用の Kerberos テクニカル補完](https://docs.microsoft.com/previous-versions/msp-n-p/ff649429(v=pandp.10))」を参照してください。 エンドポイント id の詳細については、「[認証モードの認証](../../../../docs/framework/wcf/feature-details/securitybindingelement-authentication-modes.md)」を参照してください。

[!code-csharp[C_SecurityScenarios#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#12)]
[!code-vb[C_SecurityScenarios#12](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#12)]

### <a name="configuration"></a>の構成

コードの代わりに次の構成を使用できます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.serviceModel>
    <behaviors />
    <services>
      <service behaviorConfiguration="" name="ServiceModel.Calculator">
        <endpoint address="http://localhost/Calculator"
                  binding="wsHttpBinding"
                  bindingConfiguration="KerberosBinding"
                  name="WSHttpBinding_ICalculator"
                  contract="ServiceModel.ICalculator"
                  listenUri="net.tcp://localhost/metadata" >
         <identity>
            <servicePrincipalName value="service_spn_name" />
         </identity>
        </endpoint>
      </service>
    </services>
    <bindings>
      <wsHttpBinding>
        <binding name="KerberosBinding">
          <security>
            <message negotiateServiceCredential="false"
                     establishSecurityContext="false" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    <client />
  </system.serviceModel>
</configuration>
```

## <a name="client"></a>クライアント

次のコードと構成は、別々に実行します。 以下のいずれかを実行します。

- コード (およびクライアント コード) を使用してスタンドアロン クライアントを作成します。

- エンドポイント アドレスを定義しないクライアントを作成します。 代わりに、引数として構成名を受け取るクライアント コンストラクターを使用します。 例:

  [!code-csharp[C_SecurityScenarios#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#0)]
  [!code-vb[C_SecurityScenarios#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#0)]

### <a name="code"></a>コード

クライアントを構成する場合のコード例を次に示します。 セキュリティ モードは Message に設定され、クライアント資格情報の種類は Windows に設定されています。 <xref:System.ServiceModel.MessageSecurityOverHttp.NegotiateServiceCredential%2A> プロパティと <xref:System.ServiceModel.NonDualMessageSecurityOverHttp.EstablishSecurityContext%2A> プロパティには、`false` が設定されていることに注意してください。

> [!NOTE]
> ネゴシエートせずに Windows の資格情報を使用するには、サービスとの通信を開始する前に、サービスのアカウントの SPN を使用してクライアントを構成する必要があります。 クライアントは SPN を使用して Kerberos トークンを取得し、サービスとの通信を認証し保護します。 サービスの SPN を使用してクライアントを構成する方法を次の例に示します。 [ServiceModel メタデータユーティリティツール (svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)を使用してクライアントを生成する場合、サービスのメタデータにその情報が含まれていれば、サービスの SPN は、サービスのメタデータ (WSDL) からクライアントに自動的に伝達されます。 サービスのメタデータに SPN を含めるようにサービスを構成する方法の詳細については、このトピックで後述する「サービス」セクションを参照してください。
>
> Spn、Kerberos、および Active Directory の詳細については、「 [Windows 用の Kerberos テクニカル補完](https://docs.microsoft.com/previous-versions/msp-n-p/ff649429(v=pandp.10))」を参照してください。 エンドポイント id の詳細については、「[認証モードの認証](../../../../docs/framework/wcf/feature-details/securitybindingelement-authentication-modes.md)」を参照してください。

[!code-csharp[C_SecurityScenarios#19](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#19)]
[!code-vb[C_SecurityScenarios#19](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#19)]

### <a name="configuration"></a>の構成

クライアントを構成する場合のコード例を次に示します。 [\<servicePrincipalName >](../../../../docs/framework/configure-apps/file-schema/wcf/serviceprincipalname.md)要素は、Active Directory ドメイン内のサービスのアカウントに登録されているサービスの SPN と一致するように設定する必要があります。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.serviceModel>
    <bindings>
      <wsHttpBinding>
        <binding name="WSHttpBinding_ICalculator" >
          <security mode="Message">
            <message clientCredentialType="Windows"
                     negotiateServiceCredential="false"
                     establishSecurityContext="false" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    <client>
      <endpoint address="http://localhost/Calculator"
                binding="wsHttpBinding"
                bindingConfiguration="WSHttpBinding_ICalculator"
                contract="ICalculator"
                name="WSHttpBinding_ICalculator">
        <identity>
          <servicePrincipalName value="service_spn_name" />
        </identity>
      </endpoint>
    </client>
  </system.serviceModel>
</configuration>
```

## <a name="see-also"></a>関連項目

- [セキュリティの概要](../../../../docs/framework/wcf/feature-details/security-overview.md)
- [サービス ID と認証](../../../../docs/framework/wcf/feature-details/service-identity-and-authentication.md)
- [Windows Server App Fabric のセキュリティモデル](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
