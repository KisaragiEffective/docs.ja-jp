---
title: DataGridView セル内のコントロールのホスト
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- controls [Windows Forms], hosting in cells
- DataGridView control [Windows Forms], hosting controls in cells
- cells [Windows Forms], hosting controls
ms.assetid: e79a9d4e-64ec-41f5-93ec-f5492633cbb2
ms.openlocfilehash: a64521a15a272ca8140302f39d15e7f17e0c423b
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736541"
---
# <a name="how-to-host-controls-in-windows-forms-datagridview-cells"></a>方法 : Windows フォーム DataGridView Cells でコントロールをホストする
<xref:System.Windows.Forms.DataGridView> コントロールには数種類の列があり、ユーザーはさまざまな方法で値を入力し、編集できます。 ただし、これらの種類の列がデータ入力の要件を満たさない場合は、独自の種類の列を作成して、任意のコントロールをホストするセルを用意できます。 これを作成するには、<xref:System.Windows.Forms.DataGridViewColumn> および <xref:System.Windows.Forms.DataGridViewCell> から派生する各クラスを定義する必要があります。 また、<xref:System.Windows.Forms.Control> から派生し、<xref:System.Windows.Forms.IDataGridViewEditingControl> インターフェイスを実装するクラスを定義する必要もあります。  
  
 予定表の列を作成する方法を次のコード例に示します。 この列のセルは、通常のテキスト ボックスのセルに日付を表示しますが、ユーザーがセルを編集するときには <xref:System.Windows.Forms.DateTimePicker> コントロールが表示されます。 テキスト ボックスの表示機能を再度実装しなくてもすむように、`CalendarCell` クラスは、<xref:System.Windows.Forms.DataGridViewTextBoxCell> クラスを直接継承するのではなく <xref:System.Windows.Forms.DataGridViewCell> クラスから派生します。  
  
> [!NOTE]
> <xref:System.Windows.Forms.DataGridViewCell> や <xref:System.Windows.Forms.DataGridViewColumn> から派生したクラスに新しいプロパティを追加するときは、`Clone` メソッドをオーバーライドし、複製操作時に新しいプロパティをコピーする必要があります。 また、基底クラスの `Clone` メソッドを呼び出して、基底クラスのプロパティを新しいセルまたは列にコピーする必要もあります。  
  
## <a name="example"></a>例  
 [!code-csharp[System.Windows.Forms.DataGridViewCalendarColumn#000](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewCalendarColumn/CS/datagridviewcalendarcolumn.cs#000)]
 [!code-vb[System.Windows.Forms.DataGridViewCalendarColumn#000](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewCalendarColumn/VB/datagridviewcalendarcolumn.vb#000)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例には、次の項目が必要です。  
  
- System アセンブリおよび System.Windows.Forms アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewColumn>
- <xref:System.Windows.Forms.DataGridViewCell>
- <xref:System.Windows.Forms.DataGridViewTextBoxCell>
- <xref:System.Windows.Forms.IDataGridViewEditingControl>
- <xref:System.Windows.Forms.DateTimePicker>
- [Windows フォーム DataGridView コントロールのカスタマイズ](customizing-the-windows-forms-datagridview-control.md)
- [DataGridView コントロールのアーキテクチャ](datagridview-control-architecture-windows-forms.md)
- [Windows フォーム DataGridView コントロールの列型](column-types-in-the-windows-forms-datagridview-control.md)
