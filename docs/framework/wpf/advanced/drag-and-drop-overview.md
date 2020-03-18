---
title: ドラッグ アンド ドロップの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- drag-and-drop [WPF], implementing
- drag sources [WPF], drag-and-drop
- data transfer [WPF], drag-and-drop
- drag-and-drop [WPF], about
- drag-and-drop [WPF], events
- drop targets [WPF], drag-and-drop
ms.assetid: 1a5b27b0-0ac5-4cdf-86c0-86ac0271fa64
ms.openlocfilehash: 72dc443e5653b9871c3f67b003bd1af0536d5993
ms.sourcegitcommit: 9c3a4f2d3babca8919a1e490a159c1500ba7a844
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/12/2019
ms.locfileid: "72291475"
---
# <a name="drag-and-drop-overview"></a>ドラッグ アンド ドロップの概要
このトピックでは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] アプリケーションでのドラッグ アンド ドロップのサポートの概要について説明します。 一般的に、ドラッグ アンド ドロップとは、マウス (または何らかのポインティング デバイス) を使用して 1 つ以上のオブジェクトを選択し、これらのオブジェクトを [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] の目的のドロップ先までドラッグしてからドロップするデータ転送方式をいいます。  

<a name="Drag_and_Drop_Support"></a>   
## <a name="drag-and-drop-support-in-wpf"></a>WPF でのドラッグ アンド ドロップのサポート  
 ドラッグ アンド ドロップ操作には、一般に、ドラッグするオブジェクトがドラッグを始めるドラッグ元と、ドロップするオブジェクトを受け取るドロップ先という 2 者があります。  ドラッグ元とドロップ先は、同じアプリケーションまたは異なるアプリケーションの UI 要素になることがあります。  
  
 ドラッグ アンド ドロップで操作できるオブジェクトの種類と数は、まったく任意です。 たとえば、ファイル、フォルダー、およびコンテンツの選択項目は、ドラッグ アンド ドロップ操作を介して操作する、より一般的なオブジェクトの一部です。  
  
 ドラッグ アンド ドロップ操作中に実行される特定の操作はアプリケーション固有で、多くの場合コンテキストによって決定されます。  たとえば、選択したファイルを同じストレージデバイス上のフォルダーから別のフォルダーにドラッグすると、既定でファイルが移動されます。一方、汎用名前付け規則 (UNC) 共有からローカルフォルダーにファイルをドラッグすると、既定でファイルがコピーされます。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] が提供するドラッグ アンド ドロップ機能は、さまざまなドラッグ アンド ドロップのシナリオをサポートするよう、非常に柔軟かつカスタマイズできるように設計されています。  ドラッグ アンド ドロップでは、1 つのアプリケーション内で、または異なるアプリケーションの間でのオブジェクトの操作をサポートします。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションとその他の Windows アプリケーションの間でのドラッグアンドドロップも完全にサポートされています。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、任意の <xref:System.Windows.UIElement> または <xref:System.Windows.ContentElement> がドラッグ アンド ドロップに参加できます。 ドラッグ アンド ドロップ操作に必要なイベントとメソッドは、<xref:System.Windows.DragDrop> クラスで定義されています。 <xref:System.Windows.UIElement> と <xref:System.Windows.ContentElement> クラスには、<xref:System.Windows.DragDrop> のアタッチ済みのイベントのエイリアスが含まれています。これにより、<xref:System.Windows.UIElement> や <xref:System.Windows.ContentElement> が基本要素として継承されるときに、イベントがクラスのメンバーとして表示されます。 これらのイベントにアタッチされたイベント ハンドラーは、基になる <xref:System.Windows.DragDrop> のアタッチ済みのイベントにアタッチされ、同じイベント データのインスタンスを受信します。 詳細については、<xref:System.Windows.UIElement.Drop?displayProperty=nameWithType> イベントを参照してください。  
  
> [!IMPORTANT]
> OLE のドラッグ アンド ドロップは、インターネット ゾーンにある間は機能しません。  
  
<a name="Data_Transfer"></a>   
## <a name="data-transfer"></a>データ転送  
 ドラッグ アンド ドロップは、より一般的な領域のデータ転送の一部です。 データ転送には、ドラッグ アンド ドロップ操作およびコピーと貼り付けの操作があります。 ドラッグ アンド ドロップ操作は、システムのクリップボードを使用して、あるオブジェクトまたはアプリケーションから別のオブジェクトまたはアプリケーションへのデータ転送に使用する、コピーと貼り付け操作または切り取りと貼り付け操作に似ています。 いずれの種類の操作でも、次のものが必要です。  
  
- データを提供するソース オブジェクト。  
  
- 転送されたデータを一時的に格納する方法。  
  
- データを受け取るターゲット オブジェクト。  
  
 コピーと貼り付け操作では、システムのクリップボードを使用して、転送されたデータを一時的に保存します。ドラッグ アンド ドロップ操作では、<xref:System.Windows.DataObject> を使用してデータを格納します。 データ オブジェクトは、概念的に、実際のデータと対応するデータ形式の識別子を格納する 1 組以上の <xref:System.Object> で構成されています。  
  
 ドラッグ元では、静的な <xref:System.Windows.DragDrop.DoDragDrop%2A?displayProperty=nameWithType> メソッドを呼び出して、転送するデータをそのメソッドに渡すことでドラッグ アンド ドロップ操作を開始します。 必要な場合、<xref:System.Windows.DragDrop.DoDragDrop%2A> メソッドは <xref:System.Windows.DataObject> にデータを自動的にラップします。 データ形式の制御を拡大するには、データを <xref:System.Windows.DataObject> メソッドに渡す前に <xref:System.Windows.DragDrop.DoDragDrop%2A> にラップします。 ドロップ先は、<xref:System.Windows.DataObject> からデータを抽出する役割を務めます。 データ オブジェクトの操作の詳細については、「[データとデータ オブジェクト](data-and-data-objects.md)」を参照してください。  
  
 ドラッグ アンド ドロップ操作のソースとターゲットは UI 要素ですが、一般に、実際に転送されているデータには視覚的表現はありません。 Windows エクスプローラーでファイルをドラッグするときに起こるような視覚的表現を、ドラッグするデータで実行するように、コードを記述することができます。 既定では、データを移動するのかコピーするのかなど、ドラッグ アンド ドロップ操作によりデータに起こる効果を表すようにカーソルを変更して、ユーザーにフィードバックします。  
  
### <a name="drag-and-drop-effects"></a>ドラッグ アンド ドロップの効果  
 ドラッグ アンド ドロップ操作には、転送するデータにさまざまな効果を持たせることができます。 たとえば、データをコピーしたり、データを移動したりできるなどです。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、ドラッグアンドドロップ操作の効果を指定するために使用できる <xref:System.Windows.DragDropEffects> 列挙体を定義します。 ドラッグ元では、ソースが <xref:System.Windows.DragDrop.DoDragDrop%2A> メソッドで許可する効果を指定できます。 ドロップ先では、<xref:System.Windows.DragEventArgs.Effects%2A> クラスの <xref:System.Windows.DragEventArgs> プロパティでターゲットの目的の効果を指定できます。 <xref:System.Windows.DragDrop.DragOver> イベントでドロップ先の目的の効果を指定した場合、その情報が <xref:System.Windows.DragDrop.GiveFeedback> イベントのドラッグ元に渡されます。 ドラッグ元では、この情報を使用して、ドロップ先がデータにどのような効果を起こそうとしているかをユーザーに伝えます。 データがドロップされると、ドロップ先では <xref:System.Windows.DragDrop.Drop> イベントでの実際の効果を指定します。 この情報は、<xref:System.Windows.DragDrop.DoDragDrop%2A> メソッドの戻り値としてドラッグ元に渡されます。 ドラッグ元の `allowedEffects` の一覧にない効果をドロップ先が返す場合、ドラッグ アンド ドロップ操作は取り消され、データ転送は発生しません。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、ドラッグ アンド ドロップ操作の効果に関しては、<xref:System.Windows.DragDropEffects> 値はドラッグ元とドロップ先間の通信にのみ使用されるということを覚えておくのは重要です。 ドラッグ アンド ドロップ操作の実際の効果は、アプリケーションで適切なコードを記述することに依存します。  
  
 たとえば、ドロップ先では、ドロップ先にデータをドロップすることはデータが移動することと指定される可能性があります。 ただし、データを移動するには、データがターゲット要素に追加されるとともに、ソース要素から削除される必要があります。 ソース要素は、データの移動を許可していますが、ソース要素からデータを削除するコードを指定しない場合、最終的にデータは移動ではなく、コピーされるという結果になります。  
  
<a name="Drag_and_Drop_Events"></a>   
## <a name="drag-and-drop-events"></a>ドラッグ アンド ドロップのイベント  
 ドラッグ アンド ドロップの操作は、イベント ドリブン モデルをサポートしています。  ドラッグ元とドロップ先の両方でイベントの標準セットを使用して、ドラッグ アンド ドロップの操作を処理します。  次の表は、標準のドラッグ アンド ドロップのイベントをまとめたものです。 これらは、<xref:System.Windows.DragDrop> クラスでアタッチされるイベントです。 アタッチされるイベントの詳細については、「[添付イベントの概要](attached-events-overview.md)」を参照してください。  
  
### <a name="drag-source-events"></a>ドラッグ元のイベント  
  
|イベント|概要|  
|-----------|-------------|  
|<xref:System.Windows.DragDrop.GiveFeedback>|このイベントは、ドラッグ アンド ドロップ操作中に継続的に発生し、ドロップ ソースがユーザーに情報をフィードバックできるようにします。 このフィードバックは、通常、マウス ポインターの外観を変えて、ドロップ先が許可する効果を示すことで実行されます。  これは、 バブリング イベントです。|  
|<xref:System.Windows.DragDrop.QueryContinueDrag>|このイベントは、ドラッグ アンド ドロップ操作時にキーボードやマウス ボタンの状態が変化し、ドロップ ソースがキーやボタンの状態によってドラッグ アンド ドロップ操作の取り消しができるようになったときに発生します。 これは、 バブリング イベントです。|  
|<xref:System.Windows.DragDrop.PreviewGiveFeedback>|トンネリング バージョンの <xref:System.Windows.DragDrop.GiveFeedback> です。|  
|<xref:System.Windows.DragDrop.PreviewQueryContinueDrag>|トンネリング バージョンの <xref:System.Windows.DragDrop.QueryContinueDrag> です。|  
  
### <a name="drop-target-events"></a>ドロップ先のイベント  
  
|イベント|概要|  
|-----------|-------------|  
|<xref:System.Windows.DragDrop.DragEnter>|このイベントは、オブジェクトがドロップ先の境界の中にドラッグされるときに発生します。 これは、 バブリング イベントです。|  
|<xref:System.Windows.DragDrop.DragLeave>|このイベントは、オブジェクトがドロップ先の境界の外にドラッグされるときに発生します。  これは、 バブリング イベントです。|  
|<xref:System.Windows.DragDrop.DragOver>|このイベントは、オブジェクトがドロップ先の境界内でドラッグ (移動) する間、継続的に発生します。 これは、 バブリング イベントです。|  
|<xref:System.Windows.DragDrop.Drop>|このイベントは、オブジェクトがドロップ先にドロップするときに発生します。  これは、 バブリング イベントです。|  
|<xref:System.Windows.DragDrop.PreviewDragEnter>|トンネリング バージョンの <xref:System.Windows.DragDrop.DragEnter> です。|  
|<xref:System.Windows.DragDrop.PreviewDragLeave>|トンネリング バージョンの <xref:System.Windows.DragDrop.DragLeave> です。|  
|<xref:System.Windows.DragDrop.PreviewDragOver>|トンネリング バージョンの <xref:System.Windows.DragDrop.DragOver> です。|  
|<xref:System.Windows.DragDrop.PreviewDrop>|トンネリング バージョンの <xref:System.Windows.DragDrop.Drop> です。|  
  
 オブジェクトのインスタンスのドラッグ アンド ドロップ イベントを処理するには、前述の表に一覧表示されたイベントにハンドラーを追加します。 クラス レベルでドラッグ アンド ドロップ イベントを処理するには、対応する仮想 On*Event イベントおよび On\*PreviewEvent メソッドをオーバーライドします。 詳細については、「[制御の基底クラスによるルーティング イベントのクラスの処理](marking-routed-events-as-handled-and-class-handling.md#Class_Handling_of_Routed_Events)」を参照してください。  
  
<a name="Implementing_Drag_And_Drop"></a>   
## <a name="implementing-drag-and-drop"></a>ドラッグ アンド ドロップの実装  
 UI 要素は、ドラッグ元、ドロップ先、またはその両方になれます。 基本的なドラッグ アンド ドロップを実装するには、ドラッグ アンド ドロップ操作を開始して、ドロップしたデータを処理するコードを記述します。 オプションのドラッグ アンド ドロップ イベントを処理すると、ドラッグ アンド ドロップの操作性を拡張できます。  
  
 基本的なドラッグ アンド ドロップを実装するには、次のタスクを実行します。  
  
- ドラッグ元となる要素を特定します。 ドラッグ元には <xref:System.Windows.UIElement> または <xref:System.Windows.ContentElement> を指定できます。  
  
- ドラッグ アンド ドロップ操作を開始するドラッグ元のイベント ハンドラーを作成します。 通常、イベントは <xref:System.Windows.UIElement.MouseMove> イベントです。  
  
- ドラッグ アンド ドロップ操作を開始するには、ドラッグ元のイベント ハンドラーで <xref:System.Windows.DragDrop.DoDragDrop%2A> メソッドを呼び出します。 <xref:System.Windows.DragDrop.DoDragDrop%2A> の呼び出しで、ドラッグ元、転送するデータ、および許可される効果を指定します。  
  
- ドロップ先となる要素を特定します。 ドロップ先には、<xref:System.Windows.UIElement> または <xref:System.Windows.ContentElement> を指定できます。  
  
- ドロップ先で、<xref:System.Windows.UIElement.AllowDrop%2A> プロパティを `true` に設定します。  
  
- ドロップ先で、ドロップしたデータを処理する <xref:System.Windows.DragDrop.Drop> イベント ハンドラーを作成します。  
  
- <xref:System.Windows.DragDrop.Drop> イベント ハンドラーで、<xref:System.Windows.DragEventArgs> メソッドと <xref:System.Windows.DataObject.GetDataPresent%2A> メソッドを使用して、<xref:System.Windows.DataObject.GetData%2A> からデータを抽出します。  
  
- <xref:System.Windows.DragDrop.Drop> イベント ハンドラーで、データを使用して、目的のドラッグ アンド ドロップ操作を実行します。  
  
 次のタスクに示すように、カスタム <xref:System.Windows.DataObject> を作成し、オプションのドラッグ元とドロップ先のイベントを処理すると、ドラッグ アンド ドロップの実装を拡張できます。  
  
- カスタム データまたは複数のデータ項目を転送するには、<xref:System.Windows.DataObject> メソッドに渡す <xref:System.Windows.DragDrop.DoDragDrop%2A> を作成します。  
  
- ドラッグ中に他の操作を実行するには、ドロップ先で <xref:System.Windows.DragDrop.DragEnter>、<xref:System.Windows.DragDrop.DragOver>、および <xref:System.Windows.DragDrop.DragLeave> イベントを処理します。  
  
- マウス ポインターの外観を変更するには、ドラッグ元で <xref:System.Windows.DragDrop.GiveFeedback> イベントを処理します。  
  
- ドラッグ アンド ドロップ操作の取り消し方法を変更するには、ドラッグ元で <xref:System.Windows.DragDrop.QueryContinueDrag> イベントを処理します。  
  
<a name="Drag_And_Drop_Example"></a>   
## <a name="drag-and-drop-example"></a>ドラッグ アンド ドロップの例  
 このセクションでは、<xref:System.Windows.Shapes.Ellipse> 要素のドラッグ アンド ドロップを実装する方法について説明します。 <xref:System.Windows.Shapes.Ellipse> はドラッグ元とドロップ先の両方です。 転送されるデータは、楕円の <xref:System.Windows.Shapes.Shape.Fill%2A> プロパティの文字列表現です。 次の XAML は、<xref:System.Windows.Shapes.Ellipse> 要素と、XAML が処理するドラッグ アンド ドロップ関連のイベントを示しています。 ドラッグ アンド ドロップの実装方法の完全な手順については、「[チュートリアル: ユーザー コントロールでドラッグ アンド ドロップを有効にする](walkthrough-enabling-drag-and-drop-on-a-user-control.md)」を参照してください。  
  
 [!code-xaml[DragDropSnippets#EllipseXaml](~/samples/snippets/csharp/VS_Snippets_Wpf/dragdropsnippets/cs/mainwindow.xaml#ellipsexaml)]  
  
### <a name="enabling-an-element-to-be-a-drag-source"></a>要素がドラッグ元になるようにする  
 ドラッグ元であるオブジェクトは次の役割を務めます。  
  
- いつドラッグが発生するかを識別する。  
  
- ドラッグ アンド ドロップ操作を開始する。  
  
- 転送するデータを特定する。  
  
- ドラッグ アンド ドロップ操作によってデータ転送時に起こすことができる効果を指定する。  
  
 ドラッグ元には、許可される操作 (移動、コピー、なし) に関してユーザーにフィードバックすることもできます。また、ドラッグ中に Esc キーを押すなどのユーザーの追加の入力に基づいて、ドラッグ アンド ドロップ操作を取り消すことができます。  
  
 いつドラッグが発生したかを判断してから、<xref:System.Windows.DragDrop.DoDragDrop%2A> メソッドを呼び出してドラッグ アンド ドロップ操作を開始することは、アプリケーションの役割です。 通常、これはマウス ボタンが押されている間に、要素上で <xref:System.Windows.UIElement.MouseMove> イベントが発生してドラッグする場合です。 次の例は、<xref:System.Windows.UIElement.MouseMove> 要素の <xref:System.Windows.Shapes.Ellipse> イベント ハンドラーからドラッグ アンド ドロップ操作を開始してドラッグ元にする方法を示しています。 転送されるデータは、楕円の <xref:System.Windows.Shapes.Shape.Fill%2A> プロパティの文字列表現です。  
  
 [!code-csharp[DragDropSnippets#DoDragDrop](~/samples/snippets/csharp/VS_Snippets_Wpf/dragdropsnippets/cs/mainwindow.xaml.cs#dodragdrop)]
 [!code-vb[DragDropSnippets#DoDragDrop](~/samples/snippets/visualbasic/VS_Snippets_Wpf/dragdropsnippets/vb/mainwindow.xaml.vb#dodragdrop)]  
  
 <xref:System.Windows.UIElement.MouseMove> イベント ハンドラー内で、<xref:System.Windows.DragDrop.DoDragDrop%2A> メソッドを呼び出して、ドラッグ アンド ドロップ操作を開始します。 <xref:System.Windows.DragDrop.DoDragDrop%2A> メソッドは 3 つのパラメーターを受け取ります。  
  
- `dragSource` –転送されたデータのソースである依存関係オブジェクトへの参照。これは、通常、<xref:System.Windows.UIElement.MouseMove> イベントのソースです。  
  
- `data`-<xref:System.Windows.DataObject>にラップされた、転送されたデータを格納しているオブジェクト。  
  
- `allowedEffects`-ドラッグアンドドロップ操作の許可された効果を指定する <xref:System.Windows.DragDropEffects> 列挙値の1つ。  
  
 シリアル化可能なオブジェクトはすべて `data` パラメーターに渡すことができます。 データがまだ <xref:System.Windows.DataObject> にラップされていない場合、データは新しい <xref:System.Windows.DataObject> に自動的にラップされます。 複数のデータ項目を渡すには、自分で <xref:System.Windows.DataObject> を作成し、<xref:System.Windows.DragDrop.DoDragDrop%2A> メソッドに渡す必要があります。 詳細については、「[データとデータ オブジェクト](data-and-data-objects.md)」を参照してください。  
  
 `allowedEffects` パラメーターを使用すると、ドラッグ元が、ドロップ先に対して、データの転送時に何を行えるかを指定できます。 ドラッグ元の一般的な値は <xref:System.Windows.DragDropEffects.Copy>、<xref:System.Windows.DragDropEffects.Move>、および <xref:System.Windows.DragDropEffects.All> です。  
  
> [!NOTE]
> ドロップ先でも、ドロップされたデータに対応してどのような効果を意図するか指定できます。 たとえば、ドロップ先がドロップするデータ型を認識しない場合、許可された効果を <xref:System.Windows.DragDropEffects.None> に設定してデータを拒否することができます。 これは通常、 <xref:System.Windows.DragDrop.DragOver> イベント ハンドラーで実行します。  
  
 ドラッグ元は、オプションで <xref:System.Windows.DragDrop.GiveFeedback> イベントと <xref:System.Windows.DragDrop.QueryContinueDrag> イベントを処理できます。 これらのイベントには、イベントを処理済みとマークしない限り使用される既定のハンドラーがあります。 既定の動作を変更する特定のニーズがない限り、通常はこれらのイベントを無視します。  
  
 ドラッグ元がドラッグされている間、<xref:System.Windows.DragDrop.GiveFeedback> イベントは継続的に発生します。 このイベントの既定のハンドラーは、ドラッグ元が有効なドロップ先の上にドラッグしているかどうかを確認します。 有効である場合は、このハンドラーは、ドロップ先の許可された効果を確認します。 その後、許可されているドロップ効果に関してエンドユーザーにフィードバックします。 通常は、マウス カーソルを非ドロップ、コピー、または移動のカーソルに変更してこれを実行します。 カスタムのカーソルを使用してユーザーにフィードバックする必要がある場合のみ、このイベントを処理する必要があります。 このイベントを処理する場合は、既定のハンドラーが自作のハンドラーをオーバーライドしないよう、必ずイベントを処理済みとマークしてください。  
  
 ドラッグ元がドラッグされている間、<xref:System.Windows.DragDrop.QueryContinueDrag> イベントは継続的に発生します。 このイベントを処理すると、Esc、Shift、Ctrl、および Alt の各キーの状態に加えて、マウス ボタンの状態に基づいて、どのアクションがドラッグ アンド ドロップ操作を終了させるかを決定することができます。 このイベントの既定のハンドラーは、Esc キーを押した場合はドラッグ アンド ドロップ操作を取り消し、マウス ボタンを放した場合はデータをドロップします。  
  
> [!CAUTION]
> これらのイベントは、ドラッグ アンド ドロップ操作中に継続的に発生します。 そのため、イベント ハンドラーではリソースを集中的に使用するタスクを避ける必要があります。  たとえば、<xref:System.Windows.DragDrop.GiveFeedback> イベントが発生するたびに新しいカーソルを作成するのではなく、キャッシュされたカーソルを使用します。  
  
### <a name="enabling-an-element-to-be-a-drop-target"></a>要素がドロップ先になるようにする  
 ドロップ先であるオブジェクトは次の役割を務めます。  
  
- 有効なドロップ先であることを指定する。  
  
- ドラッグ元に対して、ターゲットの上にドラッグしたときに応答する。  
  
- 転送されたデータが受信できる形式であることを確認する。  
  
- ドロップしたデータを処理する。  
  
 要素がドロップ先であることを指定するには、<xref:System.Windows.UIElement.AllowDrop%2A> プロパティを `true` に設定します。 ドロップ先のイベントが要素で発生して、処理できるようになります。 ドラッグ アンド ドロップ操作中に、次の一連のイベントがドロップ先で発生します。  
  
1. <xref:System.Windows.DragDrop.DragEnter>  
  
2. <xref:System.Windows.DragDrop.DragOver>  
  
3. <xref:System.Windows.DragDrop.DragLeave> または <xref:System.Windows.DragDrop.Drop>  
  
 <xref:System.Windows.DragDrop.DragEnter> イベントは、データがドロップ先の境界の中にドラッグされるときに発生します。 通常は、アプリケーションに適していれば、ドラッグ アンド ドロップ操作の効果のプレビューを提供するように、このイベントを処理します。 <xref:System.Windows.DragEventArgs.Effects%2A?displayProperty=nameWithType> イベントで <xref:System.Windows.DragDrop.DragEnter> プロパティを設定しないでください。このプロパティは <xref:System.Windows.DragDrop.DragOver> イベントで上書きされるためです。  
  
 次の例は、<xref:System.Windows.DragDrop.DragEnter> 要素の <xref:System.Windows.Shapes.Ellipse> イベント ハンドラーを示しています。 このコードでは、現在の <xref:System.Windows.Shapes.Shape.Fill%2A> ブラシを保存して、ドラッグ アンド ドロップ操作の効果をプレビューします。 続いて、<xref:System.Windows.DataObject.GetDataPresent%2A> メソッドを使用して、楕円の上にドラッグした <xref:System.Windows.DataObject> に、<xref:System.Windows.Media.Brush> に変換できる文字列データが含まれているかどうかを確認します。 含まれている場合、データは <xref:System.Windows.DataObject.GetData%2A> メソッドによって抽出されます。 その後、データは <xref:System.Windows.Media.Brush> に変換されて、楕円に適用されます。 変更は <xref:System.Windows.DragDrop.DragLeave> イベント ハンドラーで元に戻ります。 データが <xref:System.Windows.Media.Brush> に変換できない場合、アクションは実行されません。  
  
 [!code-csharp[DragDropSnippets#DragEnter](~/samples/snippets/csharp/VS_Snippets_Wpf/dragdropsnippets/cs/mainwindow.xaml.cs#dragenter)]
 [!code-vb[DragDropSnippets#DragEnter](~/samples/snippets/visualbasic/VS_Snippets_Wpf/dragdropsnippets/vb/mainwindow.xaml.vb#dragenter)]  
  
 <xref:System.Windows.DragDrop.DragOver> イベントは、データがドロップ先の上にドラッグされる間、継続的に発生します。 このイベントはドラッグ元の <xref:System.Windows.DragDrop.GiveFeedback> イベントとペアになります。 <xref:System.Windows.DragDrop.DragOver> イベント ハンドラーでは、通常は <xref:System.Windows.DataObject.GetDataPresent%2A> メソッドと <xref:System.Windows.DataObject.GetData%2A> メソッドを使用して、転送されたデータがドロップ先で処理できる形式であるかどうかを確認します。 また、いずれかの修飾キーが押されたかどうかも確認できます。修飾キーは、通常、ユーザーが移動またはコピー操作を意図しているかどうかを示します。 これらの確認を行った後、<xref:System.Windows.DragEventArgs.Effects%2A?displayProperty=nameWithType> プロパティを、データをドロップするとどのような効果が起こるかをドラッグ元に通知するように設定します。 ドラッグ元は、<xref:System.Windows.DragDrop.GiveFeedback> のイベント引数でこの情報を受信し、適切なカーソルを設定してユーザーにフィードバックします。  
  
 次の例は、<xref:System.Windows.DragDrop.DragOver> 要素の <xref:System.Windows.Shapes.Ellipse> イベント ハンドラーを示しています。 このコードは、楕円の上にドラッグされている <xref:System.Windows.DataObject> に、<xref:System.Windows.Media.Brush> に変換できる文字列データが含まれているかどうかを確認します。 含まれている場合、<xref:System.Windows.DragEventArgs.Effects%2A?displayProperty=nameWithType> プロパティを <xref:System.Windows.DragDropEffects.Copy> に設定します。 これにより、ドラッグ元に対して、データが楕円にコピーできることを示します。 データが <xref:System.Windows.Media.Brush> に変換できない場合、<xref:System.Windows.DragEventArgs.Effects%2A?displayProperty=nameWithType> プロパティは <xref:System.Windows.DragDropEffects.None> に設定されます。 これにより、ドラッグ元に対して、楕円はデータの有効なドロップ先ではないことを示します。  
  
 [!code-csharp[DragDropSnippets#DragOver](~/samples/snippets/csharp/VS_Snippets_Wpf/dragdropsnippets/cs/mainwindow.xaml.cs#dragover)]
 [!code-vb[DragDropSnippets#DragOver](~/samples/snippets/visualbasic/VS_Snippets_Wpf/dragdropsnippets/vb/mainwindow.xaml.vb#dragover)]  
  
 <xref:System.Windows.DragDrop.DragLeave> イベントは、データがドロップされることなくターゲットの境界の外にドラッグされるときに発生します。 このイベントを処理すると、<xref:System.Windows.DragDrop.DragEnter> イベント ハンドラーで行ったことをすべて元に戻すことができます。  
  
 次の例は、<xref:System.Windows.DragDrop.DragLeave> 要素の <xref:System.Windows.Shapes.Ellipse> イベント ハンドラーを示しています。 このコードは、保存された <xref:System.Windows.DragDrop.DragEnter> を楕円に適用することで、<xref:System.Windows.Media.Brush> イベント ハンドラーで行ったプレビュー内容を元に戻します。  
  
 [!code-csharp[DragDropSnippets#DragLeave](~/samples/snippets/csharp/VS_Snippets_Wpf/dragdropsnippets/cs/mainwindow.xaml.cs#dragleave)]
 [!code-vb[DragDropSnippets#DragLeave](~/samples/snippets/visualbasic/VS_Snippets_Wpf/dragdropsnippets/vb/mainwindow.xaml.vb#dragleave)]  
  
 <xref:System.Windows.DragDrop.Drop> イベントは、データがドロップ先にドロップされると発生します。既定では、これはマウス ボタンが放されたときに発生します。 <xref:System.Windows.DragDrop.Drop> イベント ハンドラーでは、<xref:System.Windows.DataObject.GetData%2A> メソッドを使用して <xref:System.Windows.DataObject> から転送されたデータを抽出し、アプリケーションが必要とするデータ処理を実行します。 <xref:System.Windows.DragDrop.Drop> イベントは、ドラッグ アンド ドロップ操作を終了します。  
  
 次の例は、<xref:System.Windows.DragDrop.Drop> 要素の <xref:System.Windows.Shapes.Ellipse> イベント ハンドラーを示しています。 このコードは、ドラッグ アンド ドロップ操作の効果を適用します。<xref:System.Windows.DragDrop.DragEnter> イベント ハンドラーのコードと似ています。 このコードは、楕円の上にドラッグされている <xref:System.Windows.DataObject> に、<xref:System.Windows.Media.Brush> に変換できる文字列データが含まれているかどうかを確認します。 含まれている場合は、 <xref:System.Windows.Media.Brush> は楕円に適用されます。 データが <xref:System.Windows.Media.Brush> に変換できない場合、アクションは実行されません。  
  
 [!code-csharp[DragDropSnippets#Drop](~/samples/snippets/csharp/VS_Snippets_Wpf/dragdropsnippets/cs/mainwindow.xaml.cs#drop)]
 [!code-vb[DragDropSnippets#Drop](~/samples/snippets/visualbasic/VS_Snippets_Wpf/dragdropsnippets/vb/mainwindow.xaml.vb#drop)]  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Clipboard>
- [チュートリアル: ユーザー コントロールでのドラッグ アンド ドロップの有効化](walkthrough-enabling-drag-and-drop-on-a-user-control.md)
- [方法のトピック](drag-and-drop-how-to-topics.md)
- [ドラッグ アンド ドロップ](drag-and-drop.md)
