---
title: DataGrid コントロールを使用してマスター/詳細リストを作成する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- master-details lists
- DataGrid control [Windows Forms], master-details lists
- related tables [Windows Forms], displaying in DataGrid control
ms.assetid: 20388c6a-94f9-4d96-be18-8c200491247f
ms.openlocfilehash: e8ab63d52d97a8a6e6f60da741186e3937350e1e
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76730990"
---
# <a name="how-to-create-masterdetail-lists-with-the-windows-forms-datagrid-control"></a>方法 : Windows フォーム DataGrid コントロールを使用してマスター/詳細リストを作成する
> [!NOTE]
> <xref:System.Windows.Forms.DataGridView> コントロールは、<xref:System.Windows.Forms.DataGrid> コントロールに代わると共に追加の機能を提供します。ただし、<xref:System.Windows.Forms.DataGrid> コントロールは、下位互換性を保つ目的および将来使用する目的で保持されます。 詳細については、「[Windows フォームの DataGridView コントロールと DataGrid コントロールの違いについて](differences-between-the-windows-forms-datagridview-and-datagrid-controls.md)」を参照してください。  
  
 <xref:System.Data.DataSet> に一連の関連テーブルが含まれている場合は、2つの <xref:System.Windows.Forms.DataGrid> コントロールを使用して、マスター/詳細形式でデータを表示できます。 1つの <xref:System.Windows.Forms.DataGrid> はマスターグリッドとして指定され、2つ目は詳細グリッドとして指定されます。 マスターリストでエントリを選択すると、関連するすべての子エントリが詳細一覧に表示されます。 たとえば、<xref:System.Data.DataSet> に Customers テーブルと関連する Orders テーブルが含まれている場合は、Customers テーブルをマスターグリッドに指定し、Orders テーブルを詳細グリッドとして指定します。 マスターグリッドから顧客を選択すると、Orders テーブル内のその顧客に関連付けられているすべての注文が詳細グリッドに表示されます。  
  
### <a name="to-set-a-masterdetail-relationship-programmatically"></a>マスター/詳細関係をプログラムによって設定するには  
  
1. 2つの新しい <xref:System.Windows.Forms.DataGrid> コントロールを作成し、それらのプロパティを設定します。  
  
2. テーブルをデータセットに追加します。  
  
3. 作成するリレーションシップを表す <xref:System.Data.DataRelation> 型の変数を宣言します。  
  
4. リレーションシップの名前を指定し、2つのテーブルを関連付けるテーブル、列、およびアイテムを指定して、リレーションシップをインスタンス化します。  
  
5. <xref:System.Data.DataSet> オブジェクトの <xref:System.Data.DataSet.Relations%2A> コレクションにリレーションシップを追加します。  
  
6. <xref:System.Windows.Forms.DataGrid> の <xref:System.Windows.Forms.DataGrid.SetDataBinding%2A> メソッドを使用して、各グリッドを <xref:System.Data.DataSet>にバインドします。  
  
     次の例では、以前に生成された <xref:System.Data.DataSet> (`ds`) の Customers テーブルと Orders テーブルの間にマスター/詳細リレーションシップを設定する方法を示します。  
  
    ```vb  
    Dim myDataRelation As DataRelation  
    myDataRelation = New DataRelation _  
       ("CustOrd", ds.Tables("Customers").Columns("CustomerID"), _  
       ds.Tables("Orders").Columns("CustomerID"))  
    ' Add the relation to the DataSet.  
    ds.Relations.Add(myDataRelation)  
    GridOrders.SetDataBinding(ds, "Customers")  
    GridDetails.SetDataBinding(ds, "Customers.CustOrd")  
    ```  
  
    ```csharp  
    DataRelation myDataRelation;  
    myDataRelation = new DataRelation("CustOrd", ds.Tables["Customers"].Columns["CustomerID"], ds.Tables["Orders"].Columns["CustomerID"]);  
    // Add the relation to the DataSet.  
    ds.Relations.Add(myDataRelation);  
    GridOrders.SetDataBinding(ds,"Customers");  
    GridDetails.SetDataBinding(ds,"Customers.CustOrd");  
    ```  
  
    ```cpp  
    DataRelation^ myDataRelation;  
    myDataRelation = gcnew DataRelation("CustOrd",  
       ds->Tables["Customers"]->Columns["CustomerID"],  
       ds->Tables["Orders"]->Columns["CustomerID"]);  
    // Add the relation to the DataSet.  
    ds->Relations->Add(myDataRelation);  
    GridOrders->SetDataBinding(ds, "Customers");  
    GridDetails->SetDataBinding(ds, "Customers.CustOrd");  
    ```  
  
## <a name="see-also"></a>参照

- [DataGrid コントロール](datagrid-control-windows-forms.md)
- [DataGrid コントロールの概要](datagrid-control-overview-windows-forms.md)
- [方法: データ ソースに Windows フォーム DataGrid コントロールをバインドする](how-to-bind-the-windows-forms-datagrid-control-to-a-data-source.md)
