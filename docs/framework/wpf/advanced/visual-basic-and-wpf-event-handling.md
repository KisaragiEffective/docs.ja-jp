---
title: Visual Basic でのイベントの処理
ms.date: 03/30/2017
helpviewer_keywords:
- Visual Basic [WPF], event handlers
- event handlers [WPF], Visual Basic
ms.assetid: ad4eb9aa-3afc-4a71-8cf6-add3fbea54a1
ms.openlocfilehash: 959ef66f41f6c5f06e18a202109fba058c522d1d
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76735409"
---
# <a name="visual-basic-and-wpf-event-handling"></a>Visual Basic と WPF のイベント処理
特に Microsoft Visual Basic .NET 言語では、言語固有の `Handles` キーワードを使用してイベントハンドラーをインスタンスに関連付けることができます。これには、イベントハンドラーを属性または <xref:System.Windows.UIElement.AddHandler%2A> メソッドを使用してアタッチする必要があります。 ただし、インスタンスにハンドラーをアタッチする `Handles` 技法にはいくつかの制限があります。これは、`Handles` 構文が [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] イベントシステムの特定のルーティングイベント機能の一部をサポートできないためです。  
  
## <a name="using-handles-in-a-wpf-application"></a>WPF アプリケーションでの "ハンドル" の使用  
 `Handles` を持つインスタンスおよびイベントに接続されているイベントハンドラーはすべて、インスタンスの部分クラス宣言内で定義する必要があります。これは、要素の属性値によって割り当てられるイベントハンドラーにも必要です。 `Handles` は、<xref:System.Windows.FrameworkContentElement.Name%2A> プロパティ値 (または[X:Name ディレクティブ](../../../desktop-wpf/xaml-services/xname-directive.md)が宣言されている) を持つページ上の要素に対してのみ指定できます。 これは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] の <xref:System.Windows.FrameworkContentElement.Name%2A> によって、`Handles` 構文で必要とされるインスタンス参照が作成されるためです *。* <xref:System.Windows.FrameworkContentElement.Name%2A> 参照のない `Handles` に使用できる唯一の要素は、部分クラスを定義するルート要素のインスタンスです。  
  
 インスタンスを分離することによって、複数の要素に同じハンドラーを割り当てることができます。 `Handles` の後にコンマが含まれるように*なり*ます。  
  
 `Handles` を使用して、同じインスタンスに複数のハンドラーを割り当てることができ*ます。イベント*の参照。 `Handles` 参照でハンドラーが指定されている順序には、重要度を割り当てないでください。同じイベントを処理するハンドラーを任意の順序で呼び出すことを想定してください。  
  
 宣言内の `Handles` と共に追加されたハンドラーを削除するには、<xref:System.Windows.UIElement.RemoveHandler%2A>を呼び出すことができます。  
  
 メンバーテーブルで処理されるイベントを定義するインスタンスにハンドラーをアタッチする限り、`Handles` を使用して、ルーティングイベントのハンドラーをアタッチできます。 ルーティングイベントの場合、`Handles` にアタッチされるハンドラーは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 属性として、または <xref:System.Windows.UIElement.AddHandler%2A>の共通署名と共にアタッチされるハンドラーと同じルーティング規則に従います。 これは、イベントが既に処理済みとしてマークされている場合 (イベントデータの <xref:System.Windows.RoutedEventArgs.Handled%2A> プロパティが `True`)、そのイベントインスタンスへの応答として `Handles` にアタッチされたハンドラーが呼び出されないことを意味します。 イベントは、ルート内の別の要素のインスタンスハンドラーによって処理されるようにマークすることも、現在の要素またはルートの前の要素に対するクラス処理によって処理することもできます。 ペアのトンネル/バブルイベントをサポートする入力イベントの場合、トンネリングルートでは、イベントペアが処理済みとマークされている可能性があります。 ルーティング イベントの詳細については、「[ルーティング イベントの概要](routed-events-overview.md)」を参照してください。  
  
## <a name="limitations-of-handles-for-adding-handlers"></a>ハンドラーを追加するための "処理" の制限事項  
 `Handles` は、添付イベントのハンドラーを参照できません。 この添付イベントには `add` アクセサーメソッドを使用し、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]では*typename*イベント属性を使用する必要があります。 詳細については、「[ルーティングイベントの概要](routed-events-overview.md)」を参照してください。  
  
 ルーティングイベントの場合、インスタンスメンバーテーブルにそのイベントが存在するインスタンスに対してのみ、`Handles` を使用してハンドラーを割り当てることができます。 ただし、一般的なルーティングイベントでは、親要素がメンバーテーブルにそのイベントを持たない場合でも、子要素からのイベントのリスナーになります。 属性構文では、 *typename. membername*属性フォームを使用して指定できます。これは、処理するイベントを実際に定義する型を修飾します。 たとえば、(`Click` イベントが定義されていない) 親 `Page` は、`Button.Click`の形式で属性ハンドラーを割り当てることにより、ボタンクリックイベントをリッスンできます。 ただし `Handles` では、競合する*インスタンスのイベント*フォームをサポートする必要があるため、 *typename*フォームはサポートされません。 詳細については、「[ルーティングイベントの概要](routed-events-overview.md)」を参照してください。  
  
 既に処理済みとマークされているイベントに対して呼び出されるハンドラーをアタッチ `Handles` ことはできません。 代わりに、コードを使用し、<xref:System.Windows.UIElement.AddHandler%28System.Windows.RoutedEvent%2CSystem.Delegate%2CSystem.Boolean%29>の `handledEventsToo` オーバーロードを呼び出す必要があります。  
  
> [!NOTE]
> XAML で同じイベントに対してイベントハンドラーを指定する場合は、Visual Basic コードで `Handles` 構文を使用しないでください。 この場合、イベントハンドラーは2回呼び出されます。  
  
## <a name="how-wpf-implements-handles-functionality"></a>WPF が "ハンドル" 機能を実装する方法  
 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] ページがコンパイルされると、中間ファイルによって、<xref:System.Windows.FrameworkContentElement.Name%2A> プロパティが設定されている (または[X:Name ディレクティブ](../../../desktop-wpf/xaml-services/xname-directive.md)が宣言されている) ページ上のすべての要素への `Friend` `WithEvents` 参照が宣言されます。 各名前付きインスタンスは、`Handles`によってハンドラーに割り当てることができる要素である可能性があります。  
  
> [!NOTE]
> Visual Studio では、IntelliSense を使用して、ページ内の `Handles` 参照に使用できる要素の完了を確認できます。 ただし、この場合、中間ファイルがすべての `Friends` 参照を設定できるように、1つのコンパイルパスが必要になることがあります。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.UIElement.AddHandler%2A>
- [ルーティング イベントの処理済みとしてのマーキング、およびクラス処理](marking-routed-events-as-handled-and-class-handling.md)
- [ルーティング イベントの概要](routed-events-overview.md)
- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
