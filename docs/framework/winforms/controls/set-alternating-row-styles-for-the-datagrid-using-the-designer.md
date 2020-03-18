---
title: デザイナーを使用して DataGridView コントロールに交互の行のスタイルを設定する
ms.date: 03/30/2017
helpviewer_keywords:
- ledger-like formats
- DataGridView control [Windows Forms], row style alternation
- Windows Forms, rows
- rows [Windows Forms], alternating
- data [Windows Forms], displaying
ms.assetid: 02373442-bf94-4470-9f8a-e44c4a9d5b88
ms.openlocfilehash: 74f65d03a359136de943f8a2937ed5e5f1e62519
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743048"
---
# <a name="how-to-set-alternating-row-styles-for-the-windows-forms-datagridview-control-using-the-designer"></a>方法 : デザイナーを使用して Windows フォーム DataGridView コントロールに交互の行のスタイルを設定する

多くの場合、表形式のデータは、交互の行が異なる背景色を持つ、台帳に似た形式で表示されます。 この形式を使用すると、多数の列がある幅の広いテーブルで、ユーザーが各行にあるセルを簡単に識別できるようになります。

<xref:System.Windows.Forms.DataGridView> コントロールを使用すると、1 行おきの完全なスタイル情報を指定できます。 背景色に加えて、前景色やフォントなどのスタイル特性を使用して、交互の行を区別することができます。 詳細については、「 [Windows フォーム DataGridView コントロールのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)」を参照してください。

次の手順では、<xref:System.Windows.Forms.DataGridView> コントロールを含むフォームを含む**Windows アプリケーション**プロジェクトが必要です。 このようなプロジェクトの設定の詳細については、「[方法: Windows フォームアプリケーションプロジェクトを作成](/visualstudio/ide/step-1-create-a-windows-forms-application-project)する」および「[方法: Windows フォームにコントロールを追加](how-to-add-controls-to-windows-forms.md)する」を参照してください。

### <a name="define-styles-for-alternating-rows"></a>交互の行のスタイルを定義する

1. デザイナーで <xref:System.Windows.Forms.DataGridView> コントロールを選択します。

2. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.DataGridView.AlternatingRowsDefaultCellStyle%2A>] プロパティの横にある省略記号ボタン ([...]) をクリックし、[Visual Studio のプロパティウィンドウの](./media/visual-studio-ellipsis-button.png))] を![ます。

3. **[CellStyle ビルダー]** ダイアログボックスで、プロパティを設定してスタイルを定義し、**プレビュー**ウィンドウを使用して選択内容を確認します。 指定したスタイルは、コントロールに表示される他のすべての行に使用され、2番目のスタイルから始まります。

4. 残りの行のスタイルを定義するには、<xref:System.Windows.Forms.DataGridView.RowsDefaultCellStyle%2A> プロパティを使用して手順 2. と 3. を繰り返します。

    > [!NOTE]
    > 複数のプロパティから継承されたスタイルを使用して、セルが表示されます。 スタイルの継承の詳細については、「 [Windows フォーム DataGridView コントロールのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)」を参照してください。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- [Windows フォーム DataGridView コントロールでのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)
- [Windows フォームの DataGridView コントロールの基本的な書式設定およびスタイル設定](basic-formatting-and-styling-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでのデザイナーの使用](using-the-designer-with-the-windows-forms-datagridview-control.md)
- [方法: Windows フォームアプリケーションプロジェクトを作成する](/visualstudio/ide/step-1-create-a-windows-forms-application-project)
- [方法: Windows フォームにコントロールを追加する](how-to-add-controls-to-windows-forms.md)
