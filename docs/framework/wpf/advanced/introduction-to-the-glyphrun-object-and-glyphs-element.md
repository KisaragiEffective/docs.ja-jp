---
title: GlyphRun オブジェクトと Glyphs 要素の概要
ms.date: 03/30/2017
helpviewer_keywords:
- typography [WPF], Glyphs element
- Glyphs elements [WPF]
- GlyphRun object [WPF]
- text [WPF], glyphs
- glyphs [WPF]
- typography [WPF], GlyphRun object
ms.assetid: 746ca769-a331-4435-9b95-f72a883b67c1
ms.openlocfilehash: 9af07d48877fee0e94f8e5fa2556c4361795df6a
ms.sourcegitcommit: 9a97c76e141333394676bc5d264c6624b6f45bcf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2020
ms.locfileid: "75740356"
---
# <a name="introduction-to-the-glyphrun-object-and-glyphs-element"></a>GlyphRun オブジェクトと Glyphs 要素の概要
このトピックでは、<xref:System.Windows.Media.GlyphRun> オブジェクトと <xref:System.Windows.Documents.Glyphs> 要素について説明します。  

<a name="text_glyphrunovw_intro"></a>   
## <a name="introduction-to-glyphrun"></a>GlyphRun の概要  
 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] は、書式設定後にテキストを傍受して保持する必要がある顧客のために、グリフレベルのマークアップ、<xref:System.Windows.Documents.Glyphs> への直接アクセスを含む高度なテキストサポートを提供します。 これらの機能は、下記のようなシナリオでのさまざまなテキスト レンダリング要件をサポートする不可欠なものです。  
  
1. 固定形式のドキュメントの画面表示。  
  
2. 印刷シナリオ。  
  
    - デバイス プリンター言語としての [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]。  
  
    - Microsoft XPS ドキュメントライター。  
  
    - 以前のプリンタードライバーは、Win32 アプリケーションから固定形式に出力されます。  
  
    - 印刷スプール形式。  
  
3. 以前のバージョンの Windows およびその他のコンピューティングデバイス用のクライアントを含む、固定形式のドキュメント表現。  
  
> [!NOTE]
> <xref:System.Windows.Documents.Glyphs> と <xref:System.Windows.Media.GlyphRun> は、固定形式のドキュメントのプレゼンテーションと印刷のシナリオ向けに設計されています。 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] には、一般的なレイアウトのためのいくつかの要素と、<xref:System.Windows.Controls.Label> や <xref:System.Windows.Controls.TextBlock>などの [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] シナリオが用意されています。 レイアウトと [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] シナリオの詳細については、[WPF のタイポグラフィ](typography-in-wpf.md)を参照してください。  
  
<a name="text_glyphrunovw_glyphrunobject"></a>   
## <a name="the-glyphrun-object"></a>GlyphRun オブジェクト  
 <xref:System.Windows.Media.GlyphRun> オブジェクトは、単一のフォントの1つの書体から1つのサイズで1つの描画スタイルを持つ一連のグリフを表します。  
  
 <xref:System.Windows.Media.GlyphRun> には、グリフ <xref:System.Windows.Documents.Glyphs.Indices%2A> や個々のグリフ位置などのフォントの詳細が含まれます。 また、実行が生成された元の Unicode コードポイント、文字とグリフのバッファーオフセットのマッピング情報、および文字数とグリフごとのフラグも含まれます。  
  
 <xref:System.Windows.Media.GlyphRun> には、対応する高レベル <xref:System.Windows.FrameworkElement>、<xref:System.Windows.Documents.Glyphs>があります。 <xref:System.Windows.Documents.Glyphs> は、要素ツリーおよび [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] マークアップで使用して <xref:System.Windows.Media.GlyphRun> 出力を表すことができます。  
  
<a name="text_glyphrunovw_glyphselement"></a>   
## <a name="the-glyphs-element"></a>Glyphs 要素  
 <xref:System.Windows.Documents.Glyphs> 要素は [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]内の <xref:System.Windows.Media.GlyphRun> の出力を表します。 <xref:System.Windows.Documents.Glyphs> 要素を記述するには、次のマークアップ構文を使用します。  
  
 [!code-xaml[GlyphsOvwSample1#1](~/samples/snippets/csharp/VS_Snippets_Wpf/GlyphsOvwSample1/CS/default.xaml#1)]  
  
 次のプロパティの定義は、サンプルのマークアップ内の最初の 4 つの属性に対応しています。  
  
|property|説明|  
|--------------|-----------------|  
|<xref:System.Windows.Documents.Glyphs.FontUri%2A>|リソース識別子として、ファイル名、Web uniform resource identifier (URI)、またはアプリケーションの .exe またはコンテナー内のリソース参照を指定します。|  
|<xref:System.Windows.Documents.Glyphs.FontRenderingEmSize%2A>|フォント サイズを描画サーフェイスの単位で指定します (既定値は .96 インチ)。|  
|<xref:System.Windows.Documents.Glyphs.StyleSimulations%2A>|太字や斜体のスタイルのフラグを指定します。|  
|<xref:System.Windows.Documents.Glyphs.BidiLevel%2A>|双方向のレイアウト レベルを指定します。 偶数とゼロの値は左から右のレイアウトを意味し、奇数の値は右から左のレイアウトを意味します。|  
  
<a name="text_glyphrunovw_indicesproperty"></a>   
### <a name="indices-property"></a>Indices プロパティ  
 <xref:System.Windows.Documents.Glyphs.Indices%2A> プロパティは、グリフの指定文字列です。 グリフのシーケンスが 1 つのクラスターを形成している場合、クラスター内の最初のグリフの仕様は、クラスターを形成するために組み合わせるグリフの数とコード ポイントの数の仕様によって先行されます。 <xref:System.Windows.Documents.Glyphs.Indices%2A> プロパティは、次のプロパティを1つの文字列で収集します。  
  
- グリフ インデックス  
  
- グリフのアドバンス幅  
  
- グリフアタッチメントベクターの結合  
  
- コードポイントからグリフへのクラスターマッピング  
  
- グリフフラグ  
  
 各グリフの仕様には、次の形式があります。  
  
 `[GlyphIndex][,[Advance][,[uOffset][,[vOffset][,[Flags]]]]]`  
  
<a name="text_glyphrunovw_glyphmetrics"></a>   
## <a name="glyph-metrics"></a>グリフのメトリック  
 各グリフは、他の <xref:System.Windows.Documents.Glyphs>とどのように配置するかを指定するメトリックを定義します。 次の図では、2 つの異なるグリフ文字のさまざまな印刷用品質を定義しています。  
  
 ![グリフの測定値のグリフ単位](./media/glyph-example.png "glyph_example")  
  
<a name="text_glyphrunovw_glyphsmarkup"></a>   
## <a name="glyphs-markup"></a>グリフ マークアップ  
 次のコード例は、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]で <xref:System.Windows.Documents.Glyphs> 要素のさまざまなプロパティを使用する方法を示しています。  
  
 [!code-xaml[GlyphsOvwSamp2#1](~/samples/snippets/csharp/VS_Snippets_Wpf/GlyphsOvwSamp2/CS/default.xaml#1)]  
  
## <a name="see-also"></a>関連項目

- [WPF のタイポグラフィ](typography-in-wpf.md)
- [WPF のドキュメント](documents-in-wpf.md)
- [テキスト](optimizing-performance-text.md)
