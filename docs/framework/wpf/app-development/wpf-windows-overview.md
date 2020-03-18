---
title: Windows の概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- XAML [WPF], displaying content via
- XAML pages [WPF], displaying
- content [WPF], displaying via XAML
- window objects [WPF]
- hosting [WPF], applications
- managing windows [WPF]
- dialog boxes [WPF]
- Page object [WPF]
- NavigationWindow objects [WPF]
- applications [WPF], hosting
- content [WPF], displaying
- events [WPF]
- content [WPF], displaying via procedural code
- modal windows [WPF]
- procedural code [WPF], displaying content via
- displaying content via procedural code [WPF]
- window management [WPF]
- displaying content [WPF]
- window events [WPF]
- windows [WPF]
- modal dialog boxes [WPF]
- displaying XAML pages [WPF]
ms.assetid: 737d04ec-8861-46c3-8d44-fa11d3528d23
ms.openlocfilehash: 17582192fabf85777bba250f6f53047a84f264b9
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742359"
---
# <a name="wpf-windows-overview"></a>WPF ウィンドウの概要
ユーザーは、Windows を通じて Windows Presentation Foundation (WPF) スタンドアロンアプリケーションと対話します。 ウィンドウの主な目的は、データを視覚化してユーザーがデータと対話できるコンテンツをホストすることです。 スタンドアロン WPF アプリケーションは、<xref:System.Windows.Window> クラスを使用して独自のウィンドウを提供します。 このトピックでは、スタンドアロンアプリケーションでのウィンドウの作成と管理の基礎について説明する前に、<xref:System.Windows.Window> について説明します。  
  
> [!NOTE]
> XAML ブラウザーアプリケーション (Xbap) や疎 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] ページなど、ブラウザーでホストされる WPF アプリケーションは、独自のウィンドウを提供しません。 代わりに、Windows Internet Explorer によって提供される windows でホストされます。 「 [WPF XAML ブラウザーアプリケーションの概要](wpf-xaml-browser-applications-overview.md)」を参照してください。  

<a name="TheWindowClass"></a>   
## <a name="the-window-class"></a>ウィンドウ クラス  
 次の図は、ウィンドウの構成部分を示しています。  
  
 ![ウィンドウ要素を示すスクリーンショット。](./media/wpf-windows-overview/window-constituent-elements.png)  
  
 ウィンドウは、非クライアント領域とクライアント領域の 2 つに分かれます。  
  
 ウィンドウの*非クライアント領域*は WPF によって実装され、次のような、ほとんどのウィンドウに共通のウィンドウ部分が含まれています。  
  
- 境界線。  
  
- タイトル バー。  
  
- アイコン。  
  
- 最小化ボタン、最大化ボタン、および元に戻すボタン。  
  
- 閉じるボタン。  
  
- ウィンドウを最小化、最大化、元のサイズに戻す、移動、サイズ変更、および閉じるためのメニュー項目を含むシステム メニュー。  
  
 ウィンドウの*クライアント領域*は、ウィンドウの非クライアント領域内の領域であり、開発者がメニューバー、ツールバー、コントロールなどのアプリケーション固有のコンテンツを追加するために使用されます。  
  
 WPF では、ウィンドウは、次の操作を実行するために使用する <xref:System.Windows.Window> クラスによってカプセル化されます。  
  
- ウィンドウを表示する。  
  
- ウィンドウのサイズ、位置、および外観を構成する。  
  
- アプリケーション固有のコンテンツをホストする。  
  
- ウィンドウの有効期間を管理する。  
  
<a name="DefiningAWindow"></a>   
## <a name="implementing-a-window"></a>ウィンドウの実装  
 一般的なウィンドウの実装は、外観と動作の両方で構成されます。*外観*はウィンドウがユーザーにどのように見えるかを定義し、*動作*は、ユーザーがウィンドウを操作するときにウィンドウがどのように機能するかを定義します。 WPF では、コードまたは [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] マークアップのいずれかを使用して、ウィンドウの外観と動作を実装できます。  
  
 ただし、一般に、ウィンドウの外観は [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] マークアップを使用して実装され、その動作は分離コードを使用して実装されます。次に例を示します。  
  
 [!code-xaml[WindowsOverviewSnippets#MarkupAndCodeBehindWindowMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/MarkupAndCodeBehindWindow.xaml#markupandcodebehindwindowmarkup)]  
  
 [!code-csharp[WindowsOverviewSnippets#MarkupAndCodeBehindWindowCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/MarkupAndCodeBehindWindow.xaml.cs#markupandcodebehindwindowcodebehind)]
 [!code-vb[WindowsOverviewSnippets#MarkupAndCodeBehindWindowCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WindowsOverviewSnippets/VisualBasic/MarkupAndCodeBehindWindow.xaml.vb#markupandcodebehindwindowcodebehind)]  
  
 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] マークアップファイルと分離コードファイルを連携させるには、次のものが必要です。  
  
- マークアップでは、`Window` 要素に `x:Class` 属性が含まれている必要があります。 アプリケーションがビルドされると、マークアップファイルに `x:Class` が存在することにより、Microsoft build engine (MSBuild) によって <xref:System.Windows.Window> から派生した `partial` クラスが作成され、`x:Class` 属性によって指定された名前になります。 そのためには、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] スキーマ (`xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`) の XML 名前空間宣言を追加する必要があります。 生成された `partial` クラスは、`InitializeComponent` メソッドを実装します。このメソッドは、イベントを登録し、マークアップで実装されるプロパティを設定するために呼び出されます。  
  
- 分離コードでは、クラスは、マークアップで `x:Class` 属性によって指定された名前と同じ名前を持つ `partial` クラスである必要があり、<xref:System.Windows.Window>から派生する必要があります。 これにより、分離コードファイルは、アプリケーションのビルド時にマークアップファイル用に生成される `partial` クラスに関連付けられます (「 [WPF アプリケーションのビルド](building-a-wpf-application-wpf.md)」を参照してください)。  
  
- 分離コードでは、<xref:System.Windows.Window> クラスは、`InitializeComponent` メソッドを呼び出すコンストラクターを実装する必要があります。 `InitializeComponent` は、マークアップファイルによって生成される `partial` クラスによって実装され、イベントを登録したり、マークアップで定義されているプロパティを設定したりします。  
  
> [!NOTE]
> Visual Studio を使用して新しい <xref:System.Windows.Window> をプロジェクトに追加すると、<xref:System.Windows.Window> はマークアップと分離コードの両方を使用して実装され、ここで説明するマークアップファイルと分離コードファイルの関連付けを作成するために必要な構成が含まれます。  
  
 この構成を使用して、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] マークアップでのウィンドウの外観を定義し、分離コードでその動作を実装することに集中できます。 次の例では、ボタンが [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] マークアップに実装されているウィンドウと、コードビハインドで実装されているボタンの <xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントのイベントハンドラーを示します。  
  
 [!code-xaml[WindowsOverviewWindowWithButtonSnippets#MarkupAndCodeBehindWindowMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewWindowWithButtonSnippets/CSharp/MarkupAndCodeBehindWindow.xaml#markupandcodebehindwindowmarkup)]  
  
 [!code-csharp[WindowsOverviewWindowWithButtonSnippets#MarkupAndCodeBehindWindowCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewWindowWithButtonSnippets/CSharp/MarkupAndCodeBehindWindow.xaml.cs#markupandcodebehindwindowcodebehind)]
 [!code-vb[WindowsOverviewWindowWithButtonSnippets#MarkupAndCodeBehindWindowCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WindowsOverviewWindowWithButtonSnippets/VisualBasic/MarkupAndCodeBehindWindow.xaml.vb#markupandcodebehindwindowcodebehind)]  
  
<a name="ConfiguringWindowForMSBuild"></a>   
## <a name="configuring-a-window-definition-for-msbuild"></a>MSBuild 用のウィンドウ定義の構成  
 ウィンドウを実装する方法によって、MSBuild に対してどのように構成されるかが決まります。 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] マークアップと分離コードの両方を使用して定義されたウィンドウの場合:  
  
- XAML マークアップファイルは、MSBuild `Page` 項目として構成されます。  
  
- 分離コードファイルは、MSBuild `Compile` 項目として構成されます。  
  
 これは、次の MSBuild プロジェクトファイルに示されています。  
  
```xml  
<Project ...  
                xmlns="http://schemas.microsoft.com/developer/msbuild/2003">  
    ...  
    <Page Include="MarkupAndCodeBehindWindow.xaml" />  
    <Compile Include=" MarkupAndCodeBehindWindow.xaml.cs" />  
    ...  
</Project>  
```  
  
 WPF アプリケーションのビルドの詳細については、「 [Wpf アプリケーションのビルド](building-a-wpf-application-wpf.md)」を参照してください。  
  
<a name="WindowLifetime"></a>   
## <a name="window-lifetime"></a>ウィンドウの有効期間  
 クラスと同様に、ウィンドウにも有効期間があります。有効期間は、ウィンドウが開いて最初にインスタンス化されたときに開始し、アクティブ化と非アクティブ化を経て、最後に閉じられるまで継続します。  

<a name="Opening_a_Window"></a>   
### <a name="opening-a-window"></a>ウィンドウを開く  
 ウィンドウを開くには、次の例に示すように最初にインスタンスを作成します。  
  
 [!code-xaml[WindowsOverviewStartupEventSnippets#AppMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewStartupEventSnippets/CSharp/App.xaml#appmarkup)]  
  
 [!code-csharp[WindowsOverviewStartupEventSnippets#AppCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewStartupEventSnippets/CSharp/App.xaml.cs#appcodebehind)]  
  
 この例では、アプリケーションの起動時に、`MarkupAndCodeBehindWindow` がインスタンス化されます。これは、<xref:System.Windows.Application.Startup> イベントが発生したときに発生します。  
  
 ウィンドウがインスタンス化されると、そのウィンドウへの参照が、<xref:System.Windows.Application> オブジェクトによって管理されるウィンドウの一覧に自動的に追加されます (<xref:System.Windows.Application.Windows%2A?displayProperty=nameWithType>を参照)。 また、既定では、インスタンス化される最初のウィンドウは、メインアプリケーションウィンドウとして <xref:System.Windows.Application> によって設定されます (<xref:System.Windows.Application.MainWindow%2A?displayProperty=nameWithType>を参照してください)。  
  
 最後に、<xref:System.Windows.Window.Show%2A> メソッドを呼び出してウィンドウを開きます。結果を次の図に示します。  
  
 ![Window. Show を呼び出して開かれたウィンドウです。](./media/wpf-windows-overview//window-opened-show-method.png)  
  
 <xref:System.Windows.Window.Show%2A> を呼び出すことによって開かれるウィンドウは、モードレスウィンドウです。つまり、アプリケーションは、ユーザーが同じアプリケーション内の他のウィンドウをアクティブ化できるモードで動作します。  
  
> [!NOTE]
> ダイアログボックスなどのウィンドウをモーダルで開くために <xref:System.Windows.Window.ShowDialog%2A> が呼び出されます。 詳細については、「[ダイアログボックスの概要](dialog-boxes-overview.md)」を参照してください。  
  
 <xref:System.Windows.Window.Show%2A> が呼び出されると、ウィンドウは、ユーザー入力を受け取ることを許可するインフラストラクチャを確立するために、表示される前に初期化作業を実行します。 ウィンドウが初期化されると、<xref:System.Windows.Window.SourceInitialized> イベントが発生し、ウィンドウが表示されます。  
  
 ショートカットとして、<xref:System.Windows.Application.StartupUri%2A> を設定して、アプリケーションの起動時に自動的に開かれる最初のウィンドウを指定できます。  
  
 [!code-xaml[WindowsOverviewSnippets#ApplicationStartupUriMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/App.xaml#applicationstartupurimarkup)]  
  
 アプリケーションが起動すると、<xref:System.Windows.Application.StartupUri%2A> の値によって指定されたウィンドウがモードレスで; に開かれます。内部的には、<xref:System.Windows.Window.Show%2A> メソッドを呼び出すことによってウィンドウが開きます。  
  
<a name="Ownership"></a>   
#### <a name="window-ownership"></a>ウィンドウの所有権  
 <xref:System.Windows.Window.Show%2A> メソッドを使用して開いたウィンドウには、それを作成したウィンドウとの暗黙的な関係がありません。ユーザーは、どちらのウィンドウとも独立して対話できます。つまり、どちらのウィンドウも次の操作を実行できます。  
  
- もう一方をカバーします (いずれかのウィンドウの [<xref:System.Windows.Window.Topmost%2A>] プロパティが [`true`] に設定されている場合を除きます)。  
  
- もう一方のウィンドウに影響を与えずに、最小化/最大化し、元のサイズに戻す。  
  
 一部のウィンドウには、そのウィンドウを開いたウィンドウとの関係が必要です。 たとえば、統合開発環境 (IDE: Integrated Development Environment) アプリケーションでは、プロパティウィンドウとツールウィンドウを開くことができます。このウィンドウは、通常の動作が、そのウィンドウを作成するウィンドウをカバーするために使用されます。 また、そのようなウィンドウは、必ず作成元のウィンドウと一緒に閉じ、最小化/最大化し、元のサイズに戻す必要があります。 このような関係を確立するには、1*つのウィンドウを別の*ウィンドウにし、所有*ウィンドウ*の [<xref:System.Windows.Window.Owner%2A>] プロパティに [*所有者] ウィンドウ*への参照を設定します。 次の例を参照してください。  
  
 [!code-csharp[WindowOwnerOwnedWindowsSnippets#SetWindowOwnerCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowOwnerOwnedWindowsSnippets/CSharp/MainWindow.xaml.cs#setwindowownercode)]
 [!code-vb[WindowOwnerOwnedWindowsSnippets#SetWindowOwnerCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WindowOwnerOwnedWindowsSnippets/visualbasic/mainwindow.xaml.vb#setwindowownercode)]  
  
 所有権が確立されると、次のようになります。  
  
- 所有ウィンドウは、その <xref:System.Windows.Window.Owner%2A> プロパティの値を調べることによって所有者ウィンドウを参照できます。  
  
- [所有者] ウィンドウでは、<xref:System.Windows.Window.OwnedWindows%2A> プロパティの値を調べることによって、所有しているすべてのウィンドウを検出できます。  
  
<a name="Preventing"></a>   
#### <a name="preventing-window-activation"></a>ウィンドウのアクティブ化の防止  
 表示されたときにウィンドウをアクティブにしないシナリオがあります。たとえば、電子メールアプリケーションのインターネット messenger スタイルのアプリケーションや通知ウィンドウのメッセージ交換ウィンドウなどです。  
  
 アプリケーションに、表示されたときにアクティブにならないウィンドウがある場合は、<xref:System.Windows.Window.Show%2A> メソッドを初めて呼び出す前に、その <xref:System.Windows.Window.ShowActivated%2A> プロパティを `false` に設定できます。 結果は次のようになります。  
  
- ウィンドウはアクティブになりません。  
  
- ウィンドウの <xref:System.Windows.Window.Activated> イベントが発生していません。  
  
- 現在アクティブなウィンドウは、アクティブのままです。  
  
 ただし、ユーザーがクライアント領域または非クライアント領域をクリックすると、ウィンドウは直ちにアクティブになります。 この場合、次のようになります。  
  
- ウィンドウはアクティブになります。  
  
- ウィンドウの <xref:System.Windows.Window.Activated> イベントが発生します。  
  
- 直前にアクティブだったウィンドウは非アクティブになります。  
  
- その後、ユーザーの操作に応答して、ウィンドウの <xref:System.Windows.Window.Deactivated> イベントと <xref:System.Windows.Window.Activated> イベントが期待どおりに発生します。  
  
<a name="Window_Activation"></a>   
### <a name="window-activation"></a>ウィンドウのアクティブ化  
 ウィンドウを初めて開いたときには、アクティブなウィンドウになります (<xref:System.Windows.Window.ShowActivated%2A> が `false`に設定されている場合を除きます)。 *アクティブウィンドウ*は、キーストロークやマウスクリックなど、現在ユーザー入力をキャプチャしているウィンドウです。 ウィンドウがアクティブになると、<xref:System.Windows.Window.Activated> イベントが発生します。  
  
> [!NOTE]
> ウィンドウを最初に開いたときに、<xref:System.Windows.FrameworkElement.Loaded> イベントと <xref:System.Windows.Window.ContentRendered> イベントが発生するのは、<xref:System.Windows.Window.Activated> イベントが発生した後だけです。 これを念頭に置いて、<xref:System.Windows.Window.ContentRendered> が発生したときに、ウィンドウを効果的に開いていると見なすことができます。  
  
 ウィンドウがアクティブになった後で、ユーザーは同じアプリケーションの別のウィンドウをアクティブ化したり、別のアプリケーションをアクティブ化したりできます。 これが発生すると、現在アクティブなウィンドウが非アクティブ化され、<xref:System.Windows.Window.Deactivated> イベントが発生します。 同様に、ユーザーが現在非アクティブになっているウィンドウを選択すると、ウィンドウが再びアクティブになり <xref:System.Windows.Window.Activated> が発生します。  
  
 <xref:System.Windows.Window.Activated> と <xref:System.Windows.Window.Deactivated> を処理する一般的な理由の1つは、ウィンドウがアクティブなときにのみ実行できる機能を有効または無効にすることです。 たとえば、ゲームやビデオ プレーヤーなど、ユーザーの一定の入力や介入が必要な対話型コンテンツが表示されるウィンドウがあります。 次の例は、この動作を実装するために <xref:System.Windows.Window.Activated> と <xref:System.Windows.Window.Deactivated> を処理する方法を示す簡略化されたビデオプレーヤーです。  
  
 [!code-xaml[WindowsOverviewSnippets#ActivationDeactivationMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/CustomMediaPlayerWindow.xaml#activationdeactivationmarkup)]  
  
 [!code-csharp[WindowsOverviewSnippets#ActivationDeactivationCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/CustomMediaPlayerWindow.xaml.cs#activationdeactivationcodebehind)]
 [!code-vb[WindowsOverviewSnippets#ActivationDeactivationCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WindowsOverviewSnippets/VisualBasic/CustomMediaPlayerWindow.xaml.vb#activationdeactivationcodebehind)]  
  
 ウィンドウが非アクティブでも、バックグラウンドでコードを実行できる種類のアプリケーションもあります。 たとえば、メール クライアントは、ユーザーが他のアプリケーションを使用している間もメール サーバーへのポーリングを続けています。 このようなアプリケーションは、メイン ウィンドウが非アクティブのときにも、別の動作や追加の動作を頻繁に実行します。 メール プログラムでは、新しいメール アイテムを受信トレイに追加し、通知アイコンをシステム トレイに追加することがあります。 通知アイコンは、メールウィンドウがアクティブでない場合にのみ表示する必要があります。これは、<xref:System.Windows.Window.IsActive%2A> プロパティを調べることによって決定できます。  
  
 バックグラウンドタスクが完了すると、<xref:System.Windows.Window.Activate%2A> メソッドを呼び出すことにより、ユーザーにより緊急通知を行うことができます。 <xref:System.Windows.Window.Activate%2A> が呼び出されたときに、ユーザーが別のアプリケーションと対話している場合は、ウィンドウのタスクバーボタンが点滅します。 ユーザーが現在のアプリケーションと対話している場合は、<xref:System.Windows.Window.Activate%2A> を呼び出すとウィンドウが前面に表示されます。  
  
> [!NOTE]
> <xref:System.Windows.Application.Activated?displayProperty=nameWithType> イベントと <xref:System.Windows.Application.Deactivated?displayProperty=nameWithType> イベントを使用して、アプリケーションスコープのアクティブ化を処理できます。  
  
<a name="Closing_a_Window"></a>   
### <a name="closing-a-window"></a>ウィンドウを閉じる  
 ウィンドウの有効期間は、表示されたときに開始し、ユーザーが閉じたときに終了します。 ウィンドウを閉じるには、非クライアント領域の要素を使用します。これには、次のものが含まれます。  
  
- **システム**メニューの**閉じる**項目。  
  
- Alt キーを押しながら F4 キーを押す。  
  
- **[閉じる]** ボタンを押します。  
  
 クライアント領域にさらに機構を追加してウィンドウを閉じることもできます。その一般的な例を、次に示します。  
  
- 通常はメインアプリケーションウィンドウの **[ファイル]** メニューの **[終了]** 項目。  
  
- 通常はセカンダリアプリケーションウィンドウで、 **[ファイル]** メニューの **[閉じる]** 項目。  
  
- **[キャンセル**] ボタン (通常はモーダルダイアログボックス)。  
  
- **[閉じる]** ボタン。通常は、モードレスダイアログボックスを選択します。  
  
 これらのカスタム機構のいずれかに応答してウィンドウを閉じるには、<xref:System.Windows.Window.Close%2A> メソッドを呼び出す必要があります。 次の例では、 **[ファイル]** メニューの **[終了]** をクリックして、ウィンドウを閉じる機能を実装しています。  
  
 [!code-xaml[WindowsOverviewSnippets#WindowWithFileExitMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/WindowWithFileExit.xaml#windowwithfileexitmarkup)]  
  
 [!code-csharp[WindowsOverviewSnippets#WindowWithFileExitCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/WindowWithFileExit.xaml.cs#windowwithfileexitcodebehind)]
 [!code-vb[WindowsOverviewSnippets#WindowWithFileExitCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WindowsOverviewSnippets/VisualBasic/WindowWithFileExit.xaml.vb#windowwithfileexitcodebehind)]  
  
 ウィンドウが閉じると、<xref:System.Windows.Window.Closing> と <xref:System.Windows.Window.Closed>の2つのイベントが発生します。  
  
 ウィンドウが閉じられる前に <xref:System.Windows.Window.Closing> が発生し、ウィンドウを閉じることができないようにするためのメカニズムが用意されています。 ウィンドウが閉じるのを防ぐのは、一般的に、ウィンドウ コンテンツに変更したデータが含まれている場合です。 このような状況では、<xref:System.Windows.Window.Closing> イベントを処理して、データがダーティかどうかを判断し、データがダーティであるかどうかを判断し、データを保存せずにウィンドウを閉じるか、ウィンドウの終了を取り消すかどうかをユーザーに確認することができます。 次の例は、<xref:System.Windows.Window.Closing>を処理する主な側面を示しています。  
  
 [!code-csharp[WindowClosingSnippets](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowClosingSnippets/CSharp/DataWindow.xaml.cs)]
 [!code-vb[WindowClosingSnippets](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WindowClosingSnippets/visualbasic/datawindow.xaml.vb)]  

 <xref:System.Windows.Window.Closing> イベントハンドラーには <xref:System.ComponentModel.CancelEventArgs>が渡されます。これは、ウィンドウが閉じられないように `true` に設定した `Boolean`<xref:System.ComponentModel.CancelEventArgs.Cancel%2A> プロパティを実装します。  
  
 <xref:System.Windows.Window.Closing> が処理されない場合、または処理されても取り消されない場合は、ウィンドウが閉じます。 ウィンドウが実際に閉じられる直前に、<xref:System.Windows.Window.Closed> が発生します。 この時点で、ウィンドウが閉じるのを防ぐことはできません。  
  
> [!NOTE]
> メインアプリケーションウィンドウを閉じるか、最後のウィンドウを閉じると、アプリケーションを自動的にシャットダウンするように構成できます (「<xref:System.Windows.Application.MainWindow%2A>」を参照)。 詳細については、<xref:System.Windows.Application.ShutdownMode%2A> を参照してください。  
  
 ウィンドウは、非クライアント領域とクライアント領域に用意されている機構を使用して明示的に閉じることができますが、次のようなアプリケーションまたはウィンドウの他の部分での動作の結果として、ウィンドウを暗黙的に閉じることもできます。  
  
- ユーザーは、Windows をログオフまたはシャットダウンします。  
  
- ウィンドウの所有者が閉じます (「<xref:System.Windows.Window.Owner%2A>)」を参照してください。  
  
- メインアプリケーションウィンドウが閉じられ、<xref:System.Windows.Application.ShutdownMode%2A> が <xref:System.Windows.ShutdownMode.OnMainWindowClose>ます。  
  
- <xref:System.Windows.Application.Shutdown%2A> が呼び出されます  
  
> [!NOTE]
> ウィンドウを閉じると、再度開くことはできません。  
  
<a name="Window_Lifetime_Events"></a>   
### <a name="window-lifetime-events"></a>ウィンドウの有効期間イベント  
 次の図は、ウィンドウの有効期間におけるプリンシパルイベントのシーケンスを示しています。  
  
 ![ウィンドウの有効期間内のイベントを表示する図。](./media/wpf-windows-overview/window-lifetime-events.png)  
  
 次の図は、アクティブ化せずに表示されるウィンドウの有効期間におけるプリンシパルイベントのシーケンスを示しています (<xref:System.Windows.Window.ShowActivated%2A> は、ウィンドウが表示される前に `false` に設定されています)。  
  
 ![アクティブ化を行わずに、ウィンドウの有効期間内のイベントを表示する図。](./media/wpf-windows-overview/window-lifetime-no-activation.png)  
  
<a name="WindowLocation"></a>   
## <a name="window-location"></a>ウィンドウの位置  
 ウィンドウが開いているとき、ウィンドウはデスクトップに対して相対的な x ディメンションと y ディメンションの位置にあります。 この場所は、<xref:System.Windows.Window.Left%2A> と <xref:System.Windows.Window.Top%2A> のプロパティをそれぞれ調べることによって決定できます。 これらのプロパティを設定して、ウィンドウの位置を変更できます。  
  
 また、<xref:System.Windows.Window.WindowStartupLocation%2A> プロパティに次のいずれかの <xref:System.Windows.WindowStartupLocation> 列挙値を設定して最初に表示されたときに <xref:System.Windows.Window> の初期位置を指定することもできます。  
  
- <xref:System.Windows.WindowStartupLocation.CenterOwner> (規定値)  
  
- <xref:System.Windows.WindowStartupLocation.CenterScreen>  
  
- <xref:System.Windows.WindowStartupLocation.Manual>  
  
 起動時の場所が <xref:System.Windows.WindowStartupLocation.Manual>として指定され、<xref:System.Windows.Window.Left%2A> プロパティと <xref:System.Windows.Window.Top%2A> プロパティが設定されていない場合 <xref:System.Windows.Window> は、に表示される場所を Windows に確認します。  
  
<a name="Topmost_Windows_and_Z_Order"></a>   
### <a name="topmost-windows-and-z-order"></a>最上位ウィンドウと Z オーダー  
 ウィンドウには、x 位置と y 位置に加えて、他のウィンドウを基準にして垂直位置を決定する z ディメンションの位置もあります。 これはウィンドウの z オーダーともいい、標準 z オーダーと最上位 z オーダーの 2 種類があります。 *通常の z オーダー*におけるウィンドウの位置は、現在アクティブかどうかによって決まります。 既定では、ウィンドウは標準 z オーダーにあります。 *最上位 z オーダー*のウィンドウの位置は、現在アクティブかどうかによって決まります。 また、最上位 z オーダーにあるウィンドウは、常に、標準 z オーダーにあるウィンドウの上に位置します。 ウィンドウは、その <xref:System.Windows.Window.Topmost%2A> プロパティを `true`に設定することにより、最上位の z オーダーに配置されます。  
  
 [!code-xaml[WindowsOverviewSnippets#TopmostWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/TopmostWindow.xaml#topmostwindowmarkup1)]  
  
 各 z オーダー内では、現在アクティブなウィンドウは、同じ z オーダーにある他のすべてのウィンドウの上に表示されます。  
  
<a name="WindowSize"></a>   
## <a name="window-size"></a>ウィンドウ サイズ  
 ウィンドウのサイズは、デスクトップの位置を指定するだけでなく、さまざまな幅と高さのプロパティ、<xref:System.Windows.Window.SizeToContent%2A>など、いくつかのプロパティによって決定されます。  
  
 <xref:System.Windows.FrameworkElement.MinWidth%2A>、<xref:System.Windows.FrameworkElement.Width%2A>、および <xref:System.Windows.FrameworkElement.MaxWidth%2A> は、有効期間中にウィンドウが持つことができる幅の範囲を管理するために使用され、次の例に示すように構成されます。  
  
 [!code-xaml[WindowsOverviewSnippets#WidthWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/WidthWindow.xaml#widthwindowmarkup1)]  
  
 ウィンドウの高さは、<xref:System.Windows.FrameworkElement.MinHeight%2A>、<xref:System.Windows.FrameworkElement.Height%2A>、および <xref:System.Windows.FrameworkElement.MaxHeight%2A>によって管理され、次の例に示すように構成されます。  
  
 [!code-xaml[WindowsOverviewSnippets#HeightWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/HeightWindow.xaml#heightwindowmarkup1)]  
  
 さまざまな幅の値と高さの値はそれぞれ範囲を指定しているため、サイズを変更できるウィンドウの幅と高さは、それぞれの寸法に指定された範囲内のいずれかの値を取ります。 現在の幅と高さを検出するには、<xref:System.Windows.FrameworkElement.ActualWidth%2A> と <xref:System.Windows.FrameworkElement.ActualHeight%2A>をそれぞれ調べます。  
  
 ウィンドウの幅と高さに、ウィンドウのコンテンツのサイズに合わせてサイズを設定する場合は、次の値を持つ <xref:System.Windows.Window.SizeToContent%2A> プロパティを使用できます。  
  
- [https://login.microsoftonline.com/consumers/](<xref:System.Windows.SizeToContent.Manual>) 効果 (既定値)。  
  
- [https://login.microsoftonline.com/consumers/](<xref:System.Windows.SizeToContent.Width>) コンテンツの幅に合わせる。 <xref:System.Windows.FrameworkElement.MinWidth%2A> と <xref:System.Windows.FrameworkElement.MaxWidth%2A> の両方をコンテンツの幅に設定するのと同じ効果があります。  
  
- [https://login.microsoftonline.com/consumers/](<xref:System.Windows.SizeToContent.Height>) コンテンツの高さに合わせる。 <xref:System.Windows.FrameworkElement.MinHeight%2A> と <xref:System.Windows.FrameworkElement.MaxHeight%2A> の両方をコンテンツの高さに設定するのと同じ効果があります。  
  
- [https://login.microsoftonline.com/consumers/](<xref:System.Windows.SizeToContent.WidthAndHeight>) コンテンツの幅と高さに合わせる。 <xref:System.Windows.FrameworkElement.MinHeight%2A> と <xref:System.Windows.FrameworkElement.MaxHeight%2A> の両方をコンテンツの高さに設定し、<xref:System.Windows.FrameworkElement.MinWidth%2A> と <xref:System.Windows.FrameworkElement.MaxWidth%2A> の両方をコンテンツの幅に設定した場合と同じ効果を持ちます。  
  
 次の例では、ウィンドウを最初に表示するときに、そのコンテンツに合わせて垂直方向と水平方向の両方のサイズを自動的に変更するウィンドウを示しています。  
  
 [!code-xaml[WindowsOverviewSnippets#SizeToContentWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/SizeToContentWindow.xaml#sizetocontentwindowmarkup1)]  
  
 次の例では、コンテンツに合わせてウィンドウのサイズを変更する方法を指定するために、コードで <xref:System.Windows.Window.SizeToContent%2A> プロパティを設定する方法を示します。
  
 [!code-csharp[HOWTOWindowManagementSnippets#SetWindowSizeToContentPropertyCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/HOWTOWindowManagementSnippets/CSharp/MainWindow.xaml.cs#setwindowsizetocontentpropertycode)]
 [!code-vb[HOWTOWindowManagementSnippets#SetWindowSizeToContentPropertyCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HOWTOWindowManagementSnippets/visualbasic/mainwindow.xaml.vb#setwindowsizetocontentpropertycode)]  
  
<a name="OrderOfPrecedence"></a>   
## <a name="order-of-precedence-for-sizing-properties"></a>サイズ変更プロパティの優先順位  
 基本的に、ウィンドウのさまざまなサイズのプロパティを組み合わせて、サイズを変更できるウィンドウの幅と高さの範囲を定義します。 有効な範囲を確実に維持するために、<xref:System.Windows.Window> は次の優先順位を使用してサイズプロパティの値を評価します。  
  
 **高さのプロパティ:**  
  
1. <xref:System.Windows.FrameworkElement.MinHeight%2A?displayProperty=nameWithType>
  
2. <xref:System.Windows.FrameworkElement.MaxHeight%2A?displayProperty=nameWithType>
  
3. <xref:System.Windows.SizeToContent.Height?displayProperty=nameWithType>/<xref:System.Windows.SizeToContent.WidthAndHeight?displayProperty=nameWithType>
  
4. <xref:System.Windows.FrameworkElement.Height%2A?displayProperty=nameWithType>  
  
 **幅のプロパティ:**  
  
1. <xref:System.Windows.FrameworkElement.MinWidth%2A?displayProperty=nameWithType>
  
2. <xref:System.Windows.FrameworkElement.MaxWidth%2A?displayProperty=nameWithType>
  
3. <xref:System.Windows.SizeToContent.Width?displayProperty=nameWithType>/<xref:System.Windows.SizeToContent.WidthAndHeight?displayProperty=nameWithType>
  
4. <xref:System.Windows.FrameworkElement.Width%2A?displayProperty=nameWithType>  
  
 優先順位は、ウィンドウが最大化されたときのサイズを決定することもできます。これは、<xref:System.Windows.Window.WindowState%2A> プロパティを使用して管理されます。  
  
<a name="WindowState"></a>   
## <a name="window-state"></a>ウィンドウの状態  
 サイズを変更できるウィンドウには、有効期間中、通常、最小化、最大化の 3 つの状態があります。 *通常*の状態のウィンドウは、ウィンドウの既定の状態です。 この状態のウィンドウは、サイズ変更グリップ、またはサイズ変更可能な場合は境界線を使用して、ユーザーが移動したりサイズ変更したりできます。  
  
 <xref:System.Windows.Window.ShowInTaskbar%2A> が `true`に設定されている場合、*最小化*された状態のウィンドウは、タスクバーボタンに折りたたまれます。それ以外の場合は、可能な限り小さいサイズに縮小され、デスクトップの左下隅に再配置されます。 最小化されたウィンドウはいずれの種類も、境界線またはサイズ変更グリップを使用してサイズ変更できません。ただし、タスク バーに表示されていない最小化されたウィンドウはデスクトップの任意の場所にドラッグできます。  
  
 最大化され*た状態の*ウィンドウは、最大サイズに拡張されます。これは、<xref:System.Windows.FrameworkElement.MaxWidth%2A>、<xref:System.Windows.FrameworkElement.MaxHeight%2A>、および <xref:System.Windows.Window.SizeToContent%2A> プロパティによって決まります。 最小化されたウィンドウと同様、最大化されたウィンドウは、サイズ変更グリップを使用したり、境界線をドラッグしたりすることによってサイズ変更できません。  
  
> [!NOTE]
> ウィンドウの <xref:System.Windows.Window.Top%2A>、<xref:System.Windows.Window.Left%2A>、<xref:System.Windows.FrameworkElement.Width%2A>、および <xref:System.Windows.FrameworkElement.Height%2A> の各プロパティの値は、ウィンドウが現在最大化または最小化されている場合でも、常に通常の状態の値を表します。  
  
 ウィンドウの状態を構成するには、その <xref:System.Windows.Window.WindowState%2A> プロパティを設定します。次の <xref:System.Windows.WindowState> 列挙値のいずれかを指定できます。  
  
- <xref:System.Windows.WindowState.Normal> (規定値)  
  
- <xref:System.Windows.WindowState.Maximized>  
  
- <xref:System.Windows.WindowState.Minimized>  
  
 開くときに最大化されて表示されるウィンドウを作成する方法を、次の例に示します。  
  
 [!code-xaml[WindowsOverviewSnippets#WindowStateWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/WindowStateWindow.xaml#windowstatewindowmarkup1)]  
  
 一般に、ウィンドウの初期状態を構成するには <xref:System.Windows.Window.WindowState%2A> を設定する必要があります。 サイズ変更可能なウィンドウが表示されると、ユーザーはウィンドウのタイトル バーにある最小化ボタン、最大化ボタン、および元に戻すボタンを使用して、ウィンドウの状態を変更できます。  
  
<a name="WindowAppearance"></a>   
## <a name="window-appearance"></a>ウィンドウの外観  
 ウィンドウのクライアント領域の外観を変更するために、ボタン、ラベル、テキスト ボックスなど、ウィンドウ固有のコンテンツを追加します。 非クライアント領域を構成するために、<xref:System.Windows.Window> にはいくつかのプロパティが用意され <xref:System.Windows.Window.Title%2A> ています。これには、ウィンドウのアイコンを設定したり、タイトルを設定したりするための <xref:System.Windows.Window.Icon%2A> が含まれます。  
  
 また、ウィンドウのサイズ変更モード、ウィンドウ スタイル、デスクトップのタスク バーにボタンとして表示するかどうかを構成して、非クライアント領域の境界線の外観と動作も変更できます。  

<a name="Resize_Mode"></a>   
### <a name="resize-mode"></a>サイズ変更モード  
 <xref:System.Windows.Window.WindowStyle%2A> プロパティに応じて、ユーザーがウィンドウのサイズを変更する方法 (および場合) を制御できます。 ウィンドウスタイルの選択は、ユーザーがマウスで境界線をドラッグしてウィンドウのサイズを変更できるかどうか、非クライアント領域に**最小化**、**最大化**、および**サイズ変更**の各ボタンが表示されるかどうか、また、有効になっているかどうかにかかわらず、表示されるかどうかに影響します。  
  
 <xref:System.Windows.Window.ResizeMode%2A> プロパティを設定して、ウィンドウのサイズを変更する方法を構成できます。これは、次の <xref:System.Windows.ResizeMode> 列挙値のいずれかになります。  
  
- <xref:System.Windows.ResizeMode.NoResize>  
  
- <xref:System.Windows.ResizeMode.CanMinimize>  
  
- <xref:System.Windows.ResizeMode.CanResize> (規定値)  
  
- <xref:System.Windows.ResizeMode.CanResizeWithGrip>  
  
 <xref:System.Windows.Window.WindowStyle%2A>と同様に、ウィンドウのサイズ変更モードは、その有効期間中に変更される可能性はほとんどありません。つまり、ほとんどの場合、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] マークアップから設定されます。  
  
 [!code-xaml[WindowsOverviewSnippets#ResizeModeWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/ResizeModeWindow.xaml#resizemodewindowmarkup1)]  
  
 <xref:System.Windows.Window.WindowState%2A> プロパティを調べることで、ウィンドウが最大化、最小化、または復元されているかどうかを検出できます。  
  
<a name="Window_Style"></a>   
### <a name="window-style"></a>ウィンドウ スタイル  
 ウィンドウの非クライアント領域から公開される境界線は、多くのアプリケーションに適しています。 ただし、ウィンドウの種類によって、異なる種類の境界線が必要な状況や、境界線がまったく必要ない状況があります。  
  
 ウィンドウで取得する境界線の種類を制御するには、その <xref:System.Windows.Window.WindowStyle%2A> プロパティを、<xref:System.Windows.WindowStyle> 列挙体の次のいずれかの値に設定します。  
  
- <xref:System.Windows.WindowStyle.None>  
  
- <xref:System.Windows.WindowStyle.SingleBorderWindow> (規定値)  
  
- <xref:System.Windows.WindowStyle.ThreeDBorderWindow>  
  
- <xref:System.Windows.WindowStyle.ToolWindow>  
  
 これらのウィンドウスタイルの効果を次の図に示します。  
  
 ![ウィンドウの境界線スタイルの図。](./media/wpf-windows-overview/window-border-styles.png)  
  
 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] マークアップまたはコードのいずれかを使用して <xref:System.Windows.Window.WindowStyle%2A> を設定できます。ウィンドウの有効期間中は変更される可能性が低いため、ほとんどの場合、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] マークアップを使用して構成します。  
  
 [!code-xaml[WindowsOverviewSnippets#WindowStyleWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/WindowStyleWindow.xaml#windowstylewindowmarkup1)]  
  
#### <a name="non-rectangular-window-style"></a>四角形以外のウィンドウ スタイル  
 <xref:System.Windows.Window.WindowStyle%2A> に許可されている罫線のスタイルでは不十分な場合もあります。 たとえば、Microsoft Windows Media Player で使用するような、四角形以外の境界線を持つアプリケーションを作成することができます。  
  
 たとえば、次の図に示すように、音声バブルウィンドウについて考えてみます。  
  
 !["ドラッグアンドドロップ" という音声バブルウィンドウ。](./media/wpf-windows-overview/non-rectangular-window-figure.png)  
  
 この種類のウィンドウを作成するには、[<xref:System.Windows.Window.WindowStyle%2A>] プロパティを <xref:System.Windows.WindowStyle.None>に設定し、透明度に <xref:System.Windows.Window> の特別なサポートを使用します。  
  
 [!code-xaml[WindowsOverviewSnippets#TransparentWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/TransparentWindow.xaml#transparentwindowmarkup1)]  
  
 値をこの組み合わせで使用し、ウィンドウが完全に透明にレンダリングされるように設定します。 この状態では、ウィンドウの非クライアント領域の表示要素 (閉じるメニュー、最小化ボタン、最大化ボタン、元に戻すボタンなど) は使用できません。 したがって、独自の表示要素を用意する必要があります。  
  
<a name="Task_Bar_Presence"></a>   
### <a name="task-bar-presence"></a>タスク バーのプレゼンス  

ウィンドウの既定の外観には、次の図に示すようなタスクバーボタンが表示されます。

 ![タスクバーボタンのあるウィンドウを示すスクリーンショット。](./media/wpf-windows-overview/window-taskbar-button.png)  
  
 ウィンドウの種類によっては、メッセージボックスやダイアログボックスなどのタスクバーボタンが表示されない場合があります (「[ダイアログボックスの概要](dialog-boxes-overview.md)」を参照してください)。 <xref:System.Windows.Window.ShowInTaskbar%2A> プロパティ (既定では`true`) を設定して、ウィンドウのタスクバーボタンを表示するかどうかを制御できます。  
  
 [!code-xaml[WindowsOverviewSnippets#ShowInTaskbarWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/ShowInTaskbarWindow.xaml#showintaskbarwindowmarkup1)]  
  
<a name="SecurityConsiderations"></a>   
## <a name="security-considerations"></a>セキュリティに関する考慮事項  
 <xref:System.Windows.Window> には、`UnmanagedCode` セキュリティ権限をインスタンス化する必要があります。 ローカル コンピューターにインストールされ、ローカル コンピューターから起動されるアプリケーションの場合は、アプリケーションに付与されるアクセス許可セットの範囲内になります。  
  
 ただし、これは、ClickOnce を使用してインターネットまたはローカルイントラネットゾーンから起動されるアプリケーションに付与されるアクセス許可のセットの範囲外です。 その結果、ユーザーには ClickOnce のセキュリティ警告が表示され、アプリケーションのアクセス許可セットを完全信頼に昇格させる必要があります。  
  
 また、Xbap では既定でウィンドウまたはダイアログボックスを表示できません。 スタンドアロンアプリケーションのセキュリティに関する考慮事項については、「 [WPF のセキュリティ方針-プラットフォームのセキュリティ](../wpf-security-strategy-platform-security.md)」を参照してください。  
  
<a name="Other_Types_of_Windows"></a>   
## <a name="other-types-of-windows"></a>その他の種類のウィンドウ  
 <xref:System.Windows.Navigation.NavigationWindow> は、誘導可能なコンテンツをホストするように設計されたウィンドウです。 詳細については、「[ナビゲーションの概要](navigation-overview.md)」を参照してください。  
  
 ダイアログ ボックスは、ユーザーから情報を収集して機能を完了するためによく使用されるウィンドウです。 たとえば、ユーザーがファイルを開こうとした場合、通常、 **[ファイルを開く]** ダイアログボックスは、ユーザーからファイル名を取得するためにアプリケーションによって表示されます。 詳細については、「[ダイアログ ボックスの概要](dialog-boxes-overview.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Window>
- <xref:System.Windows.MessageBox>
- <xref:System.Windows.Navigation.NavigationWindow>
- <xref:System.Windows.Application>
- [ダイアログ ボックスの概要](dialog-boxes-overview.md)
- [WPF アプリケーションのビルド](building-a-wpf-application-wpf.md)
