---
title: '方法 : メタファイルを読み込んで表示する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- examples [Windows Forms], metafiles
- metafiles [Windows Forms], displaying
ms.assetid: 60af1714-f148-4d85-a739-0557965ffa73
ms.openlocfilehash: 6c17e0b2d023ccf80b0d32301b7ee6765edcae9f
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73424821"
---
# <a name="how-to-load-and-display-metafiles"></a>方法 : メタファイルを読み込んで表示する
<xref:System.Drawing.Image> クラスから継承される <xref:System.Drawing.Imaging.Metafile> クラスは、ベクターイメージの記録、表示、および検査のためのメソッドを提供します。  
  
## <a name="example"></a>例  
 ベクターイメージ (メタファイル) を画面に表示するには、<xref:System.Drawing.Imaging.Metafile> オブジェクトと <xref:System.Drawing.Graphics> オブジェクトが必要です。 ファイル (またはストリーム) の名前を <xref:System.Drawing.Imaging.Metafile> コンストラクターに渡します。 <xref:System.Drawing.Imaging.Metafile> オブジェクトを作成したら、その <xref:System.Drawing.Imaging.Metafile> オブジェクトを <xref:System.Drawing.Graphics> オブジェクトの <xref:System.Drawing.Graphics.DrawImage%2A> メソッドに渡します。  
  
 この例では、EMF (拡張メタファイル) ファイルから <xref:System.Drawing.Imaging.Metafile> オブジェクトを作成し、(60, 10) の左上隅でイメージを描画します。  
  
 次の図は、指定した位置に描画されたメタファイルを示しています。  
  
 ![画像の位置を示すスクリーンショット。](./media/how-to-load-and-display-metafiles/metafile-drawn-specified-location.png "imageposition2")  
  
 [!code-csharp[System.Drawing.WorkingWithImages#41](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.WorkingWithImages/CS/Class1.cs#41)]
 [!code-vb[System.Drawing.WorkingWithImages#41](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.WorkingWithImages/VB/Class1.vb#41)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 前の例は、Windows フォームで使用するように設計されています。また、<xref:System.Windows.Forms.Control.Paint> イベントハンドラーのパラメーターである <xref:System.Windows.Forms.PaintEventArgs> `e`が必要です。  
  
## <a name="see-also"></a>関連項目

- [イメージ、ビットマップ、アイコン、およびメタファイルの操作](working-with-images-bitmaps-icons-and-metafiles.md)
