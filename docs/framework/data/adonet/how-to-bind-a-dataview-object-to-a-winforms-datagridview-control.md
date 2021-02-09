---
description: '詳細情報: 方法:DataView オブジェクトを Windows フォーム DataGridView コントロールにバインドする'
title: '方法: DataView オブジェクトを Windows フォーム DataGridView コントロールにバインドする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 2b73d60a-6049-446a-85a7-3e5a68b183e2
ms.openlocfilehash: 59bc1a0f6b7b2b0d6da4fa6d88e06922d95c9f46
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99723908"
---
# <a name="how-to-bind-a-dataview-object-to-a-windows-forms-datagridview-control"></a>方法: DataView オブジェクトを Windows フォーム DataGridView コントロールにバインドする

<xref:System.Windows.Forms.DataGridView> コントロールには、データを表形式で表示するための強力で柔軟な機能が用意されています。 <xref:System.Windows.Forms.DataGridView> コントロールは、標準 Windows フォーム データ バインディング モデルをサポートするため、<xref:System.Data.DataView> およびその他の各種のデータ ソースにバインドします。 ただし、ほとんどの状況では、データ ソースとの対話の詳細を管理する <xref:System.Windows.Forms.BindingSource> コンポーネントにバインドします。  
  
 <xref:System.Windows.Forms.DataGridView> コントロールについて詳しくは、「[DataGridView コントロールの概要](/dotnet/desktop/winforms/controls/datagridview-control-overview-windows-forms)」をご覧ください。  
  
### <a name="to-connect-a-datagridview-control-to-a-dataview"></a>DataGridView コントロールを DataView に接続するには  
  
1. データベースからデータを取得する操作の詳細を処理するメソッドを実装します。 次のコード例では、`GetData` コンポーネントを初期化する <xref:System.Data.SqlClient.SqlDataAdapter> メソッドを実装し、これを使用して <xref:System.Data.DataSet> に値を設定します。 `connectionString` 変数は、使用しているデータベースに合った値に設定してください。 AdventureWorks SQL Server サンプル データベースがインストールされているサーバーにアクセスする必要があります。  
  
     [!code-csharp[DP DataViewWinForms Sample#LDVSample1GetData](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataViewWinForms Sample/CS/Form1.cs#ldvsample1getdata)]
     [!code-vb[DP DataViewWinForms Sample#LDVSample1GetData](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataViewWinForms Sample/VB/Form1.vb#ldvsample1getdata)]  
  
2. フォームの <xref:System.Windows.Forms.Form.Load> イベント ハンドラーで、<xref:System.Windows.Forms.DataGridView> コントロールを <xref:System.Windows.Forms.BindingSource> コンポーネントにバインドし、`GetData` メソッドを呼び出してデータベースからデータを取得します。 <xref:System.Data.DataView> は、Contact <xref:System.Data.DataTable> に対する LINQ to DataSet クエリから作成された後、<xref:System.Windows.Forms.BindingSource> コンポーネントにバインドされます。  
  
     [!code-csharp[DP DataViewWinForms Sample#LDVSample1FormLoad](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataViewWinForms Sample/CS/Form1.cs#ldvsample1formload)]
     [!code-vb[DP DataViewWinForms Sample#LDVSample1FormLoad](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataViewWinForms Sample/VB/Form1.vb#ldvsample1formload)]  
  
## <a name="see-also"></a>関連項目

- [データ バインディングと LINQ to DataSet](data-binding-and-linq-to-dataset.md)
