---
description: '詳細については、「方法: クラスを使用して Windows Communication Foundation コントラクトを作成する」を参照してください。'
title: '方法 : クラスを使用して Windows Communication Foundation コントラクトを作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1ad69393-3915-4e7f-9b91-b6fc59c6f5ba
ms.openlocfilehash: 63a5cc4e6a7966af69f2d5f3c7db0c7f4e313faf
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99756163"
---
# <a name="how-to-create-a-windows-communication-foundation-contract-with-a-class"></a>方法 : クラスを使用して Windows Communication Foundation コントラクトを作成する

Windows Communication Foundation (WCF) コントラクトを作成する場合は、インターフェイスを使用することをお勧めします。 詳細については、「 [方法: サービスコントラクトを定義](../how-to-define-a-wcf-service-contract.md)する」を参照してください。 ここで説明する代替方法では、クラスを作成してから、<xref:System.ServiceModel.ServiceContractAttribute> 属性を直接そのクラスに適用し、<xref:System.ServiceModel.OperationContractAttribute> 属性をコントラクトに含まれるクラス内の各メソッドに適用します。  
  
> [!WARNING]
> `[ServiceContract]` と `[ServiceContractAttribute]` は、同じことを行います。 とについても同じことが当てはまり `[OperationContract]` `[OperationContractAttribute]` ます。 どちらの場合も、前者は後者の短縮形です。  
  
 サービスコントラクトの詳細については、「 [サービスコントラクトの設計](../designing-service-contracts.md)」を参照してください。  
  
### <a name="creating-a-windows-communication-foundation-contract-with-a-class"></a>クラスを使用した Windows Communication Foundation コントラクトの作成  
  
1. Visual Basic、C#、またはその他の共通言語ランタイム言語を使用して、新しいクラスを作成します。  
  
2. クラスに <xref:System.ServiceModel.ServiceContractAttribute> クラスを適用します。  
  
3. クラスでメソッドを作成します。  
  
4. <xref:System.ServiceModel.OperationContractAttribute>パブリック WCF コントラクトの一部として公開する必要がある各メソッドにクラスを適用します。  
  
## <a name="example"></a>例  

 次のコード例は、サービス コントラクトを定義するクラスを示しています。  
  
 [!code-csharp[c_HowTo_CreateContractWithClass#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createcontractwithclass/cs/source.cs#1)]
 [!code-vb[c_HowTo_CreateContractWithClass#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_createcontractwithclass/vb/source.vb#1)]  
  
 <xref:System.ServiceModel.OperationContractAttribute> クラスが適用されたメソッドは、既定で要求/応答メッセージ パターンを使用します。 このメッセージパターンの詳細については、「 [方法: Request-Reply コントラクトを作成](how-to-create-a-request-reply-contract.md)する」を参照してください。 属性のプロパティを設定することにより、他のメッセージ パターンを作成および使用できるようになります。 その他の例については、「 [方法: One-Way コントラクトを作成](how-to-create-a-one-way-contract.md) する」および「 [方法: 双方向コントラクトを作成する](how-to-create-a-duplex-contract.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.ServiceContractAttribute>
- <xref:System.ServiceModel.OperationContractAttribute>
