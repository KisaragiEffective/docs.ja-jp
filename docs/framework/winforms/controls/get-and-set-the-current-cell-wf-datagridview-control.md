---
title: DataGridView コントロールの現在のセルを取得および設定します。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGridView control [Windows Forms], getting current cell
- DataGridView control [Windows Forms], setting current cell
- cells [Windows Forms], getting and setting current
ms.assetid: b0e41e57-493a-4bd0-9376-a6f76723540c
ms.openlocfilehash: 0fb01d5392e02b53e0e5bf905c872dbeeb22fad9
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745662"
---
# <a name="how-to-get-and-set-the-current-cell-in-the-windows-forms-datagridview-control"></a>方法 : Windows フォーム DataGridView コントロールの現在のセルを取得および設定する
多くの場合、<xref:System.Windows.Forms.DataGridView> とのやり取りでは、どのセルが現在アクティブであるかをプログラムで検出する必要があります。 また、現在のセルの変更が必要になる場合もあります。 これらのタスクは、<xref:System.Windows.Forms.DataGridView.CurrentCell%2A> プロパティを使用して実行できます。  
  
> [!NOTE]
> <xref:System.Windows.Forms.DataGridViewBand.Visible%2A> プロパティが `false`に設定されている行または列の現在のセルを設定することはできません。  
  
 <xref:System.Windows.Forms.DataGridView> コントロールの選択モードによっては、現在のセルを変更すると選択内容を変更できます。 詳細については、「 [Windows フォーム DataGridView コントロールの選択モード](selection-modes-in-the-windows-forms-datagridview-control.md)」を参照してください。  
  
### <a name="to-get-the-current-cell-programmatically"></a>現在のセルをプログラムで取得するには  
  
- <xref:System.Windows.Forms.DataGridView> コントロールの <xref:System.Windows.Forms.DataGridView.CurrentCell%2A> プロパティを使用します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#080](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#080)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#080](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#080)]  
  
### <a name="to-set-the-current-cell-programmatically"></a>現在のセルをプログラムによって設定するには  
  
- <xref:System.Windows.Forms.DataGridView> コントロールの <xref:System.Windows.Forms.DataGridView.CurrentCell%2A> プロパティを設定します。 次のコード例では、現在のセルは行0、列1に設定されています。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#085](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#085)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#085](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#085)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- `getCurrentCellButton` と `setCurrentCellButton`という名前のコントロールを <xref:System.Windows.Forms.Button> します。 ビジュアルC#では、各ボタンの <xref:System.Windows.Forms.Control.Click> イベントを、コード例の関連付けられたイベントハンドラーにアタッチする必要があります。  
  
- <xref:System.Windows.Forms.DataGridView> という名前の `dataGridView1` コントロール。  
  
- <xref:System?displayProperty=nameWithType> アセンブリおよび <xref:System.Windows.Forms?displayProperty=nameWithType> アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.CurrentCell%2A?displayProperty=nameWithType>
- [Windows フォーム DataGridView コントロールでの列、行、およびセルの基本機能](basic-column-row-and-cell-features-wf-datagridview-control.md)
- [Windows フォーム DataGridView コントロールの選択モード](selection-modes-in-the-windows-forms-datagridview-control.md)
