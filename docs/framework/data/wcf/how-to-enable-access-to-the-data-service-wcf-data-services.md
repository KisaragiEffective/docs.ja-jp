---
title: '方法: データ サービスへのアクセスを有効にする (WCF Data Services)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, configuring
ms.assetid: 3d830bcd-32b4-4f26-9287-d58a071452c6
ms.openlocfilehash: 0ec9c9a730516b22b4eaa215e042e9393c01d752
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569107"
---
# <a name="how-to-enable-access-to-the-data-service-wcf-data-services"></a>方法: データ サービスへのアクセスを有効にする (WCF Data Services)
WCF Data Services では、データサービスによって公開されているリソースへのアクセス権を明示的に付与する必要があります。 つまり、新しいデータ サービスを作成した後も、個々のリソースに対し、エンティティ セットとして明示的にアクセスを許可する必要があります。 このトピックでは、[クイックスタート](quickstart-wcf-data-services.md)の完了時に作成された Northwind データサービスの5つのエンティティセットに対する読み取りおよび書き込みアクセスを有効にする方法について説明します。 <xref:System.Data.Services.EntitySetRights> 列挙体は <xref:System.FlagsAttribute> を使用して定義されているので、論理和演算子を使用して 1 つのエンティティ セットに複数のアクセス許可を指定できます。  
  
> [!NOTE]
> ASP.NET アプリケーションにアクセスできるクライアントは、データ サービスによって公開されるリソースにもアクセスできます。 運用データ サービスで、リソースへの承認されていないアクセスを防止するために、アプリケーション自身もセキュリティで保護する必要があります。 詳細については、「 [ASP.NET Web Sites のセキュリティ保護](https://docs.microsoft.com/previous-versions/aspnet/91f66yxt(v=vs.100))」を参照してください。  
  
### <a name="to-enable-access-to-the-data-service"></a>データ サービスへのアクセスを有効にするには  
  
- データ サービスのコードで、`InitializeService` 関数のプレースホルダーのコードを次の内容で置き換えます。  
  
     [!code-csharp[Astoria Quickstart Service#AllReadConfig](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_service/cs/northwind.svc.cs#allreadconfig)]
     [!code-vb[Astoria Quickstart Service#AllReadConfig](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_service/vb/northwind.svc.vb#allreadconfig)]  
  
     これにより、クライアントから `Orders` および `Order_Details` エンティティ セットへの読み取りおよび書き込みアクセスと、`Customers` エンティティ セットへの読み取り専用アクセスが有効になります。  
  
## <a name="see-also"></a>参照

- [方法: IIS 上で実行する WCF Data Service を開発する](how-to-develop-a-wcf-data-service-running-on-iis.md)
- [データ サービスの構成](configuring-the-data-service-wcf-data-services.md)
