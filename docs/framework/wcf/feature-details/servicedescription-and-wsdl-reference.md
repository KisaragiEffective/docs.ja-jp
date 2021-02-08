---
description: 詳細については、「ServiceDescription と WSDL リファレンス」を参照してください。
title: ServiceDescription と WSDL 参照
ms.date: 03/30/2017
ms.assetid: eedc025d-abd9-46b1-bf3b-61d2d5c95fd6
ms.openlocfilehash: 0ce3dc49d71eda1f9fe36940f153735b43be917b
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99793520"
---
# <a name="servicedescription-and-wsdl-reference"></a>ServiceDescription と WSDL 参照

このトピックでは、Windows Communication Foundation (WCF) が、Web サービス記述言語 (WSDL) ドキュメントをインスタンスとの間でどのようにマップするかについて説明し <xref:System.ServiceModel.Description.ServiceDescription> ます。  
  
## <a name="how-servicedescription-maps-to-wsdl-11"></a>ServiceDescription から WSDL 1.1 へのマッピング  

 WCF を使用して、サービスのインスタンスから WSDL ドキュメントをエクスポートでき <xref:System.ServiceModel.Description.ServiceDescription> ます。 WSDL ドキュメントは、メタデータ エンドポイントを公開したときに自動的にサービスに対して生成されます。  
  
 また、<xref:System.ServiceModel.Description.ServiceEndpoint> 型を使用して、WSDL ドキュメントから <xref:System.ServiceModel.Description.ContractDescription> インスタンス、<xref:System.ServiceModel.Channels.Binding> インスタンス、および `WsdlImporter` インスタンスをインポートできます。  
  
 WCF によってエクスポートされた WSDL ドキュメントは、外部 XML スキーマドキュメントから使用される XML スキーマ定義をインポートします。 データ型がサービスで使用するターゲット名前空間ごとに、個別の XML スキーマ ドキュメントがエクスポートされます。 同様に、サービス コントラクトが使用するターゲット名前空間ごとに、個別の WSDL ドキュメントがエクスポートされます。  
  
### <a name="servicedescription"></a>ServiceDescription  

 <xref:System.ServiceModel.Description.ServiceDescription> インスタンスは `wsdl:service` 要素にマップされます。 <xref:System.ServiceModel.Description.ServiceDescription> インスタンスは、それぞれが個別の `wsdl:port` 要素にマップされる <xref:System.ServiceModel.Description.ServiceEndpoint> インスタンスのコレクションを格納します。  
  
|プロパティ|WSDL マッピング|  
|----------------|------------------|  
|`Name`|`wsdl:service` /@name サービスの値。|  
|`Namespace`|サービスの `wsdl:service` 定義の targetNamespace|  
|`Endpoints`|サービスの `wsdl:port` 定義|  
  
### <a name="serviceendpoint"></a>ServiceEndpoint  

 <xref:System.ServiceModel.Description.ServiceEndpoint> インスタンスは `wsdl:port` 要素にマップされます。 <xref:System.ServiceModel.Description.ServiceEndpoint> インスタンスは、アドレス、バインディング、およびコントラクトを格納します。  
  
 <xref:System.ServiceModel.Description.IWsdlExportExtension> インターフェイスを実装するエンドポイント動作は、その動作が関連付けられているエンドポイントの `wsdl:port` 要素を変更できます。  
  
|プロパティ|WSDL マッピング|  
|----------------|------------------|  
|`Name`|エンドポイント `wsdl:port` /@name の値と `wsdl:binding` /@name エンドポイントバインドの値。|  
|`Address`|エンドポイントの `wsdl:port` 定義のアドレス<br /><br /> アドレスの形式は、エンドポイントのトランスポートによって決まります。 たとえば、WCF でサポートされているトランスポートの場合、SOAP アドレスまたはエンドポイント参照を使用できます。|  
|`Binding`|エンドポイントの `wsdl:binding` 定義<br /><br /> 定義とは異なり `wsdl:binding` 、WCF のバインディングは、どのコントラクトにも関連付けられていません。|  
|`Contract`|エンドポイントの `wsdl:portType` 定義|  
|`Behaviors`|<xref:System.ServiceModel.Description.IWsdlExportExtension> インターフェイスを実装するエンドポイント動作は、エンドポイントの `wsdl:port` を変更できます。|  
  
### <a name="bindings"></a>バインド  

 `ServiceEndpoint` インスタンスのバインディング インスタンスは、`wsdl:binding` 定義にマップされます。 `wsdl:binding`特定の定義に関連付けられている必要がある定義とは異なり `wsdl:portType` 、WCF バインドはどのコントラクトにも依存しません。  
  
 バインディングは、バインド要素のコレクションで構成されます。 各要素は、エンドポイントがクライアントと通信する方法の一部分を記述します。 また、バインディングには、エンドポイントの <xref:System.ServiceModel.Channels.MessageVersion> と <xref:System.ServiceModel.EnvelopeVersion> を示すための <xref:System.ServiceModel.Channels.AddressingVersion> があります。  
  
|プロパティ|WSDL マッピング|  
|----------------|------------------|  
|`Name`|エンドポイントの既定の名前で使用されます。この名前は、バインディング名にコントラクト名を追加し、アンダースコアで区切った形で表記されます。|  
|`Namespace`|`targetNamespace` 定義の `wsdl:binding`。<br /><br /> インポートでは、WSDL ポートに関連付けられたポリシーがある場合、インポートされたバインディング名前空間は、`targetNamespace` 定義の `wsdl:port` にマップされます。|  
|`BindingElementCollection` (`CreateBindingElements`() メソッドによって返されるプロパティとして)|`wsdl:binding` 定義に対するさまざまなドメイン固有の拡張。通常は、ポリシー アサーション。|  
|`MessageVersion`|エンドポイントの `EnvelopeVersion` および `AddressingVersion`。<br /><br /> `MessageVersion.None` が指定されている場合、WSDL バインディングは SOAP バインディングを格納せず、WSDL ポートは WS-Addressing コンテンツを含みません。 この設定は、通常、Plain Old XML (POX) エンドポイントに対して使用されます。|  
  
#### <a name="bindingelements"></a>BindingElements  

 エンドポイント バインディングのバインド要素は、ポリシー アサーションなど、`wsdl:binding` のさまざまな WSDL 拡張にマップされます。  
  
 SOAP バインディングのトランスポート URI (Uniform Resource Identifier) は、バインディングの <xref:System.ServiceModel.Channels.TransportBindingElement> によって決まります。  
  
#### <a name="addressingversion"></a>AddressingVersion  

 バインディングの `AddressingVersion` は、`wsd:port` で使用されるアドレス指定のバージョンにマップされます。 WCF では、SOAP 1.1 および SOAP 1.2 アドレス、WS-Addressing 08/2004 および WS-Addressing 1.0 エンドポイント参照がサポートされています。  
  
#### <a name="envelopeversion"></a>EnvelopeVersion  

 バインディングの `EnvelopeVersion` は、`wsdl:binding` で使用される SOAP のバージョンにマップされます。 WCF は SOAP 1.1 および SOAP 1.2 のバインドをサポートしています。  
  
### <a name="contracts"></a>コントラクト  

 <xref:System.ServiceModel.Description.ContractDescription> インスタンスの `ServiceEndpoint` インスタンスは、`wsdl:portType` にマップされます。 `ContractDescription` インスタンスは、特定のコントラクトのすべての操作を記述します。  
  
|プロパティ|WSDL マッピング|  
|----------------|------------------|  
|`Name`|`wsdl:portType` /@name コントラクトの値。|  
|`Namespace`|`wsdl:portType` 定義の targetNamespace|  
|`SessionMode`|`wsdl:portType` /@msc:usingSession コントラクトの値。 この属性は、WSDL 1.1 の WCF 拡張機能です。|  
|`Operations`|コントラクトの `wsdl:operation` 定義|  
  
### <a name="operations"></a>操作  

 <xref:System.ServiceModel.Description.OperationDescription>インスタンスは、にマップさ `wsdl:portType` / `wsdl:operation` れます。 `OperationDescription` は、操作のメッセージを記述する `MessageDescription` インスタンスのコレクションを格納します。  
  
 主に、2 つの操作動作 `OperationDescription` および `DataContractSerializerOperationBehavior` が、`XmlSerializerOperationBehavior` を WSDL ドキュメントにマップする方法に関与しています。  
  
|プロパティ|WSDL マッピング|  
|----------------|------------------|  
|`Name`|`wsdl:portType` / `wsdl:operation` /@name 操作の値。|  
|`ProtectionLevel`|この操作の `wsdl:binding/wsdl:operation` メッセージに関連付けられたセキュリティ ポリシーの保護アサーション|  
|`IsInitiating`|`wsdl:portType` / `wsdl:operation` /@msc:isInitiating 操作の値。 この属性は、WSDL 1.1 の WCF 拡張機能です。|  
|`IsTerminating`|`wsdl:portType` / `wsdl:operation` /@msc:isTerminating 操作の値。 この属性は、WSDL 1.1 の WCF 拡張機能です。|  
|`Messages`|`wsdl:portType` / `wsdl:operation` / `wsdl:input` `wsdl:portType` / `wsdl:operation` / `wsdl:output` 操作のメッセージとメッセージ。|  
|`Faults`|`wsdl:portType` / `wsdl:operation` / `wsdl:fault` 操作の定義。|  
|`Behaviors`|`DataContractSerializerOperationBehavior` および `XmlSerializerOperationBehavior` は、操作バインディングおよび操作メッセージを扱います。|  
  
#### <a name="the-datacontractserializeroperationbehavior"></a>DataContractSerializerOperationBehavior  

 操作の `DataContractSerializerOperationBehavior` は、その操作の WSDL メッセージとバインディングをエクスポートする `IWsdlExportExtension` の実装です。 XML スキーマ型は、`XsdDataContractExporter` を使用してエクスポートされます。 また、`DataContractSerializerOperationBehavior` では、その操作に使用する使用方法、スタイル、およびスキーマ エクスポーターとインポーターを決定します。  
  
|プロパティ|WSDL マッピング|  
|----------------|------------------|  
|`DataContractFormatAttribute`|`Style`この属性のプロパティは、 `wsdl:binding` / `wsdl:operation` / `soap:operation` /@style 操作の値にマップされます。<br /><br /> `DataContractSerializerOperationBehavior` は、WSDL のスキーマ型のリテラル使用だけをサポートします。|  
  
#### <a name="the-xmlserializeroperationbehavior"></a>XmlSerializerOperationBehavior  

 操作の `XmlSerializerOperationBehavior` は、その操作の WSDL メッセージとバインディングをエクスポートする `IWsdlExportExtension` の実装です。 XML スキーマ型は、`XmlSchemaExporter` を使用してエクスポートされます。 また、`XmlSerializerOperationBehavior` では、その操作に使用する使用方法、スタイル、およびスキーマ エクスポーターとインポーターを決定します。  
  
|プロパティ|WSDL マッピング|  
|----------------|------------------|  
|`XmlSerializerFormatAttribute`|`Style`この属性のプロパティは、 `wsdl:binding` / `wsdl:operation` / `soap:operation` /@style 操作の値にマップされます。<br /><br /> `Use`この属性のプロパティは、 `wsdl:binding` / `wsdl:operation` / `soap:operation` /@use 操作内のすべてのメッセージの/* 値にマップされます。|  
  
### <a name="messages"></a>メッセージ  

 インスタンスは、 `MessageDescription` `wsdl:message` `wsdl:portType` / `wsdl:operation` / `wsdl:input` `wsdl:portType` / `wsdl:operation` / `wsdl:output` 操作内のまたはメッセージによって参照されるにマップされます。 `MessageDescription` には、本文とヘッダーがあります。  
  
|プロパティ|WSDL マッピング|  
|----------------|------------------|  
|`Action`|メッセージの SOAP アクションまたは WS-Addressing アクション。<br /><br /> Action 文字列 "*" を使用する操作は、WSDL では表示されません。|  
|`Direction`|`MessageDirection.Input` は `wsdl:input` にマップされます。<br /><br /> `MessageDirection.Output` は `wsdl:output` にマップされます。|  
|`ProtectionLevel`|このメッセージの `wsdl:message` 定義に関連付けられたセキュリティ ポリシーの保護アサーション|  
|`Body`|メッセージのメッセージ本文|  
|`Headers`|メッセージのヘッダー|  
|`ContractDescription.Name`, `OperationContract.Name`|エクスポート時に、値を派生させるために使用し `wsdl:message` /@name ます。|  
  
#### <a name="message-body"></a>メッセージ本文  

 インスタンスは、 `MessageBodyDescription` `wsdl:message` / `wsdl:part` メッセージの本文の定義にマップされます。 メッセージ本文は、ラップされている場合とベアの場合があります。  
  
|プロパティ|WSDL マッピング|  
|----------------|------------------|  
|`WrapperName`|スタイルが RPC でない場合、は `WrapperName` `wsdl:message` / `wsdl:part` @name "parameters" に設定されたを使用して参照される要素名にマップされます。|  
|`WrapperNamespace`|スタイルが RPC でない場合、は `WrapperNamespace` `wsdl:message` / `wsdl:part` @name "parameters" に設定されたの要素の名前空間にマップされます。|  
|`Parts`|このメッセージ本文のメッセージ部分|  
|`ReturnValue`|ラッパー要素が存在する場合は、ラッパー要素の子要素 (ドキュメントのラップされたスタイル、または RPC スタイル)。それ以外の場合は、メッセージの最初の `wsdl:message` / `wsdl:part` 。|  
  
#### <a name="message-parts"></a>メッセージ部分  

 `MessagePartDescription`インスタンスは、 `wsdl:message` / `wsdl:part` およびメッセージ部分が指す XML スキーマ型または要素にマップされます。  
  
|プロパティ|WSDL マッピング|  
|----------------|------------------|  
|`Name`|メッセージ `wsd:message` / `wsdl:part` /@name 部分の値と、メッセージ部分が指す要素の名前。|  
|`Namespace`|メッセージ部分が指す要素の名前空間|  
|`Index`|メッセージののインデックス `wsdl:message` / `wsdl:part` 。|  
|`ProtectionLevel`|このメッセージ部分の `wsdl:message` 定義に関連付けられたセキュリティ ポリシーの保護アサーション。 ポリシーは、特定のメッセージ部分を指すためにパラメーター化されます。|  
|`MessageType`|メッセージ部分が指す要素の XML スキーマ型|  
  
#### <a name="message-headers"></a>メッセージ ヘッダー  

 `MessageHeaderDescription` インスタンスは、メッセージ部分の `soap:header` バインディングにもマップされるメッセージ部分です。  
  
### <a name="faults"></a>障害  

 インスタンスは、 `FaultDescription` `wsdl:portType` / `wsdl:operation` / `wsdl:fault` 定義とそれに関連付けられている `wsdl:message` 定義にマップされます。 `wsdl:message` は、関連する WSDL ポートの種類と同じターゲット名前空間に追加されます。 `wsdl:message` には、`DefaultType` インスタンスの `FaultDescription` プロパティ値に相当する XML スキーマ要素を指す、"detail" という名前の 1 つのメッセージ部分があります。  
  
|プロパティ|WSDL マッピング|  
|----------------|------------------|  
|`Name`|`wsdl:portType` / `wsdl:operation` / `wsdl:fault` /@name エラーの値。|  
|`Namespace`|エラー詳細メッセージ部分が指す XML スキーマ要素の名前空間|  
|`Action`|エラーの SOAP アクションまたは WS-Addressing アクション|  
|`ProtectionLevel`|このエラーの `wsdl:message` 定義に関連付けられたセキュリティ ポリシーの保護アサーション|  
|`DetailType`|詳細メッセージ部分が指す要素の XML スキーマ型|  
|`Name, ContractDescription.Name, OperationDescription.Name,`|`wsdl:message`エラーメッセージの値を派生させるために使用し /@name ます。|  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Description>
