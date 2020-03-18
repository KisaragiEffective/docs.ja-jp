---
title: インク入力コントロールの作成
ms.date: 03/30/2017
helpviewer_keywords:
- ink strokes [WPF], managing
- managing ink strokes [WPF]
- ink input control [WPF]
- input from mouse [WPF], accepting
- mouse input [WPF], accepting
- ink [WPF], rendering
- ink strokes [WPF], collecting
- rendering ink [WPF]
- collecting ink strokes [WPF]
- DynamicRenderer objects [WPF]
- StylusPlugIn objects [WPF]
ms.assetid: c31f3a67-cb3f-4ded-af9e-ed21f6575b26
ms.openlocfilehash: 98b2abf813a232e36b80683254174b12b97659bb
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095217"
---
# <a name="creating-an-ink-input-control"></a>インク入力コントロールの作成
インクを動的に、または静的にレンダリングするカスタムコントロールを作成できます。 つまり、ユーザーがストロークを描画し、タブレットペンからインクが "flow" に見えるようにインクを描画し、タブレットペンを使用して、またはクリップボードから貼り付けた後、またはファイルから読み込まれた後にインクを表示します。 インクを動的にレンダリングするには、コントロールで <xref:System.Windows.Input.StylusPlugIns.DynamicRenderer>を使用する必要があります。 インクを静的にレンダリングするには、スタイラスイベントメソッド (<xref:System.Windows.UIElement.OnStylusDown%2A>、<xref:System.Windows.UIElement.OnStylusMove%2A>、および <xref:System.Windows.UIElement.OnStylusUp%2A>) をオーバーライドして <xref:System.Windows.Input.StylusPoint> データを収集し、ストロークを作成して、それらを <xref:System.Windows.Controls.InkPresenter> に追加する必要があります (コントロールのインクをレンダリングします)。  
  
 このトピックは次のサブセクションで構成されています。  
  
- [方法: スタイラスポイントデータを収集し、インクストロークを作成する](#CollectingStylusPointDataAndCreatingInkStrokes)  
  
- [方法: コントロールでマウスからの入力を受け入れることができるようにする](#EnablingYourControlToAcceptInputTromTheMouse)  
  
- [まとめ](#PuttingItTogether)  
  
- [追加のプラグインと DynamicRenderers の使用](#UsingAdditionalPluginsAndDynamicRenderers)  
  
- [まとめ](#AdvancedInkHandling_Conclusion)  
  
<a name="CollectingStylusPointDataAndCreatingInkStrokes"></a>   
## <a name="how-to-collect-stylus-point-data-and-create-ink-strokes"></a>方法: スタイラスポイントデータを収集し、インクストロークを作成する  
 インクストロークを収集して管理するコントロールを作成するには、次の手順を実行します。  
  
1. <xref:System.Windows.Controls.Control> または <xref:System.Windows.Controls.Control>から派生したクラス (<xref:System.Windows.Controls.Label>など) のいずれかからクラスを派生させます。  
  
     [!code-csharp[AdvancedInkTopicsSamples#20](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/StylusControl.cs#20)]  
    [!code-csharp[AdvancedInkTopicsSamples#14](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/StylusControlSnippets.cs#14)]  
    [!code-csharp[AdvancedInkTopicsSamples#15](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/StylusControlSnippets.cs#15)]  
  
2. クラスに <xref:System.Windows.Controls.InkPresenter> を追加し、<xref:System.Windows.Controls.ContentControl.Content%2A> プロパティを新しい <xref:System.Windows.Controls.InkPresenter>に設定します。  
  
     [!code-csharp[AdvancedInkTopicsSamples#16](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/StylusControlSnippets.cs#16)]  
  
3. <xref:System.Windows.Controls.InkPresenter.AttachVisuals%2A> メソッドを呼び出して <xref:System.Windows.Controls.InkPresenter> に <xref:System.Windows.Input.StylusPlugIns.DynamicRenderer> の <xref:System.Windows.Input.StylusPlugIns.DynamicRenderer.RootVisual%2A> をアタッチし、<xref:System.Windows.Input.StylusPlugIns.DynamicRenderer> を <xref:System.Windows.UIElement.StylusPlugIns%2A> コレクションに追加します。 これにより、コントロールによってスタイラスポイントデータが収集されるときに、<xref:System.Windows.Controls.InkPresenter> がインクを表示できるようになります。  
  
     [!code-csharp[AdvancedInkTopicsSamples#17](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/StylusControlSnippets.cs#17)]  
    [!code-csharp[AdvancedInkTopicsSamples#18](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/StylusControlSnippets.cs#18)]  
  
4. <xref:System.Windows.UIElement.OnStylusDown%2A> メソッドをオーバーライドします。  このメソッドでは、<xref:System.Windows.Input.Stylus.Capture%2A>の呼び出しを使用してスタイラスをキャプチャします。 スタイラスをキャプチャすることにより、コントロールは、スタイラスがコントロールの境界内から出た場合でも、<xref:System.Windows.UIElement.StylusMove> と <xref:System.Windows.UIElement.StylusUp> のイベントを受信し続けます。 これは厳密には必須ではありませんが、優れたユーザーエクスペリエンスを実現するためにほとんどの場合に必要です。 新しい <xref:System.Windows.Input.StylusPointCollection> を作成して <xref:System.Windows.Input.StylusPoint> データを収集します。 最後に、<xref:System.Windows.Input.StylusPoint> データの初期セットを <xref:System.Windows.Input.StylusPointCollection>に追加します。  
  
     [!code-csharp[AdvancedInkTopicsSamples#7](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/StylusControl.cs#7)]  
  
5. <xref:System.Windows.UIElement.OnStylusMove%2A> メソッドをオーバーライドし、前に作成した <xref:System.Windows.Input.StylusPointCollection> オブジェクトに <xref:System.Windows.Input.StylusPoint> データを追加します。  
  
     [!code-csharp[AdvancedInkTopicsSamples#8](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/StylusControl.cs#8)]  
  
6. <xref:System.Windows.UIElement.OnStylusUp%2A> メソッドをオーバーライドし、<xref:System.Windows.Input.StylusPointCollection> データを使用して新しい <xref:System.Windows.Ink.Stroke> を作成します。 作成した新しい <xref:System.Windows.Ink.Stroke> を <xref:System.Windows.Controls.InkPresenter> の <xref:System.Windows.Controls.InkPresenter.Strokes%2A> コレクションに追加し、スタイラスのキャプチャを解放します。  
  
     [!code-csharp[AdvancedInkTopicsSamples#10](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/StylusControl.cs#10)]  
  
<a name="EnablingYourControlToAcceptInputTromTheMouse"></a>   
## <a name="how-to-enable-your-control-to-accept-input-from-the-mouse"></a>方法: コントロールでマウスからの入力を受け入れることができるようにする  
 前のコントロールをアプリケーションに追加して実行し、マウスを入力デバイスとして使用した場合、ストロークは永続化されていないことがわかります。 入力デバイスとしてマウスが使用されているときにストロークを保持するには、次の手順を実行します。  
  
1. <xref:System.Windows.UIElement.OnMouseLeftButtonDown%2A> をオーバーライドし、新しい <xref:System.Windows.Input.StylusPointCollection> 作成して、イベントが発生したときにマウスの位置を取得し、ポイントデータを使用して <xref:System.Windows.Input.StylusPoint> を作成して、<xref:System.Windows.Input.StylusPoint> を <xref:System.Windows.Input.StylusPointCollection>に追加します。  
  
     [!code-csharp[AdvancedInkTopicsSamples#11](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/StylusControl.cs#11)]  
  
2. <xref:System.Windows.UIElement.OnMouseMove%2A> メソッドをオーバーライドします。 イベントが発生したときのマウスの位置を取得し、ポイントデータを使用して <xref:System.Windows.Input.StylusPoint> を作成します。  前の手順で作成した <xref:System.Windows.Input.StylusPointCollection> オブジェクトに <xref:System.Windows.Input.StylusPoint> を追加します。  
  
     [!code-csharp[AdvancedInkTopicsSamples#12](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/StylusControl.cs#12)]  
  
3. <xref:System.Windows.UIElement.OnMouseLeftButtonUp%2A> メソッドをオーバーライドします。  <xref:System.Windows.Input.StylusPointCollection> データを使用して新しい <xref:System.Windows.Ink.Stroke> を作成し、作成した新しい <xref:System.Windows.Ink.Stroke> を <xref:System.Windows.Controls.InkPresenter>の <xref:System.Windows.Controls.InkPresenter.Strokes%2A> コレクションに追加します。  
  
     [!code-csharp[AdvancedInkTopicsSamples#13](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/StylusControl.cs#13)]  
  
<a name="PuttingItTogether"></a>   
## <a name="putting-it-together"></a>まとめ  
 次の例は、ユーザーがマウスまたはペンを使用したときにインクを収集するカスタムコントロールです。  
  
 [!code-csharp[AdvancedInkTopicsSamples#20](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/StylusControl.cs#20)]  
[!code-csharp[AdvancedInkTopicsSamples#6](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/StylusControl.cs#6)]  
  
<a name="UsingAdditionalPluginsAndDynamicRenderers"></a>   
## <a name="using-additional-plug-ins-and-dynamicrenderers"></a>追加のプラグインと DynamicRenderers の使用  
 System.windows.controls.inkcanvas> と同様に、カスタムコントロールには、カスタム <xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> および追加の <xref:System.Windows.Input.StylusPlugIns.DynamicRenderer> オブジェクトを含めることができます。 これらの <xref:System.Windows.UIElement.StylusPlugIns%2A> コレクションに追加します。 <xref:System.Windows.Input.StylusPlugIns.StylusPlugInCollection> 内の <xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> オブジェクトの順序は、描画時のインクの外観に影響します。 `dynamicRenderer` という <xref:System.Windows.Input.StylusPlugIns.DynamicRenderer> と、タブレットペンからインクをオフセットする `translatePlugin` と呼ばれるカスタム <xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> があるとします。 `translatePlugin` が <xref:System.Windows.Input.StylusPlugIns.StylusPlugInCollection>の最初の <xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> であり、`dynamicRenderer` が2番目の場合は、ユーザーがペンを動かしたときに "flow" というインクがオフセットされます。 `dynamicRenderer` が最初で、`translatePlugin` が2番目の場合、ユーザーがペンを持ち上げるまでインクはオフセットされません。  
  
<a name="AdvancedInkHandling_Conclusion"></a>   
## <a name="conclusion"></a>まとめ  
 スタイラスイベントメソッドをオーバーライドすることにより、インクを収集して表示するコントロールを作成できます。 独自のコントロールを作成し、独自の <xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> クラスを派生させ、それを <xref:System.Windows.Input.StylusPlugIns.StylusPlugInCollection>に挿入することにより、デジタルインクで可能なすべての動作を実質的に実装できます。 生成された <xref:System.Windows.Input.StylusPoint> データにアクセスできるため、<xref:System.Windows.Input.Stylus> 入力をカスタマイズして、アプリケーションに合わせて画面に表示することができます。 <xref:System.Windows.Input.StylusPoint> データへの低レベルのアクセス権があるため、インクコレクションを実装し、アプリケーションに最適なパフォーマンスでレンダリングできます。  
  
## <a name="see-also"></a>参照

- [高度なインク処理](advanced-ink-handling.md)
- [ペン入力にアクセスして操作する](https://docs.microsoft.com/previous-versions/ms818317(v=msdn.10))
