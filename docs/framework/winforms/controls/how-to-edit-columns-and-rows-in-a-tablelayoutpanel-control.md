---
title: '方法 : TableLayoutPanel コントロールの列と行を編集する'
ms.date: 03/30/2017
f1_keywords:
- net.ComponentModel.StyleCollectionEditor
helpviewer_keywords:
- columns [Windows Forms], editing
- TableLayoutPanel control [Windows Forms], editing
- rows [Windows Forms], editing
ms.assetid: c367ed43-40dc-49eb-9e0f-ba70e83dfec0
ms.openlocfilehash: 4473b20eea57088104a51eb1b6c080219223d214
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628645"
---
# <a name="how-to-edit-columns-and-rows-in-a-tablelayoutpanel-control"></a>方法 : TableLayoutPanel コントロールの列と行を編集する

**[列と行のスタイル]** ダイアログボックスと呼ばれる <xref:System.Windows.Forms.TableLayoutPanel> コントロールのコレクションエディターを使用して、コントロールの行と列を編集できます。

> [!NOTE]
> コントロールが複数の行または列にまたがるようにするには、コントロールの `RowSpan` と `ColumnSpan` のプロパティを設定します。 詳細については、「 [Walkthrough: Arranging Controls on Windows Forms Using a TableLayoutPanel](walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel.md)」を参照してください。
>
> セル内でコントロールを配置する場合、またはコントロールをセル内で伸縮する場合は、コントロールの <xref:System.Windows.Forms.Control.Anchor%2A> プロパティを使用します。 詳細については、「 [Walkthrough: Arranging Controls on Windows Forms Using a TableLayoutPanel](walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel.md)」を参照してください。

## <a name="to-edit-rows-and-columns"></a>行と列を編集するには

1. <xref:System.Windows.Forms.TableLayoutPanel> ツールボックス **から** コントロールをフォームにドラッグします。

2. <xref:System.Windows.Forms.TableLayoutPanel> コントロールのデザイナーアクショングリフ (![小さい黒い矢印](./media/designer-actions-glyph.gif)) をクリックし、 **[行と列の編集]** を選択して **[列と行のスタイル]** ダイアログボックスを開きます。 <xref:System.Windows.Forms.TableLayoutPanel> コントロールを右クリックし、ショートカットメニューの **[行と列の編集]** をクリックすることもできます。

3. 列を追加または削除するには、 **[メンバーの種類]** ボックスの一覧から**列**を選択します。

4. 行を追加または削除するには、 **[メンバーの種類]** ボックスの一覧から **[行]** を選択します。

5. **メンバー**リストの末尾に行または列を追加するには、 **[追加]** ボタンをクリックします。

6. 一覧で現在選択されている項目の前に行または列を追加するには、 **[挿入]** ボタンをクリックします。

7. 行または列を追加する場合は、新しい行または列の**サイズの種類**を選択します。 詳細については、<xref:System.Windows.Forms.SizeType> を参照してください。

8. 行または列を削除するには、 **[削除]** ボタンをクリックして、**メンバー**リストで現在選択されているアイテムを削除します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.SizeType>
- [TableLayoutPanel コントロール](tablelayoutpanel-control-windows-forms.md)
