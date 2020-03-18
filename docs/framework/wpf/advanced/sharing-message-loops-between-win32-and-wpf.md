---
title: Win32 と WPF 間でのメッセージ ループの共有
titleSuffix: ''
ms.date: 03/30/2017
helpviewer_keywords:
- Win32 code [WPF], sharing message loops
- message loops [WPF]
- sharing message loops [WPF]
- interoperability [WPF], Win32
ms.assetid: 39ee888c-e5ec-41c8-b11f-7b851a554442
ms.openlocfilehash: e1b96284d69645876d3e383beb03a2cc540d8b7b
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76731711"
---
# <a name="sharing-message-loops-between-win32-and-wpf"></a>Win32 と WPF 間でのメッセージ ループの共有
このトピックでは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]との相互運用のためのメッセージループを実装する方法について説明します。そのためには、<xref:System.Windows.Threading.Dispatcher> の既存のメッセージループの露出を使用するか、相互運用コードの Win32 側で別のメッセージループを作成します。  
  
## <a name="componentdispatcher-and-the-message-loop"></a>ComponentDispatcher と Message ループ  
 相互運用とキーボードイベントサポートの通常のシナリオは、<xref:System.Windows.Interop.IKeyboardInputSink>を実装するか、既に <xref:System.Windows.Interop.IKeyboardInputSink>を実装しているクラス (<xref:System.Windows.Interop.HwndSource> や <xref:System.Windows.Interop.HwndHost>など) からサブクラス化することです。 ただし、相互運用の境界を越えてメッセージを送受信するときに発生する可能性のあるメッセージループのニーズには、キーボードシンクのサポートでは対処できません。 アプリケーションメッセージループアーキテクチャを形式化するために、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] には、メッセージループに従う単純なプロトコルを定義する <xref:System.Windows.Interop.ComponentDispatcher> クラスが用意されています。  
  
 <xref:System.Windows.Interop.ComponentDispatcher> は、複数のメンバーを公開する静的クラスです。 各メソッドのスコープは、呼び出し元のスレッドに暗黙的に関連付けられます。 メッセージループは、次のセクションで定義されているように、これらの Api の一部を重要なタイミングで呼び出す必要があります。  
  
 <xref:System.Windows.Interop.ComponentDispatcher> には、他のコンポーネント (キーボードシンクなど) がリッスンできるイベントが用意されています。 <xref:System.Windows.Threading.Dispatcher> クラスは、適切な順序ですべての適切な <xref:System.Windows.Interop.ComponentDispatcher> メソッドを呼び出します。 独自のメッセージループを実装する場合は、コードが同様の方法で <xref:System.Windows.Interop.ComponentDispatcher> メソッドを呼び出す必要があります。  
  
 スレッドで <xref:System.Windows.Interop.ComponentDispatcher> メソッドを呼び出すと、そのスレッドに登録されたイベントハンドラーのみが呼び出されます。  
  
## <a name="writing-message-loops"></a>メッセージループの作成  
 独自のメッセージループを記述する場合に使用する <xref:System.Windows.Interop.ComponentDispatcher> メンバーのチェックリストを次に示します。  
  
- <xref:System.Windows.Interop.ComponentDispatcher.PushModal%2A>: スレッドがモーダルであることを示すために、メッセージループはこれを呼び出す必要があります。  
  
- <xref:System.Windows.Interop.ComponentDispatcher.PopModal%2A>: スレッドが非モーダルに戻されたことを示すために、メッセージループはこれを呼び出す必要があります。  
  
- <xref:System.Windows.Interop.ComponentDispatcher.RaiseIdle%2A>: メッセージループは、<xref:System.Windows.Interop.ComponentDispatcher> が <xref:System.Windows.Interop.ComponentDispatcher.ThreadIdle> イベントを発生させることを示すために、これを呼び出す必要があります。 <xref:System.Windows.Interop.ComponentDispatcher.IsThreadModal%2A> が `true`場合、<xref:System.Windows.Interop.ComponentDispatcher> は <xref:System.Windows.Interop.ComponentDispatcher.ThreadIdle> を発生させませんが、モーダル状態の間は <xref:System.Windows.Interop.ComponentDispatcher.RaiseIdle%2A> が応答できない場合でも、メッセージループが <xref:System.Windows.Interop.ComponentDispatcher> の呼び出しを選択する可能性があります。  
  
- <xref:System.Windows.Interop.ComponentDispatcher.RaiseThreadMessage%2A>: 新しいメッセージが使用可能であることを示すために、メッセージループはこれを呼び出す必要があります。 戻り値は、<xref:System.Windows.Interop.ComponentDispatcher> イベントのリスナーがメッセージを処理したかどうかを示します。 <xref:System.Windows.Interop.ComponentDispatcher.RaiseThreadMessage%2A> が `true` (処理済み) を返す場合、ディスパッチャーはメッセージに対してそれ以上の処理を行う必要はありません。 戻り値が `false`場合、ディスパッチャーは Win32 関数 `TranslateMessage`を呼び出す必要があります。その後 `DispatchMessage`を呼び出します。  
  
## <a name="using-componentdispatcher-and-existing-message-handling"></a>ComponentDispatcher と既存のメッセージ処理の使用  
 固有の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] メッセージループに依存している場合に使用する <xref:System.Windows.Interop.ComponentDispatcher> メンバーのチェックリストを次に示します。  
  
- <xref:System.Windows.Interop.ComponentDispatcher.IsThreadModal%2A>: アプリケーションがモーダルになったかどうかを返します (モーダルメッセージループがプッシュされたなど)。 クラスはメッセージループからの <xref:System.Windows.Interop.ComponentDispatcher.PushModal%2A> と <xref:System.Windows.Interop.ComponentDispatcher.PopModal%2A> の呼び出しの数を保持するため、<xref:System.Windows.Interop.ComponentDispatcher> はこの状態を追跡できます。  
  
- <xref:System.Windows.Interop.ComponentDispatcher.ThreadFilterMessage> イベントと <xref:System.Windows.Interop.ComponentDispatcher.ThreadPreprocessMessage> イベントは、デリゲート呼び出しの標準規則に従います。 デリゲートは、指定されていない順序で呼び出されます。また、最初のデリゲートがメッセージを処理済みとしてマークしている場合でも、すべてのデリゲートが呼び出されます。  
  
- <xref:System.Windows.Interop.ComponentDispatcher.ThreadIdle>: アイドル処理を実行するための適切かつ効率的な時間を示します (スレッドに対して保留中のメッセージは他にありません)。 スレッドがモーダルの場合、<xref:System.Windows.Interop.ComponentDispatcher.ThreadIdle> は発生しません。  
  
- <xref:System.Windows.Interop.ComponentDispatcher.ThreadFilterMessage>: メッセージポンプによって処理されるすべてのメッセージに対して発生します。  
  
- <xref:System.Windows.Interop.ComponentDispatcher.ThreadPreprocessMessage>: <xref:System.Windows.Interop.ComponentDispatcher.ThreadFilterMessage>中に処理されなかったすべてのメッセージに対して発生します。  
  
 <xref:System.Windows.Interop.ComponentDispatcher.ThreadFilterMessage> イベントまたは <xref:System.Windows.Interop.ComponentDispatcher.ThreadPreprocessMessage> イベントの後に、イベントデータで参照によって渡された `handled` パラメーターが `true`場合、メッセージは処理されたと見なされます。 `handled` が `true`場合、イベントハンドラーはメッセージを無視する必要があります。これは、別のハンドラーが最初にメッセージを処理したことを意味します。 両方のイベントに対するイベントハンドラーによって、メッセージが変更されることがあります。 ディスパッチャーは変更されたメッセージをディスパッチする必要があり、変更されていない元のメッセージはディスパッチしません。 <xref:System.Windows.Interop.ComponentDispatcher.ThreadPreprocessMessage> はすべてのリスナーに配信されますが、アーキテクチャの目的は、メッセージへの応答でコードを呼び出す必要のある HWND を含むトップレベルウィンドウだけです。  
  
## <a name="how-hwndsource-treats-componentdispatcher-events"></a>System.windows.interop.hwndsource> が ComponentDispatcher イベントを扱う方法  
 <xref:System.Windows.Interop.HwndSource> がトップレベルウィンドウ (親 HWND なし) の場合は、<xref:System.Windows.Interop.ComponentDispatcher>に登録されます。 <xref:System.Windows.Interop.ComponentDispatcher.ThreadPreprocessMessage> が発生し、メッセージが <xref:System.Windows.Interop.HwndSource> または子ウィンドウを対象としている場合、<xref:System.Windows.Interop.HwndSource> は <xref:System.Windows.Interop.HwndSource.System%23Windows%23Interop%23IKeyboardInputSink%23TranslateAccelerator%2A>、<xref:System.Windows.Interop.IKeyboardInputSink.TranslateChar%2A>、<xref:System.Windows.Interop.IKeyboardInputSink.OnMnemonic%2A> キーボードシンクシーケンスを呼び出します。  
  
 <xref:System.Windows.Interop.HwndSource> がトップレベルウィンドウ (親 HWND を持つ) でない場合、処理は行われません。 最上位レベルのウィンドウのみが処理を実行することが想定されており、相互運用のシナリオの一部として、キーボードシンクサポートがある最上位レベルのウィンドウであることが想定されています。  
  
 適切なキーボードシンクメソッドが最初に呼び出されずに <xref:System.Windows.Interop.HwndSource> の <xref:System.Windows.Interop.HwndHost.WndProc%2A> が呼び出されると、アプリケーションは <xref:System.Windows.UIElement.KeyDown>などの上位レベルのキーボードイベントを受け取ります。 ただし、キーボードシンクメソッドは呼び出されません。これにより、アクセスキーのサポートなどの望ましくないキーボード入力モデル機能を回避できます。 これは、メッセージループが <xref:System.Windows.Interop.ComponentDispatcher>の関連スレッドに適切に通知しなかったか、親 HWND が適切なキーボードシンク応答を呼び出していなかったことが原因で発生する可能性があります。  
  
 <xref:System.Windows.Interop.HwndSource.AddHook%2A> メソッドを使用してそのメッセージにフックを追加した場合、キーボードシンクに送信されるメッセージが HWND に送られないことがあります。 メッセージがメッセージポンプレベルで直接処理され、`DispatchMessage` 関数に送信されていない可能性があります。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Interop.ComponentDispatcher>
- <xref:System.Windows.Interop.IKeyboardInputSink>
- [WPF と Win32 の相互運用性](wpf-and-win32-interoperation.md)
- [スレッド モデル](threading-model.md)
- [入力の概要](input-overview.md)
