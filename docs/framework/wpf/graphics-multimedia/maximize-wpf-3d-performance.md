---
title: 3D パフォーマンスの最大化
ms.date: 03/30/2017
helpviewer_keywords:
- 3-D graphics [WPF]
ms.assetid: 4bcf949d-d92f-4d8d-8a9b-1e4c61b25bf6
ms.openlocfilehash: 414a4677a1e05cd69c382e35194328d6ce1bddf9
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744597"
---
# <a name="maximize-wpf-3d-performance"></a>WPF の 3D パフォーマンスの最大化
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] を使用して3D コントロールを作成し、アプリケーションに3D シーンを含めると、パフォーマンスの最適化を考慮することが重要になります。 このトピックでは、アプリケーションのパフォーマンスに影響を与える3D クラスとプロパティの一覧と、それらを使用するときのパフォーマンスを最適化するための推奨事項について説明します。  
  
 このトピックでは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] 3D 機能について高度に理解することを前提としています。 このドキュメントの提案は、"レンダリング層 2" に適用されます。これは、ピクセルシェーダーバージョン2.0 と頂点シェーダーバージョン2.0 をサポートするハードウェアとしてほぼ定義されています。 詳細については、「[グラフィックスの描画層](../advanced/graphics-rendering-tiers.md)」を参照してください。  
  
## <a name="performance-impact-high"></a>パフォーマンスへの影響: 高  
  
|プロパティ|推奨|  
|-|-|  
|<xref:System.Windows.Media.Brush>|ブラシの速度 (最速から低速):<br /><br /> <xref:System.Windows.Media.SolidColorBrush><br /><br /> <xref:System.Windows.Media.LinearGradientBrush><br /><br /> <xref:System.Windows.Media.ImageBrush><br /><br /> <xref:System.Windows.Media.DrawingBrush> (キャッシュ済み)<br /><br /> <xref:System.Windows.Media.VisualBrush> (キャッシュ済み)<br /><br /> <xref:System.Windows.Media.RadialGradientBrush><br /><br /> <xref:System.Windows.Media.DrawingBrush> (キャッシュなし)<br /><br /> <xref:System.Windows.Media.VisualBrush> (キャッシュなし)|  
|<xref:System.Windows.UIElement.ClipToBoundsProperty>|<xref:System.Windows.Controls.Viewport3D> の内容を Viewport3D's 四角形に明示的にクリップ [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] 必要がない場合は常に、`Viewport3D.ClipToBounds` を false に設定します。 アンチエイリアス化されたクリップ [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] は非常に遅くなり、<xref:System.Windows.Controls.Viewport3D>では `ClipToBounds` が既定で有効 (低速) になります。|  
|<xref:System.Windows.UIElement.IsHitTestVisible%2A>|マウスヒットテストの実行時に <xref:System.Windows.Controls.Viewport3D> の内容を考慮する必要 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] がない場合は常に、`Viewport3D.IsHitTestVisible` を false に設定します。  ヒットテスト3D コンテンツはソフトウェアで実行され、大きなメッシュでは速度が低下する可能性があります。 <xref:System.Windows.Controls.Viewport3D>では、既定で <xref:System.Windows.UIElement.IsHitTestVisible%2A> が有効 (低速) になります。|  
|<xref:System.Windows.Media.Media3D.GeometryModel3D>|異なるモデルを作成するのは、異なる素材または変換が必要な場合のみにしてください。  それ以外の場合は、同じ素材を持つ多数の <xref:System.Windows.Media.Media3D.GeometryModel3D> インスタンスを結合し、いくつかの大きな <xref:System.Windows.Media.Media3D.GeometryModel3D> と <xref:System.Windows.Media.Media3D.MeshGeometry3D> インスタンスに変換します。|  
|<xref:System.Windows.Media.Media3D.MeshGeometry3D>|メッシュアニメーション—フレームごとにメッシュの個々の頂点を変更することは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]では常に効率的であるとは限りません。  各頂点が変更されたときの変更通知のパフォーマンスへの影響を最小限に抑えるには、頂点ごとの変更を実行する前に、ビジュアルツリーからメッシュをデタッチします。  メッシュが変更されたら、ビジュアルツリーに再アタッチします。  また、この方法でアニメーション化されるメッシュのサイズを最小化してみてください。|  
|3D アンチエイリアシング|表示速度を上げるには、添付プロパティ <xref:System.Windows.Media.RenderOptions.EdgeMode%2A> を `Aliased`に設定して、<xref:System.Windows.Controls.Viewport3D> のマルチサンプリングを無効にします。  既定では、3D アンチエイリアシングは、1ピクセルあたり4サンプルの Windows で有効になっています。|  
|Text|3D シーンのライブテキスト (<xref:System.Windows.Media.DrawingBrush> または <xref:System.Windows.Media.VisualBrush>内にあるためライブテキスト) が低速になることがあります。 テキストが変更されない限り、(<xref:System.Windows.Media.Imaging.RenderTargetBitmap>経由で) テキストの画像を使用してください。|  
|<xref:System.Windows.Media.TileBrush>|ブラシの内容が静的でないために3D シーンで <xref:System.Windows.Media.VisualBrush> または <xref:System.Windows.Media.DrawingBrush> を使用する必要がある場合は、ブラシのキャッシュを試します (添付プロパティ <xref:System.Windows.Media.RenderOptions.CachingHint%2A> を `Cache`に設定します)。  必要な品質レベルを維持したまま、キャッシュされたブラシが頻繁に再生成されないように、(添付プロパティ <xref:System.Windows.Media.RenderOptions.CacheInvalidationThresholdMinimum%2A> および <xref:System.Windows.Media.RenderOptions.CacheInvalidationThresholdMaximum%2A>) のスケール無効化の最小しきい値と最大値を設定します。  既定では、<xref:System.Windows.Media.DrawingBrush> と <xref:System.Windows.Media.VisualBrush> はキャッシュされません。つまり、ブラシで塗りつぶされたものを再描画する必要があるたびに、ブラシのコンテンツ全体を中間サーフェイスに再描画する必要があります。|  
|<xref:System.Windows.Media.Effects.BitmapEffect>|<xref:System.Windows.Media.Effects.BitmapEffect> は、影響を受けるすべてのコンテンツを強制的にハードウェアアクセラレータなしで表示します。  最適なパフォーマンスを得るには、<xref:System.Windows.Media.Effects.BitmapEffect>を使用しないでください。|  
  
## <a name="performance-impact-medium"></a>パフォーマンスへの影響: 中  
  
|プロパティ|推奨|  
|-|-|  
|<xref:System.Windows.Media.Media3D.MeshGeometry3D>|メッシュが共有頂点を持つ隣接する三角形として定義され、それらの頂点の位置、法線、およびテクスチャの座標が同じである場合は、各共有頂点を1回だけ定義し、<xref:System.Windows.Media.Media3D.MeshGeometry3D.TriangleIndices%2A>でインデックスを使用して三角形を定義します。|  
|<xref:System.Windows.Media.ImageBrush>|サイズを明示的に制御している場合 (<xref:System.Windows.Media.Imaging.RenderTargetBitmap> や <xref:System.Windows.Media.ImageBrush>を使用している場合) は、テクスチャのサイズを最小限に抑えてください。  解像度が低くなると、画質が低下する可能性があるので、品質とパフォーマンスのバランスを適切に見つけることができます。|  
|Opacity|半透明の3D コンテンツ (反射など) をレンダリングする場合は、`Viewport3D.Opacity` を1未満の値に設定することによって、別の半透明の <xref:System.Windows.Controls.Viewport3D> を作成するのではなく、(<xref:System.Windows.Media.Brush.Opacity%2A> または <xref:System.Windows.Media.Media3D.DiffuseMaterial.Color%2A>によって) ブラシまたはマテリアルの不透明度プロパティを使用します。|  
|<xref:System.Windows.Controls.Viewport3D>|シーンで使用している <xref:System.Windows.Controls.Viewport3D> オブジェクトの数を最小限に抑えます。  モデルごとに個別の Viewport3D インスタンスを作成するのではなく、複数の3D モデルを同じ Viewport3D に配置します。|  
|<xref:System.Windows.Freezable>|通常は、<xref:System.Windows.Media.Media3D.MeshGeometry3D>、<xref:System.Windows.Media.Media3D.GeometryModel3D>、ブラシ、マテリアルを再利用すると便利です。  これらは `Freezable`から派生したものであるため、すべてが可能です。|  
|<xref:System.Windows.Freezable>|アプリケーションでプロパティが変更されていない場合は、Freezable で <xref:System.Windows.Freezable.Freeze%2A> メソッドを呼び出します。  固定すると、ワーキングセットを減らして速度を上げることができます。|  
|<xref:System.Windows.Media.Brush>|ブラシの内容が変更されない場合は、<xref:System.Windows.Media.VisualBrush> または <xref:System.Windows.Media.DrawingBrush> ではなく <xref:System.Windows.Media.ImageBrush> を使用します。  2D コンテンツは、<xref:System.Windows.Media.Imaging.RenderTargetBitmap> を使用して <xref:System.Windows.Controls.Image> に変換し、<xref:System.Windows.Media.ImageBrush>で使用できます。|  
|<xref:System.Windows.Media.Media3D.GeometryModel3D.BackMaterial%2A>|<xref:System.Windows.Media.Media3D.GeometryModel3D>の背面を実際に表示する必要がある場合を除き、<xref:System.Windows.Media.Media3D.GeometryModel3D.BackMaterial%2A> は使用しないでください。|  
|<xref:System.Windows.Media.Media3D.Light>|ライト速度 (最速から低速):<br /><br /> <xref:System.Windows.Media.Media3D.AmbientLight><br /><br /> <xref:System.Windows.Media.Media3D.DirectionalLight><br /><br /> <xref:System.Windows.Media.Media3D.PointLight><br /><br /> <xref:System.Windows.Media.Media3D.SpotLight>|  
|<xref:System.Windows.Media.Media3D.MeshGeometry3D>|次の制限事項の下でメッシュサイズを維持してください:<br /><br /> <xref:System.Windows.Media.Media3D.MeshGeometry3D.Positions%2A>: 20001 <xref:System.Windows.Media.Media3D.Point3D> インスタンス<br /><br /> <xref:System.Windows.Media.Media3D.MeshGeometry3D.TriangleIndices%2A>: 60003 <xref:System.Int32> インスタンス|  
|<xref:System.Windows.Media.Media3D.Material>|素材の速度 (最速から低速):<br /><br /> <xref:System.Windows.Media.Media3D.EmissiveMaterial><br /><br /> <xref:System.Windows.Media.Media3D.DiffuseMaterial><br /><br /> <xref:System.Windows.Media.Media3D.SpecularMaterial>|  
|<xref:System.Windows.Media.Brush>|[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] 3D は、一貫した方法で非表示のブラシ (黒のアンビエントブラシ、クリアブラシなど) をオプトアウトしません。  これらをシーンから省略することを検討してください。|  
|<xref:System.Windows.Media.Media3D.MaterialGroup>|<xref:System.Windows.Media.Media3D.MaterialGroup> 内の各 <xref:System.Windows.Media.Media3D.Material> によって別のレンダリングパスが生成されます。そのため、多くの素材を含め、単純な素材でも、GPU での入力要求が大幅に増加する可能性があります。  <xref:System.Windows.Media.Media3D.MaterialGroup>内の素材の数を最小限に抑えます。|  
  
## <a name="performance-impact-low"></a>パフォーマンスへの影響: 低  
  
|プロパティ|推奨|  
|-|-|  
|<xref:System.Windows.Media.Media3D.Transform3DGroup>|アニメーションまたはデータバインディングが不要な場合は、複数の変換を含む変換グループを使用するのではなく、1つの <xref:System.Windows.Media.Media3D.MatrixTransform3D>を使用して、変換グループに個別に存在する可能性があるすべての変換の積として設定します。|  
|<xref:System.Windows.Media.Media3D.Light>|シーンのライト数を最小限に抑えます。 シーン内のライトが多すぎると、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] 強制的にソフトウェアレンダリングにフォールバックされます。  制限は、約 110 <xref:System.Windows.Media.Media3D.DirectionalLight> オブジェクト、70 <xref:System.Windows.Media.Media3D.PointLight> オブジェクト、または 40 <xref:System.Windows.Media.Media3D.SpotLight> オブジェクトです。|  
|<xref:System.Windows.Media.Media3D.ModelVisual3D>|オブジェクトを別の <xref:System.Windows.Media.Media3D.ModelVisual3D> インスタンスに配置することで、オブジェクトを静的オブジェクトから分離します。  ModelVisual3D は、変換された境界をキャッシュするため <xref:System.Windows.Media.Media3D.GeometryModel3D> よりも "重い" です。  GeometryModel3D はモデルとして最適化されています。ModelVisual3D はシーンノードになるように最適化されています。  ModelVisual3D を使用して、GeometryModel3D の共有インスタンスをシーンに配置します。|  
|<xref:System.Windows.Media.Media3D.Light>|シーンのライト数を変更する回数を最小限に抑えます。  これらの構成が既に存在していて、シェーダーがキャッシュされている場合を除き、ライト数を変更するたびにシェーダーの再生成と再コンパイルが行われます。|  
|浅煎り|黒のライトは表示されませんが、レンダリング時間が追加されます。省略することを検討してください。|  
|<xref:System.Windows.Media.Media3D.MeshGeometry3D>|MeshGeometry3D's <xref:System.Windows.Media.Media3D.MeshGeometry3D.Positions%2A>、<xref:System.Windows.Media.Media3D.MeshGeometry3D.Normals%2A>、<xref:System.Windows.Media.Media3D.MeshGeometry3D.TextureCoordinates%2A>、<xref:System.Windows.Media.Media3D.MeshGeometry3D.TriangleIndices%2A>など、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]の大きなコレクションの構築時間を最小限に抑えるには、値を作成する前にコレクションのサイズを事前に設定します。 可能であれば、コレクションのコンストラクターに、配列やリストなどの事前設定されたデータ構造を渡します。|  
  
## <a name="see-also"></a>参照

- [3-D グラフィックスの概要](3-d-graphics-overview.md)
