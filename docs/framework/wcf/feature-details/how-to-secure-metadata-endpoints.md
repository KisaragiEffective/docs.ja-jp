---
description: '詳細情報: 方法: セキュリティで保護されたメタデータ エンドポイント'
title: '方法: セキュリティで保護されたメタデータ エンドポイント'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 9f71b6ae-737c-4382-8d89-0a7b1c7e182b
ms.openlocfilehash: bcce3fd0708049435c791cae5064f84133dfd612
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643307"
---
# <a name="how-to-secure-metadata-endpoints"></a>方法: セキュリティで保護されたメタデータ エンドポイント

サービスのメタデータには、悪意のあるユーザーに利用される可能性がある、アプリケーションに関する機密情報が含まれています。 また、サービスのコンシューマーにも、サービスのメタデータを取得するためのセキュリティで保護された機構が必要です。 したがって、状況に応じて、セキュリティで保護されたエンドポイントを使用してメタデータを公開する必要があります。

メタデータ エンドポイントは、一般に、アプリケーション エンドポイントをセキュリティで保護するために Windows Communication Foundation (WCF) に定義されている標準のセキュリティ機構を使用して保護されます。 詳細については、[セキュリティの概要](security-overview.md)に関するページを参照してください。

ここでは、SSL (Secure Sockets Layer) 証明書によって保護されたエンドポイント (つまり、HTTPS エンドポイント) を作成する手順を示します。

### <a name="to-create-a-secure-https-get-metadata-endpoint-in-code"></a>セキュリティで保護された HTTPS GET メタデータ エンドポイントをコードで作成するには

1. 適切な X.509 証明書を使用してポートを構成します。 この証明書は信頼された証明機関から発行され、かつ "サービス承認" の用途に使用される必要があります。 証明書をポートに関連付けるには、HttpCfg.exe ツールを使用する必要があります。 「[方法: SSL 証明書を使用してポートを構成する](how-to-configure-a-port-with-an-ssl-certificate.md)」を参照してください。

    > [!IMPORTANT]
    > 証明書またはそのドメイン ネーム システム (DNS: Domain Name System) のサブジェクトが、コンピューターの名前と一致している必要があります。 これは、HTTPS 機構が最初に実行する手順に、呼び出されたアドレスと同じ URI (Uniform Resource Identifier) に対して証明書が発行されているかどうかのチェックが含まれるために必要です。

2. <xref:System.ServiceModel.Description.ServiceMetadataBehavior> クラスの新しいインスタンスを作成します。

3. <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetEnabled%2A> クラスの <xref:System.ServiceModel.Description.ServiceMetadataBehavior> プロパティを `true` に設定します。

4. <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetUrl%2A> プロパティを適切な URL に設定します。 絶対アドレスを指定する場合、URL は `https://` で開始する必要があります。 相対アドレスを指定する場合は、サービス ホストに HTTPS ベースのアドレスを指定する必要があります。 このプロパティが設定されていない場合、既定のアドレスは ""、またはサービスに HTTPS ベースのアドレスを直接指定したものになります。

5. 作成したインスタンスを、<xref:System.ServiceModel.Description.ServiceDescription.Behaviors%2A> クラスの <xref:System.ServiceModel.Description.ServiceDescription> プロパティから返される動作コレクションに追加します。コードは次のようになります。

    [!code-csharp[c_HowToSecureEndpoint#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howtosecureendpoint/cs/source.cs#1)]
    [!code-vb[c_HowToSecureEndpoint#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howtosecureendpoint/vb/source.vb#1)]

### <a name="to-create-a-secure-https-get-metadata-endpoint-in-configuration"></a>セキュリティで保護された HTTPS GET メタデータ エンドポイントを構成で作成するには

1. サービスの構成ファイルで、[\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md) 要素を [\<system.serviceModel>](../../configure-apps/file-schema/wcf/system-servicemodel.md) 要素に追加します。

2. [\<serviceBehaviors>](../../configure-apps/file-schema/wcf/servicebehaviors.md) 要素を [\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md) 要素に追加します。

3. [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-servicebehaviors.md) 要素を `<serviceBehaviors>` 要素に追加します。

4. `name` 要素の `<behavior>` 属性を適切な値に設定します。 `name` 属性は必須です。 下の例では、`mySvcBehavior` を値として使用しています。

5. [\<serviceMetadata>](../../configure-apps/file-schema/wcf/servicemetadata.md) を `<behavior>` 要素に追加します。

6. `httpsGetEnabled` 要素の `<serviceMetadata>` 属性を `true` に設定します。

7. `httpsGetUrl` 要素の `<serviceMetadata>` 属性を適切な値に設定します。 絶対アドレスを指定する場合、URL は `https://` で開始する必要があります。 相対アドレスを指定する場合は、サービス ホストに HTTPS ベースのアドレスを指定する必要があります。 このプロパティが設定されていない場合、既定のアドレスは ""、またはサービスに HTTPS ベースのアドレスを直接指定したものになります。

8. サービスで動作を使用するには、[\<service>](../../configure-apps/file-schema/wcf/service.md) 要素の `behaviorConfiguration` 属性を動作要素の名前属性の値に設定します。 完全な構成コードを次の例に示します。

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
     <system.serviceModel>
      <behaviors>
       <serviceBehaviors>
        <behavior name="mySvcBehavior">
         <serviceMetadata httpsGetEnabled="true"
              httpsGetUrl="https://localhost:8036/calcMetadata" />
        </behavior>
       </serviceBehaviors>
      </behaviors>
     <services>
      <service behaviorConfiguration="mySvcBehavior"
            name="Microsoft.Security.Samples.Calculator">
       <endpoint address="http://localhost:8037/ServiceModelSamples/calculator"
       binding="wsHttpBinding" bindingConfiguration=""
       contract="Microsoft.Security.Samples.ICalculator" />
      </service>
     </services>
    </system.serviceModel>
    </configuration>
    ```

## <a name="example"></a>例

次の例では、<xref:System.ServiceModel.ServiceHost> クラスのインスタンスを作成して、エンドポイントを追加します。 次に、<xref:System.ServiceModel.Description.ServiceMetadataBehavior> クラスのインスタンスを作成し、プロパティを設定してセキュリティで保護された Metadata Exchange ポイントを作成しています。

[!code-csharp[c_HowToSecureEndpoint#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howtosecureendpoint/cs/source.cs#0)]
[!code-vb[c_HowToSecureEndpoint#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howtosecureendpoint/vb/source.vb#0)]

## <a name="compiling-the-code"></a>コードのコンパイル

このコード例では、次の名前空間を使用します。

- <xref:System.ServiceModel?displayProperty=nameWithType>

- <xref:System.ServiceModel.Description?displayProperty=nameWithType>

## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetEnabled%2A>
- <xref:System.ServiceModel.Description.ServiceMetadataBehavior>
- <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetUrl%2A>
- [方法: SSL 証明書を使用してポートを構成する](how-to-configure-a-port-with-an-ssl-certificate.md)
- [証明書の使用](working-with-certificates.md)
- [メタデータを使用する場合のセキュリティ上の考慮事項](security-considerations-with-metadata.md)
- [サービスおよびクライアントのセキュリティ保護](securing-services-and-clients.md)
