---
title: '方法: クラスまたは構造体に基本的なデータ コントラクトを作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataMemberAttribute
- DataContractAttribute class
- data contracts [WCF], creating for a class or structure
ms.assetid: bc464889-3070-4a2f-91d2-e788a0f686a7
ms.openlocfilehash: 15c59f3ee7cbefafef7a304cfd1477685fff68f2
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/22/2019
ms.locfileid: "69968461"
---
# <a name="how-to-create-a-basic-data-contract-for-a-class-or-structure"></a>方法: クラスまたは構造体に基本的なデータ コントラクトを作成する
このトピックでは、クラスまたは構造体を使用してデータ コントラクトを作成する基本的な手順を示します。 データコントラクトとその使用方法の詳細については、「[データコントラクトの使用](../../../../docs/framework/wcf/feature-details/using-data-contracts.md)」を参照してください。  
  
 基本的な Windows Communication Foundation (WCF) サービスとクライアントを作成する手順を説明するチュートリアルについては、[はじめにのチュートリアル](../../../../docs/framework/wcf/getting-started-tutorial.md)を参照してください。 基本的なサービスとクライアントで構成される実用的なサンプルアプリケーションについては、「[基本的なデータコントラクト](../../../../docs/framework/wcf/samples/basic-data-contract.md)」を参照してください。  
  
### <a name="to-create-a-basic-data-contract-for-a-class-or-structure"></a>クラスまたは構造体に基本的なデータ コントラクトを作成するには  
  
1. クラスに <xref:System.Runtime.Serialization.DataContractAttribute> 属性を適用することにより、データ コントラクトを持つ型であることを宣言します。 パブリック型は、属性のないものも含めてすべてシリアル化されます。 <xref:System.Runtime.Serialization.DataContractSerializer> 属性がない場合は、<xref:System.Runtime.Serialization.DataContractAttribute> によってデータ コントラクトが推論されます。 詳細については、「[シリアル化](../../../../docs/framework/wcf/feature-details/serializable-types.md)可能な型」を参照してください。  
  
2. シリアル化するメンバー (プロパティ、フィールド、またはイベント) を定義します。これは、該当する各メンバーに <xref:System.Runtime.Serialization.DataMemberAttribute> 属性を適用することで行います。 このようなメンバーのことを、データ メンバーと呼びます。 既定では、すべてのパブリック型がシリアル化されます。 詳細については、「[シリアル化](../../../../docs/framework/wcf/feature-details/serializable-types.md)可能な型」を参照してください。  
  
    > [!NOTE]
    > プライベート フィールドであっても、<xref:System.Runtime.Serialization.DataMemberAttribute> 属性を適用すると、データが外部に公開されることになります。 機密性のあるデータが含まれていないかどうか確認してください。  
  
## <a name="example"></a>例  
 次の例では、`Person` 属性と <xref:System.Runtime.Serialization.DataContractAttribute> 属性をクラスとそのメンバーに適用して、<xref:System.Runtime.Serialization.DataMemberAttribute> 型のデータ コントラクトを作成する方法を示します。  
  
 [!code-csharp[DataContractAttribute#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/datacontractattribute/cs/overview.cs#2)]
 [!code-vb[DataContractAttribute#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/datacontractattribute/vb/overview.vb#2)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- [データ コントラクトの使用](../../../../docs/framework/wcf/feature-details/using-data-contracts.md)
- [チュートリアル入門](../../../../docs/framework/wcf/getting-started-tutorial.md)
- [はじめに](../../../../docs/framework/wcf/samples/getting-started-sample.md)
