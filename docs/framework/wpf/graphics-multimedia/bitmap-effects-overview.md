---
title: ビットマップ効果の概要
ms.date: 03/30/2017
helpviewer_keywords:
- bitmap effects [WPF]
ms.assetid: 23cb338e-4b59-4b52-b294-96431f9c9568
ms.openlocfilehash: 4a5c304363efe7d0e9bd9e14ff916f8d852ab3a8
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636017"
---
# <a name="bitmap-effects-overview"></a>ビットマップ効果の概要
ビットマップ効果を使用すると、デザイナーと開発者は、レンダリングされた Windows Presentation Foundation (WPF) コンテンツに視覚効果を適用できます。 たとえば、ビットマップ効果を使用すると、イメージまたはボタンに <xref:System.Windows.Media.Effects.DropShadowBitmapEffect> 効果またはぼかし効果を簡単に適用できます。  
  
> [!IMPORTANT]
> .NET Framework 4 以降では、<xref:System.Windows.Media.Effects.BitmapEffect> クラスは互換性のために残されています。 <xref:System.Windows.Media.Effects.BitmapEffect> クラスを使用しようとすると、互換性のために残されている例外が発生します。 <xref:System.Windows.Media.Effects.BitmapEffect> クラスに代わるものではないのは、<xref:System.Windows.Media.Effects.Effect> クラスです。 ほとんどの場合、<xref:System.Windows.Media.Effects.Effect> クラスは非常に高速です。  

<a name="wpf_effects"></a>   
## <a name="wpf-bitmap-effects"></a>WPF のビットマップ効果  
 ビットマップ効果 (<xref:System.Windows.Media.Effects.BitmapEffect> オブジェクト) は、単純なピクセル処理操作です。 ビットマップ効果は、入力として <xref:System.Windows.Media.Imaging.BitmapSource> を受け取り、ぼかしやドロップシャドウなどの効果を適用した後に新しい <xref:System.Windows.Media.Imaging.BitmapSource> を生成します。 各ビットマップ効果は、<xref:System.Windows.Media.Effects.BlurBitmapEffect>の <xref:System.Windows.Media.Effects.BlurBitmapEffect.Radius%2A> など、フィルターのプロパティを制御できるプロパティを公開します。  
  
 特殊なケースとして、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]では、<xref:System.Windows.Controls.Button> や <xref:System.Windows.Controls.TextBox>など、ライブ <xref:System.Windows.Media.Visual> オブジェクトのプロパティとして効果を設定できます。 ピクセル処理は、実行時に適用されてレンダリングされます。 この場合、レンダリング時には、<xref:System.Windows.Media.Visual> が自動的に同等の <xref:System.Windows.Media.Imaging.BitmapSource> に変換され、<xref:System.Windows.Media.Effects.BitmapEffect>に入力として渡されます。 出力は、<xref:System.Windows.Media.Visual> オブジェクトの既定の表示動作に置き換わるものです。 このため、<xref:System.Windows.Media.Effects.BitmapEffect> オブジェクトは、視覚エフェクトが適用されている場合に、ビジュアルにハードウェアアクセラレータが適用されないようにするだけで、ビジュアルをソフトウェアでレンダリングすることになります。  
  
- <xref:System.Windows.Media.Effects.BlurBitmapEffect> は、フォーカスがないオブジェクトをシミュレートします。  
  
- <xref:System.Windows.Media.Effects.OuterGlowBitmapEffect> は、オブジェクトの周囲に色のハローを作成します。  
  
- <xref:System.Windows.Media.Effects.DropShadowBitmapEffect> は、オブジェクトの背後に影を作成します。  
  
- <xref:System.Windows.Media.Effects.BevelBitmapEffect> は、指定された曲線に従ってイメージの表面を生成するベベルを作成します。  
  
- <xref:System.Windows.Media.Effects.EmbossBitmapEffect> は <xref:System.Windows.Media.Visual> のバンプマッピングを作成して、人工光源の奥行とテクスチャの印象を与えます。  
  
> [!NOTE]
> WPF ビットマップ効果は、ソフトウェアモードで表示されます。 効果を適用するオブジェクトも、ソフトウェアでレンダリングされます。 大きなビジュアルにビットマップ効果を使うとき、またはビットマップ効果のプロパティをアニメーション化するときに、パフォーマンスが最も低下します。 このような方法ではビットマップ効果をまったく使ってはならないということではありませんが、注意を払い、十分にテストを行って、ユーザー エクスペリエンスが期待どおりになることを確認する必要があります。  
  
> [!NOTE]
> WPF ビットマップ効果では、部分信頼実行はサポートされていません。 ビットマップ効果を使うには、アプリケーションが完全な信頼のアクセス許可を持つ必要があります。  
  
<a name="applyeffects"></a>   
## <a name="how-to-apply-an-effect"></a>効果を適用する方法  
 <xref:System.Windows.Media.Effects.BitmapEffect> は <xref:System.Windows.Media.Visual>のプロパティです。 したがって、<xref:System.Windows.Controls.Button>、<xref:System.Windows.Controls.Image>、<xref:System.Windows.Media.DrawingVisual>、<xref:System.Windows.UIElement>などのビジュアルに効果を適用するのは、プロパティを設定するのと同じように簡単です。 <xref:System.Windows.UIElement.BitmapEffect%2A> は、1つの <xref:System.Windows.Media.Effects.BitmapEffect> オブジェクトに設定することも、<xref:System.Windows.Media.Effects.BitmapEffectGroup> オブジェクトを使用して複数の効果を連結することもできます。  
  
 次の例は、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]で <xref:System.Windows.Media.Effects.BitmapEffect> を適用する方法を示しています。  
  
 [!code-xaml[EffectsGallery_snip#BlurSimpleExampleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/EffectsGallery_snip/CSharp/blursimpleexample.xaml#blursimpleexampleinline)]  
  
 次の例は、<xref:System.Windows.Media.Effects.BitmapEffect> をコードに適用する方法を示しています。  
  
 [!code-csharp[EffectsGallery_snip#CodeBehindBlurCodeBehindExampleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/EffectsGallery_snip/CSharp/blurcodebehindexample.xaml.cs#codebehindblurcodebehindexampleinline)]  
  
> [!NOTE]
> <xref:System.Windows.Controls.DockPanel> や <xref:System.Windows.Controls.Canvas>などのレイアウトコンテナーに <xref:System.Windows.Media.Effects.BitmapEffect> が適用されると、そのすべての子要素を含む要素またはビジュアルのビジュアルツリーに効果が適用されます。  
  
<a name="customeffects"></a>   
## <a name="creating-custom-effects"></a>カスタム効果の作成  
 WPF には、マネージ WPF アプリケーションで使用できるカスタム効果を作成するためのアンマネージインターフェイスも用意されています。 カスタム ビットマップ効果の作成に関する参考資料については、「[Unmanaged WPF Bitmap Effect](https://docs.microsoft.com/previous-versions/windows/desktop/wibe/-wibe-lh)」(アンマネージ WPF ビットマップ効果) ドキュメントをご覧ください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Media.Effects.BitmapEffectGroup>
- <xref:System.Windows.Media.Effects.BitmapEffectInput>
- <xref:System.Windows.Media.Effects.BitmapEffectCollection>
- [アンマネージ WPF ビットマップ効果](https://docs.microsoft.com/previous-versions/windows/desktop/wibe/-wibe-lh)
- [イメージングの概要](imaging-overview.md)
- [Security](../security-wpf.md)
- [WPF グラフィックス レンダリングの概要](wpf-graphics-rendering-overview.md)
- [2D グラフィックスとイメージング](../advanced/optimizing-performance-2d-graphics-and-imaging.md)
