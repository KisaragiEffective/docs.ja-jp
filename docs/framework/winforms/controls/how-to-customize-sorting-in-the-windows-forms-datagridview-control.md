---
title: DataGridView コントロールの並べ替えをカスタマイズする
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- sorting [Windows Forms], DataGridView control
- DataGridView control [Windows Forms], sorting
- data grids [Windows Forms], customizing sorting
ms.assetid: 92fb5c14-afab-4cf5-a97e-924fd9cb99f5
ms.openlocfilehash: 20f581b2df6fd172a0a1998aed60c56b0306f2eb
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743183"
---
# <a name="how-to-customize-sorting-in-the-windows-forms-datagridview-control"></a>方法 : Windows フォーム DataGridView コントロールの並べ替え機能をカスタマイズする
<xref:System.Windows.Forms.DataGridView> コントロールは、自動並べ替え機能を提供しますが、ニーズに応じて、並べ替え操作をカスタマイズすることが必要な場合があります。 たとえば、プログラムによる並べ替え機能を使用して、代替のユーザー インターフェイス (UI) を作成することができます。 また、複数の列の並べ替えなど、並べ替え柔軟性を高めるために、<xref:System.Windows.Forms.DataGridView.SortCompare> イベントを処理したり、`Sort(IComparer)` メソッドの <xref:System.Windows.Forms.DataGridView.Sort%2A> のオーバーロードを呼び出したりすることができます。  
  
 次のコード例は、カスタムの並べ替えの 3 つの方法を示します。 詳細については、「[Windows フォーム DataGridView コントロール内の列の並べ替えモード](column-sort-modes-in-the-windows-forms-datagridview-control.md)」を参照してください。  
  
## <a name="programmatic-sorting"></a>プログラムによる並べ替え  
 次のコード例は、<xref:System.Windows.Forms.DataGridView.SortOrder%2A> プロパティと <xref:System.Windows.Forms.DataGridView.SortedColumn%2A> プロパティを使用して並べ替えの方向を判断し、<xref:System.Windows.Forms.DataGridViewColumnHeaderCell.SortGlyphDirection%2A> プロパティを使用してナラベカエグリフを手動で設定する、プログラムによる並べ替えを示しています。 `Sort(DataGridViewColumn,ListSortDirection)` メソッドの <xref:System.Windows.Forms.DataGridView.Sort%2A> のオーバーロードは 1 つの列のみでのデータの並べ替えに使用します。  
  
 [!code-csharp[System.Windows.Forms.DataGridViewProgrammaticSort#00](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewProgrammaticSort/CS/form1.cs#00)]
 [!code-vb[System.Windows.Forms.DataGridViewProgrammaticSort#00](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewProgrammaticSort/VB/form1.vb#00)]  
  
## <a name="custom-sorting-using-the-sortcompare-event"></a>SortCompare イベントを使用したカスタムの並べ替え  
 次のコード例は、<xref:System.Windows.Forms.DataGridView.SortCompare> イベント ハンドラーを使用したカスタムの並べ替えを示しています。 選択した <xref:System.Windows.Forms.DataGridViewColumn> が並べ替えられて、列に重複する値がある場合は、最終的な順序を決定する、ID 列が使用されます。  
  
 [!code-csharp[System.Windows.Forms.DataGridView.SortCompare#00](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.SortCompare/CS/form1.cs#00)]
 [!code-vb[System.Windows.Forms.DataGridView.SortCompare#00](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.SortCompare/VB/form1.vb#00)]  
  
## <a name="custom-sorting-using-the-icomparer-interface"></a>IComparer インターフェイスを使用したカスタムの並べ替え  
 次のコード例は、複数列の並べ替えを実行する `Sort(IComparer)` インターフェイスの実装を取得する <xref:System.Windows.Forms.DataGridView.Sort%2A> メソッドの <xref:System.Collections.IComparer> のオーバーロードを使用したカスタムの並べ替えを示しています。  
  
 [!code-csharp[System.Windows.Forms.DataGridViewIComparerSort#00](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewIComparerSort/CS/form1.cs#00)]
 [!code-vb[System.Windows.Forms.DataGridViewIComparerSort#00](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewIComparerSort/VB/form1.vb#00)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 これらの例には以下のものが必要です。  
  
- System、System.Drawing、および System.Windows.Forms の各アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- [Windows フォームの DataGridView コントロールでのデータの並べ替え](sorting-data-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロール内の列の並べ替えモード](column-sort-modes-in-the-windows-forms-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロール内の列の並べ替えモードを設定する](set-the-sort-modes-for-columns-wf-datagridview-control.md)
