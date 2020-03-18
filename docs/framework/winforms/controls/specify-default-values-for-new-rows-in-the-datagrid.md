---
title: DataGridView コントロールの新しい行に既定値を指定する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data grids [Windows Forms], default values for new rows
- DataGridView control [Windows Forms], data entry
- rows [Windows Forms], specifying default values
- DataGridView control [Windows Forms], default values for new rows
ms.assetid: 8d127963-d9f8-4e4e-9f7f-beb66688f1f2
ms.openlocfilehash: 364f922aefc10e57f2ed7f3a0c2a5b25c922d87a
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742933"
---
# <a name="how-to-specify-default-values-for-new-rows-in-the-windows-forms-datagridview-control"></a>方法 : Windows フォーム DataGridView コントロールの新しい行に既定値を指定する
新しく追加された行に対してアプリケーションで既定値がいっぱいになったときに、データエントリをより使いやすくすることができます。 <xref:System.Windows.Forms.DataGridView> クラスを使用すると、<xref:System.Windows.Forms.DataGridView.DefaultValuesNeeded> イベントに既定値を入力できます。 このイベントは、ユーザーが新しいレコードの行を入力したときに発生します。 コードでこのイベントを処理する場合は、目的のセルに選択した値を設定できます。  
  
 次のコード例では、<xref:System.Windows.Forms.DataGridView.DefaultValuesNeeded> イベントを使用して、新しい行に既定値を指定する方法を示します。  
  
## <a name="example"></a>例  
 [!code-csharp[System.Windows.Forms.DataGridViewMisc#120](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#120)]
 [!code-vb[System.Windows.Forms.DataGridViewMisc#120](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#120)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- <xref:System.Windows.Forms.DataGridView> という名前の `dataGridView1` コントロール。  
  
- 一意の `CustomerID` 値を生成するための `NewCustomerId` 関数。  
  
- <xref:System?displayProperty=nameWithType> アセンブリおよび <xref:System.Windows.Forms?displayProperty=nameWithType> アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.DefaultValuesNeeded?displayProperty=nameWithType>
- [Windows フォーム DataGridView コントロールでのデータ入力](data-entry-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールにおける新規レコード行の使用](using-the-row-for-new-records-in-the-windows-forms-datagridview-control.md)
