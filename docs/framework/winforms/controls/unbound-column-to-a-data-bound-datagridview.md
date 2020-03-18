---
title: データバインドされた DataGridView コントロールに非バインド列を追加する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- columns [Windows Forms], unbound data
- data grids [Windows Forms], adding unbound columns
- DataGridView control [Windows Forms], unbound data
ms.assetid: f7bdc4d8-ba8e-421b-ad62-82d936f01372
ms.openlocfilehash: 807bbc121f33c35d70068571e76637c078ecb3da
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76747062"
---
# <a name="how-to-add-an-unbound-column-to-a-data-bound-windows-forms-datagridview-control"></a>方法 : データバインドされた Windows フォーム DataGridView コントロールに非バインド列を追加する
<xref:System.Windows.Forms.DataGridView> コントロールに表示するデータは、通常なんらかのデータ ソースから取得されるデータですが、データ ソース以外からのデータの列を表示する必要が生じることがあります。 この種類の列は、非バインド列と呼ばれます。 非バインド列の形式はさまざまです。 しばしば、データ行の詳細へのアクセスに使用されます。  
  
 次のコード例では、マスター/詳細シナリオを実装するときに、親テーブルの特定の行に関連する子テーブルを表示する、 **[詳細]** ボタンのバインドされていない列を作成する方法を示します。 ボタンのクリックに応答するには、子テーブルを含むフォームを表示する <xref:System.Windows.Forms.DataGridView.CellClick?displayProperty=nameWithType> イベント ハンドラーを実装します。  
  
 Visual Studio では、このタスクに対するサポートが用意されています。  「[方法: デザイナーを使用して Windows フォーム DataGridView コントロールで列を追加および削除する](add-and-remove-columns-in-the-datagrid-using-the-designer.md)」も参照してください。  
  
## <a name="example"></a>例  
 [!code-csharp[System.Windows.Forms.DataGridViewMisc#010](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#010)]
 [!code-vb[System.Windows.Forms.DataGridViewMisc#010](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#010)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- <xref:System.Windows.Forms.DataGridView> という名前の `dataGridView1` コントロール。  
  
- <xref:System?displayProperty=nameWithType> アセンブリおよび <xref:System.Windows.Forms?displayProperty=nameWithType> アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- [Windows フォーム DataGridView コントロールでのデータの表示](displaying-data-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでのデータ表示モード](data-display-modes-in-the-windows-forms-datagridview-control.md)
