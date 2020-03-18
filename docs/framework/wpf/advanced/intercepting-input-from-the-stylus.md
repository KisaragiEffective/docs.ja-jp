---
title: スタイラスからの入力のインターセプト
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- 'architecture [WPF], '
- ', '
- ', '
- ', '
ms.assetid: 791bb2f0-4e5c-4569-ac3c-211996808d44
ms.openlocfilehash: 7629843730a82584e94448ceac1ea574906876c9
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095139"
---
# <a name="intercepting-input-from-the-stylus"></a>スタイラスからの入力のインターセプト
<xref:System.Windows.Input.StylusPlugIns> アーキテクチャは、<xref:System.Windows.Input.Stylus> の入力とデジタルインク <xref:System.Windows.Ink.Stroke> オブジェクトの作成に対して低レベルの制御を実装するためのメカニズムを提供します。 <xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> クラスは、カスタム動作を実装し、最適なパフォーマンスを得るためにスタイラスデバイスから送信されるデータストリームに適用するためのメカニズムを提供します。  
  
 このトピックは次のサブセクションで構成されています。  
  
- [アーキテクチャ](#Architecture)  
  
- [スタイラスプラグインの実装](#ImplementingStylusPlugins)  
  
- [System.windows.controls.inkcanvas> にプラグインを追加する](#AddingYourPluginToAnInkCanvas)  
  
- [まとめ](#Conclusion)  
  
<a name="Architecture"></a>   
## <a name="architecture"></a>Architecture  
 <xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> は、「[ペン入力にアクセスして操作](https://docs.microsoft.com/previous-versions/ms818317(v%3dmsdn.10))する」で説明されている[StylusInput](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms574861(v=vs.90)) api の進化です。  
  
 各 <xref:System.Windows.UIElement> には、<xref:System.Windows.Input.StylusPlugIns.StylusPlugInCollection>である <xref:System.Windows.UIElement.StylusPlugIns%2A> プロパティがあります。 要素の <xref:System.Windows.UIElement.StylusPlugIns%2A> プロパティに <xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> を追加して、生成時に <xref:System.Windows.Input.StylusPoint> データを操作できます。 <xref:System.Windows.Input.StylusPoint> データは、システムデジタイザーによってサポートされるすべてのプロパティ (<xref:System.Windows.Input.StylusPoint.X%2A> や <xref:System.Windows.Input.StylusPoint.Y%2A> ポイントデータや <xref:System.Windows.Input.StylusPoint.PressureFactor%2A> データを含む) で構成されます。  
  
 <xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> を <xref:System.Windows.UIElement.StylusPlugIns%2A> プロパティに追加すると、<xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> オブジェクトが <xref:System.Windows.Input.Stylus> デバイスからのデータストリームに直接挿入されます。 プラグインが <xref:System.Windows.UIElement.StylusPlugIns%2A> コレクションに追加される順序によって、<xref:System.Windows.Input.StylusPoint> データを受信する順序が決まります。 たとえば、特定の領域に対する入力を制限するフィルタープラグインを追加した後、ジェスチャを認識するプラグインを追加した場合、ジェスチャを認識するプラグインは、フィルター処理された <xref:System.Windows.Input.StylusPoint> データを受信します。  
  
<a name="ImplementingStylusPlugins"></a>   
## <a name="implementing-stylus-plug-ins"></a>スタイラスプラグインの実装  
 プラグインを実装するには、<xref:System.Windows.Input.StylusPlugIns.StylusPlugIn>からクラスを派生させます。 このクラスは、<xref:System.Windows.Input.Stylus>からのデータストリームとして適用されます。 このクラスでは、<xref:System.Windows.Input.StylusPoint> データの値を変更できます。  
  
> [!CAUTION]
> <xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> が例外をスローした場合、または例外が発生した場合、アプリケーションは終了します。 <xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> を使用するコントロールを十分にテストし、<xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> が例外をスローしないことがわかっている場合にのみ、コントロールを使用します。  
  
 次の例では、<xref:System.Windows.Input.Stylus> デバイスから取得した <xref:System.Windows.Input.StylusPoint> データの <xref:System.Windows.Input.StylusPoint.X%2A> と <xref:System.Windows.Input.StylusPoint.Y%2A> の値を変更することによって、スタイラスの入力を制限するプラグインを示しています。  
  
 [!code-csharp[AdvancedInkTopicsSamples#19](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/DynamicRenderer.cs#19)]
 [!code-vb[AdvancedInkTopicsSamples#19](~/samples/snippets/visualbasic/VS_Snippets_Wpf/AdvancedInkTopicsSamples/VisualBasic/DynamicRenderer.vb#19)]  
[!code-csharp[AdvancedInkTopicsSamples#3](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/DynamicRenderer.cs#3)]
[!code-vb[AdvancedInkTopicsSamples#3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/AdvancedInkTopicsSamples/VisualBasic/DynamicRenderer.vb#3)]  
  
<a name="AddingYourPluginToAnInkCanvas"></a>   
## <a name="adding-your-plug-in-to-an-inkcanvas"></a>System.windows.controls.inkcanvas> にプラグインを追加する  
 カスタムプラグインを使用する最も簡単な方法は、System.windows.controls.inkcanvas> から派生するクラスを実装し、それを <xref:System.Windows.UIElement.StylusPlugIns%2A> プロパティに追加することです。  
  
 次の例は、インクをフィルター処理するカスタム <xref:System.Windows.Controls.InkCanvas> を示しています。  
  
 [!code-csharp[AdvancedInkTopicsSamples#4](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/Window1.xaml.cs#4)]  
  
 アプリケーションに `FilterInkCanvas` を追加して実行すると、ユーザーがストロークを完了するまで、インクが領域に限定されないことがわかります。 これは、<xref:System.Windows.Controls.InkCanvas> のプロパティが <xref:System.Windows.Controls.InkCanvas.DynamicRenderer%2A> であり、<xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> であり、既に <xref:System.Windows.UIElement.StylusPlugIns%2A> コレクションのメンバーであるためです。 <xref:System.Windows.UIElement.StylusPlugIns%2A> コレクションに追加したカスタム <xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> は、<xref:System.Windows.Input.StylusPlugIns.DynamicRenderer> データを受信した後に <xref:System.Windows.Input.StylusPoint> データを受け取ります。 その結果、ユーザーがペンを離してストロークを終了するまで、<xref:System.Windows.Input.StylusPoint> データはフィルター処理されません。 ユーザーが描画するときにインクをフィルター処理するには、<xref:System.Windows.Input.StylusPlugIns.DynamicRenderer>の前に `FilterPlugin` を挿入する必要があります。  
  
 次C#のコードは、描画時にインクをフィルター処理するカスタム <xref:System.Windows.Controls.InkCanvas> を示しています。  
  
 [!code-csharp[AdvancedInkTopicsSamples#5](~/samples/snippets/csharp/VS_Snippets_Wpf/AdvancedInkTopicsSamples/CSharp/Window1.xaml.cs#5)]  
  
<a name="Conclusion"></a>   
## <a name="conclusion"></a>まとめ  
 独自の <xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> クラスを派生させて <xref:System.Windows.Input.StylusPlugIns.StylusPlugInCollection> コレクションに挿入することにより、デジタルインクの動作を大幅に向上させることができます。 生成された <xref:System.Windows.Input.StylusPoint> データにアクセスできるため、<xref:System.Windows.Input.Stylus> 入力をカスタマイズすることができます。 <xref:System.Windows.Input.StylusPoint> のデータにはこのような低レベルのアクセス権があるため、アプリケーションに最適なパフォーマンスでインクの収集とレンダリングを実装できます。  
  
## <a name="see-also"></a>参照

- [高度なインク処理](advanced-ink-handling.md)
- [ペン入力にアクセスして操作する](https://docs.microsoft.com/previous-versions/ms818317(v%3dmsdn.10))
