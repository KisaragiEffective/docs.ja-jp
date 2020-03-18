---
title: '方法: 非同期データ サービス クエリを実行する (WCF Data Services)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, asynchronous operations
- asynchronous operations [WCF Data Services]
ms.assetid: 902a2dc1-d0e9-4b00-90a8-becc4cb1f6a7
ms.openlocfilehash: 68e2035315780b7c6dd60e93ae6eb10d252aabb3
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569068"
---
# <a name="how-to-execute-asynchronous-data-service-queries-wcf-data-services"></a>方法: 非同期データ サービス クエリを実行する (WCF Data Services)
WCF Data Services クライアントライブラリを使用すると、クエリの実行や変更の保存などのクライアントサーバー操作を非同期に実行できます。 詳細については、「[非同期操作](asynchronous-operations-wcf-data-services.md)」を参照してください。  
  
> [!NOTE]
> 特定のスレッドでコールバックが呼び出される必要のあるアプリケーションでは、<xref:System.Data.Services.Client.DataServiceQuery%601.EndExecute%2A> メソッドの実行を明示的にマーシャリングする必要があります。 詳細については、「[非同期操作](asynchronous-operations-wcf-data-services.md)」を参照してください。  
  
 このトピックの例では、Northwind サンプル データ サービスおよび自動生成されたクライアント データ サービス クラスを使用します。 このサービスとクライアントデータクラスは、 [WCF Data Services のクイックスタート](quickstart-wcf-data-services.md)を完了したときに作成されます。  
  
## <a name="example"></a>使用例  
 次の例では、<xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A> メソッドを呼び出してクエリを開始することで、非同期クエリを実行する方法を示します。 インライン デリゲートは、<xref:System.Data.Services.Client.DataServiceQuery%601.EndExecute%2A> メソッドを呼び出してクエリ結果を表示します。  
  
 [!code-csharp[Astoria Northwind Client#ExecuteQueryAsync](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#executequeryasync)]
 [!code-vb[Astoria Northwind Client#ExecuteQueryAsync](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#executequeryasync)]  
  
## <a name="see-also"></a>参照

- [WCF Data Services クライアント ライブラリ](wcf-data-services-client-library.md)
