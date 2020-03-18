---
title: 入力の概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- commands [WPF]
- input [WPF], overview
- keyboard focus [WPF]
- keyboard input [WPF]
- touch events [WPF]
- event routing [WPF]
- touch input [WPF]
- manipulation [WPF]
- logical focus [WPF]
- stylus input [WPF]
- text input [WPF]
- input events [WPF], handling
- WPF [WPF], input overview
- manipulation events [WPF]
- mouse input [WPF]
- mouse capture [WPF]
- focus [WPF]
- mouse position [WPF]
ms.assetid: ee5258b7-6567-415a-9b1c-c0cbe46e79ef
ms.openlocfilehash: a90c74542c1a6604ed27d37f882385b67402dd92
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005032"
---
# <a name="input-overview"></a>入力の概要
<a name="introduction"></a>[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] サブシステムは、マウス、キーボード、タッチ、スタイラスなど、さまざまなデバイスからの入力を取得するための強力な API を提供します。 このトピックでは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] で提供されるサービスと、入力システムのアーキテクチャについて説明します。

<a name="input_api"></a>
## <a name="input-api"></a>入力 API
 プライマリ入力 API の露出は、基本要素クラス (<xref:System.Windows.UIElement>、<xref:System.Windows.ContentElement>、<xref:System.Windows.FrameworkElement>、および <xref:System.Windows.FrameworkContentElement>にあります。  基本要素の詳細については、「[基本要素の概要](base-elements-overview.md)」を参照してください。  これらのクラスは、キー操作、マウス ボタン、マウス ホイール、マウス動作、フォーカス管理、マウス キャプチャなどに関連する入力イベントの機能を提供しています。 入力アーキテクチャでは、すべての入力イベントをサービスとして扱うのではなく、基本要素に入力 API を配置することで、入力イベントを UI 内の特定のオブジェクトによって供給され、複数の要素が opp を持つイベントルーティングスキームをサポートすることができます。入力イベントを処理します。 多くの入力イベントには、それぞれに関連付けられたペアとなるイベントがあります。  たとえば、キー ダウンのイベントには、<xref:System.Windows.Input.Keyboard.KeyDown>と<xref:System.Windows.Input.Keyboard.PreviewKeyDown>イベントが関連付けられています。  これらのイベントの違いは、ターゲット要素にルーティングされる方法です。  プレビュー イベントは、ルート要素からターゲット要素へ、要素ツリーを下位に向かいます (トンネル)。  バブル イベントは、ターゲット要素からルート要素へ、上位に向かいます (バブル)。  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のイベント ルーティングについては、この概要の後半、および「[ルーティング イベントの概要](routed-events-overview.md)」でさらに詳しく説明されています。

### <a name="keyboard-and-mouse-classes"></a>Keyboard クラスと Mouse クラス
 <xref:System.Windows.Input.Keyboard> クラスと <xref:System.Windows.Input.Mouse> クラスは、基本要素クラスの入力 API に加えて、キーボードおよびマウス入力を操作するための追加の API を提供します。

 <xref:System.Windows.Input.Keyboard> クラスの入力 API の例としては、現在押されている <xref:System.Windows.Input.ModifierKeys> を返す <xref:System.Windows.Input.Keyboard.Modifiers%2A> プロパティと、指定されたキーが押されたかどうかを判断する <xref:System.Windows.Input.Keyboard.IsKeyDown%2A> メソッドがあります。

 次の例では、<xref:System.Windows.Input.Keyboard.GetKeyStates%2A>メソッドを用いて、ある<xref:System.Windows.Input.Key>が押された状態かどうかを調べます。

 [!code-csharp[keyargssnippetsample#KeyEventArgsKeyBoardGetKeyStates](~/samples/snippets/csharp/VS_Snippets_Wpf/KeyArgsSnippetSample/CSharp/Window1.xaml.cs#keyeventargskeyboardgetkeystates)]
 [!code-vb[keyargssnippetsample#KeyEventArgsKeyBoardGetKeyStates](~/samples/snippets/visualbasic/VS_Snippets_Wpf/KeyArgsSnippetSample/visualbasic/window1.xaml.vb#keyeventargskeyboardgetkeystates)]

 <xref:System.Windows.Input.Mouse> クラスの入力 API の例としては、マウスの中央ボタンの状態を取得し、マウスポインターの現在の要素を取得する <xref:System.Windows.Input.Mouse.DirectlyOver%2A>の <xref:System.Windows.Input.Mouse.MiddleButton%2A>があります。

 次の例では、マウスの<xref:System.Windows.Input.Mouse.LeftButton%2A>が<xref:System.Windows.Input.MouseButtonState.Pressed>の状態かどうかを調べています。

 [!code-csharp[mouserelatedsnippets#MouseRelatedSnippetsGetLeftButtonMouse](~/samples/snippets/csharp/VS_Snippets_Wpf/MouseRelatedSnippets/CSharp/Window1.xaml.cs#mouserelatedsnippetsgetleftbuttonmouse)]
 [!code-vb[mouserelatedsnippets#MouseRelatedSnippetsGetLeftButtonMouse](~/samples/snippets/visualbasic/VS_Snippets_Wpf/MouseRelatedSnippets/visualbasic/window1.xaml.vb#mouserelatedsnippetsgetleftbuttonmouse)]

 <xref:System.Windows.Input.Mouse>クラスと<xref:System.Windows.Input.Keyboard>クラスについては、この記事でさらに詳細を扱います。

### <a name="stylus-input"></a>スタイラス入力
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、<xref:System.Windows.Input.Stylus>のサポートが統合されています。  <xref:System.Windows.Input.Stylus> は、Tablet PC で広く使われているペン入力です。  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションは、マウス API を使用してスタイラスをマウスとして扱うことができますが、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、キーボードやマウスと同様のモデルを使用するスタイラスデバイスの抽象化も公開します。  スタイラス関連のすべての Api には、"スタイラス" という単語が含まれています。

 スタイラスはマウスとして動作できるため、マウス入力のみをサポートするアプリケーションでも、ある程度のスタイラス入力が自動的にサポートされます。 スタイラスがこのような手法で使用される場合、アプリケーションでは、適切なスタイラス イベントを処理する機会が与えられた後に、対応するマウス イベントを処理します。 さらに、インク入力などのより高レベルなサービスも、スタイラス デバイスの抽象型を使って利用できます。  入力としてのインクの詳細については、「[インクの概要](getting-started-with-ink.md)」を参照してください。

<a name="event_routing"></a>
## <a name="event-routing"></a>イベント ルーティング
 <xref:System.Windows.FrameworkElement> には、要素のツリーを形成する他の要素をコンテンツモデル内の子要素として含めることができます。  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、イベントを処理することで、親要素が、その子要素またはその他の子孫に命令された入力に関与できます。 これは、小さいコントロールからコントロールをビルドする場合に特に役立ちます。このプロセスは "コントロール合成" または "合成" と呼ばれます。 要素ツリー、および要素ツリーをイベント ルートに関連させる方法の詳細については、「[WPF のツリー](trees-in-wpf.md)」を参照してください。

 イベント ルーティングは、ルートに沿った特定のオブジェクトや要素が、異なる要素によって供給されたイベントに、(処理を介して) 重要な応答を提供できるように、イベントを複数の要素に転送するプロセスです。  ルーティング イベントには、直接、バブル、トンネルのいずれかのルーティング メカニズムが使用されます。  直接ルーティングでは、ソース要素が通知を受ける唯一の要素であり、イベントは他の要素にルーティングされません。 ただし、直接ルーティングイベントには、標準の CLR イベントとは対照的に、ルーティングイベントにのみ存在する追加の機能もいくつか用意されています。 バブル ルーティングでは、最初にイベントの発生元である要素に通知し、次にその親要素へ、その次へと順に通知することで、要素ツリーの上位へ処理が実行されます。  トンネル ルーティングでは、要素ツリーのルートから始まり、下位へと処理が実行され、元のソース要素で終了します。  ルーティング イベントの詳細については、「[ルーティング イベントの概要](routed-events-overview.md)」を参照してください。

 一般に、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の入力イベントは、トンネル イベントとバブル イベントのペアで構成されます。  トンネリング イベントは、"Preview" プレフィックスでバブルリング イベントと区別されます。  たとえば、<xref:System.Windows.Input.Mouse.PreviewMouseMove> はマウス移動イベントのトンネリングバージョンであり、<xref:System.Windows.Input.Mouse.MouseMove> はこのイベントのバブルバージョンです。 このイベントのペアは、要素レベルで実装される規則であり、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] イベント システムの継承機能ではありません。 詳細については、「[ルーティング イベントの概要](routed-events-overview.md)」の「WPF の入力イベント」を参照してください。

<a name="handling_input_events"></a>
## <a name="handling-input-events"></a>入力イベントの処理
 要素で入力を受け取るには、イベント ハンドラーをその特定のイベントに関連付ける必要があります。  [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] では、これは簡単です。イベントの名前を、このイベントをリッスンする要素の属性として参照します。  次に、属性の値を、デリゲートに基づいて、定義するイベント ハンドラーの名前に設定します。  イベントハンドラーは、などC#のコードで記述する必要があり、分離コードファイルに含めることができます。

 キーボード イベントは、キーボード フォーカスが要素上にある状態で、オペレーティング システムがキー操作を報告すると発生します。 マウス イベントとスタイラス イベントはそれぞれ、要素との相対的なポインター位置の変化を報告するイベントと、 ボタンの状態の変更を報告するイベントの 2 つのカテゴリに分類されます。

### <a name="keyboard-input-event-example"></a>キーボード入力イベントの例
 次の例では、左方向キーが押下されるのをリッスンします。  <xref:System.Windows.Controls.StackPanel>を含む<xref:System.Windows.Controls.Button>が作成されます。  左矢印キーの押下をリッスンするイベント ハンドラーは、<xref:System.Windows.Controls.Button>インスタンスに関連付けられます。

 例の最初のセクションでは、<xref:System.Windows.Controls.StackPanel>と<xref:System.Windows.Controls.Button>を作成し、 <xref:System.Windows.UIElement.KeyDown>のイベント ハンドラーをアタッチしています。

 [!code-xaml[InputOvw#Input_OvwKeyboardExampleXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml#input_ovwkeyboardexamplexaml)]

 [!code-csharp[InputOvw#Input_OvwKeyboardExampleUICodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml.cs#input_ovwkeyboardexampleuicodebehind)]
 [!code-vb[InputOvw#Input_OvwKeyboardExampleUICodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InputOvw/VisualBasic/Page1.xaml.vb#input_ovwkeyboardexampleuicodebehind)]

 2 番目のセクションは、コード内に記述され、イベント ハンドラーを定義しています。  左方向キーが押下されたときに<xref:System.Windows.Controls.Button>がキーボード フォーカスを持っていれば、 イベント ハンドラーが実行され、<xref:System.Windows.Controls.Control.Background%2A>の<xref:System.Windows.Controls.Button>の色が変更されます。  左矢印キー以外のキーが押下された場合、<xref:System.Windows.Controls.Control.Background%2A>の<xref:System.Windows.Controls.Button>の色は元に戻されます。

 [!code-csharp[InputOvw#Input_OvwKeyboardExampleHandlerCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml.cs#input_ovwkeyboardexamplehandlercodebehind)]
 [!code-vb[InputOvw#Input_OvwKeyboardExampleHandlerCodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InputOvw/VisualBasic/Page1.xaml.vb#input_ovwkeyboardexamplehandlercodebehind)]

### <a name="mouse-input-event-example"></a>マウス入力イベントの例
 次の例では、<xref:System.Windows.Controls.Control.Background%2A>の<xref:System.Windows.Controls.Button>の色は、 <xref:System.Windows.Controls.Button>。  <xref:System.Windows.Controls.Control.Background%2A>マウス ポインターが<xref:System.Windows.Controls.Button>に入ったときに変更され、マウス ポインターが離れるときに元に戻されます。

 例の最初のセクションでは、<xref:System.Windows.Controls.StackPanel>と<xref:System.Windows.Controls.Button>を作成し、 <xref:System.Windows.UIElement.MouseEnter>と<xref:System.Windows.UIElement.MouseLeave>イベントのイベント ハンドラーを<xref:System.Windows.Controls.Button>にアタッチします。

 [!code-xaml[InputOvw#Input_OvwMouseExampleXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml#input_ovwmouseexamplexaml)]

 [!code-csharp[InputOvw#Input_OvwMouseExampleUICodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml.cs#input_ovwmouseexampleuicodebehind)]
 [!code-vb[InputOvw#Input_OvwMouseExampleUICodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InputOvw/VisualBasic/Page1.xaml.vb#input_ovwmouseexampleuicodebehind)]

 この例の 2 番目のセクションは、コード内に記述され、イベント ハンドラーを定義しています。  マウスが <xref:System.Windows.Controls.Button>に入ると、<xref:System.Windows.Controls.Button> の <xref:System.Windows.Controls.Control.Background%2A> 色が <xref:System.Windows.Media.Brushes.SlateGray%2A>に変わります。  マウスが <xref:System.Windows.Controls.Button>から離れると、<xref:System.Windows.Controls.Button> の <xref:System.Windows.Controls.Control.Background%2A> 色が <xref:System.Windows.Media.Brushes.AliceBlue%2A>に戻されます。

 [!code-csharp[InputOvw#Input_OvwMouseExampleEneterHandler](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml.cs#input_ovwmouseexampleeneterhandler)]
 [!code-vb[InputOvw#Input_OvwMouseExampleEneterHandler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InputOvw/VisualBasic/Page1.xaml.vb#input_ovwmouseexampleeneterhandler)]

 [!code-csharp[InputOvw#Input_OvwMouseExampleLeaveHandler](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml.cs#input_ovwmouseexampleleavehandler)]
 [!code-vb[InputOvw#Input_OvwMouseExampleLeaveHandler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InputOvw/VisualBasic/Page1.xaml.vb#input_ovwmouseexampleleavehandler)]

<a name="text_input"></a>
## <a name="text-input"></a>テキスト入力
 <xref:System.Windows.ContentElement.TextInput> イベントを使用すると、デバイスに依存しない方法でテキスト入力をリッスンできます。 テキスト入力の主な手段はキーボードですが、音声認識、手書き入力、その他の入力デバイスでもテキストを入力できます。

 キーボード入力の場合、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]は、
まず適切な<xref:System.Windows.ContentElement.KeyDown>/ <xref:System.Windows.ContentElement.KeyUp>イベントを発生させます。 これらのイベントが処理されず、かつキーが方向キーやファンクション キーなどではなくテキストである場合、
<xref:System.Windows.ContentElement.TextInput>イベントが発生します。  <xref:System.Windows.ContentElement.KeyDown>/ <xref:System.Windows.ContentElement.KeyUp>と<xref:System.Windows.ContentElement.TextInput>イベントの間には、常に単純な1対1のマッピングがあるわけではありません。
複数のキーストロークが1つの文字を入力したり、単一のキーストロークで複数の文字から成る文字列を入力したりできるからです。  これは、中国語、日本語、韓国語などの言語で、入力方式エディター (Ime) を使用して、対応するアルファベットで何千もの文字を生成する場合に特に当てはまります。

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] が <xref:System.Windows.ContentElement.KeyUp>/<xref:System.Windows.ContentElement.KeyDown> イベントを送信する場合、キーストロークが <xref:System.Windows.Input.KeyEventArgs.Key%2A> イベントの一部になる (たとえば、ALT + S キーが押されている) 場合、<xref:System.Windows.Input.Key.System?displayProperty=nameWithType> が <xref:System.Windows.ContentElement.TextInput> に設定されます。 これにより、<xref:System.Windows.ContentElement.KeyDown> イベントハンドラーのコードが <xref:System.Windows.Input.Key.System?displayProperty=nameWithType> を確認し、見つかった場合は、その後に発生した <xref:System.Windows.ContentElement.TextInput> イベントのハンドラーの処理を終了します。 このような場合は、<xref:System.Windows.Input.TextCompositionEventArgs> 引数のさまざまなプロパティを使用して、元のキー入力を決定できます。 同様に、IME がアクティブな場合、<xref:System.Windows.Input.Key> の値は <xref:System.Windows.Input.Key.ImeProcessed?displayProperty=nameWithType>であり、<xref:System.Windows.Input.KeyEventArgs.ImeProcessedKey%2A> によって元のキーストロークまたはキーストロークが付与されます。

 次の例では、<xref:System.Windows.Controls.Primitives.ButtonBase.Click>イベントと<xref:System.Windows.UIElement.KeyDown>イベントのイベント ハンドラーを定義しています。

 コードまたはマークアップの最初のセグメントでは、ユーザー インターフェイスを作成します。

 [!code-xaml[InputOvw#Input_OvwTextInputXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml#input_ovwtextinputxaml)]

 [!code-csharp[InputOvw#Input_OvwTextInputUICodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml.cs#input_ovwtextinputuicodebehind)]
 [!code-vb[InputOvw#Input_OvwTextInputUICodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InputOvw/VisualBasic/Page1.xaml.vb#input_ovwtextinputuicodebehind)]

 次のセクションには、イベント ハンドラーが含まれています。

 [!code-csharp[InputOvw#Input_OvwTextInputHandlersCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml.cs#input_ovwtextinputhandlerscodebehind)]
 [!code-vb[InputOvw#Input_OvwTextInputHandlersCodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InputOvw/VisualBasic/Page1.xaml.vb#input_ovwtextinputhandlerscodebehind)]

 入力イベントはイベントのルートをバブルアップするため、 <xref:System.Windows.Controls.StackPanel>は要素がキーボード フォーカスを持っているかどうかに関係なく入力を受け取ります。 <xref:System.Windows.Controls.TextBox>コントロールが最初に通知を受け取り、 `OnTextInputKeyDown`が入力を処理しなかった場合にのみ<xref:System.Windows.Controls.TextBox>ハンドラーが呼び出されます。 <xref:System.Windows.UIElement.PreviewKeyDown>イベントが<xref:System.Windows.UIElement.KeyDown>イベントの代わりに使用された場合、 `OnTextInputKeyDown`ハンドラーが最初に呼び出されます。

 この例では、Ctrl + O キー操作とボタンのクリック イベントの、2 回の処理ロジックが記述されています。 これは、入力イベントを直接処理するのではなく、コマンドを使用すると、簡略化できる場合があります。  コマンドについては、この概要と「[コマンド実行の概要](commanding-overview.md)」で説明されています。

<a name="touch_and_manipulation"></a>
## <a name="touch-and-manipulation"></a>タッチおよび操作
 Windows 7 オペレーティング システムの新しいハードウェアと API では、アプリケーションが、複数のタッチからの入力を同時に受け取ることができる機能を提供しています。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] を使用すると、タッチが行われたときにイベントを発生させることで、マウスやキーボードなどの他の入力に応答する場合と同様に、アプリケーションでタッチを検出して応答できます。

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、タッチが行われると、タッチ イベントと操作イベントという 2 種類のイベントを公開します。 タッチ イベントは、タッチスクリーン上の各指とその動きについて生データを提供します。 操作イベントは、入力を特定のアクションとして解釈します。 このセクションでは、この 2 種類のイベントについて説明します。

### <a name="prerequisites"></a>前提条件
 タッチに応答するアプリケーションを開発するには、次のコンポーネントが必要です。

- Visual Studio 2010。

- Windows 7。

- Windows タッチをサポートするデバイス (タッチスクリーンなど)。

### <a name="terminology"></a>用語
 タッチについて説明するときに使用される用語を次に示します。

- **タッチ**は、Windows 7 で認識されるユーザー入力の種類です。 通常、タッチを検知するスクリーンに指を当てると、タッチが開始されます。 デバイスによって指の位置と動きがマウス入力として変換されるだけの場合、ノート PC で一般的なタッチパッドなどのデバイスでは、タッチがサポートされないことに注意してください。

- **マルチタッチ**は、複数のポイントが同時に行われるときのタッチです。 Windows 7 および [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、マルチタッチをサポートします。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] に関するドキュメントでタッチについて説明されている場合、その概念はマルチタッチを対象とします。

- **操作**は、タッチが、オブジェクトに適用される物理的なアクションとして解釈されるときに発生します。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、操作イベントによって、平行移動、拡大縮小、または回転の各操作として入力が解釈されます。

- `touch device` は、タッチスクリーン上での 1 本の指など、タッチ入力を生成するデバイスを表します。

### <a name="controls-that-respond-to-touch"></a>タッチに応答するコントロール
 次のコントロールは、スクロールされて見えないコンテンツがある場合に、コントロール上を指でドラッグするとスクロールできます。

- <xref:System.Windows.Controls.ComboBox>

- <xref:System.Windows.Controls.ContextMenu>

- <xref:System.Windows.Controls.DataGrid>

- <xref:System.Windows.Controls.ListBox>

- <xref:System.Windows.Controls.ListView>

- <xref:System.Windows.Controls.MenuItem>

- <xref:System.Windows.Controls.TextBox>

- <xref:System.Windows.Controls.ToolBar>

- <xref:System.Windows.Controls.TreeView>

 <xref:System.Windows.Controls.ScrollViewer> は <xref:System.Windows.Controls.ScrollViewer.PanningMode%2A?displayProperty=nameWithType> 添付プロパティを定義します。このプロパティを使用すると、タッチパンを水平方向、垂直方向、または両方のどちらで有効にするかを指定できます。 <xref:System.Windows.Controls.ScrollViewer.PanningDeceleration%2A?displayProperty=nameWithType> プロパティは、ユーザーがタッチスクリーンから指を離したときに、スクロール速度を遅くする速度を指定します。 <xref:System.Windows.Controls.ScrollViewer.PanningRatio%2A?displayProperty=nameWithType> 添付プロパティは、操作オフセットを平行移動するためのスクロールオフセットの比率を指定します。

### <a name="touch-events"></a>タッチ イベント
 基底クラス、<xref:System.Windows.UIElement>、<xref:System.Windows.UIElement3D>、および <xref:System.Windows.ContentElement>は、アプリケーションがタッチに応答するようにサブスクライブできるイベントを定義します。 タッチ イベントは、アプリケーションでタッチがオブジェクトの操作以外の動作として解釈される場合に役立ちます。 たとえば、ユーザーが 1 本以上の指を使って描画できるアプリケーションは、タッチ イベントをサブスクライブします。

 この 3 つのクラスはすべて、定義クラスに関係なく、動作がよく似た次のイベントを定義します。

- <xref:System.Windows.UIElement.TouchDown>

- <xref:System.Windows.UIElement.TouchMove>

- <xref:System.Windows.UIElement.TouchUp>

- <xref:System.Windows.UIElement.TouchEnter>

- <xref:System.Windows.UIElement.TouchLeave>

- <xref:System.Windows.UIElement.PreviewTouchDown>

- <xref:System.Windows.UIElement.PreviewTouchMove>

- <xref:System.Windows.UIElement.PreviewTouchUp>

- <xref:System.Windows.UIElement.GotTouchCapture>

- <xref:System.Windows.UIElement.LostTouchCapture>

 キーボード イベントやマウス イベントと同様に、タッチ イベントはルーティング イベントです。 `Preview` で始まるイベントはトンネル イベントで、`Touch` で始まるイベントはバブル イベントです。 ルーティング イベントの詳細については、「[ルーティング イベントの概要](routed-events-overview.md)」を参照してください。 これらのイベントを処理するときに、<xref:System.Windows.Input.TouchEventArgs.GetTouchPoint%2A> または <xref:System.Windows.Input.TouchEventArgs.GetIntermediateTouchPoints%2A> メソッドを呼び出すことによって、任意の要素を基準とした入力の位置を取得できます。

 タッチ イベント間の対話について理解するには、ユーザーがある要素の上に指を 1 本置き、その要素内で指を動かした後、要素から指を離すシナリオを考えてみます。 次の図は、バブル イベントの実行について示しています (わかりやすくするため、トンネル イベントは省略しています)。

 ![タッチイベントのシーケンス。](./media/ndp-touchevents.png "NDP_TouchEvents")タッチイベント

 次のリストに、前の図のイベントのシーケンスを示します。

1. <xref:System.Windows.UIElement.TouchEnter> イベントは、ユーザーが要素に指を置いたときに1回発生します。

2. <xref:System.Windows.UIElement.TouchDown> イベントが1回発生します。

3. <xref:System.Windows.UIElement.TouchMove> イベントは、ユーザーが要素内で指を動かしたときに複数回発生します。

4. <xref:System.Windows.UIElement.TouchUp> イベントは、ユーザーが要素から指を離したときに1回発生します。

5. <xref:System.Windows.UIElement.TouchLeave> イベントが1回発生します。

 3 本以上の指を使用した場合は、それぞれの指に対してイベントが発生します。

### <a name="manipulation-events"></a>操作イベント
 アプリケーションでユーザーがオブジェクトを操作できるようにする場合、<xref:System.Windows.UIElement> クラスは操作イベントを定義します。 単にタッチの位置を報告するタッチ イベントとは異なり、操作イベントは、入力がどのように解釈されるかを報告します。 操作には、平行移動、拡大縮小、および回転の 3 種類があります。 次のリストでは、3 種類の操作を呼び出す方法を示します。

- 平行移動の操作を呼び出すには、オブジェクトの上に指を置き、タッチスクリーン上で指を動かします。 通常、この操作を行うと、オブジェクトが移動します。

- 拡大縮小の操作を呼び出すには、オブジェクトの上に 2 本の指を置き、2 本の指を近づけたり離したりします。 通常、この操作を行うと、オブジェクトのサイズが変更されます。

- 回転の操作を呼び出すには、オブジェクトの上に 2 本の指を置き、一方の指を中心にもう一方の指を回転させます。 通常、この操作を行うと、オブジェクトが回転します。

 2 種類以上の操作を同時に発生させることもできます。

 オブジェクトを操作に応答させると、オブジェクトに慣性があるように見せることができます。 これにより、オブジェクトに現実の世界をシミュレートさせることができます。 たとえば、テーブル上で書籍を押す場合、強く押すと、書籍は離した後も移動し続けます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] を使用すると、ユーザーがオブジェクトから指を離した後に、操作イベントを発生させることで、この動作をシミュレートできます。

 ユーザーがオブジェクトの移動、サイズ変更、および回転を実行できるアプリケーションを作成する方法は、「[チュートリアル: 初めてのタッチ アプリケーションの作成](walkthrough-creating-your-first-touch-application.md)」を参照してください。

 <xref:System.Windows.UIElement> は、次の操作イベントを定義します。

- <xref:System.Windows.UIElement.ManipulationStarting>

- <xref:System.Windows.UIElement.ManipulationStarted>

- <xref:System.Windows.UIElement.ManipulationDelta>

- <xref:System.Windows.UIElement.ManipulationInertiaStarting>

- <xref:System.Windows.UIElement.ManipulationCompleted>

- <xref:System.Windows.UIElement.ManipulationBoundaryFeedback>

 既定では、<xref:System.Windows.UIElement> は、これらの操作イベントを受け取りません。 <xref:System.Windows.UIElement>で操作イベントを受信するには、<xref:System.Windows.UIElement.IsManipulationEnabled%2A?displayProperty=nameWithType> を `true`に設定します。

#### <a name="the-execution-path-of-manipulation-events"></a>操作イベントの実行パス
 ユーザーがオブジェクトを "スローする" シナリオを検討します。 ユーザーは、オブジェクトの上に指を置き、タッチスクリーン上で指を短い距離移動させ、オブジェクトの移動中に指を離します。 この結果、オブジェクトはユーザーの指の下で移動し、ユーザーが指を離した後も移動を続けます。

 次の図は、操作イベントの実行パスと各イベントに関する重要な情報を示しています。

 ![操作イベントのシーケンス。](./media/ndp-manipulationevents.png "NDP_ManipulationEvents")操作イベント

 次のリストに、前の図のイベントのシーケンスを示します。

1. <xref:System.Windows.UIElement.ManipulationStarting> イベントは、ユーザーがオブジェクトに指を置いたときに発生します。 特に、このイベントでは、<xref:System.Windows.Input.ManipulationStartingEventArgs.ManipulationContainer%2A> プロパティを設定できます。 後続のイベントでは、操作の位置は <xref:System.Windows.Input.ManipulationStartingEventArgs.ManipulationContainer%2A>に対して相対的になります。 <xref:System.Windows.UIElement.ManipulationStarting>以外のイベントでは、このプロパティは読み取り専用であるため、このプロパティを設定できるのは <xref:System.Windows.UIElement.ManipulationStarting> イベントだけです。

2. <xref:System.Windows.UIElement.ManipulationStarted> イベントが次に発生します。 このイベントは、操作の起点を報告します。

3. <xref:System.Windows.UIElement.ManipulationDelta> イベントは、タッチスクリーン上のユーザーの指の動きに応じて複数回発生します。 <xref:System.Windows.Input.ManipulationDeltaEventArgs> クラスの <xref:System.Windows.Input.ManipulationDeltaEventArgs.DeltaManipulation%2A> プロパティは、操作が移動、展開、または変換として解釈されるかどうかを報告します。 ここで、オブジェクトを操作する作業のほとんどを実行します。

4. <xref:System.Windows.UIElement.ManipulationInertiaStarting> イベントは、ユーザーの指がオブジェクトとの接続を失ったときに発生します。 このイベントでは、慣性による処理中の操作の減速を指定できます。 これは、選択した場合、オブジェクトが異なる物理領域や属性をエミュレートできるようにするためです。 たとえば、アプリケーションが現実の世界のアイテムを表す 2 つのオブジェクトを保持していて、一方のオブジェクトがもう一方のオブジェクトよりも重いとします。 重いオブジェクトを軽いオブジェクトよりもすばやく減速させるようにすることができます。

5. <xref:System.Windows.UIElement.ManipulationDelta> イベントは、慣性が発生すると複数回発生します。 このイベントは、ユーザーの指がタッチスクリーン上で移動したとき、および [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] が慣性による処理をシミュレートしたときに発生することに注意してください。 つまり、<xref:System.Windows.UIElement.ManipulationDelta> は、<xref:System.Windows.UIElement.ManipulationInertiaStarting> イベントの前後に発生します。 <xref:System.Windows.Input.ManipulationDeltaEventArgs.IsInertial%2A?displayProperty=nameWithType> プロパティは、<xref:System.Windows.UIElement.ManipulationDelta> イベントが慣性中に発生するかどうかを報告します。そのため、プロパティを確認し、その値に応じて異なるアクションを実行できます。

6. <xref:System.Windows.UIElement.ManipulationCompleted> イベントは、操作と慣性が終了したときに発生します。 つまり、すべての <xref:System.Windows.UIElement.ManipulationDelta> イベントが発生すると、操作が完了したことを通知するために <xref:System.Windows.UIElement.ManipulationCompleted> イベントが発生します。

 <xref:System.Windows.UIElement> は、<xref:System.Windows.UIElement.ManipulationBoundaryFeedback> イベントも定義します。 このイベントは、<xref:System.Windows.UIElement.ManipulationDelta> イベントで <xref:System.Windows.Input.ManipulationDeltaEventArgs.ReportBoundaryFeedback%2A> メソッドが呼び出されたときに発生します。 <xref:System.Windows.UIElement.ManipulationBoundaryFeedback> イベントは、オブジェクトが境界に達したときに、アプリケーションまたはコンポーネントが視覚的なフィードバックを提供できるようにします。 たとえば、<xref:System.Windows.Window> クラスは、<xref:System.Windows.UIElement.ManipulationBoundaryFeedback> イベントを処理して、ウィンドウが枠を置いたときに少し移動するようにします。

 操作を取り消すには、<xref:System.Windows.UIElement.ManipulationBoundaryFeedback> イベント以外の操作イベントのイベント引数に対して <xref:System.Windows.Input.ManipulationStartingEventArgs.Cancel%2A> メソッドを呼び出します。 <xref:System.Windows.Input.ManipulationStartingEventArgs.Cancel%2A>を呼び出すと、操作イベントは発生しなくなり、タッチのためにマウスイベントが発生します。 次の表は、操作が取り消される時間と発生するマウス イベントとの間の関係を示しています。

|Cancel が呼び出されるイベント|既に行われている入力に対して発生するマウス イベント|
|----------------------------------------|-----------------------------------------------------------------|
|<xref:System.Windows.UIElement.ManipulationStarting> および <xref:System.Windows.UIElement.ManipulationStarted>|マウス ボタンを押すイベント。|
|<xref:System.Windows.UIElement.ManipulationDelta>|マウス ボタンを押すイベントおよびマウス移動イベント。|
|<xref:System.Windows.UIElement.ManipulationInertiaStarting> および <xref:System.Windows.UIElement.ManipulationCompleted>|マウス ボタンを押すイベント、マウス移動イベント、およびマウス ボタンを放すイベント。|

 操作が慣性の場合に <xref:System.Windows.Input.ManipulationStartingEventArgs.Cancel%2A> を呼び出すと、メソッドは `false` を返し、入力はマウスイベントを発生させないことに注意してください。

### <a name="the-relationship-between-touch-and-manipulation-events"></a>タッチ イベントと操作イベントの関係
 <xref:System.Windows.UIElement> は常にタッチイベントを受け取ることができます。 <xref:System.Windows.UIElement.IsManipulationEnabled%2A> プロパティが `true`に設定されている場合、<xref:System.Windows.UIElement> はタッチイベントと操作イベントの両方を受け取ることができます。  <xref:System.Windows.UIElement.TouchDown> イベントが処理されない場合 (つまり、<xref:System.Windows.RoutedEventArgs.Handled%2A> プロパティが `false`)、操作ロジックは要素にタッチをキャプチャし、操作イベントを生成します。 <xref:System.Windows.RoutedEventArgs.Handled%2A> プロパティが <xref:System.Windows.UIElement.TouchDown> イベントの `true` に設定されている場合、操作ロジックは操作イベントを生成しません。 次の図は、タッチ イベントと操作イベントの関係を示しています。

 タッチイベントと操作イベント(./media/ndp-touchmanipulateevents.png "NDP_TouchManipulateEvents")タッチイベントと操作イベント![の関係]

 次のリストに、前の図に示したタッチ イベントと操作イベントの関係を示します。

- 最初のタッチデバイスが <xref:System.Windows.UIElement>で <xref:System.Windows.UIElement.TouchDown> イベントを生成する場合、操作ロジックは <xref:System.Windows.UIElement.CaptureTouch%2A> メソッドを呼び出します。これにより、<xref:System.Windows.UIElement.GotTouchCapture> イベントが生成されます。

- <xref:System.Windows.UIElement.GotTouchCapture> が発生すると、操作ロジックによって <xref:System.Windows.Input.Manipulation.AddManipulator%2A?displayProperty=nameWithType> メソッドが呼び出され、<xref:System.Windows.UIElement.ManipulationStarting> イベントが生成されます。

- <xref:System.Windows.UIElement.TouchMove> イベントが発生すると、操作ロジックによって、<xref:System.Windows.UIElement.ManipulationInertiaStarting> イベントの前に発生する <xref:System.Windows.UIElement.ManipulationDelta> イベントが生成されます。

- 要素の最後のタッチデバイスによって <xref:System.Windows.UIElement.TouchUp> イベントが発生すると、操作ロジックによって <xref:System.Windows.UIElement.ManipulationInertiaStarting> イベントが生成されます。

<a name="focus"></a>
## <a name="focus"></a>フォーカス
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、フォーカスに関してキーボード フォーカスと論理フォーカスという 2 つの主要な概念があります。

### <a name="keyboard-focus"></a>キーボード フォーカス
 キーボード フォーカスは、キーボード入力を受け取っている要素を参照しています。  キーボード フォーカスを持つ要素は、デスクトップ全体で 1 つしかありません。  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]では、 キーボード フォーカスを持つ要素の<xref:System.Windows.IInputElement.IsKeyboardFocused%2A>は`true`に設定されます。  <xref:System.Windows.Input.Keyboard>クラスの静的メソッド<xref:System.Windows.Input.Keyboard.FocusedElement%2A>は、 現在キーボード フォーカスがある要素を返します。

 tab キーで要素に移動したり、<xref:System.Windows.Controls.TextBox>など特定の要素上でマウスをクリックしたりすることで、 キーボード フォーカスを取得できます。  また、<xref:System.Windows.Input.Keyboard.Focus%2A>クラスの<xref:System.Windows.Input.Keyboard>メソッドを使用して、 プログラムでキーボード フォーカスを取得することもできます。  <xref:System.Windows.Input.Keyboard.Focus%2A>メソッドは、指定された要素にキーボード フォーカスを与えようとします。  <xref:System.Windows.Input.Keyboard.Focus%2A>の返す要素が、現在キーボード フォーカスを持つ要素です。

 要素にキーボード フォーカスを設定するために、<xref:System.Windows.UIElement.Focusable%2A>プロパティ および<xref:System.Windows.UIElement.IsVisible%2A>プロパティは**true**に設定しておく必要があります。  <xref:System.Windows.Controls.Panel>などのいくつかのクラスでは、 <xref:System.Windows.UIElement.Focusable%2A>がデフォルトで`false`に設定されているため、 その要素にフォーカスを設定したい場合は`true`に設定する必要があります。

 次の例では<xref:System.Windows.Input.Keyboard.Focus%2A>にキーボード フォーカスを設定するために<xref:System.Windows.Controls.Button>を使用します。  アプリケーションの初期フォーカスを設定する推奨場所は、<xref:System.Windows.FrameworkElement.Loaded>イベント ハンドラーです。

 [!code-csharp[focussample#FocusSampleSetFocus](~/samples/snippets/csharp/VS_Snippets_Wpf/FocusSample/CSharp/Window1.xaml.cs#focussamplesetfocus)]
 [!code-vb[focussample#FocusSampleSetFocus](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FocusSample/visualbasic/window1.xaml.vb#focussamplesetfocus)]

 キーボード フォーカスの詳細については、「[フォーカスの概要](focus-overview.md)」を参照してください。

### <a name="logical-focus"></a>論理フォーカス
 論理フォーカスは、フォーカス スコープ内の<xref:System.Windows.Input.FocusManager.FocusedElement%2A?displayProperty=nameWithType>を指します。  アプリケーション内の複数の要素が論理フォーカスを持つことがありますが、特定のフォーカス スコープで論理フォーカスを持つ要素は 1 つだけに限られます。

 フォーカス スコープはコンテナー要素で、そのスコープ内の<xref:System.Windows.Input.FocusManager.FocusedElement%2A>を追跡します。  フォーカスがフォーカス範囲を離れると、フォーカスがある要素はキーボード フォーカスを失いますが、論理フォーカスはそのまま保持されます。  フォーカスがフォーカス範囲に戻ると、フォーカスがある要素はキーボード フォーカスを取得します。  これにより、キーボード フォーカスを複数のフォーカス スコープ間で変更できますが、 フォーカスが戻ると、そのフォーカス スコープ内の論理フォーカスのある要素がキーボード フォーカスのある要素として保持されることが保証されます。

 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]で <xref:System.Windows.Input.FocusManager>の<xref:System.Windows.Input.FocusManager.IsFocusScope%2A>添付プロパティを`true`に設定するか、 コードを使用して<xref:System.Windows.Input.FocusManager.SetIsFocusScope%2A>メソッドを使用して添付プロパティを設定することにより、 要素をフォーカス スコープに変換できます。

 次の例では、<xref:System.Windows.Controls.StackPanel>の<xref:System.Windows.Input.FocusManager.IsFocusScope%2A>添付プロパティを設定してフォーカス スコープに変換しています。

 [!code-xaml[MarkupSnippets#MarkupIsFocusScopeXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/MarkupSnippets/CSharp/Window1.xaml#markupisfocusscopexaml)]

 [!code-csharp[FocusSnippets#FocusSetIsFocusScope](~/samples/snippets/csharp/VS_Snippets_Wpf/FocusSnippets/CSharp/Window1.xaml.cs#focussetisfocusscope)]
 [!code-vb[FocusSnippets#FocusSetIsFocusScope](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FocusSnippets/visualbasic/window1.xaml.vb#focussetisfocusscope)]

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]で、デフォルトでフォーカス スコープであるクラスは、<xref:System.Windows.Window>、 <xref:System.Windows.Controls.Menu>、 <xref:System.Windows.Controls.ToolBar>、<xref:System.Windows.Controls.ContextMenu>です。

 キーボードフォーカスを持つ要素は、それが属するフォーカス スコープに対して論理フォーカスがあります。そのため、<xref:System.Windows.Input.Keyboard.Focus%2A>クラスまたは基本要素クラスのフォーカスメソッドを使用して要素にフォーカスを設定すると、要素に
<xref:System.Windows.Input.Keyboard>と論理フォーカスが設定されます。

 フォーカス スコープでフォーカスがある要素を調べるには<xref:System.Windows.Input.FocusManager.GetFocusedElement%2A>します。 フォーカス スコープのフォーカスがある要素を変更する<xref:System.Windows.Input.FocusManager.SetFocusedElement%2A>します。

 論理フォーカスの詳細については、「[フォーカスの概要](focus-overview.md)」を参照してください。

<a name="mouse_position"></a>
## <a name="mouse-position"></a>マウスの位置
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 入力 API は、座標空間に関して有用な情報を提供します。  たとえば、座標 `(0,0)` は左上からの座標ですが、これはツリーのどの要素の左上でしょうか。 入力対象の要素でしょうか。 イベント ハンドラーを適用した要素でしょうか。 または、それ以外でしょうか。 混乱を避けるために、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 入力 API では、マウスで取得した座標を操作するときに参照のフレームを指定する必要があります。 <xref:System.Windows.Input.Mouse.GetPosition%2A>メソッドは、マウス ポインターの指定された要素に対する相対的な座標を返します。

<a name="mouse_capture"></a>
## <a name="mouse-capture"></a>マウス キャプチャ
 マウス デバイスは、マウス キャプチャと呼ばれるモーダル特性を特別に備えています。 マウス キャプチャは、ドラッグ アンド ドロップ操作が開始されたときの入力の遷移状態を保持するために使用されます。これにより、マウス ポインターの画面上の標準位置に関係する他の操作は、必ずしも発生しません。 ドラッグの間、ユーザーはドラッグ アンド ドロップを中止しない限り、クリックすることはできません。このため、マウス キャプチャはドラッグ元によって保持され、マウスを置いたときのキューの大部分は適切でなくなります。 入力システムは、マウスキャプチャの状態を決定できる Api と、特定の要素に対してマウスキャプチャを強制する Api、またはマウスキャプチャの状態をクリアする api を公開します。 ドラッグ アンド ドロップ操作の詳細については、「[ドラッグ アンド ドロップの概要](drag-and-drop-overview.md)」を参照してください。

<a name="commands"></a>
## <a name="commands"></a>コマンド
 コマンドでは、デバイス入力よりもセマンティックなレベルの入力処理が可能です。  コマンドは、`Cut`、`Copy`、`Paste`、`Open`などの簡単なディレクティブです。  コマンドは、コマンド ロジックを一元管理するために役立ちます。  <xref:System.Windows.Controls.Menu>や<xref:System.Windows.Controls.ToolBar>やキーボード ショートカットを使用して
同じコマンドにアクセスできるかもしれません。 またコマンドは、コマンドが使用できないときにコントロールを無効にするための機構も提供します。

 <xref:System.Windows.Input.RoutedCommand>は、
[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]における<xref:System.Windows.Input.ICommand>の実装です。  <xref:System.Windows.Input.RoutedCommand>が実行されると、<xref:System.Windows.Input.CommandManager.PreviewExecuted>イベントと
<xref:System.Windows.Input.CommandManager.Executed>イベントがコマンド ターゲットで発生し、他の入力と同様に要素ツリーをトンネルあるいはバブリングします。  コマンドの対象が設定されていない場合は、キーボード フォーカスを持つ要素がコマンドの対象になります。  コマンドを実行するロジックは<xref:System.Windows.Input.CommandBinding>にアタッチされています。  <xref:System.Windows.Input.CommandManager.Executed>イベントがそのコマンドの<xref:System.Windows.Input.CommandBinding>に到達すると、
<xref:System.Windows.Input.ExecutedRoutedEventHandler>上の<xref:System.Windows.Input.CommandBinding>が呼び出されます。  このハンドラーが、コマンドのアクションを実行します。

 コマンドの詳細については、[コマンド実行の概要](commanding-overview.md)を参照してください。

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]は、<xref:System.Windows.Input.ApplicationCommands>、 
<xref:System.Windows.Input.MediaCommands>、<xref:System.Windows.Input.ComponentCommands>、<xref:System.Windows.Input.NavigationCommands>、
<xref:System.Windows.Documents.EditingCommands>で一般的なコマンドのライブラリを提供しています。また、コマンドを独自に定義することもできます。

 次の例では、<xref:System.Windows.Controls.MenuItem>がクリックされたとき、
<xref:System.Windows.Input.ApplicationCommands.Paste%2A>上で<xref:System.Windows.Controls.TextBox>コマンドを実行するよう設定する方法を示します。
<xref:System.Windows.Controls.TextBox>にキーボード フォーカスがあると想定しています。

 [!code-xaml[CommandingOverviewSnippets#CommandingOverviewSimpleCommand](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml#commandingoverviewsimplecommand)]

 [!code-csharp[CommandingOverviewSnippets#CommandingOverviewCommandTargetCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml.cs#commandingoverviewcommandtargetcodebehind)]
 [!code-vb[CommandingOverviewSnippets#CommandingOverviewCommandTargetCodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandingOverviewSnippets/visualbasic/window1.xaml.vb#commandingoverviewcommandtargetcodebehind)]

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]におけるのコマンドの詳細については、 「[コマンド実行の概要](commanding-overview.md)」を参照してください。

<a name="the_input_system_and_base_elements"></a>
## <a name="the-input-system-and-base-elements"></a>入力システムと基本要素
 <xref:System.Windows.Input.Mouse>、<xref:System.Windows.Input.Keyboard>、<xref:System.Windows.Input.Stylus>クラスの添付イベントのような 入力イベントは、入力システムが発生させ、ヒット テストの実行時に、ビジュアル ツリーに基づくオブジェクト モデル内の特定位置に挿入します。

 <xref:System.Windows.Input.Mouse>、<xref:System.Windows.Input.Keyboard>、<xref:System.Windows.Input.Stylus>で 添付イベントとして定義されている各イベントは、<xref:System.Windows.UIElement>と<xref:System.Windows.ContentElement>という基本要素クラスで、 新しいルーティング イベントとして再び公開されています。 基本要素のルーティング イベントは、元の添付イベントを処理し、イベント データを再利用するクラスによって生成されます。

 入力イベントが、その基本要素の入力イベントの実装によって、特定のソース要素に関連付けられると、論理ツリー オブジェクトとビジュアル ツリー オブジェクトの組み合わせに基づく残りのイベント ルートを通じてルーティングされ、アプリケーション コードで処理することができます。  一般に、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] とコードの両方でより直感的なイベントハンドラー構文を使用できるため、<xref:System.Windows.UIElement> と <xref:System.Windows.ContentElement>でルーティングイベントを使用して、これらのデバイス関連の入力イベントを処理する方が便利です。 プロセスを開始した添付イベントを処理することもできますが、いくつかの問題があります。添付イベントには、基本要素クラス処理によって処理されることがマークされる可能性があり、添付イベントにハンドラーを添付するために、実際のイベント構文ではなく、アクセサー メソッドを使用する必要があります。

<a name="whats_next"></a>
## <a name="whats-next"></a>次の課題
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] で入力を処理するには、さまざまな方法があります。  さまざまな種類の入力イベントや[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]のルーティング イベントについて、 さらに理解を深める必要があります。

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のフレームワーク要素とイベントのルーティングの詳細については、他のリソースを参照することができます。 詳細については、「[コマンド実行の概要](commanding-overview.md)」、「[フォーカスの概要](focus-overview.md)」、「[基本要素の概要](base-elements-overview.md)」、「[WPF のツリー](trees-in-wpf.md)」、および「[ルーティング イベントの概要](routed-events-overview.md)」を参照してください。

## <a name="see-also"></a>参照

- [フォーカスの概要](focus-overview.md)
- [コマンド実行の概要](commanding-overview.md)
- [ルーティング イベントの概要](routed-events-overview.md)
- [基本要素の概要](base-elements-overview.md)
- [プロパティ](properties-wpf.md)
