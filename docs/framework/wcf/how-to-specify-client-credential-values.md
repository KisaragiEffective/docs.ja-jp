---
title: '方法 : クライアントの資格情報の値を指定する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 82293d7f-471a-4549-8f19-0be890e7b074
ms.openlocfilehash: 2417c2dd16224d6cbf00d3f1f4a8958420830b6c
ms.sourcegitcommit: 628e8147ca10187488e6407dab4c4e6ebe0cac47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72319864"
---
# <a name="how-to-specify-client-credential-values"></a>方法 : クライアントの資格情報の値を指定する

サービスでは、Windows Communication Foundation (WCF) を使用して、サービスに対するクライアントの認証方法を指定できます。 たとえば、証明書を使用してクライアントを認証するように指定できます。

## <a name="to-determine-the-client-credential-type"></a>クライアント資格情報の種類を特定するには

1. サービスのメタデータ エンドポイントからメタデータを取得します。 一般的に、メタデータは、選択したプログラミング言語 (既定は Visual C#) のクライアント コードおよび XML 構成ファイルという 2 つのファイルで構成されています。 メタデータは、クライアント コードおよびクライアント構成を返す Svcutil.exe ツールを使用して取得できます。 詳細については、「[メタデータの取得](./feature-details/retrieving-metadata.md)」および「 [ServiceModel メタデータユーティリティツール (svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)」を参照してください。

2. XML 構成ファイルを開きます。 Svcutil.exe ツールを使用する場合、ファイルの既定の名前は、Output.config です。

3. **Mode**属性 ( **\<security mode =** `MessageOrTransport` **>** を持つ **\<security >** 要素を見つけます。 `MessageOrTransport` はいずれかのセキュリティモードに設定されています。

4. mode 値に一致する子要素を見つけます。 たとえば、モードが**Message**に設定されている場合は、 **\<security >** 要素に含まれている **\<メッセージ >** 要素を検索します。

5. **ClientCredentialType**属性に割り当てられている値を確認します。 実際の値は、使用されているモード (トランスポートまたはメッセージ) に依存します。

次の XML コードは、メッセージ セキュリティを使用し、クライアントの認証に証明書を要求するクライアントの構成を示します。

```xml
<security mode="Message">
    <transport clientCredentialType="Windows" proxyCredentialType="None"
        realm="" />
     <message clientCredentialType="Certificate" negotiateServiceCredential="true"
    algorithmSuite="Default" establishSecurityContext="true" />
</security>
```

## <a name="example-tcp-transport-mode-with-certificate-as-client-credential"></a>例: クライアント資格情報としての証明書による TCP トランスポート モード

この例では、セキュリティ モードを "Transport (トランスポート)" モードに設定し、クライアント資格情報の値を X.509 証明書に設定します。 次の手順では、クライアントでクライアント資格情報の値をコードと構成を使用して設定する方法を示します。 これは、 [ServiceModel メタデータユーティリティツール (svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)を使用して、サービスからメタデータ (コードと構成) を返すことを前提としています。 詳細については、「[方法: クライアントを作成](how-to-create-a-wcf-client.md)する」を参照してください。

### <a name="to-specify-the-client-credential-value-on-the-client-in-code"></a>クライアントでクライアント資格情報の値をコードによって指定するには

1. [ServiceModel メタデータユーティリティツール (svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)を使用して、サービスからコードと構成を生成します。

2. 生成されたコードを使用して、WCF クライアントのインスタンスを作成します。

3. クライアント クラスで、<xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> クラスの <xref:System.ServiceModel.ClientBase%601> プロパティを適切な値に設定します。 この例では、<xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A> クラスの <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential> メソッドを使用して、このプロパティを X.509 証明書に設定します。

     [!code-csharp[c_TcpService#4](../../../samples/snippets/csharp/VS_Snippets_CFX/c_tcpservice/cs/source.cs#4)]
     [!code-vb[c_TcpService#4](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_tcpservice/vb/source.vb#4)]

     <xref:System.Security.Cryptography.X509Certificates.X509FindType> クラスの任意の列挙体を使用できます。 ここでは、有効期限が切れて証明書が変更された場合に備えて、サブジェクト名を使用しています。 サブジェクト名を使用することにより、インフラストラクチャは証明書を再検索できます。

### <a name="to-specify-the-client-credential-value-on-the-client-in-configuration"></a>クライアントでクライアント資格情報の値を構成によって指定するには

1. [\<behavior >](../configure-apps/file-schema/wcf/behaviors.md)要素に[\<動作 >](../configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md)要素を追加します。

2. [\<behavior >](../configure-apps/file-schema/wcf/behaviors.md)要素に[\<clientCredentials >](../configure-apps/file-schema/wcf/clientcredentials.md)要素を追加します。 `name` 属性 (必須) を適切な値に必ず設定してください。

3. [\<clientCertificate >](../configure-apps/file-schema/wcf/clientcertificate-of-servicecredentials.md)要素を[\<clientCredentials >](../configure-apps/file-schema/wcf/clientcredentials.md)要素に追加します。

4. 次のコードに示すように、`storeLocation`、`storeName`、`x509FindType`、および `findValue` の各属性を適切な値に設定します。 証明書の詳細については、「[証明書の使用](./feature-details/working-with-certificates.md)」を参照してください。

    ```xml
    <behaviors>
       <endpointBehaviors>
          <behavior name="endpointCredentialBehavior">
            <clientCredentials>
              <clientCertificate findValue="Contoso.com"
                                 storeLocation="LocalMachine"
                                 storeName="TrustedPeople"
                                 x509FindType="FindBySubjectName" />
            </clientCredentials>
          </behavior>
       </endpointBehaviors>
    </behaviors>
    ```

5. クライアントを構成するときは、次のコードに示すように、`behaviorConfiguration` 要素の `<endpoint>` 属性を設定して動作を指定します。 エンドポイント要素は[\<client >](../configure-apps/file-schema/wcf/client.md)要素の子です。 また、`bindingConfiguration` 属性をクライアントのバインディングに設定することにより、バインド構成の名前を指定します。 生成された構成ファイルを使用している場合は、バインディングの名前は自動的に生成されます。 この例では、名前は `"tcpBindingWithCredential"` です。

    ```xml
    <client>
      <endpoint name =""
                address="net.tcp://contoso.com:8036/aloha"
                binding="netTcpBinding"
                bindingConfiguration="tcpBindingWithCredential"
                behaviorConfiguration="endpointCredentialBehavior" />
    </client>
    ```

## <a name="see-also"></a>参照

- <xref:System.ServiceModel.NetTcpBinding>
- <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential.SetCertificate%2A>
- <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential>
- <xref:System.ServiceModel.ClientBase%601>
- <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential>
- [WCF セキュリティのプログラミング](./feature-details/programming-wcf-security.md)
- [資格情報の種類の選択](./feature-details/selecting-a-credential-type.md)
- [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)
- [証明書の使用](./feature-details/working-with-certificates.md)
- [方法: クライアントを作成する](how-to-create-a-wcf-client.md)
- [\<netTcpBinding>](../configure-apps/file-schema/wcf/nettcpbinding.md)
- [\<security>](../configure-apps/file-schema/wcf/security-of-nettcpbinding.md)
- [\<message>](../configure-apps/file-schema/wcf/message-element-of-nettcpbinding.md)
- [\<behavior>](../configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md)
- [\<behaviors>](../configure-apps/file-schema/wcf/behaviors.md)
- [\<clientCertificate>](../configure-apps/file-schema/wcf/clientcertificate-of-servicecredentials.md)
- [\<clientCredentials>](../configure-apps/file-schema/wcf/clientcredentials.md)
