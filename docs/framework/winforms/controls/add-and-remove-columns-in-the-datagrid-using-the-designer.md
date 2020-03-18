---
title: デザイナーを使用して DataGridView コントロールの列を追加および削除する
ms.date: 03/30/2017
f1_keywords:
- vs.DataGridViewAddColumnDialog
helpviewer_keywords:
- DataGridView control [Windows Forms], adding columns
- DataGridView control [Windows Forms], removing columns
ms.assetid: 9e709f35-0a8c-4e7e-b4c4-bacb7a834077
ms.openlocfilehash: 8843b1d30f3e5f31a060e27b41b0105e6584f155
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628606"
---
# <a name="how-to-add-and-remove-columns-in-the-windows-forms-datagridview-control-using-the-designer"></a>方法 : デザイナーを使用して Windows フォーム DataGridView コントロールの列を追加および削除する
データを表示するには、Windows フォーム <xref:System.Windows.Forms.DataGridView> コントロールに列が含まれている必要があります。 コントロールを手動で設定する場合は、自分で列を追加する必要があります。 または、データソースにコントロールをバインドして、列を自動的に生成して設定することもできます。 表示する列数よりも多くの列がデータソースに含まれている場合は、不要な列を削除できます。

 次の手順では、<xref:System.Windows.Forms.DataGridView> コントロールを含むフォームを含む**Windows アプリケーション**プロジェクトが必要です。 このようなプロジェクトの設定の詳細については、「[方法: Windows フォームアプリケーションプロジェクトを作成](/visualstudio/ide/step-1-create-a-windows-forms-application-project)する」および「[方法: Windows フォームにコントロールを追加](how-to-add-controls-to-windows-forms.md)する」を参照してください。

## <a name="to-add-a-column-using-the-designer"></a>デザイナーを使用して列を追加するには

1. <xref:System.Windows.Forms.DataGridView> コントロールの右上隅にある [デザイナーアクション] グリフ (![小さい黒い矢印](./media/designer-actions-glyph.gif)) をクリックし、[列の**追加**] を選択します。

2. **[列の追加]** ダイアログボックスで、データ **[バインド列]** オプションを選択し、データソースから列を選択するか、 **[非バインド列]** オプションを選択して、提供されたフィールドを使用して列を定義します。

3. **[追加]** ボタンをクリックして列を追加すると、既存の列がコントロールの表示領域にまだ表示されていない場合に、その列がデザイナーに表示されます。

    > [!NOTE]
    > **[列の編集]** ダイアログボックスでは、コントロールのスマートタグからアクセスできる列のプロパティを変更できます。

## <a name="to-remove-a-column-using-the-designer"></a>デザイナーを使用して列を削除するには

1. コントロールのスマートタグから **[列の編集]** を選択します。

2. **[選択された列]** ボックスの一覧から列を選択します。

3. 列を削除するには、 **[削除]** ボタンをクリックします。これにより、デザイナーに表示されなくなります。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- [方法: Windows フォームアプリケーションプロジェクトを作成する](/visualstudio/ide/step-1-create-a-windows-forms-application-project)
- [方法: Windows フォームにコントロールを追加する](how-to-add-controls-to-windows-forms.md)
