---
title: 座標
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Forms coordinates
- screen coordinates
- client coordinates
- coordinates [Windows Forms], Windows Forms
ms.assetid: cc06e61f-43b6-4408-a676-2542dcfcd96e
ms.openlocfilehash: c95888f31bfc867e9c028d53072ab3d710256708
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76734664"
---
# <a name="windows-forms-coordinates"></a>Windows フォームの座標
Windows フォームの座標系はデバイス座標に基づいており、Windows フォームで描画するときの基本的な測定単位はデバイス単位 (通常はピクセル) です。 画面上の点は、x 座標と y 座標のペアで表されます。 x 座標は右に上がり、y 座標は上から下に増加します。 画面を基準とした原点の位置は、画面またはクライアントの座標を指定するかどうかによって異なります。  
  
## <a name="screen-coordinates"></a>画面座標  
 Windows フォームアプリケーションでは、画面上のウィンドウの位置を画面座標で指定します。 画面座標の場合、原点は画面の左上隅になります。 ウィンドウの完全な位置は、多くの場合、ウィンドウの左上隅と右下隅を定義する2つの点の画面座標を含む <xref:System.Drawing.Rectangle> 構造体によって記述されます。  
  
## <a name="client-coordinates"></a>クライアント座標  
 Windows フォームアプリケーションは、クライアント座標を使用して、フォームまたはコントロール内の点の位置を指定します。 クライアント座標の原点は、コントロールまたはフォームのクライアント領域の左上隅です。 クライアント座標は、画面上のフォームまたはコントロールの位置に関係なく、フォームまたはコントロールに描画するときに、アプリケーションが一貫した座標値を使用できるようにします。  
  
 クライアント領域の大きさは、領域のクライアント座標を含む <xref:System.Drawing.Rectangle> 構造によっても記述されます。 いずれの場合も、四角形の左上の座標はクライアント領域に含まれ、右下の座標は除外されます。 グラフィックス操作には、クライアント領域の右端と下端は含まれません。 たとえば、<xref:System.Drawing.Graphics.FillRectangle%2A> メソッドは、指定された四角形の右端と下端までを塗りつぶしますが、これらの辺は含まれません。  
  
## <a name="mapping-from-one-type-of-coordinate-to-another"></a>ある種類の座標から別の型へのマッピング  
 場合によっては、画面座標からクライアント座標にマップする必要があります。 これは、<xref:System.Windows.Forms.Control> クラスで使用可能な <xref:System.Windows.Forms.Control.PointToClient%2A> および <xref:System.Windows.Forms.Control.PointToScreen%2A> メソッドを使用して簡単に実行できます。 たとえば、<xref:System.Windows.Forms.Control> の <xref:System.Windows.Forms.Control.MousePosition%2A> プロパティは画面座標で報告されますが、これらをクライアント座標に変換することができます。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Control.PointToClient%2A>
- <xref:System.Windows.Forms.Control.PointToScreen%2A>
