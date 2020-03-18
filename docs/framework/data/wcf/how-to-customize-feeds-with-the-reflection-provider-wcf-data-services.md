---
title: '方法: リフレクション プロバイダーでフィードをカスタマイズする (WCF Data Services)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, customizing
- WCF Data Services, customizing feeds
ms.assetid: 00c23dcf-9bb8-459a-a012-6c4d9bcad7e9
ms.openlocfilehash: d3e2d587978a4c82784c8cfc8a7acc17cf601c3a
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569143"
---
# <a name="how-to-customize-feeds-with-the-reflection-provider-wcf-data-services"></a>方法: リフレクション プロバイダーでフィードをカスタマイズする (WCF Data Services)
WCF Data Services を使用すると、データサービス応答の Atom シリアル化をカスタマイズして、AtomPub プロトコルで定義されている未使用の要素にエンティティのプロパティをマップできます。 このトピックでは、リフレクション プロバイダーを使用して、定義されているデータ モデル内のエンティティ型のマッピング属性を定義する方法について説明します。 詳細については、「[フィードのカスタマイズ](feed-customization-wcf-data-services.md)」を参照してください。  
  
 この例のデータモデルは、「[方法: リフレクションプロバイダーを使用してデータサービスを作成](create-a-data-service-using-rp-wcf-data-services.md)する」で定義されています。  
  
## <a name="example"></a>使用例  
 次の例では、`Order` 型の両方のプロパティが既存の Atom 要素にマップされます。 `Product` 型の `Item` プロパティは、別の名前空間のカスタム フィード属性にマップされます。  
  
 [!code-csharp[Astoria Custom Feeds#CustomIQueryableFeeds](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_custom_feeds/cs/orderitems.svc.cs#customiqueryablefeeds)]
 [!code-vb[Astoria Custom Feeds#CustomIQueryableFeeds](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_custom_feeds/vb/orderitems.svc.vb#customiqueryablefeeds)]  
  
## <a name="example"></a>使用例  
 前の例では、URI `http://myservice/OrderItems.svc/Orders(0)?$expand=Items` に次の結果が返されます。  
  
 [!code-xml[Astoria Custom Feeds#IQueryableFeedResultInline](../../../../samples/snippets/xml/VS_Snippets_Misc/astoria_custom_feeds/xml/iqueryablefeedresultinline.xml#iqueryablefeedresultinline)]  
  
## <a name="see-also"></a>参照

- [リフレクション プロバイダー](reflection-provider-wcf-data-services.md)
