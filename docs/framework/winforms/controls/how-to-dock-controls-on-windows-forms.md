---
title: コントロールをドッキングする
ms.date: 03/30/2017
helpviewer_keywords:
- controls [Windows Forms], docking
- Explorer-style applications [Windows Forms], creating
- Windows Forms controls, filling client area
ms.assetid: bc11f2e4-e90a-4830-b0e2-f43b6e2b8bec
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 02f1c26dcb322a39c41781c83d8c820bd2fd27e0
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745522"
---
# <a name="how-to-dock-controls-on-windows-forms"></a>方法: Windows フォームにコントロールをドッキングする

コントロールをフォームの端にドッキングしたり、コントロールのコンテナー (フォームまたはコンテナーコントロールのいずれか) に入力したりすることができます。 たとえば、Windows エクスプローラーでは、ウィンドウの左側に <xref:System.Windows.Forms.TreeView> コントロールがドッキングされ、<xref:System.Windows.Forms.ListView> コントロールがウィンドウの右側に表示されます。 表示されているすべての Windows フォームコントロールの <xref:System.Windows.Forms.Control.Dock%2A> プロパティを使用して、ドッキングモードを定義します。

> [!NOTE]
> コントロールは、逆 z オーダーでドッキングされます。

<xref:System.Windows.Forms.Control.Dock%2A> プロパティは、<xref:System.Windows.Forms.Control.AutoSize%2A> プロパティと対話します。 詳細については、「 [AutoSize プロパティの概要](autosize-property-overview.md)」を参照してください。

## <a name="to-dock-a-control"></a>コントロールをドッキングするには

1. Visual Studio で、ドッキングするコントロールを選択します。

2. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.Control.Dock%2A>] プロパティの右側にある矢印をクリックします。

   フォームの端と中央を表す一連のボックスを示すエディターが表示されます。

3. コントロールをドッキングするフォームの端を表すボタンをクリックします。 コントロールのフォームまたはコンテナーコントロールの内容を塗りつぶすには、中央のボックスをクリックします。 **[(なし)]** をクリックして、ドッキングを無効にします。

   ドッキングされた端の境界に合わせてコントロールのサイズが自動的に変更されます。

   > [!NOTE]
   > 継承できるようにするには、継承されたコントロールを `Protected` する必要があります。 コントロールのアクセスレベルを変更するには、 **[プロパティ]** ウィンドウでその **[修飾子]** プロパティを設定します。

## <a name="see-also"></a>参照

- [Windows フォーム コントロール](index.md)
- [各 Windows フォーム コントロールのラベル設定とショートカットの作成](labeling-individual-windows-forms-controls-and-providing-shortcuts-to-them.md)
- [Windows フォームで使用するコントロール](controls-to-use-on-windows-forms.md)
- [Windows フォーム コントロールの機能別一覧](windows-forms-controls-by-function.md)
- [方法: FlowLayoutPanel コントロールで子コントロールを固定およびドッキングする](how-to-anchor-and-dock-child-controls-in-a-flowlayoutpanel-control.md)
- [方法: TableLayoutPanel コントロールで子コントロールを固定およびドッキングする](how-to-anchor-and-dock-child-controls-in-a-tablelayoutpanel-control.md)
- [AutoSize プロパティの概要](autosize-property-overview.md)
- [方法: Windows フォームにコントロールを固定する](how-to-anchor-controls-on-windows-forms.md)
