---
title: ハイブリッド アプリケーションのトラブルシューティング
ms.date: 03/30/2017
helpviewer_keywords:
- overlapping controls [WPF]
- Windows Forms [WPF], interoperability with
- Windows Forms [WPF], WPF interoperation
- interoperability [WPF], Windows Forms
- hybrid applications [WPF interoperability]
- message loops [WPF]
ms.assetid: f440c23f-fa5d-4d5a-852f-ba61150e6405
ms.openlocfilehash: 7af110b9b00b080bf40bc9ee4b85aa293940bbc3
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794261"
---
# <a name="troubleshooting-hybrid-applications"></a>ハイブリッド アプリケーションのトラブルシューティング
<a name="introduction"></a>このトピックでは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] と Windows フォームの両方のテクノロジを使用するハイブリッドアプリケーションを作成するときに発生する可能性がある一般的な問題をいくつか紹介します。  

<a name="overlapping_controls"></a>   
## <a name="overlapping-controls"></a>コントロールの重複  
 コントロールは、意図したとおりに重ならない場合があります。 Windows フォームは、コントロールごとに個別の HWND を使用します。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、ページ上のすべてのコンテンツに対して1つの HWND を使用します。 この実装の違いにより、予期しない重複動作が発生します。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] でホストされている Windows フォームコントロールは、常に [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツの上に表示されます。  
  
 <xref:System.Windows.Forms.Integration.ElementHost> コントロールでホストされている [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツは、<xref:System.Windows.Forms.Integration.ElementHost> コントロールの z オーダーに表示されます。 <xref:System.Windows.Forms.Integration.ElementHost> コントロールを重ねることはできますが、ホストされている [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツは結合または対話しません。  
  
<a name="child_property"></a>   
## <a name="child-property"></a>子プロパティ  
 <xref:System.Windows.Forms.Integration.WindowsFormsHost> クラスと <xref:System.Windows.Forms.Integration.ElementHost> クラスは、1つの子コントロールまたは要素だけをホストできます。 複数のコントロールまたは要素をホストするには、子コンテンツとしてコンテナーを使用する必要があります。 たとえば、Windows フォームボタンとチェックボックスコントロールを <xref:System.Windows.Forms.Panel?displayProperty=nameWithType> コントロールに追加し、パネルを <xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールの <xref:System.Windows.Forms.Integration.WindowsFormsHost.Child%2A> プロパティに割り当てることができます。 ただし、ボタンとチェックボックスのコントロールを、同じ <xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールに個別に追加することはできません。  
  
<a name="scaling"></a>   
## <a name="scaling"></a>スケーリング  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] と Windows フォームには、異なるスケーリングモデルがあります。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] スケーリング変換には、Windows フォームコントロールにとって意味があるものもあれば、そうでないものもあります。 たとえば、Windows フォームコントロールのスケーリングを0に設定した場合は機能しますが、同じコントロールを0以外の値にスケーリングしようとすると、コントロールのサイズは0のままになります。 詳細については、「 [WindowsFormsHost 要素のレイアウトに関する考慮事項](layout-considerations-for-the-windowsformshost-element.md)」を参照してください。  
  
<a name="adapter"></a>   
## <a name="adapter"></a>アダプタ  
 <xref:System.Windows.Forms.Integration.WindowsFormsHost> クラスと <xref:System.Windows.Forms.Integration.ElementHost> クラスを操作するときに混乱が生じる可能性があります。これは、非表示のコンテナーが含まれているためです。 <xref:System.Windows.Forms.Integration.WindowsFormsHost> クラスと <xref:System.Windows.Forms.Integration.ElementHost> クラスには、コンテンツをホストするために使用する*アダプター*と呼ばれる非表示のコンテナーがあります。 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素の場合、アダプターは <xref:System.Windows.Forms.ContainerControl?displayProperty=nameWithType> クラスから派生します。 <xref:System.Windows.Forms.Integration.ElementHost> コントロールの場合、アダプターは <xref:System.Windows.Controls.DockPanel> 要素から派生します。 他の相互運用のトピックでアダプターへの参照が表示されている場合は、このコンテナーについて説明します。  
  
<a name="nesting"></a>   
## <a name="nesting"></a>巣  
 <xref:System.Windows.Forms.Integration.ElementHost> コントロール内での <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素の入れ子はサポートされていません。 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素内の <xref:System.Windows.Forms.Integration.ElementHost> コントロールの入れ子もサポートされていません。  
  
<a name="focus"></a>   
## <a name="focus"></a>フォーカス  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] と Windows フォームではフォーカスの動作が異なります。これは、ハイブリッドアプリケーションでフォーカスの問題が発生する可能性があることを意味します。 たとえば、<xref:System.Windows.Forms.Integration.WindowsFormsHost> の要素の中にフォーカスがあり、ページを最小化して復元するか、モーダルダイアログボックスを表示すると、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素内のフォーカスが失われる可能性があります。 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素にはフォーカスがありますが、その中のコントロールはフォーカスを持つことはできません。  
  
 データの検証は、フォーカスによっても影響を受けます。 検証は <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素で機能しますが、<xref:System.Windows.Forms.Integration.WindowsFormsHost> の要素から tab キーを使用するか、2つの異なる <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素の間で tab キーを使用しても機能しません。  
  
<a name="property_mapping"></a>   
## <a name="property-mapping"></a>プロパティのマッピング  
 一部のプロパティマッピングでは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] と Windows フォームテクノロジ間の異なる実装をブリッジするために広範な解釈が必要です。 プロパティマッピングを使用すると、フォント、色、およびその他のプロパティの変更にコードを反応させることができます。 一般に、プロパティマッピングは、*プロパティ変更イベント*または*プロパティ*変更された呼び出しをリッスンし、子コントロールまたはそのアダプターで適切なプロパティを設定することによって機能します。 詳細については、「 [Windows フォームと WPF プロパティのマッピング](windows-forms-and-wpf-property-mapping.md)」を参照してください。  
  
<a name="layoutrelated_properties_on_hosted_content"></a>   
## <a name="layout-related-properties-on-hosted-content"></a>ホストされたコンテンツのレイアウト関連のプロパティ  
 <xref:System.Windows.Forms.Integration.WindowsFormsHost.Child%2A?displayProperty=nameWithType> または <xref:System.Windows.Forms.Integration.ElementHost.Child%2A?displayProperty=nameWithType> プロパティが割り当てられると、ホストされているコンテンツのいくつかのレイアウト関連のプロパティが自動的に設定されます。 これらのコンテンツプロパティを変更すると、予期しないレイアウト動作が発生する可能性があります。  
  
 ホストされたコンテンツは、<xref:System.Windows.Forms.Integration.WindowsFormsHost> と <xref:System.Windows.Forms.Integration.ElementHost> 親に合わせてドッキングされます。 この塗りつぶし動作を有効にするために、子プロパティを設定するときにいくつかのプロパティが設定されます。 次の表に、<xref:System.Windows.Forms.Integration.ElementHost> クラスと <xref:System.Windows.Forms.Integration.WindowsFormsHost> クラスによって設定されるコンテンツプロパティを示します。  
  
|ホストクラス|コンテンツのプロパティ|  
|----------------|------------------------|  
|<xref:System.Windows.Forms.Integration.ElementHost>|<xref:System.Windows.FrameworkElement.Height%2A><br /><br /> <xref:System.Windows.FrameworkElement.Width%2A><br /><br /> <xref:System.Windows.FrameworkElement.Margin%2A><br /><br /> <xref:System.Windows.FrameworkElement.VerticalAlignment%2A><br /><br /> <xref:System.Windows.FrameworkElement.HorizontalAlignment%2A>|  
|<xref:System.Windows.Forms.Integration.WindowsFormsHost>|<xref:System.Windows.Forms.Control.Margin%2A><br /><br /> <xref:System.Windows.Forms.Control.Dock%2A><br /><br /> <xref:System.Windows.Forms.Control.AutoSize%2A><br /><br /> <xref:System.Windows.Forms.Control.Location%2A><br /><br /> <xref:System.Windows.Forms.Control.MaximumSize%2A>|  
  
 これらのプロパティは、ホストされているコンテンツで直接設定しないでください。 詳細については、「 [WindowsFormsHost 要素のレイアウトに関する考慮事項](layout-considerations-for-the-windowsformshost-element.md)」を参照してください。  
  
<a name="navigation_applications"></a>   
## <a name="navigation-applications"></a>ナビゲーションアプリケーション  
 ナビゲーションアプリケーションは、ユーザー状態を維持できない場合があります。 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素は、ナビゲーションアプリケーションで使用されるときにコントロールを再作成します。 子コントロールの再作成は、ユーザーが <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素をホストしているページから移動し、その後に戻ると発生します。 ユーザーによって入力されたコンテンツはすべて失われます。  
  
<a name="message_loop_interoperation"></a>   
## <a name="message-loop-interoperation"></a>メッセージループの相互運用  
 Windows フォームメッセージループを使用する場合、メッセージが想定どおりに処理されない可能性があります。 <xref:System.Windows.Forms.Integration.WindowsFormsHost.EnableWindowsFormsInterop%2A> メソッドは、<xref:System.Windows.Forms.Integration.WindowsFormsHost> コンストラクターによって呼び出されます。 このメソッドは、メッセージフィルターを [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] メッセージループに追加します。 このフィルターは、<xref:System.Windows.Forms.Control?displayProperty=nameWithType> がメッセージのターゲットであり、メッセージを変換/ディスパッチする場合に、<xref:System.Windows.Forms.Control.PreProcessMessage%2A?displayProperty=nameWithType> メソッドを呼び出します。  
  
 <xref:System.Windows.Forms.Application.Run%2A?displayProperty=nameWithType>を持つ Windows フォームメッセージループに <xref:System.Windows.Window> を表示する場合、<xref:System.Windows.Forms.Integration.ElementHost.EnableModelessKeyboardInterop%2A> メソッドを呼び出さないと、何も入力できません。 <xref:System.Windows.Forms.Integration.ElementHost.EnableModelessKeyboardInterop%2A> メソッドは、<xref:System.Windows.Window> を受け取り、<xref:System.Windows.Forms.IMessageFilter?displayProperty=nameWithType>を追加します。これにより、キー関連のメッセージを [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] メッセージループに再ルーティングします。 詳細については、「 [Windows フォームと WPF の相互運用性入力アーキテクチャ](windows-forms-and-wpf-interoperability-input-architecture.md)」を参照してください。  
  
<a name="opacity_and_layering"></a>   
## <a name="opacity-and-layering"></a>不透明度とレイヤー化  
 <xref:System.Windows.Interop.HwndHost> クラスは、レイヤーをサポートしていません。 つまり、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素の <xref:System.Windows.UIElement.Opacity%2A> プロパティを設定しても効果はありません。 `true`に設定され <xref:System.Windows.Window.AllowsTransparency%2A> ている他の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ウィンドウでは、ブレンドは行われません。  
  
<a name="dispose"></a>   
## <a name="dispose"></a>Dispose  
 クラスを正しく破棄しないと、リソースがリークする可能性があります。 ハイブリッドアプリケーションでは、<xref:System.Windows.Forms.Integration.WindowsFormsHost> クラスと <xref:System.Windows.Forms.Integration.ElementHost> クラスが破棄されていること、またはリソースがリークしていることを確認します。 非モーダル <xref:System.Windows.Forms.Form> 親が閉じるときに <xref:System.Windows.Forms.Integration.ElementHost> コントロールを破棄 Windows フォーム。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションのシャットダウン時に <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素を破棄します。 Windows フォームメッセージループの <xref:System.Windows.Window> に <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素を表示することができます。 この場合、アプリケーションがシャットダウンしているという通知をコードで受信できないことがあります。  
  
<a name="enabling_visual_styles"></a>   
## <a name="enabling-visual-styles"></a>視覚スタイルを有効にする  
 Windows フォームコントロールの Microsoft Windows XP visual スタイルが有効になっていない可能性があります。 <xref:System.Windows.Forms.Application.EnableVisualStyles%2A?displayProperty=nameWithType> メソッドは、Windows フォームアプリケーションのテンプレートで呼び出されます。 このメソッドは既定では呼び出されませんが、Visual Studio を使用してプロジェクトを作成する場合、Comctl32.dll のバージョン6.0 が使用可能な場合は、Microsoft Windows XP visual スタイルのコントロールが表示されます。 スレッドでハンドルが作成される前に、<xref:System.Windows.Forms.Application.EnableVisualStyles%2A> メソッドを呼び出す必要があります。 詳細については、「[方法: ハイブリッドアプリケーションで Visual スタイルを有効](how-to-enable-visual-styles-in-a-hybrid-application.md)にする」を参照してください。  
  
<a name="licensed_controls"></a>   
## <a name="licensed-controls"></a>ライセンスされたコントロール  
 ライセンスされた Windows フォームコントロールが、ユーザーに対してメッセージボックスにライセンス情報を表示すると、ハイブリッドアプリケーションで予期しない動作が発生する可能性があります。 一部のライセンスされたコントロールでは、ハンドルの作成に応じてダイアログボックスが表示されます。 たとえば、ライセンスが付与されたコントロールは、ライセンスが必要であることをユーザーに通知します。または、ユーザーが3つの試用版のコントロールを使用していることを示します。  
  
 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素は <xref:System.Windows.Interop.HwndHost> クラスから派生し、子コントロールのハンドルは <xref:System.Windows.Forms.Integration.WindowsFormsHost.BuildWindowCore%2A> メソッド内に作成されます。 <xref:System.Windows.Interop.HwndHost> クラスでは、<xref:System.Windows.Forms.Integration.WindowsFormsHost.BuildWindowCore%2A> メソッドでメッセージを処理することはできませんが、ダイアログボックスを表示すると、メッセージが送信されます。 このライセンスシナリオを有効にするには、コントロールの <xref:System.Windows.Forms.Control.CreateControl%2A?displayProperty=nameWithType> メソッドを呼び出してから、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素の子として割り当てます。  
  
<a name="wpf_designer"></a>   
## <a name="wpf-designer"></a>WPF デザイナー  
 Wpf コンテンツをデザインするには、Visual Studio の WPF デザイナーを使用します。 次のセクションでは、WPF デザイナーを使用してハイブリッドアプリケーションを作成するときに発生する可能性がある一般的な問題について説明します。  
  
### <a name="backcolortransparent-is-ignored-at-design-time"></a>BackColorTransparent はデザイン時には無視されます  
 <xref:System.Windows.Forms.Integration.ElementHost.BackColorTransparent%2A> プロパティは、デザイン時に予期したとおりに動作しない可能性があります。  
  
 WPF コントロールが可視の親上にない場合、WPF ランタイムは <xref:System.Windows.Forms.Integration.ElementHost.BackColorTransparent%2A> 値を無視します。 <xref:System.Windows.Forms.Integration.ElementHost.BackColorTransparent%2A> が無視される理由は、<xref:System.Windows.Forms.Integration.ElementHost> オブジェクトが別の <xref:System.AppDomain>に作成されているためです。 ただし、アプリケーションを実行すると <xref:System.Windows.Forms.Integration.ElementHost.BackColorTransparent%2A> が想定どおりに動作します。  
  
### <a name="design-time-error-list-appears-when-the-obj-folder-is-deleted"></a>Obj フォルダーが削除されるとデザイン時のエラー一覧が表示される  
 Obj フォルダーが削除されると、デザイン時のエラー一覧が表示されます。  
  
 <xref:System.Windows.Forms.Integration.ElementHost>を使用してデザインする場合、Windows フォームデザイナーは、プロジェクトの obj フォルダー内の Debug フォルダーまたは Release フォルダーに生成されたファイルを使用します。 これらのファイルを削除すると、デザイン時のエラー一覧が表示されます。 この問題を解決するには、プロジェクトをリビルドします。 詳細については、「 [Windows フォームデザイナーのデザイン時エラー](../../winforms/controls/design-time-errors-in-the-windows-forms-designer.md)」を参照してください。  
  
<a name="elementhost_and_ime"></a>   
## <a name="elementhost-and-ime"></a>[エンティティと IME]  
 現在、<xref:System.Windows.Forms.Integration.ElementHost> でホストされている WPF コントロールは、<xref:System.Windows.Forms.Control.ImeMode%2A> プロパティをサポートしていません。 <xref:System.Windows.Forms.Control.ImeMode%2A> への変更は、ホストされているコントロールによって無視されます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.Integration.ElementHost>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [WPF デザイナーでの相互運用性](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/bb628658(v=vs.100))
- [Windows フォームと WPF の相互運用性入力アーキテクチャ](windows-forms-and-wpf-interoperability-input-architecture.md)
- [方法: ハイブリッド アプリケーションで視覚スタイルを有効にする](how-to-enable-visual-styles-in-a-hybrid-application.md)
- [WindowsFormsHost 要素のレイアウトに関する考慮事項](layout-considerations-for-the-windowsformshost-element.md)
- [Windows フォームと WPF プロパティの割り当て](windows-forms-and-wpf-property-mapping.md)
- [Windows フォーム デザイナーでのデザイン時エラー](../../winforms/controls/design-time-errors-in-the-windows-forms-designer.md)
- [移行と相互運用性](migration-and-interoperability.md)
