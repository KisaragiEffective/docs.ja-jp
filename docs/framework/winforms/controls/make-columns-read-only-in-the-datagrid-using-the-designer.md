---
title: デザイナーを使用して DataGridView コントロールで列を読み取り専用にする
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Forms, columns
- DataGridView control [Windows Forms], read-only columns
- data [Windows Forms], displaying
- columns [Windows Forms], read-only
ms.assetid: b4ef7a75-ab33-4ee3-b2cf-201530e454e9
ms.openlocfilehash: 2dd3f8bfcad39ca3d530c79a6e6a8170585f7adf
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77627956"
---
# <a name="how-to-make-columns-read-only-in-the-windows-forms-datagridview-control-using-the-designer"></a>方法 : デザイナーを使用して Windows フォームの DataGridView コントロールで列を読み取り専用にする
既定では、ユーザーは Windows フォーム <xref:System.Windows.Forms.DataGridView> コントロールに表示されるテキストおよび数値データを変更できます。 変更を意図していないデータを表示する場合は、データを含む列を読み取り専用にする必要があります。 コントロールを完全に読み取り専用にする方法については、「[方法: デザイナーを使用して Windows フォーム DataGridView コントロールで行の追加と削除を回避](prevent-row-addition-and-deletion-in-the-datagrid-using-the-designer.md)する」を参照してください。

 次の手順では、<xref:System.Windows.Forms.DataGridView> コントロールを含むフォームを含む**Windows アプリケーション**プロジェクトが必要です。 このようなプロジェクトの設定の詳細については、「[方法: Windows フォームアプリケーションプロジェクトを作成](/visualstudio/ide/step-1-create-a-windows-forms-application-project)する」および「[方法: Windows フォームにコントロールを追加](how-to-add-controls-to-windows-forms.md)する」を参照してください。

## <a name="to-make-a-column-read-only-by-using-the-designer"></a>デザイナーを使用して列を読み取り専用にするには

1. <xref:System.Windows.Forms.DataGridView> コントロールの右上隅にある [デザイナーアクション] グリフ (![小さい黒い矢印](./media/designer-actions-glyph.gif)) をクリックし、[列の**編集**] を選択します。

2. **[選択された列]** ボックスの一覧から列を選択します。

3. 列の**プロパティ**グリッドで、[<xref:System.Windows.Forms.DataGridViewColumn.ReadOnly%2A>] プロパティを `true`に設定します。

    > [!NOTE]
    > また、 **[列の追加]** ダイアログボックスの **[読み取り専用]** チェックボックスをオンにすると、列を追加するときに読み取り専用にすることもできます。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewColumn.ReadOnly%2A?displayProperty=nameWithType>
- [方法: デザイナーを使用して Windows フォーム DataGridView コントロールの列を追加および削除する](add-and-remove-columns-in-the-datagrid-using-the-designer.md)
- [方法: デザイナーを使用して Windows フォーム DataGridView コントロールで行が追加および削除されないようにする](prevent-row-addition-and-deletion-in-the-datagrid-using-the-designer.md)
- [方法: Windows フォームアプリケーションプロジェクトを作成する](/visualstudio/ide/step-1-create-a-windows-forms-application-project)
- [方法: Windows フォームにコントロールを追加する](how-to-add-controls-to-windows-forms.md)
