---
title: ErrorProvider コンポーネントを使用してデータセット内のエラーを表示する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- errors [Windows Forms], dataset errors
- error messages [Windows Forms], viewing in datasets
- ErrorProvider component [Windows Forms], dataset errors
ms.assetid: cbae023f-d651-4210-bdea-bcc5f037e321
ms.openlocfilehash: 8c2155bf288db89b5d53567738fd399b915d50b6
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745451"
---
# <a name="how-to-view-errors-within-a-dataset-with-the-windows-forms-errorprovider-component"></a>方法 : Windows フォーム ErrorProvider コンポーネントで DataSet 内にエラーを表示する
Windows フォーム <xref:System.Windows.Forms.ErrorProvider> コンポーネントを使用すると、データセットまたはその他のデータソース内の列のエラーを表示できます。 <xref:System.Windows.Forms.ErrorProvider> コンポーネントでフォームにデータエラーを表示するには、コントロールに直接関連付ける必要はありません。 データソースにバインドされると、同じデータソースにバインドされているコントロールの横にエラーアイコンが表示されます。  
  
> [!NOTE]
> 実行時にエラープロバイダーの <xref:System.Windows.Forms.ErrorProvider.DataSource%2A> および <xref:System.Windows.Forms.ErrorProvider.DataMember%2A> プロパティを変更した場合は、<xref:System.Windows.Forms.ErrorProvider.BindToDataAndErrors%2A> メソッドを使用して競合を回避する必要があります。  
  
### <a name="to-display-data-errors"></a>データエラーを表示するには  
  
1. コンポーネントをデータテーブル内の特定の列にバインドします。  
  
    ```vb  
    ' Assumes existence of DataSet1, DataTable1  
    TextBox1.DataBindings.Add("Text", DataSet1, "Customers.Name")  
    ErrorProvider1.DataSource = DataSet1  
    ErrorProvider1.DataMember = "Customers"  
    ```  
  
    ```csharp  
    // Assumes existence of DataSet1, DataTable1  
    textBox1.DataBindings.Add("Text", DataSet1, "Customers.Name");  
    errorProvider1.DataSource = DataSet1;  
    errorProvider1.DataMember = "Customers";  
    ```  
  
2. <xref:System.Windows.Forms.ErrorProvider.ContainerControl%2A> プロパティをフォームに設定します。  
  
    ```vb  
    ErrorProvider1.ContainerControl = Me  
    ```  
  
    ```csharp  
    errorProvider1.ContainerControl = this;  
    ```  
  
3. 現在のレコードの位置を、列エラーを含む行に設定します。  
  
    ```vb  
    DataTable1.Rows(5).SetColumnError("Name", "Bad data in this row.")  
    Me.BindingContext(DataTable1).Position = 5  
    ```  
  
    ```csharp  
    DataTable1.Rows[5].SetColumnError("Name", "Bad data in this row.");  
    this.BindingContext [DataTable1].Position = 5;  
    ```  
  
## <a name="see-also"></a>参照

- [ErrorProvider コンポーネントの概要](errorprovider-component-overview-windows-forms.md)
- [方法: Windows フォーム ErrorProvider コンポーネントを使用してフォーム検証でエラー アイコンを表示する](display-error-icons-for-form-validation-with-wf-errorprovider.md)
