---
title: 構成とメタデータのサポート
ms.date: 03/30/2017
ms.assetid: 27c240cb-8cab-472c-87f8-c864f4978758
ms.openlocfilehash: 3f6d506d719cbb1b2ecc8bae223dfe73e7e2d1a9
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425132"
---
# <a name="configuration-and-metadata-support"></a>構成とメタデータのサポート
ここでは、バインディングおよびバインド要素用に構成とメタデータのサポートを有効にする方法を説明します。  
  
## <a name="overview-of-configuration-and-metadata"></a>構成とメタデータの概要  
 このトピックでは、次のタスクについて説明します。これらのタスクは、[チャネルの開発](developing-channels.md)タスク一覧のオプションの項目1、2、および4です。  
  
- バインド要素の構成ファイル サポートを有効にします。  
  
- バインディングの構成ファイル サポートを有効にします。  
  
- バインド要素の WSDL とポリシー アサーションをエクスポートします。  
  
- バインディングまたはバインド要素に挿入して構成する WSDL とポリシー アサーションを特定します。  
  
 ユーザー定義バインディングとバインド要素の作成の詳細については、それぞれ「[ユーザー定義バインディングの作成](creating-user-defined-bindings.md)」と「 [BindingElement の作成](creating-a-bindingelement.md)」を参照してください。  
  
## <a name="adding-configuration-support"></a>構成サポートの追加  
 チャネルに対する構成ファイルのサポートを有効にするには、2 つの構成セクションを実装する必要があります。1 つはバインド要素に対する構成のサポートを有効にする <xref:System.ServiceModel.Configuration.BindingElementExtensionElement?displayProperty=nameWithType> で、もう 1 つはバインディングに対する構成のサポートを有効にする <xref:System.ServiceModel.Configuration.StandardBindingElement?displayProperty=nameWithType> と <xref:System.ServiceModel.Configuration.StandardBindingCollectionElement%602?displayProperty=nameWithType> です。  
  
 これを行う簡単な方法は、 [Configurationcodegenerator](../samples/configurationcodegenerator.md)サンプルツールを使用して、バインドとバインド要素の構成コードを生成することです。  
  
### <a name="extending-bindingelementextensionelement"></a>BindingElementExtensionElement の拡張  
 次のコード例は、 [Transport: UDP](../samples/transport-udp.md)サンプルから取得したものです。 `UdpTransportElement` は <xref:System.ServiceModel.Configuration.BindingElementExtensionElement> を構成システムに公開する `UdpTransportBindingElement` です。 いくつかの基本的なオーバーライドを行うことで、サンプルでは構成セクション名、バインド要素の種類とバインド要素の作成方法が定義されます。 その後、次のようにして拡張セクションを構成ファイルに登録できます。  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <extensions>  
      <bindingElementExtensions>  
      <add name="udpTransport" type="Microsoft.ServiceModel.Samples.UdpTransportElement, UdpTransport />  
      </bindingElementExtensions>  
    </extensions>  
  </system.serviceModel>  
</configuration>  
```  
  
 この拡張は、UDP をトランスポートとして使用するためにカスタム バインドから参照されます。  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <bindings>  
      <customBinding>  
       <binding configurationName="UdpCustomBinding">  
         <udpTransport/>  
       </binding>  
      </customBinding>  
    </bindings>  
  </system.serviceModel>  
</configuration>  
```  
  
### <a name="adding-configuration-for-a-binding"></a>バインディングの構成の追加  
 セクション `SampleProfileUdpBindingCollectionElement` は <xref:System.ServiceModel.Configuration.StandardBindingCollectionElement%602> で、`SampleProfileUdpBinding` を構成システムに公開します。 実装の大部分は `SampleProfileUdpBindingConfigurationElement` で代行されます。これは <xref:System.ServiceModel.Configuration.StandardBindingElement> の派生です。 `SampleProfileUdpBindingConfigurationElement` には、`SampleProfileUdpBinding`のプロパティに対応するプロパティと、`ConfigurationElement` binding からマップする関数があります。 次のサンプル コードで示すように、最後に、`OnApplyConfiguration` メソッドが `SampleProfileUdpBinding` 内でオーバーライドされます。  
  
```csharp 
protected override void OnApplyConfiguration(string configurationName)  
{  
            if (binding == null)  
                throw new ArgumentNullException("binding");  
  
            if (binding.GetType() != typeof(SampleProfileUdpBinding))  
            {  
                var expectedType = typeof(SampleProfileUdpBinding).AssemblyQualifiedName;
                var typePassedIn = binding.GetType().AssemblyQualifiedName;
                throw new ArgumentException($"Invalid type for binding. Expected type: {expectedType}. Type passed in: {typePassedIn}.");  
            }  
            SampleProfileUdpBinding udpBinding = (SampleProfileUdpBinding)binding;  
  
            udpBinding.OrderedSession = this.OrderedSession;  
            udpBinding.ReliableSessionEnabled = this.ReliableSessionEnabled;  
            udpBinding.SessionInactivityTimeout = this.SessionInactivityTimeout;  
            if (this.ClientBaseAddress != null)  
                   udpBinding.ClientBaseAddress = ClientBaseAddress;  
}  
```  
  
 このハンドラーを構成システムに登録するには、次のセクションを関連する構成ファイルに追加します。  
  
```xml  
<configuration>  
  <configSections>  
     <sectionGroup name="system.serviceModel">  
         <sectionGroup name="bindings">  
                 <section name="sampleProfileUdpBinding" type="Microsoft.ServiceModel.Samples.SampleProfileUdpBindingCollectionElement, UdpTransport />  
         </sectionGroup>  
     </sectionGroup>  
  </configSections>  
</configuration>  
```  
  
 その後、 [\<system.servicemodel >](../../configure-apps/file-schema/wcf/system-servicemodel.md)構成セクションから参照できます。  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <client>  
      <endpoint configurationName="calculator"  
                address="soap.udp://localhost:8001/"   
                bindingConfiguration="CalculatorServer"  
                binding="sampleProfileUdpBinding"   
                contract= "Microsoft.ServiceModel.Samples.ICalculatorContract">  
      </endpoint>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="adding-metadata-support-for-a-binding-element"></a>バインド要素のメタデータ サポートの追加  
 チャネルをメタデータ システムに統合するには、ポリシーのインポートとエクスポートの両方がサポートされている必要があります。 これにより、 [ServiceModel メタデータユーティリティツール (svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)などのツールで、バインド要素のクライアントを生成できます。  
  
### <a name="adding-wsdl-support"></a>WSDL サポートの追加  
 バインディングのトランスポート バインド要素は、メタデータのアドレス指定情報のインポートとエクスポートを行います。 SOAP バインディングを使用する場合は、トランスポート バインド要素によっても、メタデータの正しいトランスポート URI がエクスポートされます。 次のコード例は、 [Transport: UDP](../samples/transport-udp.md)サンプルから取得したものです。  
  
#### <a name="wsdl-export"></a>WSDL エクスポート  
 アドレス指定情報をエクスポートするために、`UdpTransportBindingElement` は <xref:System.ServiceModel.Description.IWsdlExportExtension?displayProperty=nameWithType> インターフェイスを実装します。 <xref:System.ServiceModel.Description.IWsdlExportExtension.ExportEndpoint%2A?displayProperty=nameWithType> メソッドにより、正しいアドレス指定情報が WSDL ポートに追加されます。  
  
```csharp  
if (context.WsdlPort != null)  
{  
    AddAddressToWsdlPort(context.WsdlPort, context.Endpoint.Address, encodingBindingElement.MessageVersion.Addressing);  
}  
```  
  
 エンドポイントが SOAP バインディングを使用する場合、`UdpTransportBindingElement` メソッドの <xref:System.ServiceModel.Description.IWsdlExportExtension.ExportEndpoint%2A> の実装でも、次のようにトランスポート URI がエクスポートされます。  
  
```csharp  
WsdlNS.SoapBinding soapBinding = GetSoapBinding(context, exporter);  
if (soapBinding != null)  
{  
    soapBinding.Transport = UdpPolicyStrings.UdpNamespace;  
}  
```  
  
#### <a name="wsdl-import"></a>WSDL インポート  
 WSDL インポート システムを拡張してアドレスのインポートを処理するには、次の構成を Svcutil.exe の構成ファイルに追加する必要があります。Svcutil.exe.config ファイルの次の例を参照してください。  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <client>  
      <metadata>  
        <wsdlImporters>  
          <extension type=" Microsoft.ServiceModel.Samples.UdpBindingElementImporter, UdpTransport" />  
        </policyImporters>  
      </metadata>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
 Svcutil.exe を実行する場合、Svcutil.exe に WSDL インポートの拡張を読み込ませるために次の 2 つのオプションがあります。  
  
1. /SvcutilConfig:\<ファイル > を使用して、Svcutil.exe に構成ファイルをポイントします。  
  
2. Svcutil.exe と同じディレクトリにある Svcutil.exe.config に構成セクションを追加します。  
  
 `UdpBindingElementImporter` 型は、<xref:System.ServiceModel.Description.IWsdlImportExtension?displayProperty=nameWithType> インターフェイスを実装します。 `ImportEndpoint` メソッドは、次のようにして WSDL ポートからアドレスをインポートします。  
  
```csharp  
BindingElementCollection bindingElements = context.Endpoint.Binding.CreateBindingElements();  
TransportBindingElement transportBindingElement = bindingElements.Find<TransportBindingElement>();  
if (transportBindingElement is UdpTransportBindingElement)  
{  
    ImportAddress(context);  
}  
```  
  
### <a name="adding-policy-support"></a>ポリシー サポートの追加  
 カスタム バインド要素では、WSDL バインディング内のポリシー アサーションをエクスポートして、サービス エンドポイントでそのバインド要素の機能を表現します。 次のコード例は、 [Transport: UDP](../samples/transport-udp.md)サンプルから取得したものです。  
  
#### <a name="policy-export"></a>ポリシーのエクスポート  
 `UdpTransportBindingElement` の種類は <xref:System.ServiceModel.Description.IPolicyExportExtension?displayProperty=nameWithType> を実装して、ポリシーのエクスポートのサポートを追加します。 その結果、<xref:System.ServiceModel.Description.MetadataExporter?displayProperty=nameWithType> には、これを含む任意のバインディングでのポリシーの生成時に `UdpTransportBindingElement` が含まれます。  
  
 <xref:System.ServiceModel.Description.IPolicyExportExtension.ExportPolicy%2A?displayProperty=nameWithType> では、UDP のアサーションを追加します。チャネルがマルチキャスト モードである場合は、別のアサーションを追加します。 これは、マルチキャスト モードは通信スタックの構築方法に影響を与えるため、両方の側において調整される必要があるためです。  
  
```csharp  
ICollection<XmlElement> bindingAssertions = context.GetBindingAssertions();  
XmlDocument xmlDocument = new XmlDocument();  
bindingAssertions.Add(xmlDocument.CreateElement(  
UdpPolicyStrings.Prefix, UdpPolicyStrings.TransportAssertion, UdpPolicyStrings.UdpNamespace));  
if (Multicast)  
{  
    bindingAssertions.Add(xmlDocument.CreateElement(  
UdpPolicyStrings.Prefix, UdpPolicyStrings.MulticastAssertion,     UdpPolicyStrings.UdpNamespace));  
}  
```  
  
 カスタム トランスポート バインド要素はアドレス指定の処理を実行するため、<xref:System.ServiceModel.Description.IPolicyExportExtension?displayProperty=nameWithType> への `UdpTransportBindingElement` の実装でも、WS-Addressing ポリシー アサーションのエクスポートが処理されて、使用されている WS-Addressing のバージョンが示されます。  
  
```csharp  
AddWSAddressingAssertion(context, encodingBindingElement.MessageVersion.Addressing);  
```  
  
#### <a name="policy-import"></a>ポリシーのインポート  
 ポリシー インポート システムを拡張するには、次の構成を Svcutil.exe の構成ファイルに追加する必要があります。Svcutil.exe.config ファイルの次の例を参照してください。  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <client>  
      <metadata>  
        <policyImporters>  
          <extension type=" Microsoft.ServiceModel.Samples.UdpBindingElementImporter, UdpTransport" />  
        </policyImporters>  
      </metadata>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
 次に、登録されたクラス (<xref:System.ServiceModel.Description.IPolicyImportExtension?displayProperty=nameWithType>) から `UdpBindingElementImporter` を実装します。 <xref:System.ServiceModel.Description.IPolicyImportExtension.ImportPolicy%2A?displayProperty=nameWithType> で、適切な名前空間にあるアサーションを調べ、トランスポートを生成するためにそのアサーションを処理して、マルチキャストであるかどうかをチェックします。 さらに、インポーターが処理するアサーションをバインディング アサーションの一覧から削除します。 Svcutil.exe を実行する場合、ここでも、統合用に次の 2 つのオプションがあります。  
  
1. /SvcutilConfig:\<ファイル > を使用して、Svcutil.exe に構成ファイルをポイントします。  
  
2. Svcutil.exe と同じディレクトリにある Svcutil.exe.config に構成セクションを追加します。  
  
### <a name="adding-a-custom-standard-binding-importer"></a>カスタムの標準バインディング インポーターの追加  
 Svcutil.exe と <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> 型は、既定でシステム指定のバインディングを識別してインポートします。 これ以外の場合、バインディングは、<xref:System.ServiceModel.Channels.CustomBinding?displayProperty=nameWithType> インスタンスとしてインポートされます。 Svcutil.exe と <xref:System.ServiceModel.Description.WsdlImporter> を有効にして `SampleProfileUdpBinding` をインポートする場合、`UdpBindingElementImporter` もカスタムの標準バインディング インポーターとして機能します。  
  
 カスタムの標準バインディングインポーターは、<xref:System.ServiceModel.Description.IWsdlImportExtension?displayProperty=nameWithType> インターフェイスの `ImportEndpoint` メソッドを実装して、メタデータからインポートされた <xref:System.ServiceModel.Channels.CustomBinding?displayProperty=nameWithType> インスタンスを調べ、特定の標準バインディングによって生成された可能性があるかどうかを確認します。  
  
```csharp  
if (context.Endpoint.Binding is CustomBinding)  
{  
    Binding binding;  
    if (transportBindingElement is UdpTransportBindingElement)  
    {  
        //if TryCreate is true, the CustomBinding will be replace by a SampleProfileUdpBinding in the  
        //generated config file for better typed generation.  
        if (SampleProfileUdpBinding.TryCreate(bindingElements, out binding))  
        {  
            binding.Name = context.Endpoint.Binding.Name;  
            binding.Namespace = context.Endpoint.Binding.Namespace;  
            context.Endpoint.Binding = binding;  
        }  
    }  
}  
```  
  
 一般に、カスタムの標準バインディング インポーターを実装すると、インポートしたバインド要素のプロパティを調べて、標準バインディングによって設定されたプロパティのみが変更され、その他のすべてのプロパティは既定のままであることが検証されます。 標準バインディング インポーターを実装する場合の基本的な方法は、標準バインディングのインスタンスを作成し、標準バインディングがサポートするバインド要素のプロパティを標準バインディング インスタンスに反映し、標準バインディングのバインド要素とインポートしたバインド要素とを比較します。
