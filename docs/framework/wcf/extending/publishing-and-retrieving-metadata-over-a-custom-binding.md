---
description: '詳細情報: カスタム バインディングを介したメタデータの公開と取得'
title: カスタム バインディングを介したメタデータの公開と取得
ms.date: 03/30/2017
ms.assetid: 904e11b4-d90e-45c6-9ee5-c3472c90008c
ms.openlocfilehash: 26532c3478d8250e9f6ec7dbb9431be5052239b4
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99644126"
---
# <a name="publishing-and-retrieving-metadata-over-a-custom-binding"></a>カスタム バインディングを介したメタデータの公開と取得

<xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> は、サービスにメタデータ エンドポイントを追加するためのサポートを提供します。 これらのメタデータ エンドポイントは、`?wsdl` クエリ文字列を持つ URL での HTTP GET 要求、および WS-MetadataExchange (MEX) 仕様で定義された WS-Transfer GET 要求に応答できます。 MEX エンドポイントは、<xref:System.ServiceModel.Description.IMetadataExchange?displayProperty=nameWithType> コントラクトを実装します。  
  
## <a name="publishing-metadata-over-a-custom-binding"></a>カスタム バインドを介したメタデータの公開  

 HTTP GET メタデータ エンドポイントと HTTPS GET メタデータ エンドポイントを有効にするには、<xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpGetEnabled%2A?displayProperty=nameWithType> プロパティまたは <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetEnabled%2A?displayProperty=nameWithType> プロパティを `true` に設定します。 これらのエンドポイントのバインディングは構成できません。  
  
 ただし、<xref:System.ServiceModel.Description.IMetadataExchange> コントラクトは、カスタム バインドを使用するエンドポイントを含むすべてのエンドポイントで使用できます。これは、<xref:System.ServiceModel.Description.IMetadataExchange> エンドポイントは他のすべての Windows Communication Foundation (WCF) サービス エンドポイントと同じであるためです。 システム指定のバインディングの構成を変更する方法または <xref:System.ServiceModel.Channels.CustomBinding?displayProperty=nameWithType> の構成方法を理解している場合は、<xref:System.ServiceModel.Description.IMetadataExchange> エンドポイントで使用されるバインディングを構成できます。  
  
## <a name="retrieving-metadata-over-a-custom-binding"></a>カスタム バインドを介したメタデータの取得  

 メタデータは、標準の HTTP 要求または HTTPS GET 要求を使用して HTTP Get および HTTPS Get メタデータ エンドポイントから取得できます。  
  
 MEX メタデータ エンドポイントからメタデータを取得するには、通常、WCF でサポートされている標準の MEX バインディングのいずれかを使用できます。 詳細については、「<xref:System.ServiceModel.Description.MetadataExchangeBindings?displayProperty=nameWithType>」を参照してください。 <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> 型および Svcutil.exe ツールでは、指定したメタデータ エンドポイントのアドレスに基づいて、標準の MEX バインディングの 1 つが自動的に選択されます。  
  
 MEX メタデータ エンドポイントで標準の MEX バインディングとは異なるバインディングを使用する場合は、コードを使用するか、<xref:System.ServiceModel.Description.MetadataExchangeClient> クライアント エンドポイント構成を提供することにより、<xref:System.ServiceModel.Description.IMetadataExchange> で使用されるバインディングを構成できます。 Svcutil.exe ツールは、構成ファイルから、メタデータ エンドポイント アドレスの URI スキームと同じ名前を持つ <xref:System.ServiceModel.Description.IMetadataExchange> クライアント エンドポイント構成を自動的に読み込みます。  
  
## <a name="security"></a>セキュリティ  

 カスタム バインドを介してメタデータを公開する場合、メタデータに必要なセキュリティ サポートをそのカスタム バインドが提供することを確認してください。 たとえば、情報の漏えいを防止し、メタデータを取得するための権限がクライアントにあることを確認するには、認証および暗号化を要求するように <xref:System.ServiceModel.Description.IMetadataExchange> エンドポイントを構成することによってメタデータとアプリケーションのセキュリティを強化します。 サンプルの[カスタム セキュア メタデータ エンドポイント](../samples/custom-secure-metadata-endpoint.md)は、このシナリオを示しています。  
  
## <a name="see-also"></a>関連項目

- [サービスのセキュリティ保護](../securing-services.md)
- [WS-MetadataExchange のバインディング](ws-metadataexchange-bindings.md)
- [方法: カスタム WS-Metadata Exchange バインディングを構成する](how-to-configure-a-custom-ws-metadata-exchange-binding.md)
- [方法: MEX 以外のバインディングを介してメタデータを取得する](how-to-retrieve-metadata-over-a-non-mex-binding.md)
