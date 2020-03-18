---
title: OpenType フォントの機能
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- fonts [WPF], OpenType
- typography [WPF], OpenType font technology
- OpenType font technology [WPF]
ms.assetid: 4061a9d1-fe8b-4921-9e17-18ec7d2e3ea2
ms.openlocfilehash: 65ecfc4269ff894d45c9b4ee15e349b1a7ddbb73
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094515"
---
# <a name="opentype-font-features"></a>OpenType フォントの機能

このトピックでは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]における OpenType フォントテクノロジの主な機能の概要について説明します。  
  
<a name="overview"></a>   
## <a name="opentype-font-format"></a>OpenType フォントの書式  
 OpenType フォント形式は、TrueType®フォント形式を拡張したもので、PostScript フォントデータのサポートが追加されています。 OpenType フォント形式は、Microsoft と Adobe Corporation によって共同で開発されました。 Opentype フォントと、OpenType フォントをサポートするオペレーティングシステムサービスにより、フォントに TrueType のアウトラインや CFF (PostScript) のアウトラインが含まれているかどうかにかかわらず、ユーザーはフォントを簡単にインストールして使用することができます。  
  
 OpenType フォント形式では、次の開発者の課題に対処します。  
  
- より広範なマルチプラットフォームのサポート。  
  
- 国際文字セットのサポート強化。  
  
- フォント データの保護強化。  
  
- ファイルのサイズを小さくして、より効率的にフォントを配布。  
  
- 高度なテキスト編集コントロールの幅広いサポート。  
  
> [!NOTE]
> Windows SDK には、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] アプリケーションで使用できるサンプルの OpenType フォントのセットが含まれています。 これらのフォントでは、このトピックで説明していく機能の大半が提供されています。 詳細については、「[OpenType フォント パックのサンプル](sample-opentype-font-pack.md)」をご覧ください。  
  
OpenType フォント形式の詳細については、「 [opentype の仕様](https://docs.microsoft.com/typography/opentype/spec/)」を参照してください。  
  
### <a name="advanced-typographic-extensions"></a>高度なテキスト編集の拡張機能  
 高度なタイポグラフィテーブル (OpenType レイアウトテーブル) は、TrueType または CFF のアウトラインを使用してフォントの機能を拡張します。 OpenType レイアウトのフォントには、高品質の国際タイポグラフィをサポートするためにフォントの機能を拡張する追加情報が含まれています。 ほとんどの OpenType フォントは、使用可能な全 OpenType 機能のサブセットのみを公開しています。 OpenType フォントは、次の機能を提供します。  
  
- 合字、位置フォーム、代替、およびその他のフォント置換をサポートする、文字とグリフの間の充実したマッピング。  
  
- 2 次元配置とグリフ結合のサポート。  
  
- フォントには明示的なスクリプトと言語情報が含まれるため、テキスト処理アプリケーションはそれに従って動作を調整可能。  
  
 OpenType レイアウトテーブルの詳細については、OpenType 仕様の[「フォントファイルテーブル」](https://www.microsoft.com/typography/otspec/otff.htm)セクションを参照してください。  
  
 この概要の残りの部分では、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティによって公開される、視覚的に興味深い OpenType 機能の幅と柔軟性を紹介します。 このオブジェクトの詳細については、「[タイポグラフィ クラス](#typography_class)」を参照してください。  
  
<a name="variants"></a>   
## <a name="variants"></a>バリエーション  
 バリアントを使用して、上付き文字と下付きなどのさまざまなタイポグラフィ スタイルを表示します。  
  
### <a name="superscripts-and-subscripts"></a>上付き/下付きの文字  
 <xref:System.Windows.Documents.Typography.Variants%2A> プロパティを使用すると、OpenType フォントの上付き文字と下付き文字の値を設定できます。  
  
 次のテキストは、Palatino Linotype フォントの上付き文字を示したものです。  
  
 ![OpenType の上付き文字を使用するテキスト](./media/opentype-font-features/opentype-superscripts.gif "OpenType の上付き文字を使用するテキスト")  
  
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Linotype フォントの上付き文字を定義する方法を示しています。  
  
 [!code-xaml[OpenTypeFontSamples#12](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#12)]  
  
 次のテキストは、Linotype フォントの添字を示しています。  
  
 ![OpenType の下付き文字を使用するテキスト](./media/opentype-font-features/opentype-subscripts.gif "OpenType の下付き文字を使用するテキスト")  
  
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Linotype フォントの添字を定義する方法を示しています。  
  
 [!code-xaml[OpenTypeFontSamples#13](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#13)]  
  
### <a name="decorative-uses-of-superscripts-and-subscripts"></a>上付き文字と下付き文字の装飾的な用途  
 上付き文字と下付き文字を使用して、大文字と小文字が混在したテキストに装飾的効果をつけることもできます。 次のテキストは、Palatino Linotype フォントの上付き文字と下付き文字を示したものです。 大文字には影響がないことに注目してください。  
  
 ![OpenType の上付き文字と下付き文字を使用するテキスト](./media/opentype-font-features/opentype-superscripts-subscripts.gif "OpenType の上付き文字と下付き文字を使用するテキスト")  

 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、フォントの上付き文字と下付き文字を定義する方法を示しています。  
  
 [!code-xaml[OpenTypeFontSamples#14](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#14)]  
  
<a name="capitals"></a>   
## <a name="capitals"></a>大文字  
 大文字は、大文字スタイルのグリフでテキストをレンダリングするタイポグラフィ形式のセットです。 通常、テキストをすべて大文字で表示すると、文字間隔が狭すぎるように見え、文字の印象と縦横比が重すぎるように感じられます。 OpenType では、小型英大文字、超小型英大文字、タイトル、および大文字の間隔など、大文字のスタイル形式をいくつかサポートしています。 これらのスタイル形式を使用して、英大文字の外観を変えることができます。  
  
 次のテキストは、Pescadero フォントの標準の大文字と、その後に "SmallCaps" および "AllSmallCaps" のスタイルをあてた文字を示したものです。 この場合、3つのすべての単語に同じフォントサイズが使用されます。  
  
 ![OpenType 大文字を使用するテキスト](./media/opentype-font-features/opentype-capitals.gif "OpenType の大文字を使用するテキスト")  
  
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Pescadero フォントの大文字を定義する方法を示しています。 "SmallCaps" 形式を使用する場合は、先頭の大文字は無視されます。  
  
 [!code-xaml[OpenTypeFontSamples#9](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#9)]  
  
### <a name="titling-capitals"></a>タイトル用大文字  
 タイトル用大文字は、重みと縦横比が軽く、標準の大文字よりも洗練された印象を与えるように設計されています。 タイトルの大文字は通常、見出しとして大きいフォントサイズで使用されます。 次のテキストは、Pescadero フォントの標準とタイトルの大文字を示しています。 2行目のテキストの幅が狭いことに注目してください。  
  
 ![OpenType のタイトル大文字を使用するテキスト](./media/opentype-font-features/opentype-titling-capitals.gif "OpenType のタイトル大文字を使用するテキスト")  
  
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Pescadero フォントのタイトル用大文字を定義する方法を示しています。  
  
 [!code-xaml[OpenTypeFontSamples#OpenTypeFontSnippet17](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#opentypefontsnippet17)]  
  
### <a name="capital-spacing"></a>大文字スペーシング  
 大文字スペーシングは、テキストをすべて大文字にする場合に間隔を広くする機能です。 大文字は、通常、小文字で合成するように設計されています。 大文字が使用されている場合は、大文字と小文字の間に空白が表示されることがあります。 次のテキストは、Pescadero フォントの標準と大文字の間隔を示しています。  
  
 ![OpenType の大文字スペーシングを使用するテキスト](./media/opentype-font-features/opentype-capital-spacing.gif "OpenType の大文字スペーシングを使用するテキスト")  
 
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Pescadero フォントの大文字と小文字の間隔を定義する方法を示しています。  
  
 [!code-xaml[OpenTypeFontSamples#OpenTypeFontSnippet18](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#opentypefontsnippet18)]  
  
<a name="ligatures"></a>   
## <a name="ligatures"></a>合字  
 合字とは、2 つ以上のグリフを、より読みやすい、あるいはより魅力的なテキストにするために、1 つのグリフに形成したものです。 OpenType フォントでは、次の4種類の合字がサポートしています。  
  
- **標準合字**。 読みやすさを高めるように設計されています。 標準合字には、"fi"、"fl" および "ff" があります。  
  
- **コンテキスト合字**。 合字を構成する文字の間の結合を向上させることによって、読みやすさを高めるよう設計されています。  
  
- **随意合字**。 装飾を加える設計で、特に読みやすくなるようには設計されていません。  
  
- **歴史的合字**。 歴史的な記述に相応しい設計で、特に読みやすくなるようには設計されていません。  
  
 次のテキストは、Pericles フォントの標準合字グリフを示したものです。  
  
 ![OpenType の標準合字を使用するテキスト](./media/opentype-font-features/opentype-standard-ligatures.gif "OpenType の標準合字を使用するテキスト")  
  
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Pericles フォントの標準合字グリフを定義する方法を示しています。  
  
 [!code-xaml[OpenTypeFontSamples#4](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#4)]  
  
 次のテキストは、Pericles フォントの随意合字グリフを示したものです。  
  
 ![OpenType の随意合字を使用するテキスト](./media/opentype-font-features/opentype-discretionary-ligatures.gif "OpenType の随意合字を使用するテキスト")  
  
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Pericles フォントの随意合字のグリフを定義する方法を示しています。  
  
 [!code-xaml[OpenTypeFontSamples#5](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#5)]  
  
 既定では、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] の OpenType フォントによって、標準合字が有効になります。 たとえば、Palatino Linotype フォントを使用する場合、標準合字 "fi"、"ff" および "fl" は組み合わせ文字グリフとして表示されます。 各標準合字の文字ペアが相互に接していることに注意してください。  
  
 ![OpenType の標準合字と、Linotype Atino を使用するテキスト](./media/opentype-font-features/opentype-standard-ligatures-palatino.gif "OpenType の標準合字と、Linotype Atino を使用するテキスト")    
   
 ただし、標準合字機能を無効にして、"ff" などの標準合字が、組み合わせ文字グリフとしてではなく、2 つの別々のグリフとして表示されるようにできます。  
  
 ![OpenType の無効な標準合字を使用するテキスト](./media/opentype-font-features/disabled-opentype-standard-ligatures.gif "OpenType の無効な標準合字を使用するテキスト")  
    
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Linotype フォントの標準合字グリフを無効にする方法を示しています。  
  
 [!code-xaml[OpenTypeFontSamples#6](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#6)]  
  
<a name="swashes"></a>   
## <a name="swashes"></a>飾り付き  
 飾り付きは装飾的なグリフで、カリグラフィを連想させる、手の込んだ装飾が使用されます。 次のテキストは、Pescadero フォントの標準グリフと巻きひげグリフを示しています。  
  
 ![OpenType の標準グリフとスワッシュ グリフを使用するテキスト](./media/opentype-font-features/opentype-standard-swash-glyphs.gif "OpenType の標準グリフと飾り付きグリフを使用するテキスト")  

 飾り付きは、季節のご挨拶などの短いフレーズで装飾的な要素としてよく使用されます。 次のテキストでは、スワッシュを使用して、イベントの名前の大文字を強調しています。  
  
 ![OpenType の飾り付きを使用するテキスト](./media/opentype-font-features/opentype-swashes.gif "OpenType の巻き髭を使用するテキスト")  
  
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、フォントのスワッシュ文字を定義する方法を示しています。  
  
 [!code-xaml[OpenTypeFontSamples#7](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#7)]  
  
### <a name="contextual-swashes"></a>コンテキスト飾り付き  
 飾り付きグリフの特定の組み合わせでは、隣りあう文字の下に延びる部分が重なり合うなど、美しくない外観になる可能性があります。 コンテキスト巻きひげを使用すると、より良い外観を生成する代替のスワッシュ字形を使用できます。 次のテキストは、コンテキストの巻きひげが適用される前後の同じ単語を示しています。  
  
 ![OpenType のコンテキスト飾り付きを使用するテキスト](./media/opentype-font-features/opentype-contextual-swashes.gif "OpenType のコンテキスト巻き髭を使用するテキスト")  
  
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Pescadero フォントのコンテキスト巻きひげを定義する方法を示しています。  
  
 [!code-xaml[OpenTypeFontSamples#OpenTypeFontSnippet16](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#opentypefontsnippet16)]  
  
<a name="alternates"></a>   
## <a name="alternates"></a>代替  
 代替文字は、標準的なグリフの代わりに使用できるグリフです。 次の例で使用する Pericles フォントなどの OpenType フォントには、テキストのさまざまな外観を作成するために使用できる代替グリフを含めることができます。 次のテキストは、Pericles フォントの標準グリフを示したものです。  
  
 ![OpenType の標準グリフを使用するテキスト](./media/opentype-font-features/opentype-standard-glyphs.gif "OpenType の標準グリフを使用するテキスト")  

 Pericles OpenType フォントには、グリフの標準セットに対してスタイルの代替を提供する追加のグリフが含まれています。 次のテキストでは、スタイル代替グリフが表示されています。  
  
 ![OpenType のスタイル代替グリフを使用するテキスト](./media/opentype-font-features/opentype-stylistic-alternate-glyphs.gif "OpenType のスタイル代替グリフを使用するテキスト")  
  
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Pericles フォント用のスタイルの代替グリフを定義する方法を示しています。  
  
 [!code-xaml[OpenTypeFontSamples#2](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#2)]  
  
 次のテキストは、Pericles フォントの他のスタイルの代替グリフをいくつか表示します。  
  
 ![OpenType のスタイルを使用したテキスト Pericles フォントの代替グリフ](./media/opentype-font-features/opentype-stylistic-alternate-glyphs-pericles.gif "OpenType のスタイルを使用したテキスト Pericles フォントの代替グリフ")

 次のマークアップの例では、これらの他のスタイル代替グリフを定義する方法を示します。  
  
 [!code-xaml[OpenTypeFontSamples#3](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#3)]  
  
### <a name="random-contextual-alternates"></a>ランダムなコンテキスト代替  
 ランダムなコンテキスト代替は、単一文字に複数の代替グリフを提供します。 この機能は、スクリプト型のフォントを使用して実装された場合、ランダムに選択されたグリフのセットを使用して、外観がわずかに異なる、手書きをシミュレートできます。 次のテキストでは、Lindsey フォントにランダムなコンテキスト代替が使用されています。 文字 "a" の外観が少し異なることに注意してください。  
  
 ![OpenType のランダムなコンテキスト代替を使用するテキスト](./media/opentype-font-features/opentype-random-contextual-alternates.gif "OpenType のランダムなコンテキスト代替を使用するテキスト")  
  
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Lindsey フォントのランダムなコンテキスト代替を定義する方法を示しています。  
  
 [!code-xaml[OpenTypeFontSamples#OpenTypeFontSnippet20](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/Window1.xaml#opentypefontsnippet20)]  
  
### <a name="historical-forms"></a>歴史的形式  
 歴史的形式は、過去に一般的であった表示形式です。 次のテキストは、"ボストン, マサチューセッツ" という語句を示しています。これは、Linotype フォントの履歴形式のグリフを使用しています。  
  
 ![OpenType の歴史的形式を使用するテキスト](./media/opentype-font-features/opentype-historical-forms.gif "OpenType の履歴フォームを使用するテキスト")  
   
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Linotype フォントの履歴形式を定義する方法を示しています。  
  
 [!code-xaml[OpenTypeFontSamples#8](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#8)]  
  
<a name="numerical_styles"></a>   
## <a name="numerical-styles"></a>数値スタイル  
 OpenType フォントでは、テキスト内の数値に使用できる数多くの機能をサポートします。  
  
### <a name="fractions"></a>小数  
 OpenType フォントは、スラッシュや積み上げなど、分数のスタイルをサポートしています。  
  
 次のテキストは、Palatino Linotype フォントの小数スタイルを示したものです。  
  
 ![OpenType の分数 (横) と分数 (縦) を使用するテキスト](./media/opentype-font-features/opentype-slashed-stacked-fractions.gif "OpenType の分数 (横) と分数 (縦) を使用するテキスト")  
   
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Linotype フォントの分数のスタイルを定義する方法を示しています。  
  
 [!code-xaml[OpenTypeFontSamples#10](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#10)]  
  
### <a name="old-style-numerals"></a>旧式スタイルの数字  
 OpenType フォントは、古いスタイルの数字形式をサポートしています。 この形式は、もはや標準ではなくなったスタイルで数字を表示するのに便利です。 次のテキストは、Linotype フォントの標準形式と古いスタイルの数字形式で、18世紀の日付を示しています。  
  
 ![OpenType の旧式スタイルの数字を使用するテキスト](./media/opentype-font-features/opentype-old-style-numerals.gif "OpenType の古いスタイルの数字を使用するテキスト")  
    
 次のテキストは、Palatino Linotype フォントの標準の数字と、旧式スタイルの数字を示したものです。  
  
 ![OpenType の旧式スタイルの数字セットを使用するテキスト](./media/opentype-font-features/opentype-old-style-numeral-sets.gif "OpenType の古いスタイルの数字セットを使用するテキスト")  
  
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Linotype フォントの古いスタイルの数字を定義する方法を示しています。  
  
 [!code-xaml[OpenTypeFontSamples#11](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#11)]  
  
### <a name="proportional-and-tabular-figures"></a>プロポーショナルと表形式の数字  
 OpenType フォントでは、数字を使用するときの幅の調整を制御するための、プロポーショナルおよび表形式の図の機能がサポートされています。 プロポーショナルの数字では、それぞれの数字は異なる幅を持つものとして扱われます。たとえば "1" は "5" より狭い幅です。 表形式の数値は、幅が均等になるように処理されるため、財務の種類の情報が読みやすくなります。  
  
 次のテキストは、Miramonte フォントを使用した最初の列の2つの比例する数値を示しています。 数字 "5" と "1" の間の幅の違いに注意してください。 2番目の列には、同じ2つの数値が表形式の図機能を使用して調整された幅と共に表示されます。  
  
 ![OpenType のプロポーショナルと表形式の数字を使用するテキスト](./media/opentype-font-features/opentype-proportional-tabular-figures.gif "OpenType のプロポーショナルと表形式の数字を使用するテキスト")  
    
 次のマークアップの例では、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Miramonte フォントのプロポーショナルと表形式の数値を定義する方法を示します。  
  
 [!code-xaml[OpenTypeFontSamples#OpenTypeFontSnippet19](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/Window1.xaml#opentypefontsnippet19)]  
  
### <a name="slashed-zero"></a>スラッシュ付きゼロ  
 OpenType フォントは、文字 "O" と数字 "0" の違いを強調するために、スラッシュゼロの数字形式をサポートしています。 スラッシュ付きゼロは、財務およびビジネス情報における ID によく使用されます。  
  
 次のテキストは、Miramonte フォントを使用したサンプル注文識別子を示しています。 最初の行では、標準の数字を使用します。 2番目の行では、大文字の "O" 文字とのコントラストを上げるためにスラッシュゼロの数字を使用しています。  
  
 ![OpenType のスラッシュ付きのゼロを使用するテキスト](./media/opentype-font-features/opentype-slashed-zero-numerals.gif "OpenType のスラッシュ付きのゼロを使用するテキスト")  
    
 次のマークアップの例では、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Miramonte フォントのスラッシュ付きゼロの数字を定義する方法を示します。  
  
 [!code-xaml[OpenTypeFontSamples#OpenTypeFontSnippet15](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#opentypefontsnippet15)]  
  
<a name="typography_class"></a>   
## <a name="typography-class"></a>タイポグラフィ クラス  
 <xref:System.Windows.Documents.Typography> オブジェクトは、OpenType フォントがサポートする一連の機能を公開します。 マークアップで <xref:System.Windows.Documents.Typography> のプロパティを設定することにより、OpenType 機能を利用するドキュメントを簡単に作成できます。  
  
 次のテキストは、Pescadero フォントの標準の大文字と、その後に "SmallCaps" および "AllSmallCaps" のスタイルをあてた文字を示したものです。 この場合、3つのすべての単語に同じフォントサイズが使用されます。  
  
 ![OpenType 大文字を使用するテキスト](./media/opentype-font-features/opentype-capitals.gif "OpenType の大文字を使用するテキスト")  
    
 次のマークアップの例は、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティを使用して、Pescadero フォントの大文字を定義する方法を示しています。 "SmallCaps" 形式を使用する場合は、先頭の大文字は無視されます。  
  
 [!code-xaml[OpenTypeFontSamples#9](~/samples/snippets/csharp/VS_Snippets_Wpf/OpenTypeFontSamples/CS/PageOne.xaml#9)]  
  
 次のコード例では、前のマークアップの例と同じタスクを実行します。  
  
 [!code-csharp[TypographyCodeSnippets#TypographyCodeSnippet1](~/samples/snippets/csharp/VS_Snippets_Wpf/TypographyCodeSnippets/CSharp/Page1.xaml.cs#typographycodesnippet1)]
 [!code-vb[TypographyCodeSnippets#TypographyCodeSnippet1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TypographyCodeSnippets/visualbasic/page1.xaml.vb#typographycodesnippet1)]  
  
### <a name="typography-class-properties"></a>タイポグラフィ クラスのプロパティ  
 次の表に、<xref:System.Windows.Documents.Typography> オブジェクトのプロパティ、値、および既定の設定を示します。  
  
|プロパティ|値|Default value|  
|--------------|----------------|-------------------|  
|<xref:System.Windows.Documents.Typography.AnnotationAlternates%2A>|数値 - バイト|0|  
|<xref:System.Windows.Documents.Typography.Capitals%2A>|<xref:System.Windows.FontCapitals.AllPetiteCaps> &#124; <xref:System.Windows.FontCapitals.AllSmallCaps> &#124; <xref:System.Windows.FontCapitals.Normal> &#124; <xref:System.Windows.FontCapitals.PetiteCaps> &#124; <xref:System.Windows.FontCapitals.SmallCaps> &#124; <xref:System.Windows.FontCapitals.Titling> &#124; <xref:System.Windows.FontCapitals.Unicase>|<xref:System.Windows.FontCapitals.Normal?displayProperty=nameWithType>|  
|<xref:System.Windows.Documents.Typography.CapitalSpacing%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.CaseSensitiveForms%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.ContextualAlternates%2A>|<xref:System.Boolean>|`true`|  
|<xref:System.Windows.Documents.Typography.ContextualLigatures%2A>|<xref:System.Boolean>|`true`|  
|<xref:System.Windows.Documents.Typography.ContextualSwashes%2A>|数値 - バイト|0|  
|<xref:System.Windows.Documents.Typography.DiscretionaryLigatures%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.EastAsianExpertForms%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.EastAsianLanguage%2A>|<xref:System.Windows.FontEastAsianLanguage.HojoKanji> &#124; <xref:System.Windows.FontEastAsianLanguage.Jis04> &#124; <xref:System.Windows.FontEastAsianLanguage.Jis78> &#124; <xref:System.Windows.FontEastAsianLanguage.Jis83> &#124; <xref:System.Windows.FontEastAsianLanguage.Jis90> &#124; &#124; &#124; <xref:System.Windows.FontEastAsianLanguage.NlcKanji> <xref:System.Windows.FontEastAsianLanguage.Normal> <xref:System.Windows.FontEastAsianLanguage.Simplified> <xref:System.Windows.FontEastAsianLanguage.Traditional> &#124; &#124; <xref:System.Windows.FontEastAsianLanguage.TraditionalNames>|<xref:System.Windows.FontEastAsianLanguage.Normal?displayProperty=nameWithType>|  
|<xref:System.Windows.Documents.Typography.EastAsianWidths%2A>|<xref:System.Windows.FontEastAsianWidths.Full> &#124; <xref:System.Windows.FontEastAsianWidths.Half> &#124; <xref:System.Windows.FontEastAsianWidths.Normal> &#124; <xref:System.Windows.FontEastAsianWidths.Proportional> &#124; <xref:System.Windows.FontEastAsianWidths.Quarter> &#124; <xref:System.Windows.FontEastAsianWidths.Third>|<xref:System.Windows.FontEastAsianWidths.Normal?displayProperty=nameWithType>|  
|<xref:System.Windows.Documents.Typography.Fraction%2A>|<xref:System.Windows.FontFraction.Normal> &#124; <xref:System.Windows.FontFraction.Slashed> &#124; <xref:System.Windows.FontFraction.Stacked>|<xref:System.Windows.FontFraction.Normal?displayProperty=nameWithType>|  
|<xref:System.Windows.Documents.Typography.HistoricalForms%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.HistoricalLigatures%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.Kerning%2A>|<xref:System.Boolean>|`true`|  
|<xref:System.Windows.Documents.Typography.MathematicalGreek%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.NumeralAlignment%2A>|<xref:System.Windows.FontNumeralAlignment.Normal> &#124; <xref:System.Windows.FontNumeralAlignment.Proportional> &#124; <xref:System.Windows.FontNumeralAlignment.Tabular>|<xref:System.Windows.FontNumeralAlignment.Normal?displayProperty=nameWithType>|  
|<xref:System.Windows.Documents.Typography.NumeralStyle%2A>|<xref:System.Boolean>|<xref:System.Windows.FontNumeralStyle.Normal?displayProperty=nameWithType>|  
|<xref:System.Windows.Documents.Typography.SlashedZero%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StandardLigatures%2A>|<xref:System.Boolean>|`true`|  
|<xref:System.Windows.Documents.Typography.StandardSwashes%2A>|数値 - バイト|0|  
|<xref:System.Windows.Documents.Typography.StylisticAlternates%2A>|数値 - バイト|0|  
|<xref:System.Windows.Documents.Typography.StylisticSet1%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet2%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet3%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet4%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet5%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet6%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet7%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet8%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet9%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet10%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet11%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet12%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet13%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet14%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet15%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet16%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet17%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet18%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet19%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.StylisticSet20%2A>|<xref:System.Boolean>|`false`|  
|<xref:System.Windows.Documents.Typography.Variants%2A>|<xref:System.Windows.FontVariants.Inferior> &#124; <xref:System.Windows.FontVariants.Normal> &#124; <xref:System.Windows.FontVariants.Ordinal> &#124; <xref:System.Windows.FontVariants.Ruby> &#124; <xref:System.Windows.FontVariants.Subscript> &#124; <xref:System.Windows.FontVariants.Superscript>|<xref:System.Windows.FontVariants.Normal?displayProperty=nameWithType>|  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Documents.Typography>
- [OpenType 仕様](https://docs.microsoft.com/typography/opentype/spec/)
- [WPF のタイポグラフィ](typography-in-wpf.md)
- [OpenType フォント パックのサンプル](sample-opentype-font-pack.md)
- [アプリケーションでのフォントのパッケージング](packaging-fonts-with-applications.md)
