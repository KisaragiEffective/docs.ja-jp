---
title: WPF での Win32 コントロールのホスト
titleSuffix: ''
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- hosting Win32 control in WPF [WPF]
- Win32 code [WPF], WPF interoperation
ms.assetid: a676b1eb-fc55-4355-93ab-df840c41cea0
ms.openlocfilehash: eb497a88c119dece85d61d6a32e7b86fb03b44b5
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744935"
---
# <a name="walkthrough-host-a-win32-control-in-wpf"></a>チュートリアル: WPF での Win32 コントロールのホスト
Windows Presentation Foundation (WPF) は、アプリケーションを作成するための豊富な環境を提供します。 ただし、Win32 コードに多大な投資をしている場合は、完全に書き直すのではなく、少なくともそのコードの一部を WPF アプリケーションで再利用する方が効果的な場合があります。 Wpf は、WPF ページで Win32 ウィンドウをホストするための簡単なメカニズムを提供します。  
  
 このトピックでは、Win32 のリストボックスコントロールをホストする、 [WPF の ListBox コントロール](https://github.com/Microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/WPFHostingWin32Control)をホストするアプリケーションについて説明します。 この一般的な手順は、Win32 ウィンドウをホストするように拡張できます。  

<a name="requirements"></a>   
## <a name="requirements"></a>必要条件  
 このトピックでは、WPF と Windows API プログラミングの基本的な知識があることを前提としています。 WPF プログラミングの基本的な概要については、「[はじめに](../getting-started/index.md)」を参照してください。 Windows API プログラミングの概要については、「チャールズ Petzold 著) による特定の*プログラミングウィンドウ*」に記載されている、多くの書籍を参照してください。  
  
 このトピックに付属するサンプルはにC#実装されているため、プラットフォーム呼び出しサービス (PInvoke) を使用して Windows API にアクセスします。 PInvoke の知識は役に立ちますが、必須ではありません。  
  
> [!NOTE]
> このトピックには、関連付けられているサンプルのコード例が多数含まれています。 しかし、読みやすくするため、完全なサンプル コードは含まれていません。 完全なコードを取得または表示する[には、WPF の ListBox コントロールのホストに関するサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/WPFHostingWin32Control)を参照してください。  
  
<a name="basic_procedure"></a>   
## <a name="the-basic-procedure"></a>基本手順  
 このセクションでは、WPF ページで Win32 ウィンドウをホストするための基本的な手順について説明します。 残りのセクションでは、各手順の詳細について説明します。  
  
 基本的なホスティング手順は次のとおりです。  
  
1. ウィンドウをホストする WPF ページを実装します。 1つの方法として、<xref:System.Windows.Controls.Border> 要素を作成して、ホストされているウィンドウのページのセクションを予約します。  
  
2. <xref:System.Windows.Interop.HwndHost>から継承するコントロールをホストするクラスを実装します。  
  
3. そのクラスで、<xref:System.Windows.Interop.HwndHost> クラスのメンバー <xref:System.Windows.Interop.HwndHost.BuildWindowCore%2A>をオーバーライドします。  
  
4. ホストされているウィンドウを、WPF ページを含むウィンドウの子として作成します。 従来の WPF プログラミングでは、明示的に使用する必要はありませんが、ホストページはハンドル (HWND) を持つウィンドウです。 <xref:System.Windows.Interop.HwndHost.BuildWindowCore%2A> メソッドの `hwndParent` パラメーターを使用して、ページ HWND を受け取ります。 ホストされているウィンドウは、この HWND の子として作成される必要があります。  
  
5. ホストウィンドウを作成したら、ホストされているウィンドウの HWND を返します。 1つ以上の Win32 コントロールをホストする場合は、通常、ホストウィンドウを HWND の子として作成し、そのホストウィンドウの子コントロールを作成します。 ホストウィンドウでコントロールをラップすると、WPF ページでコントロールからの通知を簡単に受信できるようになります。これは、HWND 境界を越えた通知に関する特定の Win32 の問題を処理します。  
  
6. 子コントロールからの通知など、ホストウィンドウに送信される選択されたメッセージを処理します。 2 つの方法があります。  
  
    - ホスティングクラスでメッセージを処理する場合は、<xref:System.Windows.Interop.HwndHost> クラスの <xref:System.Windows.Interop.HwndHost.WndProc%2A> メソッドをオーバーライドします。  
  
    - WPF でメッセージを処理する場合は、分離コードで <xref:System.Windows.Interop.HwndHost> クラスの <xref:System.Windows.Interop.HwndHost.MessageHook> イベントを処理します。 このイベントは、ホストされているウィンドウが受信するすべてのメッセージに対して発生します。 このオプションを選択した場合でも、<xref:System.Windows.Interop.HwndHost.WndProc%2A>をオーバーライドする必要がありますが、最低限の実装のみが必要です。  
  
7. <xref:System.Windows.Interop.HwndHost>の <xref:System.Windows.Interop.HwndHost.DestroyWindowCore%2A> および <xref:System.Windows.Interop.HwndHost.WndProc%2A> メソッドをオーバーライドします。 <xref:System.Windows.Interop.HwndHost> コントラクトを満たすには、これらのメソッドをオーバーライドする必要がありますが、必要なのは最小限の実装だけです。  
  
8. 分離コードファイルで、コントロールホストクラスのインスタンスを作成し、ウィンドウをホストするための <xref:System.Windows.Controls.Border> 要素の子にします。  
  
9. Microsoft Windows メッセージを送信し、その子ウィンドウからのメッセージ (コントロールによって送信された通知など) を処理することによって、ホストされたウィンドウと通信します。  
  
<a name="page_layout"></a>   
## <a name="implement-the-page-layout"></a>ページレイアウトを実装する  
 ListBox コントロールをホストする WPF ページのレイアウトは、2つの領域で構成されます。 ページの左側には、Win32 コントロールを操作するための [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] を提供するいくつかの WPF コントロールがホストされています。 ページの右上隅には、ホストされた ListBox コントロールの四角形の領域があります。  
  
 このレイアウトを実装するコードは非常に単純です。 ルート要素は、2つの子要素を持つ <xref:System.Windows.Controls.DockPanel> です。 1つ目は、ListBox コントロールをホストする <xref:System.Windows.Controls.Border> 要素です。 ページの右上隅に 200 x 200 の四角形があります。 2つ目は、情報を表示し、公開されている相互運用プロパティを設定することによって ListBox コントロールを操作できる、一連の WPF コントロールを含む <xref:System.Windows.Controls.StackPanel> 要素です。 <xref:System.Windows.Controls.StackPanel>の子である各要素のリファレンス資料を参照してください。これらの要素の詳細については、以下のコード例に記載されていますが、ここでは説明しません (基本的な相互運用モデルでは、これらの要素については説明しません)。  
  
 [!code-xaml[WPFHostingWin32Control#WPFUI](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFHostingWin32Control/CSharp/Page1.xaml#wpfui)]  
  
<a name="host_class"></a>   
## <a name="implement-a-class-to-host-the-microsoft-win32-control"></a>Microsoft Win32 コントロールをホストするクラスを実装する  
 このサンプルの中核となるのは、ControlHost.cs コントロールを実際にホストするクラスです。 <xref:System.Windows.Interop.HwndHost> から継承します。 コンストラクターは、2つのパラメーター (height と width) を受け取ります。このパラメーターは、ListBox コントロールをホストする <xref:System.Windows.Controls.Border> 要素の高さと幅に対応しています。 これらの値は後で使用して、コントロールのサイズが <xref:System.Windows.Controls.Border> 要素と一致するようにします。  
  
 [!code-csharp[WPFHostingWin32Control#ControlHostClass](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFHostingWin32Control/CSharp/ControlHost.cs#controlhostclass)]
 [!code-vb[WPFHostingWin32Control#ControlHostClass](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFHostingWin32Control/VisualBasic/ControlHost.vb#controlhostclass)]  
  
 定数のセットもあります。 これらの定数は、ほとんどの場合 Winuser. h から取得され、Win32 関数を呼び出すときに従来の名前を使用できます。  
  
 [!code-csharp[WPFHostingWin32Control#ControlHostConstants](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFHostingWin32Control/CSharp/ControlHost.cs#controlhostconstants)]
 [!code-vb[WPFHostingWin32Control#ControlHostConstants](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFHostingWin32Control/VisualBasic/ControlHost.vb#controlhostconstants)]  
  
<a name="buildwindowcore"></a>   
### <a name="override-buildwindowcore-to-create-the-microsoft-win32-window"></a>BuildWindowCore をオーバーライドして Microsoft Win32 ウィンドウを作成します  
 このメソッドをオーバーライドして、ページによってホストされる Win32 ウィンドウを作成し、ウィンドウとページ間の接続を確立します。 このサンプルには ListBox コントロールのホストが含まれているため、2つのウィンドウが作成されます。 1つ目は、WPF ページによって実際にホストされるウィンドウです。 ListBox コントロールは、そのウィンドウの子として作成されます。  
  
 この方法は、コントロールから通知を受信するプロセスを簡略化するためのものです。 <xref:System.Windows.Interop.HwndHost> クラスを使用すると、ホストしているウィンドウに送信されたメッセージを処理できます。 Win32 コントロールを直接ホストする場合は、コントロールの内部メッセージループに送信されたメッセージを受け取ります。 コントロールを表示してメッセージを送信することはできますが、コントロールから親ウィンドウに送信される通知は受け取りません。 これは特に、ユーザーがコントロールと対話するときに検出する方法がないことを意味します。 代わりに、ホストウィンドウを作成し、そのウィンドウの子にコントロールを設定します。 これにより、コントロールによって送信された通知を含め、ホストウィンドウのメッセージを処理できます。 便宜上、ホストウィンドウはコントロールの単純なラッパーよりもわずかであるため、パッケージは ListBox コントロールと呼ばれます。  
  
<a name="create_the_window_and_listbox"></a>   
#### <a name="create-the-host-window-and-listbox-control"></a>ホストウィンドウと ListBox コントロールの作成  
 PInvoke を使用して、ウィンドウクラスを作成して登録することで、コントロールのホストウィンドウを作成できます。 ただし、はるかに簡単な方法は、定義済みの "静的" ウィンドウクラスを使用してウィンドウを作成することです。 これにより、コントロールからの通知を受信するために必要なウィンドウプロシージャが提供され、最小限のコーディングが必要になります。  
  
 コントロールの HWND は、読み取り専用プロパティを通じて公開されます。これにより、ホストページは、このプロパティを使用して、コントロールにメッセージを送信できます。  
  
 [!code-csharp[WPFHostingWin32Control#IntPtrProperty](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFHostingWin32Control/CSharp/ControlHost.cs#intptrproperty)]
 [!code-vb[WPFHostingWin32Control#IntPtrProperty](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFHostingWin32Control/VisualBasic/ControlHost.vb#intptrproperty)]  
  
 ListBox コントロールは、ホストウィンドウの子として作成されます。 両方のウィンドウの高さと幅は、前に説明したように、コンストラクターに渡される値に設定されます。 これにより、ホストウィンドウとコントロールのサイズは、ページ上の予約領域と同じになります。  ウィンドウが作成されると、このサンプルはホストウィンドウの HWND を含む <xref:System.Runtime.InteropServices.HandleRef> オブジェクトを返します。  
  
 [!code-csharp[WPFHostingWin32Control#BuildWindowCore](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFHostingWin32Control/CSharp/ControlHost.cs#buildwindowcore)]
 [!code-vb[WPFHostingWin32Control#BuildWindowCore](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFHostingWin32Control/VisualBasic/ControlHost.vb#buildwindowcore)]  
  
 [!code-csharp[WPFHostingWin32Control#BuildWindowCoreHelper](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFHostingWin32Control/CSharp/ControlHost.cs#buildwindowcorehelper)]
 [!code-vb[WPFHostingWin32Control#BuildWindowCoreHelper](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFHostingWin32Control/VisualBasic/ControlHost.vb#buildwindowcorehelper)]  
  
<a name="destroywindow_wndproc"></a>   
### <a name="implement-destroywindow-and-wndproc"></a>DestroyWindow と WndProc を実装する  
 <xref:System.Windows.Interop.HwndHost.BuildWindowCore%2A>に加えて、<xref:System.Windows.Interop.HwndHost>の <xref:System.Windows.Interop.HwndHost.WndProc%2A> および <xref:System.Windows.Interop.HwndHost.DestroyWindowCore%2A> メソッドもオーバーライドする必要があります。 この例では、コントロールのメッセージは <xref:System.Windows.Interop.HwndHost.MessageHook> ハンドラーによって処理されるため、<xref:System.Windows.Interop.HwndHost.WndProc%2A> と <xref:System.Windows.Interop.HwndHost.DestroyWindowCore%2A> の実装は最小限に抑えられます。 <xref:System.Windows.Interop.HwndHost.WndProc%2A>の場合は、`handled` を `false` に設定して、メッセージが処理されなかったことを示し、0を返します。 <xref:System.Windows.Interop.HwndHost.DestroyWindowCore%2A>には、単にウィンドウを破棄します。  
  
 [!code-csharp[WPFHostingWin32Control#WndProcDestroy](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFHostingWin32Control/CSharp/ControlHost.cs#wndprocdestroy)]
 [!code-vb[WPFHostingWin32Control#WndProcDestroy](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFHostingWin32Control/VisualBasic/ControlHost.vb#wndprocdestroy)]  
  
 [!code-csharp[WPFHostingWin32Control#WndProcDestroyHelper](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFHostingWin32Control/CSharp/ControlHost.cs#wndprocdestroyhelper)]
 [!code-vb[WPFHostingWin32Control#WndProcDestroyHelper](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFHostingWin32Control/VisualBasic/ControlHost.vb#wndprocdestroyhelper)]  
  
<a name="host_the_control"></a>   
## <a name="host-the-control-on-the-page"></a>ページ上のコントロールをホストする  
 ページ上のコントロールをホストするには、最初に `ControlHost` クラスの新しいインスタンスを作成します。 コントロールを含む境界要素の高さと幅 (`ControlHostElement`) を `ControlHost` コンストラクターに渡します。 これにより、ListBox のサイズが正しく設定されます。 次に、`ControlHost` オブジェクトをホスト <xref:System.Windows.Controls.Border>の <xref:System.Windows.Controls.Decorator.Child%2A> プロパティに割り当てることによって、ページ上のコントロールをホストします。  
  
 このサンプルでは、コントロールからメッセージを受信するために、`ControlHost` の <xref:System.Windows.Interop.HwndHost.MessageHook> イベントにハンドラーをアタッチします。 このイベントは、ホストされているウィンドウに送信されるすべてのメッセージに対して発生します。 この場合、コントロールからの通知など、実際の ListBox コントロールをラップするウィンドウに送信されるメッセージです。 このサンプルでは、SendMessage を呼び出して、コントロールから情報を取得し、その内容を変更します。 ページがコントロールと通信する方法の詳細については、次のセクションで説明します。  
  
> [!NOTE]
> SendMessage の PInvoke 宣言が2つあることに注意してください。 これが必要なのは、1つは `wParam` パラメーターを使用して文字列を渡し、もう1つは、それを使用して整数を渡すためです。 データが正しくマーシャリングされるようにするには、署名ごとに個別の宣言が必要です。  
  
 [!code-csharp[WPFHostingWin32Control#HostWindowClass](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFHostingWin32Control/CSharp/Page1.xaml.cs#hostwindowclass)]
 [!code-vb[WPFHostingWin32Control#HostWindowClass](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFHostingWin32Control/VisualBasic/Page1.xaml.vb#hostwindowclass)]  
  
 [!code-csharp[WPFHostingWin32Control#ControlMsgFilterSendMessage](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFHostingWin32Control/CSharp/Page1.xaml.cs#controlmsgfiltersendmessage)]
 [!code-vb[WPFHostingWin32Control#ControlMsgFilterSendMessage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFHostingWin32Control/VisualBasic/Page1.xaml.vb#controlmsgfiltersendmessage)]  
  
<a name="communication"></a>   
## <a name="implement-communication-between-the-control-and-the-page"></a>コントロールとページ間の通信を実装する  
 コントロールを操作するには、Windows メッセージを送信します。 コントロールは、ユーザーがホストウィンドウに通知を送信することによって対話するときに通知します。 [WPF での Win32 ListBox コントロールのホストサンプルに](https://github.com/Microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/WPFHostingWin32Control)含まれる UI には、そのしくみの例がいくつか用意されています。  
  
- リストに項目を追加します。  
  
- 選択した項目を一覧から削除します  
  
- 現在選択されている項目のテキストを表示します。  
  
- リスト内の項目の数を表示します。  
  
 ユーザーは、従来の Win32 アプリケーションの場合と同様に、リストボックス内の項目をクリックして選択することもできます。 表示されるデータは、ユーザーが項目を選択、追加、または追加することによってリストボックスの状態を変更するたびに更新されます。  
  
 項目を追加するには、リストボックスに[`LB_ADDSTRING` メッセージ](/windows/desktop/Controls/lb-addstring)を送信します。 項目を削除するには、 [`LB_GETCURSEL`](/windows/desktop/Controls/lb-getcursel)を送信して現在の選択範囲のインデックスを取得し、 [`LB_DELETESTRING`](/windows/desktop/Controls/lb-deletestring)して項目を削除します。 このサンプルは[`LB_GETCOUNT`](/windows/desktop/Controls/lb-getcount)も送信し、返された値を使用して、項目数を示す表示を更新します。 これらの[`SendMessage`](/windows/desktop/api/winuser/nf-winuser-sendmessage)のインスタンスはどちらも、前のセクションで説明した PInvoke 宣言のいずれかを使用します。  
  
 [!code-csharp[WPFHostingWin32Control#AppendDeleteText](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFHostingWin32Control/CSharp/Page1.xaml.cs#appenddeletetext)]
 [!code-vb[WPFHostingWin32Control#AppendDeleteText](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFHostingWin32Control/VisualBasic/Page1.xaml.vb#appenddeletetext)]  
  
 ユーザーが項目を選択するか、項目の選択内容を変更すると、コントロールは[`WM_COMMAND` メッセージ](/windows/desktop/menurc/wm-command)を送信することでホストウィンドウに通知します。これにより、ページの <xref:System.Windows.Interop.HwndHost.MessageHook> イベントが発生します。 ハンドラーは、ホストウィンドウのメインウィンドウプロシージャと同じ情報を受け取ります。 また、`handled`ブール値への参照も渡します。 `handled` を `true` に設定すると、メッセージを処理したことを示し、それ以上の処理は必要ありません。  
  
 [`WM_COMMAND`](/windows/desktop/menurc/wm-command)はさまざまな理由で送信されるため、通知 ID を調べて、処理するイベントであるかどうかを確認する必要があります。 ID は `wParam` パラメーターの上位ワードに含まれています。 このサンプルでは、ビットごとの演算子を使用して ID を抽出します。 ユーザーが選択内容を変更した場合は、ID が[`LBN_SELCHANGE`](/windows/desktop/Controls/lbn-selchange)されます。  
  
 [`LBN_SELCHANGE`](/windows/desktop/Controls/lbn-selchange)を受信すると、このサンプルでは、コントロールに[`LB_GETCURSEL` メッセージ](/windows/desktop/Controls/lb-getcursel)を送信することによって、選択した項目のインデックスを取得します。 テキストを取得するには、最初に <xref:System.Text.StringBuilder>を作成します。 次に、コントロールに[`LB_GETTEXT` メッセージ](/windows/desktop/Controls/lb-gettext)を送信します。 空の <xref:System.Text.StringBuilder> オブジェクトを `wParam` パラメーターとして渡します。 [`SendMessage`](/windows/desktop/api/winuser/nf-winuser-sendmessage)が返された場合、<xref:System.Text.StringBuilder> には選択した項目のテキストが含まれます。 この[`SendMessage`](/windows/desktop/api/winuser/nf-winuser-sendmessage)の使用には、もう1つの PInvoke 宣言が必要です。  
  
 最後に、`handled` を `true` に設定して、メッセージが処理されたことを示します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Interop.HwndHost>
- [WPF と Win32 の相互運用性](wpf-and-win32-interoperation.md)
- [チュートリアル: 初めての WPF デスクトップ アプリケーション](../getting-started/walkthrough-my-first-wpf-desktop-application.md)
