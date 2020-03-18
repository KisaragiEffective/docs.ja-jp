---
title: '方法: 関連エンティティを読み込む (WCF Data Services)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, deferred content
- WCF Data Services, loading data
ms.assetid: 6f143d30-d997-4e6b-bcf0-d5c394ecb108
ms.openlocfilehash: 84c2448f317e813a95688feaaac1c97436de1b16
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569016"
---
# <a name="how-to-load-related-entities-wcf-data-services"></a>方法: 関連エンティティを読み込む (WCF Data Services)
WCF Data Services に関連付けられているエンティティを読み込む必要がある場合は、<xref:System.Data.Services.Client.DataServiceContext> クラスの <xref:System.Data.Services.Client.DataServiceContext.LoadProperty%2A> メソッドを使用できます。 また、<xref:System.Data.Services.Client.DataServiceQuery%601> で <xref:System.Data.Services.Client.DataServiceQuery%601.Expand%2A> メソッドを使用して、関連エンティティを同じクエリの応答で集中的に読み込むように要求することもできます。  
  
 このトピックの例では、Northwind サンプル データ サービスおよび自動生成されたクライアント データ サービス クラスを使用します。 このサービスとクライアントデータクラスは、 [WCF Data Services のクイックスタート](quickstart-wcf-data-services.md)を完了したときに作成されます。  
  
## <a name="example"></a>使用例  
 次の例は、返される各 `Customer` インスタンスに関連付けられた `Orders` を明示的に読み込む方法を示します。  
  
 [!code-csharp[Astoria Northwind Client#LoadRelatedOrderCustomer](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#loadrelatedordercustomer)]
 [!code-vb[Astoria Northwind Client#LoadRelatedOrderCustomer](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#loadrelatedordercustomer)]  
  
## <a name="example"></a>使用例  
 次の例は、<xref:System.Data.Services.Client.DataServiceQuery%601.Expand%2A> メソッドを使用して、クエリによって返される `Order Details` に属する `Orders` を返す方法を示します。  
  
 [!code-csharp[Astoria Northwind Client#ExpandOrderDetails](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#expandorderdetails)]
 [!code-vb[Astoria Northwind Client#ExpandOrderDetails](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#expandorderdetails)]  
  
## <a name="see-also"></a>参照

- [データ サービスに対するクエリ](querying-the-data-service-wcf-data-services.md)
