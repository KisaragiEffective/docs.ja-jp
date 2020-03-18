---
title: ユーザーが DataGridView コントロールからクリップボードに複数のセルをコピーできるようにする
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cells [Windows Forms], copying to Clipboard
- DataGridView control [Windows Forms], copying multiple cells
- data grids [Windows Forms], copying multiple cells
- Clipboard [Windows Forms], copying multiple cells
ms.assetid: fd0403b2-d0e3-4ae0-839c-0f737e1eb4a9
ms.openlocfilehash: 2bb74a1f0c59b28ab496ce9c89c1c1b5f9d8147b
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745774"
---
# <a name="how-to-enable-users-to-copy-multiple-cells-to-the-clipboard-from-the-windows-forms-datagridview-control"></a>方法 : ユーザーが、Windows フォーム DataGridView コントロールからクリップボードに複数のセルをコピーできるようにする
セルのコピーを有効にすると、<xref:System.Windows.Forms.DataGridView> コントロール内のデータを <xref:System.Windows.Forms.Clipboard> 経由で他のアプリケーションが簡単に利用できるようになります。 選択したセルの値は文字列に変換されてクリップボードに追加され、メモ帳や Excel などのアプリケーションにはタブ区切りのテキスト値として貼り付けられ、Word などのアプリケーションには HTML 形式のテーブルとして貼り付けられます。  
  
 セルのコピーは、セル値のみをコピーするか、行と列のヘッダー テキストをクリップボード データに含めるか、またはユーザーが行または列全体を選択したときのみヘッダー テキストを含めるように設定できます。  
  
 選択モードによっては、ユーザーは、連続しない複数のセル グループを選択できます。 ユーザーがセルをクリップボードにコピーする際、セルが選択されていない行と列はコピーされません。 その他の行と列はすべて、クリップボードにコピーされたデータのテーブル内の行と列になります。 これらの行や列で選択されていないセルは、空白のプレースホルダーとしてクリップボードにコピーされます。  
  
### <a name="to-enable-cell-copying"></a>セルのコピーを有効にするには  
  
- <xref:System.Windows.Forms.DataGridView.ClipboardCopyMode%2A?displayProperty=nameWithType> プロパティを設定します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewClipboardDemo#15](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewClipboardDemo/CS/datagridviewclipboarddemo.cs#15)]
     [!code-vb[System.Windows.Forms.DataGridViewClipboardDemo#15](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewClipboardDemo/VB/datagridviewclipboarddemo.vb#15)]  
  
## <a name="example"></a>例  
 セルをクリップボードにコピーする完全なコード例を次に示します。 この例には、<xref:System.Windows.Forms.DataGridView.GetClipboardContent%2A?displayProperty=nameWithType> メソッドを使用して、選択したセルをクリップボードにコピーし、クリップボードの内容をテキスト ボックスに表示するボタンが含まれています。  
  
 [!code-csharp[System.Windows.Forms.DataGridViewClipboardDemo#00](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewClipboardDemo/CS/datagridviewclipboarddemo.cs#00)]
 [!code-vb[System.Windows.Forms.DataGridViewClipboardDemo#00](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewClipboardDemo/VB/datagridviewclipboarddemo.vb#00)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 このコードには、次のものが必要です。  
  
- N:System アセンブリおよび N:System.Windows.Forms アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.ClipboardCopyMode%2A>
- <xref:System.Windows.Forms.DataGridView.GetClipboardContent%2A>
- [Windows フォーム DataGridView コントロールでの選択およびクリップボードの使用](selection-and-clipboard-use-with-the-windows-forms-datagridview-control.md)
