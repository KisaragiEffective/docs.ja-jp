---
title: '方法: クエリ結果を射影する (WCF Data Services)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- projection [WCF Data Services]
- WCF Data Services, projection
- query projection [WCF Data Services]
- WCF Data Services, querying
ms.assetid: 474ac625-8770-43ba-8320-d3315ea9530f
ms.openlocfilehash: 1e94b5f65229887b2d310f15499a2b14ef7c0536
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569001"
---
# <a name="how-to-project-query-results-wcf-data-services"></a>方法: クエリ結果を射影する (WCF Data Services)
射影は、エンティティの特定のプロパティのみが応答で返されるように指定することにより、クエリによって返されるデータの量を減らすためのメカニズムです。 `$select` クエリオプションを使用するか、LINQ クエリで[select](../../../csharp/language-reference/keywords/select-clause.md)句 ([select](../../../visual-basic/language-reference/queries/select-clause.md) in Visual Basic) を使用して、WCF Data Services クエリの結果に対して射影を実行できます。 詳細については、「[データサービスのクエリ](querying-the-data-service-wcf-data-services.md)」を参照してください。  
  
 このトピックの例では、Northwind サンプル データ サービスおよび自動生成されたクライアント データ サービス クラスを使用します。 このサービスとクライアントデータクラスは、 [WCF Data Services のクイックスタート](quickstart-wcf-data-services.md)を完了したときに作成されます。  
  
## <a name="example"></a>使用例  
 次の例では、アドレス特有のプロパティと Identity プロパティのみを含む新しい CustomerAddress 型に Customer エンティティを射影する LINQ クエリを示します。 この `CustomerAddress` クラスはクライアントで定義され、クライアント ライブラリがエンティティ型として認識できるように属性化されています。  
  
 [!code-csharp[Astoria Northwind Client#SelectCustomerAddress](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#selectcustomeraddress)]
 [!code-vb[Astoria Northwind Client#SelectCustomerAddress](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#selectcustomeraddress)]  
  
## <a name="example"></a>使用例  
 次の例では、返された Customers エンティティを、Identity プロパティは含まずにアドレス特有のプロパティのみを含む新しい CustomerAddressNonEntity 型に射影する LINQ クエリを示します。 この `CustomerAddressNonEntity` クラスはクライアントで定義され、エンティティ型としては属性化されません。  
  
 [!code-csharp[Astoria Northwind Client#SelectCustomerAddressNonEntity](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#selectcustomeraddressnonentity)]
 [!code-vb[Astoria Northwind Client#SelectCustomerAddressNonEntity](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#selectcustomeraddressnonentity)]  
  
## <a name="example"></a>使用例  
 次の例では、前の例で使用した `CustomerAddress` および `CustomerAddressNonEntity` 型の定義を示します。  
  
 [!code-csharp[Astoria Northwind Client#CustomerAddressDefinition](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/customeraddress.cs#customeraddressdefinition)]
 [!code-vb[Astoria Northwind Client#CustomerAddressDefinition](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customeraddress.vb#customeraddressdefinition)]
