---
title: コマンド実行の概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- interfaces [WPF], ICommandSource
- command library [WPF]
- commands [WPF], definition of
- CommandBindings [WPF]
- ICommandSource interfaces [WPF]
- commanding [WPF]
- CommandManager [WPF]
ms.assetid: bc208dfe-367d-426a-99de-52b7e7511e81
ms.openlocfilehash: 192fe629493947ffe4e0aa8ade417b7701ff95b4
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72004598"
---
# <a name="commanding-overview"></a>コマンド実行の概要
<a name="introduction"></a> コマンド実行は [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] の入力メカニズムであり、デバイス入力よりいっそうセマンティックなレベルでの入力処理を提供します。 コマンドの例は、多くのアプリケーションで見られる**コピー**、**切り取り**、**貼り付け**などの操作です。  
  
 この概要では、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] でのコマンドとはどういうものか、コマンド実行モデルに含まれるクラス、アプリケーションでコマンドを作成して使用する方法を定義します。  
  
 このトピックには、次のセクションが含まれています。  
  
- [コマンドとは](#commands_at_10000_feet)  
  
- [WPF での簡単なコマンドの例](#simple_command)  
  
- [WPF のコマンド実行における 4 つの主要な概念](#Four_main_Concepts)  
  
- [コマンド ライブラリ](#Command_Library)  
  
- [カスタム コマンドの作成](#creating_commands)  
  
<a name="commands_at_10000_feet"></a>   
## <a name="what-are-commands"></a>コマンドとは  
 コマンドには、いくつかの目的があります。 第 1 の目的は、コマンドを呼び出すセマンティクスとオブジェクトを、コマンドを実行するロジックから分離することです。 これにより、複数の異なるソースから同じコマンド ロジックを呼び出すことができ、異なるターゲットに合わせてコマンド ロジックをカスタマイズできます。 たとえば、多くのアプリケーションで使われる編集操作である**コピー**、**切り取り**、**貼り付け**は、コマンドを使って実装されていれば、異なるユーザー アクションを使って呼び出すことができます。 ユーザーは、ボタンをクリックして、メニューの項目を選択して、または Ctrl + X などのキーの組み合わせを使って、選んだオブジェクトやテキストを切り取ることができます。 コマンドを使うことにより、各種のユーザー アクションを同じロジックにバインドできます。  
  
 コマンドのもう 1 つの目的は、アクションを実行できるかどうかを示すことです。 再びオブジェクトやテキストの切り取りを例にすると、アクションは何かを選択したときにのみ意味があります。 何も選ばないでオブジェクトまたはテキストを切り取ろうとすると、何も起こりません。 ユーザーにこのことを示すため、多くのアプリケーションでは、ボタンやメニュー項目を無効にして、アクションを実行できるかどうかをユーザーが認識できるようにしています。 コマンドでは、<xref:System.Windows.Input.ICommand.CanExecute%2A> メソッドを実装することで、アクションが実行可能かどうかを示すことができます。 ボタンは、<xref:System.Windows.Input.ICommand.CanExecuteChanged> イベントをサブスクライブでき、<xref:System.Windows.Input.ICommand.CanExecute%2A> で `false` が返された場合は無効に、<xref:System.Windows.Input.ICommand.CanExecute%2A> で `true` が返された場合は有効にできます。  
  
 コマンドのセマンティクスはアプリケーションおよびクラス全体で統一できますが、アクションのロジックは対象となる特定のオブジェクトに固有です。 キーの組み合わせ Ctrl + X キーはテキスト クラス、イメージ クラス、Web ブラウザーの**切り取り**コマンドを呼び出しますが、**切り取り**操作を実行する実際のロジックは、切り取りを実行するアプリケーションによって定義されています。 <xref:System.Windows.Input.RoutedCommand> では、クライアントでロジックを実装できます。 テキスト オブジェクトは選択されたテキストを切り取ってクリップボードに移動できるのに対し、イメージ オブジェクトは選択されたイメージを切り取ることができます。 アプリケーションは、<xref:System.Windows.Input.CommandManager.Executed> イベントを処理するときに、コマンドのターゲットにアクセスし、ターゲットの種類に応じた適切なアクションを実行できます。  
  
<a name="simple_command"></a>   
## <a name="simple-command-example-in-wpf"></a>WPF での簡単なコマンドの例  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] でコマンドを使う最も簡単な方法は、いずれかのコマンド ライブラリ クラスの定義済みの <xref:System.Windows.Input.RoutedCommand> を使用するか、コマンドの処理をネイティブにサポートするコントロールを使用するか、コマンドの呼び出しをネイティブにサポートするコントロールを使用することです。  <xref:System.Windows.Input.ApplicationCommands.Paste%2A> コマンドは、<xref:System.Windows.Input.ApplicationCommands> クラスで定義済みのコマンドのいずれかです。  <xref:System.Windows.Controls.TextBox> コントロールには、<xref:System.Windows.Input.ApplicationCommands.Paste%2A> コマンドを処理するためのロジックが組み込まれています。  <xref:System.Windows.Controls.MenuItem> クラスはコマンドの呼び出しをネイティブにサポートします。  
  
 次に示すのは、<xref:System.Windows.Controls.MenuItem> にキーボード フォーカスがある場合に、クリックされたときに <xref:System.Windows.Input.ApplicationCommands.Paste%2A> で <xref:System.Windows.Controls.TextBox> コマンドを呼び出すように <xref:System.Windows.Controls.TextBox> を設定する方法の例です。  
  
 [!code-xaml[CommandingOverviewSnippets#CommandingOverviewSimpleCommand](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml#commandingoverviewsimplecommand)]  
  
 [!code-csharp[CommandingOverviewSnippets#CommandingOverviewCommandTargetCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml.cs#commandingoverviewcommandtargetcodebehind)]
 [!code-vb[CommandingOverviewSnippets#CommandingOverviewCommandTargetCodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandingOverviewSnippets/visualbasic/window1.xaml.vb#commandingoverviewcommandtargetcodebehind)]  
  
<a name="Four_main_Concepts"></a>   
## <a name="four-main-concepts-in-wpf-commanding"></a>WPF のコマンド実行における 4 つの主要な概念  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のルーティング コマンド モデルは、コマンド、コマンド ソース、コマンド ターゲット、コマンド バインディングという 4 つの主要な概念に分けることができます。  
  
- "*コマンド*" は、実行するアクションです。  
  
- "*コマンド ソース*" は、コマンドを呼び出すオブジェクトです。  
  
- "*コマンド ターゲット*" は、コマンドが実行されているオブジェクトです。  
  
- "*コマンド バインディング*" は、コマンド ロジックをコマンドにマップするオブジェクトです。  
  
 前の例では、<xref:System.Windows.Input.ApplicationCommands.Paste%2A> コマンドがコマンド、<xref:System.Windows.Controls.MenuItem> がコマンド ソース、<xref:System.Windows.Controls.TextBox> がコマンド ターゲットで、コマンド バインドは <xref:System.Windows.Controls.TextBox> コントロールによって提供されます。  注意しなければならないのは、<xref:System.Windows.Input.CommandBinding> はコマンド ターゲット クラスであるコントロールによって常に提供されるわけではないということです。  多くの場合は、アプリケーション開発者が <xref:System.Windows.Input.CommandBinding> を作成する必要があるか、または <xref:System.Windows.Input.CommandBinding> はコマンド ターゲットの先祖に関連付けられています。  
  
<a name="Commands"></a>   
### <a name="commands"></a>コマンド  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のコマンドは、<xref:System.Windows.Input.ICommand> インターフェイスを実装することで作成されます。  <xref:System.Windows.Input.ICommand> では、2 つのメソッド (<xref:System.Windows.Input.ICommand.Execute%2A> と <xref:System.Windows.Input.ICommand.CanExecute%2A>)、およびイベント (<xref:System.Windows.Input.ICommand.CanExecuteChanged>) を公開します。 <xref:System.Windows.Input.ICommand.Execute%2A> では、コマンドに関連付けられているアクションを実行します。 <xref:System.Windows.Input.ICommand.CanExecute%2A> では、現在のコマンド ターゲットでコマンドを実行できるかどうかを判断します。 コマンド実行操作を集中管理するコマンド マネージャーが、既に発生しているがコマンド バインドによってまだ実行されていないコマンドを無効にする可能性のある変更をコマンド ソースで検出した場合、<xref:System.Windows.Input.ICommand.CanExecuteChanged> が発生します。  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] での <xref:System.Windows.Input.ICommand> の実装は <xref:System.Windows.Input.RoutedCommand> クラスであり、この概要の焦点になります。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] での主な入力ソースは、マウス、キーボード、インク、およびルーティング コマンドです。  デバイス指向性の高い入力では、<xref:System.Windows.RoutedEvent> を使って、入力イベントが発生したことをアプリケーション ページのオブジェクトに通知します。  <xref:System.Windows.Input.RoutedCommand> も同様です。  <xref:System.Windows.Input.RoutedCommand.Execute%2A> の <xref:System.Windows.Input.RoutedCommand.CanExecute%2A> メソッドと <xref:System.Windows.Input.RoutedCommand> メソッドにはコマンドのアプリケーション ロジックは含まれませんが、これらのメソッドが発生させたルーティング イベントは、<xref:System.Windows.Input.CommandBinding> を含むオブジェクトに遭遇するまで、要素ツリー内をトンネリングおよびバブリングします。  <xref:System.Windows.Input.CommandBinding> にはこれらのイベント用のハンドラーが含まれ、そのハンドラーがコマンドを実行します。  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] でのルーティング イベントについて詳しくは、「[ルーティング イベントの概要](routed-events-overview.md)」をご覧ください。  
  
 <xref:System.Windows.Input.RoutedCommand.Execute%2A> の <xref:System.Windows.Input.RoutedCommand> メソッドは、コマンド ターゲットで <xref:System.Windows.Input.CommandManager.PreviewExecuted> イベントと <xref:System.Windows.Input.CommandManager.Executed> イベントを発生させます。  <xref:System.Windows.Input.RoutedCommand.CanExecute%2A> の <xref:System.Windows.Input.RoutedCommand> メソッドは、コマンド ターゲットで <xref:System.Windows.Input.CommandManager.CanExecute> イベントと <xref:System.Windows.Input.CommandManager.PreviewCanExecute> イベントを発生させます。  これらのイベントは、その特定のコマンドの <xref:System.Windows.Input.CommandBinding> を持つオブジェクトに遭遇するまで、要素ツリー内をトンネリングおよびバブリングします。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、<xref:System.Windows.Input.MediaCommands>、<xref:System.Windows.Input.ApplicationCommands>、<xref:System.Windows.Input.NavigationCommands>、<xref:System.Windows.Input.ComponentCommands>、<xref:System.Windows.Documents.EditingCommands> などの複数のクラスにわたる共通のルーティング コマンドのセットが提供されます。  これらのクラスは <xref:System.Windows.Input.RoutedCommand> オブジェクトのみで構成され、コマンドの実装ロジックは含みません。  実装ロジックは、コマンドが実行されるオブジェクトに存在します。  
  
<a name="Command_Sources"></a>   
### <a name="command-sources"></a>コマンド ソース  
 コマンド ソースは、コマンドを呼び出すオブジェクトです。  コマンド ソースの例は、<xref:System.Windows.Controls.MenuItem>、<xref:System.Windows.Controls.Button>、<xref:System.Windows.Input.KeyGesture> などです。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のコマンド ソースでは、通常、<xref:System.Windows.Input.ICommandSource> インターフェイスを実装します。  
  
 <xref:System.Windows.Input.ICommandSource> では、<xref:System.Windows.Input.ICommandSource.Command%2A>、<xref:System.Windows.Input.ICommandSource.CommandTarget%2A>、<xref:System.Windows.Input.ICommandSource.CommandParameter%2A> という 3 つのプロパティを公開します。  
  
- <xref:System.Windows.Input.ICommandSource.Command%2A> は、コマンド ソースが呼び出されたときに実行されるコマンドです。  
  
- <xref:System.Windows.Input.ICommandSource.CommandTarget%2A> は、コマンドを実行するオブジェクトです。  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、<xref:System.Windows.Input.ICommandSource.CommandTarget%2A> の <xref:System.Windows.Input.ICommandSource> プロパティは、<xref:System.Windows.Input.ICommand> が <xref:System.Windows.Input.RoutedCommand> である場合にのみ適用可能であることに注意してください。  <xref:System.Windows.Input.ICommandSource.CommandTarget%2A> で <xref:System.Windows.Input.ICommandSource> が設定されていて、対応するコマンドが <xref:System.Windows.Input.RoutedCommand> ではない場合、コマンド ターゲットは無視されます。 <xref:System.Windows.Input.ICommandSource.CommandTarget%2A> が設定されていない場合は、キーボード フォーカスを持つ要素がコマンド ターゲットになります。  
  
- <xref:System.Windows.Input.ICommandSource.CommandParameter%2A> は、コマンドを実装しているハンドラーに情報を渡すために使われるユーザー定義データ型です。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] を実装する <xref:System.Windows.Input.ICommandSource> クラスは、<xref:System.Windows.Controls.Primitives.ButtonBase>、<xref:System.Windows.Controls.MenuItem>、<xref:System.Windows.Documents.Hyperlink>、<xref:System.Windows.Input.InputBinding> です。  <xref:System.Windows.Controls.Primitives.ButtonBase>、<xref:System.Windows.Controls.MenuItem>、<xref:System.Windows.Documents.Hyperlink> は、クリックされたときにコマンドを呼び出します。<xref:System.Windows.Input.InputBinding> は、関連付けられている <xref:System.Windows.Input.InputGesture> が実行されたときにコマンドを呼び出します。  
  
 次の例では、<xref:System.Windows.Controls.MenuItem> コマンドのコマンド ソースとして、<xref:System.Windows.Controls.ContextMenu> で <xref:System.Windows.Input.ApplicationCommands.Properties%2A> を使用する方法を示します。  
  
 [!code-xaml[CommandingOverviewSnippets#CommandingOverviewCmdSourceXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml#commandingoverviewcmdsourcexaml)]  
  
 [!code-csharp[CommandingOverviewSnippets#CommandingOverviewCmdSource](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml.cs#commandingoverviewcmdsource)]
 [!code-vb[CommandingOverviewSnippets#CommandingOverviewCmdSource](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandingOverviewSnippets/visualbasic/window1.xaml.vb#commandingoverviewcmdsource)]  
  
 通常、コマンド ソースは <xref:System.Windows.Input.RoutedCommand.CanExecuteChanged> イベントをリッスンします。  このイベントは、現在のコマンド ターゲットでのコマンド実行機能が変化した可能性があることを、コマンド ソースに通知します。  コマンド ソースは、<xref:System.Windows.Input.RoutedCommand> の現在の状態を、<xref:System.Windows.Input.RoutedCommand.CanExecute%2A> メソッドを使って照会できます。  コマンドを実行できない場合、コマンド ソースはそれ自体を無効にできます。  たとえば、コマンドを実行できない場合、<xref:System.Windows.Controls.MenuItem> はそれ自体を灰色で表示します。  
  
 <xref:System.Windows.Input.InputGesture> はコマンド ソースとして使用できます。  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の 2 種類の入力ジェスチャは、<xref:System.Windows.Input.KeyGesture> と <xref:System.Windows.Input.MouseGesture> です。  <xref:System.Windows.Input.KeyGesture> は、Ctrl + C キーなどのキーボード ショートカットと考えることができます。  <xref:System.Windows.Input.KeyGesture> は、<xref:System.Windows.Input.Key> と、<xref:System.Windows.Input.ModifierKeys> のセットで構成されます。  <xref:System.Windows.Input.MouseGesture> は、<xref:System.Windows.Input.MouseAction> と、オプションの <xref:System.Windows.Input.ModifierKeys> のセットで構成されます。  
  
 <xref:System.Windows.Input.InputGesture> がコマンド ソースとして機能するためには、コマンドと関連付けられる必要があります。 これを実現するにはいくつかの方法があります。  1 つ目は、<xref:System.Windows.Input.InputBinding> を使用する方法です。  
  
 <xref:System.Windows.Input.KeyBinding> と <xref:System.Windows.Input.KeyGesture> の間に <xref:System.Windows.Input.RoutedCommand> を作成する方法の例を以下に示します。  
  
 [!code-xaml[CommandingOverviewSnippets#CommandingOverviewXAMLKeyBinding](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml#commandingoverviewxamlkeybinding)]  
  
 [!code-csharp[CommandingOverviewSnippets#CommandingOverviewKeyBinding](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml.cs#commandingoverviewkeybinding)]
 [!code-vb[CommandingOverviewSnippets#CommandingOverviewKeyBinding](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandingOverviewSnippets/visualbasic/window1.xaml.vb#commandingoverviewkeybinding)]  
  
 <xref:System.Windows.Input.InputGesture> を <xref:System.Windows.Input.RoutedCommand> に関連付けるもう 1 つの方法は、<xref:System.Windows.Input.InputGesture> の <xref:System.Windows.Input.InputGestureCollection> に <xref:System.Windows.Input.RoutedCommand> を追加するというものです。  
  
 <xref:System.Windows.Input.KeyGesture> の <xref:System.Windows.Input.InputGestureCollection> に <xref:System.Windows.Input.RoutedCommand> を追加する方法を次の例に示します。  
  
 [!code-csharp[CommandingOverviewSnippets#CommandingOverviewKeyGestureOnCmd](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml.cs#commandingoverviewkeygestureoncmd)]
 [!code-vb[CommandingOverviewSnippets#CommandingOverviewKeyGestureOnCmd](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandingOverviewSnippets/visualbasic/window1.xaml.vb#commandingoverviewkeygestureoncmd)]  
  
<a name="Command_Binding"></a>   
### <a name="commandbinding"></a>CommandBinding  
 <xref:System.Windows.Input.CommandBinding> は、コマンドと、そのコマンドを実装するイベント ハンドラーを関連付けます。  
  
 <xref:System.Windows.Input.CommandBinding> クラスには、<xref:System.Windows.Input.CommandBinding.Command%2A> プロパティと、<xref:System.Windows.Input.CommandBinding.PreviewExecuted>、<xref:System.Windows.Input.CommandBinding.Executed>、<xref:System.Windows.Input.CommandBinding.PreviewCanExecute>、<xref:System.Windows.Input.CommandBinding.CanExecute> イベントが含まれます。  
  
 <xref:System.Windows.Input.CommandBinding.Command%2A> は、<xref:System.Windows.Input.CommandBinding> が関連付けられているコマンドです。  <xref:System.Windows.Input.CommandBinding.PreviewExecuted> および <xref:System.Windows.Input.CommandBinding.Executed> イベントに関連付けられているイベント ハンドラーは、コマンド ロジックを実装します。  <xref:System.Windows.Input.CommandBinding.PreviewCanExecute> および <xref:System.Windows.Input.CommandBinding.CanExecute> イベントに関連付けられているイベント ハンドラーは、現在のコマンド ターゲットでコマンドを実行できるかどうかを判断します。  
  
 アプリケーションのルート <xref:System.Windows.Input.CommandBinding> で <xref:System.Windows.Window> を作成する方法の例を以下に示します。  <xref:System.Windows.Input.CommandBinding> は、<xref:System.Windows.Input.ApplicationCommands.Open%2A> コマンドと、<xref:System.Windows.Input.CommandManager.Executed> および <xref:System.Windows.Input.CommandBinding.CanExecute> ハンドラーを関連付けます。  
  
 [!code-xaml[commandwithhandler#CommandHandlerCommandBinding](~/samples/snippets/csharp/VS_Snippets_Wpf/commandWithHandler/CSharp/Window1.xaml#commandhandlercommandbinding)]  
  
 [!code-csharp[CommandHandlerProcedural#CommandHandlerBindingInit](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandHandlerProcedural/CSharp/Window1.xaml.cs#commandhandlerbindinginit)]
 [!code-vb[CommandHandlerProcedural#CommandHandlerBindingInit](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandHandlerProcedural/visualbasic/window1.xaml.vb#commandhandlerbindinginit)]  
  
 次に、<xref:System.Windows.Input.ExecutedRoutedEventHandler> と <xref:System.Windows.Input.CanExecuteRoutedEventHandler> が作成されます。  <xref:System.Windows.Input.ExecutedRoutedEventHandler> は、コマンドが実行されたことを知らせる文字列を表示する <xref:System.Windows.MessageBox> を開きます。  <xref:System.Windows.Input.CanExecuteRoutedEventHandler> は <xref:System.Windows.Input.CanExecuteRoutedEventArgs.CanExecute%2A> プロパティを `true` に設定します。  
  
 [!code-csharp[commandwithhandler#CommandHandlerExecutedHandler](~/samples/snippets/csharp/VS_Snippets_Wpf/commandWithHandler/CSharp/Window1.xaml.cs#commandhandlerexecutedhandler)]
 [!code-vb[commandwithhandler#CommandHandlerExecutedHandler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/commandWithHandler/VisualBasic/Window1.xaml.vb#commandhandlerexecutedhandler)]  
  
 [!code-csharp[commandwithhandler#CommandHandlerCanExecuteHandler](~/samples/snippets/csharp/VS_Snippets_Wpf/commandWithHandler/CSharp/Window1.xaml.cs#commandhandlercanexecutehandler)]
 [!code-vb[commandwithhandler#CommandHandlerCanExecuteHandler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/commandWithHandler/VisualBasic/Window1.xaml.vb#commandhandlercanexecutehandler)]  
  
 <xref:System.Windows.Input.CommandBinding> は、アプリケーションやコントロールのルート <xref:System.Windows.Window> など、特定のオブジェクトに関連付けられます。  <xref:System.Windows.Input.CommandBinding> が関連付けられるオブジェクトにより、バインドのスコープが定義されます。  たとえば、<xref:System.Windows.Input.CommandBinding> イベントは、コマンド ターゲットの先祖に関連付けられた <xref:System.Windows.Input.CommandBinding.Executed> には到達できますが、コマンド ターゲットの子孫に関連付けられた <xref:System.Windows.Input.CommandBinding> には到達できません。  これは、イベントを発生させたオブジェクトからの <xref:System.Windows.RoutedEvent> のトンネリングとバブリングの方法の直接的な結果です。  
  
 状況によっては、<xref:System.Windows.Input.CommandBinding> は、<xref:System.Windows.Controls.TextBox> クラスや <xref:System.Windows.Input.ApplicationCommands.Cut%2A>、<xref:System.Windows.Input.ApplicationCommands.Copy%2A>、<xref:System.Windows.Input.ApplicationCommands.Paste%2A> コマンドなど、コマンド ターゲット自体に関連付けられます。 ただし、ほとんどの場合は、メイン <xref:System.Windows.Input.CommandBinding> や Application オブジェクトなど、コマンド ターゲットの先祖に <xref:System.Windows.Window> を関連付ける方が便利です。同じ <xref:System.Windows.Input.CommandBinding> を複数のコマンド ターゲットに使うことができる場合は特にそうです。  これらは、コマンド実行のインフラストラクチャを作成するときに考慮する設計上の決定です。  
  
<a name="Commane_Target"></a>   
### <a name="command-target"></a>コマンド ターゲット  
 コマンド ターゲットは、コマンドが実行される要素です。  <xref:System.Windows.Input.RoutedCommand> の場合、コマンド ターゲットは、<xref:System.Windows.Input.CommandManager.Executed> および <xref:System.Windows.Input.CommandManager.CanExecute> のルーティングが開始される要素です。  前述のとおり、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、<xref:System.Windows.Input.ICommandSource.CommandTarget%2A> の <xref:System.Windows.Input.ICommandSource> プロパティは、<xref:System.Windows.Input.ICommand> が <xref:System.Windows.Input.RoutedCommand> である場合にのみ適用されます。  <xref:System.Windows.Input.ICommandSource.CommandTarget%2A> で <xref:System.Windows.Input.ICommandSource> が設定されていて、対応するコマンドが <xref:System.Windows.Input.RoutedCommand> ではない場合、コマンド ターゲットは無視されます。  
  
 コマンド ソースは、コマンド ターゲットを明示的に設定できます。  コマンド ターゲットが定義されていない場合は、キーボード フォーカスを持つ要素がコマンド ターゲットとして使われます。  キーボード フォーカスを持つ要素をコマンド ターゲットとして使うことの、アプリケーション開発者にとっての利点の 1 つは、コマンド ターゲットを追跡しなくても、同じコマンド ソースを使って、複数のターゲットでコマンドを呼び出すことができることです。  たとえば、<xref:System.Windows.Controls.MenuItem> が **コントロールと** コントロールのあるアプリケーションの<xref:System.Windows.Controls.TextBox>貼り付け<xref:System.Windows.Controls.PasswordBox>コマンドを呼び出した場合、どちらのコントロールにキーボード フォーカスがあるかに応じて、ターゲットは <xref:System.Windows.Controls.TextBox> または <xref:System.Windows.Controls.PasswordBox> になります。  
  
 次の例では、マークアップおよび分離コードでコマンド ターゲットを明示的に設定する方法を示します。  
  
 [!code-xaml[CommandingOverviewSnippets#CommandingOverviewXAMLCommandTarget](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml#commandingoverviewxamlcommandtarget)]  
  
 [!code-csharp[CommandingOverviewSnippets#CommandingOverviewCommandTargetCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml.cs#commandingoverviewcommandtargetcodebehind)]
 [!code-vb[CommandingOverviewSnippets#CommandingOverviewCommandTargetCodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandingOverviewSnippets/visualbasic/window1.xaml.vb#commandingoverviewcommandtargetcodebehind)]  
  
<a name="Command_Manager"></a>   
### <a name="the-commandmanager"></a>CommandManager  
 <xref:System.Windows.Input.CommandManager> は、コマンドに関連する複数の関数を提供します。  特定の要素の <xref:System.Windows.Input.CommandManager.PreviewExecuted>、<xref:System.Windows.Input.CommandManager.Executed>、<xref:System.Windows.Input.CommandManager.PreviewCanExecute>、<xref:System.Windows.Input.CommandManager.CanExecute> イベント ハンドラーを追加および削除するための、静的メソッドのセットを提供します。  <xref:System.Windows.Input.CommandBinding> および <xref:System.Windows.Input.InputBinding> オブジェクトを特定のクラスに登録するための手段を提供します。  また、<xref:System.Windows.Input.CommandManager> は、<xref:System.Windows.Input.CommandManager.RequerySuggested> イベントを発生させる必要があるタイミングをコマンドに通知する手段を、<xref:System.Windows.Input.ICommand.CanExecuteChanged> イベントによって提供します。  
  
 <xref:System.Windows.Input.CommandManager.InvalidateRequerySuggested%2A> メソッドは、<xref:System.Windows.Input.CommandManager> に <xref:System.Windows.Input.CommandManager.RequerySuggested> イベントを強制的に発生させます。  これは、コマンドを無効または有効にする必要がある状況であっても、<xref:System.Windows.Input.CommandManager> がそれを認識しない場合に便利です。  
  
<a name="Command_Library"></a>   
## <a name="command-library"></a>コマンド ライブラリ  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、定義済みのコマンドのセットが用意されています。  コマンド ライブラリは、<xref:System.Windows.Input.ApplicationCommands>、<xref:System.Windows.Input.NavigationCommands>、<xref:System.Windows.Input.MediaCommands>、<xref:System.Windows.Documents.EditingCommands>、<xref:System.Windows.Input.ComponentCommands> というクラスで構成されています。  これらのクラスでは、<xref:System.Windows.Input.ApplicationCommands.Cut%2A>、<xref:System.Windows.Input.NavigationCommands.BrowseBack%2A> と <xref:System.Windows.Input.NavigationCommands.BrowseForward%2A>、<xref:System.Windows.Input.MediaCommands.Play%2A>、<xref:System.Windows.Input.MediaCommands.Stop%2A>、<xref:System.Windows.Input.MediaCommands.Pause%2A> などのコマンドが提供されます。  
  
 これらのコマンドの多くには、既定の入力バインディングのセットが含まれます。  たとえば、アプリケーションでコピーコマンドを処理するように指定すると、キーボードバインド "CTRL + C" が自動的に取得されます。これには、Tablet PC のペンジェスチャや音声情報など、他の入力デバイスのバインドも表示されます。  
  
 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] を使ってさまざまなコマンド ライブラリのコマンドを参照するときは、通常、静的なコマンド プロパティを公開するライブラリ クラスのクラス名を省略できます。 一般に、コマンド名に文字列としての曖昧さはなく、所有する型は、コマンドの論理グループを提供するために存在しますが、曖昧さの解消のためには必要ありません。 たとえば、詳細な `Command="Cut"` ではなく`Command="ApplicationCommands.Cut"` と指定できます。 これは、コマンドに対する [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサに組み込まれた便利なメカニズムです (つまり、これは <xref:System.Windows.Input.ICommand> の型コンバーターの動作であり、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサは読み込み時にこれを参照します)。  
  
<a name="creating_commands"></a>   
## <a name="creating-custom-commands"></a>カスタム コマンドの作成  
 コマンド ライブラリ クラスのコマンドがニーズを満たさない場合は、独自のコマンドを作成できます。  カスタム コマンドを作成するには 2 つの方法があります。  1 つは、一から始めて <xref:System.Windows.Input.ICommand> インターフェイスを実装する方法です。  もう 1 つのさらに一般的な方法は、<xref:System.Windows.Input.RoutedCommand> または <xref:System.Windows.Input.RoutedUICommand> を作成するものです。  
  
 カスタムの <xref:System.Windows.Input.RoutedCommand> を作成する例については、「[Create a Custom RoutedCommand Sample](https://github.com/Microsoft/WPF-Samples/tree/master/Input%20and%20Commands/CustomRoutedCommand)」 (カスタム RoutedCommand の作成のサンプル) を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Input.RoutedCommand>
- <xref:System.Windows.Input.CommandBinding>
- <xref:System.Windows.Input.InputBinding>
- <xref:System.Windows.Input.CommandManager>
- [入力の概要](input-overview.md)
- [ルーティング イベントの概要](routed-events-overview.md)
- [ICommandSource を実装する](how-to-implement-icommandsource.md)
- [方法: メニュー アイテムにコマンドを追加する](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms741839(v=vs.90))
- [カスタム RoutedCommand の作成のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Input%20and%20Commands/CustomRoutedCommand)
