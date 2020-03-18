---
title: デザイナーを使用して ListView コントロールに列を追加する
ms.date: 03/30/2017
helpviewer_keywords:
- ListView control [Windows Forms], adding column headers
- columns [Windows Forms], adding to ListView controls
ms.assetid: 5b1a8b4d-587e-479a-95c1-f9b90884f13a
ms.openlocfilehash: 627f8627aac861877a05c13def07427199807754
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744609"
---
# <a name="how-to-add-columns-to-the-windows-forms-listview-control-using-the-designer"></a>方法 :デザイナーを使って Windows フォーム ListView コントロールに列を追加する

Windows フォーム <xref:System.Windows.Forms.ListView> コントロールでは、**詳細**ビューで各リスト項目に対して複数の列を表示できます。 列を使用して、各リスト項目に関するいくつかの種類の情報を表示できます。 たとえば、ファイルの一覧には、ファイル名、ファイルの種類、サイズ、ファイルが最後に変更された日付が表示されます。 作成後に列を設定する方法の詳細については、「[方法: Windows フォーム ListView コントロールを使用して列にサブ項目を表示する](how-to-display-subitems-in-columns-with-the-windows-forms-listview-control.md)」を参照してください。

次の手順では、<xref:System.Windows.Forms.ListView> コントロールを含むフォームを含む**Windows アプリケーション**プロジェクトが必要です。 このようなプロジェクトの設定の詳細については、「[方法: Windows フォームアプリケーションプロジェクトを作成](/visualstudio/ide/step-1-create-a-windows-forms-application-project)する」および「[方法: Windows フォームにコントロールを追加](how-to-add-controls-to-windows-forms.md)する」を参照してください。

### <a name="to-add-columns-in-the-designer"></a>デザイナーで列を追加するには

1. **[プロパティ]** ウィンドウで、コントロールの <xref:System.Windows.Forms.ListView.View%2A> プロパティを <xref:System.Windows.Forms.View.Details>に設定します。

2. **[プロパティ]** ウィンドウで、[ **<xref:System.Windows.Forms.ListView.Columns%2A>] プロパティ**の横にある省略記号ボタン ([...]) をクリックし、[Visual Studio のプロパティウィンドウの](./media/visual-studio-ellipsis-button.png))] を![ます。

     **Columnheader コレクションエディター**が表示されます。

3. 新しい列を追加するには、 **[追加]** ボタンを使用します。 次に、列ヘッダーを選択し、そのテキスト (列のキャプション)、テキストの配置、および幅を設定できます。

## <a name="see-also"></a>参照

- [ListView コントロールの概要](listview-control-overview-windows-forms.md)
- [方法: Windows フォーム ListView コントロールで項目を追加および削除する](how-to-add-and-remove-items-with-the-windows-forms-listview-control.md)
- [方法: Windows フォーム ListView コントロールの列にサブ項目を表示する](how-to-display-subitems-in-columns-with-the-windows-forms-listview-control.md)
- [方法: Windows フォーム ListView コントロールのアイコンを表示する](how-to-display-icons-for-the-windows-forms-listview-control.md)
- [方法: TreeView コントロールまたは ListView コントロール (Windows フォーム) にカスタム情報を追加する](add-custom-information-to-a-treeview-or-listview-control-wf.md)
