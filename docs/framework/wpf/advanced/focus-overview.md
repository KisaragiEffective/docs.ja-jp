---
title: フォーカスの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- applications [WPF], focus
- focus in applications [WPF]
ms.assetid: 0230c4eb-0c8a-462b-ac4b-ae3e511659f4
ms.openlocfilehash: 8546516124b2156e02446e15d7bdcaf3cdbd717d
ms.sourcegitcommit: 4d8efe00f2e5ab42e598aff298d13b8c052d9593
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/16/2019
ms.locfileid: "68238608"
---
# <a name="focus-overview"></a>フォーカスの概要
[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、キーボード フォーカスと論理フォーカスという、フォーカスに関する 2 つの主要な概念があります。  キーボード フォーカスはキーボード入力を受け取る要素を指し、論理フォーカスはフォーカスを持つフォーカス範囲内の要素を指します。  これらの概念については、この概要で詳しく説明します。  フォーカスを取得可能な領域を複数持つ複雑なアプリケーションを作成する場合は、これらの概念の違いを理解することが重要です。  
  
 フォーカス管理に参加する主要なクラスは、<xref:System.Windows.Input.Keyboard>クラス、<xref:System.Windows.Input.FocusManager>クラス、および基本要素などのクラス、<xref:System.Windows.UIElement>と<xref:System.Windows.ContentElement>です。  基本要素の詳細については、「[基本要素の概要](base-elements-overview.md)」を参照してください。  
  
 <xref:System.Windows.Input.Keyboard>クラスでは、主にキーボード フォーカス、および<xref:System.Windows.Input.FocusManager>が論理フォーカスを中心に絶対の違いはありません。  キーボード フォーカスを持つ要素は論理フォーカスも持ちますが、論理フォーカスを持つ要素は必ずしもキーボード フォーカスを持ちません。  使用するときにこれが明らかなもの、<xref:System.Windows.Input.Keyboard>にキーボード フォーカスを持つ要素を設定するクラスが要素にも論理フォーカスを設定します。  

<a name="Keyboard_Focus"></a>   
## <a name="keyboard-focus"></a>キーボード フォーカス  
 キーボード フォーカスは、現在キーボード入力を受け取っている要素を指します。  キーボード フォーカスを持つ要素は、デスクトップ全体で 1 つしかありません。  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]では、 キーボード フォーカスを持つ要素の<xref:System.Windows.IInputElement.IsKeyboardFocused%2A>は`true`に設定されます。  静的プロパティ<xref:System.Windows.Input.Keyboard.FocusedElement%2A>上、<xref:System.Windows.Input.Keyboard>クラスは、現在キーボード フォーカスがある要素を取得します。  
  
 キーボード フォーカスを取得する要素の順序で、<xref:System.Windows.UIElement.Focusable%2A>と<xref:System.Windows.UIElement.IsVisible%2A>に基本要素のプロパティを設定する必要があります`true`します。  などのいくつかのクラス、<xref:System.Windows.Controls.Panel>基底クラスが<xref:System.Windows.UIElement.Focusable%2A>に設定`false`既定ではそのため、する必要があります設定<xref:System.Windows.UIElement.Focusable%2A>に`true`する場合はこのような要素にキーボード フォーカスを取得することになります。  
  
 キーボード フォーカスは、要素への Tab キーでの移動や特定の要素でのマウスのクリックなど、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] でのユーザー操作を通じて取得できます。  また、<xref:System.Windows.Input.Keyboard>クラスの<xref:System.Windows.Input.Keyboard.Focus%2A>メソッドを使用して、 プログラムでキーボード フォーカスを取得することもできます。  <xref:System.Windows.Input.Keyboard.Focus%2A>メソッドは、指定された要素にキーボード フォーカスを与えるよう試みます。  返される要素はキーボード フォーカスが設定された要素ですが、古いフォーカス オブジェクトまたは新しいフォーカス オブジェクトが要求をブロックする場合は、要求された要素とは異なる可能性があります。  
  
 次の例では、<xref:System.Windows.Input.Keyboard.Focus%2A>にキーボード フォーカスを設定する方法、<xref:System.Windows.Controls.Button>します。  
  
 [!code-csharp[focussample#FocusSampleSetFocus](~/samples/snippets/csharp/VS_Snippets_Wpf/FocusSample/CSharp/Window1.xaml.cs#focussamplesetfocus)]
 [!code-vb[focussample#FocusSampleSetFocus](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FocusSample/visualbasic/window1.xaml.vb#focussamplesetfocus)]  
  
 基本要素クラスの<xref:System.Windows.UIElement.IsKeyboardFocused%2A>プロパティは、その要素にキーボード フォーカスがあるかどうかを示す値を取得します。  基本要素クラスの<xref:System.Windows.UIElement.IsKeyboardFocusWithin%2A>プロパティは、その要素またはその子ビジュアル要素のいずれかにキーボード フォーカスがあるかどうかを示す値を取得します。  
  
 フォーカスを受け取る要素が、アプリケーションによって読み込まれる初期ウィンドウのビジュアル ツリーである必要がありますにアプリケーションの起動時に初期フォーカスを設定するときと、要素があります<xref:System.Windows.UIElement.Focusable%2A>と<xref:System.Windows.UIElement.IsVisible%2A>設定`true`します。  初期フォーカスを設定する推奨場所は、<xref:System.Windows.FrameworkElement.Loaded>イベント ハンドラー。  A<xref:System.Windows.Threading.Dispatcher>コールバックを呼び出すことによって使用も<xref:System.Windows.Threading.Dispatcher.Invoke%2A>または<xref:System.Windows.Threading.Dispatcher.BeginInvoke%2A>します。  
  
<a name="Logical_Focus"></a>   
## <a name="logical-focus"></a>論理フォーカス  
 論理フォーカスは、フォーカス スコープ内の<xref:System.Windows.Input.FocusManager.FocusedElement%2A?displayProperty=nameWithType>を指します。  フォーカス範囲を追跡する要素とは、<xref:System.Windows.Input.FocusManager.FocusedElement%2A>そのスコープ内で。  キーボード フォーカスがフォーカス範囲を離れると、フォーカスがある要素はキーボード フォーカスを失いますが、論理フォーカスは引き続き保持します。  キーボード フォーカスがフォーカス範囲に戻ると、フォーカスがある要素はキーボード フォーカスを取得します。  これにより、キーボード フォーカスが複数のフォーカス範囲間で変更されても、フォーカスがフォーカス範囲に戻ると、そのフォーカス範囲内のフォーカスがある要素はキーボード フォーカスを取り戻すことができます。  
  
 アプリケーション内の複数の要素が論理フォーカスを持つことがありますが、特定のフォーカス スコープで論理フォーカスを持つ要素は 1 つだけに限られます。  
  
 キーボード フォーカスを持つ要素は、その要素が属するフォーカス範囲の論理フォーカスを持ちます。  
  
 フォーカス範囲内に要素を変換できます[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]を設定して、<xref:System.Windows.Input.FocusManager>添付プロパティ<xref:System.Windows.Input.FocusManager.IsFocusScope%2A>に`true`します。  コードでは、要素に変換できますフォーカス範囲を呼び出して<xref:System.Windows.Input.FocusManager.SetIsFocusScope%2A>します。  
  
 次の例では、<xref:System.Windows.Controls.StackPanel>の<xref:System.Windows.Input.FocusManager.IsFocusScope%2A>添付プロパティを設定してフォーカス スコープに変換しています。  
  
 [!code-xaml[MarkupSnippets#MarkupIsFocusScopeXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/MarkupSnippets/CSharp/Window1.xaml#markupisfocusscopexaml)]  
  
 [!code-csharp[FocusSnippets#FocusSetIsFocusScope](~/samples/snippets/csharp/VS_Snippets_Wpf/FocusSnippets/CSharp/Window1.xaml.cs#focussetisfocusscope)]
 [!code-vb[FocusSnippets#FocusSetIsFocusScope](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FocusSnippets/visualbasic/window1.xaml.vb#focussetisfocusscope)]  
  
 <xref:System.Windows.Input.FocusManager.GetFocusScope%2A> は指定した要素のフォーカス スコープを返します。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]で、デフォルトでフォーカス スコープであるクラスは、<xref:System.Windows.Window>、 <xref:System.Windows.Controls.MenuItem>、 <xref:System.Windows.Controls.ToolBar>、<xref:System.Windows.Controls.ContextMenu>です。  
  
 <xref:System.Windows.Input.FocusManager.GetFocusedElement%2A> は、指定したフォーカス スコープのフォーカスがある要素を取得します。  <xref:System.Windows.Input.FocusManager.SetFocusedElement%2A> は、指定したフォーカスのスコープでフォーカスのある要素を設定します。  <xref:System.Windows.Input.FocusManager.SetFocusedElement%2A>は通常、初期フォーカスのある要素の設定に使用されます。  
  
 フォーカス範囲にフォーカスを持つ要素を設定し、フォーカス範囲のフォーカスを持つ要素を取得する例を次に示します。  
  
 [!code-csharp[FocusSnippets#FocusGetSetFocusedElement](~/samples/snippets/csharp/VS_Snippets_Wpf/FocusSnippets/CSharp/Window1.xaml.cs#focusgetsetfocusedelement)]
 [!code-vb[FocusSnippets#FocusGetSetFocusedElement](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FocusSnippets/visualbasic/window1.xaml.vb#focusgetsetfocusedelement)]  
  
<a name="Keyboard_Navigation"></a>   
## <a name="keyboard-navigation"></a>キーボード ナビゲーション  
 <xref:System.Windows.Input.KeyboardNavigation>クラスは、ナビゲーション キーのいずれかが押されたときに既定のキーボード フォーカスのナビゲーションを実装する責任を負います。  ナビゲーション キーは次のとおりです。タブ、SHIFT + TAB、CTRL + TAB、CTRL + SHIFT + TAB、上方向、下方向、LEFTARROW、および右矢印キー。  
  
 <xref:System.Windows.Input.KeyboardNavigation>の添付プロパティである<xref:System.Windows.Input.KeyboardNavigation.TabNavigation%2A>、 <xref:System.Windows.Input.KeyboardNavigation.ControlTabNavigation%2A>、<xref:System.Windows.Input.KeyboardNavigation.DirectionalNavigation%2A>を設定することにより、ナビゲーション コンテナーの動作を変更できます。  これらのプロパティは<xref:System.Windows.Input.KeyboardNavigationMode>型であり、<xref:System.Windows.Input.KeyboardNavigationMode.Continue>、 <xref:System.Windows.Input.KeyboardNavigationMode.Local>、 <xref:System.Windows.Input.KeyboardNavigationMode.Contained>、 <xref:System.Windows.Input.KeyboardNavigationMode.Cycle>、 <xref:System.Windows.Input.KeyboardNavigationMode.Once>、<xref:System.Windows.Input.KeyboardNavigationMode.None>の値を取れます。  既定値は、要素はナビゲーション コンテナーでないことを示す<xref:System.Windows.Input.KeyboardNavigationMode.Continue>です。  
  
 次の例では、作成、<xref:System.Windows.Controls.Menu>の数が<xref:System.Windows.Controls.MenuItem>オブジェクト。  <xref:System.Windows.Input.KeyboardNavigation.TabNavigation%2A>添付プロパティに設定されて<xref:System.Windows.Input.KeyboardNavigationMode.Cycle>上、<xref:System.Windows.Controls.Menu>します。  内で tab キーを使用して、フォーカスが変更されたときに、<xref:System.Windows.Controls.Menu>最初の要素にフォーカスが戻りますの最後の要素に達すると、各要素からフォーカスが移動されます。  
  
 [!code-xaml[MarkupSnippets#MarkupKeyboardNavigationTabNavigationXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/MarkupSnippets/CSharp/Window1.xaml#markupkeyboardnavigationtabnavigationxaml)]  
  
 [!code-csharp[MarkupSnippets#MarkupKeyboardNavigationTabNavigationCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/MarkupSnippets/CSharp/Window1.xaml.cs#markupkeyboardnavigationtabnavigationcode)]
 [!code-vb[MarkupSnippets#MarkupKeyboardNavigationTabNavigationCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/MarkupSnippets/visualbasic/window1.xaml.vb#markupkeyboardnavigationtabnavigationcode)]  
  
<a name="Manipulating_Focus_Programmatically"></a>   
## <a name="navigating-focus-programmatically"></a>プログラムによるフォーカスのナビゲーション  
 フォーカスを使用する追加の API は<xref:System.Windows.UIElement.MoveFocus%2A>と<xref:System.Windows.UIElement.PredictFocus%2A>します。  
  
 <xref:System.Windows.FrameworkElement.MoveFocus%2A>は、アプリケーション内で次の要素にフォーカスを移動します。  方向を指定するために<xref:System.Windows.Input.TraversalRequest>を使用します。   <xref:System.Windows.UIElement.MoveFocus%2A>に渡される<xref:System.Windows.Input.FocusNavigationDirection>は、フォーカスが移動可能な方向を示します。例えば、<xref:System.Windows.Input.FocusNavigationDirection.First>、 <xref:System.Windows.Input.FocusNavigationDirection.Last>、<xref:System.Windows.Input.FocusNavigationDirection.Up>、<xref:System.Windows.Input.FocusNavigationDirection.Down>などです。  
  
 次の例では、<xref:System.Windows.FrameworkElement.MoveFocus%2A>を使ってフォーカスのある要素を変更します。  
  
 [!code-csharp[focussample#FocusSampleMoveFocus](~/samples/snippets/csharp/VS_Snippets_Wpf/FocusSample/CSharp/Window1.xaml.cs#focussamplemovefocus)]
 [!code-vb[focussample#FocusSampleMoveFocus](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FocusSample/visualbasic/window1.xaml.vb#focussamplemovefocus)]  
  
 <xref:System.Windows.FrameworkElement.PredictFocus%2A>は、フォーカスが変更された場合にフォーカスを受け取るオブジェクトを返します。  現時点では、<xref:System.Windows.FrameworkElement.PredictFocus%2A>によってサポートされているのは、<xref:System.Windows.Input.FocusNavigationDirection.Up>、<xref:System.Windows.Input.FocusNavigationDirection.Down>、<xref:System.Windows.Input.FocusNavigationDirection.Left>、および <xref:System.Windows.Input.FocusNavigationDirection.Right> だけです。  
  
<a name="Focus_Events"></a>   
## <a name="focus-events"></a>フォーカス イベント  
 キーボード フォーカスに関連するイベントは、<xref:System.Windows.Input.Keyboard.PreviewGotKeyboardFocus>、<xref:System.Windows.Input.Keyboard.GotKeyboardFocus>、<xref:System.Windows.Input.Keyboard.PreviewLostKeyboardFocus>、<xref:System.Windows.Input.Keyboard.LostKeyboardFocus>です。  これらのイベントは<xref:System.Windows.Input.Keyboard>クラスの添付イベントとして定義されていますが、対応する基本要素クラスのルーティング イベントとしてより簡単にアクセスできます。  イベントの詳細については、「[ルーティング イベントの概要](routed-events-overview.md)」を参照してください。  
  
 <xref:System.Windows.Input.Keyboard.GotKeyboardFocus>は、要素がキーボード フォーカスを取得したときに発生します。  <xref:System.Windows.Input.Keyboard.LostKeyboardFocus>は、要素がキーボード フォーカスを失ったときに発生します。  <xref:System.Windows.Input.Keyboard.PreviewGotKeyboardFocus>イベントや<xref:System.Windows.Input.Keyboard.PreviewLostKeyboardFocusEvent>イベントが処理されて、<xref:System.Windows.RoutedEventArgs.Handled%2A>が`true`に設定されたなら、フォーカスは変更されません。  
  
 次の例では、<xref:System.Windows.UIElement.GotKeyboardFocus>と<xref:System.Windows.UIElement.LostKeyboardFocus>のイベント ハンドラーを、<xref:System.Windows.Controls.TextBox>にアタッチしています。  
  
 [!code-xaml[keyboardsample#KeyboardSampleXAMLHandlerHookup](~/samples/snippets/csharp/VS_Snippets_Wpf/KeyboardSample/CSharp/Window1.xaml#keyboardsamplexamlhandlerhookup)]  
  
 ときに、<xref:System.Windows.Controls.TextBox>キーボード フォーカス、<xref:System.Windows.Controls.Control.Background%2A>のプロパティ、<xref:System.Windows.Controls.TextBox>に変更されます<xref:System.Windows.Media.Brushes.LightBlue%2A>します。  
  
 [!code-csharp[keyboardsample#KeyboardSampleGotFocus](~/samples/snippets/csharp/VS_Snippets_Wpf/KeyboardSample/CSharp/Window1.xaml.cs#keyboardsamplegotfocus)]
 [!code-vb[keyboardsample#KeyboardSampleGotFocus](~/samples/snippets/visualbasic/VS_Snippets_Wpf/KeyboardSample/visualbasic/window1.xaml.vb#keyboardsamplegotfocus)]  
  
 <xref:System.Windows.Controls.TextBox> がキーボードフォーカスを失うと、<xref:System.Windows.Controls.TextBox> の <xref:System.Windows.Controls.Control.Background%2A> プロパティが白に戻されます。  
  
 [!code-csharp[keyboardsample#KeyboardSampleLostFocus](~/samples/snippets/csharp/VS_Snippets_Wpf/KeyboardSample/CSharp/Window1.xaml.cs#keyboardsamplelostfocus)]
 [!code-vb[keyboardsample#KeyboardSampleLostFocus](~/samples/snippets/visualbasic/VS_Snippets_Wpf/KeyboardSample/visualbasic/window1.xaml.vb#keyboardsamplelostfocus)]  
  
 論理フォーカスに関連するイベントは、<xref:System.Windows.UIElement.GotFocus>と<xref:System.Windows.UIElement.LostFocus>です。  これらのイベントは<xref:System.Windows.Input.FocusManager>の添付イベントとして定義されていますが、<xref:System.Windows.Input.FocusManager> はイベントの CLR ラッパーを公開していません。  <xref:System.Windows.UIElement>および<xref:System.Windows.ContentElement>が、これらのイベントをより簡単に公開しています。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Input.FocusManager>
- <xref:System.Windows.UIElement>
- <xref:System.Windows.ContentElement>
- [入力の概要](input-overview.md)
- [基本要素の概要](base-elements-overview.md)
