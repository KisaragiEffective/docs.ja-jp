---
title: 座標系の種類
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- transformations [Windows Forms], page
- unit of measure
- world coordinate system
- page coordinate system
- page transformation
- device coordinate system
- world transformation
- coordinate systems
- transformations [Windows Forms], world
ms.assetid: c61ff50a-eb1d-4e6c-83cd-f7e9764cfa9f
ms.openlocfilehash: bbb0d4908d4a281499336d262c653d7b72f3ccdc
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159807"
---
# <a name="types-of-coordinate-systems"></a>座標系の種類
GDI + では、ワールド、ページ、デバイスという3つの座標空間が使用されます。 ワールド座標は、特定のグラフィックワールドをモデル化するために使用される座標であり、.NET Framework のメソッドに渡す座標です。 ページ座標は、フォームやコントロールなど、描画サーフェイスによって使用される座標系を表します。 デバイス座標は、画面や用紙など、描画される物理デバイスで使用される座標です。 `myGraphics.DrawLine(myPen, 0, 0, 160, 80)`呼び出しを行うと、<xref:System.Drawing.Graphics.DrawLine%2A> メソッド (`(0, 0)` および `(160, 80)`) に渡す点は、ワールド座標空間にあります。 GDI + が画面上に線を描画する前に、座標は一連の変換を通過します。 ワールド変換と呼ばれる1つの変換は、ワールド座標をページ座標に変換し、ページ変換と呼ばれる別の変換はページ座標をデバイス座標に変換します。  
  
## <a name="transforms-and-coordinate-systems"></a>変換と座標系  
 左上隅ではなく、クライアント領域の本体に原点がある座標系を操作するとします。 たとえば、原点をクライアント領域の左端から100ピクセル、クライアント領域の上端から50ピクセルにする必要があるとします。 次の図は、このような座標系を示しています。  
  
 ![座標系](./media/aboutgdip05-art01.gif "AboutGdip05_art01")  
  
 `myGraphics.DrawLine(myPen, 0, 0, 160, 80)`呼び出しを行うと、次の図のような行が表示されます。  
  
 ![座標系](./media/aboutgdip05-art02.gif "AboutGdip05_art02")  
  
 3つの座標空間における直線の終点の座標は、次のようになります。  
  
|||  
|-|-|  
|World|(0, 0) ~ (160, 80)|  
|ページ|(100, 50) から (260、130)|  
|Device|(100, 50) から (260、130)|  
  
 ページ座標空間の原点がクライアント領域の左上隅にあることに注意してください。常にこれが当てはまります。 また、測定単位はピクセルであるため、デバイス座標はページ座標と同じであることに注意してください。 測定単位をピクセル以外の値 (インチなど) に設定した場合、デバイス座標はページ座標とは異なります。  
  
 ワールド座標をページ座標にマップするワールド変換は、<xref:System.Drawing.Graphics> クラスの <xref:System.Drawing.Graphics.Transform%2A> プロパティに保持されます。 前の例では、ワールド変換は、x 方向の平行移動100単位と y 方向の50単位です。 次の例では、<xref:System.Drawing.Graphics> オブジェクトのワールド変換を設定し、その <xref:System.Drawing.Graphics> オブジェクトを使用して、前の図に示した線を描画します。  
  
 [!code-csharp[System.Drawing.CoordinateSystems#31](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.CoordinateSystems/CS/Class1.cs#31)]
 [!code-vb[System.Drawing.CoordinateSystems#31](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.CoordinateSystems/VB/Class1.vb#31)]  
  
 ページ変換は、ページ座標をデバイス座標にマップします。 <xref:System.Drawing.Graphics> クラスは、ページ変換を操作するための <xref:System.Drawing.Graphics.PageUnit%2A> および <xref:System.Drawing.Graphics.PageScale%2A> プロパティを提供します。 <xref:System.Drawing.Graphics> クラスには、ディスプレイデバイスの水平方向と垂直方向のドットを調べるために、<xref:System.Drawing.Graphics.DpiX%2A> と <xref:System.Drawing.Graphics.DpiY%2A>の2つの読み取り専用プロパティも用意されています。  
  
 <xref:System.Drawing.Graphics> クラスの <xref:System.Drawing.Graphics.PageUnit%2A> プロパティを使用して、ピクセル以外の測定単位を指定できます。  
  
> [!NOTE]
> <xref:System.Drawing.Graphics.PageUnit%2A> プロパティを <xref:System.Drawing.GraphicsUnit.World>に設定することはできません。これは物理単位ではないため、例外が発生します。  
  
 次の例では、(0, 0) から (2, 1) までの線を描画します。ここで、点 (2, 1) は右に2インチ、ポイント (0, 0) から1インチ下になります。  
  
 [!code-csharp[System.Drawing.CoordinateSystems#32](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.CoordinateSystems/CS/Class1.cs#32)]
 [!code-vb[System.Drawing.CoordinateSystems#32](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.CoordinateSystems/VB/Class1.vb#32)]  
  
> [!NOTE]
> ペンを作成するときにペンの幅を指定しなかった場合、前の例では1インチ幅の線が描画されます。 <xref:System.Drawing.Pen> コンストラクターの2番目の引数には、ペンの幅を指定できます。  
  
 [!code-csharp[System.Drawing.CoordinateSystems#33](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.CoordinateSystems/CS/Class1.cs#33)]
 [!code-vb[System.Drawing.CoordinateSystems#33](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.CoordinateSystems/VB/Class1.vb#33)]  
  
 ディスプレイデバイスの水平方向には96ドット/インチ、垂直方向には96ドットがあると想定している場合、前の例の線の端点には、3つの座標空間に次の座標があります。  
  
|||  
|-|-|  
|World|(0, 0) ~ (2, 1)|  
|ページ|(0, 0) ~ (2, 1)|  
|Device|(0, 0) ~ (192, 96)|  
  
 ワールド座標空間の原点がクライアント領域の左上隅にあるため、ページ座標はワールド座標と同じであることに注意してください。  
  
 世界およびページの変換を組み合わせて、さまざまな効果を実現できます。 たとえば、ミリメートルを測定単位として使用する場合に、座標系の原点をクライアント領域の左端から2インチ、クライアント領域の上端から1/2 インチにする必要があるとします。 次の例では、<xref:System.Drawing.Graphics> オブジェクトのワールド変換とページ変換を設定し、(0, 0) から (2, 1) までの線を描画します。  
  
 [!code-csharp[System.Drawing.CoordinateSystems#34](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.CoordinateSystems/CS/Class1.cs#34)]
 [!code-vb[System.Drawing.CoordinateSystems#34](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.CoordinateSystems/VB/Class1.vb#34)]  
  
 次の図は、線と座標系を示しています。  
  
 ![座標系](./media/aboutgdip05-art03.gif "AboutGdip05_art03")  
  
 ディスプレイデバイスの水平方向には96ドット/インチ、垂直方向には96ドットがあると想定している場合、前の例の線の端点には、3つの座標空間に次の座標があります。  
  
|||  
|-|-|  
|World|(0, 0) ~ (2, 1)|  
|ページ|(2, 0.5) から (4, 1.5)|  
|Device|(192, 48) から (384、144)|  
  
## <a name="see-also"></a>参照

- [座標系と変換](coordinate-systems-and-transformations.md)
- [変換の行列表現](matrix-representation-of-transformations.md)
