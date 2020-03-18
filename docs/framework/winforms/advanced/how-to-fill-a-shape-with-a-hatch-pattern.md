---
title: '方法 : ハッチ パターンで図形を塗りつぶす'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- patterns [Windows Forms], adding to shapes
- shapes [Windows Forms], filling with patterns
- brushes [Windows Forms], using hatch brushes
ms.assetid: 9c8300ff-187b-404f-af1f-ebd499f5b16f
ms.openlocfilehash: b80708f0ce722b1809fe49190639231e7e4c8329
ms.sourcegitcommit: 628e8147ca10187488e6407dab4c4e6ebe0cac47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72320050"
---
# <a name="how-to-fill-a-shape-with-a-hatch-pattern"></a>方法 : ハッチ パターンで図形を塗りつぶす
ハッチパターンは2色から作成されます。1つは背景用で、もう1つは背景のパターンを形成する線です。 閉じた図形をハッチパターンで塗りつぶすには、<xref:System.Drawing.Drawing2D.HatchBrush> オブジェクトを使用します。 次の例は、楕円にハッチパターンで塗りつぶす方法を示しています。  
  
## <a name="example"></a>例  
 <xref:System.Drawing.Drawing2D.HatchBrush.%23ctor%2A> コンストラクターは、ハッチスタイル、ハッチ線の色、および背景色の3つの引数を受け取ります。 ハッチスタイル引数には、<xref:System.Drawing.Drawing2D.HatchStyle> 列挙型の任意の値を指定できます。 <xref:System.Drawing.Drawing2D.HatchStyle> 列挙体に50個を超える要素があります。これらの要素のいくつかを次の一覧に示します。  
  
- <xref:System.Drawing.Drawing2D.HatchStyle.Horizontal>  
  
- <xref:System.Drawing.Drawing2D.HatchStyle.Vertical>  
  
- <xref:System.Drawing.Drawing2D.HatchStyle.ForwardDiagonal>  
  
- <xref:System.Drawing.Drawing2D.HatchStyle.BackwardDiagonal>  
  
- <xref:System.Drawing.Drawing2D.HatchStyle.Cross>  
  
- <xref:System.Drawing.Drawing2D.HatchStyle.DiagonalCross>  
  
 次の図は、塗りつぶされた楕円を示しています。  
  
  ![ハッチパターンで塗りつぶされた楕円のスクリーンショットは次のようになります。](./media/how-to-fill-a-shape-with-a-hatch-pattern/ellipse-filled-hatch.png "hatch1")
  
 [!code-csharp[System.Drawing.UsingABrush#41](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.UsingABrush/CS/Class1.cs#41)]
 [!code-vb[System.Drawing.UsingABrush#41](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.UsingABrush/VB/Class1.vb#41)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 前の例は、Windows フォームで使用するために設計されていて、<xref:System.Windows.Forms.PaintEventArgs> イベント ハンドラーのパラメーターである `e`<xref:System.Windows.Forms.Control.Paint> を必要とします。  
  
## <a name="see-also"></a>参照

- [ブラシを使用した図形の塗りつぶし](using-a-brush-to-fill-shapes.md)
