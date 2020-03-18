---
title: 添付プロパティの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- attached properties [WPF Designer]
ms.assetid: 75928354-dc01-47e8-a018-8409aec1f32d
ms.openlocfilehash: f4e8ea9fb0643a4a434bf20fa719c3fd2d01435b
ms.sourcegitcommit: 7f8eeef060ddeb2cabfa52843776faf652c5a1f5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74089332"
---
# <a name="attached-properties-overview"></a>添付プロパティの概要

添付プロパティは、XAML によって定義された概念です。 添付プロパティは、任意のオブジェクトに設定可能なグローバル プロパティの型として使用されることを意図しています。 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] では通常、添付プロパティは従来のプロパティ "ラッパー" を含まない依存関係プロパティの特殊な形式として定義されています。

## 応募<a name="prerequisites"></a>

このトピックでは、ユーザーが [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] クラスの既存の依存関係プロパティの使用という観点から依存関係プロパティを理解し、「[依存関係プロパティの概要](dependency-properties-overview.md)」トピックを通読していることを前提としています。 このトピックの例を実行するには、XAML について理解し、WPF アプリケーションの記述方法を理解しておく必要もあります。

## 添付プロパティを使用する理由<a name="attached_properties_usage"></a>

添付プロパティの目的の 1 つは、親要素に実際に定義されているプロパティに対する一意の値を、異なる子要素が指定できるようにすることです。 このシナリオの適用例として、子要素から親要素に、[!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] での表示方法を通知させることがあります。 1つの例として、<xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> プロパティがあります。 <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> プロパティは、<xref:System.Windows.Controls.DockPanel> 自体ではなく、<xref:System.Windows.Controls.DockPanel>内に含まれる要素に設定されるように設計されているため、添付プロパティとして作成されます。 <xref:System.Windows.Controls.DockPanel> クラスは <xref:System.Windows.Controls.DockPanel.DockProperty>という名前の静的 <xref:System.Windows.DependencyProperty> フィールドを定義し、<xref:System.Windows.Controls.DockPanel.GetDock%2A> および <xref:System.Windows.Controls.DockPanel.SetDock%2A> メソッドを添付プロパティのパブリックアクセサーとして提供します。

## アタッチされたプロパティ (XAML の)<a name="attached_properties_xaml"></a>

XAML では、構文 *AttachedPropertyProvider*.*PropertyName* を使用して添付プロパティを設定します

XAML で <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> を設定する方法の例を次に示します。

[!code-xaml[PropertiesOvwSupport#APBasicUsage](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page4.xaml#apbasicusage)]

使用方法は静的プロパティに似ていることに注意してください。name で指定されたインスタンスを参照するのではなく、添付プロパティを所有および登録する型 <xref:System.Windows.Controls.DockPanel> を常に参照します。

さらに、XAML の添付プロパティはマークアップに設定する属性であるため、設定操作にのみ関連性があります。 XAML でプロパティを直接取得することはできませんが、スタイルのトリガー (詳細については、「[スタイルとテンプレート](../controls/styling-and-templating.md)」を参照) などの値を比較するための間接的な機構があります。

### <a name="attached-property-implementation-in-wpf"></a>WPF での添付プロパティの実装

[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]では、UI プレゼンテーションに関連する WPF 型に存在する添付プロパティのほとんどは、依存関係プロパティとして実装されます。 添付プロパティは XAML の概念ですが、依存関係プロパティは WPF の概念です。 WPF の添付プロパティは依存関係プロパティであるため、プロパティメタデータなどの依存関係プロパティの概念と、そのプロパティメタデータの既定値をサポートします。

## 所有する型によって添付プロパティがどのように使用されるか<a name="howused"></a>

添付プロパティはどのオブジェクトにも設定できますが、プロパティを設定したことによって自動的に意味のある結果が得られるわけでも、値が別のオブジェクトによって使用されるわけでもありません。 一般に、添付プロパティの目的は、想定されるさまざまなクラス階層または論理関係から生じるオブジェクトが、添付プロパティを定義する型に共通する情報をレポートできるようにすることです。 添付プロパティを定義する型は、一般的に次のいずれかのモデルに従っています。

- 添付プロパティを定義する型が、添付プロパティの値を設定する要素の親要素になるように設計されている。 この型の子オブジェクトは、一部のオブジェクト ツリー構造で内部ロジックを反復し、値を取得して、その値に対する処理を実行します。

- 添付プロパティを定義する型が、想定されるさまざまな親要素およびコンテンツ モデルの子要素として使用される。

- 添付プロパティを定義する型が、サービスを表す。 その他の型は、添付プロパティの値を設定します。 プロパティを設定する要素がサービスのコンテキストで評価されると、添付プロパティの値がサービス クラスの内部ロジックにより取得されます。

### <a name="an-example-of-a-parent-defined-attached-property"></a>親定義の添付プロパティの例

WPF が添付プロパティを定義する最も一般的なシナリオは、親要素が子要素のコレクションをサポートし、さらに、各子要素に対して動作の詳細が個別に報告される動作を実装する場合です。

<xref:System.Windows.Controls.DockPanel> は <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> 添付プロパティを定義し、<xref:System.Windows.Controls.DockPanel> レンダリングロジックの一部としてクラスレベルのコードを持つことができます (具体的には、<xref:System.Windows.Controls.DockPanel.MeasureOverride%2A> および <xref:System.Windows.Controls.DockPanel.ArrangeOverride%2A>)。 <xref:System.Windows.Controls.DockPanel> インスタンスは、その直接の子要素のいずれかに <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType>の値が設定されているかどうかを常に確認します。 設定されている場合は、その値が、その子要素に適用されるレンダリング ロジックの入力になります。 入れ子になった <xref:System.Windows.Controls.DockPanel> インスタンスはそれぞれ独自の直接の子要素のコレクションを処理しますが、その動作は <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> 値を <xref:System.Windows.Controls.DockPanel> プロセスする方法に固有の実装です。 直接の親以外の要素に影響を与える添付プロパティを所有することは、理論上は可能です。 <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> 添付プロパティが、<xref:System.Windows.Controls.DockPanel> 親要素を持たない要素に設定されている場合は、エラーまたは例外は発生しません。 これは、グローバルプロパティ値が設定されていることを意味しますが、情報を使用できる現在の <xref:System.Windows.Controls.DockPanel> 親がありません。

## アタッチされるプロパティ (コードの)<a name="attached_properties_code"></a>

WPF の添付プロパティには、get/set アクセスを簡単にするための一般的な CLR "ラッパー" メソッドはありません。 これは、プロパティが設定されているインスタンスの場合、添付プロパティは必ずしも CLR 名前空間の一部ではないためです。 ただし、XAML の解析時に XAML プロセッサがその値を設定できる必要があります。 有効な添付プロパティの使用をサポートするには、添付プロパティの所有者の型が**Get_PropertyName_** と**Set_PropertyName_** の形式で専用のアクセサーメソッドを実装する必要があります。 この専用のアクセサー メソッドは、コード内の添付プロパティの取得/設定でも役立ちます。 コードの観点では、添付プロパティはプロパティ アクセサーではなくメソッド アクセサーを含むバッキング フィールドに似ており、そのバッキング フィールドは特に定義することなくすべてのオブジェクトに存在することができます。

次の例は、コードに添付プロパティを設定する方法を示しています。 この例では、`myCheckBox` は <xref:System.Windows.Controls.CheckBox> クラスのインスタンスです。

[!code-csharp[PropertiesOvwSupport#APCode](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page4.xaml.cs#apcode)]
[!code-vb[PropertiesOvwSupport#APCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertiesOvwSupport/visualbasic/page4.xaml.vb#apcode)]

XAML の場合と同様に、コードの3行目で `myDockPanel` の子要素として `myCheckBox` がまだ追加されていない場合、コードの4行目では例外は生成されませんが、プロパティ値は <xref:System.Windows.Controls.DockPanel> 親と対話しないため、何もない。 子要素に設定された <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> 値だけが <xref:System.Windows.Controls.DockPanel> 親要素の存在と組み合わせられると、レンダリングされたアプリケーションで有効な動作が発生します。 (この場合、添付プロパティを設定してからツリーに接続するか、 ツリーに接続してから添付プロパティを設定することができます。 どちらの操作でも、結果は同じです。)

## 添付プロパティのメタデータ<a name="attached_properties_metadata"></a>

プロパティを登録するときに、プロパティがレンダリングや測定などに影響するかどうかなど、プロパティの特性を指定するように <xref:System.Windows.FrameworkPropertyMetadata> が設定されます。 添付プロパティのメタデータは、一般的に依存関係プロパティとの違いがありません。 オーバーライドの既定値を添付プロパティのメタデータに指定すると、その値がオーバーライドするクラスのインスタンスの暗黙的な添付プロパティの既定値になります。 具体的には、一部のプロセスが添付プロパティの `Get` メソッド アクセサーを使用してそのプロパティの値のクエリを行った場合に、メタデータを指定したクラスのインスタンスが指定されており、その添付プロパティの値が設定されていないと、既定値がレポートされます。

プロパティでプロパティ値の継承を有効にする場合は、未接続の依存関係プロパティではなく添付プロパティを使用する必要があります。 詳細については、「[プロパティ値の継承](property-value-inheritance.md)」を参照してください。

## カスタム添付プロパティ<a name="custom"></a>

### 添付プロパティを作成する場合<a name="create_attached_properties"></a>

添付プロパティは、定義クラスではないクラスで使用できるプロパティ設定機構を用意する理由がある場合に作成できます。 この最も一般的なシナリオが、レイアウトです。 既存のレイアウトプロパティの例としては、<xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType>、<xref:System.Windows.Controls.Panel.ZIndex%2A?displayProperty=nameWithType>、および <xref:System.Windows.Controls.Canvas.Top%2A?displayProperty=nameWithType>があります。 これによって実現するシナリオは、レイアウト制御要素の子要素として存在する要素が、レイアウト親要素に対して個別にレイアウト要件を表現するというものです。親が添付プロパティとして定義したプロパティ値を、個々の子要素が設定します。

クラスがサービスを表しており、クラスでサービスをより透過的に統合できるようにしたい場合にも、添付プロパティを使用します。

もう1つのシナリオは、 **[プロパティ]** ウィンドウの編集など、VISUAL Studio WPF デザイナーのサポートを受け取ることです。 詳細については、「[コントロールの作成の概要](../controls/control-authoring-overview.md)」を参照してください。

前述のように、プロパティ値の継承を使用する場合には、添付プロパティを登録する必要があります。

### 添付プロパティを作成する方法<a name="how_do_i_create_attached_properties"></a>

クラスが、他の型で使用するために厳密に添付プロパティを定義している場合、クラスは <xref:System.Windows.DependencyObject>から派生する必要はありません。 ただし、添付プロパティが依存関係プロパティでもある WPF モデル全体に従う場合は、<xref:System.Windows.DependencyObject> から派生する必要があります。

<xref:System.Windows.DependencyProperty>型の `public static readonly` フィールドを宣言することによって、添付プロパティを依存関係プロパティとして定義します。 このフィールドを定義するには、<xref:System.Windows.DependencyProperty.RegisterAttached%2A> メソッドの戻り値を使用します。 フィールド名は、定義されている WPF パターンに従って、識別フィールドとそれが表すプロパティの名前が付けられている場合に、添付プロパティ名が文字列 `Property`と一致する必要があります。 添付プロパティプロバイダーは、添付プロパティのアクセサーとして静的**Get_PropertyName_** および**Set_PropertyName_** メソッドを提供する必要もあります。この操作を行わないと、プロパティシステムが添付プロパティを使用できなくなります。

> [!NOTE]
> 添付プロパティの get アクセサーを省略した場合、Visual Studio や Blend for Visual Studio などのデザインツールでは、プロパティのデータバインディングは機能しません。

#### <a name="the-get-accessor"></a>Get アクセサー

**Get_PropertyName_** アクセサーのシグネチャは次のようにする必要があります。

`public static object GetPropertyName(object target)`

- `target` オブジェクトは、実装のより具体的な型として指定することができます。 たとえば、<xref:System.Windows.Controls.DockPanel.GetDock%2A?displayProperty=nameWithType> メソッドは、パラメーターを <xref:System.Windows.UIElement>として入力します。これは、添付プロパティが <xref:System.Windows.UIElement> インスタンスでのみ設定されるためです。

- 戻り値は、実装のより具体的な型として指定することができます。 たとえば、<xref:System.Windows.Controls.DockPanel.GetDock%2A> メソッドでは、値をその列挙にのみ設定できるため、<xref:System.Windows.Controls.Dock>として型指定されます。

#### <a name="the-set-accessor"></a>Set アクセサー

**Set_PropertyName_** アクセサーのシグネチャは次のようにする必要があります。

`public static void SetPropertyName(object target, object value)`

- `target` オブジェクトは、実装のより具体的な型として指定することができます。 たとえば、<xref:System.Windows.Controls.DockPanel.SetDock%2A> メソッドでは、添付プロパティは <xref:System.Windows.UIElement> インスタンスでのみ設定されるため、<xref:System.Windows.UIElement>として型指定されます。

- `value` オブジェクトは、実装のより具体的な型として指定することができます。 たとえば、<xref:System.Windows.Controls.DockPanel.SetDock%2A> メソッドでは、値をその列挙にのみ設定できるため、<xref:System.Windows.Controls.Dock>として型指定されます。 このメソッドの値は、マークアップの添付プロパティの使用で添付プロパティが検出されたときに XAML ローダーから生じる入力であることに注意してください。 この入力はマークアップの XAML 属性値として指定された値です。 したがって、適切な型を属性値 (最終的には単なる文字列) から作成できるように、使用する型の型変換、値シリアライザー、またはマークアップ拡張サポートが必要です。

次の例は、(<xref:System.Windows.DependencyProperty.RegisterAttached%2A> メソッドを使用した) 依存関係プロパティの登録と、 **Get_PropertyName_** アクセサーと**Set_PropertyName_** アクセサーを示しています。 この例では、添付プロパティ名は `IsBubbleSource` です。 したがって、アクセサーの名前は `GetIsBubbleSource` および `SetIsBubbleSource` である必要があります。

[!code-csharp[WPFAquariumSln#RegisterAttachedBubbler](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFAquariumSln/CSharp/WPFAquariumObjects/Class1.cs#registerattachedbubbler)]
[!code-vb[WPFAquariumSln#RegisterAttachedBubbler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFAquariumSln/visualbasic/wpfaquariumobjects/class1.vb#registerattachedbubbler)]

#### <a name="attached-property-attributes"></a>添付プロパティの属性

WPF では、添付プロパティに関する情報をリフレクションプロセスに提供するためのいくつかの .NET 属性、およびリフレクションとプロパティ情報 (デザイナーなど) の一般的なユーザーを定義します。 添付プロパティに含まれる型は膨大な範囲に及ぶため、デザイナーには XAML を使用する特定のテクノロジの実装に定義されたすべての添付プロパティのグローバル リストがユーザーに表示されないようにするための手段が必要となります。 WPF が添付プロパティ用に定義する .NET 属性を使用して、特定の添付プロパティを [プロパティ] ウィンドウに表示する必要がある状況のスコープを設定できます。 また、この属性をカスタム添付プロパティに適用するという選択肢もあります。 .NET 属性の目的と構文については、適切なリファレンスページで説明されています。

- <xref:System.Windows.AttachedPropertyBrowsableAttribute>

- <xref:System.Windows.AttachedPropertyBrowsableForChildrenAttribute>

- <xref:System.Windows.AttachedPropertyBrowsableForTypeAttribute>

- <xref:System.Windows.AttachedPropertyBrowsableWhenAttributePresentAttribute>

## 添付プロパティの詳細を学習する<a name="more"></a>

- 添付プロパティの作成の詳細については、「[方法: 添付プロパティを登録する](how-to-register-an-attached-property.md)」を参照してください。

- 依存関係プロパティおよび添付プロパティの高度な使用シナリオについては、「[カスタム依存関係プロパティ](custom-dependency-properties.md)」を参照してください。

- プロパティは添付プロパティとしても依存関係プロパティとしても登録できますが、"ラッパー" 実装は公開したままにすることができます。 この場合、プロパティをその要素に設定することも、XAML の添付プロパティの構文を使用して任意の要素に設定することもできます。 標準とアタッチの両方の使用に適したシナリオを持つプロパティの例としては、<xref:System.Windows.FrameworkElement.FlowDirection%2A?displayProperty=nameWithType>があります。

## <a name="see-also"></a>関連項目

- <xref:System.Windows.DependencyProperty>
- [依存関係プロパティの概要](dependency-properties-overview.md)
- [カスタム依存関係プロパティ](custom-dependency-properties.md)
- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
- [方法: 添付プロパティを登録する](how-to-register-an-attached-property.md)
