---
description: '詳細情報: 方法:リフレクション プロバイダーを使用してデータ サービスを作成する (WCF Data Services)'
title: '方法: リフレクション プロバイダーを使用してデータ サービスを作成する (WCF Data Services)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, providers
ms.assetid: 7315c6d8-f452-4fb2-a0c1-76ab0593c146
ms.openlocfilehash: 1c4d131b21c69e11dd6d8b574e4c22a6af7c5a25
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99766231"
---
# <a name="how-to-create-a-data-service-using-the-reflection-provider-wcf-data-services"></a>方法: リフレクション プロバイダーを使用してデータ サービスを作成する (WCF Data Services)

[!INCLUDE [wcf-deprecated](~/includes/wcf-deprecated.md)]

WCF Data Services では、<xref:System.Linq.IQueryable%601> インターフェイスを実装するオブジェクトとして公開されているクラスに基づいてデータ モデルを定義できます。 詳細については、「[Data Services プロバイダー](data-services-providers-wcf-data-services.md)」を参照してください。  
  
## <a name="example"></a>例  

 次の例は、`Orders` および `Items` を含むデータ モデルを定義します。 エンティティ コンテナー クラス `OrderItemData` には、<xref:System.Linq.IQueryable%601> インターフェイスを返す 2 つのパブリック メソッドがあります。 これらのインターフェイスは、`Orders` エンティティ型と `Items` エンティティ型のエンティティ セットです。 `Order` には複数の `Items` を含めることができるので、`Orders` エンティティ型には `Items` オブジェクトのコレクションを返す `Items` ナビゲーション プロパティがあります。 `OrderItemData` エンティティ コンテナー クラスは、<xref:System.Data.Services.DataService%601> データ サービスが派生する `OrderItems` クラスのジェネリック型です。  
  
> [!NOTE]
> この例はメモリ内のデータ プロバイダーを示し、変更は現在のオブジェクト インスタンスの外部で永続化されないので、<xref:System.Data.Services.IUpdatable> インターフェイスを実装する利点はありません。 <xref:System.Data.Services.IUpdatable> インターフェイスを実装する例については、「[方法: LINQ to SQL データ ソースを使用してデータ サービスを作成する](create-a-data-service-using-linq-to-sql-wcf.md)」を参照してください。  
  
 [!code-csharp[Astoria Reflection Provider#CustomIQueryable](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_reflection_provider/cs/orderitems.svc.cs#customiqueryable)]
 [!code-vb[Astoria Reflection Provider#CustomIQueryable](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_reflection_provider/vb/orderitems.svc.vb#customiqueryable)]  
  
## <a name="see-also"></a>関連項目

- [方法: LINQ to SQL データ ソースを使用してデータ サービスを作成する](create-a-data-service-using-linq-to-sql-wcf.md)
- [Data Services プロバイダー](data-services-providers-wcf-data-services.md)
- [方法: ADO.NET Entity Framework データ ソースを使用してデータ サービスを作成する](create-a-data-service-using-an-adonet-ef-data-wcf.md)
