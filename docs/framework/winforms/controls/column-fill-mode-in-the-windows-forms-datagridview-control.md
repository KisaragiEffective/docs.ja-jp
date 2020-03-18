---
title: DataGridView コントロールの列の塗りつぶしモード
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data grids [Windows Forms], automatically resizing columns
- DataGridView control [Windows Forms], column fill mode
- data grids [Windows Forms], column fill mode
ms.assetid: b4ef7411-ebf4-4e26-bb33-aecec90de80c
ms.openlocfilehash: 43b8915efe303b6f56cd4adf5fdbd69f51b0b754
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736873"
---
# <a name="column-fill-mode-in-the-windows-forms-datagridview-control"></a>Windows フォーム DataGridView コントロールの列フィル モード
列フィル モードでは、<xref:System.Windows.Forms.DataGridView> コントロールの列は、コントロールの表示領域の幅を満たすように自動的にサイズ変更されます。 すべての列の幅を <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A> プロパティの値以上にするために水平スクロール バーが必要な場合を除き、コントロールに水平スクロール バーは表示されません。  
  
 各列のサイズ変更動作は、<xref:System.Windows.Forms.DataGridViewColumn.InheritedAutoSizeMode%2A> プロパティによって異なります。 このプロパティの値は、列の <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A> プロパティから継承されるか、または列の値が既定値の <xref:System.Windows.Forms.DataGridView.AutoSizeColumnsMode%2A> である場合はコントロールの <xref:System.Windows.Forms.DataGridViewAutoSizeColumnMode.NotSet> プロパティから継承されます。  
  
 列ごとに異なるサイズ変更モードを使用できます。ただし、サイズ変更モードが <xref:System.Windows.Forms.DataGridViewAutoSizeColumnMode.Fill> であるすべての列は、それ以外の列が使用していない表示領域を共有します。 この領域の幅は、フィル モードの列の間で、それぞれの <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> プロパティの値の比率に基づいて分割されます。 たとえば、<xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> の値が 100 の列と 200 の列がある場合、最初の列はもう 1 つの列の半分の幅になります。  
  
## <a name="user-resizing-in-fill-mode"></a>フィル モードでのユーザーによるサイズ変更  
 セルの内容に基づいてサイズが変更されるモードとは異なり、フィル モードでは、ユーザーは <xref:System.Windows.Forms.DataGridViewColumn.Resizable%2A> プロパティの値が `true` の列のサイズを変更できます。 ユーザーが 1 つのフィル モード列のサイズを変更すると、サイズ変更した列より後のすべてのフィル モード列 (<xref:System.Windows.Forms.Control.RightToLeft%2A> が `false` の場合は右側の列、それ以外の場合は左側の列) のサイズも、表示に使用できる幅の変化とつりあうように変わります。 サイズ変更した列より後にフィル モード列がない場合は、コントロール内の他のすべてのフィル モード列のサイズが変更されます。 コントロール内に他のフィル モード列がない場合は、サイズ変更は無視されます。 フィル モードでない列のサイズを変更した場合は、コントロール内のすべてのフィル モード列のサイズが、変更とつりあうように変わります。  
  
 1 つのフィル モード列のサイズを変更すると、それに合わせて変更されたすべての列の <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> の値が比率に応じて調整されます。 たとえば、<xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> 値が 100 である 4 つのフィル モード列のうち、2 番目の列の幅を元の幅の半分にすると、各フィル モード列の <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> 値はそれぞれ 100、50、125、125 になります。 フィル モードでない列のサイズを変更しても、フィル モード列のサイズは同じ比率を維持するように変更されるだけなので、<xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> 値は変わりません。  
  
## <a name="content-based-fillweight-adjustment"></a>内容に基づく FillWeight の調整  
 フィル モード列の <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> の値は、<xref:System.Windows.Forms.DataGridView> の自動サイズ変更メソッドを使用して初期化できます。たとえば、<xref:System.Windows.Forms.DataGridView.AutoResizeColumns%2A> メソッドがあります。 このメソッドは、まず、列の内容を表示するのに必要な幅を計算します。 次に、計算した幅の比率に一致するように、すべてのフィル モード列の <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> の値がコントロールによって調整されます。 最後に、この新しい <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> の比率を使用して、コントロール内のすべての列が水平方向に使用可能な領域を満たすようにフィル モード列のサイズがコントロールによって変更されます。  
  
## <a name="example"></a>例  
  
### <a name="description"></a>[説明]  
 <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A>、<xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A>、<xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A>、および <xref:System.Windows.Forms.DataGridViewColumn.Resizable%2A> の各プロパティに適切な値を使用すると、さまざまなシナリオに応じて列のサイズ変更動作をカスタマイズできます。  
  
 次のデモ コードでは、<xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A>、<xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A>、および <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A> の各プロパティのさまざまな値をさまざまな列で試すことができます。 この例では、<xref:System.Windows.Forms.DataGridView> コントロールはそれ自体の <xref:System.Windows.Forms.DataGridView.Columns%2A> コレクションにバインドしており、それぞれの列は <xref:System.Windows.Forms.DataGridViewColumn.HeaderText%2A>、<xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A>、<xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A>、<xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A>、および <xref:System.Windows.Forms.DataGridViewColumn.Width%2A> の各プロパティにバインドしています。 各列はコントロール内の行でも表されており、1 つの行で値を変更すると対応する列のプロパティも更新されるので、値の相互作用を確認できます。  
  
### <a name="code"></a>コード  
 [!code-csharp[System.Windows.Forms.DataGridViewFillColumnsDemo#00](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewFillColumnsDemo/CS/fillcolumns.cs#00)]
 [!code-vb[System.Windows.Forms.DataGridViewFillColumnsDemo#00](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewFillColumnsDemo/vb/fillcolumns.vb#00)]  
  
### <a name="comments"></a>説明  
 このデモ アプリケーションは、次のようにして使用します。  
  
- フォームのサイズを変更します。 <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> プロパティの値で示される比率が維持されたまま、列の幅が変わることを確認します。  
  
- マウスで列の区分線をドラッグして、列のサイズを変更します。 <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> 値がどのように変わるかを確認します。  
  
- 1 つの列の <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A> 値を変更してから、フォームのサイズをドラッグで変更します。 フォームをどれだけ小さくしても <xref:System.Windows.Forms.DataGridViewColumn.Width%2A> 値が <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A> 値を下回らないことを確認します。  
  
- すべての列の <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A> 値を大きくして、その合計値がコントロールの幅を上回るようにします。 水平スクロール バーが表示されることを確認します。  
  
- 一部の列の <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A> 値を変更します。 列またはフォームのサイズの変更によって何が起きるかを確認します。  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- System、System.Drawing、および System.Windows.Forms の各アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.AutoResizeColumns%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AutoSizeColumnsMode%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewAutoSizeColumnsMode>
- <xref:System.Windows.Forms.DataGridViewColumn>
- <xref:System.Windows.Forms.DataGridViewColumn.InheritedAutoSizeMode%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewAutoSizeColumnMode>
- <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumn.Width%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumn.Resizable%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.Control.RightToLeft%2A?displayProperty=nameWithType>
- [Windows フォーム DataGridView コントロール内の列と行のサイズ変更](resizing-columns-and-rows-in-the-windows-forms-datagridview-control.md)
