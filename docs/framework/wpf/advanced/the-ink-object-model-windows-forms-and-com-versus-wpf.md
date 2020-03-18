---
title: デジタルインク-Windows フォームと COM と WPF の比較
titleSuffix: ''
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- enabling ink [WPF]
- InkCanvas control [WPF]
- ink object model [WPF]
- tablet pen [WPF], events [WPF]
- ink [WPF], enabling
- events [WPF], tablet pen
ms.assetid: 577835be-b145-4226-8570-1d309e9b3901
ms.openlocfilehash: 4a183bba2c5cfb2d12a9cf435ae1f92b4cf63948
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76737286"
---
# <a name="the-ink-object-model-windows-forms-and-com-versus-wpf"></a>インク オブジェクト モデル : Windows フォームおよび COM と WPF の比較

基本的に、デジタルインクをサポートするプラットフォームには、Tablet PC Windows フォームプラットフォーム、Tablet PC COM プラットフォーム、および Windows Presentation Foundation (WPF) プラットフォームの3つがあります。  Windows フォームと COM プラットフォームは同様のオブジェクトモデルを共有しますが、WPF プラットフォームのオブジェクトモデルは大きく異なります。  このトピックでは、1つのオブジェクトモデルを操作した開発者がもう一方のオブジェクトモデルを理解しやすくなるように、大まかな違いについて説明します。  
  
## <a name="enabling-ink-in-an-application"></a>アプリケーションでのインクの有効化  
 3つのすべてのプラットフォームには、アプリケーションがタブレットペンから入力を受け取ることができるようにするオブジェクトとコントロールが付属しています。  Windows フォームと COM[プラットフォームには](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583740(v=vs.90))、 [InkCollector](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583683(v=vs.90)) [クラスと、](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms552322(v=vs.90)) [microsoft..](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms552265(v=vs.90))...............。  アプリケーションに追加してインクを収集する[コントロールは、](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms552265(v=vs.90)) [microsoft](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583740(v=vs.90))のアプリケーションに追加することができます。  [InkCollector と microsoft](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583683(v=vs.90))は、既存のウィンドウに[アタッチして](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms552322(v=vs.90))、windows およびカスタムコントロールをインク対応にすることができます。  
  
 WPF プラットフォームには、<xref:System.Windows.Controls.InkCanvas> コントロールが含まれています。  アプリケーションに <xref:System.Windows.Controls.InkCanvas> を追加して、すぐにインクの収集を開始することができます。 <xref:System.Windows.Controls.InkCanvas>では、ユーザーはインクのコピー、選択、およびサイズ変更を行うことができます。  <xref:System.Windows.Controls.InkCanvas>に他のコントロールを追加することができ、ユーザーはこれらのコントロールを手書きでも挿入できます。  インクが有効なカスタムコントロールを作成するには、<xref:System.Windows.Controls.InkPresenter> を追加し、そのスタイラスポイントを収集します。  
  
 次の表に、アプリケーションでインクを有効にする方法の詳細について説明します。  
  
|操作方法|WPF プラットフォームで...|Windows フォーム/COM プラットフォームで...|  
|-----------------|--------------------------|------------------------------------------|  
|アプリケーションにインク対応コントロールを追加する|「[インクを使用したはじめに」を](getting-started-with-ink.md)参照してください。|[自動要求フォームのサンプル](/windows/desktop/tablet/auto-claims-form-sample)を参照してください|  
|カスタムコントロールでインクを有効にする|「[インク入力コントロールの作成」を](creating-an-ink-input-control.md)参照してください。|「[インククリップボードのサンプル](/windows/desktop/tablet/ink-clipboard-sample)」を参照してください。|  
  
## <a name="ink-data"></a>インクデータ  
 Windows フォームと COM プラットフォームでは、 [InkCollector](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583683(v=vs.90)) [、microsoft](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms552322(v=vs.90))は、それぞれが、microsoft のインクを持つようになります。[また、microsoft](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms552265(v=vs.90))は、それぞれ、microsoft の[ink オブジェクトを](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583670(v=vs.90))公開[します。](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583740(v=vs.90)) [Microsoft ink](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583670(v=vs.90))オブジェクトには、1つまたは複数の[microsoft ink](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms552692(v=vs.90))オブジェクトのデータが含まれており、これらのストロークを管理および操作するための一般的なメソッドとプロパティを公開します。  [Microsoft ink](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583670(v=vs.90))オブジェクトは、含まれているストロークの有効期間を管理します。[Microsoft ink](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583670(v=vs.90))オブジェクトは、所有しているストロークを作成および削除します。  各[Microsoft ink](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms552692(v=vs.90))は、その親である[microsoft ink](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583670(v=vs.90))オブジェクト内で一意の識別子を持っています。  
  
 WPF プラットフォームでは、<xref:System.Windows.Ink.Stroke?displayProperty=nameWithType> クラスは独自の有効期間を所有し、管理します。 <xref:System.Windows.Ink.Stroke> オブジェクトのグループは、<xref:System.Windows.Ink.StrokeCollection>にまとめて収集できます。これにより、ヒットテスト、消去、変換、およびインクのシリアル化などの一般的なインクデータ管理操作のためのメソッドが提供されます。 <xref:System.Windows.Ink.Stroke> は、任意のタイミングで、0個、1個、または複数の <xref:System.Windows.Ink.StrokeCollection> オブジェクトに属することができます。  <xref:System.Windows.Controls.InkCanvas> と <xref:System.Windows.Controls.InkPresenter> には、<xref:System.Windows.Ink.StrokeCollection?displayProperty=nameWithType>が含まれてい[ます。](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583670(v=vs.90))  
  
 次の図は、インクデータオブジェクトモデルを比較したものです。  Windows フォームと COM プラットフォームでは[、microsoft の ink オブジェクトは](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583670(v=vs.90))、Microsoft の[Stroke](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms552692(v=vs.90))オブジェクトの有効期間を制限し、スタイラスパケットは個々のストロークに属します。  次の図に示すように、2つ以上のストロークで同じ[DrawingAttributes](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583636(v=vs.90))オブジェクトを参照できます。  
  
 ![COM&#47;Winforms のインクオブジェクトモデルの図。](./media/ink-inkownsstrokes.png "Ink_InkOwnsStrokes")  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]では、各 <xref:System.Windows.Ink.Stroke?displayProperty=nameWithType> は共通言語ランタイムオブジェクトであり、そこに何らかの参照が含まれている限り存在します。  各 <xref:System.Windows.Ink.Stroke> は、共通言語ランタイムオブジェクトでもある <xref:System.Windows.Input.StylusPointCollection> および <xref:System.Windows.Ink.DrawingAttributes?displayProperty=nameWithType> オブジェクトを参照します。  
  
 ![WPF のインクオブジェクトモデルの図。](./media/ink-wpfinkobjectmodel.png "Ink_WPFInkObjectModel")  
  
 次の表は、WPF プラットフォームと Windows フォームと COM プラットフォームで一般的なタスクを実行する方法を比較したものです。  
  
|タスク|Windows Presentation Foundation|Windows フォームと COM|  
|----------|-------------------------------------|---------------------------|  
|インクを保存する|<xref:System.Windows.Ink.StrokeCollection.Save%2A>|[Microsoft. Ink. 保存](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms571335(v=vs.90))|  
|インクの読み込み|<xref:System.Windows.Ink.StrokeCollection.%23ctor%2A> コンストラクターを使用して <xref:System.Windows.Ink.StrokeCollection> を作成します。|[Microsoft Ink。読み込み](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms569609(v=vs.90))|  
|ヒットテスト|<xref:System.Windows.Ink.StrokeCollection.HitTest%2A>|[System.windows.media.visualtreehelper.hittest (Microsoft Ink)](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms571330(v=vs.90))|  
|インクをコピーする|<xref:System.Windows.Controls.InkCanvas.CopySelection%2A>|[Microsoft Ink クリップボードのコピー](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms571316(v=vs.90))|  
|インクの貼り付け|<xref:System.Windows.Controls.InkCanvas.Paste%2A>|[Microsoft. Ink. クリップボードの貼り付け](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms571318(v=vs.90))|  
|ストロークのコレクションのカスタムプロパティにアクセスする|<xref:System.Windows.Ink.StrokeCollection.AddPropertyData%2A> (プロパティは内部的に格納され、<xref:System.Windows.Ink.StrokeCollection.AddPropertyData%2A>、<xref:System.Windows.Ink.StrokeCollection.RemovePropertyData%2A>、および <xref:System.Windows.Ink.StrokeCollection.ContainsPropertyData%2A>を介してアクセスされます)|[Microsoft Ink プロパティ](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms582214(v=vs.90))を使用する|  
  
### <a name="sharing-ink-between-platforms"></a>プラットフォーム間でインクを共有する  
 プラットフォームにはインクデータ用の異なるオブジェクトモデルがありますが、プラットフォーム間でデータを共有するのは非常に簡単です。 次の例では、Windows フォームアプリケーションからインクを保存し、Windows Presentation Foundation アプリケーションにインクを読み込みます。  
  
 [!code-csharp[WinFormWPFInk#UsingWinforms](~/samples/snippets/csharp/VS_Snippets_Wpf/WinformWPFInk/CSharp/Program.cs#usingwinforms)]
 [!code-vb[WinFormWPFInk#UsingWinforms](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WinformWPFInk/VisualBasic/Module1.vb#usingwinforms)]  
[!code-csharp[WinFormWPFInk#SaveWinforms](~/samples/snippets/csharp/VS_Snippets_Wpf/WinformWPFInk/CSharp/Program.cs#savewinforms)]
[!code-vb[WinFormWPFInk#SaveWinforms](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WinformWPFInk/VisualBasic/Module1.vb#savewinforms)]  
  
 [!code-csharp[WinFormWPFInk#UsingWPF](~/samples/snippets/csharp/VS_Snippets_Wpf/WinformWPFInk/CSharp/Program.cs#usingwpf)]
 [!code-vb[WinFormWPFInk#UsingWPF](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WinformWPFInk/VisualBasic/Module1.vb#usingwpf)]  
[!code-csharp[WinFormWPFInk#LoadWPF](~/samples/snippets/csharp/VS_Snippets_Wpf/WinformWPFInk/CSharp/Program.cs#loadwpf)]
[!code-vb[WinFormWPFInk#LoadWPF](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WinformWPFInk/VisualBasic/Module1.vb#loadwpf)]  
  
 次の例では、Windows Presentation Foundation アプリケーションからインクを保存し、Windows フォームアプリケーションにインクを読み込みます。  
  
 [!code-csharp[WinFormWPFInk#UsingWPF](~/samples/snippets/csharp/VS_Snippets_Wpf/WinformWPFInk/CSharp/Program.cs#usingwpf)]
 [!code-vb[WinFormWPFInk#UsingWPF](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WinformWPFInk/VisualBasic/Module1.vb#usingwpf)]  
[!code-csharp[WinFormWPFInk#SaveWPF](~/samples/snippets/csharp/VS_Snippets_Wpf/WinformWPFInk/CSharp/Program.cs#savewpf)]
[!code-vb[WinFormWPFInk#SaveWPF](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WinformWPFInk/VisualBasic/Module1.vb#savewpf)]  
  
 [!code-csharp[WinFormWPFInk#UsingWinforms](~/samples/snippets/csharp/VS_Snippets_Wpf/WinformWPFInk/CSharp/Program.cs#usingwinforms)]
 [!code-vb[WinFormWPFInk#UsingWinforms](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WinformWPFInk/VisualBasic/Module1.vb#usingwinforms)]  
[!code-csharp[WinFormWPFInk#LoadWinforms](~/samples/snippets/csharp/VS_Snippets_Wpf/WinformWPFInk/CSharp/Program.cs#loadwinforms)]
[!code-vb[WinFormWPFInk#LoadWinforms](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WinformWPFInk/VisualBasic/Module1.vb#loadwinforms)]
## <a name="events-from-the-tablet-pen"></a>タブレットペンからのイベント  

 Windows フォームと COM プラットフォーム上の[InkCollector](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583683(v=vs.90))および[microsoft は、](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms552322(v=vs.90))ユーザーがペンデータを入力したときに[イベントを受信](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583740(v=vs.90))することができます。 InkCollector[は、ウィンドウ](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms552322(v=vs.90))またはコントロール[Microsoft.Ink.InkCollector](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms583683(v=vs.90))にアタッチされ、タブレット入力データによって生成されたイベントをサブスクライブできます。 これらのイベントが発生するスレッドは、ペン、マウス、またはプログラムによってイベントが発生したかどうかによって異なります。 これらのイベントに関連したスレッド処理の詳細については、「[イベントが発生](/windows/desktop/tablet/threads-on-which-an-event-can-fire)する[一般的なスレッド処理の考慮事項](/windows/desktop/tablet/general-threading-considerations)とスレッド」を参照してください。  
  
 Windows Presentation Foundation プラットフォームでは、<xref:System.Windows.UIElement> クラスにはペン入力のイベントがあります。 これは、すべてのコントロールがスタイラスイベントの完全なセットを公開することを意味します。  スタイラスイベントは、トンネルイベントとバブルイベントのペアを持ち、常にアプリケーションスレッド上で発生します。  詳細については、「[ルーティングイベントの概要](routed-events-overview.md)」を参照してください。  
  
 次の図は、スタイラスイベントを発生させるクラスのオブジェクトモデルを比較したものです。 Windows Presentation Foundation オブジェクトモデルでは、対応するトンネリングイベントだけではなく、バブルイベントのみが表示されます。  
  
 ![WPF と Winforms のスタイラスイベントの図。](./media/ink-stylusevents.png "Ink_StylusEvents")  
  
## <a name="pen-data"></a>ペンデータ  
 3つのすべてのプラットフォームで、タブレットペンから取得したデータをインターセプトして操作する方法が提供されます。  Windows フォームと COM プラットフォームでは、 [StylusInput](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms585724(v=vs.90))を作成し、StylusInput またはコントロールをアタッチし、 [IStylusSyncPlugin](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms575201(v=vs.90))または[StylusInput](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms575194(v=vs.90))インターフェイスを実装するクラスを作成することによってこれを実現します。 その後、カスタムプラグインが[StylusInput](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms585724(v=vs.90))のプラグインコレクションに追加されています。 このオブジェクトモデルの詳細については、「 [StylusInput api のアーキテクチャ](/windows/desktop/tablet/architecture-of-the-stylusinput-apis)」を参照してください。  
  
 WPF プラットフォームでは、<xref:System.Windows.UIElement> クラスは、 [StylusInput](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms585724(v=vs.90))と同じように設計されているプラグインのコレクションを公開します。  ペンデータを受け取るには、<xref:System.Windows.Input.StylusPlugIns.StylusPlugIn> から継承するクラスを作成し、そのオブジェクトを <xref:System.Windows.UIElement>の <xref:System.Windows.UIElement.StylusPlugIns%2A> コレクションに追加します。 この相互作用の詳細については、「[スタイラスからの入力のインターセプト](intercepting-input-from-the-stylus.md)」を参照してください。  
  
 すべてのプラットフォームで、スレッドプールはスタイラスイベントを使用してインクデータを受け取り、それをアプリケーションスレッドに送信します。  COM プラットフォームと Windows プラットフォームでのスレッド処理の詳細については、「 [StylusInput api のスレッド処理に関する考慮事項](/windows/desktop/tablet/threading-considerations-for-the-stylusinput-apis)」を参照してください。  Windows プレゼンテーションソフトウェアでのスレッド処理の詳細については、「[インクスレッドモデル](the-ink-threading-model.md)」を参照してください。  
  
 次の図は、ペンスレッドプールでペンデータを受け取るクラスのオブジェクトモデルを比較しています。  
  
 ![StylusPlugin モデル WPF vs Winforms の図。](./media/ink-stylusplugins.png "Ink_StylusPlugins")
