---
title: 書式設定されたテキストの描画
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- text [WPF]
- typography [WPF], drawing formatted text
- formatted text [WPF]
- drawing [WPF], formatted text
ms.assetid: b1d851c1-331c-4814-9964-6fe769db6f1f
ms.openlocfilehash: f23f54283849ddaa827a98f0f28a39a72305dc1d
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095230"
---
# <a name="drawing-formatted-text"></a>書式設定されたテキストの描画
このトピックでは、<xref:System.Windows.Media.FormattedText> オブジェクトの機能の概要について説明します。 このオブジェクトは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] アプリケーションでのテキストの描画に対する低レベルの制御を提供します。  

## <a name="technology-overview"></a>テクノロジの概要  
 <xref:System.Windows.Media.FormattedText> オブジェクトを使用すると、複数行のテキストを描画することができます。この場合、テキスト内の各文字を個別に書式設定できます。 複数の書式が適用されたテキストを次の例に示します。  
  
 ![FormattedText オブジェクトを使用して表示されるテキスト](./media/typography-in-wpf/text-formatted-linear-gradient.jpg)  
  
> [!NOTE]
> Win32 API から移行する開発者にとって、Win32 の[移行](#win32_migration)に関するセクションの表には、Win32 の DrawText フラグと、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]におけるおおよその等価を示しています。  
  
### <a name="reasons-for-using-formatted-text"></a>書式設定されたテキストを使用する理由  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には画面にテキストを描画するための複数のコントロールが含まれています。 各コントロールは異なるシナリオを対象にしており、それぞれに一連の機能と制限があります。 一般に、[!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)]の簡単な文など、制限されたテキストのサポートが必要な場合は、<xref:System.Windows.Controls.TextBlock> 要素を使用する必要があります。 <xref:System.Windows.Controls.Label> は、最小限のテキストのサポートが必要な場合に使用できます。 詳細については、「[WPF のドキュメント](documents-in-wpf.md)」を参照してください。  
  
 <xref:System.Windows.Media.FormattedText> オブジェクトは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] テキストコントロールよりもテキストの書式設定機能を提供します。これは、テキストを装飾的な要素として使用する場合に便利です。 詳細については、後の「[書式設定されたテキストのジオメトリへの変換](#converting_formatted_text)」を参照してください。  
  
 また、<xref:System.Windows.Media.FormattedText> オブジェクトは、テキスト指向の <xref:System.Windows.Media.DrawingVisual>派生オブジェクトを作成する場合に便利です。 <xref:System.Windows.Media.DrawingVisual> は、図形、画像、またはテキストを表示するために使用される軽量の描画クラスです。 詳細については、「[DrawingVisuals を使用したヒット テストのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Visual%20Layer/DrawingVisual)」を参照してください。  
  
## <a name="using-the-formattedtext-object"></a>FormattedText オブジェクトの使用  
 書式設定されたテキストを作成するには、<xref:System.Windows.Media.FormattedText.%23ctor%2A> コンストラクターを呼び出して、<xref:System.Windows.Media.FormattedText> オブジェクトを作成します。 最初の書式設定済みテキスト文字列を作成したら、書式スタイルの範囲を適用できます。  
  
 テキストを特定の幅に制限するには、<xref:System.Windows.Media.FormattedText.MaxTextWidth%2A> プロパティを使用します。 テキストは、指定された幅を超えないように自動的に折り返されます。 テキストを特定の高さに制限するには、<xref:System.Windows.Media.FormattedText.MaxTextHeight%2A> プロパティを使用します。 テキストは、指定された高さを超えた部分に省略記号 "…" を表示します。  
  
 ![ワードラップと省略記号付きで表示されるテキスト。](./media/drawing-formatted-text/formatted-text-wordwrap-ellipsis.png)    
  
 複数の書式スタイルを 1 つ以上の文字に適用できます。 たとえば、<xref:System.Windows.Media.FormattedText.SetFontSize%2A> と <xref:System.Windows.Media.FormattedText.SetForegroundBrush%2A> の両方のメソッドを呼び出して、テキスト内の最初の5文字の書式を変更することができます。  
  
 次のコード例では <xref:System.Windows.Media.FormattedText> オブジェクトを作成し、テキストにいくつかの書式スタイルを適用します。  
  
 [!code-csharp[FormattedTextSnippets#FormattedTextSnippets1](~/samples/snippets/csharp/VS_Snippets_Wpf/FormattedTextSnippets/CSharp/Window1.xaml.cs#formattedtextsnippets1)]
 [!code-vb[FormattedTextSnippets#FormattedTextSnippets1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FormattedTextSnippets/visualbasic/window1.xaml.vb#formattedtextsnippets1)]  
  
### <a name="font-size-unit-of-measure"></a>フォント サイズの単位  
 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] アプリケーションの他のテキストオブジェクトと同様に、<xref:System.Windows.Media.FormattedText> オブジェクトは、デバイスに依存しないピクセルを測定単位として使用します。 ただし、ほとんどの Win32 アプリケーションでは、測定単位としてポイントが使用されます。 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] アプリケーションのポイントの単位で表示テキストを使用する場合は、デバイスに依存しない単位 (1/1/96 インチ/ユニット) をポイントに変換する必要があります。 この変換を実行する方法を次のコード例に示します。  
  
 [!code-csharp[FormattedTextSnippets#FormattedTextSnippets2](~/samples/snippets/csharp/VS_Snippets_Wpf/FormattedTextSnippets/CSharp/Window1.xaml.cs#formattedtextsnippets2)]
 [!code-vb[FormattedTextSnippets#FormattedTextSnippets2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FormattedTextSnippets/visualbasic/window1.xaml.vb#formattedtextsnippets2)]  
  
<a name="converting_formatted_text"></a>   
### <a name="converting-formatted-text-to-a-geometry"></a>書式設定されたテキストのジオメトリへの変換  
 書式設定されたテキストを <xref:System.Windows.Media.Geometry> オブジェクトに変換して、その他の種類の視覚的に興味のあるテキストを作成できます。 たとえば、テキスト文字列のアウトラインに基づいて <xref:System.Windows.Media.Geometry> オブジェクトを作成できます。  
  
 ![線形グラデーション ブラシを使用するテキスト アウトライン](./media/typography-in-wpf/text-outline-linear-gradient.jpg)    
  
 変換されたテキストのストローク、塗りつぶし、強調表示を変更して、人の目をひく視覚効果を作成するいくつかの方法を次の例に示します。  
  
 ![塗りつぶしとストロークに別の色を使用するテキスト](./media/typography-in-wpf/fill-stroke-text-effect.jpg)  
  
 ![ストロークに適用されるイメージ ブラシを含むテキスト](./media/typography-in-wpf/image-brush-application.jpg)
  
 ![ストロークと強調表示に適用されたイメージブラシを含むテキスト](./media/typography-in-wpf/image-brush-text-application.jpg)
  
 テキストが <xref:System.Windows.Media.Geometry> オブジェクトに変換されると、文字のコレクションではなくなります。テキスト文字列内の文字を変更することはできません。 ただし、変換されたテキストのストロークおよび塗りつぶしのプロパティを変更することで、テキストの外観を変えることができます。 ストロークは、変換したテキストのアウトラインを参照します。塗りつぶしは、変換したテキストのアウトラインの内側の領域を参照します。 詳細については、「[方法: 中抜きの文字列を作成する](how-to-create-outlined-text.md)」を参照してください。  
  
 書式設定されたテキストを <xref:System.Windows.Media.PathGeometry> オブジェクトに変換し、そのテキストを強調表示するためにオブジェクトを使用することもできます。 たとえば、<xref:System.Windows.Media.PathGeometry> オブジェクトにアニメーションを適用して、書式設定されたテキストのアウトラインに従うようにすることができます。  
  
 次の例は、<xref:System.Windows.Media.PathGeometry> オブジェクトに変換された書式付きテキストを示しています。 アニメーション化された楕円は、レンダリングされたテキストのストロークのパスに従います。  
  
 ![テキストのパス ジオメトリに続く球](./media/drawing-formatted-text/sphere-following-geometry-path.gif)  
テキストのパス ジオメトリに続く球  
  
 詳細については、「[How to: Create a PathGeometry Animation for Text](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms743610(v=vs.100))」(方法: テキストの PathGeometry アニメーションを作成する) を参照してください。  
  
 <xref:System.Windows.Media.PathGeometry> オブジェクトに変換した後は、書式設定されたテキストに対して他の興味深い用途を作成できます。 たとえば、ビデオをクリップしてテキスト内に表示することができます。  
  
 ![テキストのパス ジオメトリに表示されるビデオ](./media/drawing-formatted-text/video-displaying-text-path-geometry.png)
  
<a name="win32_migration"></a>   
## <a name="win32-migration"></a>Win32 の移行  
 テキストを描画するための <xref:System.Windows.Media.FormattedText> の機能は、Win32 DrawText 関数の機能に似ています。 Win32 API から移行する開発者にとって、次の表は、Win32 の DrawText フラグと、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]におけるおおよその等価を示しています。  
  
|DrawText フラグ|同等の WPF 操作|メモ|  
|-------------------|--------------------|-----------|  
|DT_BOTTOM|<xref:System.Windows.Media.FormattedText.Height%2A>|<xref:System.Windows.Media.FormattedText.Height%2A> プロパティを使用して、適切な Win32 DrawText ' y ' 位置を計算します。|  
|DT_CALCRECT|<xref:System.Windows.Media.FormattedText.Height%2A>, <xref:System.Windows.Media.FormattedText.Width%2A>|<xref:System.Windows.Media.FormattedText.Height%2A> と <xref:System.Windows.Media.FormattedText.Width%2A> のプロパティを使用して、出力四角形を計算します。|  
|DT_CENTER|<xref:System.Windows.Media.FormattedText.TextAlignment%2A>|<xref:System.Windows.TextAlignment.Center>に設定された値を使用して、<xref:System.Windows.Media.FormattedText.TextAlignment%2A> プロパティを使用します。|  
|DT_EDITCONTROL|なし|不要。 スペースの幅および最後の行のレンダリングは、フレームワークのエディット コントロールと同じです。|  
|DT_END_ELLIPSIS|<xref:System.Windows.Media.FormattedText.Trimming%2A>|値が <xref:System.Windows.TextTrimming.CharacterEllipsis>の <xref:System.Windows.Media.FormattedText.Trimming%2A> プロパティを使用します。<br /><br /> <xref:System.Windows.TextTrimming.WordEllipsis> を使用して、DT_WORD_ELIPSIS 終了の省略記号で Win32 DT_END_ELLIPSIS を取得します。この場合、文字の省略記号は単一行に収まりきらない単語に対してのみ発生します。|  
|DT_EXPAND_TABS|なし|不要。 タブは自動的に全角 4 文字分ごとに展開されます。これは、言語非依存文字の約 8 文字分の幅です。|  
|DT_EXTERNALLEADING|なし|不要。 外部レディングは、常に行間隔に含まれます。 ユーザー定義の行間を作成するには、<xref:System.Windows.Media.FormattedText.LineHeight%2A> プロパティを使用します。|  
|DT_HIDEPREFIX|なし|サポートされていません。 <xref:System.Windows.Media.FormattedText> オブジェクトを構築する前に、文字列から ' & ' を削除してください。|  
|DT_LEFT|<xref:System.Windows.Media.FormattedText.TextAlignment%2A>|これは、テキストの配置の既定値です。 <xref:System.Windows.TextAlignment.Left>に設定された値を使用して、<xref:System.Windows.Media.FormattedText.TextAlignment%2A> プロパティを使用します。 (WPF のみ)。|  
|DT_MODIFYSTRING|なし|サポートされていません。|  
|DT_NOCLIP|<xref:System.Windows.Media.Visual.VisualClip%2A>|クリッピングは自動的には行われません。 テキストをクリップする場合は、<xref:System.Windows.Media.Visual.VisualClip%2A> プロパティを使用します。|  
|DT_NOFULLWIDTHCHARBREAK|なし|サポートされていません。|  
|DT_NOPREFIX|なし|不要。 文字列内の '&' 文字は、常に通常の文字として扱われます。|  
|DT_PATHELLIPSIS|なし|値が <xref:System.Windows.TextTrimming.WordEllipsis>の <xref:System.Windows.Media.FormattedText.Trimming%2A> プロパティを使用します。|  
|DT_PREFIX|なし|サポートされていません。 アクセラレータキーやリンクなどのテキストにアンダースコアを使用する場合は、<xref:System.Windows.Media.FormattedText.SetTextDecorations%2A> メソッドを使用します。|  
|DT_PREFIXONLY|なし|サポートされていません。|  
|DT_RIGHT|<xref:System.Windows.Media.FormattedText.TextAlignment%2A>|<xref:System.Windows.TextAlignment.Right>に設定された値を使用して、<xref:System.Windows.Media.FormattedText.TextAlignment%2A> プロパティを使用します。 (WPF のみ)。|  
|DT_RTLREADING|<xref:System.Windows.Media.FormattedText.FlowDirection%2A>|<xref:System.Windows.Media.FormattedText.FlowDirection%2A> プロパティを <xref:System.Windows.FlowDirection.RightToLeft> に設定します。|  
|DT_SINGLELINE|なし|不要。 <xref:System.Windows.Media.FormattedText> オブジェクトは、<xref:System.Windows.Media.FormattedText.MaxTextWidth%2A> プロパティが設定されているか、テキストにキャリッジリターン/ラインフィード (CR/LF) が含まれている場合を除き、単一行コントロールとして動作します。|  
|DT_TABSTOP|なし|ユーザー定義のタブ位置はサポートされていません。|  
|DT_TOP|<xref:System.Windows.Media.FormattedText.Height%2A>|不要。 上端揃えが既定値です。 その他の垂直方向の配置値は、<xref:System.Windows.Media.FormattedText.Height%2A> プロパティを使用して、適切な Win32 DrawText ' y ' 位置を計算することによって定義できます。|  
|DT_VCENTER|<xref:System.Windows.Media.FormattedText.Height%2A>|<xref:System.Windows.Media.FormattedText.Height%2A> プロパティを使用して、適切な Win32 DrawText ' y ' 位置を計算します。|  
|DT_WORDBREAK|なし|不要。 単語分割は、<xref:System.Windows.Media.FormattedText> オブジェクトで自動的に行われます。 無効にすることはできません。|  
|DT_WORD_ELLIPSIS|<xref:System.Windows.Media.FormattedText.Trimming%2A>|値が <xref:System.Windows.TextTrimming.WordEllipsis>の <xref:System.Windows.Media.FormattedText.Trimming%2A> プロパティを使用します。|  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Media.FormattedText>
- [WPF のドキュメント](documents-in-wpf.md)
- [WPF のタイポグラフィ](typography-in-wpf.md)
- [中抜きの文字列を作成する](how-to-create-outlined-text.md)
- [方法: テキストの PathGeometry アニメーションを作成する](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms743610(v=vs.100))
