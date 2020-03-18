---
title: '方法 : ToolStripTextBox を拡大して ToolStrip の残りの幅に合わせる'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- text boxes [Windows Forms], stretching in ToolStrip control [Windows Forms]
- ToolStrip control [Windows Forms], stretching a text box
ms.assetid: 0e610fbf-85fe-414c-900c-9704a5dd5cc6
ms.openlocfilehash: c60cc2a377f08a73159f25b2ab5f2812d41f0c10
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742844"
---
# <a name="how-to-stretch-a-toolstriptextbox-to-fill-the-remaining-width-of-a-toolstrip-windows-forms"></a>方法 : ToolStripTextBox を拡大して ToolStrip の残りの幅に合わせる (Windows フォーム)
<xref:System.Windows.Forms.ToolStrip> コントロールの <xref:System.Windows.Forms.ToolStrip.Stretch%2A> プロパティを `true`に設定すると、コントロールによってコンテナーが端から端に挿入され、コンテナーのサイズが変更されるときにサイズが変更されます。 この構成では、<xref:System.Windows.Forms.ToolStripTextBox>などのコントロールの項目を拡張して、使用可能な領域を塗りつぶすと共に、コントロールのサイズを変更するときにサイズを変更すると便利な場合があります。 この拡大は、Microsoft® Internet Explorer のアドレスバーと同様の外観と動作を実現する場合などに便利です。  
  
## <a name="example"></a>例  
 次のコード例では、`ToolStripSpringTextBox`と呼ばれる <xref:System.Windows.Forms.ToolStripTextBox> から派生したクラスを提供しています。 このクラスは、<xref:System.Windows.Forms.ToolStripTextBox.GetPreferredSize%2A> メソッドをオーバーライドして、他のすべての項目の合計幅が減算された後に、親 <xref:System.Windows.Forms.ToolStrip> コントロールの使用可能な幅を計算します。 このコード例には、<xref:System.Windows.Forms.Form> クラスと、新しい動作を示すための `Program` クラスも用意されています。  
  
 [!code-csharp[ToolStripSpringTextBox#00](~/samples/snippets/csharp/VS_Snippets_Winforms/ToolStripSpringTextBox/cs/ToolStripSpringTextBox.cs#00)]
 [!code-vb[ToolStripSpringTextBox#00](~/samples/snippets/visualbasic/VS_Snippets_Winforms/ToolStripSpringTextBox/vb/ToolStripSpringTextBox.vb#00)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- System、System.Drawing、および System.Windows.Forms の各アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ToolStrip>
- <xref:System.Windows.Forms.ToolStrip.Stretch%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.ToolStripTextBox>
- <xref:System.Windows.Forms.ToolStripTextBox.GetPreferredSize%2A?displayProperty=nameWithType>
- [ToolStrip コントロールのアーキテクチャ](toolstrip-control-architecture.md)
- [方法: StatusStrip 内で Spring プロパティを対話的に使用する](how-to-use-the-spring-property-interactively-in-a-statusstrip.md)
