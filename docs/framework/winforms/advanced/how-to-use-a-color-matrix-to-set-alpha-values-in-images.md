---
title: '方法 : カラー行列を使用してイメージのアルファ値を設定する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- images [Windows Forms], using color matrices for semi-transparent
- transparency [Windows Forms], color matrices
- matrices [Windows Forms], alpha values
- bitmaps [Windows Forms], using color matrices for semi-transparent
ms.assetid: a27121e6-f7e9-4c09-84e2-f05aa9d2a1bb
ms.openlocfilehash: 73e820845d040856a0ae367da8b9371ad6afa142
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73423734"
---
# <a name="how-to-use-a-color-matrix-to-set-alpha-values-in-images"></a>方法 : カラー行列を使用してイメージのアルファ値を設定する
<xref:System.Drawing.Bitmap> クラス (<xref:System.Drawing.Image> クラスから継承) と <xref:System.Drawing.Imaging.ImageAttributes> クラスは、ピクセル値を取得および設定するための機能を提供します。 <xref:System.Drawing.Imaging.ImageAttributes> クラスを使用してイメージ全体のアルファ値を変更することも、<xref:System.Drawing.Bitmap> クラスの <xref:System.Drawing.Bitmap.SetPixel%2A> メソッドを呼び出して個々のピクセル値を変更することもできます。  
  
## <a name="example"></a>例  
 <xref:System.Drawing.Imaging.ImageAttributes> クラスには、レンダリング時にイメージを変更するために使用できる多くのプロパティがあります。 次の例では、<xref:System.Drawing.Imaging.ImageAttributes> オブジェクトを使用して、すべてのアルファ値を80% の値に設定しています。 これは、カラーマトリックスを初期化し、マトリックスのアルファスケーリング値を0.8 に設定することによって行われます。 カラーマトリックスのアドレスが <xref:System.Drawing.Imaging.ImageAttributes> オブジェクトの <xref:System.Drawing.Imaging.ImageAttributes.SetColorMatrix%2A> メソッドに渡され、<xref:System.Drawing.Imaging.ImageAttributes> オブジェクトが <xref:System.Drawing.Graphics> オブジェクトの <xref:System.Drawing.Graphics.DrawString%2A> メソッドに渡されます。  
  
 レンダリング中、ビットマップのアルファ値は、その値の80% に変換されます。 これにより、背景とブレンドされるイメージが生成されます。 次の図に示すように、ビットマップイメージは透明に見えます。黒い線が表示されます。  
  
 ![マトリックスを使用したアルファブレンドのスクリーンショット。](./media/how-to-use-a-color-matrix-to-set-alpha-values-in-images/alpha-blending-matrix.png "image2")  
  
 画像が背景の白い部分の上にある場合、画像は白の色にブレンドされています。 画像が黒い線と交差する場合、イメージは黒の色とブレンドされます。  
  
 [!code-csharp[System.Drawing.AlphaBlending#21](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.AlphaBlending/CS/Class1.cs#21)]
 [!code-vb[System.Drawing.AlphaBlending#21](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.AlphaBlending/VB/Class1.vb#21)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 前の例は Windows フォームで使用するように設計されており、<xref:System.Windows.Forms.PaintEventHandler>のパラメーターである <xref:System.Windows.Forms.PaintEventArgs> `e`が必要です。  
  
## <a name="see-also"></a>関連項目

- [Windows フォームにおけるグラフィックスと描画](graphics-and-drawing-in-windows-forms.md)
- [アルファ ブレンドの直線と塗りつぶし](alpha-blending-lines-and-fills.md)
