---
title: 外観をカスタマイズできるコントロールの作成
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- controls [WPF], customizing
- VisualStateManager [WPF], managing the state of a control
- ControlTemplate [WPF], customizing appearance
- controls [WPF], defining the visual structure and behavior of
- customizing appearance [WPF], ControlTemplate
- managing control states [WPF], VisualStateManager
- VisualStateManager [WPF], best practice
ms.assetid: 9e356d3d-a3d0-4b01-a25f-2d43e4d53fe5
ms.openlocfilehash: d9cf092cf47d4fb70b15033d039777d3279b633a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2019
ms.locfileid: "74283574"
---
# <a name="creating-a-control-that-has-a-customizable-appearance"></a>外観をカスタマイズできるコントロールの作成

<a name="introduction"></a>
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] を使用すると、外観をカスタマイズできるコントロールを作成できます。 たとえば、新しい <xref:System.Windows.Controls.ControlTemplate>を作成することによって設定されるプロパティを超えて <xref:System.Windows.Controls.CheckBox> の外観を変更できます。 次の図は、既定の <xref:System.Windows.Controls.ControlTemplate> とカスタム <xref:System.Windows.Controls.ControlTemplate>を使用する <xref:System.Windows.Controls.CheckBox> を使用する <xref:System.Windows.Controls.CheckBox> を示しています。

![既定のコントロール テンプレートを使用するチェックボックス。](./media/ndp-checkboxdefault.png "NDP_CheckBoxDefault")
既定のコントロール テンプレートを使用する CheckBox

![カスタム コントロール テンプレートを使用するチェックボックス。](./media/ndp-checkboxcustom.png "NDP_CheckBoxCustom")
カスタム コントロール テンプレートを使用する CheckBox

コントロールを作成するときに parts と states のモデルに従うと、コントロールの外観がカスタマイズ可能になります。 Blend for Visual Studio などのデザイナーツールでは、パーツと状態モデルがサポートされているため、このモデルに従うと、そのような種類のアプリケーションでコントロールをカスタマイズできます。  このトピックでは、独自のコントロールを作成するときに、パーツと状態のモデルとその実行方法について説明します。 このトピックでは、このモデルの理念を示すために、カスタムコントロール `NumericUpDown`の例を使用します。  `NumericUpDown` コントロールには数値が表示されます。この値は、コントロールのボタンをクリックすることによって増減できます。  次の図は、このトピックで説明する `NumericUpDown` コントロールを示しています。

![NumericUpDown カスタムコントロール。](./media/ndp-numericupdown.png "NDP_NumericUPDown")
カスタム NumericUpDown コントロール

このトピックには、次のセクションが含まれます。

- [必要条件](#prerequisites)

- [部品と状態モデル](#parts_and_states_model)

- [ControlTemplate でのコントロールの視覚的な構造と視覚的な動作の定義](#defining_the_visual_structure_and_visual_behavior_of_a_control_in_a_controltemplate)

- [コードでの ControlTemplate の一部の使用](#using_parts_of_the_controltemplate_in_code)

- [コントロールコントラクトの提供](#providing_the_control_contract)

- [完全な例](#complete_example)

<a name="prerequisites"></a>

## <a name="prerequisites"></a>前提条件

このトピックでは、既存のコントロールの新しい <xref:System.Windows.Controls.ControlTemplate> を作成する方法、コントロールコントラクトの要素について理解し、「[コントロールのテンプレートを作成](../../../desktop-wpf/themes/how-to-create-apply-template.md)する」で説明されている概念を理解していることを前提としています。

> [!NOTE]
> 外観をカスタマイズできるコントロールを作成するには、<xref:System.Windows.Controls.Control> クラスまたは <xref:System.Windows.Controls.UserControl>以外のサブクラスの1つを継承するコントロールを作成する必要があります。  <xref:System.Windows.Controls.UserControl> から継承するコントロールは、すばやく作成できるコントロールですが、<xref:System.Windows.Controls.ControlTemplate> を使用せず、外観をカスタマイズすることもできません。

<a name="parts_and_states_model"></a>

## <a name="parts-and-states-model"></a>部品と状態モデル

Parts and states モデルでは、コントロールの視覚的な構造と視覚的な動作を定義する方法を指定します。 パートと状態モデルに従うには、次の手順を実行する必要があります。

- コントロールの <xref:System.Windows.Controls.ControlTemplate> での視覚的な構造と視覚的な動作を定義します。

- コントロールのロジックがコントロールテンプレートの一部と対話する場合は、特定のベストプラクティスに従ってください。

- <xref:System.Windows.Controls.ControlTemplate>に含める内容を指定するコントロールコントラクトを提供します。

コントロールの <xref:System.Windows.Controls.ControlTemplate> で視覚的な構造と視覚的な動作を定義すると、アプリケーションの作成者は、コードを記述する代わりに新しい <xref:System.Windows.Controls.ControlTemplate> を作成することによって、コントロールの視覚的な構造と視覚的な動作を変更できます。   <xref:System.Windows.Controls.ControlTemplate>で定義するオブジェクトと状態 <xref:System.Windows.FrameworkElement> をアプリケーションの作成者に通知するコントロールコントラクトを指定する必要があります。 <xref:System.Windows.Controls.ControlTemplate> 内のパーツを操作するときは、いくつかのベストプラクティスに従う必要があります。これにより、コントロールが不完全な <xref:System.Windows.Controls.ControlTemplate>を正しく処理できるようになります。  これら3つの原則に従うと、アプリケーションの作成者は、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]に付属するコントロールの場合と同様に、コントロールの <xref:System.Windows.Controls.ControlTemplate> を簡単に作成できるようになります。  次のセクションでは、これらの推奨事項について詳しく説明します。

<a name="defining_the_visual_structure_and_visual_behavior_of_a_control_in_a_controltemplate"></a>

## <a name="defining-the-visual-structure-and-visual-behavior-of-a-control-in-a-controltemplate"></a>ControlTemplate でのコントロールの視覚的な構造と視覚的な動作の定義

Parts および states モデルを使用してカスタムコントロールを作成する場合は、コントロールの視覚的な構造と視覚的な動作をロジックではなく <xref:System.Windows.Controls.ControlTemplate> で定義します。  コントロールの視覚的な構造は、コントロールを構成する <xref:System.Windows.FrameworkElement> オブジェクトの複合です。  視覚的な動作とは、コントロールが特定の状態にあるときのコントロールの外観です。   コントロールの視覚的な構造と視覚動作を指定する <xref:System.Windows.Controls.ControlTemplate> の作成の詳細については、「[コントロールのテンプレートを作成する](../../../desktop-wpf/themes/how-to-create-apply-template.md)」を参照してください。

`NumericUpDown` コントロールの例では、ビジュアル構造に2つの <xref:System.Windows.Controls.Primitives.RepeatButton> コントロールと1つの <xref:System.Windows.Controls.TextBlock>が含まれています。  これらのコントロールを `NumericUpDown` コントロールのコードに追加した場合、たとえば、そのコンストラクターでは、これらのコントロールの位置は unalterable になります。  コントロールのビジュアル構造とビジュアル動作をコード内で定義するのではなく、<xref:System.Windows.Controls.ControlTemplate>で定義する必要があります。  次に、ボタンと <xref:System.Windows.Controls.TextBlock> の位置をカスタマイズし、<xref:System.Windows.Controls.ControlTemplate> を置き換えることができるので `Value` が負の場合に発生する動作を指定します。

次の例は、`NumericUpDown` コントロールの視覚的な構造を示しています。これには `Value`を増やすための <xref:System.Windows.Controls.Primitives.RepeatButton>、`Value`を減らすための <xref:System.Windows.Controls.Primitives.RepeatButton>、<xref:System.Windows.Controls.TextBlock> を表示するための `Value`が含まれています。

[!code-xaml[VSMCustomControl#VisualStructure](~/samples/snippets/csharp/VS_Snippets_Wpf/vsmcustomcontrol/csharp/window1.xaml#visualstructure)]

`NumericUpDown` コントロールの視覚的な動作は、値が負の場合に赤いフォントで表示されることです。  `Value` が負の場合にコード内の <xref:System.Windows.Controls.TextBlock> の <xref:System.Windows.Controls.TextBlock.Foreground%2A> を変更すると、`NumericUpDown` には常に赤色の負の値が表示されます。 <xref:System.Windows.Controls.ControlTemplate>に <xref:System.Windows.VisualState> オブジェクトを追加することによって、<xref:System.Windows.Controls.ControlTemplate> でのコントロールの視覚的な動作を指定します。  次の例は、`Positive` と `Negative` の状態の <xref:System.Windows.VisualState> オブジェクトを示しています。  `Positive` と `Negative` は相互に排他的です (コントロールは常に2つのうちの1つになります)。そのため、この例では、<xref:System.Windows.VisualState> オブジェクトを1つの <xref:System.Windows.VisualStateGroup>に配置します。  コントロールが `Negative` 状態になると、<xref:System.Windows.Controls.TextBlock> の <xref:System.Windows.Controls.TextBlock.Foreground%2A> が赤に変わります。  コントロールが `Positive` 状態の場合、<xref:System.Windows.Controls.TextBlock.Foreground%2A> は元の値に戻ります。  <xref:System.Windows.Controls.ControlTemplate> での <xref:System.Windows.VisualState> オブジェクトの定義については、「[コントロールのテンプレートを作成する](../../../desktop-wpf/themes/how-to-create-apply-template.md)」で詳しく説明します。

> [!NOTE]
> <xref:System.Windows.Controls.ControlTemplate>のルート <xref:System.Windows.FrameworkElement> には、<xref:System.Windows.VisualStateManager.VisualStateGroups%2A?displayProperty=nameWithType> 添付プロパティを必ず設定してください。

[!code-xaml[VSMCustomControl#ValueStates](~/samples/snippets/csharp/VS_Snippets_Wpf/vsmcustomcontrol/csharp/window1.xaml#valuestates)]

<a name="using_parts_of_the_controltemplate_in_code"></a>

## <a name="using-parts-of-the-controltemplate-in-code"></a>コードでの ControlTemplate の一部の使用

<xref:System.Windows.Controls.ControlTemplate> 作成者は、意図的にまたは誤って、<xref:System.Windows.FrameworkElement> または <xref:System.Windows.VisualState> オブジェクトを省略する場合がありますが、コントロールのロジックでは、これらの部分が正しく機能するために必要になる場合があります。 Parts および states モデルは、<xref:System.Windows.FrameworkElement> オブジェクトまたは <xref:System.Windows.VisualState> オブジェクトが不足している <xref:System.Windows.Controls.ControlTemplate> に対して、コントロールの回復性を備えていることを指定します。  <xref:System.Windows.FrameworkElement>、<xref:System.Windows.VisualState>、または <xref:System.Windows.VisualStateGroup> が <xref:System.Windows.Controls.ControlTemplate>にない場合は、コントロールで例外をスローしたり、エラーを報告したりしないでください。 ここでは、<xref:System.Windows.FrameworkElement> オブジェクトとの対話と状態の管理について推奨される運用方法について説明します。

### <a name="anticipate-missing-frameworkelement-objects"></a>不足している FrameworkElement オブジェクトの予測

<xref:System.Windows.Controls.ControlTemplate>で <xref:System.Windows.FrameworkElement> オブジェクトを定義する場合は、コントロールのロジックがその一部と対話する必要がある場合があります。  たとえば、`NumericUpDown` コントロールは、ボタンの <xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントをサブスクライブして `Value` を増減し、<xref:System.Windows.Controls.TextBlock> の <xref:System.Windows.Controls.TextBlock.Text%2A> プロパティを `Value`に設定します。 カスタム <xref:System.Windows.Controls.ControlTemplate> で <xref:System.Windows.Controls.TextBlock> またはボタンが省略されている場合は、コントロールがその機能の一部を失うことは許容されますが、コントロールでエラーが発生しないことを確認する必要があります。 たとえば、<xref:System.Windows.Controls.ControlTemplate> に `Value`を変更するボタンが含まれていない場合、`NumericUpDown` はその機能を失いますが、<xref:System.Windows.Controls.ControlTemplate> を使用するアプリケーションは引き続き実行されます。

次の方法を使用すると、コントロールが不足している <xref:System.Windows.FrameworkElement> オブジェクトに正しく応答するようになります。

1. コードで参照する必要のある各 <xref:System.Windows.FrameworkElement> の `x:Name` 属性を設定します。

2. 操作する必要がある各 <xref:System.Windows.FrameworkElement> のプライベートプロパティを定義します。

3. <xref:System.Windows.FrameworkElement> プロパティの set アクセサーで、コントロールが処理するすべてのイベントをサブスクライブおよびサブスクライブ解除します。

4. <xref:System.Windows.FrameworkElement.OnApplyTemplate%2A> メソッドで、手順 2. で定義した <xref:System.Windows.FrameworkElement> のプロパティを設定します。 これは、<xref:System.Windows.Controls.ControlTemplate> 内の <xref:System.Windows.FrameworkElement> をコントロールで使用できる最も早い段階です。 <xref:System.Windows.Controls.ControlTemplate>から取得するには、<xref:System.Windows.FrameworkElement> の `x:Name` を使用します。

5. メンバーにアクセスする前に、<xref:System.Windows.FrameworkElement> が `null` ていないことを確認してください。  `null`場合は、エラーを報告しないでください。

次の例は、前の一覧の推奨事項に従って、`NumericUpDown` コントロールが <xref:System.Windows.FrameworkElement> オブジェクトとどのように対話するかを示しています。

<xref:System.Windows.Controls.ControlTemplate>内の `NumericUpDown` コントロールのビジュアル構造を定義する例では、`Value` を向上させる <xref:System.Windows.Controls.Primitives.RepeatButton> に `x:Name` 属性が `UpButton`に設定されています。  次の例では、<xref:System.Windows.Controls.ControlTemplate>で宣言されている <xref:System.Windows.Controls.Primitives.RepeatButton> を表す `UpButtonElement` という名前のプロパティを宣言しています。 `set` アクセサーは、最初にボタンの <xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントにアンサブスクライブします。 `UpDownElement` が `null`されていない場合は、プロパティが設定され、次に <xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントがサブスクライブされます。 また、プロパティも定義されていますが、ここには示されていませんが、`DownButtonElement`と呼ばれる他の <xref:System.Windows.Controls.Primitives.RepeatButton>についても説明しています。

[!code-csharp[VSMCustomControl#UpButtonProperty](~/samples/snippets/csharp/VS_Snippets_Wpf/vsmcustomcontrol/csharp/numericupdown.cs#upbuttonproperty)]
[!code-vb[VSMCustomControl#UpButtonProperty](~/samples/snippets/visualbasic/VS_Snippets_Wpf/vsmcustomcontrol/visualbasic/numericupdown.vb#upbuttonproperty)]

次の例は、`NumericUpDown` コントロールの <xref:System.Windows.FrameworkElement.OnApplyTemplate%2A> を示しています。  この例では、<xref:System.Windows.FrameworkElement.GetTemplateChild%2A> メソッドを使用して、<xref:System.Windows.Controls.ControlTemplate>から <xref:System.Windows.FrameworkElement> オブジェクトを取得します。  この例では、指定した名前の <xref:System.Windows.FrameworkElement> が予期される型ではない場合に、<xref:System.Windows.FrameworkElement.GetTemplateChild%2A> が検出されることに注意してください。 また、指定した `x:Name` を持つが、型が間違っている要素は無視することをお勧めします。

[!code-csharp[VSMCustomControl#ApplyTemplate](~/samples/snippets/csharp/VS_Snippets_Wpf/vsmcustomcontrol/csharp/numericupdown.cs#applytemplate)]
[!code-vb[VSMCustomControl#ApplyTemplate](~/samples/snippets/visualbasic/VS_Snippets_Wpf/vsmcustomcontrol/visualbasic/numericupdown.vb#applytemplate)]

前の例で示した手順に従うことで、<xref:System.Windows.Controls.ControlTemplate> に <xref:System.Windows.FrameworkElement>がない場合に、コントロールが引き続き実行されるようにすることができます。

### <a name="use-the-visualstatemanager-to-manage-states"></a>VisualStateManager を使用して状態を管理する

<xref:System.Windows.VisualStateManager> は、コントロールの状態を追跡し、状態間の遷移に必要なロジックを実行します。 <xref:System.Windows.Controls.ControlTemplate>に <xref:System.Windows.VisualState> オブジェクトを追加する場合は、それらを <xref:System.Windows.VisualStateGroup> に追加し、<xref:System.Windows.VisualStateManager.VisualStateGroups%2A?displayProperty=nameWithType> にアクセスできるように <xref:System.Windows.VisualStateManager> 添付プロパティに <xref:System.Windows.VisualStateGroup> を追加します。

次の例では、前の例を繰り返して、コントロールの `Positive` と `Negative` の状態に対応する <xref:System.Windows.VisualState> オブジェクトを示します。 `Negative`<xref:System.Windows.VisualState> の <xref:System.Windows.Media.Animation.Storyboard> により、<xref:System.Windows.Controls.TextBlock> 赤の <xref:System.Windows.Controls.TextBlock.Foreground%2A> が変わります。   `NumericUpDown` コントロールが `Negative` 状態になると、`Negative` 状態のストーリーボードが開始されます。  その後、コントロールが `Positive` 状態に戻ったときに、`Negative` の状態の <xref:System.Windows.Media.Animation.Storyboard> が停止します。  `Positive`<xref:System.Windows.VisualState> には <xref:System.Windows.Media.Animation.Storyboard> を含める必要はありません。これは、`Negative` の <xref:System.Windows.Media.Animation.Storyboard> が停止したときに、<xref:System.Windows.Controls.TextBlock.Foreground%2A> が元の色に戻るためです。

[!code-xaml[VSMCustomControl#ValueStates](~/samples/snippets/csharp/VS_Snippets_Wpf/vsmcustomcontrol/csharp/window1.xaml#valuestates)]

<xref:System.Windows.Controls.TextBlock> には名前が指定されていますが、コントロールのロジックが <xref:System.Windows.Controls.TextBlock>を参照しないため、<xref:System.Windows.Controls.TextBlock> は `NumericUpDown` の制御コントラクトに含まれていません。  <xref:System.Windows.Controls.ControlTemplate> で参照される要素には名前がありますが、コントロールの新しい <xref:System.Windows.Controls.ControlTemplate> でその要素を参照する必要がない場合があるため、コントロールコントラクトに含める必要はありません。  たとえば、`NumericUpDown` の新しい <xref:System.Windows.Controls.ControlTemplate> を作成するユーザーは、<xref:System.Windows.Controls.Control.Foreground%2A>を変更することによって `Value` が負であることを示すことがない場合があります。  この場合、コードと <xref:System.Windows.Controls.ControlTemplate> はどちらも名前で <xref:System.Windows.Controls.TextBlock> を参照しません。

コントロールのロジックは、コントロールの状態を変更する役割を担います。 次の例では、`NumericUpDown` コントロールが <xref:System.Windows.VisualStateManager.GoToState%2A> メソッドを呼び出して、`Value` が0以上の場合に `Positive` 状態に移行し、`Negative` が0未満の場合は `Value` 状態になることを示します。

[!code-csharp[VSMCustomControl#ValueStateChange](~/samples/snippets/csharp/VS_Snippets_Wpf/vsmcustomcontrol/csharp/numericupdown.cs#valuestatechange)]
[!code-vb[VSMCustomControl#ValueStateChange](~/samples/snippets/visualbasic/VS_Snippets_Wpf/vsmcustomcontrol/visualbasic/numericupdown.vb#valuestatechange)]

<xref:System.Windows.VisualStateManager.GoToState%2A> メソッドは、ストーリーボードを適切に開始および停止するために必要なロジックを実行します。 コントロールが <xref:System.Windows.VisualStateManager.GoToState%2A> を呼び出してその状態を変更すると、<xref:System.Windows.VisualStateManager> は次の操作を実行します。

- コントロールが移動する <xref:System.Windows.VisualState> に <xref:System.Windows.Media.Animation.Storyboard>がある場合は、ストーリーボードが開始されます。 次に、コントロールの送信元の <xref:System.Windows.VisualState> に <xref:System.Windows.Media.Animation.Storyboard>がある場合は、ストーリーボードが終了します。

- コントロールが既に指定されている状態にある場合、<xref:System.Windows.VisualStateManager.GoToState%2A> は何も処理を行わず、`true`を返します。

- 指定された状態が `control`の <xref:System.Windows.Controls.ControlTemplate> に存在しない場合、<xref:System.Windows.VisualStateManager.GoToState%2A> はアクションを実行せず、`false`を返します。

#### <a name="best-practices-for-working-with-the-visualstatemanager"></a>VisualStateManager を使用するためのベストプラクティス

コントロールの状態を維持するには、次の操作を行うことをお勧めします。

- プロパティを使用して、状態を追跡します。

- 状態を切り替えるためのヘルパーメソッドを作成します。

`NumericUpDown` コントロールでは、`Value` プロパティを使用して、`Positive` または `Negative` 状態であるかどうかを追跡します。  また、`NumericUpDown` コントロールは、<xref:System.Windows.UIElement.IsFocused%2A> プロパティを追跡する `Focused` と `UnFocused` の状態も定義します。 自然にコントロールのプロパティに対応していない状態を使用する場合は、プライベートプロパティを定義して状態を追跡できます。

すべての状態を更新する1つのメソッドは、<xref:System.Windows.VisualStateManager> の呼び出しを一元化し、コードを管理しやすくします。 次の例は、`NumericUpDown` コントロールのヘルパーメソッドである `UpdateStates`を示しています。 `Value` が0以上の場合、<xref:System.Windows.Controls.Control> は `Positive` 状態になります。  `Value` が0未満の場合、コントロールは `Negative` 状態になります。  <xref:System.Windows.UIElement.IsFocused%2A> が `true`場合、コントロールは `Focused` の状態になります。それ以外の場合は、`Unfocused` 状態になります。  コントロールは、状態の変化に関係なく、状態を変更する必要があるときに `UpdateStates` を呼び出すことができます。

[!code-csharp[VSMCustomControl#UpdateStates](~/samples/snippets/csharp/VS_Snippets_Wpf/vsmcustomcontrol/csharp/numericupdown.cs#updatestates)]
[!code-vb[VSMCustomControl#UpdateStates](~/samples/snippets/visualbasic/VS_Snippets_Wpf/vsmcustomcontrol/visualbasic/numericupdown.vb#updatestates)]

コントロールが既にその状態にあるときに状態名を <xref:System.Windows.VisualStateManager.GoToState%2A> に渡すと、<xref:System.Windows.VisualStateManager.GoToState%2A> は何も行われないため、コントロールの現在の状態を確認する必要はありません。  たとえば、負の数値から別の負の数値に変更し `Value` と、`Negative` 状態のストーリーボードは中断されず、ユーザーはコントロールの変更を確認できません。

<xref:System.Windows.VisualStateManager> は <xref:System.Windows.VisualStateGroup> オブジェクトを使用して、<xref:System.Windows.VisualStateManager.GoToState%2A>を呼び出すときに終了する状態を判断します。 コントロールは、<xref:System.Windows.Controls.ControlTemplate> で定義されている <xref:System.Windows.VisualStateGroup> ごとに常に1つの状態になり、同じ <xref:System.Windows.VisualStateGroup>から別の状態になったときにのみ状態が保持されます。 たとえば、`NumericUpDown` コントロールの <xref:System.Windows.Controls.ControlTemplate> では、`Positive` と `Negative`の <xref:System.Windows.VisualState> オブジェクトを1つの <xref:System.Windows.VisualStateGroup> に定義し、`Focused` オブジェクトと `Unfocused`オブジェクトを別の <xref:System.Windows.VisualState> にします。 (`Focused` および `Unfocused`<xref:System.Windows.VisualState> は、このトピックの「[完全な例](#complete_example)」セクションで定義されています。コントロールが `Positive` 状態から `Negative` 状態になったとき、またはその逆の場合、コントロールは `Focused` 状態または `Unfocused` 状態のままになります。

コントロールの状態が変わる可能性がある一般的な場所には、次の3つがあります。

- <xref:System.Windows.Controls.ControlTemplate> が <xref:System.Windows.Controls.Control>に適用される場合。

- プロパティが変更したとき。

- イベントが発生したとき。

次の例は、このような場合に `NumericUpDown` コントロールの状態を更新する方法を示しています。

<xref:System.Windows.Controls.ControlTemplate> が適用されたときにコントロールが正しい状態で表示されるように、<xref:System.Windows.FrameworkElement.OnApplyTemplate%2A> メソッドでコントロールの状態を更新する必要があります。 次の例では、<xref:System.Windows.FrameworkElement.OnApplyTemplate%2A> の `UpdateStates` を呼び出して、コントロールが適切な状態であることを確認します。  たとえば、`NumericUpDown` コントロールを作成し、その <xref:System.Windows.Controls.Control.Foreground%2A> を緑色に、`Value` を-5 に設定したとします。  <xref:System.Windows.Controls.ControlTemplate> が `NumericUpDown` コントロールに適用されているときに `UpdateStates` を呼び出さない場合、コントロールは `Negative` 状態ではなく、値は赤ではなく緑色になります。  コントロールを `Negative` 状態にするには、`UpdateStates` を呼び出す必要があります。

[!code-csharp[VSMCustomControl#ApplyTemplate](~/samples/snippets/csharp/VS_Snippets_Wpf/vsmcustomcontrol/csharp/numericupdown.cs#applytemplate)]
[!code-vb[VSMCustomControl#ApplyTemplate](~/samples/snippets/visualbasic/VS_Snippets_Wpf/vsmcustomcontrol/visualbasic/numericupdown.vb#applytemplate)]

プロパティが変更されたときに、多くの場合、コントロールの状態を更新する必要があります。 次の例は `ValueChangedCallback` メソッド全体を示しています。 `Value` が変更されると `ValueChangedCallback` が呼び出されるため、メソッドは `Value` を正の値から負の値に変更した場合、またはその逆の場合に `UpdateStates` を呼び出します。 `Value` 変更されても、正または負のままである場合は、`UpdateStates` を呼び出すことができます。その場合、コントロールは状態を変更しないためです。

[!code-csharp[VSMCustomControl#EntireValueChangedCallback](~/samples/snippets/csharp/VS_Snippets_Wpf/vsmcustomcontrol/csharp/numericupdown.cs#entirevaluechangedcallback)]
[!code-vb[VSMCustomControl#EntireValueChangedCallback](~/samples/snippets/visualbasic/VS_Snippets_Wpf/vsmcustomcontrol/visualbasic/numericupdown.vb#entirevaluechangedcallback)]

また、イベントが発生したときに状態を更新することが必要になる場合もあります。 次の例は、`NumericUpDown` が <xref:System.Windows.UIElement.GotFocus> イベントを処理するために <xref:System.Windows.Controls.Control> で `UpdateStates` を呼び出すことを示しています。

[!code-csharp[VSMCustomControl#OnGotFocus](~/samples/snippets/csharp/VS_Snippets_Wpf/vsmcustomcontrol/csharp/numericupdown.cs#ongotfocus)]
[!code-vb[VSMCustomControl#OnGotFocus](~/samples/snippets/visualbasic/VS_Snippets_Wpf/vsmcustomcontrol/visualbasic/numericupdown.vb#ongotfocus)]

<xref:System.Windows.VisualStateManager> は、コントロールの状態を管理するのに役立ちます。 <xref:System.Windows.VisualStateManager>を使用すると、コントロールが状態間で正しく遷移するようになります。  このセクションに記載されている推奨事項に従って <xref:System.Windows.VisualStateManager>を操作すると、コントロールのコードは読みやすく、保守も容易になります。

<a name="providing_the_control_contract"></a>

## <a name="providing-the-control-contract"></a>コントロールコントラクトの提供

テンプレートに配置する内容を <xref:System.Windows.Controls.ControlTemplate> の作成者が認識できるように、コントロールコントラクトを提供します。 コントロール コントラクトには、次の 3 つの要素があります。

- コントロールのロジックが使用する視覚的要素。

- コントロールの状態および各状態が所属するグループ。

- コントロールに対して視覚的に作用するパブリック プロパティ。

新しい <xref:System.Windows.Controls.ControlTemplate> を作成するユーザーは、コントロールのロジックで使用される <xref:System.Windows.FrameworkElement> オブジェクト、各オブジェクトの種類、およびその名前を知る必要があります。 また、<xref:System.Windows.Controls.ControlTemplate> 作成者は、コントロールが持つことができる各状態の名前と、状態の <xref:System.Windows.VisualStateGroup> を把握している必要があります。

`NumericUpDown` の例に戻るために、コントロールは、<xref:System.Windows.Controls.ControlTemplate> に次の <xref:System.Windows.FrameworkElement> オブジェクトがあることを想定しています。

- `UpButton`と呼ばれる <xref:System.Windows.Controls.Primitives.RepeatButton>。

- という <xref:System.Windows.Controls.Primitives.RepeatButton> `DownButton.`

 コントロールは、次の状態になります。

- `ValueStates`<xref:System.Windows.VisualStateGroup>

  - `Positive`

  - `Negative`

- `FocusStates`<xref:System.Windows.VisualStateGroup>

  - `Focused`

  - `Unfocused`

コントロールが必要とする <xref:System.Windows.FrameworkElement> オブジェクトを指定するには、予期される要素の名前と型を指定する <xref:System.Windows.TemplatePartAttribute>を使用します。  コントロールの状態を指定するには、<xref:System.Windows.TemplateVisualStateAttribute>を使用します。これは、状態の名前と、それが属する <xref:System.Windows.VisualStateGroup> を指定します。  <xref:System.Windows.TemplatePartAttribute> と <xref:System.Windows.TemplateVisualStateAttribute> をコントロールのクラス定義に配置します。

コントロールの外観に影響するすべてのパブリックプロパティは、コントロールコントラクトの一部でもあります。

次の例では、`NumericUpDown` コントロールの <xref:System.Windows.FrameworkElement> オブジェクトと状態を指定しています。

[!code-csharp[VSMCustomControl#ControlContract](~/samples/snippets/csharp/VS_Snippets_Wpf/vsmcustomcontrol/csharp/numericupdown.cs#controlcontract)]
[!code-vb[VSMCustomControl#ControlContract](~/samples/snippets/visualbasic/VS_Snippets_Wpf/vsmcustomcontrol/visualbasic/numericupdown.vb#controlcontract)]

<a name="complete_example"></a>

## <a name="complete-example"></a>コード例全体

次の例は、`NumericUpDown` コントロールの <xref:System.Windows.Controls.ControlTemplate> 全体を示しています。

[!code-xaml[VSMCustomControl#NUDTemplate](~/samples/snippets/csharp/VS_Snippets_Wpf/vsmcustomcontrol/csharp/themes/generic.xaml#nudtemplate)]

次の例は、`NumericUpDown`のロジックを示しています。

[!code-csharp[VSMCustomControl#ControlLogic](~/samples/snippets/csharp/VS_Snippets_Wpf/vsmcustomcontrol/csharp/numericupdown.cs#controllogic)]
[!code-vb[VSMCustomControl#ControlLogic](~/samples/snippets/visualbasic/VS_Snippets_Wpf/vsmcustomcontrol/visualbasic/numericupdown.vb#controllogic)]

## <a name="see-also"></a>参照

- [コントロールのテンプレートを作成する](../../../desktop-wpf/themes/how-to-create-apply-template.md)
- [コントロールのカスタマイズ](control-customization.md)
