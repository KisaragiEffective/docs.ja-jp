---
title: DataGridView 行にバインドされたオブジェクトにアクセスする
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- object binding [Windows Forms], accessing bound objects
- data grids [Windows Forms], accessing bound objects
- DataGridView control [Windows Forms], accessing objects bound to rows
ms.assetid: 0e05748f-4403-4eb8-8b2f-b098108181b5
ms.openlocfilehash: 0b9a4becb78ae817141728467c1e9ea5b693476d
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743168"
---
# <a name="how-to-access-objects-bound-to-windows-forms-datagridview-rows"></a>方法 : Windows フォームの DataGridView 行にバインドされたオブジェクトにアクセスする
場合によっては、ビジネス オブジェクトのコレクションに格納されている情報のテーブルを表示すると便利です。 <xref:System.Windows.Forms.DataGridView> コントロールをそのようなコレクションにバインドすると、<xref:System.ComponentModel.BrowsableAttribute> によって参照不可にマークされない限り、各パブリック プロパティが独自の列に表示されます。 たとえば、`Customer` オブジェクトのコレクションに**名前**や**アドレス**などの列が含まれるようになります。  
  
 アクセスする追加情報とコードがこれらのオブジェクトに含まれている場合は、行オブジェクトを介してアクセスできます。 次のコード例では、ユーザーが複数の行を選択し、ボタンをクリックすると、対応する顧客のそれぞれに請求書を送信できます。  
  
### <a name="to-access-row-bound-objects"></a>行にバインドされたオブジェクトにアクセスするには  
  
- <xref:System.Windows.Forms.DataGridViewRow.DataBoundItem%2A?displayProperty=nameWithType> プロパティを使用します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewObjectBinding#10](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewObjectBinding/CS/datagridviewobjectbinding.cs#10)]
     [!code-vb[System.Windows.Forms.DataGridViewObjectBinding#10](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewObjectBinding/VB/datagridviewobjectbinding.vb#10)]  
  
## <a name="example"></a>例  
 完全なコード例には、単純な `Customer` 実装が含まれ、<xref:System.Windows.Forms.DataGridView> を <xref:System.Collections.ArrayList> オブジェクトがいくつか含まれる `Customer` にバインドします。 <xref:System.Windows.Forms.Control.Click> の <xref:System.Windows.Forms.Button?displayProperty=nameWithType> イベント ハンドラーは、行を介して `Customer` オブジェクトにアクセスする必要があります。これは、<xref:System.Windows.Forms.Form.Load?displayProperty=nameWithType> イベント ハンドラーの外部で、顧客のコレクションにアクセスできないためです。  
  
 [!code-csharp[System.Windows.Forms.DataGridViewObjectBinding#00](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewObjectBinding/CS/datagridviewobjectbinding.cs#00)]
 [!code-vb[System.Windows.Forms.DataGridViewObjectBinding#00](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewObjectBinding/VB/datagridviewobjectbinding.vb#00)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- System アセンブリおよび System.Windows.Forms アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewRow>
- <xref:System.Windows.Forms.DataGridViewRow.DataBoundItem%2A?displayProperty=nameWithType>
- [Windows フォーム DataGridView コントロールでのデータの表示](displaying-data-in-the-windows-forms-datagridview-control.md)
- [方法: オブジェクトを Windows フォーム DataGridView コントロールにバインドする](how-to-bind-objects-to-windows-forms-datagridview-controls.md)
