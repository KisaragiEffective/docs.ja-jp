---
title: テキストの高度な書式設定
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- formatting [WPF]
- text [WPF]
- typography [WPF], text formatting
ms.assetid: f0a7986e-f5b2-485c-a27d-f8e922022212
ms.openlocfilehash: 26ee3c2b8a431200c3dc04130deb2247b6c1446d
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095243"
---
# <a name="advanced-text-formatting"></a>テキストの高度な書式設定
Windows Presentation Foundation (WPF) は、アプリケーションにテキストを含めるための堅牢な Api のセットを提供します。 レイアウトと [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] Api (<xref:System.Windows.Controls.TextBlock>など) は、テキスト表示に最も一般的で一般的な要素を提供します。 <xref:System.Windows.Media.GlyphRunDrawing> や <xref:System.Windows.Media.FormattedText>などの描画 Api は、書式設定されたテキストを描画に含めるための手段を提供します。 最も高度なレベルでは、テキストストア管理、テキストラン書式設定管理、埋め込みオブジェクト管理など、テキスト表示のあらゆる側面を制御するための拡張テキスト書式設定エンジンが WPF に用意されています。  
  
 このトピックでは、WPF テキストの書式設定の概要について説明します。 クライアントの実装と WPF テキスト書式設定エンジンの使用に焦点を当てています。  
  
> [!NOTE]
> このドキュメント内のすべてのコード例は、[高度なテキストの書式設定のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/PerMonitorDPI/TextFormatting)に記載されています。  

<a name="prereq"></a>   
## <a name="prerequisites"></a>前提条件  
 このトピックでは、テキスト表示に使用される高レベルの Api について理解していることを前提としています。 ほとんどのユーザーシナリオでは、このトピックで説明する高度なテキスト書式設定 Api は必要ありません。 さまざまなテキスト Api の概要については、「 [WPF のドキュメント](documents-in-wpf.md)」を参照してください。  
  
<a name="section1"></a>   
## <a name="advanced-text-formatting"></a>テキストの高度な書式設定  
 WPF のテキストレイアウトと [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] コントロールには、書式設定されたテキストをアプリケーションに簡単に含めることができる書式設定プロパティが用意されています。 これらのコントロールでは、テキストの表示を処理するさまざまなプロパティが公開されます。書体、大きさ、色などです。 通常の状況では、これらのコントロールはアプリケーションの大半のテキスト表示を処理できます。 しかしながら、一部の高度なシナリオでは、テキスト表示と同様にテキスト保存を制御する必要があります。 WPF には、この目的のための拡張テキスト書式設定エンジンが用意されています。  
  
 WPF の高度なテキスト書式設定機能は、テキストの書式設定エンジン、テキストストア、テキストラン、および書式設定のプロパティで構成されています。 テキストの書式設定エンジン <xref:System.Windows.Media.TextFormatting.TextFormatter>は、プレゼンテーションに使用されるテキストの行を作成します。 これは、行の書式設定プロセスを開始し、テキストフォーマッタの <xref:System.Windows.Media.TextFormatting.TextFormatter.FormatLine%2A>を呼び出すことで実現されます。 テキストフォーマッタは、ストアの <xref:System.Windows.Media.TextFormatting.TextSource.GetTextRun%2A> メソッドを呼び出すことによって、テキストストアからテキストランを取得します。 次に、<xref:System.Windows.Media.TextFormatting.TextRun> オブジェクトがテキストフォーマッタによって <xref:System.Windows.Media.TextFormatting.TextLine> オブジェクトに形成され、アプリケーションに対して検査または表示されます。  
  
<a name="section2"></a>   
## <a name="using-the-text-formatter"></a>テキスト フォーマッタを使用する  
 <xref:System.Windows.Media.TextFormatting.TextFormatter> は、WPF テキスト書式設定エンジンであり、テキスト行の書式設定と区切りを行うためのサービスを提供します。 テキスト フォーマッタは、さまざまなテキスト文字書式や段落スタイルを処理し、国際的なテキスト レイアウトに対応しています。  
  
 従来のテキスト API とは異なり、<xref:System.Windows.Media.TextFormatting.TextFormatter> は、一連のコールバックメソッドを使用してテキストレイアウトクライアントと対話します。 クライアントは、<xref:System.Windows.Media.TextFormatting.TextSource> クラスの実装でこれらのメソッドを提供する必要があります。 次の図は、クライアントアプリケーションと <xref:System.Windows.Media.TextFormatting.TextFormatter>間のテキストレイアウトの相互作用を示しています。  
  
 ![テキスト レイアウト クライアントと TextFormatter のダイアグラム](./media/advanced-text-formatting/text-layout-textformatter-interaction.png)  
  
 テキストフォーマッタは、<xref:System.Windows.Media.TextFormatting.TextSource>の実装であるテキストストアから書式設定されたテキスト行を取得するために使用します。 これを行うには、最初に <xref:System.Windows.Media.TextFormatting.TextFormatter.Create%2A> メソッドを使用してテキストフォーマッタのインスタンスを作成します。 このメソッドはテキスト フォーマッタのインスタンスを作成し、行の高さと幅の最大値を設定します。 テキストフォーマッタのインスタンスが作成されるとすぐに、<xref:System.Windows.Media.TextFormatting.TextFormatter.FormatLine%2A> メソッドを呼び出すことによって、行の作成プロセスが開始されます。 <xref:System.Windows.Media.TextFormatting.TextFormatter> は、テキストソースにコールバックしてテキストを取得し、行を形成するテキストの実行のパラメーターを書式設定します。  
  
 次の例は、テキスト ストアを書式設定するプロセスを示しています。 <xref:System.Windows.Media.TextFormatting.TextFormatter> オブジェクトは、テキストストアからテキスト行を取得し、<xref:System.Windows.Media.DrawingContext>に描画するためのテキスト行を書式設定するために使用されます。  
  
 [!code-csharp[TextFormatterExample#100](~/samples/snippets/csharp/VS_Snippets_Wpf/TextFormatterExample/CSharp/Window1.xaml.cs#100)]
 [!code-vb[TextFormatterExample#100](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TextFormatterExample/VisualBasic/Window1.xaml.vb#100)]  
  
<a name="section3"></a>   
## <a name="implementing-the-client-text-store"></a>クライアント テキスト ストアを実装する  
 テキスト書式設定エンジンを拡張するとき、テキスト ストアのあらゆる側面を実装し、管理する必要があります。 これは簡単な作業ではありません。 テキスト ストアは、テキスト ランのプロパティ、段落のプロパティ、埋め込みオブジェクト、その他の同様のコンテンツの追跡を担当します。 また、テキストフォーマッタに個々の <xref:System.Windows.Media.TextFormatting.TextRun> オブジェクトを提供し、テキストフォーマッタが <xref:System.Windows.Media.TextFormatting.TextLine> オブジェクトを作成するために使用します。  
  
 テキストストアの仮想化を処理するには、テキストストアを <xref:System.Windows.Media.TextFormatting.TextSource>から派生させる必要があります。 <xref:System.Windows.Media.TextFormatting.TextSource> テキストストアからテキストランを取得するためにテキストフォーマッタが使用するメソッドを定義します。 <xref:System.Windows.Media.TextFormatting.TextSource.GetTextRun%2A> は、テキストフォーマッタが行の書式設定で使用されるテキストランを取得するために使用するメソッドです。 <xref:System.Windows.Media.TextFormatting.TextSource.GetTextRun%2A> の呼び出しは、次のいずれかの条件が発生するまで、テキストフォーマッタによって繰り返し実行されます。  
  
- <xref:System.Windows.Media.TextFormatting.TextEndOfLine> またはサブクラスが返されます。  
  
- テキストランの累積幅が、テキストフォーマッタを作成するための呼び出しまたはテキストフォーマッタの <xref:System.Windows.Media.TextFormatting.TextFormatter.FormatLine%2A> メソッドの呼び出しのいずれかで指定された最大線幅を超えています。  
  
- "CF"、"LF"、"CRLF" などの Unicode 改行シーケンスが返されます。  
  
<a name="section4"></a>   
## <a name="providing-text-runs"></a>テキスト ランを提供する  
 テキスト書式設定プロセスの中心となるものは、テキスト フォーマッタとテキスト ストアの間のやりとりです。 <xref:System.Windows.Media.TextFormatting.TextSource> の実装では、テキストフォーマッタに <xref:System.Windows.Media.TextFormatting.TextRun> オブジェクトと、テキストランの書式設定に使用するプロパティを提供します。 この相互作用は、テキストフォーマッタによって呼び出される <xref:System.Windows.Media.TextFormatting.TextSource.GetTextRun%2A> メソッドによって処理されます。  
  
 次の表は、定義済みの <xref:System.Windows.Media.TextFormatting.TextRun> オブジェクトの一部を示しています。  
  
|TextRun の種類|使用法|  
|------------------|-----------|  
|<xref:System.Windows.Media.TextFormatting.TextCharacters>|文字グリフの表示をテキスト フォーマッタに返すために使用される特殊なテキスト ラン。|  
|<xref:System.Windows.Media.TextFormatting.TextEmbeddedObject>|テキスト内のボタンやイメージなど、測定、ヒット テスト、描画が全部行われるコンテンツを提供するための特殊なテキスト ラン。|  
|<xref:System.Windows.Media.TextFormatting.TextEndOfLine>|行の終わりをマークするための特殊なテキスト ラン。|  
|<xref:System.Windows.Media.TextFormatting.TextEndOfParagraph>|段落の終わりをマークするための特殊なテキスト ラン。|  
|<xref:System.Windows.Media.TextFormatting.TextEndOfSegment>|セグメントの末尾を示すために使用される特殊なテキストラン。たとえば、前の <xref:System.Windows.Media.TextFormatting.TextModifier> 実行の影響を受けるスコープを終了するために使用します。|  
|<xref:System.Windows.Media.TextFormatting.TextHidden>|隠れ文字の範囲をマークするための特殊なテキスト ラン。|  
|<xref:System.Windows.Media.TextFormatting.TextModifier>|その範囲でテキスト ランのプロパティを変更するための特殊なテキスト ラン。 スコープは、次に一致する <xref:System.Windows.Media.TextFormatting.TextEndOfSegment> テキストラン、または次の <xref:System.Windows.Media.TextFormatting.TextEndOfParagraph>に一致します。|  
  
 定義済みの <xref:System.Windows.Media.TextFormatting.TextRun> オブジェクトはすべて、サブクラス化できます。 それにより、テキスト ソースはテキスト フォーマッタにカスタム データを含むテキスト ランを提供できます。  
  
 <xref:System.Windows.Media.TextFormatting.TextSource.GetTextRun%2A> メソッドの例を次に示します。 このテキストストアは、処理のためにテキストフォーマッタに <xref:System.Windows.Media.TextFormatting.TextRun> オブジェクトを返します。  
  
 [!code-csharp[TextFormatterExample#101](~/samples/snippets/csharp/VS_Snippets_Wpf/TextFormatterExample/CSharp/CustomTextSource.cs#101)]
 [!code-vb[TextFormatterExample#101](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TextFormatterExample/VisualBasic/CustomTextSource.vb#101)]  
  
> [!NOTE]
> この例では、テキスト ストアはすべてのテキストに同じテキスト プロパティを提供します。 高度なテキスト ストアでは、場合によっては、独自のスパン管理を実装し、個々の文字に異なるプロパティを与えるようにする必要があります。  
  
<a name="section5"></a>   
## <a name="specifying-formatting-properties"></a>書式設定プロパティを指定する  
 <xref:System.Windows.Media.TextFormatting.TextRun> オブジェクトは、テキストストアによって提供されるプロパティを使用して書式設定されます。 これらのプロパティには、<xref:System.Windows.Media.TextFormatting.TextParagraphProperties> と <xref:System.Windows.Media.TextFormatting.TextRunProperties>の2種類があります。 <xref:System.Windows.TextAlignment> や <xref:System.Windows.FlowDirection>などの段落包括プロパティを <xref:System.Windows.Media.TextFormatting.TextParagraphProperties> ハンドルします。 <xref:System.Windows.Media.TextFormatting.TextRunProperties> は、前景ブラシ、<xref:System.Windows.Media.Typeface>、フォントサイズなど、段落内の各テキストランで異なる可能性があるプロパティです。 カスタム段落およびカスタムテキスト実行プロパティの型を実装するには、アプリケーションで <xref:System.Windows.Media.TextFormatting.TextParagraphProperties> から派生したクラスを作成し、それぞれ <xref:System.Windows.Media.TextFormatting.TextRunProperties> する必要があります。  
  
## <a name="see-also"></a>参照

- [WPF のタイポグラフィ](typography-in-wpf.md)
- [WPF のドキュメント](documents-in-wpf.md)
