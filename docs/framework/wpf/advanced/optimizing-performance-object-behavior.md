---
title: 'パフォーマンスの最適化 : オブジェクトの動作'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- user interface virtualization [WPF]
- dependency properties [WPF], performance
- event handlers [WPF]
- object performance considerations [WPF]
- Freezable objects [WPF], performance
ms.assetid: 73aa2f47-1d73-439a-be1f-78dc4ba2b5bd
ms.openlocfilehash: 64e567cd28e9458b483b0963e0dedd924ad23bab
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094489"
---
# <a name="optimizing-performance-object-behavior"></a>パフォーマンスの最適化 : オブジェクトの動作
[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] オブジェクトに固有の動作を理解することは、機能とパフォーマンスのバランスを見極めるうえで役に立ちます。  

<a name="Not_Removing_Event_Handlers"></a>   
## <a name="not-removing-event-handlers-on-objects-may-keep-objects-alive"></a>オブジェクトのイベント ハンドラーを削除しないとオブジェクトが維持される可能性がある  
 オブジェクトがそのイベントに渡すデリゲートは、事実上そのオブジェクトへの参照です。 このため、イベント ハンドラーによってオブジェクトが本来より長く維持される可能性があります。 オブジェクトのイベントをリッスンするように登録されているオブジェクトのクリーンアップを実行するときには、オブジェクトを解放する前にそのデリゲートを削除することが重要です。 不要なオブジェクトが残っていると、アプリケーションのメモリ使用量が増加します。 これは、オブジェクトが論理ツリーやビジュアル ツリーのルートである場合には特に重要になります。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] によって導入される弱いイベント リスナー パターンは、ソースとリスナーのオブジェクト有効期間の関係の追跡が困難な場合に役に立ちます。 一部の既存の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] イベントはこのパターンを使用します。 カスタム イベントを持つオブジェクトを実装する場合に、このパターンが役に立つこともあります。 詳細については、「[弱いイベント パターン](weak-event-patterns.md)」を参照してください。  
  
 CLR プロファイラーや作業セット ビューアーなど、特定のプロセスのメモリ使用量に関する情報を入手できるツールもいくつかあります。 CLR プロファイラーには、割り当てられた型のヒストグラム、割り当てグラフと呼び出しグラフ、さまざまなジェネレーションのガベージ コレクションとその結果のマネージド ヒープの状態を示す時系列、メソッドごとの割り当てとアセンブリの読み込みを示す呼び出しツリーなど、非常に便利な割り当てプロファイルのビューがいくつか含まれています。 詳しくは、「[パフォーマンス](https://docs.microsoft.com/previous-versions/aa497289(v=msdn.10))」をご覧ください。  
  
<a name="DPs_and_Objects"></a>   
## <a name="dependency-properties-and-objects"></a>依存関係プロパティと依存関係オブジェクト  
 一般に、<xref:System.Windows.DependencyObject> の依存関係プロパティへのアクセスは、CLR プロパティにアクセスする場合よりも遅くなりません。 プロパティ値を設定する場合、パフォーマンスのオーバーヘッドはわずかですが、値を取得することは、CLR プロパティから値を取得するのと同じくらい高速です。 この小さなパフォーマンス オーバーヘッドがある代わりに、依存関係プロパティでは、データ バインディング、アニメーション、継承、スタイル設定などの堅牢な機能がサポートされています。 依存関係プロパティの詳細については、「[依存関係プロパティの概要](dependency-properties-overview.md)」を参照してください。  
  
### <a name="dependencyproperty-optimizations"></a>DependencyProperty の最適化  
 アプリケーションで依存関係プロパティを定義するときには注意が必要です。 <xref:System.Windows.DependencyProperty> が、<xref:System.Windows.FrameworkPropertyMetadata.AffectsMeasure%2A>などの他のメタデータオプションではなく、render 型のメタデータオプションにのみ影響する場合は、そのメタデータをオーバーライドすることによって、それをマークする必要があります。 プロパティ メタデータをオーバーライドまたは取得する方法の詳細については、「[依存関係プロパティのメタデータ](dependency-property-metadata.md)」を参照してください。  
  
 すべてのプロパティ変更が実際に測定、配置、描画に影響するわけではない場合は、測定、配置、描画の各パスを、プロパティ変更ハンドラーによって手動で無効にした方が効率的な場合もあります。 たとえば、設定した制限値より値が大きい場合にのみ背景を再描画する場合は、 設定した制限値を値が超えていた場合にのみプロパティ変更ハンドラーで描画を無効にします。  
  
### <a name="making-a-dependencyproperty-inheritable-is-not-free"></a>DependencyProperty を継承可能にするとパフォーマンスへの負荷が発生する  
 既定では、登録した依存関係プロパティは継承不可になります。 ただし、プロパティはどれでも明示的に継承可能にすることができます。 この機能は便利ですが、プロパティを継承可能にすると、プロパティの無効化の時間が増加して、パフォーマンスに影響します。  
  
### <a name="use-registerclasshandler-carefully"></a>RegisterClassHandler は慎重に使用する  
 <xref:System.Windows.EventManager.RegisterClassHandler%2A> を呼び出すことによってインスタンスの状態を保存できますが、すべてのインスタンスでハンドラーが呼び出されることに注意してください。これにより、パフォーマンスの問題が発生する可能性があります。 <xref:System.Windows.EventManager.RegisterClassHandler%2A> は、アプリケーションでインスタンスの状態を保存する必要がある場合にのみ使用してください。  
  
### <a name="set-the-default-value-for-a-dependencyproperty-during-registration"></a>DependencyProperty の既定値は登録時に設定する  
 既定値を必要とする <xref:System.Windows.DependencyProperty> を作成する場合は、<xref:System.Windows.DependencyProperty>の <xref:System.Windows.DependencyProperty.Register%2A> メソッドにパラメーターとして渡される既定のメタデータを使用して、値を設定します。 コンストラクターや要素の各インスタンスでプロパティ値を設定するのではなく、この方法を使用するようにしてください。  
  
### <a name="set-the-propertymetadata-value-using-register"></a>Register を使用して PropertyMetadata の値を設定する  
 <xref:System.Windows.DependencyProperty>を作成する場合は、<xref:System.Windows.DependencyProperty.Register%2A> または <xref:System.Windows.DependencyProperty.OverrideMetadata%2A> のいずれかの方法を使用して <xref:System.Windows.PropertyMetadata> を設定することもできます。 オブジェクトは <xref:System.Windows.DependencyProperty.OverrideMetadata%2A>を呼び出すための静的コンストラクターを持つこともできますが、これは最適なソリューションではなく、パフォーマンスに影響します。 最適なパフォーマンスを得るには、<xref:System.Windows.DependencyProperty.Register%2A>の呼び出し中に <xref:System.Windows.PropertyMetadata> を設定します。  
  
<a name="Freezable_Objects"></a>   
## <a name="freezable-objects"></a>Freezable オブジェクト  
 <xref:System.Windows.Freezable> は、固定されていない2つの状態を持つ特殊な種類のオブジェクトです。 可能な場合は常にオブジェクトを固定するようにすると、アプリケーションのパフォーマンスが向上し、作業セットを縮小できます。 詳細については、「[Freezable オブジェクトの概要](freezable-objects-overview.md)」を参照してください。  
  
 各 <xref:System.Windows.Freezable> には、変更されるたびに発生する <xref:System.Windows.Freezable.Changed> イベントがあります。 ただし、変更通知はアプリケーションのパフォーマンスに影響を与えます。  
  
 各 <xref:System.Windows.Shapes.Rectangle> が同じ <xref:System.Windows.Media.Brush> オブジェクトを使用する次の例を考えてみます。  
  
 [!code-csharp[Performance#PerformanceSnippet2](~/samples/snippets/csharp/VS_Snippets_Wpf/Performance/CSharp/Window1.xaml.cs#performancesnippet2)]
 [!code-vb[Performance#PerformanceSnippet2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/Performance/visualbasic/window1.xaml.vb#performancesnippet2)]  
  
 既定では、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、<xref:System.Windows.Shapes.Rectangle> オブジェクトの <xref:System.Windows.Shapes.Shape.Fill%2A> プロパティを無効にするために、<xref:System.Windows.Media.SolidColorBrush> オブジェクトの <xref:System.Windows.Freezable.Changed> イベントのイベントハンドラーを提供します。 この場合、<xref:System.Windows.Media.SolidColorBrush> が <xref:System.Windows.Freezable.Changed> イベントを起動するたびに、<xref:System.Windows.Shapes.Rectangle>ごとにコールバック関数を呼び出す必要があります。これらのコールバック関数呼び出しを蓄積すると、パフォーマンスが大幅に低下します。 さらに、この時点でハンドラーを追加または削除すると、アプリケーションでリスト全体を検査しなければならなくなるため、パフォーマンスに大きく影響します。 アプリケーションのシナリオによって <xref:System.Windows.Media.SolidColorBrush>が変更されない場合は、<xref:System.Windows.Freezable.Changed> イベントハンドラーを不必要に維持するコストを支払う必要があります。  
  
 <xref:System.Windows.Freezable> を固定すると、変更通知の保持にリソースを費やす必要がなくなるため、パフォーマンスが向上します。 次の表は、<xref:System.Windows.Freezable.IsFrozen%2A> プロパティが `true`に設定されている場合の単純な <xref:System.Windows.Media.SolidColorBrush> のサイズを示していますが、それ以外の場合はと比較しています。 これは、10個の <xref:System.Windows.Shapes.Rectangle> オブジェクトの <xref:System.Windows.Shapes.Shape.Fill%2A> プロパティに1つのブラシを適用することを前提としています。  
  
|**State**|**[サイズ]**|  
|---------------|--------------|  
|凍結された <xref:System.Windows.Media.SolidColorBrush>|212 バイト|  
|固定されていない <xref:System.Windows.Media.SolidColorBrush>|972 バイト|  
  
 この概念の例を次のコード サンプルに示します。  
  
 [!code-csharp[Performance#PerformanceSnippet3](~/samples/snippets/csharp/VS_Snippets_Wpf/Performance/CSharp/Window1.xaml.cs#performancesnippet3)]
 [!code-vb[Performance#PerformanceSnippet3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/Performance/visualbasic/window1.xaml.vb#performancesnippet3)]  
  
### <a name="changed-handlers-on-unfrozen-freezables-may-keep-objects-alive"></a>固定されていない Freezable の Changed ハンドラーによってオブジェクトが維持される可能性がある  
 オブジェクトが <xref:System.Windows.Freezable> オブジェクトの <xref:System.Windows.Freezable.Changed> イベントに渡すデリゲートは、事実上、そのオブジェクトへの参照です。 したがって、<xref:System.Windows.Freezable.Changed> イベントハンドラーでは、オブジェクトが予想よりも長く保持される可能性があります。 <xref:System.Windows.Freezable> オブジェクトの <xref:System.Windows.Freezable.Changed> イベントをリッスンするように登録されているオブジェクトのクリーンアップを実行する場合は、オブジェクトを解放する前にそのデリゲートを削除することが不可欠です。  
  
 また [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、<xref:System.Windows.Freezable.Changed> イベントを内部でフックします。 たとえば、値として <xref:System.Windows.Freezable> を受け取るすべての依存関係プロパティは、<xref:System.Windows.Freezable.Changed> イベントを自動的にリッスンします。 <xref:System.Windows.Media.Brush>を受け取る <xref:System.Windows.Shapes.Shape.Fill%2A> プロパティは、この概念を示しています。  
  
 [!code-csharp[Performance#PerformanceSnippet4](~/samples/snippets/csharp/VS_Snippets_Wpf/Performance/CSharp/Window1.xaml.cs#performancesnippet4)]
 [!code-vb[Performance#PerformanceSnippet4](~/samples/snippets/visualbasic/VS_Snippets_Wpf/Performance/visualbasic/window1.xaml.vb#performancesnippet4)]  
  
 `myRectangle.Fill`への `myBrush` の割り当てでは、<xref:System.Windows.Shapes.Rectangle> オブジェクトを指すデリゲートが <xref:System.Windows.Media.SolidColorBrush> オブジェクトの <xref:System.Windows.Freezable.Changed> イベントに追加されます。 このため、次のコードを使用しても、`myRect` は実際にはガベージ コレクションの対象にはなりません。  
  
 [!code-csharp[Performance#PerformanceSnippet5](~/samples/snippets/csharp/VS_Snippets_Wpf/Performance/CSharp/Window1.xaml.cs#performancesnippet5)]
 [!code-vb[Performance#PerformanceSnippet5](~/samples/snippets/visualbasic/VS_Snippets_Wpf/Performance/visualbasic/window1.xaml.vb#performancesnippet5)]  
  
 この場合、`myBrush` は `myRectangle` 生きたままであり、<xref:System.Windows.Freezable.Changed> イベントが発生したときにコールバックされます。 新しい <xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.Shapes.Shape.Fill%2A> プロパティに `myBrush` を割り当てると、単に `myBrush`に別のイベントハンドラーが追加されることに注意してください。  
  
 これらの種類のオブジェクトをクリーンアップするには、<xref:System.Windows.Shapes.Shape.Fill%2A> プロパティから <xref:System.Windows.Media.Brush> を削除することをお勧めします。これにより、<xref:System.Windows.Freezable.Changed> イベントハンドラーが削除されます。  
  
 [!code-csharp[Performance#PerformanceSnippet6](~/samples/snippets/csharp/VS_Snippets_Wpf/Performance/CSharp/Window1.xaml.cs#performancesnippet6)]
 [!code-vb[Performance#PerformanceSnippet6](~/samples/snippets/visualbasic/VS_Snippets_Wpf/Performance/visualbasic/window1.xaml.vb#performancesnippet6)]  
  
<a name="User_Interface_Virtualization"></a>   
## <a name="user-interface-virtualization"></a>ユーザー インターフェイスの仮想化  
 また [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、データバインドされた子コンテンツを自動的に "仮想化" する <xref:System.Windows.Controls.StackPanel> 要素のバリエーションも提供します。 ここで、"仮想化" は、どの項目を画面に表示するかに基づいて、オブジェクトのサブセットを多数のデータ項目から生成する手法を指します。 特定の時間に画面に UI 要素が少ししか表示されていない場合に多数の UI 要素を生成すると、メモリおよびプロセッサの両方に負荷がかかります。 <xref:System.Windows.Controls.VirtualizingStackPanel> (<xref:System.Windows.Controls.VirtualizingPanel>によって提供される機能を使用) によって、表示可能な項目が計算され、<xref:System.Windows.Controls.ItemsControl> (<xref:System.Windows.Controls.ListBox> や <xref:System.Windows.Controls.ListView>など) の <xref:System.Windows.Controls.ItemContainerGenerator> と連動して、表示される項目の要素のみが作成されます。  
  
 パフォーマンスの最適化の一環として、これらの項目のビジュアル オブジェクトは、画面に表示される場合にのみ生成または維持されます。 既にコントロールの表示可能領域にないビジュアル オブジェクトは削除される可能性があります。 これをデータの仮想化と混同しないようにしてください。データの仮想化では、すべてのデータ オブジェクトがローカル コレクションに存在するのではなく、データ オブジェクトが必要に応じてストリームされます。  
  
 次の表は、<xref:System.Windows.Controls.StackPanel> と <xref:System.Windows.Controls.VirtualizingStackPanel>に 5000 <xref:System.Windows.Controls.TextBlock> 要素を追加して表示する経過時間を示しています。 このシナリオでは、<xref:System.Windows.Controls.ItemsControl> オブジェクトの <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> プロパティにテキスト文字列をアタッチしてから、パネル要素にテキスト文字列が表示されるまでの時間を測定します。  
  
|**ホスト パネル**|**レンダリング時間 (ミリ秒)**|  
|--------------------|----------------------------|  
|<xref:System.Windows.Controls.StackPanel>|3210|  
|<xref:System.Windows.Controls.VirtualizingStackPanel>|46|  
  
## <a name="see-also"></a>参照

- [WPF アプリケーションのパフォーマンスの最適化](optimizing-wpf-application-performance.md)
- [アプリケーション パフォーマンスの計画](planning-for-application-performance.md)
- [ハードウェアの活用](optimizing-performance-taking-advantage-of-hardware.md)
- [レイアウトとデザイン](optimizing-performance-layout-and-design.md)
- [2D グラフィックスとイメージング](optimizing-performance-2d-graphics-and-imaging.md)
- [アプリケーション リソース](optimizing-performance-application-resources.md)
- [[テキスト]](optimizing-performance-text.md)
- [データ バインディング](optimizing-performance-data-binding.md)
- [パフォーマンスに関するその他の推奨事項](optimizing-performance-other-recommendations.md)
