---
title: 匿名クライアントを使用したトランスポートセキュリティ
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 056653a5-384e-4a02-ae3c-1b0157d2ccb4
ms.openlocfilehash: c3e44c87dfa70ac3a7acc5a83ac596efc22b6155
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75344758"
---
# <a name="transport-security-with-an-anonymous-client"></a>匿名クライアントを使用したトランスポートセキュリティ

この Windows Communication Foundation (WCF) シナリオでは、トランスポートセキュリティ (HTTPS) を使用して、機密性と整合性を確保します。 サーバーは SSL (Secure Sockets Layer) 証明書で認証される必要があり、クライアントはサーバーの証明書を信頼する必要があります。 クライアントを認証する機構はないため、匿名となります。

サンプルアプリケーションについては、「 [WS トランスポートセキュリティ](../samples/ws-transport-security.md)」を参照してください。 トランスポートセキュリティの詳細については、「[トランスポートセキュリティの概要](transport-security-overview.md)」を参照してください。

サービスで証明書を使用する方法の詳細については、「[証明書の操作](working-with-certificates.md)」および「[方法: SSL 証明書を使用してポートを構成する](how-to-configure-a-port-with-an-ssl-certificate.md)」を参照してください。

![匿名クライアントを利用したトランスポート セキュリティの使用](./media/8fa2e931-0cfb-4aaa-9272-91d652b85d8d.gif)

|特徴|説明|
|--------------------|-----------------|
|セキュリティ モード|Transport|
|相互運用性|既存の Web サービスとクライアントを使用する|
|認証 (サーバー)<br /><br /> 認証 (クライアント)|○<br /><br /> アプリケーションレベル (WCF サポートなし)|
|整合性|○|
|機密性|○|
|Transport|HTTPS|
|バインディング|<xref:System.ServiceModel.WSHttpBinding>|

## <a name="service"></a>サービス

次のコードと構成は、別々に実行します。 以下のいずれかを実行します。

- 構成を使用せずに、コードを使用してスタンドアロン サービスを作成します。

- 提供された構成を使用してサービスを作成しますが、エンドポイントを定義しません。

### <a name="code"></a>コード

次のコードは、トランスポート セキュリティを使用してエンドポイントを作成する方法を示しています。

[!code-csharp[c_SecurityScenarios#5](~/samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#5)]
[!code-vb[c_SecurityScenarios#5](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#5)]

### <a name="configuration"></a>の構成

次のコードは、構成を使用して同一のエンドポイントをセットアップします。 クライアントを認証する機構はないため、匿名となります。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="ServiceModel.Calculator">
        <endpoint address="https://localhost/Calculator"
                  binding="wsHttpBinding"
                  bindingConfiguration="WSHttpBinding_ICalculator"
                  name="SecuredByTransportEndpoint"
                  contract="ServiceModel.ICalculator" />
      </service>
    </services>
    <bindings>
      <wsHttpBinding>
        <binding name="WSHttpBinding_ICalculator">
          <security mode="Transport">
            <transport clientCredentialType="None" />
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

     [!code-csharp[C_SecurityScenarios#0](~/samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#0)]
     [!code-vb[C_SecurityScenarios#0](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#0)]

### <a name="code"></a>コード

[!code-csharp[c_SecurityScenarios#6](~/samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#6)]
[!code-vb[c_SecurityScenarios#6](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#6)]

### <a name="configuration"></a>の構成

コードの代わりに次の構成を使用して、サービスをセットアップできます。

```xml
<configuration>
  <system.serviceModel>
    <bindings>
      <wsHttpBinding>
        <binding name="WSHttpBinding_ICalculator" >
          <security mode="Transport">
            <transport clientCredentialType="None" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    <client>
      <endpoint address="https://machineName/Calculator"
                binding="wsHttpBinding"
                bindingConfiguration="WSHttpBinding_ICalculator"
                contract="ICalculator"
                name="WSHttpBinding_ICalculator" />
    </client>
  </system.serviceModel>
</configuration>
```

## <a name="see-also"></a>関連項目

- [セキュリティの概要](security-overview.md)
- [WS トランスポート セキュリティ](../samples/ws-transport-security.md)
- [トランスポート セキュリティの概要](transport-security-overview.md)
- [Windows Server App Fabric のセキュリティモデル](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
