---
title: コントロールのフォーカスのスタイルと FocusVisualStyle
ms.date: 03/30/2017
helpviewer_keywords:
- keyboard focus [WPF]
- focus [WPF], visual styling
- styles [WPF], focus visual style
ms.assetid: 786ac576-011b-4d72-913b-558deccb9b35
ms.openlocfilehash: 988b084144532df6f7a6073184fcf035b0813bfd
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458679"
---
# <a name="styling-for-focus-in-controls-and-focusvisualstyle"></a>コントロールのフォーカスのスタイルと FocusVisualStyle
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] には、キーボードフォーカスを受け取ったときのコントロールの外観を変更するための2つの並列機構が用意されています。 1つ目のメカニズムでは、コントロールに適用されるスタイルまたはテンプレート内の <xref:System.Windows.UIElement.IsKeyboardFocused%2A> などのプロパティのプロパティセッターを使用します。 2番目のメカニズムは、<xref:System.Windows.FrameworkElement.FocusVisualStyle%2A> プロパティの値として別のスタイルを提供することです。"focus visual スタイル" は、コントロールまたはその他の UI 要素を置き換えることで、コントロールの上に描画する装飾用の独立したビジュアルツリーを作成します。 このトピックでは、これらの各メカニズムが適切であるシナリオについて説明します。  

<a name="Purpose"></a>   
## <a name="the-purpose-of-focus-visual-style"></a>フォーカスの視覚スタイルの目的  
 Visual スタイルのフォーカス機能は、UI 要素へのキーボードナビゲーションに基づくビジュアルユーザーフィードバックを導入するための一般的な "オブジェクトモデル" を提供します。 これは、新しいテンプレートをコントロールに適用したり、特定のテンプレートの構成を把握したりすることなく実行できます。  
  
 ただし、visual スタイルのフォーカス機能はコントロールテンプレートを知らなくても動作するため、フォーカスのある視覚スタイルを使用しているコントロールに表示できるビジュアルフィードバックは必ずしも制限されています。 この機能の実際の動作は、テンプレートを使用してコントロールのレンダリングによって作成された、ビジュアルツリーの上に別のビジュアルツリー (装飾) を重ね合わせることです。 この個別のビジュアルツリーは、<xref:System.Windows.FrameworkElement.FocusVisualStyle%2A> プロパティを満たすスタイルを使用して定義します。  
  
<a name="Default"></a>   
## <a name="default-focus-visual-style-behavior"></a>既定のフォーカスのビジュアルスタイル動作  
 フォーカスの visual スタイルは、フォーカスアクションがキーボードによって開始された場合にのみ機能します。 マウス操作またはプログラムによるフォーカスの変更により、フォーカスの視覚スタイルのモードが無効になります。 フォーカスモードの違いの詳細については、「[フォーカスの概要](focus-overview.md)」を参照してください。  
  
 コントロールのテーマには、テーマ内のすべてのコントロールのフォーカスの視覚スタイルになる、既定のフォーカスのビジュアルスタイルの動作が含まれます。 このテーマスタイルは、静的なキー <xref:System.Windows.SystemParameters.FocusVisualStyleKey%2A>の値によって識別されます。 アプリケーションレベルで独自のフォーカスの視覚スタイルを宣言する場合は、この既定のスタイルの動作をテーマから置き換えます。 または、テーマ全体を定義する場合は、この同じキーを使用して、テーマ全体の既定の動作のスタイルを定義する必要があります。  
  
 テーマでは、通常、既定のフォーカスの視覚スタイルは非常に単純です。 大まかな概算は次のとおりです。  
  
```xaml  
<Style x:Key="{x:Static SystemParameters.FocusVisualStyleKey}">  
  <Setter Property="Control.Template">  
    <Setter.Value>  
      <ControlTemplate>  
        <Rectangle StrokeThickness="1"  
          Stroke="Black"  
          StrokeDashArray="1 2"  
          SnapsToDevicePixels="true"/>  
      </ControlTemplate>  
    </Setter.Value>  
  </Setter>  
</Style>  
```  
  
<a name="When"></a>   
## <a name="when-to-use-focus-visual-styles"></a>フォーカスの視覚スタイルを使用する場合  
 概念的には、コントロールに適用されるフォーカスの視覚スタイルの外観は、コントロールからコントロールまで一貫している必要があります。 一貫性を確保する方法の1つとして、テーマ全体を作成する場合にのみ、フォーカスのビジュアルスタイルを変更します。テーマに定義されている各コントロールは、非常に同じフォーカスの表示スタイルを取得します。または、コントロールから継続して表示されるスタイルのバリエーションを取得します。. また、同じスタイル (または同様のスタイル) を使用して、ページまたは UI のすべてのキーボードフォーカスがある要素のスタイルを作成することもできます。  
  
 テーマの一部ではない個々のコントロールスタイルに <xref:System.Windows.FrameworkElement.FocusVisualStyle%2A> を設定することは、フォーカスの視覚スタイルの使用には適していません。 これは、コントロール間の視覚的な動作が一貫していないと、キーボードフォーカスに関してユーザーエクスペリエンスが混乱する可能性があるためです。 テーマ全体で一貫していないキーボードフォーカスに対してコントロール固有の動作を行う場合は、<xref:System.Windows.UIElement.IsFocused%2A> や <xref:System.Windows.UIElement.IsKeyboardFocused%2A>など、個々の入力状態プロパティのスタイルでトリガーを使用する方がはるかに適しています。  
  
 フォーカスの visual スタイルは、キーボードフォーカス専用です。 そのため、フォーカスの視覚的なスタイルは、ユーザー補助機能の一種です。 マウス、キーボード、またはプログラムによってどのような種類のフォーカスにも UI を変更する場合は、フォーカスの視覚スタイルを使用しないでください。代わりに、一般的なフォーカスプロパティの値から動作するスタイルまたはテンプレートで setter とトリガーを使用する必要があります。<xref:System.Windows.UIElement.IsFocused%2A> や <xref:System.Windows.UIElement.IsKeyboardFocusWithin%2A>などです。  
  
<a name="How"></a>   
## <a name="how-to-create-a-focus-visual-style"></a>フォーカスのある視覚スタイルを作成する方法  
 フォーカスのある視覚スタイル用に作成するスタイルには、常に <xref:System.Windows.Controls.Control>の <xref:System.Windows.Style.TargetType%2A> が必要です。 このスタイルは、主に <xref:System.Windows.Controls.ControlTemplate>で構成されている必要があります。 対象の型は、フォーカスの visual スタイルが <xref:System.Windows.FrameworkElement.FocusVisualStyle%2A>に割り当てられている型として指定しないでください。  
  
 ターゲットの型は常に <xref:System.Windows.Controls.Control>ので、すべてのコントロールに共通のプロパティを使用してスタイルを設定する必要があります (<xref:System.Windows.Controls.Control> クラスとその基本クラスのプロパティを使用します)。 UI 要素へのオーバーレイとして正しく機能し、コントロールの機能領域が不明瞭にならないテンプレートを作成する必要があります。 一般に、ビジュアルフィードバックはコントロールの余白の外側に表示されるか、またはフォーカスのある視覚スタイルが適用されているコントロールのヒットテストをブロックしない一時的または控えめな効果として表示されることを意味します。 オーバーレイテンプレートのサイズと位置を決定するのに役立つテンプレートバインドで使用できるプロパティには、<xref:System.Windows.FrameworkElement.ActualHeight%2A>、<xref:System.Windows.FrameworkElement.ActualWidth%2A>、<xref:System.Windows.FrameworkElement.Margin%2A>、および <xref:System.Windows.Controls.Control.Padding%2A>があります。  
  
<a name="Alternatives"></a>   
## <a name="alternatives-to-using-a-focus-visual-style"></a>フォーカスの視覚スタイルを使用する代替手段  
 1つのコントロールのスタイルを設定するだけであるため、またはコントロールテンプレートをより細かく制御する必要があるために、フォーカスの視覚スタイルを使用することが適切でない場合は、ビジュアルを作成できる他の多くのアクセス可能なプロパティと手法があります。フォーカスの変化に応じた動作。  
  
 トリガー、セッター、およびイベントセッターの詳細については、「[スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)」をご覧ください。 ルーティングイベントの処理については、 [「ルーティングイベントの概要](routed-events-overview.md)」で説明されています。  
  
### <a name="iskeyboardfocused"></a>IsKeyboardFocused  
 特にキーボードフォーカスを知りたい場合は、プロパティ <xref:System.Windows.Trigger>に対して <xref:System.Windows.UIElement.IsKeyboardFocused%2A> 依存関係プロパティを使用できます。 スタイルまたはテンプレートのプロパティトリガーは、キーボードフォーカスの動作を定義するためのより適切な手法であり、1つのコントロールに対してのみ使用され、他のコントロールのキーボードフォーカスの動作を視覚的に一致させることはできません。  
  
 もう1つの類似した依存関係プロパティも <xref:System.Windows.UIElement.IsKeyboardFocusWithin%2A>ます。これは、キーボードフォーカスが複合内またはコントロールの機能領域内のどこかにあることを視覚的に呼び出す場合に適しています。 たとえば、複数のコントロールをグループ化するパネルが、そのパネル内の個々の要素に対してより正確に表示される場合でも、複数のコントロールをグループ化するパネルが異なるように表示されるように <xref:System.Windows.UIElement.IsKeyboardFocusWithin%2A> トリガーを配置できます。  
  
 また、イベント <xref:System.Windows.UIElement.GotKeyboardFocus> と <xref:System.Windows.UIElement.LostKeyboardFocus> (およびそのプレビューに相当するもの) を使用することもできます。 これらのイベントは、<xref:System.Windows.EventSetter>の基礎として使用することも、分離コードでイベントのハンドラーを記述することもできます。  
  
### <a name="other-focus-properties"></a>その他のフォーカスプロパティ  
 フォーカスを変更して視覚的な動作を生成する可能性のあるすべての原因を確認するには、setter または trigger を <xref:System.Windows.UIElement.IsFocused%2A> 依存関係プロパティに基づいて作成するか、<xref:System.Windows.EventSetter>に使用される <xref:System.Windows.UIElement.GotFocus> または <xref:System.Windows.UIElement.LostFocus> イベントの基にする必要があります。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.FrameworkElement.FocusVisualStyle%2A>
- [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
- [フォーカスの概要](focus-overview.md)
- [入力の概要](input-overview.md)
