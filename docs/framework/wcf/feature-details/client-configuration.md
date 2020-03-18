---
title: クライアント構成
ms.date: 03/30/2017
ms.assetid: 5da5bd3b-65d9-43b7-91b9-cc9e989b1350
ms.openlocfilehash: 6a5cbf5d536af8649b0b8bb93600e94a48c507c1
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74141772"
---
# <a name="client-configuration"></a>クライアント構成
Windows Communication Foundation (WCF) クライアント構成を使用して、クライアントエンドポイントの "ABC" プロパティであるアドレス、バインド、動作、およびコントラクトを指定できます。このプロパティは、クライアントがサービスエンドポイントに接続するために使用します。 [\<client >](../../configure-apps/file-schema/wcf/client.md)要素には、エンドポイント abcs の構成に使用される属性を持つ[\<エンドポイント >](../../configure-apps/file-schema/wcf/endpoint-of-client.md)要素があります。 これらの属性については、「[エンドポイントの構成](#configuring-endpoints)」セクションで説明します。  
  
 [\<エンドポイントの >](../../configure-apps/file-schema/wcf/endpoint-of-client.md)要素には、メタデータのインポートとエクスポートの設定を指定したり、カスタムアドレスヘッダーのコレクションを格納する\<[ヘッダー >](../../configure-apps/file-schema/wcf/headers.md)要素を使用したり、メッセージを交換する他のエンドポイントによるエンドポイントの認証を可能にする[\<id >](../../configure-apps/file-schema/wcf/identity.md)要素も含ま[\<](../../configure-apps/file-schema/wcf/metadata.md)れます。 [\<ヘッダー >](../../configure-apps/file-schema/wcf/headers.md)と[\<id >](../../configure-apps/file-schema/wcf/identity.md)要素は <xref:System.ServiceModel.EndpointAddress> に含まれており、[アドレス](../../wcf/feature-details/endpoint-addresses.md)に関する記事で説明されています。 メタデータ拡張機能の使用方法について説明するトピックへのリンクについては、「[メタデータの構成](#configuring-metadata)」セクションを参照してください。  
  
## <a name="configuring-endpoints"></a>エンドポイントの構成  
 クライアント構成は、クライアントが1つまたは複数のエンドポイントを指定できるように設計されています。各エンドポイントには、それぞれ独自の名前、アドレス、およびコントラクトがあります。各エンドポイントは[\<バインド >](../../../../docs/framework/configure-apps/file-schema/wcf/bindings.md)を参照し、 [\<動作](../../../../docs/framework/configure-apps/file-schema/wcf/behaviors.md)は、エンドポイントを構成するために使用されるクライアント構成の要素 > ます。 クライアント構成ファイルは、WCF ランタイムが想定している名前であるため、"App.config" という名前にする必要があります。 クライアント構成ファイルの例を次に示します。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
        <client>  
          <endpoint  
            name="endpoint1"  
            address="http://localhost/ServiceModelSamples/service.svc"  
            binding="wsHttpBinding"  
            bindingConfiguration="WSHttpBinding_IHello"  
            behaviorConfiguration="IHello_Behavior"  
            contract="IHello" >  
  
            <metadata>  
              <wsdlImporters>  
                <extension  
                  type="Microsoft.ServiceModel.Samples.WsdlDocumentationImporter, WsdlDocumentation"/>  
              </wsdlImporters>  
            </metadata>  
  
            <identity>  
              <servicePrincipalName value="host/localhost" />  
            </identity>  
          </endpoint>  
// Add another endpoint by adding another <endpoint> element.  
          <endpoint  
            name="endpoint2">  
           //Configure another endpoint here.  
          </endpoint>  
        </client>  
  
//The bindings section references by the bindingConfiguration endpoint attribute.  
    <bindings>  
      <wsHttpBinding>  
        <binding name="WSHttpBinding_IHello"   
                 bypassProxyOnLocal="false"   
                 hostNameComparisonMode="StrongWildcard">  
          <readerQuotas maxDepth="32"/>  
          <reliableSession ordered="true"   
                           enabled="false" />  
          <security mode="Message">  
           //Security settings go here.  
          </security>  
        </binding>  
        <binding name="Another Binding"  
        //Configure this binding here.  
        </binding>  
          </wsHttpBinding>  
        </bindings>  
  
//The behavior section references by the behaviorConfiguration endpoint attribute.  
        <behaviors>  
            <endpointBehaviors>  
                <behavior name=" IHello_Behavior ">  
                    <clientVia />  
                </behavior>  
            </endpointBehaviors>  
        </behaviors>  
  
    </system.serviceModel>  
</configuration>  
```  
  
 省略可能な `name` 属性は、特定のコントラクトのエンドポイントを一意に識別します。 この属性は、サービスへのチャネルの作成時に、クライアント構成のどのエンドポイントをターゲットとし、読み込む必要があるかを指定するために、<xref:System.ServiceModel.ChannelFactory%601.%23ctor%2A> または <xref:System.ServiceModel.ClientBase%601.%23ctor%2A> が使用します。 ワイルドカードエンドポイント構成名 "\*" を使用できます。また、使用可能なエンドポイントが1つある場合は、ファイルにエンドポイント構成を読み込む必要があることを <xref:System.ServiceModel.ChannelFactory.ApplyConfiguration%2A> メソッドに示します。それ以外の場合は、例外がスローされます。 この属性が省略されている場合、指定されたコントラクトの種類に関連する既定のエンドポイントとして、対応するエンドポイントが使用されます。 `name` 属性の既定値は、他の名前と同様に一致する空の文字列です。  
  
 すべてのエンドポイントには、エンドポイントを検索および識別するために、アドレスが関連付けられている必要があります。 `address` 属性は、エンドポイントの場所を示す URL を指定するために使用できます。 ただし、サービス エンドポイントのアドレスは、コードで指定することもできます。その場合は、URI (Uniform Resource Identifier) を作成し、<xref:System.ServiceModel.ServiceHost> メソッドのいずれかを使用して、この URI を <xref:System.ServiceModel.ServiceHost.AddServiceEndpoint%2A> に追加します。 詳細については、「[アドレス](../../../../docs/framework/wcf/feature-details/endpoint-addresses.md)」を参照してください。 概要を示すように、 [\<ヘッダー >](../../../../docs/framework/configure-apps/file-schema/wcf/headers.md)と[\<id >](../../../../docs/framework/configure-apps/file-schema/wcf/identity.md)要素は <xref:System.ServiceModel.EndpointAddress> の一部であり、「[アドレス](../../../../docs/framework/wcf/feature-details/endpoint-addresses.md)」のトピックでも説明されています。  
  
 `binding` 属性は、エンドポイントがサービスに接続する際に使用するバインディングの種類を示します。 参照できるようにするには、種類は登録された構成セクションを持っている必要があります。 前の例では、これは[\<wsHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md)セクションです。これは、エンドポイントが <xref:System.ServiceModel.WSHttpBinding>を使用することを示します。 ただし、エンドポイントが使用できる、指定された種類のバインディングが複数存在することもあります。 これらのそれぞれには、(binding) type 要素内の独自の[\<バインド >](../../configure-apps/file-schema/wcf/bindings.md)要素があります。 `bindingconfiguration` 属性は、同じ種類のバインディングを識別するために使用されます。 この値は、 [\<binding >](../../configure-apps/file-schema/wcf/bindings.md)要素の `name` 属性と一致します。 構成を使用してクライアントバインディングを構成する方法の詳細については、「[方法: 構成でクライアントバインディングを指定](../../../../docs/framework/wcf/how-to-specify-a-client-binding-in-configuration.md)する」を参照してください。  
  
 `behaviorConfiguration` 属性は、エンドポイントが使用する[\<endpointBehaviors behaviors](../../../../docs/framework/configure-apps/file-schema/wcf/endpointbehaviors.md)の[\<動作](../../../../docs/framework/configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md)を指定するために使用されます。 値は、 [\<behavior >](../../../../docs/framework/configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md)要素の `name` 属性と一致します。 構成を使用してクライアントの動作を指定する例については、「[クライアントの動作の構成](../../../../docs/framework/wcf/configuring-client-behaviors.md)」を参照してください。  
  
 `contract` 属性は、エンドポイントが公開するコントラクトを指定します。 この値は、<xref:System.ServiceModel.ServiceContractAttribute.ConfigurationName%2A> の <xref:System.ServiceModel.ServiceContractAttribute> にマップされます。 既定値は、サービスを実装するクラスの完全な型名です。  
  
### <a name="configuring-metadata"></a>メタデータの構成  
 メタデータのインポート拡張を登録するために使用する設定を指定するには、 [\<metadata >](../../../../docs/framework/configure-apps/file-schema/wcf/metadata.md)要素を使用します。 メタデータシステムの拡張の詳細については、「[メタデータシステムの拡張](../../../../docs/framework/wcf/extending/extending-the-metadata-system.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [エンドポイント : アドレス、バインディング、およびコントラクト](../../../../docs/framework/wcf/feature-details/endpoints-addresses-bindings-and-contracts.md)
- [クライアントの動作の構成](../../../../docs/framework/wcf/configuring-client-behaviors.md)
