---
title: データソースへの DataGrid コントロールのバインド
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- datasets [Windows Forms], binding to DataGrid control
- data binding [Windows Forms], DataGrid control
- DataGrid control [Windows Forms], data binding
- bound controls [Windows Forms], DataGrid control
- Windows Forms controls, data binding
- bound controls [Windows Forms]
- data-bound controls [Windows Forms], DataGrid
ms.assetid: 128cdb07-dfd3-4d60-9d6a-902847667c36
ms.openlocfilehash: 2634a6bd8ace36bcf7a49120162474a8c04b2b83
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746691"
---
# <a name="how-to-bind-the-windows-forms-datagrid-control-to-a-data-source"></a>方法 : データ ソースに Windows フォーム DataGrid コントロールをバインドする
> [!NOTE]
> <xref:System.Windows.Forms.DataGridView> コントロールは、<xref:System.Windows.Forms.DataGrid> コントロールに代わると共に追加の機能を提供します。ただし、<xref:System.Windows.Forms.DataGrid> コントロールは、下位互換性を保つ目的および将来使用する目的で保持されます。 詳細については、「[Windows フォームの DataGridView コントロールと DataGrid コントロールの違いについて](differences-between-the-windows-forms-datagridview-and-datagrid-controls.md)」を参照してください。  
  
 Windows フォーム <xref:System.Windows.Forms.DataGrid> コントロールは、特にデータソースからの情報を表示するように設計されています。 実行時にコントロールをバインドするには、<xref:System.Windows.Forms.DataGrid.SetDataBinding%2A> メソッドを呼び出します。 さまざまなデータソースのデータを表示できますが、最も一般的なソースはデータセットとデータビューです。  
  
### <a name="to-data-bind-the-datagrid-control-programmatically"></a>プログラムによって DataGrid コントロールをデータバインドするには  
  
1. データセットを埋めるコードを記述します。  
  
     データソースがデータセットまたはデータセットテーブルに基づくデータビューの場合は、データセットを埋めるためのコードをフォームに追加します。  
  
     実際に使用するコードは、データセットがデータを取得している場所によって異なります。 データセットがデータベースから直接作成されている場合は、通常、次の例に示すように、データアダプターの `Fill` メソッドを呼び出します。この例では、`DsCategories1`という名前のデータセットを設定します。  
  
    ```vb  
    sqlDataAdapter1.Fill(DsCategories1)  
    ```  
  
    ```csharp  
    sqlDataAdapter1.Fill(DsCategories1);  
    ```  
  
    ```cpp  
    sqlDataAdapter1->Fill(dsCategories1);  
    ```  
  
     データセットが XML Web サービスから読み込まれている場合は、通常、コード内にサービスのインスタンスを作成し、そのメソッドの1つを呼び出してデータセットを返します。 次に、データセットを XML Web サービスからローカルデータセットにマージします。 次の例は、`CategoriesService`という名前の XML Web サービスのインスタンスを作成し、その `GetCategories` メソッドを呼び出し、結果のデータセットを `DsCategories1`という名前のローカルデータセットにマージする方法を示しています。  
  
    ```vb  
    Dim ws As New MyProject.localhost.CategoriesService()  
    ws.Credentials = System.Net.CredentialCache.DefaultCredentials  
    DsCategories1.Merge(ws.GetCategories())  
    ```  
  
    ```csharp  
    MyProject.localhost.CategoriesService ws = new MyProject.localhost.CategoriesService();  
    ws.Credentials = System.Net.CredentialCache.DefaultCredentials;  
    DsCategories1.Merge(ws.GetCategories());  
    ```  
  
    ```cpp  
    MyProject::localhost::CategoriesService^ ws =   
       new MyProject::localhost::CategoriesService();  
    ws->Credentials = System::Net::CredentialCache::DefaultCredentials;  
    dsCategories1->Merge(ws->GetCategories());  
    ```  
  
2. <xref:System.Windows.Forms.DataGrid> コントロールの <xref:System.Windows.Forms.DataGrid.SetDataBinding%2A> メソッドを呼び出して、データソースとデータメンバーを渡します。 データメンバーを明示的に渡す必要がない場合は、空の文字列を渡します。  
  
    > [!NOTE]
    > グリッドを初めてバインドする場合は、コントロールの <xref:System.Windows.Forms.DataGrid.DataSource%2A> と <xref:System.Windows.Forms.DataGrid.DataMember%2A> のプロパティを設定できます。 ただし、これらのプロパティがいったん設定されると、これらのプロパティをリセットすることはできません。 したがって、常に <xref:System.Windows.Forms.DataGrid.SetDataBinding%2A> メソッドを使用することをお勧めします。  
  
     次の例では、`DsCustomers1`というデータセットの Customers テーブルにプログラムでバインドする方法を示します。  
  
    ```vb  
    DataGrid1.SetDataBinding(DsCustomers1, "Customers")  
    ```  
  
    ```csharp  
    DataGrid1.SetDataBinding(DsCustomers1, "Customers");  
    ```  
  
    ```cpp  
    dataGrid1->SetDataBinding(dsCustomers1, "Customers");  
    ```  
  
     Customers テーブルがデータセット内の唯一のテーブルである場合は、次の方法でグリッドをバインドすることもできます。  
  
    ```vb  
    DataGrid1.SetDataBinding(DsCustomers1, "")  
    ```  
  
    ```csharp  
    DataGrid1.SetDataBinding(DsCustomers1, "");  
    ```  
  
    ```cpp  
    dataGrid1->SetDataBinding(dsCustomers1, "");  
    ```  
  
3. Optional適切なテーブルスタイルと列スタイルをグリッドに追加します。 テーブルスタイルが存在しない場合は、テーブルが表示されますが、すべての列が表示されます。  
  
## <a name="see-also"></a>参照

- [DataGrid コントロールの概要](datagrid-control-overview-windows-forms.md)
- [方法: Windows フォーム DataGrid コントロールにテーブルと列を追加する](how-to-add-tables-and-columns-to-the-windows-forms-datagrid-control.md)
- [DataGrid コントロール](datagrid-control-windows-forms.md)
- [Windows フォームでのデータ バインディング](../windows-forms-data-binding.md)
