---
title: タイポグラフィ
ms.date: 03/30/2017
helpviewer_keywords:
- typography [WPF], about typography
ms.assetid: 06cbf17b-6eff-4fe5-949d-2dd533e4e1f4
ms.openlocfilehash: 3d94873931e3ee6df780df214f508258aa07a791
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76735537"
---
# <a name="typography-in-wpf"></a>WPF のタイポグラフィ
このトピックでは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の主要な文字体裁の機能について説明します。 これらの機能には、テキストレンダリングの品質とパフォーマンスの向上、OpenType 文字体裁のサポート、強化されたインターナショナルテキスト、フォントサポートの強化、および新しいテキストアプリケーションプログラミングインターフェイス (Api) が含まれます。  
  
<a name="Improved_Quality_and_Performance_of_Text"></a>   
## <a name="improved-quality-and-performance-of-text"></a>テキストに関する品質とパフォーマンスの向上  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のテキストは Microsoft ClearType を使用してレンダリングされます。これにより、テキストの明瞭さと読みやすさが向上します。 ClearType は、Microsoft によって開発されたソフトウェアテクノロジで、ラップトップの画面、Pocket PC の画面、フラットパネルモニターなど、既存の Lcd (液晶ディスプレイ) でのテキストの読みやすさを向上させます。 ClearType では、ピクセルレンダリングが使用されます。これにより、ピクセルの小数部に文字を配置することで、実際の図形に対する忠実度の高いテキストを表示できます。 解像度が上がるとテキスト表示の微細部の鮮明度が高くなるため、長時間にわたって読んでも苦になりません。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] での ClearType のもう1つの改良点は、y 方向のアンチエイリアシングです。これにより、テキスト文字での浅い曲線の上部および下部を滑らかにすることができます。 ClearType 機能の詳細については、「 [cleartype の概要](cleartype-overview.md)」を参照してください。  
  
 ![ClearType y 方向アンチエイリアシングを含むテキスト](./media/typography-in-wpf/text-y-direction-antialiasing.gif)  
ClearType の y 方向アンチエイリアシングを適用したテキスト  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、コンピューターがハードウェアの最小要件を満たしている限り、テキスト レンダリング パイプライン全体に対してハードウェア アクセラレータを使用できます。 ハードウェアを使用して実行できないレンダリングは、ソフトウェア レンダリングとなります。 ハードウェアアクセラレータは、テキストレンダリングパイプラインのすべてのフェーズに影響を及ぼします。これは、個々のグリフの格納、グリフの実行へのグリフの合成、効果の適用、最終的に表示される出力への ClearType ブレンディングアルゴリズムの適用に関するものです。 ハードウェア アクセラレータの詳細については、「[グラフィックスの描画層](graphics-rendering-tiers.md)」をご覧ください。  
  
 ![パイプラインを描画するテキストのダイアグラム](./media/typography-in-wpf/text-rendering-pipeline.png)  
  
 また、アニメーション化されたテキストは、文字とグリフのいずれによる場合でも、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] で有効化されているグラフィックス ハードウェア機能をすべて利用できます。 これにより、テキスト アニメーションが滑らかになります。  
  
<a name="Rich_Typography"></a>   
## <a name="rich-typography"></a>多彩な文字体裁  
 OpenType フォント形式は、TrueType®フォント形式を拡張したものです。 OpenType フォント形式は、Microsoft と Adobe が共同で開発したものであり、高度な文字体裁機能が豊富に用意されています。 <xref:System.Windows.Documents.Typography> オブジェクトは、スタイルの代替や巻きひげなど、OpenType フォントの高度な機能の多くを公開します。 Windows SDK には、Pericles や Pescadero フォントなどの豊富な機能を使用して設計されたサンプルの OpenType フォントのセットが用意されています。 詳細については、「[OpenType フォント パックのサンプル](sample-opentype-font-pack.md)」をご覧ください。  
  
 Pericles OpenType フォントには、グリフの標準セットに対してスタイルの代替を提供する追加のグリフが含まれています。 次のテキストでは、スタイル代替グリフが表示されています。  
  
 ![OpenType のスタイル代替グリフを使用するテキスト](./media/typography-in-wpf/opentype-stylistic-alternate-glyphs.gif "OpenType のスタイル代替グリフを使用するテキスト")  
  
 飾り付きは装飾的なグリフで、カリグラフィを連想させる、手の込んだ装飾が使用されます。 次のテキストは、Pescadero フォントの標準グリフと巻きひげグリフを示しています。  
  
 ![OpenType の標準グリフとスワッシュ グリフを使用するテキスト](./media/typography-in-wpf/opentype-standard-swash-glyphs.gif "OpenType の標準グリフと飾り付きグリフを使用するテキスト")  
  
 OpenType 機能の詳細については、「 [Opentype フォントの機能](opentype-font-features.md)」を参照してください。  
  
<a name="Enhanced_International_Text_Support"></a>   
## <a name="enhanced-international-text-support"></a>国際対応テキストのサポートの強化  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、次の機能を提供することで、国際対応テキストのサポートを強化しています。  
  
- すべての書記体系における、適用可能な単位を使用した自動行間隔設定。  
  
- 国際対応テキストの幅広いサポート。 詳細については、「[WPF のグローバリゼーション](globalization-for-wpf.md)」をご覧ください。  
  
- 言語に合わせた改行、ハイフネーション、両端揃え。  
  
<a name="Enhanced_Font_Support"></a>   
## <a name="enhanced-font-support"></a>フォントのサポートの強化  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、次の機能を提供することで、フォントのサポートを強化しています。  
  
- すべてのテキストに対応する Unicode。 フォントの動作や選択に文字セットやコードページが不要になりました。  
  
- システム ロケールなど、グローバル設定に左右されないフォント動作。  
  
- <xref:System.Windows.Media.FontFamily>を定義するために、<xref:System.Windows.FontWeight>、<xref:System.Windows.FontStretch>、および <xref:System.Windows.FontStyle> の種類を分離します。 これにより、Win32 プログラミングよりも柔軟性が向上します。ここでは、斜体と太字のブール値の組み合わせを使用してフォントファミリを定義します。  
  
- フォント名とは関係なく処理される書き込み方向 (横書きまたは縦書き)。  
  
- 複合フォントテクノロジを使用した、ポータブル XML ファイル内のフォントリンクとフォントフォールバック。 複合フォントは、完全な多言語フォントの構築を可能にします。 また、複合フォントは、欠落グリフの表示を防ぐ機能も備えています。 詳細については、<xref:System.Windows.Media.FontFamily> クラスの解説を参照してください。  
  
- 単一言語フォントのグループを使用した、複合フォントから構築される国際対応フォント。 これにより、複数言語に対応するフォントの開発時にリソースのコストを節約できます。  
  
- ドキュメントに埋め込むことで移植性が得られる複合フォント。 詳細については、<xref:System.Windows.Media.FontFamily> クラスの解説を参照してください。  
  
<a name="New_Text_APIs"></a>   
## <a name="new-text-application-programming-interfaces-apis"></a>新しいテキスト API (アプリケーション プログラミング インターフェイス)  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、開発者がアプリケーションにテキストを含めるときに使用するテキスト Api がいくつか用意されています。 これらの Api は、次の3つのカテゴリに分類されます。  
  
- **レイアウトとユーザー インターフェイス**: グラフィカルユーザーインターフェイス (GUI) 用の一般的なテキストコントロール。  
  
- **軽量テキスト描画**: オブジェクトにテキストを直接描画できます。  
  
- **テキストの高度な書式設定**: カスタム テキスト エンジンを実装できます。  
  
### <a name="layout-and-user-interface"></a>レイアウトとユーザー インターフェイス  
 最高レベルの機能では、text Api は、<xref:System.Windows.Controls.Label>、<xref:System.Windows.Controls.TextBlock>、<xref:System.Windows.Controls.TextBox>などの一般的な [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] コントロールを提供します。 これらのコントロールは、アプリケーション内に基本的な [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 要素を提供し、テキストの表示や操作を簡単に実行できるようにします。 <xref:System.Windows.Controls.RichTextBox> や <xref:System.Windows.Controls.PasswordBox> などのコントロールを使用すると、より高度なテキスト処理を行うことができます。 <xref:System.Windows.Documents.TextRange>、<xref:System.Windows.Documents.TextSelection>、<xref:System.Windows.Documents.TextPointer> などのクラスを使用すると、便利なテキスト操作を行うことができます。 これらの [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] コントロールは、<xref:System.Windows.Controls.Control.FontFamily%2A>、<xref:System.Windows.Controls.Control.FontSize%2A>、<xref:System.Windows.Controls.Control.FontStyle%2A>などのプロパティを提供します。これにより、テキストの表示に使用するフォントを制御できます。  
  
#### <a name="using-bitmap-effects-transforms-and-text-effects"></a>ビットマップ効果、変換、テキスト効果の使用  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、ビットマップ効果、変換、テキスト効果などの機能を利用して、人の目をひきつけるテキストを作成できます。 テキストに標準タイプのドロップ シャドウ効果を適用した例を次に示します。  
  
 ![ぼかし&#61; 0.25 を使用したテキストシャドウ](./media/typography-in-wpf/drop-shadow-text-effect.jpg) 
  
 テキストにドロップ シャドウ効果とノイズを適用した例を次に示します。  
  
 ![ノイズを含むテキスト シャドウ](./media/typography-in-wpf/drop-shadow-noise-text.jpg) 
  
 テキストの外縁にグロー効果を適用した例を次に示します。  
  
 ![OuterGlowBitmapEffect を使用するテキスト シャドウ](./media/typography-in-wpf/text-shadow-glow-effect.jpg)
  
 テキストにぼかし効果が適用されている例を次に示します。  
  
 ![BlurBitmapEffect を使用するテキスト シャドウ](./media/typography-in-wpf/text-shadow-blur-effect.jpg)  

 テキストの 2 行目を x 軸に沿って 150% 拡大し、3 行目を y 軸に沿って 150% 拡大した例を次に示します。  
  
 ![ScaleTransform を使用してスケールされたテキスト](./media/typography-in-wpf/scaled-text-scaletransform.jpg) 
  
 x 軸に沿って傾斜させたテキストの例を次に示します。  
  
 ![SkewTransform を使用して傾斜させたテキスト](./media/typography-in-wpf/skewed-transformed-text.jpg)
  
 <xref:System.Windows.Media.TextEffect> オブジェクトは、テキスト文字列内の1つまたは複数の文字グループとしてテキストを扱うことができるヘルパーオブジェクトです。 次の例は、回転する個々の文字を示しています。 各文字は、1 秒間隔で個別に回転します。  
  
 ![テキストを回転するテキスト効果のスクリーンショット](./media/typography-in-wpf/rotating-text-effect.jpg) 
  
#### <a name="using-flow-documents"></a>フロー ドキュメントの使用  
 一般的な [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] コントロールに加えて、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、<xref:System.Windows.Documents.FlowDocument> 要素であるテキストプレゼンテーション用のレイアウトコントロールを提供します。 <xref:System.Windows.Documents.FlowDocument> 要素は、<xref:System.Windows.Controls.DocumentViewer> 要素と共に、さまざまなレイアウト要件を持つ大量のテキストのコントロールを提供します。 レイアウトコントロールは、他の [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] コントロールの <xref:System.Windows.Documents.Typography> オブジェクトおよびフォント関連のプロパティを使用して高度なタイポグラフィにアクセスできるようにします。  
  
 次の例は、検索、ナビゲーション、改ページ調整、およびコンテンツスケーリングのサポートを提供する <xref:System.Windows.Controls.FlowDocumentReader>でホストされているテキストコンテンツを示しています。  
  
 ![OpenType フォントを示すスクリーンショット。](./media/typography-in-wpf/typography-text-flowdocumentreader.png)
  
 詳細については、「[WPF のドキュメント](documents-in-wpf.md)」を参照してください。  
  
### <a name="lightweight-text-drawing"></a>軽量テキスト描画  
 <xref:System.Windows.Media.DrawingContext> オブジェクトの <xref:System.Windows.Media.DrawingContext.DrawText%2A> メソッドを使用して、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] オブジェクトにテキストを直接描画できます。 このメソッドを使用するには、<xref:System.Windows.Media.FormattedText> オブジェクトを作成します。 このオブジェクトを使用すると、複数行のテキストを描画できます。このテキストでは、テキスト内の各文字を個々に書式設定できます。 <xref:System.Windows.Media.FormattedText> オブジェクトの機能には、Windows API の DrawText フラグの機能の多くが含まれています。 さらに、<xref:System.Windows.Media.FormattedText> オブジェクトには、テキストが境界を超えたときに省略記号が表示される省略記号サポートなどの機能が含まれています。 いくつかの書式を適用したテキストを次の例に示します。たとえば、2 番目と 3 番目の単語には線状グラデーションが適用されています。  
  
 ![FormattedText オブジェクトを使用して表示されるテキスト](./media/typography-in-wpf/text-formatted-linear-gradient.jpg) 
  
 書式設定されたテキストを <xref:System.Windows.Media.Geometry> オブジェクトに変換して、その他の種類の視覚的に興味のあるテキストを作成できます。 たとえば、テキスト文字列のアウトラインに基づいて <xref:System.Windows.Media.Geometry> オブジェクトを作成できます。  
  
 ![線形グラデーション ブラシを使用するテキスト アウトライン](./media/typography-in-wpf/text-outline-linear-gradient.jpg)  
  
 変換されたテキストのストローク、塗りつぶし、強調表示を変更して、人の目をひく視覚効果を作成するいくつかの方法を次の例に示します。  
  
 ![塗りつぶしとストロークに別の色を使用するテキスト](./media/typography-in-wpf/fill-stroke-text-effect.jpg)  
  
 ![ストロークに適用されるイメージ ブラシを含むテキスト](./media/typography-in-wpf/image-brush-application.jpg)
  
 ![ストロークと強調表示に適用されたイメージブラシを含むテキスト](./media/typography-in-wpf/image-brush-text-application.jpg)
  
 <xref:System.Windows.Media.FormattedText> オブジェクトの詳細については、「[書式設定](drawing-formatted-text.md)されたテキストの描画」を参照してください。  
  
### <a name="advanced-text-formatting"></a>テキストの高度な書式設定  
 テキスト Api の最も高度なレベルでは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] により、<xref:System.Windows.Media.TextFormatting.TextFormatter> オブジェクトと <xref:System.Windows.Media.TextFormatting> 名前空間のその他の型を使用して、カスタムテキストレイアウトを作成することができます。 <xref:System.Windows.Media.TextFormatting.TextFormatter> クラスと関連クラスを使用すると、独自の文字形式、段落スタイル、改行規則、およびその他のインターナショナルテキストのレイアウト機能をサポートするカスタムテキストレイアウトを実装できます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] テキスト レイアウト サポートに関する既定の実装のオーバーライドが必要となるケースはほとんどありません。 ただし、テキストを編集するコントロールやアプリケーションを作成する場合は、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の既定の実装とは異なる実装が必要になることがあります。  
  
 従来のテキスト API とは異なり、<xref:System.Windows.Media.TextFormatting.TextFormatter> は、一連のコールバックメソッドを使用してテキストレイアウトクライアントと対話します。 クライアントは、<xref:System.Windows.Media.TextFormatting.TextSource> クラスの実装でこれらのメソッドを提供する必要があります。 次の図は、クライアントアプリケーションと <xref:System.Windows.Media.TextFormatting.TextFormatter>間のテキストレイアウトの相互作用を示しています。  
  
 ![テキスト レイアウト クライアントと TextFormatter のダイアグラム](./media/typography-in-wpf/text-layout-text-formatter-interaction.png)  
  
 カスタム テキスト レイアウトの作成の詳細については、「[テキストの高度な書式設定](advanced-text-formatting.md)」をご覧ください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Media.FormattedText>
- <xref:System.Windows.Media.TextFormatting.TextFormatter>
- [ClearType の概要](cleartype-overview.md)
- [OpenType フォントの機能](opentype-font-features.md)
- [書式設定されたテキストの描画](drawing-formatted-text.md)
- [テキストの高度な書式設定](advanced-text-formatting.md)
- [[テキスト]](optimizing-performance-text.md)
- [Microsoft の文字体裁](https://docs.microsoft.com/typography/)
