---
title: プロパティ変更イベント
ms.date: 03/30/2017
helpviewer_keywords:
- dependency properties [WPF], change events
- property value changes [WPF]
- change events [WPF], property
- events [WPF], change in property values
- creating property triggers [WPF]
- property change events [WPF], types of
- property change events [WPF]
- property triggers [WPF]
- identifying changed property events [WPF]
- property triggers [WPF], definition of
ms.assetid: 0a7989df-9674-4cc1-bc50-5d8ef5d9c055
ms.openlocfilehash: 68be4c6bf7cce8a3aeb928e1f0b0e8131263320c
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459340"
---
# <a name="property-change-events"></a>プロパティ変更イベント
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] では、プロパティ値の変更に応答して発生するさまざまなイベントが定義されています。 通常、プロパティは依存関係プロパティです。 イベント自体はルーティングイベントであり、標準の共通言語ランタイム (CLR) イベントである場合もあります。 要素ツリーを通じて適切にルーティングされるプロパティ変更もあれば、プロパティが変更されたオブジェクトにのみ関係するプロパティ変更もあるため、イベントの定義はシナリオによって異なります。  
  
## <a name="identifying-a-property-change-event"></a>プロパティ変更イベントの識別  
 シグネチャ パターンまたは名前付けパターンに基づいてプロパティ変更を報告するすべてのイベントが、プロパティ変更イベントとして明示的に識別されるわけではありません。 一般に、SDK ドキュメント内のイベントの説明は、イベントがプロパティ値の変更に直接関連付けられているかどうかを示し、プロパティとイベント間の相互参照を提供します。  
  
### <a name="routedpropertychanged-events"></a>RoutedPropertyChanged イベント  
 特定のイベントでは、プロパティ変更イベントで明示的に使用されるイベント データ型とデリゲートを使用します。 イベントデータ型が <xref:System.Windows.RoutedPropertyChangedEventArgs%601>、デリゲートが <xref:System.Windows.RoutedPropertyChangedEventHandler%601>。 イベント データもデリゲートも、ジェネリック型パラメーターを使用します。これは、ハンドラーを定義するときに、変更するプロパティの実際の型を指定するために使用されるものです。 イベントデータには、<xref:System.Windows.RoutedPropertyChangedEventArgs%601.OldValue%2A> と <xref:System.Windows.RoutedPropertyChangedEventArgs%601.NewValue%2A>という2つのプロパティが含まれています。これらは両方ともイベントデータの型引数として渡されます。  
  
 名前の "Routed" 部分は、プロパティ変更イベントがルーティング イベントして登録されていることを示しています。 プロパティ変更イベントのルーティングには、子要素 (コントロールの複合部分) のプロパティが値を変更した場合に、コントロールのトップ レベルがプロパティ変更イベントを受信できるというメリットがあります。 たとえば、<xref:System.Windows.Controls.Slider>などの <xref:System.Windows.Controls.Primitives.RangeBase> コントロールを組み込むコントロールを作成できます。 スライダー部分で <xref:System.Windows.Controls.Primitives.RangeBase.Value%2A> プロパティの値が変更された場合は、パーツではなく親コントロールでその変更を処理することができます。  
  
 古い値と新しい値があるため、このイベント ハンドラーがプロパティ値の検証コントロールとして使用される可能性があります。 しかし、プロパティ変更イベントの多くは、このような目的で設計されているわけではありません。 通常、値の提供は、他のロジック領域のコードでその値を処理するために行われますが、イベント ハンドラー内から値を実際に変更することはお勧めできません。また、ハンドラーの実装方法によっては、意図しない再帰が生じることもあります。  
  
 プロパティがカスタム依存関係プロパティである場合、またはインスタンス化コードを定義した派生クラスを使用して作業している場合は、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プロパティシステムに組み込まれているプロパティの変更を追跡するためのより適切な機構があります。システムコールバック <xref:System.Windows.CoerceValueCallback> および <xref:System.Windows.PropertyChangedCallback>。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プロパティ システムを使用して検証および強制型変換を行う方法の詳細については、「[依存関係プロパティのコールバックと検証](dependency-property-callbacks-and-validation.md)」および「[カスタム依存関係プロパティ](custom-dependency-properties.md)」を参照してください。  
  
### <a name="dependencypropertychanged-events"></a>DependencyPropertyChanged イベント  
 プロパティ変更イベントシナリオの一部である型のもう1つのペアは、<xref:System.Windows.DependencyPropertyChangedEventArgs> と <xref:System.Windows.DependencyPropertyChangedEventHandler>です。 これらのプロパティの変更のイベントはルーティングされません。これらは標準の CLR イベントです。 <xref:System.Windows.DependencyPropertyChangedEventArgs> は <xref:System.EventArgs>から派生していないため、異常なイベントデータレポートの種類です。<xref:System.Windows.DependencyPropertyChangedEventArgs> はクラスではなく構造体です。  
  
 <xref:System.Windows.DependencyPropertyChangedEventArgs> と <xref:System.Windows.DependencyPropertyChangedEventHandler> を使用するイベントは、`RoutedPropertyChanged` イベントよりも少し一般的です。 これらの型を使用するイベントの例としては、<xref:System.Windows.UIElement.IsMouseCapturedChanged>があります。  
  
 <xref:System.Windows.RoutedPropertyChangedEventArgs%601>と同様に、<xref:System.Windows.DependencyPropertyChangedEventArgs> プロパティの古い値と新しい値も報告します。 そのため、値を適用して行う操作に関して同じ注意が当てはまります。イベントに応答して送信元で値を再度変更することは、一般的にはお勧めできません。  
  
## <a name="property-triggers"></a>プロパティ トリガー  
 プロパティ変更イベントに密接に関係する概念がプロパティ トリガーです。 プロパティ トリガーはスタイルまたはテンプレート内に作成され、プロパティ トリガーが割り当てられるプロパティの値に基づく条件付きの動作を作成できます。  
  
 プロパティ トリガーのプロパティは、依存関係プロパティでなければなりません。 これは、読み取り専用の依存関係プロパティにすることができます (多くの場合そのようになっています)。 コントロールによって公開される依存関係プロパティが、プロパティ トリガーになるように部分的に設計されている場合、プロパティ名が "Is" で始まっていることで見分けられます。 この名前付けがされたプロパティは、多くの場合、読み取り専用のブール型の依存関係プロパティです。このプロパティの主要なシナリオは、リアルタイム UI に影響を及ぼす可能性があるためにプロパティ トリガーの候補である、レポート コントロール ステートです。  
  
 これらのプロパティの一部は、専用のプロパティ変更イベントも持ちます。 たとえば、プロパティ <xref:System.Windows.UIElement.IsMouseCaptured%2A> には、イベント <xref:System.Windows.UIElement.IsMouseCapturedChanged>のプロパティが変更されています。 プロパティ自体は読み取り専用で、値は入力システムによって調整され、入力システムはリアルタイムの変更ごとに <xref:System.Windows.UIElement.IsMouseCapturedChanged> を発生させます。  
  
 プロパティ トリガーを使用してプロパティの変更に対処する場合は、本当のプロパティ変更イベントに比べ、いくつかの制限があります。  
  
 プロパティ トリガーは、完全一致ロジックを通じて動作します。 トリガーが動作する特定の値を示すプロパティと値を指定します。例: `<Setter Property="IsMouseCaptured" Value="true"> ... </Setter>`。 この制限のため、ほとんどのプロパティ トリガーの使用は、ブール型のプロパティか、専用の列挙値を使用するプロパティが対象です。この場合、有効な値の範囲は、各ケースに対応するトリガーを定義するために十分な程度管理することができます。 または、プロパティ トリガーは、項目のカウントがゼロに達した場合など、特別な値に対してのみ存在する可能性があります。プロパティ値がゼロから再び変更するケースに対処するトリガーはありません (すべてのケースに対応するトリガーではなく、コード イベント ハンドラー、または、値がゼロ以外のときにトリガー状態を再び元に戻す既定の動作が必要です)。  
  
 プロパティ トリガーの構文は、プログラミングの "if" ステートメントに似ています。 トリガー条件が true の場合、プロパティ トリガーの "本体" が "実行" されます。 プロパティ トリガーの "本体" はコードではなく、マークアップです。 このマークアップは、スタイルまたはテンプレートが適用されているオブジェクトの他のプロパティを設定するために、1つまたは複数の <xref:System.Windows.Setter> 要素を使用することに制限されています。  
  
 さまざまな値を持つプロパティトリガーの "if" 条件をオフセットするには、通常、<xref:System.Windows.Setter>を使用して同じプロパティ値を既定値に設定することをお勧めします。 このようにして、トリガー条件が true の場合に <xref:System.Windows.Trigger> 含まれる setter が優先されます。また、<xref:System.Windows.Trigger> 内にない <xref:System.Windows.Setter> は、トリガー条件が false になるたびに優先されます。  
  
 プロパティ トリガーは、通常、1 つ以上の外観プロパティが、同じ要素の別のプロパティの状態に基づいて変更されるシナリオに適しています。  
  
 プロパティ トリガーの詳細については、「[スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [ルーティング イベントの概要](routed-events-overview.md)
- [依存関係プロパティの概要](dependency-properties-overview.md)
