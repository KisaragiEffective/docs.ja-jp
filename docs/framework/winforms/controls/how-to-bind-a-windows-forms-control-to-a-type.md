---
title: コントロールを型にバインドする
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- controls [Windows Forms], binding to a type
- BindingSource component [Windows Forms], binding to a type
- types [Windows Forms], binding controls to
ms.assetid: 94faeebb-d2bc-45d6-86d7-96a42661b43d
ms.openlocfilehash: 0bc1f92ee8922990bd0e461655168f5618ba39a6
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744992"
---
# <a name="how-to-bind-a-windows-forms-control-to-a-type"></a>方法 : Windows フォーム コントロールを型にバインドする
データをやり取りするコントロールを作成する際、オブジェクトではなく型にコントロールをバインドすることが必要な場合があります。 このような状況は、特にデータを使用できないデザイン時に発生しますが、データ バインド コントロールは、型のパブリック インターフェイスから情報を表示する必要があります。 たとえば、<xref:System.Windows.Forms.DataGridView> コントロールを Web サービスによって公開されているオブジェクトにバインドし、デザイン時に <xref:System.Windows.Forms.DataGridView> コントロールで列にカスタム型のメンバー名のラベルを付けたいときがあります。  
  
 <xref:System.Windows.Forms.BindingSource> コンポーネントを使用して、コントロールを型に簡単にバインドできます。  
  
## <a name="example"></a>例  
 次のサンプル コードは、<xref:System.Windows.Forms.DataGridView> コンポーネントを使用して、<xref:System.Windows.Forms.BindingSource> コントロールをカスタム型にバインドする方法を示します。 例を実行すると、コントロールにデータを設定する前に、<xref:System.Windows.Forms.DataGridView> に `Customer` オブジェクトのプロパティを反映するラベルが付いた列があることに気付きます。 この例には、データを <xref:System.Windows.Forms.DataGridView> コントロールに追加する [顧客の追加] ボタンがあります。 ボタンをクリックすると、新しい `Customer` オブジェクトが <xref:System.Windows.Forms.BindingSource> に追加されます。 実際のシナリオでは、Web サービスまたはその他のデータ ソースへの呼び出しにより、データを取得する可能性があります。  
  
 [!code-csharp[System.Windows.Forms.DataConnector.BindingToType#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataConnector.BindingToType/CS/form1.cs#1)]
 [!code-vb[System.Windows.Forms.DataConnector.BindingToType#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataConnector.BindingToType/VB/form1.vb#1)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- System アセンブリおよび System.Windows.Forms アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.BindingNavigator>
- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.BindingSource>
- [BindingSource コンポーネント](bindingsource-component.md)
