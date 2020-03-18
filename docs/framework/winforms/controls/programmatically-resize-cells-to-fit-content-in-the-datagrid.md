---
title: DataGridView コントロールの内容に合わせてセルのサイズをプログラムによって変更する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- data grids [Windows Forms], resizing cells to fit content
- cells [Windows Forms], resizing to fit contents
- DataGridView control [Windows Forms], resizing cells
- grids [Windows Forms], resizing cells to fit content
ms.assetid: 63d770dc-b3f5-462b-901a-3125b2753792
ms.openlocfilehash: df3b378a8ba358fa0bfe549a7901b3d59d53f556
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742453"
---
# <a name="how-to-programmatically-resize-cells-to-fit-content-in-the-windows-forms-datagridview-control"></a>方法 : Windows フォームの DataGridView コントロールの内容に合わせてセルのサイズをプログラムで変更する
<xref:System.Windows.Forms.DataGridView> コントロールのさまざまなメソッドを使用すると、行、列、およびヘッダーのサイズを変更して、切り捨てることなく値の全体を表示することができます。 これらのメソッドを使用して、選択時に <xref:System.Windows.Forms.DataGridView> 要素のサイズを変更できます。 代わりに、コンテンツが変更されるたびに、これらの要素のサイズを自動的に変更するコントロールを構成することができます。 ただしこれは、大規模なデータ セットを処理しているときは、データが頻繁に変更されるときは、効率的ではありません。 詳細については、「 [Windows フォーム DataGridView コントロールのサイズ変更オプション](sizing-options-in-the-windows-forms-datagridview-control.md)」を参照してください。  
  
 通常はデータ ソースから新しいデータを読み込むときや、ユーザーが値を編集するときにのみ、内容に合わせて <xref:System.Windows.Forms.DataGridView> 要素をプログラムで調整します。 これは、パフォーマンスを最適化するために便利ですが、ユーザーがマウスを使って行および列のサイズを手動で変更できるようにする場合にも便利です。  
  
 次のコード例は、プログラムでのサイズ変更に使用できるオプションを示します。  
  
## <a name="example"></a>例  
 [!code-cpp[System.Windows.Forms.DataGridView.ProgrammaticResizing#0](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.ProgrammaticResizing/CPP/programmaticsizing.cpp#0)]
 [!code-csharp[System.Windows.Forms.DataGridView.ProgrammaticResizing#0](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.ProgrammaticResizing/CS/programmaticsizing.cs#0)]
 [!code-vb[System.Windows.Forms.DataGridView.ProgrammaticResizing#0](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.ProgrammaticResizing/VB/programmaticsizing.vb#0)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- System、System.Drawing、および System.Windows.Forms の各アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.AutoResizeColumn%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AutoResizeColumns%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AutoResizeColumnHeadersHeight%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AutoResizeRow%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AutoResizeRows%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AutoResizeRowHeadersWidth%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewAutoSizeRowMode>
- <xref:System.Windows.Forms.DataGridViewAutoSizeRowsMode>
- <xref:System.Windows.Forms.DataGridViewAutoSizeColumnMode>
- <xref:System.Windows.Forms.DataGridViewAutoSizeColumnsMode>
- <xref:System.Windows.Forms.DataGridViewColumnHeadersHeightSizeMode>
- <xref:System.Windows.Forms.DataGridViewRowHeadersWidthSizeMode>
- [Windows フォーム DataGridView コントロール内の列と行のサイズ変更](resizing-columns-and-rows-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールのサイズ変更オプション](sizing-options-in-the-windows-forms-datagridview-control.md)
- [方法: Windows フォームの DataGridView コントロールの内容変更時にセルのサイズを自動的に変更する](automatically-resize-cells-when-content-changes-in-the-datagrid.md)
