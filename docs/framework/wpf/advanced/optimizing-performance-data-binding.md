---
title: 'パフォーマンスの最適化 : データ バインド'
ms.date: 03/30/2017
helpviewer_keywords:
- binding data [WPF], performance
- data binding [WPF], performance
ms.assetid: 1506a35d-c009-43db-9f1e-4e230ad5be73
ms.openlocfilehash: 9b302be3ed9f01ccd27470063f49966dc7d74708
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73740796"
---
# <a name="optimizing-performance-data-binding"></a>パフォーマンスの最適化 : データ バインド
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] データ バインディングは、アプリケーションがデータを提示し、データと対話するための簡単で一貫性のある方法を提供します。 要素は、CLR オブジェクトおよび XML の形式で、さまざまなデータソースのデータにバインドできます。  
  
 このトピックでは、データ バインディングのパフォーマンスに関する推奨事項について説明します。  

<a name="HowDataBindingReferencesAreResolved"></a>   
## <a name="how-data-binding-references-are-resolved"></a>データ バインディングの参照が解決されるしくみ  
 データ バインディングのパフォーマンスの問題に入る前に、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] のデータ バインディング エンジンがバインディングのオブジェクト参照をどのように解決するのかを説明します。  
  
 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] データバインディングのソースには、任意の CLR オブジェクトを指定できます。 CLR オブジェクトのプロパティ、サブプロパティ、またはインデクサーにバインドできます。 バインディング参照は Microsoft .NET Framework リフレクションまたは <xref:System.ComponentModel.ICustomTypeDescriptor>を使用して解決されます。 次に、バインディングのオブジェクト参照を解決するための 3 つの方法について説明します。  
  
 1 つ目は、リフレクションを使用する方法です。 この場合、<xref:System.Reflection.PropertyInfo> オブジェクトを使用してプロパティの属性を検出し、プロパティのメタデータにアクセスできます。 <xref:System.ComponentModel.ICustomTypeDescriptor> インターフェイスを使用する場合、データバインディングエンジンはこのインターフェイスを使用してプロパティ値にアクセスします。 <xref:System.ComponentModel.ICustomTypeDescriptor> インターフェイスは、オブジェクトにプロパティの静的なセットがない場合に特に便利です。  
  
 プロパティの変更通知を提供するには、<xref:System.ComponentModel.INotifyPropertyChanged> インターフェイスを実装するか、<xref:System.ComponentModel.TypeDescriptor>に関連付けられている変更通知を使用します。 ただし、プロパティ変更通知を実装する場合は、<xref:System.ComponentModel.INotifyPropertyChanged>を使用することをお勧めします。  
  
 ソースオブジェクトが CLR オブジェクトで、source プロパティが CLR プロパティの場合、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] データバインディングエンジンは、最初にソースオブジェクトでリフレクションを使用して <xref:System.ComponentModel.TypeDescriptor>を取得し、次に <xref:System.ComponentModel.PropertyDescriptor>を照会する必要があります。 パフォーマンスの観点から見た場合、このリフレクション操作のシーケンスには非常に時間がかかる可能性があります。  
  
 オブジェクト参照を解決する2番目の方法では、<xref:System.ComponentModel.INotifyPropertyChanged> インターフェイスを実装する CLR ソースオブジェクトと、CLR プロパティであるソースプロパティが必要です。 この場合、データ バインディング エンジンは、ソースの型に対してリフレクションを直接使用して必要なプロパティを取得します。 この方法も最適な方法とは言えませんが、最初の方法よりは作業セットの要件の負荷が小さくなります。  
  
 オブジェクト参照を解決する3番目の方法では、<xref:System.Windows.DependencyObject> であるソースオブジェクトと、<xref:System.Windows.DependencyProperty>であるソースプロパティが必要です。 この場合、データ バインディング エンジンはリフレクションを使用する必要はありません。 代わりに、プロパティ エンジンとデータ バインディング エンジンが連携してプロパティ参照を個別に解決します。 これが、データ バインディングに使用されているオブジェクト参照を解決するための最適な方法です。  
  
 次の表では、これら3つの方法を使用して、1000 <xref:System.Windows.Controls.TextBlock> 要素の <xref:System.Windows.Controls.TextBlock.Text%2A> プロパティをバインドするデータの速度を比較しています。  
  
|**TextBlock の Text プロパティのバインド先**|**バインディング時間 (ミリ秒)**|**レンダリング時間 -- バインディングを含む (ミリ秒)**|  
|--------------------------------------------------|-----------------------------|--------------------------------------------------|  
|CLR オブジェクトのプロパティへの|115|314|  
|を実装する CLR オブジェクトのプロパティに <xref:System.ComponentModel.INotifyPropertyChanged>|115|305|  
|<xref:System.Windows.DependencyObject>の <xref:System.Windows.DependencyProperty>。|90|263|  
  
<a name="Binding_to_Large_CLR_Objects"></a>   
## <a name="binding-to-large-clr-objects"></a>大きな CLR オブジェクトへのバインディング  
 1つの CLR オブジェクトに何千ものプロパティを使用してデータをバインドすると、パフォーマンスに大きな影響があります。 この影響を最小限に抑えるには、1つのオブジェクトを複数の CLR オブジェクトに分割し、プロパティを減らします。 この表は、1つの大きな CLR オブジェクト、または複数の小さいオブジェクトにデータをバインドするためのバインドとレンダリング時間を示しています。  
  
|**1000 個の TextBlock オブジェクトのデータ バインディングのバインド先**|**バインディング時間 (ミリ秒)**|**レンダリング時間 -- バインディングを含む (ミリ秒)**|  
|---------------------------------------------|-----------------------------|--------------------------------------------------|  
|1000プロパティを持つ CLR オブジェクト|950|1200|  
|1つのプロパティを持つ 1000 CLR オブジェクト|115|314|  
  
<a name="Binding_to_an_ItemsSource"></a>   
## <a name="binding-to-an-itemssource"></a>ItemsSource へのバインディング  
 <xref:System.Windows.Controls.ListBox>に表示する従業員の一覧を保持する CLR <xref:System.Collections.Generic.List%601> オブジェクトがあるシナリオについて考えてみます。 これら2つのオブジェクト間の対応を作成するには、従業員リストを <xref:System.Windows.Controls.ListBox>の <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> プロパティにバインドします。 ここで、グループに新しい従業員が加わったとします。 この新しい人物をバインドされた <xref:System.Windows.Controls.ListBox> 値に挿入するには、この人物を従業員リストに追加するだけで、この変更がデータバインディングエンジンによって自動的に認識されることが期待できます。 この想定は偽であることを前提としています。実際には、変更は <xref:System.Windows.Controls.ListBox> に自動的に反映されません。 これは、CLR <xref:System.Collections.Generic.List%601> オブジェクトがコレクションの変更イベントを自動的に生成しないためです。 変更を取得するために <xref:System.Windows.Controls.ListBox> を取得するには、従業員の一覧を作成し直して、<xref:System.Windows.Controls.ListBox>の <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> プロパティに再接続する必要があります。 この解決方法で問題は解決されますが、パフォーマンスへの影響はきわめて大きくなります。 <xref:System.Windows.Controls.ListBox> の <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> を新しいオブジェクトに再割り当てするたびに、<xref:System.Windows.Controls.ListBox> はまず前の項目を破棄し、リスト全体を再生成します。 <xref:System.Windows.Controls.ListBox> が複雑な <xref:System.Windows.DataTemplate>にマップされている場合は、パフォーマンスへの影響が拡大されます。  
  
 この問題の非常に効率的な解決策は、従業員リストを <xref:System.Collections.ObjectModel.ObservableCollection%601>にすることです。 <xref:System.Collections.ObjectModel.ObservableCollection%601> オブジェクトは、データバインディングエンジンが受け取ることができる変更通知を発生させます。 イベントは、リスト全体を再生成する必要なく、<xref:System.Windows.Controls.ItemsControl> の項目を追加または削除します。  
  
 次の表は、1つの項目が追加されたときに (UI 仮想化がオフになっている) <xref:System.Windows.Controls.ListBox> の更新にかかる時間を示しています。 最初の行の数値は、CLR <xref:System.Collections.Generic.List%601> オブジェクトが <xref:System.Windows.Controls.ListBox> 要素の <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A>にバインドされた経過時間を表します。 2番目の行の数値は、<xref:System.Collections.ObjectModel.ObservableCollection%601> が <xref:System.Windows.Controls.ListBox> 要素の <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A>にバインドされた経過時間を表します。 <xref:System.Collections.ObjectModel.ObservableCollection%601> データバインディング戦略を使用して、大幅な時間を節約することに注意してください。  
  
|**ItemsSource のデータ バインディングのバインド先**|**1 項目の更新時間 (ミリ秒)**|  
|--------------------------------------|---------------------------------------|  
|CLR <xref:System.Collections.Generic.List%601> オブジェクトへの|1656|  
|<xref:System.Collections.ObjectModel.ObservableCollection%601> に|20|  
  
<a name="Binding_IList_to_ItemsControl_not_IEnumerable"></a>   
## <a name="bind-ilist-to-itemscontrol-not-ienumerable"></a>IEnumerable ではなく IList を ItemsControl にバインドする  
 <xref:System.Collections.Generic.IList%601> または <xref:System.Collections.IEnumerable> を <xref:System.Windows.Controls.ItemsControl> オブジェクトにバインドするかどうかを選択できる場合は、<xref:System.Collections.Generic.IList%601> オブジェクトを選択します。 <xref:System.Windows.Controls.ItemsControl> に <xref:System.Collections.IEnumerable> をバインドすると、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は強制的にラッパー <xref:System.Collections.Generic.IList%601> オブジェクトを作成します。これは、2番目のオブジェクトの不要なオーバーヘッドによってパフォーマンスが影響を受けることを意味します。  
  
<a name="Do_not_Convert_CLR_objects_to_Xml_Just_For_Data_Binding"></a>   
## <a name="do-not-convert-clr-objects-to-xml-just-for-data-binding"></a>データ バインディングのためだけに CLR オブジェクトを XML に変換しない  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] を使用すると、XML コンテンツにデータをバインドできます。ただし、XML コンテンツへのデータバインディングは、CLR オブジェクトへのデータバインドよりも低速です。 唯一の目的がデータバインディング用である場合は、CLR オブジェクトデータを XML に変換しないでください。  
  
## <a name="see-also"></a>関連項目

- [WPF アプリケーションのパフォーマンスの最適化](optimizing-wpf-application-performance.md)
- [アプリケーション パフォーマンスの計画](planning-for-application-performance.md)
- [ハードウェアの活用](optimizing-performance-taking-advantage-of-hardware.md)
- [レイアウトとデザイン](optimizing-performance-layout-and-design.md)
- [2D グラフィックスとイメージング](optimizing-performance-2d-graphics-and-imaging.md)
- [オブジェクトの動作](optimizing-performance-object-behavior.md)
- [アプリケーション リソース](optimizing-performance-application-resources.md)
- [[テキスト]](optimizing-performance-text.md)
- [パフォーマンスに関するその他の推奨事項](optimizing-performance-other-recommendations.md)
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [チュートリアル: WPF アプリケーション内のアプリケーション データのキャッシュ](walkthrough-caching-application-data-in-a-wpf-application.md)
