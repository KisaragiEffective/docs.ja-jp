---
title: データ テンプレートの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data binding [WPF], templates
- binding data [WPF], templates
- templates [WPF], data
- data templates [WPF]
ms.assetid: 0f4d9f8c-0230-4013-bd7b-e8e7fed01b4a
ms.openlocfilehash: 40d5ac257fa412afeb81b324d99604c0c1f5e878
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73974892"
---
# <a name="data-templating-overview"></a>データ テンプレートの概要
WPF のデータ テンプレート モデルは、データのプレゼンテーションを定義する優れた柔軟性を提供します。 WPF のコントロールには、データ プレゼンテーションのカスタマイズをサポートする組み込み機能があります。 このトピックでは、まず、<xref:System.Windows.DataTemplate> を定義する方法を示します。その後、カスタムロジックに基づいてテンプレートを選択したり、階層データの表示をサポートしたりするなど、その他のデータテンプレート機能を紹介します。

<a name="Prerequisites"></a>
## <a name="prerequisites"></a>必要条件
 このトピックは、データ テンプレートの機能に関するものであり、データ バインディングの概念の紹介ではありません。 データ バインディングの基本概念については、「[データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)」をご覧ください。

 <xref:System.Windows.DataTemplate> は、データの表示に関するものであり、WPF のスタイルとテンプレートモデルによって提供される多くの機能の1つです。 <xref:System.Windows.Style> を使用してコントロールのプロパティを設定する方法など、WPF のスタイルとテンプレートモデルの概要については、「[スタイルとテンプレート](../controls/styling-and-templating.md)」のトピックを参照してください。

 また、`Resources`を理解することが重要です。これは、基本的に、<xref:System.Windows.Style> や <xref:System.Windows.DataTemplate> などのオブジェクトを再利用できるようにするためのものです。 リソースについて詳しくは、「[XAML リソース](../advanced/xaml-resources.md)」をご覧ください。

<a name="DataTemplating_Basic"></a>
## <a name="data-templating-basics"></a>データ テンプレートの基礎

 <xref:System.Windows.DataTemplate> が重要な理由を示すために、データバインディングの例を見てみましょう。 この例では、`Task` オブジェクトの一覧にバインドされている <xref:System.Windows.Controls.ListBox> を使用します。 各 `Task` オブジェクトには `TaskName` (string)、`Description` (string)、`Priority` (int)、および `TaskType` 型のプロパティがあり、これは値が `Home` と `Work` の `Enum` です。

 [!code-xaml[DataTemplatingIntro_snip#Resources](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#resources)]
[!code-xaml[DataTemplatingIntro_snip#UI1](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#ui1)]
[!code-xaml[DataTemplatingIntro_snip#UI2](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#ui2)]

<a name="without_a_datatemplate"></a>
### <a name="without-a-datatemplate"></a>DataTemplate がない場合
 <xref:System.Windows.DataTemplate>がない場合、現在の <xref:System.Windows.Controls.ListBox> は次のようになります。

 ![データテンプレートのサンプルのスクリーンショット](./media/datatemplatingintro-fig1.png "DataTemplatingIntro_fig1")

 具体的な手順を実行しないと、コレクション内のオブジェクトを表示しようとしたときに、既定で <xref:System.Windows.Controls.ListBox> が `ToString` を呼び出します。 したがって、`Task` オブジェクトが `ToString` メソッドをオーバーライドする場合、<xref:System.Windows.Controls.ListBox> は基になるコレクション内の各ソースオブジェクトの文字列形式を表示します。

 たとえば、`Task` クラスが `ToString` メソッドを次のようにオーバーライドするものとします。`name` は `TaskName` プロパティのフィールドです。

 [!code-csharp[DataTemplatingIntro_snip#ToString](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Data.cs#tostring)]
 [!code-vb[DataTemplatingIntro_snip#ToString](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DataTemplatingIntro_snip/visualbasic/data.vb#tostring)]

 <xref:System.Windows.Controls.ListBox> は次のようになります。

 ![データテンプレートのサンプルのスクリーンショット](./media/datatemplatingintro-fig2.png "DataTemplatingIntro_fig2")

 ただし、これは制限があり、柔軟性ではありません。 また、XML データにバインドしている場合は、`ToString`をオーバーライドすることはできません。

<a name="defining_simple_datatemplate"></a>
### <a name="defining-a-simple-datatemplate"></a>簡単な DataTemplate の定義
 解決策として、<xref:System.Windows.DataTemplate>を定義します。 これを行う1つの方法は、<xref:System.Windows.Controls.ListBox> の <xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A> プロパティを <xref:System.Windows.DataTemplate>に設定することです。 <xref:System.Windows.DataTemplate> で指定した内容が、データオブジェクトの視覚的な構造になります。 次の <xref:System.Windows.DataTemplate> は非常に単純です。 各項目が <xref:System.Windows.Controls.StackPanel>内に3つの <xref:System.Windows.Controls.TextBlock> 要素として表示されるように指示します。 各 <xref:System.Windows.Controls.TextBlock> 要素は、`Task` クラスのプロパティにバインドされます。

 [!code-xaml[DataTemplatingIntro_snip#Inline](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#inline)]

 このトピックの例の基になるデータは、CLR オブジェクトのコレクションです。 XML データにバインドする場合、基本的な概念は同じですが、構文の違いはわずかです。 たとえば、`Path=TaskName`ではなく、<xref:System.Windows.Data.Binding.XPath%2A> を `@TaskName` に設定します (`TaskName` が XML ノードの属性である場合)。

 <xref:System.Windows.Controls.ListBox> は次のようになります。

 ![データテンプレートのサンプルのスクリーンショット](./media/datatemplatingintro-fig3.png "DataTemplatingIntro_fig3")

<a name="defining_datatemplate_as_a_resource"></a>
### <a name="creating-the-datatemplate-as-a-resource"></a>リソースとしての DataTemplate の作成
 上の例では、インライン <xref:System.Windows.DataTemplate> を定義しています。 再利用可能なオブジェクトになるように、リソース セクションで定義する方が一般的です。次に例を示します。

 [!code-xaml[DataTemplatingIntro_snip#R1](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#r1)]
[!code-xaml[DataTemplatingIntro_snip#AsResource](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#asresource)]
[!code-xaml[DataTemplatingIntro_snip#R2](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#r2)]

 これで、次の例のように、`myTaskTemplate` をリソースとして使用できるようになります。

 [!code-xaml[DataTemplatingIntro_snip#MyTaskTemplate](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#mytasktemplate)]

 `myTaskTemplate` はリソースであるため、<xref:System.Windows.DataTemplate> 型を受け取るプロパティを持つ他のコントロールで使用できるようになりました。 上に示すように、<xref:System.Windows.Controls.ListBox>などの <xref:System.Windows.Controls.ItemsControl> オブジェクトの場合は、<xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A> プロパティです。 <xref:System.Windows.Controls.ContentControl> オブジェクトの場合は、<xref:System.Windows.Controls.ContentControl.ContentTemplate%2A> プロパティです。

<a name="Styling_DataType"></a>
### <a name="the-datatype-property"></a>DataType プロパティ
 <xref:System.Windows.DataTemplate> クラスには、<xref:System.Windows.Style> クラスの <xref:System.Windows.Style.TargetType%2A> プロパティと非常によく似た <xref:System.Windows.DataTemplate.DataType%2A> プロパティがあります。 したがって、上記の例の <xref:System.Windows.DataTemplate> の `x:Key` を指定する代わりに、次の操作を行うことができます。

 [!code-xaml[DataTemplatingIntro_snip#DataType](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#datatype)]

 この <xref:System.Windows.DataTemplate> は、すべての `Task` オブジェクトに自動的に適用されます。 この場合、`x:Key` は暗黙的に設定されることに注意してください。 したがって、この <xref:System.Windows.DataTemplate> `x:Key` 値に割り当てた場合、暗黙的な `x:Key` はオーバーライドされ、<xref:System.Windows.DataTemplate> は自動的には適用されません。

 <xref:System.Windows.Controls.ContentControl> を `Task` オブジェクトのコレクションにバインドする場合、<xref:System.Windows.Controls.ContentControl> は上記の <xref:System.Windows.DataTemplate> を自動的には使用しません。 これは、<xref:System.Windows.Controls.ContentControl> のバインドでは、コレクション全体にバインドするか、個々のオブジェクトにバインドするかを区別するために、より多くの情報が必要になるためです。 <xref:System.Windows.Controls.ContentControl> が <xref:System.Windows.Controls.ItemsControl> の種類の選択を追跡している場合は、<xref:System.Windows.Controls.ContentControl> binding の <xref:System.Windows.Data.Binding.Path%2A> プロパティを "`/`" に設定して、現在のアイテムに関心があることを示すことができます。 この例については、「[コレクションにバインドして選択に基づく情報を表示する](how-to-bind-to-a-collection-and-display-information-based-on-selection.md)」をご覧ください。 それ以外の場合は、<xref:System.Windows.Controls.ContentControl.ContentTemplate%2A> プロパティを設定して、<xref:System.Windows.DataTemplate> を明示的に指定する必要があります。

 <xref:System.Windows.DataTemplate.DataType%2A> プロパティは、さまざまな種類のデータオブジェクトの <xref:System.Windows.Data.CompositeCollection> がある場合に特に便利です。 例については、「[CompositeCollection を実装する](how-to-implement-a-compositecollection.md)」をご覧ください。

<a name="adding_more_to_datatemplate"></a>
## <a name="adding-more-to-the-datatemplate"></a>DataTemplate へのその他の追加
 現在、データでは必要な情報が表示されますが、間違いなく改善の余地があります。 <xref:System.Windows.Controls.Border>、<xref:System.Windows.Controls.Grid>、および表示されるデータを記述するいくつかの <xref:System.Windows.Controls.TextBlock> 要素を追加して、プレゼンテーションを改良してみましょう。

 [!code-xaml[DataTemplatingIntro#AddingMore](~/samples/snippets/xaml/VS_Snippets_Wpf/DataTemplatingIntro/xaml/window1.xaml#addingmore)]
[!code-xaml[DataTemplatingIntro#AddingMore2](~/samples/snippets/xaml/VS_Snippets_Wpf/DataTemplatingIntro/xaml/window1.xaml#addingmore2)]

 次のスクリーンショットは、この変更された <xref:System.Windows.DataTemplate>の <xref:System.Windows.Controls.ListBox> を示しています。

 ![データテンプレートのサンプルのスクリーンショット](./media/datatemplatingintro-fig4.png "DataTemplatingIntro_fig4")

 <xref:System.Windows.Controls.ListBox> に <xref:System.Windows.HorizontalAlignment.Stretch> するように <xref:System.Windows.Controls.Control.HorizontalContentAlignment%2A> を設定して、項目の幅が領域全体を占めるようにすることができます。

 [!code-xaml[DataTemplatingIntro_snip#Stretch](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#stretch)]

 <xref:System.Windows.Controls.Control.HorizontalContentAlignment%2A> プロパティが <xref:System.Windows.HorizontalAlignment.Stretch>に設定されているので、<xref:System.Windows.Controls.ListBox> は次のようになります。

 ![データテンプレートのサンプルのスクリーンショット](./media/datatemplatingintro-fig5.png "DataTemplatingIntro_fig5")

<a name="DataTrigger_to_Apply_Property_Values"></a>
### <a name="use-datatriggers-to-apply-property-values"></a>DataTrigger を使ってプロパティ値を適用する
 現在のプレゼンテーションでは、`Task` が家のタスクか会社のタスクかわかりません。 `Task` オブジェクトには `TaskType` 型の `TaskType` プロパティがあり、これは値が `Home` と `Work` の列挙であることを思い出してください。

 次の例では、<xref:System.Windows.DataTrigger> は `border` という名前の要素の <xref:System.Windows.Controls.Border.BorderBrush%2A> を設定し、`TaskType` プロパティが `TaskType.Home`の場合に `Yellow` します。

 [!code-xaml[DataTemplatingIntro#DT](~/samples/snippets/xaml/VS_Snippets_Wpf/DataTemplatingIntro/xaml/window1.xaml#dt)]
[!code-xaml[DataTemplatingIntro#DataTrigger](~/samples/snippets/xaml/VS_Snippets_Wpf/DataTemplatingIntro/xaml/window1.xaml#datatrigger)]
[!code-xaml[DataTemplatingIntro#AddingMore2](~/samples/snippets/xaml/VS_Snippets_Wpf/DataTemplatingIntro/xaml/window1.xaml#addingmore2)]

 これで、アプリケーションの表示は次のようになります。 家のタスクは黄色の境界線で、会社のタスクは水色の境界線で示されます。

 ![データテンプレートのサンプルのスクリーンショット](./media/datatemplatingintro-fig6.png "DataTemplatingIntro_fig6")

 この例では、<xref:System.Windows.DataTrigger> は <xref:System.Windows.Setter> を使用してプロパティ値を設定します。 トリガークラスには、アニメーションなどの一連のアクションを開始できるようにする、<xref:System.Windows.TriggerBase.EnterActions%2A> プロパティと <xref:System.Windows.TriggerBase.ExitActions%2A> プロパティもあります。 また、複数のデータバインドプロパティ値に基づいて変更を適用できる <xref:System.Windows.MultiDataTrigger> クラスもあります。

 同じ効果を実現する別の方法として、<xref:System.Windows.Controls.Border.BorderBrush%2A> プロパティを `TaskType` プロパティにバインドし、値コンバーターを使用して `TaskType` 値に基づいて色を返すこともできます。 コンバーターを使って上の効果を作成するのは、パフォーマンスに関して若干効率的です。 さらに、独自のコンバーターを作成すると、独自のロジックを提供できるので柔軟性が向上します。 最終的に、どの手法を選ぶかは、シナリオと設定に依存します。 コンバーターの記述方法の詳細については、「<xref:System.Windows.Data.IValueConverter>」を参照してください。

<a name="what_belongs_in_datatemplate"></a>
### <a name="what-belongs-in-a-datatemplate"></a>DataTemplate に含まれるもの

前の例では、<xref:System.Windows.DataTemplate.Triggers%2A?displayProperty=nameWithType> プロパティを使用して、<xref:System.Windows.DataTemplate> 内にトリガーを配置しています。 トリガーの <xref:System.Windows.Setter> は、<xref:System.Windows.DataTemplate>内の要素 (<xref:System.Windows.Controls.Border> 要素) のプロパティの値を設定します。 ただし、`Setters` に関係するプロパティが、現在の <xref:System.Windows.DataTemplate>内にある要素のプロパティではない場合は、<xref:System.Windows.Controls.ListBoxItem> クラスの <xref:System.Windows.Style> を使用してプロパティを設定する方が適切な場合があります (バインディングするコントロールが<xref:System.Windows.Controls.ListBox>)。 たとえば、マウスが項目をポイントしたときに項目の <xref:System.Windows.UIElement.Opacity%2A> の値をアニメーション化する <xref:System.Windows.Trigger> する場合は、<xref:System.Windows.Controls.ListBoxItem> スタイル内でトリガーを定義します。 例については、「[Introduction to Styling and Templating Sample](https://github.com/Microsoft/WPF-Samples/tree/master/Styles%20&%20Templates/IntroToStylingAndTemplating)」(スタイルとテンプレートのサンプルの概要) をご覧ください。

 一般に、<xref:System.Windows.DataTemplate> は、生成された各 <xref:System.Windows.Controls.ListBoxItem> に適用されることに注意してください (実際に適用される方法と場所の詳細については、<xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A> のページを参照してください)。 <xref:System.Windows.DataTemplate> は、データオブジェクトのプレゼンテーションと外観のみに関係しています。 ほとんどの場合、項目が選択されたときの外観や、項目をレイアウトする <xref:System.Windows.Controls.ListBox> 方法など、プレゼンテーションの他のすべての側面は、<xref:System.Windows.DataTemplate>の定義に属していません。 例については、「[ItemsControl のスタイルとテンプレートの設定](#DataTemplating_ItemsControl)」セクションをご覧ください。

<a name="Styling_StyleSelection"></a>
## <a name="choosing-a-datatemplate-based-on-properties-of-the-data-object"></a>データ オブジェクトのプロパティに基づく DataTemplate の選択
 「[DataType プロパティ](#Styling_DataType)」セクションでは、異なるデータ オブジェクトに対して異なるデータ テンプレートを定義できることを説明しました。 これは、さまざまな型やコレクションの <xref:System.Windows.Data.CompositeCollection> が異なる型の項目を持つ場合に特に便利です。 [ [DataTriggers を使用してプロパティ値を適用する](#DataTrigger_to_Apply_Property_Values)] セクションでは、同じ種類のデータオブジェクトのコレクションがある場合に、<xref:System.Windows.DataTemplate> を作成し、トリガーを使用して各データオブジェクトのプロパティ値に基づいて変更を適用できることを示しています。 ただし、トリガーではプロパティ値を適用したりアニメーションを開始することはできますが、データ オブジェクトの構造を再構築できるような柔軟性はありません。 場合によっては、同じ種類のデータオブジェクトに対して別の <xref:System.Windows.DataTemplate> を作成する必要がありますが、プロパティが異なることがあります。

 たとえば、`Task` オブジェクトの `Priority` の値が `1` のときは、自分用の警告としてまったく異なる外観にしたいような場合です。 その場合は、優先順位の高い `Task` オブジェクトを表示するための <xref:System.Windows.DataTemplate> を作成します。 Resources セクションに次の <xref:System.Windows.DataTemplate> を追加してみましょう。

 [!code-xaml[DataTemplatingIntro_snip#ImportantTemplate](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#importanttemplate)]

この例では、 [system.windows.datatemplate>](xref:System.Windows.FrameworkTemplate.Resources%2A)プロパティを使用します。 このセクションで定義されているリソースは、<xref:System.Windows.DataTemplate>内の要素によって共有されます。

 データオブジェクトの `Priority` 値に基づいて使用する <xref:System.Windows.DataTemplate> を選択するロジックを指定するには <xref:System.Windows.Controls.DataTemplateSelector> のサブクラスを作成し、<xref:System.Windows.Controls.DataTemplateSelector.SelectTemplate%2A> メソッドをオーバーライドします。 次の例では、<xref:System.Windows.Controls.DataTemplateSelector.SelectTemplate%2A> メソッドは、`Priority` プロパティの値に基づいて適切なテンプレートを返すロジックを提供します。 返されるテンプレートは、エンベロープ <xref:System.Windows.Window> 要素のリソースにあります。

 [!code-csharp[DataTemplatingIntro_snip#DTSClass](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/TaskListDataTemplateSelector.cs#dtsclass)]
 [!code-vb[DataTemplatingIntro_snip#DTSClass](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DataTemplatingIntro_snip/visualbasic/tasklistdatatemplateselector.vb#dtsclass)]

 その後、リソースとして `TaskListDataTemplateSelector` を宣言できます。

 [!code-xaml[DataTemplatingIntro_snip#R1](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#r1)]
[!code-xaml[DataTemplatingIntro_snip#DTS](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#dts)]
[!code-xaml[DataTemplatingIntro_snip#R2](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#r2)]

 テンプレートセレクターリソースを使用するには、<xref:System.Windows.Controls.ListBox>の <xref:System.Windows.Controls.ItemsControl.ItemTemplateSelector%2A> プロパティに割り当てます。 <xref:System.Windows.Controls.ListBox> は、基になるコレクション内の各項目について、`TaskListDataTemplateSelector` の <xref:System.Windows.Controls.DataTemplateSelector.SelectTemplate%2A> メソッドを呼び出します。 呼び出しでは、項目パラメーターとしてデータ オブジェクトを渡します。 その後、メソッドによって返された <xref:System.Windows.DataTemplate> がそのデータオブジェクトに適用されます。

 [!code-xaml[DataTemplatingIntro_snip#ItemTemplateSelector](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#itemtemplateselector)]

 テンプレートセレクターを配置すると、<xref:System.Windows.Controls.ListBox> が次のように表示されるようになります。

 ![データテンプレートのサンプルのスクリーンショット](./media/datatemplatingintro-fig7.png "DataTemplatingIntro_fig7")

これがこの例の結論です。 完全なサンプルについては、「[Introduction to Data Templating Sample](https://github.com/Microsoft/WPF-Samples/tree/master/Data%20Binding/DataTemplatingIntro)」(データ テンプレート サンプルの概要) をご覧ください。

<a name="DataTemplating_ItemsControl"></a>
## <a name="styling-and-templating-an-itemscontrol"></a>ItemsControl のスタイルとテンプレートの設定
 <xref:System.Windows.Controls.ItemsControl> は <xref:System.Windows.DataTemplate> を使用できる唯一のコントロール型ではありませんが、<xref:System.Windows.Controls.ItemsControl> をコレクションにバインドするのは非常に一般的なシナリオです。 「 [System.windows.datatemplate> に属し](#what_belongs_in_datatemplate)ているもの」セクションでは、<xref:System.Windows.DataTemplate> の定義でデータの表示のみを考慮する必要があることについて説明しました。 <xref:System.Windows.DataTemplate> を使用するのが適切でない場合には、<xref:System.Windows.Controls.ItemsControl>によって提供されるさまざまなスタイルとテンプレートプロパティを理解することが重要です。 次の例は、これらの各プロパティの機能がわかるように設計されています。 この例の <xref:System.Windows.Controls.ItemsControl> は、前の例と同じ `Tasks` コレクションにバインドされています。 わかりやすいように、この例のスタイルとテンプレートはすべてインラインで宣言されています。

 [!code-xaml[DataTemplatingIntro_snip#ItemsControlProperties](~/samples/snippets/csharp/VS_Snippets_Wpf/DataTemplatingIntro_snip/CSharp/Window1.xaml#itemscontrolproperties)]

 次に示すのは、この例がレンダリングされたときのスクリーンショットです。

 ![ItemsControl の例のスクリーンショット](./media/databinding-itemscontrolproperties.png "DataBinding_ItemsControlProperties")

 <xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A>を使用する代わりに、<xref:System.Windows.Controls.ItemsControl.ItemTemplateSelector%2A>を使用することもできます。 例については、前のセクションをご覧ください。 同様に、<xref:System.Windows.Controls.ItemsControl.ItemContainerStyle%2A>を使用する代わりに、<xref:System.Windows.Controls.ItemsControl.ItemContainerStyleSelector%2A>を使用するオプションもあります。

 ここに表示されていない <xref:System.Windows.Controls.ItemsControl> の他の2つのスタイル関連プロパティは <xref:System.Windows.Controls.ItemsControl.GroupStyle%2A> と <xref:System.Windows.Controls.ItemsControl.GroupStyleSelector%2A>です。

<a name="DataTemplating_HeirarchicalDataTemplate"></a>
## <a name="support-for-hierarchical-data"></a>階層データのサポート
 これまでは、1 つのコレクションにバインドして表示する方法のみを説明してきました。 場合によっては、コレクションに他のコレクションが含まれることがあります。 <xref:System.Windows.HierarchicalDataTemplate> クラスは、このようなデータを表示するために <xref:System.Windows.Controls.HeaderedItemsControl> 型と共に使用するように設計されています。 次の例で、`ListLeagueList` は `League` オブジェクトのリストです。 各 `League` オブジェクトには、`Name` と、`Division` オブジェクトのコレクションがあります。 各 `Division` には、`Name` と `Team` オブジェクトのコレクションがあり、各 `Team` オブジェクトには `Name` があります。

 [!code-xaml[HierarchicalDataTemplateSnippet#HDT](~/samples/snippets/csharp/VS_Snippets_Wpf/HierarchicalDataTemplateSnippet/CS/window1.xaml#hdt)]

 この例では、<xref:System.Windows.HierarchicalDataTemplate>を使用して、他のリストを含むリストデータを簡単に表示できることを示しています。 次に示すのは、この例のスクリーンショットです。

 ![HierarchicalDataTemplate サンプルスクリーンショット](./media/databinding-hierarchicaldatatemplate.png "DataBinding_HierarchicalDataTemplate")

## <a name="see-also"></a>関連項目

- [データ バインディング](../advanced/optimizing-performance-data-binding.md)
- [DataTemplate によって生成された要素を検索する](how-to-find-datatemplate-generated-elements.md)
- [スタイルとテンプレート](../controls/styling-and-templating.md)
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [GridView の列ヘッダー スタイルおよびテンプレートの概要](../controls/gridview-column-header-styles-and-templates-overview.md)
