---
title: DataGridView コントロールの列の順序を変更する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- columns [Windows Forms], changing order
- DataGridView control [Windows Forms], column order
- data grids [Windows Forms], changing column order
ms.assetid: 5e9ac3bc-b0f0-48cb-a3d5-b005af905395
ms.openlocfilehash: 2aef196e9544a81f42a563783ce6c357869aa247
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746547"
---
# <a name="how-to-change-the-order-of-columns-in-the-windows-forms-datagridview-control"></a>方法 : Windows フォーム DataGridView コントロールの列の順序を変更する
<xref:System.Windows.Forms.DataGridView> を使用してデータをデータ ソースから表示する場合、データ ソースのスキーマの列が、表示したい順序で表示されないことがあります。 <xref:System.Windows.Forms.DataGridViewColumn.DisplayIndex%2A> クラスの <xref:System.Windows.Forms.DataGridViewColumn> プロパティを使用して、列の表示順序を変更できます。  
  
 次のコード例は、Northwind サンプル データベース内の Customers テーブルにバインドするときに自動的に生成される列のいくつかを再配置します。 <xref:System.Windows.Forms.DataGridView> コントロールをデータベーステーブルにバインドする方法の詳細については、「[方法: データを Windows フォーム DataGridView コントロールにバインド](how-to-bind-data-to-the-windows-forms-datagridview-control.md)する」を参照してください。  
  
 Visual Studio では、このタスクに対するサポートが用意されています。  「[方法: デザイナーを使用して Windows フォーム DataGridView コントロールの列の順序を変更する](change-the-order-of-columns-in-the-datagrid-using-the-designer.md)」も参照してください。  
  
## <a name="example"></a>例  
 [!code-csharp[System.Windows.Forms.DataGridViewMisc#040](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#040)]
 [!code-vb[System.Windows.Forms.DataGridViewMisc#040](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#040)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- Northwind サンプル データベース内の <xref:System.Windows.Forms.DataGridView> テーブルなど、示されている列の名前を持つテーブルにバインドされた、`customersDataGridView` という名前の `Customers` コントロール。  
  
- <xref:System?displayProperty=nameWithType>、<xref:System.Windows.Forms?displayProperty=nameWithType>、<xref:System.Data?displayProperty=nameWithType>、および <xref:System.Xml?displayProperty=nameWithType> の各アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewColumn>
- <xref:System.Windows.Forms.DataGridViewColumn.DisplayIndex%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumn.Visible%2A?displayProperty=nameWithType>
- [Windows フォーム DataGridView コントロールでのデータの表示](displaying-data-in-the-windows-forms-datagridview-control.md)
- [方法: データを Windows フォーム DataGridView コントロールにバインドする](how-to-bind-data-to-the-windows-forms-datagridview-control.md)
