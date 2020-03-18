---
title: コントロールを固定する
ms.date: 03/30/2017
helpviewer_keywords:
- Anchor property [Windows Forms], enabling resizable forms
- Windows Forms controls, screen resolutions
- resizing forms [Windows Forms]
- Windows Forms controls, size
- screen resolution and control display
- controls [Windows Forms], anchoring
- forms [Windows Forms], resizing
- Windows Forms, resizing
- controls [Windows Forms], positioning
ms.assetid: 59ea914f-fbd3-427a-80fe-decd02f7ae6d
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 7c307d8c5b3bc32e15e6de048c434854ef1bbc65
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76747183"
---
# <a name="how-to-anchor-controls-on-windows-forms"></a>方法: Windows フォームのコントロールを固定する

実行時にユーザーがサイズ変更できるフォームをデザインする場合は、フォーム上のコントロールのサイズと位置が適切に変更される必要があります。 フォームを使用してコントロールのサイズを動的に変更するには、Windows フォームコントロールの <xref:System.Windows.Forms.Control.Anchor%2A> プロパティを使用します。 <xref:System.Windows.Forms.Control.Anchor%2A> プロパティは、コントロールのアンカー位置を定義します。 コントロールがフォームに固定され、フォームのサイズが変更されると、コントロールは、コントロールとアンカー位置との距離を維持します。 たとえば、フォームのサイズが変更されたときにフォームの左端、右端、および下端に固定されている <xref:System.Windows.Forms.TextBox> コントロールがある場合は、フォームの右側と左側から同じ距離が維持されるように、<xref:System.Windows.Forms.TextBox> コントロールが水平方向にサイズ変更されます。 また、コントロールは、その位置が常にフォームの下端から同じ距離になるように垂直方向に配置されます。 コントロールが固定されておらず、フォームのサイズが変更されている場合は、フォームの端を基準としたコントロールの位置が変更されます。

<xref:System.Windows.Forms.Control.Anchor%2A> プロパティは、<xref:System.Windows.Forms.Control.AutoSize%2A> プロパティと対話します。 詳細については、「 [AutoSize プロパティの概要](autosize-property-overview.md)」を参照してください。

## <a name="anchor-a-control-on-a-form"></a>フォームにコントロールを固定する

1. Visual Studio で、アンカーするコントロールを選択します。

    > [!NOTE]
    > 複数のコントロールを同時に固定するには、CTRL キーを押しながら各コントロールをクリックして選択し、この手順の残りの部分を実行します。

2. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.Control.Anchor%2A>] プロパティの右側にある矢印をクリックします。

     クロスを示すエディターが表示されます。

3. アンカーを設定するには、交差部分の上、左、右、または下のセクションをクリックします。

     既定では、コントロールは上と左に固定されています。

4. アンカーされているコントロールの辺をクリアするには、クロスの arm をクリックします。

5. <xref:System.Windows.Forms.Control.Anchor%2A> プロパティエディターを閉じるには、<xref:System.Windows.Forms.Control.Anchor%2A> のプロパティ名をもう一度クリックします。

実行時にフォームを表示すると、フォームの端と同じ距離に配置されたままになるようにコントロールのサイズが変更されます。 固定されたエッジからの距離は、コントロールが Windows フォームデザイナーに配置されているときに定義されている距離と常に同じままです。

> [!NOTE]
> <xref:System.Windows.Forms.ComboBox> コントロールなどの特定のコントロールの高さには制限があります。 コントロールをフォームまたはコンテナーの下部に固定すると、コントロールの高さの制限を超えることはありません。

継承されたコントロールは、固定できるように `Protected` する必要があります。 コントロールのアクセスレベルを変更するには、 **[プロパティ]** ウィンドウで `Modifiers` プロパティを設定します。

## <a name="see-also"></a>参照

- [Windows フォーム コントロール](index.md)
- [AutoSize プロパティの概要](autosize-property-overview.md)
- [方法: Windows フォーム上のコントロールをドッキングする](how-to-dock-controls-on-windows-forms.md)
- [チュートリアル: FlowLayoutPanel を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-a-flowlayoutpanel.md)
- [チュートリアル: TableLayoutPanel を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel.md)
- [チュートリアル: Padding、Margin、および AutoSize プロパティを使用した Windows フォーム コントロールのレイアウト](windows-forms-controls-padding-autosize.md)
