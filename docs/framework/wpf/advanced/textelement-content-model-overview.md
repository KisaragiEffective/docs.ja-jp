---
title: TextElement コンテンツ モデルの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- documents [WPF], flow documents
- TextElement content model [WPF]
- flow content elements [WPF], TextElement content model
ms.assetid: d0a7791c-b090-438c-812f-b9d009d83ee9
ms.openlocfilehash: acd67dd870c6912eddc368bf674804d98dba2db8
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73740754"
---
# <a name="textelement-content-model-overview"></a>TextElement コンテンツ モデルの概要
このコンテンツモデルの概要では、<xref:System.Windows.Documents.TextElement>でサポートされるコンテンツについて説明します。 <xref:System.Windows.Documents.Paragraph> クラスは <xref:System.Windows.Documents.TextElement>の一種です。 コンテンツ モデルは、他のオブジェクトや要素に含めることのできるオブジェクトや要素を記述します。 この概要では、<xref:System.Windows.Documents.TextElement>から派生したオブジェクトに使用するコンテンツモデルの概要を示します。 詳細については、「[フロードキュメントの概要](flow-document-overview.md)」を参照してください。  

<a name="text_element_classes"></a>   
## <a name="content-model-diagram"></a>コンテンツ モデルの図  
 次の図は、<xref:System.Windows.Documents.TextElement> から派生したクラスのコンテンツモデルと、その他の非 `TextElement` クラスがこのモデルにどのように適合するかをまとめたものです。  
  
 ![図: フローコンテンツコンテインメントスキーマ](./media/flow-content-schema.png "Flow_Content_Schema")  
  
 上の図からわかるように、要素に使用できる子は、クラスが <xref:System.Windows.Documents.Block> クラスと <xref:System.Windows.Documents.Inline> クラスのどちらから派生したものであるかによって必ずしも決定されるとは限りません。 たとえば、<xref:System.Windows.Documents.Span> (<xref:System.Windows.Documents.Inline>派生クラス) には <xref:System.Windows.Documents.Inline> 子要素のみを含めることができますが、<xref:System.Windows.Documents.Figure> (または <xref:System.Windows.Documents.Inline>派生クラス) には <xref:System.Windows.Documents.Block> 子要素のみを含めることができます。 そのため、どの要素を別の要素に含めることができるかをすばやく判断するには、図が役立ちます。 例として、図を使用して、<xref:System.Windows.Controls.RichTextBox>のフローコンテンツを作成する方法を決定します。  
  
1. <xref:System.Windows.Controls.RichTextBox> には、<xref:System.Windows.Documents.Block>派生オブジェクトを含む必要がある <xref:System.Windows.Documents.FlowDocument> が含まれている必要があります。 上の図に対応するセグメントを次に示します。  
  
     ![図: RichTextBox コンテインメントルール](./media/flow-ovw-schemawalkthrough1.png "Flow_Ovw_SchemaWalkThrough1")  
  
     この段階では、マークアップは次のようになります。  
  
     [!code-xaml[FlowOvwSnippets_snip#SchemaWalkThrough1](~/samples/snippets/csharp/VS_Snippets_Wpf/FlowOvwSnippets_snip/CS/MiscSnippets.xaml#schemawalkthrough1)]  
  
2. 図によれば、<xref:System.Windows.Documents.Paragraph>、<xref:System.Windows.Documents.Section>、<xref:System.Windows.Documents.Table>、<xref:System.Windows.Documents.List>、<xref:System.Windows.Documents.BlockUIContainer> を含むいくつかの <xref:System.Windows.Documents.Block> 要素を選択できます (前の図のブロック派生クラスを参照してください)。 たとえば、<xref:System.Windows.Documents.Table>が必要な場合を考えてみましょう。 上の図によれば、<xref:System.Windows.Documents.Table> には <xref:System.Windows.Documents.TableRow> 要素を含む <xref:System.Windows.Documents.TableRowGroup> が含まれています。この要素には、<xref:System.Windows.Documents.Block>派生オブジェクトを含む <xref:System.Windows.Documents.TableCell> 要素が含まれています。 次に示すのは、前の図から取得した <xref:System.Windows.Documents.Table> の対応するセグメントです。  
  
     ![ダイアグラム: テーブル&#47;の親子スキーマ](./media/flow-ovw-schemawalkthrough2.png "Flow_Ovw_SchemaWalkThrough2")  
  
     対応するマークアップは次のとおりです。  
  
     [!code-xaml[FlowOvwSnippets_snip#SchemaWalkThrough2](~/samples/snippets/csharp/VS_Snippets_Wpf/FlowOvwSnippets_snip/CS/MiscSnippets.xaml#schemawalkthrough2)]  
  
3. ここでも、<xref:System.Windows.Documents.TableCell>の下に1つ以上の <xref:System.Windows.Documents.Block> 要素が必要です。 簡単にするために、セル内にいくつかのテキストを配置することにします。 これを行うには、<xref:System.Windows.Documents.Run> 要素を持つ <xref:System.Windows.Documents.Paragraph> を使用します。 次に示すのは、<xref:System.Windows.Documents.Paragraph> が <xref:System.Windows.Documents.Inline> 要素を取り、<xref:System.Windows.Documents.Run> (<xref:System.Windows.Documents.Inline> 要素) がプレーンテキストのみを受け取ることができる図の対応するセグメントです。  
  
     ![ダイアグラム: 段落&#47;の親子スキーマ](./media/flow-ovw-schemawalkthrough3.png "Flow_Ovw_SchemaWalkThrough3")  
  
     ![ダイアグラム: 実行&#47;の親子スキーマ](./media/flow-ovw-schemawalkthrough4.png "Flow_Ovw_SchemaWalkThrough4")  
  
 例全体をマークアップで次に示します。  
  
 [!code-xaml[FlowOvwSnippets_snip#SchemaExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/FlowOvwSnippets_snip/CS/SchemaExample.xaml#schemaexamplewholepage)]  
  
<a name="Using_the_Content_Property"></a>   
## <a name="working-with-textelement-content-programmatically"></a>TextElement のコンテンツをプログラムで操作する  
 <xref:System.Windows.Documents.TextElement> の内容はコレクションによって構成されるため、プログラムによって <xref:System.Windows.Documents.TextElement> オブジェクトの内容を操作するには、これらのコレクションを使用します。 <xref:System.Windows.Documents.TextElement> 派生クラスで使用されるコレクションには、次の3種類があります。  
  
- <xref:System.Windows.Documents.InlineCollection>: <xref:System.Windows.Documents.Inline> 要素のコレクションを表します。 <xref:System.Windows.Documents.InlineCollection> は、<xref:System.Windows.Documents.Paragraph>、<xref:System.Windows.Documents.Span>、<xref:System.Windows.Controls.TextBlock> の要素の使用できる子コンテンツを定義します。  
  
- <xref:System.Windows.Documents.BlockCollection>: <xref:System.Windows.Documents.Block> 要素のコレクションを表します。 <xref:System.Windows.Documents.BlockCollection> は <xref:System.Windows.Documents.FlowDocument>、<xref:System.Windows.Documents.Section>、<xref:System.Windows.Documents.ListItem>、<xref:System.Windows.Documents.TableCell>、<xref:System.Windows.Documents.Floater>、<xref:System.Windows.Documents.Figure> 要素で許容される子コンテンツを定義します。  
  
- <xref:System.Windows.Documents.ListItemCollection>: 順序付けられた <xref:System.Windows.Documents.List>または順序付けられていない内の特定のコンテンツ項目を表すフローコンテンツ要素。  
  
 これらのコレクションの項目を操作 (追加または削除) するには、**インライン**、**ブロック**、および**ListItems**の各プロパティを使用します。 次の例では、**インライン**プロパティを使用してスパンのコンテンツを操作する方法を示します。  
  
> [!NOTE]
> Table では、コンテンツの操作にいくつかのコレクションが使用されますが、これらのコレクションについてはここでは取り上げません。 詳細については、「[テーブルの概要](table-overview.md)」を参照してください。  
  
 次の例では、新しい <xref:System.Windows.Documents.Span> オブジェクトを作成し、`Add` メソッドを使用して、<xref:System.Windows.Documents.Span>の子コンテンツとして2つのテキストランを追加します。  
  
 [!code-csharp[SpanSnippets#_SpanInlinesAdd](~/samples/snippets/csharp/VS_Snippets_Wpf/SpanSnippets/CSharp/Window1.xaml.cs#_spaninlinesadd)]
 [!code-vb[SpanSnippets#_SpanInlinesAdd](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SpanSnippets/visualbasic/window1.xaml.vb#_spaninlinesadd)]  
  
 次の例では、新しい <xref:System.Windows.Documents.Run> 要素を作成し、<xref:System.Windows.Documents.Span>の先頭に挿入します。  
  
 [!code-csharp[SpanSnippets#_SpanInlinesInsert](~/samples/snippets/csharp/VS_Snippets_Wpf/SpanSnippets/CSharp/Window1.xaml.cs#_spaninlinesinsert)]
 [!code-vb[SpanSnippets#_SpanInlinesInsert](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SpanSnippets/visualbasic/window1.xaml.vb#_spaninlinesinsert)]  
  
 次の例では、<xref:System.Windows.Documents.Span>内の最後の <xref:System.Windows.Documents.Inline> 要素を削除します。  
  
 [!code-csharp[SpanSnippets#_SpanInlinesRemoveLast](~/samples/snippets/csharp/VS_Snippets_Wpf/SpanSnippets/CSharp/Window1.xaml.cs#_spaninlinesremovelast)]
 [!code-vb[SpanSnippets#_SpanInlinesRemoveLast](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SpanSnippets/visualbasic/window1.xaml.vb#_spaninlinesremovelast)]  
  
 次の例では、<xref:System.Windows.Documents.Span>からすべての内容 (<xref:System.Windows.Documents.Inline> 要素) をクリアします。  
  
 [!code-csharp[SpanSnippets#_SpanInlinesClear](~/samples/snippets/csharp/VS_Snippets_Wpf/SpanSnippets/CSharp/Window1.xaml.cs#_spaninlinesclear)]
 [!code-vb[SpanSnippets#_SpanInlinesClear](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SpanSnippets/visualbasic/window1.xaml.vb#_spaninlinesclear)]  
  
<a name="Types_that_Share_this_Content_Model"></a>   
## <a name="types-that-share-this-content-model"></a>このコンテンツ モデルを共有する種類  
 次の型は <xref:System.Windows.Documents.TextElement> クラスを継承し、この概要で説明されている内容を表示するために使用できます。  
  
 <xref:System.Windows.Documents.Bold>、<xref:System.Windows.Documents.Figure>、<xref:System.Windows.Documents.Floater>、<xref:System.Windows.Documents.Hyperlink>、<xref:System.Windows.Documents.InlineUIContainer>、<xref:System.Windows.Documents.Italic>、<xref:System.Windows.Documents.LineBreak>、<xref:System.Windows.Documents.List>、<xref:System.Windows.Documents.ListItem>、<xref:System.Windows.Documents.Paragraph>、<xref:System.Windows.Documents.Run>、<xref:System.Windows.Documents.Section>です。  
  
 この一覧には、Windows SDK と共に配布される非抽象型のみが含まれていることに注意してください。 <xref:System.Windows.Documents.TextElement>から継承するその他の型を使用できます。  
  
<a name="Types_that_Can_Contain_ContentControl_Objects"></a>   
## <a name="types-that-can-contain-textelement-objects"></a>TextElement オブジェクトを含むことのできる型  
 「 [WPF コンテンツモデル](../controls/wpf-content-model.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [Blocks プロパティを介して FlowDocument を操作する](how-to-manipulate-a-flowdocument-through-the-blocks-property.md)
- [Blocks プロパティを介してフロー コンテンツ要素を操作する](how-to-manipulate-flow-content-elements-through-the-blocks-property.md)
- [Blocks プロパティを介して FlowDocument を操作する](how-to-manipulate-a-flowdocument-through-the-blocks-property.md)
- [Columns プロパティによってテーブルの列を操作する](how-to-manipulate-table-columns-through-the-columns-property.md)
- [RowGroups プロパティを介してテーブルの行グループを操作する](how-to-manipulate-table-row-groups-through-the-rowgroups-property.md)
