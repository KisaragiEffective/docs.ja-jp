---
title: '方法: クエリで返されたエンティティの数を確認する (WCF Data Services)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, row count
ms.assetid: 03d41a82-df95-40ac-8439-a6c327d37ba8
ms.openlocfilehash: 43833566d59b15ef382872b0c40f452192b59bac
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74568927"
---
# <a name="how-to-determine-the-number-of-entities-returned-by-a-query-wcf-data-services"></a>方法: クエリで返されたエンティティの数を確認する (WCF Data Services)
WCF Data Services を使用すると、クエリ URI によって指定されたエンティティセット内のエンティティの数を確認できます。 この数は、クエリ結果と一緒に、または整数値として含まれます。 詳細については、「[データサービスのクエリ](querying-the-data-service-wcf-data-services.md)」を参照してください。  
  
 このトピックの例では、Northwind サンプル データ サービスおよび自動生成されたクライアント データ サービス クラスを使用します。 このサービスとクライアントデータクラスは、 [WCF Data Services のクイックスタート](quickstart-wcf-data-services.md)を完了したときに作成されます。  
  
## <a name="example"></a>使用例  
 次の例では、<xref:System.Data.Services.Client.DataServiceQuery%601.IncludeTotalCount%2A> メソッドを呼び出した後でクエリを実行します。 <xref:System.Data.Services.Client.QueryOperationResponse%601.TotalCount%2A> プロパティが、`Customers` エンティティ セット内のエンティティの数を返します。  
  
 [!code-csharp[Astoria Northwind Client#CountAllCustomers](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#countallcustomers)]
 [!code-vb[Astoria Northwind Client#CountAllCustomers](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#countallcustomers)]  
  
## <a name="example"></a>使用例  
 次の例では、<xref:System.Linq.Enumerable.Count%2A> エンティティ セット内のエンティティの数である整数値のみを返す `Customers` メソッドを呼び出します。  
  
 [!code-csharp[Astoria Northwind Client#CountAllCustomersValueOnly](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#countallcustomersvalueonly)]
 [!code-vb[Astoria Northwind Client#CountAllCustomersValueOnly](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#countallcustomersvalueonly)]  
  
## <a name="see-also"></a>参照

- [データ サービスに対するクエリ](querying-the-data-service-wcf-data-services.md)
