---
title: Windows フォームと WPF の相互運用機能の入力アーキテクチャ
titleSuffix: ''
ms.date: 03/30/2017
helpviewer_keywords:
- input architecture [WPF interoperability]
- messages [WPF]
- Windows Forms [WPF], interoperability with
- Windows Forms [WPF], WPF interoperation
- interoperability [WPF], Windows Forms
- modeless forms [WPF]
- ElementHost keyboard and messages [WPF]
- keyboard interoperation [WPF]
- WindowsFormsHost keyboard and messages [WPF]
- modeless dialog boxes [WPF]
ms.assetid: 0eb6f137-f088-4c5e-9e37-f96afd28f235
ms.openlocfilehash: 1c7027947d24c847ee298022344e02ce0bbff764
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794090"
---
# <a name="windows-forms-and-wpf-interoperability-input-architecture"></a>Windows フォームと WPF の相互運用性入力アーキテクチャ
[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] と Windows フォーム間の相互運用を行うには、両方のテクノロジに適切なキーボード入力処理が必要です。 このトピックでは、ハイブリッドアプリケーションでスムーズな相互運用を実現するために、これらのテクノロジがキーボードおよびメッセージ処理を実装する方法について説明します。  
  
 このトピックは次のサブセクションで構成されています。  
  
- モードレスフォームとダイアログボックス  
  
- WindowsFormsHost のキーボードとメッセージの処理  
  
- このキーボードとメッセージの処理  
  
## <a name="modeless-forms-and-dialog-boxes"></a>モードレスフォームとダイアログボックス  
 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素の <xref:System.Windows.Forms.Integration.WindowsFormsHost.EnableWindowsFormsInterop%2A> メソッドを呼び出して、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]ベースのアプリケーションからモードレスフォームまたはダイアログボックスを開きます。  
  
 <xref:System.Windows.Forms.Integration.ElementHost> コントロールの <xref:System.Windows.Forms.Integration.ElementHost.EnableModelessKeyboardInterop%2A> メソッドを呼び出して、Windows フォームベースのアプリケーションでモードレス [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ページを開きます。  
  
## <a name="windowsformshost-keyboard-and-message-processing"></a>WindowsFormsHost のキーボードとメッセージの処理  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]ベースのアプリケーションによってホストされる場合、Windows フォームキーボードとメッセージ処理は次のように構成されます。  
  
- <xref:System.Windows.Forms.Integration.WindowsFormsHost> クラスは、<xref:System.Windows.Interop.ComponentDispatcher> クラスによって実装される [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] メッセージループからメッセージを取得します。  
  
- <xref:System.Windows.Forms.Integration.WindowsFormsHost> クラスは、通常の Windows フォームキーボード処理が行われるように、サロゲート Windows フォームメッセージループを作成します。  
  
- <xref:System.Windows.Forms.Integration.WindowsFormsHost> クラスは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]でフォーカス管理を調整するための <xref:System.Windows.Interop.IKeyboardInputSink> インターフェイスを実装します。  
  
- <xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールは、自身を登録し、メッセージループを開始します。  
  
 以下のセクションでは、プロセスの各部分について詳しく説明します。  
  
### <a name="acquiring-messages-from-the-wpf-message-loop"></a>WPF メッセージループからのメッセージの取得  
 <xref:System.Windows.Interop.ComponentDispatcher> クラスは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]のメッセージループマネージャーを実装します。 <xref:System.Windows.Interop.ComponentDispatcher> クラスは、外部クライアントがメッセージを処理 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 前にフィルター処理できるようにするフックを提供します。  
  
 相互運用の実装は、<xref:System.Windows.Interop.ComponentDispatcher.ThreadFilterMessage?displayProperty=nameWithType> イベントを処理します。これにより、Windows フォームコントロールは、コントロール [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 前にメッセージを処理できるようになります。  
  
### <a name="surrogate-windows-forms-message-loop"></a>サロゲート Windows フォームメッセージループ  
 既定では、<xref:System.Windows.Forms.Application?displayProperty=nameWithType> クラスには、Windows フォームアプリケーションのプライマリメッセージループが含まれています。 相互運用中、Windows フォームメッセージループはメッセージを処理しません。 このため、このロジックを再現する必要があります。 <xref:System.Windows.Interop.ComponentDispatcher.ThreadFilterMessage?displayProperty=nameWithType> イベントのハンドラーは、次の手順を実行します。  
  
1. <xref:System.Windows.Forms.IMessageFilter> インターフェイスを使用してメッセージをフィルター処理します。  
  
2. <xref:System.Windows.Forms.Control.PreProcessMessage%2A?displayProperty=nameWithType> メソッドを呼び出します。  
  
3. 必要に応じて、メッセージを変換およびディスパッチします。  
  
4. 他のコントロールがメッセージを処理しない場合は、ホストコントロールにメッセージを渡します。  
  
### <a name="ikeyboardinputsink-implementation"></a>Iの Inputsink の実装  
 サロゲートメッセージループは、キーボード管理を処理します。 したがって、<xref:System.Windows.Interop.IKeyboardInputSink.TabInto%2A?displayProperty=nameWithType> メソッドは、<xref:System.Windows.Forms.Integration.WindowsFormsHost> クラスでの実装を必要とする唯一の <xref:System.Windows.Interop.IKeyboardInputSink> メンバーです。  
  
 既定では、<xref:System.Windows.Interop.HwndHost> クラスは <xref:System.Windows.Interop.HwndHost.System%23Windows%23Interop%23IKeyboardInputSink%23TabInto%2A> 実装の `false` を返します。 これにより、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールから Windows フォームコントロールにタブ移動できなくなります。  
  
 <xref:System.Windows.Interop.IKeyboardInputSink.TabInto%2A?displayProperty=nameWithType> メソッドの <xref:System.Windows.Forms.Integration.WindowsFormsHost> の実装では、次の手順を実行します。  
  
1. <xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールに含まれ、フォーカスを受け取ることができる、最初または最後の Windows フォームコントロールを検索します。 制御の選択は、トラバーサル情報によって異なります。  
  
2. コントロールにフォーカスを設定し、`true`を返します。  
  
3. コントロールがフォーカスを受け取ることができない場合は、`false`を返します。  
  
### <a name="windowsformshost-registration"></a>WindowsFormsHost の登録  
 <xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールのウィンドウハンドルが作成されると、<xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールは、メッセージループの存在を登録する内部静的メソッドを呼び出します。  
  
 登録時に、<xref:System.Windows.Forms.Integration.WindowsFormsHost> の制御によってメッセージループが調べられます。 メッセージループが開始されていない場合は、<xref:System.Windows.Interop.ComponentDispatcher.ThreadFilterMessage?displayProperty=nameWithType> イベントハンドラーが作成されます。 <xref:System.Windows.Interop.ComponentDispatcher.ThreadFilterMessage?displayProperty=nameWithType> イベントハンドラーがアタッチされている場合、メッセージループは実行されていると見なされます。  
  
 ウィンドウハンドルが破棄されると、<xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールが登録から削除されます。  
  
## <a name="elementhost-keyboard-and-message-processing"></a>このキーボードとメッセージの処理  
 Windows フォームアプリケーションによってホストされる場合、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] キーボードおよびメッセージ処理は次の要素で構成されます。  
  
- <xref:System.Windows.Interop.HwndSource>、<xref:System.Windows.Interop.IKeyboardInputSink>、および <xref:System.Windows.Interop.IKeyboardInputSite> インターフェイスの実装。  
  
- Tab キーと方向キー  
  
- コマンドキーとダイアログボックスのキー。  
  
- Windows フォームアクセラレータ処理。  
  
 以下のセクションでは、これらの部分について詳しく説明します。  
  
### <a name="interface-implementations"></a>インターフェイスの実装  
 Windows フォームでは、キーボードメッセージは、フォーカスのあるコントロールのウィンドウハンドルにルーティングされます。 <xref:System.Windows.Forms.Integration.ElementHost> コントロールでは、これらのメッセージはホストされる要素にルーティングされます。 これを実現するために、<xref:System.Windows.Forms.Integration.ElementHost> コントロールは <xref:System.Windows.Interop.HwndSource> インスタンスを提供します。 <xref:System.Windows.Forms.Integration.ElementHost> コントロールにフォーカスがある場合、<xref:System.Windows.Interop.HwndSource> インスタンスは [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] <xref:System.Windows.Input.InputManager> クラスで処理できるように、ほとんどのキーボード入力をルーティングします。  
  
 <xref:System.Windows.Interop.HwndSource> クラスは、<xref:System.Windows.Interop.IKeyboardInputSink> および <xref:System.Windows.Interop.IKeyboardInputSite> インターフェイスを実装します。  
  
 キーボードの相互運用は、ホストされている要素からフォーカスを移動する TAB キーと方向キー入力を処理するための <xref:System.Windows.Interop.IKeyboardInputSite.OnNoMoreTabStops%2A> メソッドの実装に依存します。  
  
### <a name="tabbing-and-arrow-keys"></a>Tab キーと方向キー  
 Windows フォーム選択ロジックは、タブと方向キーのナビゲーションを実装するための <xref:System.Windows.Interop.HwndSource.System%23Windows%23Interop%23IKeyboardInputSink%23TabInto%2A> および <xref:System.Windows.Interop.IKeyboardInputSite.OnNoMoreTabStops%2A> メソッドにマップされます。 <xref:System.Windows.Forms.Integration.ElementHost.Select%2A> メソッドをオーバーライドすると、このマッピングが行われます。  
  
### <a name="command-keys-and-dialog-box-keys"></a>コマンドキーとダイアログボックスのキー  
 コマンドキーとダイアログキーを処理する最初の機会を [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] にするために、Windows フォームコマンドのプリプロセスは <xref:System.Windows.Interop.IKeyboardInputSink.TranslateAccelerator%2A> メソッドに接続されています。 <xref:System.Windows.Forms.Control.ProcessCmdKey%2A?displayProperty=nameWithType> メソッドをオーバーライドすると、2つのテクノロジが接続されます。  
  
 <xref:System.Windows.Interop.IKeyboardInputSink.TranslateAccelerator%2A> メソッドを使用すると、ホストされる要素は、TAB、ENTER、ESC、方向キーなどのコマンドキーを含む、WM_KEYDOWN、WM_KEYUP、WM_SYSKEYDOWN、WM_SYSKEYUP などの任意のキーメッセージを処理できます。 キーメッセージが処理されない場合は、処理のために Windows フォームの先祖階層が送信されます。  
  
### <a name="accelerator-processing"></a>アクセラレータの処理  
 アクセラレータを正しく処理するには、Windows フォーム accelerator 処理が [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] <xref:System.Windows.Input.AccessKeyManager> クラスに接続されている必要があります。 また、すべての WM_CHAR メッセージは、ホストされている要素に正しくルーティングされる必要があります。  
  
 <xref:System.Windows.Interop.IKeyboardInputSink.TranslateChar%2A> メソッドの既定の <xref:System.Windows.Interop.HwndSource> 実装では `false`が返されるため、WM_CHAR メッセージは次のロジックを使用して処理されます。  
  
- <xref:System.Windows.Forms.Control.IsInputChar%2A?displayProperty=nameWithType> メソッドは、すべての WM_CHAR メッセージがホストされた要素に転送されるようにオーバーライドされます。  
  
- ALT キーが押されている場合、メッセージは WM_SYSCHAR になります。 Windows フォームは、<xref:System.Windows.Forms.Control.IsInputChar%2A> メソッドを使用してこのメッセージをプリプロセスしません。 したがって、<xref:System.Windows.Forms.Control.ProcessMnemonic%2A> メソッドは、登録されているアクセラレータの [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] <xref:System.Windows.Input.AccessKeyManager> に対してクエリを実行するためにオーバーライドされます。 登録されているアクセラレータが見つかった場合、<xref:System.Windows.Input.AccessKeyManager> はそれを処理します。  
  
- ALT キーが押されていない場合、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] <xref:System.Windows.Input.InputManager> クラスは未処理の入力を処理します。 入力がアクセラレータの場合は、<xref:System.Windows.Input.AccessKeyManager> によって処理されます。 <xref:System.Windows.Input.InputManager.PostProcessInput> イベントは、処理されなかった WM_CHAR メッセージに対して処理されます。  
  
 ユーザーが ALT キーを押すと、アクセラレータの視覚的な手掛かりがフォーム全体に表示されます。 この動作をサポートするために、アクティブフォーム上のすべての <xref:System.Windows.Forms.Integration.ElementHost> コントロールは、どのコントロールにフォーカスがあるかに関係なく、WM_SYSKEYDOWN メッセージを受信します。  
  
 メッセージは、アクティブフォーム内の <xref:System.Windows.Forms.Integration.ElementHost> コントロールにのみ送信されます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.Integration.WindowsFormsHost.EnableWindowsFormsInterop%2A>
- <xref:System.Windows.Forms.Integration.ElementHost.EnableModelessKeyboardInterop%2A>
- <xref:System.Windows.Forms.Integration.ElementHost>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [チュートリアル: WPF での Windows フォーム複合コントロールのホスト](walkthrough-hosting-a-windows-forms-composite-control-in-wpf.md)
- [チュートリアル: Windows フォームでの WPF 複合コントロールのホスト](walkthrough-hosting-a-wpf-composite-control-in-windows-forms.md)
- [WPF と Win32 の相互運用性](wpf-and-win32-interoperation.md)
