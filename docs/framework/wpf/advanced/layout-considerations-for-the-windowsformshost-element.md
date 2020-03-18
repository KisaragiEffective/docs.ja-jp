---
title: WindowsFormsHost 要素のレイアウトに関する考慮事項
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Forms [WPF], interoperability with
- Windows Forms [WPF], WPF interoperation
- interoperability [WPF], Windows Forms
- WindowsFormsHost element layout considerations [WPF]
- dynamic layout [WPF interoperability]
- device-independent pixels
ms.assetid: 3c574597-bbde-440f-95cc-01371f1a5d9d
ms.openlocfilehash: 89ed57a787b93a1326b4accd3bb1bc5ff9a825fd
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095152"
---
# <a name="layout-considerations-for-the-windowsformshost-element"></a>WindowsFormsHost 要素のレイアウトに関する考慮事項
このトピックでは、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素が [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] レイアウトシステムとどのように連携するかについて説明します。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] と Windows フォームでは、フォームまたはページ上の要素のサイズ設定と配置について、さまざまな方法で類似したロジックをサポートしています。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]で Windows フォームコントロールをホストするハイブリッドユーザーインターフェイス (UI) を作成すると、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素によって2つのレイアウトスキームが統合されます。  
  
## <a name="differences-in-layout-between-wpf-and-windows-forms"></a>WPF と Windows フォームのレイアウトの違い  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、解像度に依存しないレイアウトを使用します。 すべての [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] レイアウトディメンションは、*デバイスに依存しないピクセル*を使用して指定されます。 デバイスに依存しないピクセルは、サイズが 90 ~ 6 インチ、解像度に依存しないため、72 dpi モニターと 19200 dpi のどちらのプリンターをレンダリングしているかに関係なく、同様の結果が得られます。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は*動的レイアウト*にも基づいています。 つまり、UI 要素は、そのコンテンツ、親レイアウトコンテナー、および使用可能な画面サイズに応じて、フォームまたはページに配置されます。 動的レイアウトを使用すると、UI 要素のサイズと位置が変更されたときに UI 要素のサイズと位置が自動的に調整されるので、ローカライズが容易になります。  
  
 Windows フォームのレイアウトはデバイスに依存しており、静的である可能性が高くなります。 通常、Windows フォームコントロールは、ハードウェアピクセルで指定された寸法を使用してフォーム上に絶対に配置されます。 ただし、次の表に示すように、Windows フォームでは動的レイアウト機能がいくつかサポートされています。  
  
|レイアウト機能|[説明]|  
|--------------------|-----------------|  
|自動サイズ調整|一部の Windows フォームコントロール自体のサイズを変更して、内容が正しく表示されるようにします。 詳細については、「 [AutoSize プロパティの概要](../../winforms/controls/autosize-property-overview.md)」を参照してください。|  
|固定とドッキング|Windows フォームコントロールは、親コンテナーに基づく配置とサイズ設定をサポートします。 詳細については、「 <xref:System.Windows.Forms.Control.Anchor%2A?displayProperty=nameWithType> および <xref:System.Windows.Forms.Control.Dock%2A?displayProperty=nameWithType>」を参照してください。|  
|自動スケール|コンテナーコントロールは、出力デバイスの解像度または既定のコンテナーフォントのサイズ (ピクセル単位) に基づいて、自身とその子のサイズを変更します。 詳細については、「 [Windows フォームでの自動スケーリング](../../winforms/automatic-scaling-in-windows-forms.md)」を参照してください。|  
|レイアウトコンテナー|<xref:System.Windows.Forms.FlowLayoutPanel> コントロールと <xref:System.Windows.Forms.TableLayoutPanel> コントロールは、そのコンテンツに応じて子コントロールを配置し、サイズを調整します。|  
  
## <a name="layout-limitations"></a>レイアウトの制限事項  
 一般に、Windows フォームコントロールは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]で可能な範囲にスケーリングおよび変換することはできません。 次の一覧では、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素が、ホストされている Windows フォームコントロールを [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] レイアウトシステムに統合しようとした場合の既知の制限事項について説明します。  
  
- 場合によっては、Windows フォームコントロールのサイズを変更することはできません。また、サイズを特定の大きさに限定することもできません。 たとえば、Windows フォーム <xref:System.Windows.Forms.ComboBox> コントロールでは、コントロールのフォントサイズによって定義される1つの高さのみがサポートされます。 要素が垂直方向に引き伸ばすことができる動的レイアウト [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、ホストされている <xref:System.Windows.Forms.ComboBox> コントロールは期待どおりに拡大されません。  
  
- Windows フォームコントロールを回転または傾斜させることはできません。 傾斜変換または回転変換を適用すると、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素によって <xref:System.Windows.Forms.Integration.WindowsFormsHost.LayoutError> イベントが発生します。 <xref:System.Windows.Forms.Integration.WindowsFormsHost.LayoutError> イベントを処理しない場合は、<xref:System.InvalidOperationException> が発生します。  
  
- ほとんどの場合、Windows フォームコントロールは比例スケールをサポートしていません。 コントロールの全体の次元はスケーリングされますが、コントロールの子コントロールとコンポーネント要素は、予期したとおりにサイズ変更されない可能性があります。 この制限は、各 Windows フォーム制御がどの程度スケーリングをサポートするかによって異なります。 また、Windows フォームコントロールのサイズを0ピクセルに縮小することはできません。  
  
- Windows フォームコントロールは自動スケールをサポートしています。自動スケールでは、フォントサイズに基づいてフォーム自体とコントロールのサイズが自動的に変更されます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のユーザーインターフェイスでは、フォントサイズを変更してもレイアウト全体のサイズは変更されませんが、個々の要素のサイズは動的に変更される可能性があります。  
  
### <a name="z-order"></a>Z オーダー  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のユーザーインターフェイスでは、要素の z オーダーを変更して、重複する動作を制御できます。 ホストされた Windows フォームコントロールは別の HWND で描画されるので、常に [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 要素の上に描画されます。  
  
 ホストされている Windows フォームコントロールは、<xref:System.Windows.Documents.Adorner> の要素の上にも描画されます。  
  
## <a name="layout-behavior"></a>レイアウトの動作  
 以下のセクションでは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]で Windows フォームコントロールをホストするときのレイアウト動作の特定の側面について説明します。  
  
### <a name="scaling-unit-conversion-and-device-independence"></a>スケーリング、単位変換、およびデバイスの非依存  
 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素が [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] と Windows フォームディメンションに関係する操作を実行するたびに、2つの座標系が関係します。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の場合はデバイスに依存しないピクセル、Windows フォームの場合はハードウェアピクセルです。 したがって、一貫性のあるレイアウトを実現するには、適切な単位およびスケール変換を適用する必要があります。  
  
 座標系間の変換は、現在のデバイスの解像度と、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素またはその先祖に適用されたレイアウトまたはレンダリング変換によって異なります。  
  
 出力デバイスが 96 dpi で、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素にスケーリングが適用されていない場合、デバイスに依存しない1つのピクセルが1つのハードウェアピクセルと等しくなります。  
  
 それ以外の場合は、座標系のスケーリングが必要です。 ホストされるコントロールのサイズは変更されません。 代わりに、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素は、ホストされるコントロールとそのすべての子コントロールのスケールを試みます。 Windows フォームはスケーリングを完全にサポートしていないため、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素は特定のコントロールでサポートされている程度にスケーリングします。  
  
 <xref:System.Windows.Forms.Integration.WindowsFormsHost.ScaleChild%2A> メソッドをオーバーライドして、ホストされる Windows フォームコントロールのカスタムスケーリング動作を提供します。  
  
 スケーリングに加えて、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素は、次の表で説明する丸めとオーバーフローのケースを処理します。  
  
|変換の問題|[説明]|  
|----------------------|-----------------|  
|丸め|[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] デバイスに依存しないピクセルディメンションは `double`として指定され Windows フォームハードウェアピクセルディメンションは `int`として指定されます。 `double`ベースのディメンションが `int`ベースのディメンションに変換される場合、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素は標準の丸め処理を使用するため、0.5 未満の小数値は0に切り捨てられます。|  
|オーバーフロー|<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素が `double` 値から `int` 値に変換されると、オーバーフローが発生する可能性があります。 <xref:System.Int32.MaxValue> より大きい値は <xref:System.Int32.MaxValue>に設定されます。|  
  
### <a name="layout-related-properties"></a>レイアウト関連のプロパティ  
 Windows フォームコントロールと [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 要素のレイアウト動作を制御するプロパティは、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素によって適切にマップされます。 詳細については、「 [Windows フォームと WPF プロパティのマッピング](windows-forms-and-wpf-property-mapping.md)」を参照してください。  
  
### <a name="layout-changes-in-the-hosted-control"></a>ホストされるコントロールのレイアウトの変更  
 ホストされている Windows フォームコントロールのレイアウトの変更は、レイアウトの更新をトリガーするために [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] に反映されます。 <xref:System.Windows.Forms.Integration.WindowsFormsHost> の <xref:System.Windows.UIElement.InvalidateMeasure%2A> メソッドによって、ホストされるコントロールのレイアウトの変更によって、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] レイアウトエンジンが実行されるようになります。  
  
### <a name="continuously-sized-windows-forms-controls"></a>継続的にサイズ設定 Windows フォームコントロール  
 継続的なスケーリングをサポートするコントロールは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] レイアウトシステムと完全に対話する Windows フォームます。 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素は、<xref:System.Windows.FrameworkElement.MeasureOverride%2A> メソッドと <xref:System.Windows.FrameworkElement.ArrangeOverride%2A> メソッドを通常どおりに使用して、ホストされている Windows フォームコントロールのサイズを調整します。  
  
### <a name="sizing-algorithm"></a>サイズ設定アルゴリズム  
 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素では、次の手順を使用して、ホストされるコントロールのサイズを変更します。  
  
1. <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素は、<xref:System.Windows.FrameworkElement.MeasureOverride%2A> メソッドと <xref:System.Windows.FrameworkElement.ArrangeOverride%2A> メソッドをオーバーライドします。  
  
2. <xref:System.Windows.FrameworkElement.MeasureOverride%2A> メソッドは、ホストされるコントロールのサイズを確認するために、<xref:System.Windows.FrameworkElement.MeasureOverride%2A> メソッドに渡される制約から変換された制約を使用して、ホストされるコントロールの <xref:System.Windows.Forms.Control.GetPreferredSize%2A> メソッドを呼び出します。  
  
3. <xref:System.Windows.FrameworkElement.ArrangeOverride%2A> メソッドは、ホストされているコントロールを所定のサイズ制約に設定しようとします。  
  
4. ホストされるコントロールの <xref:System.Windows.Forms.Control.Size%2A> プロパティが指定された制約に一致する場合、ホストされるコントロールは制約に合わせてサイズが変更されます。  
  
 <xref:System.Windows.Forms.Control.Size%2A> プロパティが指定された制約と一致しない場合、ホストされているコントロールは継続的なサイズ設定をサポートしません。 たとえば、<xref:System.Windows.Forms.MonthCalendar> コントロールでは不連続サイズしか使用できません。 このコントロールで許可されるサイズは、高さと幅の両方の整数 (月数を表す) で構成されます。 このような場合、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素は次のように動作します。  
  
- <xref:System.Windows.Forms.Control.Size%2A> プロパティが、指定された制約よりも大きいサイズを返す場合、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素はホストされているコントロールをクリップします。 高さと幅は個別に処理されるため、ホストされるコントロールはどちらの方向にもクリップされる可能性があります。  
  
- <xref:System.Windows.Forms.Control.Size%2A> プロパティが、指定された制約よりも小さいサイズを返す場合、<xref:System.Windows.Forms.Integration.WindowsFormsHost> はこのサイズの値を受け入れ、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] レイアウトシステムに値を返します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Integration.ElementHost>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [チュートリアル: WPF での Windows フォーム コントロールの配置](walkthrough-arranging-windows-forms-controls-in-wpf.md)
- [WPF サンプルでの Windows フォームコントロールの配置](https://github.com/microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/WpfLayoutHostingWfWithXaml)
- [Windows フォームと WPF プロパティの割り当て](windows-forms-and-wpf-property-mapping.md)
- [移行と相互運用性](migration-and-interoperability.md)
