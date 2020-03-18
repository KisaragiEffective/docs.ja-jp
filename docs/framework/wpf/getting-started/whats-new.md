---
title: WPF Version 4.5 の新機能
titleSuffix: ''
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Presentation Foundation [WPF], what's new
- WPF [WPF], what's new
ms.assetid: db086ae4-70bb-4862-95db-2eaca5216bc3
ms.openlocfilehash: 8eff8da7746047c450b2e23af63d43b13f3b970c
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746920"
---
# <a name="whats-new-in-wpf-version-45"></a>WPF Version 4.5 の新機能
<a name="introduction"></a>このトピックでは [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] バージョン4.5 の新機能と強化された機能について説明します。  
  
 このトピックには、次のセクションが含まれます。  
  
- [リボン コントロール](#ribbon_control)  
  
- [グループ化された大きなデータ セットを表示する際のパフォーマンスが向上](#grouped_virtualization)  
  
- [VirtualizingPanel の新機能](#VirtualizingPanel)  
  
- [静的プロパティへのバインド](#static_properties)  
  
- [非 UI スレッドでのコレクションへのアクセス](#xthread_access)  
  
- [データの同期および非同期検証](#INotifyDataErrorInfo)  
  
- [データ バインディングのソースの自動更新](#delay)  
  
- [ICustomTypeProvider を実装する型へのバインド](#ICustomTypeProvider)  
  
- [バインディング式からのデータ バインディング情報の取得](#binding_state)  
  
- [有効な DataContext オブジェクトの確認](#DisconnectedSource)  
  
- [データの値変更に伴うデータの再配置 (ライブ形成)](#live_shaping)  
  
- [イベントへの弱い参照確立のサポート強化](#weak_event_pattern)  
  
- [ディスパッチャー クラスの新しいメソッド](#async)  
  
- [イベントのマークアップ拡張機能](#events_markup_extenions)  
  
<a name="ribbon_control"></a>   
## <a name="ribbon-control"></a>リボン コントロール  
 WPF 4.5 には、クイック アクセス ツール バー、アプリケーション メニュー、タブをホストする <xref:System.Windows.Controls.Ribbon.Ribbon> コントロールが付属しています。  詳細については、「[リボンの概要](/visualstudio/vsto/ribbon-overview)」を参照してください。  
  
<a name="grouped_virtualization"></a>   
## <a name="improved-performance-when-displaying-large-sets-of-grouped-data"></a>グループ化された大きなデータ セットを表示する際のパフォーマンスが向上  
 UI の仮想化は、ユーザー インターフェイス (UI) 要素のサブセットが画面上に表示する項目に基づいて、より多くのデータ項目から生成されるときに行われます。 <xref:System.Windows.Controls.VirtualizingPanel> は、グループ化されたデータの UI の仮想化を有効にする <xref:System.Windows.Controls.VirtualizingPanel.IsVirtualizingWhenGrouping%2A> 添付プロパティを定義します。  データのグループ化の詳細については、「How to: Sort and Group Data Using a View in XAML」(方法: XAML のビューを使用したデータの並べ替えとグループ化) を参照してください。  グループ化されたデータの仮想化の詳細については、<xref:System.Windows.Controls.VirtualizingPanel.IsVirtualizingWhenGrouping%2A> 添付プロパティを参照してください。  
  
<a name="VirtualizingPanel"></a>   
## <a name="new-features-for-the-virtualizingpanel"></a>VirtualizingPanel の新機能  
  
1. <xref:System.Windows.Controls.VirtualizingPanel> 添付プロパティを指定することで、<xref:System.Windows.Controls.VirtualizingStackPanel> (<xref:System.Windows.Controls.VirtualizingPanel.ScrollUnit%2A> など) に部分的な項目を表示するかどうかを指定できます。 <xref:System.Windows.Controls.VirtualizingPanel.ScrollUnit%2A> が <xref:System.Windows.Controls.ScrollUnit.Item> に設定されている場合、<xref:System.Windows.Controls.VirtualizingPanel> には完全に表示される項目だけが表示されます。 <xref:System.Windows.Controls.VirtualizingPanel.ScrollUnit%2A> が <xref:System.Windows.Controls.ScrollUnit.Pixel> に設定されている場合、<xref:System.Windows.Controls.VirtualizingPanel> には部分的に表示される項目も表示されます。  
  
2. <xref:System.Windows.Controls.VirtualizingPanel> 添付プロパティを使用して <xref:System.Windows.Controls.VirtualizingPanel.CacheLength%2A> を仮想化すると、ビューポートの前後でキャッシュのサイズを指定できます。  キャッシュは、項目が仮想化されないビューポートの上または下の領域の量です。  キャッシュを使用して、UI 要素がビューにスクロールされたときに UI 要素が生成されないようすると、パフォーマンスが向上します。 キャッシュは、操作中にアプリケーションが応答を停止しないように低い優先度で実行されます。 <xref:System.Windows.Controls.VirtualizingPanel.CacheLengthUnit%2A?displayProperty=nameWithType> プロパティは、<xref:System.Windows.Controls.VirtualizingPanel.CacheLength%2A?displayProperty=nameWithType> により使用される測定単位を決定します。  
  
<a name="static_properties"></a>   
## <a name="binding-to-static-properties"></a>静的プロパティへのバインド  
 データ バインディングのソースとして静的プロパティを使用できます。 データ バインディング エンジンは、静的イベントが発生した場合にプロパティの値が変更されたことを認識します。  たとえば、`SomeClass` クラスが `MyProperty` という静的プロパティを定義している場合、`SomeClass` は `MyProperty` の値が変更されたときに発生する静的イベントを定義できます。  静的イベントは、次のいずれかのシグネチャを使用できます。  
  
- `public static event EventHandler MyPropertyChanged;`  
  
- `public static event EventHandler<PropertyChangedEventArgs> StaticPropertyChanged;`  
  
 最初のケースでは、クラスは、イベントハンドラーに <xref:System.EventArgs> を渡す*PropertyName*`Changed` という名前の静的イベントを公開していることに注意してください。  2 番目のケースでは、イベント ハンドラーに `StaticPropertyChanged` を渡す <xref:System.ComponentModel.PropertyChangedEventArgs> という名前の静的イベントがクラスにより公開されています。 静的プロパティを実装するクラスは、いずれかの方法を使用してプロパティ変更通知を生成することを選択できます。  
  
<a name="xthread_access"></a>   
## <a name="accessing-collections-on-non-ui-threads"></a>非 UI スレッドでのコレクションへのアクセス  
 WPF により、コレクションで作成されたスレッド以外のスレッドでデータ コレクションにアクセスして変更することが可能になります。  このため、バックグラウンド スレッドを使用して、データベースなどの外部ソースからデータを受信し、UI スレッドにデータを表示することができます。  別のスレッドを使用してコレクションを変更することで、ユーザー インターフェイスはユーザー操作への応答を維持します。  
  
<a name="INotifyDataErrorInfo"></a>   
## <a name="synchronously-and-asynchronously-validating-data"></a>データの同期および非同期検証  
 <xref:System.ComponentModel.INotifyDataErrorInfo> インターフェイスにより、データ エンティティ クラスは、ユーザー定義の検証規則を実装し、検証結果を非同期的に公開することができます。 このインターフェイスは、カスタム エラー オブジェクト、プロパティごとの複数のエラー、プロパティ間のエラー、およびエンティティ レベルのエラーもサポートします。  詳細については、<xref:System.ComponentModel.INotifyDataErrorInfo> を参照してください。  
  
<a name="delay"></a>   
## <a name="automatically-updating-the-source-of-a-data-binding"></a>データ バインディングのソースの自動更新  
 データ バインディングを使用してデータ ソースを更新する場合、<xref:System.Windows.Data.BindingBase.Delay%2A> プロパティを使用して、ターゲットでプロパティが変更されてからソースが更新されるまでの時間の長さを指定できます。  たとえば、<xref:System.Windows.Controls.Slider> プロパティ データがデータ オブジェクトのプロパティに双方向にバインドされた <xref:System.Windows.Controls.Primitives.RangeBase.Value%2A> があり、<xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> プロパティが <xref:System.Windows.Data.UpdateSourceTrigger.PropertyChanged> に設定されているとします。  この例では、ユーザーが <xref:System.Windows.Controls.Slider> を移動すると、<xref:System.Windows.Controls.Slider> が移動する各ピクセルのソースが更新されます。  <xref:System.Windows.Controls.Primitives.RangeBase.Value%2A> が変更を停止した場合、ソース オブジェクトには通常スライダーの値のみ必要です。  ソースがあまり頻繁に更新されないようにするには、<xref:System.Windows.Data.BindingBase.Delay%2A> を使用して、Thumb が移動を停止してから一定の時間が経過するまでソースが更新されないように指定します。  
  
<a name="ICustomTypeProvider"></a>   
## <a name="binding-to-types-that-implement-icustomtypeprovider"></a>ICustomTypeProvider を実装する型へのバインド  
 WPF では、<xref:System.Reflection.ICustomTypeProvider> を実装するオブジェクトへのデータ バインディングがサポートされます (カスタムの型とも呼ばれます)。  カスタムの型は、次の場合に使用できます。  
  
1. データ バインディングにおける <xref:System.Windows.PropertyPath> として。 たとえば、<xref:System.Windows.Data.Binding.Path%2A> の <xref:System.Windows.Data.Binding> プロパティは、カスタムの型のプロパティを参照できます。  
  
2. <xref:System.Windows.DataTemplate.DataType%2A> プロパティの値として。  
  
3. <xref:System.Windows.Controls.DataGrid> で自動的に生成された列を決定する型として。  
  
<a name="binding_state"></a>   
## <a name="retrieving-data-binding-information-from-a-binding-expression"></a>バインディング式からのデータ バインディング情報の取得  
 場合によっては、<xref:System.Windows.Data.BindingExpression> の <xref:System.Windows.Data.Binding> を取得することがあり、バインディングのソースとターゲットのオブジェクトに関する情報を必要となる可能性があります。  ソースまたはターゲットのオブジェクトや関連プロパティを取得できるように、新しい API が追加されました。  <xref:System.Windows.Data.BindingExpression> がある場合、次の API を使用してターゲットおよびソースに関する情報を取得します。  
  
|検出するバインディングの値|使用する API|  
|---------------------------------------|------------------|  
|ターゲット オブジェクト|<xref:System.Windows.Data.BindingExpressionBase.Target%2A?displayProperty=nameWithType>|  
|ターゲット プロパティ|<xref:System.Windows.Data.BindingExpressionBase.TargetProperty%2A?displayProperty=nameWithType>|  
|ソース オブジェクト|<xref:System.Windows.Data.BindingExpression.ResolvedSource%2A?displayProperty=nameWithType>|  
|ソース プロパティ|<xref:System.Windows.Data.BindingExpression.ResolvedSourcePropertyName%2A?displayProperty=nameWithType>|  
|<xref:System.Windows.Data.BindingExpression> が <xref:System.Windows.Data.BindingGroup> に属するかどうか|<xref:System.Windows.Data.BindingExpressionBase.BindingGroup%2A?displayProperty=nameWithType>|  
|<xref:System.Windows.Data.BindingGroup> の所有者|<xref:System.Windows.Data.BindingGroup.Owner%2A>|  
  
<a name="DisconnectedSource"></a>   
## <a name="checking-for-a-valid-datacontext-object"></a>有効な DataContext オブジェクトの確認  
 <xref:System.Windows.FrameworkElement.DataContext%2A> にある項目のコンテナーの <xref:System.Windows.Controls.ItemsControl> が切断されるケースがあります。  項目コンテナーは、<xref:System.Windows.Controls.ItemsControl> に項目を表示する UI 要素です。  <xref:System.Windows.Controls.ItemsControl> がコレクションにバインドされたデータの場合、項目コンテナーは項目ごとに生成されます。 場合によっては、項目コンテナーがビジュアル ツリーから削除されます。 項目コンテナーが削除される 2 つの一般的なケースは、項目が基になるコレクションから削除されたときと、仮想化が <xref:System.Windows.Controls.ItemsControl> で有効になっているときです。 このような場合、項目コンテナーの <xref:System.Windows.FrameworkElement.DataContext%2A> プロパティは、<xref:System.Windows.Data.BindingOperations.DisconnectedSource%2A?displayProperty=nameWithType> の静的プロパティから返されるオブジェクトのセンティネル オブジェクトに設定されます。  項目コンテナーの <xref:System.Windows.FrameworkElement.DataContext%2A> にアクセスする前に、<xref:System.Windows.Data.BindingOperations.DisconnectedSource%2A> が <xref:System.Windows.FrameworkElement.DataContext%2A> オブジェクトと等しいかどうかを確認する必要があります。  
  
<a name="live_shaping"></a>   
## <a name="repositioning-data-as-the-datas-values-change-live-shaping"></a>データの値変更に伴うデータの再配置 (ライブ形成)  
 データのコレクションは、グループ化、並べ替え、またはフィルター処理が可能です。 WPF 4.5 により、データが変更されたときの再配置が可能になります。 たとえば、アプリケーションが <xref:System.Windows.Controls.DataGrid> を使用して株式市場の株式を一覧表示し、株式が株価によって並べ替えられるとします。 株式の <xref:System.Windows.Data.CollectionView> でライブ並べ替えが有効な場合、株式が別の株式の価格を上回るか下回ると、<xref:System.Windows.Controls.DataGrid> における株式の位置が移動します。   詳細については、<xref:System.ComponentModel.ICollectionViewLiveShaping> インターフェイスのトピックを参照してください。  
  
<a name="weak_event_pattern"></a>   
## <a name="improved-support-for-establishing-a-weak-reference-to-an-event"></a>イベントへの弱い参照確立のサポート強化  
 追加のインターフェイスを実装しなくてもイベントへのサブスクライバーがイベントに参加できるため、弱いパターンの実装が簡単になりました。  一般的な <xref:System.Windows.WeakEventManager> クラスでは、専用の <xref:System.Windows.WeakEventManager> が特定のイベントに存在しない場合は、サブスクライバーが弱いイベント パターンにも参加できるようになりました。  詳細については、「[弱いイベント パターン](../advanced/weak-event-patterns.md)」を参照してください。  
  
<a name="async"></a>   
## <a name="new-methods-for-the-dispatcher-class"></a>ディスパッチャー クラスの新しいメソッド  
 ディスパッチャー クラスは、同期操作および非同期操作の新しいメソッドを定義します。  同期メソッドである <xref:System.Windows.Threading.Dispatcher.Invoke%2A> は、<xref:System.Action> パラメーターまたは <xref:System.Func%601> パラメーターを受け取るオーバーロードを定義します。 新しい非同期メソッドである <xref:System.Windows.Threading.Dispatcher.InvokeAsync%2A>は、コールバック パラメーターとして <xref:System.Action> か <xref:System.Func%601> を受け取り、<xref:System.Windows.Threading.DispatcherOperation> か <xref:System.Windows.Threading.DispatcherOperation%601> を返します。   <xref:System.Windows.Threading.DispatcherOperation> クラスと <xref:System.Windows.Threading.DispatcherOperation%601> クラスは、<xref:System.Threading.Tasks.Task> プロパティを定義します。  <xref:System.Windows.Threading.Dispatcher.InvokeAsync%2A> を呼び出すと、`await` または関連付けられた <xref:System.Windows.Threading.DispatcherOperation>の <xref:System.Threading.Tasks.Task> を持つキーワードを使用できます。 <xref:System.Threading.Tasks.Task> または <xref:System.Windows.Threading.DispatcherOperation> によって返される <xref:System.Windows.Threading.DispatcherOperation%601> を同期的に待機する必要がある場合、<xref:System.Windows.Threading.TaskExtensions.DispatcherOperationWait%2A> 拡張メソッドを呼び出します。 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> を呼び出すと、呼び出し元スレッドで操作がキューに置かれた場合はデッドロックが発生します。 <xref:System.Threading.Tasks.Task> を使用して非同期操作を実行する方法の詳細については、「[タスクの並列化 (タスク並列ライブラリ)](../../../standard/parallel-programming/task-based-asynchronous-programming.md)」を参照してください。  
  
<a name="events_markup_extenions"></a>   
## <a name="markup-extensions-for-events"></a>イベントのマークアップ拡張機能  
 WPF 4.5 では、イベントのマークアップ拡張機能がサポートされます。  WPF はイベントに使用されるマークアップ拡張機能を定義しませんが、サードパーティがイベントで使用できるマークアップ拡張機能を作成できます。  
  
## <a name="see-also"></a>参照

- [.NET Framework の新機能](../../whats-new/index.md)
