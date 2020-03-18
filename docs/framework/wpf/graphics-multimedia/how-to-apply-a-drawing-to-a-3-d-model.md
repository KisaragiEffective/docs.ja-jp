---
title: '方法 : 3-D モデルに描画を適用する'
ms.date: 03/30/2017
helpviewer_keywords:
- drawings [WPF], applying to 3-D models
- 3-D models [WPF], applying drawings to
ms.assetid: 68357577-b7fc-446e-8be9-a8cc7df3a350
ms.openlocfilehash: 311a3ac1d9fa219a3a365d506d9d0c3e8b6bc229
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459371"
---
# <a name="how-to-apply-a-drawing-to-a-3-d-model"></a>方法 : 3-D モデルに描画を適用する

この例では、<xref:System.Windows.Media.DrawingBrush> を3-d モデルに適用する <xref:System.Windows.Media.Media3D.Material> として使用する方法を示します。

次のコードは、<xref:System.Windows.Media.DrawingBrush>のコンテンツとして <xref:System.Windows.Media.DrawingGroup> を定義します。  <xref:System.Windows.Media.DrawingBrush> は、3-d 平面に適用される <xref:System.Windows.Media.Media3D.DiffuseMaterial> の <xref:System.Windows.Media.Media3D.DiffuseMaterial.Brush%2A> プロパティとして設定されます。

> [!NOTE]
> 多くの場合、次の図のような複雑なオブジェクトと値をリソースとして定義することをお勧めします。これは、コードを再利用し、簡略化することができます。 詳細については、「 [XAML リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)」を参照してください。

[!code-xaml[3DGallery_snip#ApplyDrawingToMaterialInline1](~/samples/snippets/csharp/VS_Snippets_Wpf/3DGallery_snip/CS/ApplyDrawingToMaterialExample.xaml#applydrawingtomaterialinline1)]

## <a name="example"></a>例

次のコードは、サンプル全体を示しています。

[!code-xaml[3DGallery_snip#ApplyDrawingToMaterialExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/3DGallery_snip/CS/ApplyDrawingToMaterialExample.xaml#applydrawingtomaterialexamplewholepage)]

## <a name="see-also"></a>関連項目

- [XAML リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)
- [3-D シーンを作成する](how-to-create-a-3-d-scene.md)
- [Drawing オブジェクトの概要](drawing-objects-overview.md)
- [3-D グラフィックスの概要](3-d-graphics-overview.md)
