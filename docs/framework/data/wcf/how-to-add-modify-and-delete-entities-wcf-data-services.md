---
title: '方法: エンティティを追加、変更、および削除する (WCF Data Services)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, changing data
ms.assetid: a00f8933-b232-4445-95ba-adc634f055d8
ms.openlocfilehash: 501bec59a61b51ec4bece4b0ce2f941189b35ed0
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569194"
---
# <a name="how-to-add-modify-and-delete-entities-wcf-data-services"></a>方法: エンティティを追加、変更、および削除する (WCF Data Services)
WCF Data Services クライアントライブラリを使用すると、<xref:System.Data.Services.Client.DataServiceContext>内のオブジェクトに対して同等のアクションを実行することで、データサービス内のエンティティデータを作成、更新、および削除できます。 詳細については、「[データサービスの更新](updating-the-data-service-wcf-data-services.md)」を参照してください。  
  
 このトピックの例では、Northwind サンプル データ サービスおよび自動生成されたクライアント データ サービス クラスを使用します。 このサービスとクライアントデータクラスは、 [WCF Data Services のクイックスタート](quickstart-wcf-data-services.md)を完了したときに作成されます。  
  
## <a name="example"></a>使用例  
 次の例では、新しいオブジェクト インスタンスを作成し、<xref:System.Data.Services.Client.DataServiceContext.AddObject%2A> で <xref:System.Data.Services.Client.DataServiceContext> メソッドを呼び出してコンテキスト内に項目を作成します。 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> メソッドが呼び出されたときに HTTP POST メッセージがデータ サービスに送信されます。  
  
 [!code-csharp[Astoria Northwind Client#AddProduct](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addproduct)]
 [!code-vb[Astoria Northwind Client#AddProduct](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addproduct)]  
  
## <a name="example"></a>使用例  
 次の例では、既存のオブジェクトを取得および変更してから、<xref:System.Data.Services.Client.DataServiceContext.UpdateObject%2A> で <xref:System.Data.Services.Client.DataServiceContext> メソッドを呼び出してコンテキスト内の項目を更新済みとしてマークします。 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> メソッドが呼び出されたとき HTTP MERGE メッセージがデータ サービスに送信されます。  
  
 [!code-csharp[Astoria Northwind Client#ModifyCustomer](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#modifycustomer)]
 [!code-vb[Astoria Northwind Client#ModifyCustomer](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#modifycustomer)]  
  
## <a name="example"></a>使用例  
 次の例では、<xref:System.Data.Services.Client.DataServiceContext.DeleteObject%2A> で <xref:System.Data.Services.Client.DataServiceContext> メソッドを呼び出して、コンテキスト内の項目を Deleted とマークします。 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> メソッドが呼び出されたとき HTTP DELETE メッセージがデータ サービスに送信されます。  
  
 [!code-csharp[Astoria Northwind Client#DeleteProduct](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#deleteproduct)]
 [!code-vb[Astoria Northwind Client#DeleteProduct](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#deleteproduct)]  
  
## <a name="example"></a>使用例  
 次の例では、新しいオブジェクト インスタンスを作成し、<xref:System.Data.Services.Client.DataServiceContext.AddRelatedObject%2A> で <xref:System.Data.Services.Client.DataServiceContext> メソッドを呼び出して、関連する注文へのリンクと共に、コンテキスト内に項目を作成します。 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> メソッドが呼び出されたときに HTTP POST メッセージがデータ サービスに送信されます。  
  
 [!code-csharp[Astoria Northwind Client#AddOrderDetailToOrderAuto](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addorderdetailtoorderauto)]
 [!code-vb[Astoria Northwind Client#AddOrderDetailToOrderAuto](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addorderdetailtoorderauto)]  
  
## <a name="see-also"></a>参照

- [WCF Data Services クライアント ライブラリ](wcf-data-services-client-library.md)
- [方法: 既存のエンティティを DataServiceContext にアタッチする](attach-an-existing-entity-to-dc-wcf-data.md)
- [方法: エンティティ リレーションシップを定義する](how-to-define-entity-relationships-wcf-data-services.md)
- [バッチ処理](batching-operations-wcf-data-services.md)
