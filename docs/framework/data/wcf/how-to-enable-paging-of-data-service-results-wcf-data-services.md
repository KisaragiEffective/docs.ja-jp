---
title: '方法: データ サービスの結果のページングを有効にする (WCF Data Services)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- paging output [WCF Data Services]
ms.assetid: 9a316cbd-9612-4482-a541-a10bc78b2635
ms.openlocfilehash: 6b7cea2475a5c11091a04ef3044bbc958e55fc5d
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569087"
---
# <a name="how-to-enable-paging-of-data-service-results-wcf-data-services"></a>方法: データ サービスの結果のページングを有効にする (WCF Data Services)
WCF Data Services を使用すると、データサービスクエリによって返されるエンティティの数を制限できます。 ページ制限は、サービスの初期化時に呼び出されるメソッドで定義され、エンティティ セットごとに設定できます。  
  
 ページングが有効である場合、フィードの最終的なエントリには、データの次のページへのリンクが含まれます。 詳細については、「[データサービスの構成](configuring-the-data-service-wcf-data-services.md)」を参照してください。  
  
 このトピックでは、返された `Customers` エンティティ セットおよび `Orders` エンティティ セットのページングを有効にするためにデータ サービスを変更する方法について説明します。 このトピックの例では、Northwind サンプル データ サービスを使用します。 このサービスは、 [WCF Data Services のクイックスタート](quickstart-wcf-data-services.md)を完了したときに作成されます。  
  
### <a name="how-to-enable-paging-of-returned-customers-and-orders-entity-sets"></a>返された Customer エンティティ セットおよび Orders エンティティ セットのページングを有効化する方法  
  
- データ サービスのコードで、`InitializeService` 関数のプレースホルダーのコードを次の内容で置き換えます。  
  
     [!code-csharp[Astoria Northwind Service#DataServiceConfigPaging](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_service/cs/northwind.svc.cs#dataserviceconfigpaging)]
     [!code-vb[Astoria Northwind Service#DataServiceConfigPaging](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_service/vb/northwind.svc.vb#dataserviceconfigpaging)]  
  
## <a name="see-also"></a>参照

- [遅延コンテンツの読み込み](loading-deferred-content-wcf-data-services.md)
- [方法: ページングされた結果を読み込む](how-to-load-paged-results-wcf-data-services.md)
