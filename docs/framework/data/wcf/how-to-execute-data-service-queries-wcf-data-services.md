---
title: '方法: データ サービス クエリを実行する (WCF Data Services)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- querying the data service [WCF Data Services]
- WCF Data Services, querying
- WCF Data Services, accessing data
ms.assetid: 62997821-e0c6-4c4d-9fb7-1273fb5e5d18
ms.openlocfilehash: b06a21a45dcf6e67c41287c4cd59cdda4aa7b447
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569076"
---
# <a name="how-to-execute-data-service-queries-wcf-data-services"></a>方法: データ サービス クエリを実行する (WCF Data Services)
WCF Data Services を使用すると、生成されたクライアントデータサービスクラスを使用して、.NET Framework ベースのクライアントアプリケーションからデータサービスを照会できます。 次の方法のいずれかを使用してクエリを実行できます。  
  
- <xref:System.Data.Services.Client.DataServiceQuery%601> ツールによって生成される <xref:System.Data.Services.Client.DataServiceContext> から取得した名前付きの `Add Data Service Reference` に対して LINQ クエリを実行する。  
  
- 暗黙的に、<xref:System.Data.Services.Client.DataServiceQuery%601> ツールによって生成される <xref:System.Data.Services.Client.DataServiceContext> から取得した名前付きの `Add Data Service Reference` を列挙する。  
  
- 明示的に、<xref:System.Data.Services.Client.DataServiceContext.Execute%2A> で <xref:System.Data.Services.Client.DataServiceQuery%601> メソッド (非同期実行の場合は <xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A> メソッド) を呼び出す。  
  
 詳細については、「[データサービスのクエリ](querying-the-data-service-wcf-data-services.md)」を参照してください。  
  
 このトピックの例では、Northwind サンプル データ サービスおよび自動生成されたクライアント データ サービス クラスを使用します。 このサービスとクライアントデータクラスは、 [WCF Data Services のクイックスタート](quickstart-wcf-data-services.md)を完了したときに作成されます。  
  
## <a name="example"></a>使用例  
 次の例は、すべての `Customers` を返す LINQ クエリを定義して Northwind データ サービスに対して実行する方法を示します。  
  
 [!code-csharp[Astoria Northwind Client#GetAllCustomersLinq](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#getallcustomerslinq)]
 [!code-vb[Astoria Northwind Client#GetAllCustomersLinq](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#getallcustomerslinq)]  
  
## <a name="example"></a>使用例  
 次の例は、`Add Data Service Reference` ツールによって生成されるコンテキストを使用して、すべての `Customers` を返すクエリを Northwind データ サービスに対して暗黙的に実行する方法を示します。 要求された `Customers` エンティティ セットの URI は、コンテキストによって自動的に決定されます。 クエリは、列挙が行われるときに暗黙的に実行されます。  
  
 [!code-csharp[Astoria Northwind Client#GetAllCustomers](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#getallcustomers)]
 [!code-vb[Astoria Northwind Client#GetAllCustomers](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#getallcustomers)]  
  
## <a name="example"></a>使用例  
 次の例は、<xref:System.Data.Services.Client.DataServiceContext> を使用して、すべての `Customers` を返すクエリを Northwind データ サービスに対して明示的に実行する方法を示します。  
  
 [!code-csharp[Astoria Northwind Client#GetAllCustomersExplicit](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#getallcustomersexplicit)]
 [!code-vb[Astoria Northwind Client#GetAllCustomersExplicit](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#getallcustomersexplicit)]  
  
## <a name="see-also"></a>参照

- [方法: データ サービス クエリにクエリ オプションを追加する](how-to-add-query-options-to-a-data-service-query-wcf-data-services.md)
