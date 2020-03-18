---
title: メタデータ エンドポイントを公開する
ms.date: 03/30/2017
ms.assetid: 29cd8a60-dfb7-460c-bf5a-c2b31b782671
ms.openlocfilehash: 4c6ac81f0c97415dc21ff3346178dd1e9936b7a5
ms.sourcegitcommit: 628e8147ca10187488e6407dab4c4e6ebe0cac47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72319892"
---
# <a name="publishing-metadata-endpoints"></a>メタデータ エンドポイントを公開する
Windows Communication Foundation (WCF) サービスは、1つまたは複数のメタデータエンドポイントを公開することによってメタデータを公開します。 サービス メタデータを公開すると、そのメタデータで WS-MetadataExchange (MEX) や HTTP/GET 要求などの標準化プロトコルを使用できるようになります。 メタデータのエンドポイントは、アドレス、バインディング、およびコントラクトを持つ他のサービス エンドポイントに似ており、構成またはコードを使用してサービス ホストに追加できます。 メタデータ エンドポイントの公開を有効にするには、<xref:System.ServiceModel.Description.ServiceMetadataBehavior> サービス動作をサービスに追加する必要があります。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [方法 : 構成ファイルを使用してサービスのメタデータを公開する](./feature-details/how-to-publish-metadata-for-a-service-using-a-configuration-file.md)  
 Ws-metadataexchange または HTTP/GET 要求を使用して、クライアントが `?wsdl` クエリ文字列を使用してメタデータを取得できるように、メタデータを公開するように WCF サービスを構成する方法を示します。  
  
 [方法 : コードを使用してサービスのメタデータを公開する](./feature-details/how-to-publish-metadata-for-a-service-using-code.md)  
 WCF サービスのメタデータ公開をコードで有効にする方法を示します。これにより、クライアントは、Ws-metadataexchange または HTTP/GET 要求を使用して、`?wsdl` クエリ文字列を使用してメタデータを取得できます。  
  
## <a name="see-also"></a>参照

- [メタデータの公開](./feature-details/publishing-metadata.md)
