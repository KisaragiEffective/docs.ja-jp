---
title: '方法: ファイルに関連付けられているアイコンを抽出する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- displaying a file name and its file type icon in a ListView control [Windows Forms]
- file name extension icons [Windows Forms], displaying in a ListView
- extracting icons associated with a file type [Windows Forms]
ms.assetid: 88e2ad8b-c34f-415a-84f2-dad756b5c928
ms.openlocfilehash: 28769144b0e1c631a31c3c541747a6215f861d0e
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742541"
---
# <a name="how-to-extract-the-icon-associated-with-a-file-in-windows-forms"></a>方法 : Windows フォームでファイルに関連付けられているアイコンを抽出する
多くのファイルには、関連付けられたファイルの種類を視覚的に表現するアイコンが組み込まれています。 たとえば、Microsoft Word 文書には、それらを Word 文書として識別するアイコンが含まれています。 リストコントロールまたはテーブルコントロールにファイルを表示する場合、各ファイル名の横にあるファイルの種類を表すアイコンを表示することができます。 これは、<xref:System.Drawing.Icon.ExtractAssociatedIcon%2A> メソッドを使用して簡単に行うことができます。  
  
## <a name="example"></a>例  
 次のコード例は、ファイルに関連付けられたアイコンを抽出し、ファイル名とそれに関連付けられているアイコンを <xref:System.Windows.Forms.ListView> コントロールに表示する方法を示しています。  
  
 [!code-csharp[System.Drawing.Icon.ExtractAssociatedIconEx#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.Icon.ExtractAssociatedIconEx/CS/Form1.cs#1)]
 [!code-vb[System.Drawing.Icon.ExtractAssociatedIconEx#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.Icon.ExtractAssociatedIconEx/VB/Form1.vb#1)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例をコンパイルするには、次のようにします。  
  
- 前のコードを Windows フォームに貼り付け、フォームのコンストラクターまたは <xref:System.Windows.Forms.Form.Load> イベント処理メソッドから `ExtractAssociatedIconExample` メソッドを呼び出します。  
  
     フォームが <xref:System.IO> 名前空間をインポートすることを確認する必要があります。  
  
## <a name="see-also"></a>参照

- [イメージ、ビットマップ、メタファイル](images-bitmaps-and-metafiles.md)
- [ListView コントロール](../controls/listview-control-windows-forms.md)
