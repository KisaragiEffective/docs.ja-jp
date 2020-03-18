---
title: WPF と Windows フォーム相互運用機能
titleSuffix: ''
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Forms [WPF], interoperability with
- nester interoperation [WPF]
- Windows Forms [WPF], WPF interoperation
- interoperability [WPF], Windows Forms
- hybrid control [WPF interoperability]
ms.assetid: 9e8aa6b6-112c-4579-98d1-c974917df499
ms.openlocfilehash: 5141b84ecd002d920f0d4130bdc2529c0fce4994
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794051"
---
# <a name="wpf-and-windows-forms-interoperation"></a>WPF と Windows フォームの相互運用性
[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] と Windows フォームには、アプリケーションインターフェイスを作成するための2つの異なるアーキテクチャがあります。 <xref:System.Windows.Forms.Integration?displayProperty=nameWithType> 名前空間には、一般的な相互運用シナリオを可能にするクラスが用意されています。 相互運用機能を実装する2つの主要なクラスは、<xref:System.Windows.Forms.Integration.WindowsFormsHost> と <xref:System.Windows.Forms.Integration.ElementHost>です。 このトピックでは、サポートされている相互運用シナリオとサポートされていないシナリオについて説明します。  
  
> [!NOTE]
> *ハイブリッド制御*のシナリオには、特別な考慮事項があります。 ハイブリッドコントロールには、他のテクノロジのコントロールで入れ子になった1つのテクノロジからのコントロールがあります。 これは、入れ子に*なった相互運用*とも呼ばれます。 複数レベルの*ハイブリッドコントロール*には、複数レベルのハイブリッドコントロールの入れ子があります。 入れ子になった複数レベルの相互運用の例として、別の Windows フォームコントロールを含む [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールを含む Windows フォームコントロールがあります。 複数レベルのハイブリッドコントロールはサポートされていません。  

<a name="Windows_Presentation_Foundation_Application_Hosting"></a>   
## <a name="hosting-windows-forms-controls-in-wpf"></a>WPF での Windows フォームコントロールのホスト  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールが Windows フォームコントロールをホストする場合、次の相互運用シナリオがサポートされます。  
  
- [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールは、XAML を使用して1つ以上の Windows フォームコントロールをホストできます。  
  
- コードを使用して1つ以上の Windows フォームコントロールをホストできます。  
  
- 他の Windows フォームコントロールを含む Windows フォームコンテナーコントロールをホストする場合があります。  
  
- マスター/詳細形式のフォームをホストしていて、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] マスターと Windows フォーム詳細が含まれている場合があります。  
  
- マスター/詳細形式のフォームをホストしていて、Windows フォームマスターと [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 詳細が含まれている場合があります。  
  
- 1つ以上の ActiveX コントロールをホストできます。  
  
- 1つ以上の複合コントロールをホストできます。  
  
- [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]を使用してハイブリッドコントロールをホストする場合があります。  
  
- コードを使用してハイブリッドコントロールをホストする場合があります。  
  
### <a name="layout-support"></a>レイアウトのサポート  
 次の一覧では、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素が、ホストされている Windows フォームコントロールを [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] レイアウトシステムに統合しようとした場合の既知の制限事項について説明します。  
  
- 場合によっては、Windows フォームコントロールのサイズを変更することはできません。また、サイズを特定の大きさに限定することもできません。 たとえば、Windows フォーム <xref:System.Windows.Forms.ComboBox> コントロールでは、コントロールのフォントサイズによって定義される1つの高さのみがサポートされます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 動的レイアウトでは、要素を垂直方向に引き伸ばすことができると想定しているため、ホストされている <xref:System.Windows.Forms.ComboBox> コントロールは期待どおりに拡大されません。  
  
- Windows フォームコントロールを回転または傾斜させることはできません。 たとえば、ユーザーインターフェイスを90度回転させると、ホストされている Windows フォームコントロールの位置が維持されます。  
  
- ほとんどの場合、Windows フォームコントロールは比例スケールをサポートしていません。 コントロールの全体の次元はスケーリングされますが、コントロールの子コントロールとコンポーネント要素は、予期したとおりにサイズ変更されない可能性があります。 この制限は、各 Windows フォーム制御がどの程度スケーリングをサポートするかによって異なります。  
  
- [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のユーザーインターフェイスでは、要素の z オーダーを変更して、重複する動作を制御できます。 ホストされた Windows フォームコントロールは別の HWND で描画されるので、常に [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 要素の上に描画されます。  
  
- Windows フォームコントロールは、フォントサイズに基づいて自動スケールをサポートします。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のユーザーインターフェイスでは、フォントサイズを変更してもレイアウト全体のサイズは変更されませんが、個々の要素のサイズは動的に変更される可能性があります。  
  
### <a name="ambient-properties"></a>アンビエントプロパティ  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールのアンビエントプロパティには、同等の Windows フォームがあります。 これらのアンビエントプロパティは、ホストされている Windows フォームコントロールに反映され、<xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールのパブリックプロパティとして公開されます。 <xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールは、各 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アンビエントプロパティを同等の Windows フォームに変換します。  
  
 詳細については、「 [Windows フォームと WPF プロパティのマッピング](windows-forms-and-wpf-property-mapping.md)」を参照してください。  
  
### <a name="behavior"></a>動作  
 次の表では、相互運用の動作について説明します。  
  
|動作|サポートされています|サポートなし|  
|--------------|---------------|-------------------|  
|[透明度]|Windows フォームコントロールのレンダリングでは、透明度がサポートされます。 親 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールの背景は、ホストされている Windows フォームコントロールの背景になることがあります。|一部の Windows フォームコントロールは、透明度をサポートしていません。 たとえば、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]でホストされている場合、<xref:System.Windows.Forms.TextBox> コントロールと <xref:System.Windows.Forms.ComboBox> コントロールは透過的になりません。|  
|インデント|ホストされる Windows フォームコントロールのタブオーダーは、これらのコントロールが Windows フォームベースのアプリケーションでホストされている場合と同じです。<br /><br /> TAB キーおよび SHIFT + TAB キーを使用して、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールから Windows フォームコントロールに tab キーを押すと、通常どおりに動作します。<br /><br /> <xref:System.Windows.Forms.Control.TabStop%2A> プロパティ値が `false` Windows フォームコントロールは、ユーザーがコントロールを使用してタブを移動してもフォーカスを受け取りません。<br /><br /> -各 <xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールには <xref:System.Windows.Forms.Integration.WindowsFormsHost.TabIndex%2A> 値があり、<xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールがフォーカスを受け取るタイミングを決定します。<br />-<xref:System.Windows.Forms.Integration.WindowsFormsHost> コンテナー内に含まれる Windows フォームコントロールは、<xref:System.Windows.Forms.Control.TabIndex%2A> プロパティによって指定された順序に従います。 最後のタブインデックスから tab キーを置くと、次の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロール (存在する場合) にフォーカスが移動します。 フォーカスがある他の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールが存在しない場合は、タブオーダーの最初の Windows フォームコントロールに戻ります。<br /><xref:System.Windows.Forms.Integration.WindowsFormsHost> 内のコントロールの -   <xref:System.Windows.Forms.Integration.WindowsFormsHost.TabIndex%2A> 値は、<xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールに格納されている兄弟 Windows フォームコントロールを基準としています。<br />-Tab キーを指定すると、コントロール固有の動作が受け入れます。 たとえば、<xref:System.Windows.Forms.TextBoxBase.AcceptsTab%2A> プロパティ値が `true` の <xref:System.Windows.Forms.TextBox> コントロールで TAB キーを押すと、フォーカスを移動するのではなく、テキストボックスにタブが入力されます。|該当なし。|  
|方向キーを使用したナビゲーション|-<xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールの矢印キーを使用したナビゲーションは、通常の Windows フォームコンテナーコントロールと同じです。上方向キーと左方向キーは前のコントロールを選択し、下方向キーと右方向キーは次のコントロールを選択します。<br />-<xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールに含まれている最初のコントロールの上方向キーと左方向キーは、SHIFT + TAB キーショートカットと同じ操作を実行します。 フォーカスを [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールがある場合、フォーカスは <xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールの外に移動します。 この動作は、最後のコントロールがラップされないという点で、標準の <xref:System.Windows.Forms.ContainerControl> の動作とは異なります。 他のフォーカスが [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールが存在しない場合、フォーカスはタブオーダーの最後の Windows フォームコントロールに戻ります。<br />-<xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールに含まれる最後のコントロールの下方向キーと右方向キーは、TAB キーと同じ操作を実行します。 フォーカスを [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールがある場合、フォーカスは <xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールの外に移動します。 この動作は、最初のコントロールのラップが行われないという点で、標準の <xref:System.Windows.Forms.ContainerControl> の動作とは異なります。 他のフォーカスが [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールが存在しない場合は、フォーカスがタブオーダーの最初の Windows フォームコントロールに戻ります。|該当なし。|  
|ショート|アクセラレータは、"サポートされていません" 列に記載されている場合を除き、通常どおりに動作します。|テクノロジ間の重複するアクセラレータは、通常の重複アクセラレータと同じようには機能しません。 複数のテクノロジにわたってアクセラレータが複製され、少なくとも1つのが Windows フォームコントロール上にあり、もう一方が [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールにある場合、Windows フォームコントロールは常にアクセラレータを受け取ります。 重複するアクセラレータが押されている場合、フォーカスは切り替えられません。|  
|ショートカット キー|ショートカットキーは、"サポートされていません" 列に記載されている場合を除き、通常どおりに動作します。|-前処理段階で処理される Windows フォームショートカットキーは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のショートカットキーより常に優先されます。 たとえば、CTRL + S ショートカットキーが定義されている <xref:System.Windows.Forms.ToolStrip> コントロールがあり、CTRL + S にバインドされた [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コマンドがある場合、フォーカスに関係なく、常に <xref:System.Windows.Forms.ToolStrip> コントロールハンドラーが最初に呼び出されます。<br />-<xref:System.Windows.Forms.Control.KeyDown> イベントによって処理される Windows フォームのショートカットキーは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]で最後に処理されます。 この動作を回避するには、Windows フォームコントロールの <xref:System.Windows.Forms.Control.IsInputKey%2A> メソッドをオーバーライドするか、<xref:System.Windows.Forms.Control.PreviewKeyDown> イベントを処理します。 <xref:System.Windows.Forms.Control.IsInputKey%2A> メソッドから `true` を返すか、または <xref:System.Windows.Forms.PreviewKeyDownEventArgs.IsInputKey%2A?displayProperty=nameWithType> プロパティの値を <xref:System.Windows.Forms.Control.PreviewKeyDown> イベントハンドラーの `true` に設定します。|  
|AcceptsReturn、AcceptsTab、およびその他のコントロール固有の動作|既定のキーボード動作を変更するプロパティは通常どおり動作します。これは、Windows フォームコントロールが <xref:System.Windows.Forms.Control.IsInputKey%2A> メソッドをオーバーライドして `true`を返すことを前提としています。|<xref:System.Windows.Forms.Control.KeyDown> イベントを処理することによって既定のキーボード動作を変更するコントロールは、ホスト [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールで最後に処理されます。 Windows フォーム これらのコントロールは最後に処理されるため、予期しない動作が発生する可能性があります。|  
|イベントの開始と終了|フォーカスが、含まれている <xref:System.Windows.Forms.Integration.ElementHost> コントロールに移動しない場合、単一の <xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールでフォーカスが変更されると、Enter イベントと Leave イベントが通常どおりに発生します。|次のような変更が発生すると、Enter イベントと Leave イベントは発生しません。<br /><br /> -内部から <xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールの外部へ。<br />-外部から <xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロール内へ。<br />-<xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールの外側。<br />-<xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールでホストされている Windows フォームコントロールから、同じ <xref:System.Windows.Forms.Integration.WindowsFormsHost>内でホストされる <xref:System.Windows.Forms.Integration.ElementHost> コントロールに。|  
|マルチスレッド|すべての種類のマルチスレッドがサポートされています。|Windows フォームと [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の両方のテクノロジで、シングルスレッドの同時実行モデルが想定されています。 デバッグ中に、他のスレッドからフレームワークオブジェクトを呼び出すと、例外が発生し、この要件が適用されます。|  
|セキュリティ|すべての相互運用シナリオには、完全な信頼が必要です。|部分信頼では、相互運用シナリオは許可されません。|  
|ユーザー補助|すべてのユーザー補助シナリオがサポートされています。 補助技術製品は、Windows フォームと [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の両方のコントロールを含むハイブリッドアプリケーションで使用する場合、正しく機能します。|該当なし。|  
|クリップボードのトピック|すべてのクリップボード操作は通常どおりに動作します。 これには、Windows フォームコントロールと [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールの間の切り取りと貼り付けが含まれます。|該当なし。|  
|ドラッグアンドドロップ機能|すべてのドラッグアンドドロップ操作は通常どおりに動作します。 これには、Windows フォームと [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロール間の操作が含まれます。|該当なし。|  
  
<a name="Windows_Forms_Application_Hosting_Windows"></a>   
## <a name="hosting-wpf-controls-in-windows-forms"></a>Windows フォームでの WPF コントロールのホスト  
 Windows フォームコントロールが [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールをホストする場合、次の相互運用シナリオがサポートされます。  
  
- コードを使用して1つ以上の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールをホストしています。  
  
- プロパティシートと1つ以上のホストされる [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールとの関連付け。  
  
- フォーム内の1つ以上の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ページをホストしています。  
  
- [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ウィンドウを開始しています。  
  
- マスター/詳細フォームをホストして Windows フォームマスターと詳細を [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] します。  
  
- マスター/詳細フォームをホストして [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] マスターと詳細を Windows フォームします。  
  
- カスタム [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールをホストしています。  
  
- ハイブリッドコントロールをホストする。  
  
### <a name="ambient-properties"></a>アンビエントプロパティ  
 Windows フォームコントロールのアンビエントプロパティには、同等の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] があります。 これらのアンビエントプロパティは、ホストされている [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールに反映され、<xref:System.Windows.Forms.Integration.ElementHost> コントロールのパブリックプロパティとして公開されます。 <xref:System.Windows.Forms.Integration.ElementHost> コントロールは、各 Windows フォームアンビエントプロパティを同等の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] に変換します。  
  
 詳細については、「 [Windows フォームと WPF プロパティのマッピング](windows-forms-and-wpf-property-mapping.md)」を参照してください。  
  
### <a name="behavior"></a>動作  
 次の表では、相互運用の動作について説明します。  
  
|動作|サポートされています|サポートなし|  
|--------------|---------------|-------------------|  
|[透明度]|[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールのレンダリングでは、透明度がサポートされます。 親 Windows フォームコントロールの背景は、ホストされている [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールの背景になることがあります。|該当なし。|  
|マルチスレッド|すべての種類のマルチスレッドがサポートされています。|Windows フォームと [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の両方のテクノロジで、シングルスレッドの同時実行モデルが想定されています。 デバッグ中に、他のスレッドからフレームワークオブジェクトを呼び出すと、例外が発生し、この要件が適用されます。|  
|セキュリティ|すべての相互運用シナリオには、完全な信頼が必要です。|部分信頼では、相互運用シナリオは許可されません。|  
|ユーザー補助|すべてのユーザー補助シナリオがサポートされています。 補助技術製品は、Windows フォームと [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の両方のコントロールを含むハイブリッドアプリケーションで使用する場合、正しく機能します。|該当なし。|  
|クリップボードのトピック|すべてのクリップボード操作は通常どおりに動作します。 これには、Windows フォームコントロールと [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールの間の切り取りと貼り付けが含まれます。|該当なし。|  
|ドラッグアンドドロップ機能|すべてのドラッグアンドドロップ操作は通常どおりに動作します。 これには、Windows フォームと [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロール間の操作が含まれます。|該当なし。|  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.Integration.ElementHost>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [チュートリアル: WPF での Windows フォーム コントロールのホスト](walkthrough-hosting-a-windows-forms-control-in-wpf.md)
- [チュートリアル: WPF での Windows フォーム複合コントロールのホスト](walkthrough-hosting-a-windows-forms-composite-control-in-wpf.md)
- [チュートリアル: Windows フォームでの WPF 複合コントロールのホスト](walkthrough-hosting-a-wpf-composite-control-in-windows-forms.md)
- [Windows フォームと WPF プロパティの割り当て](windows-forms-and-wpf-property-mapping.md)
