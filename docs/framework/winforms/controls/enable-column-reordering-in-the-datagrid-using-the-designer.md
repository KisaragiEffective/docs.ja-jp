---
title: デザイナーを使用して DataGridView コントロールの列の並べ替えを有効にする
ms.date: 03/30/2017
helpviewer_keywords:
- DataGridView control [Windows Forms], column order
- Windows Forms, columns
- columns [Windows Forms], reordering
- data [Windows Forms], displaying
ms.assetid: d82bd69c-6799-4439-a32c-91139c5901d1
ms.openlocfilehash: 14dc1081a8608c6e6add67f641c4b55825d2fc81
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77626972"
---
# <a name="how-to-enable-column-reordering-in-the-windows-forms-datagridview-control-using-the-designer"></a>方法 : デザイナーを使用して Windows フォーム DataGridView コントロールの列の並べ替えを有効にする
Windows フォーム <xref:System.Windows.Forms.DataGridView> コントロールに表示されるデータを表示する場合、ユーザーは特定の列の値を比較することが必要になることがあります。 これは、列がコントロール内で広く使用されている場合には不便です。特に、ユーザーが関心のあるすべての列を表示するために水平方向にスクロールする必要がある場合に便利です。 ユーザーが列の順序を変更できるようにすることで、列の値を簡単に比較できるようになります。 列の並べ替えを有効にすると、ユーザーは列ヘッダーをマウスでドラッグすることで、列を新しい位置に移動できます。

 次の手順では、<xref:System.Windows.Forms.DataGridView> コントロールを含むフォームを含む**Windows アプリケーション**プロジェクトが必要です。 このようなプロジェクトの設定の詳細については、「[方法: Windows フォームアプリケーションプロジェクトを作成](/visualstudio/ide/step-1-create-a-windows-forms-application-project)する」および「[方法: Windows フォームにコントロールを追加](how-to-add-controls-to-windows-forms.md)する」を参照してください。

## <a name="to-enable-column-reordering"></a>列の並べ替えを有効にするには

- <xref:System.Windows.Forms.DataGridView> コントロールの右上隅にある [デザイナーアクション] グリフ (![小さい黒い矢印](./media/designer-actions-glyph.gif)) をクリックし、[列の**並べ替えを有効にする**] を選択します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.AllowUserToOrderColumns%2A?displayProperty=nameWithType>
- [方法: デザイナーを使用して Windows フォーム DataGridView コントロールの列を固定する](freeze-columns-in-the-datagrid-using-the-designer.md)
- [方法: Windows フォームアプリケーションプロジェクトを作成する](/visualstudio/ide/step-1-create-a-windows-forms-application-project)
- [方法: Windows フォームにコントロールを追加する](how-to-add-controls-to-windows-forms.md)
