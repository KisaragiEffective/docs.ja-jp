---
title: 依存関係プロパティの概要
description: WPF プロパティ システムによって使用されるプロパティは、依存関係プロパティと呼ばれています。 ここでは、WPF プロパティ システムと、依存関係プロパティの機能について説明します。
ms.date: 06/06/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- properties [WPF], attached
- properties [WPF], overview
- styles [WPF]
- attached properties [WPF]
- data binding [WPF]
- dependency properties [WPF]
- resources [WPF], references to
ms.assetid: d119d00c-3afb-48d6-87a0-c4da4f83dee5
ms.openlocfilehash: 5803d656d765f3f4fe3039e28b9c06202218fcfc
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73973985"
---
# <a name="dependency-properties-overview"></a>依存関係プロパティの概要

Windows Presentation Foundation (WPF) には、型の[プロパティ](../../../standard/base-types/common-type-system.md#Properties)の機能を拡張するために使用できる一連のサービスが用意されています。 通常、これらのサービスをまとめて WPF プロパティ システムと呼びます。 WPF プロパティ システムによって使用されるプロパティは、依存関係プロパティと呼ばれています。 ここでは、WPF プロパティ システムと、依存関係プロパティの機能について説明します。 この説明では、既存の依存関係プロパティを XAML およびコードで使用する方法を示します。 また、依存関係プロパティ メタデータなどの依存関係プロパティの特殊な側面や、カスタム クラスで独自の依存関係プロパティを作成する方法についても説明します。

## <a name="prerequisites"></a>必要条件
ここでは、.NET 型システムおよびオブジェクト指向プログラミングに関する基礎知識があることを前提にしています。 このトピックの例について理解するには、XAML および WPF アプリケーションの記述方法について知っておく必要もあります。 詳細については、「[チュートリアル: WPF の概要](../getting-started/walkthrough-my-first-wpf-desktop-application.md)」を参照してください。  
  
## <a name="dependency-properties-and-clr-properties"></a>依存関係プロパティおよび CLR プロパティ
 WPF では通常、プロパティは通常、標準の .NET [プロパティ](../../../standard/base-types/common-type-system.md#Properties)として公開されます。 基本的なレベルでは、これらのプロパティと直接対話でき、これらのプロパティが依存関係として実装されることを認識することはありません。 ただし、WPF プロパティ システムの一部またはすべての機能を利用できるように、これらの機能に精通しておく必要があります。

依存関係プロパティの目的は、他の入力の値に基づいてプロパティの値を計算する方法を提供することです。 他の入力には、テーマやユーザー設定などのシステム プロパティ、データ バインディングやアニメーション/ストーリーボードなどのジャスト イン タイム プロパティ判定機構、リソースやスタイルなどの多目的のテンプレート、要素ツリー内の他の要素との親子のリレーションシップから判断される値などがあります。 また、依存関係プロパティを実装して、自己完結型の検証、既定値、他のプロパティに対する変更を監視するコールバック、およびランタイム情報の可能性がある情報に基づいてプロパティ値を強制するシステムを提供できます。 既存のプロパティの実際の実装をオーバーライドしたり新しいプロパティを作成したりするのではなく、依存関係プロパティ メタデータをオーバーライドすることによって、派生クラスで既存のプロパティの特定の特性を変更することもできます。

SDK リファレンスで、プロパティのマネージド リファレンス ページの「依存関係プロパティの情報」セクションの有無によって、どのプロパティが依存関係プロパティかを特定できます。 「依存関係プロパティの情報」セクションにはその依存関係プロパティの <xref:System.Windows.DependencyProperty> 識別子フィールドへのリンクがあり、そのプロパティに設定されるメタデータ オプションのリスト、クラスごとのオーバーライド情報、およびその他の詳細も示されています。

## <a name="dependency-properties-back-clr-properties"></a>依存関係プロパティによる CLR プロパティの補足
依存関係プロパティおよび WPF プロパティ システムは、プライベート フィールドでプロパティをサポートする標準パターンの代替実装として、プロパティをサポートする型を提供することによって、プロパティ機能を拡張します。 この型の名前は <xref:System.Windows.DependencyProperty> です。 WPF プロパティ システムを定義するその他の重要な型は <xref:System.Windows.DependencyObject> です。 <xref:System.Windows.DependencyObject> は、依存関係プロパティを登録および所有できる基本クラスを定義します。

依存関係プロパティで使用される用語を次に示します。

- **依存関係プロパティ:** <xref:System.Windows.DependencyProperty> によって補助されているプロパティ。

- **依存関係プロパティの識別子:** 依存関係プロパティの登録時に戻り値として取得され、クラスの静的メンバーとして格納される <xref:System.Windows.DependencyProperty> インスタンス。 この識別子は、WPF プロパティ システムと対話する多くの API でパラメーターとして使用されます。

- **CLR "ラッパー":** プロパティの実際の get および set 実装。 これらの実装では、<xref:System.Windows.DependencyObject.GetValue%2A> と <xref:System.Windows.DependencyObject.SetValue%2A> の呼び出しで使用することにより依存関係プロパティ識別子を組み込んで、WPF プロパティ システムを使用するプロパティを補助します。

次の例では、`IsSpinning` 依存関係プロパティを定義し、<xref:System.Windows.DependencyProperty> 識別子とサポートされるプロパティの関係を示します。

[!code-csharp[PropertiesOvwSupport#DPFormBasic](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page4.xaml.cs#dpformbasic)]
[!code-vb[PropertiesOvwSupport#DPFormBasic](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertiesOvwSupport/visualbasic/page4.xaml.vb#dpformbasic)]  
  
プロパティとそれを補足する <xref:System.Windows.DependencyProperty> フィールドの名前付け規則は重要です。 フィールドの名前は常にプロパティの名前であり、サフィックス `Property` が追加されます。 この規則とその理由の詳細については、「[カスタム依存関係プロパティ](custom-dependency-properties.md)」を参照してください。  

## <a name="setting-property-values"></a>プロパティ値の設定
コードまたは XAML でプロパティを設定できます。

### <a name="setting-property-values-in-xaml"></a>XAML でのプロパティ値の設定 
ボタンの背景色を赤に指定する方法を次の XAML の例に示します。 この例では、生成されたコードで XAML 属性の単純な文字列値が WPF XAML パーサーによって WPF 型 (<xref:System.Windows.Media.SolidColorBrush> を使用した<xref:System.Windows.Media.Color>) に型変換される場合を示します。

[!code-xaml[PropertiesOvwSupport#MostBasicProperty](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/Page1.xaml#mostbasicproperty)]

XAML は、プロパティを設定するためのさまざまな構文形式をサポートします。 特定のプロパティに対してどの構文を使用するかは、プロパティで使用される値型、および型コンバーターの有無などのその他の要素によって決定されます。 プロパティ設定の XAML 構文の詳細については、「[XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)」および「[XAML 構文の詳細](xaml-syntax-in-detail.md)」を参照してください。

属性以外の構文の例として、別のボタンの背景を次の XAML の例に示します。 今回は単純な純色を設定するのではなく、背景をイメージに設定し、そのイメージおよびそのイメージのソースを表す要素を、入れ子にした要素の属性として指定します。 これは、プロパティ要素構文の例です。

[!code-xaml[PropertiesOvwSupport#PESyntaxProperty](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/Page1.xaml#pesyntaxproperty)]

### <a name="setting-properties-in-code"></a>コードでのプロパティの設定
 コードに依存関係プロパティの値を設定するのは、通常、CLR の "ラッパー" によって公開されるセットの実装の呼び出しにすぎません。

[!code-csharp[PropertiesOvwSupport#ProceduralPropertySet](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/Page1.xaml.cs#proceduralpropertyset)]
[!code-vb[PropertiesOvwSupport#ProceduralPropertySet](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertiesOvwSupport/visualbasic/page1.xaml.vb#proceduralpropertyset)]

プロパティ値を取得する場合も、基本的に get "ラッパー" 実装を呼び出します。

[!code-csharp[PropertiesOvwSupport#ProceduralPropertyGet](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/Page1.xaml.cs#proceduralpropertyget)]
 [!code-vb[PropertiesOvwSupport#ProceduralPropertyGet](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertiesOvwSupport/visualbasic/page1.xaml.vb#proceduralpropertyget)]

また、プロパティシステム Api <xref:System.Windows.DependencyObject.GetValue%2A> 呼び出して、直接 <xref:System.Windows.DependencyObject.SetValue%2A> することもできます。 これは通常、既存のプロパティを使用している場合は必要ありません (ラッパーの方が便利で、開発者ツールのプロパティがよりわかりやすくなります) が、Api を直接呼び出すことは、特定のシナリオに適しています。

プロパティは、XAML で設定してから分離コードを介してコードでアクセスすることもできます。 詳細については、「[WPF における分離コードと XAML](code-behind-and-xaml-in-wpf.md)」を参照してください。

## <a name="property-functionality-provided-by-a-dependency-property"></a>依存関係プロパティによって提供されるプロパティ機能
依存関係プロパティは、フィールドによって補足されるプロパティとは対照的に、プロパティの機能を拡張する機能を提供します。 しばしば、そのような関数では、次の特定の機能を表すかサポートします。

- [リソース](#resources)

- [データ バインディング](#data-binding)

- [スタイル](#styles)

- [アニメーション](#animations)

- [メタデータのオーバーライド](#metadata-overrides)

- [プロパティ値の継承](#property-value-inheritance)

- [WPF デザイナーの統合](#wpf-designer-integration)

### <a name="resources"></a>リソース
依存関係プロパティの値は、リソースを参照することによって設定できます。 リソースは通常、ページのルート要素またはアプリケーションの `Resources` プロパティ値として指定されます (これらの場所を使用することが、リソースにアクセスするのに最も便利な方法です)。 次の例では、<xref:System.Windows.Media.SolidColorBrush> リソースを定義する方法を示します。

[!code-xaml[PropertiesOvwSupport#ResourcesResource](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page2.xaml#resourcesresource)]

リソースを定義すると、リソースを参照し、そのリソースを使用してプロパティ値を指定できるようになります。

[!code-xaml[PropertiesOvwSupport#ResourcesReference](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page2.xaml#resourcesreference)]

この特定のリソースは、[DynamicResource マークアップ拡張機能](dynamicresource-markup-extension.md)として参照されます (WPF XAML では、静的リソース参照または動的リソース参照を使用できます)。 動的リソース参照を使用するには、依存関係プロパティに設定している必要があるため、これは具体的には、WPF プロパティ システムによって有効になる動的リソース参照の使用方法になります。 詳細については、「[XAML リソース](xaml-resources.md)」を参照してください。

> [!NOTE]
> リソースはローカル値として扱われます。つまり、別のローカル値を設定すると、リソース参照がなくなります。 詳細については、「[依存関係プロパティ値の優先順位](dependency-property-value-precedence.md)」を参照してください。

### <a name="data-binding"></a>データ バインディング
依存関係プロパティは、データ バインディングを介して値を参照できます。 データ バインドは、XAML で特定のマークアップ拡張機能構文を介して機能するか、コードで <xref:System.Windows.Data.Binding> オブジェクトを介して機能します。 データ バインディングを使用すると、プロパティ値の最終的な決定が、データ ソースから値が取得される実行時まで延期されます。

次の例では、XAML で宣言されたバインディングを使用して、<xref:System.Windows.Controls.Button> の <xref:System.Windows.Controls.ContentControl.Content%2A> プロパティを設定します。 このバインディングは、継承されたデータ コンテキストと、<xref:System.Windows.Data.XmlDataProvider> データ ソース (ここでは示していません) を使用します。 バインディング自体は、データ ソース内で <xref:System.Windows.Data.Binding.XPath%2A> によって目的のソース プロパティを指定します。

[!code-xaml[PropertiesOvwSupport#BasicInlineBinding](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page3.xaml#basicinlinebinding)]

> [!NOTE]
> バインディングはローカル値として扱われます。つまり、別のローカル値を設定すると、バインディングがなくなります。 詳細については、「[依存関係プロパティ値の優先順位](dependency-property-value-precedence.md)」を参照してください。

依存関係プロパティまたは <xref:System.Windows.DependencyObject> クラスは、データ バインディング操作用の <xref:System.Windows.DependencyObject> ソース プロパティ値の変更通知を生成するための <xref:System.ComponentModel.INotifyPropertyChanged> は、ネイティブにはサポートしていません。 データ バインディング ターゲットに対する変更を報告できる、データ バインディングで使用するためのプロパティを作成する方法の詳細については、「[データ バインドの概要](../data/data-binding-overview.md)」を参照してください。

### <a name="styles"></a>スタイル
スタイルおよびテンプレートは、依存関係プロパティの使用に関する 2 つの主なシナリオです。 スタイルは、アプリケーション[!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] を定義するプロパティを設定する際に特に役立ちます。 通常、スタイルは XAML のリソースとして定義されます。 スタイルには通常、特定のプロパティの "setter" および別のプロパティのリアルタイム値に基づいてプロパティ値を変更する "トリガー" が含まれるため、スタイルはプロパティ システムと対話します。

次の例は、(表示されていない <xref:System.Windows.FrameworkElement.Resources%2A> ディクショナリ内に定義される) 非常に単純なスタイルを作成し、そのスタイルを直接 <xref:System.Windows.FrameworkElement.Style%2A> プロパティに <xref:System.Windows.Controls.Button> 用に適用します。 このスタイル内のセッターは、スタイル設定された <xref:System.Windows.Controls.Button> の <xref:System.Windows.Controls.Control.Background%2A> プロパティを緑色に設定します。

[!code-xaml[PropertiesOvwSupport#SimpleStyleDef](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page3.xaml#simplestyledef)]

[!code-xaml[PropertiesOvwSupport#SimpleStyle](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page3.xaml#simplestyle)]

詳しくは、「 [スタイルとテンプレート](../controls/styling-and-templating.md)」をご覧ください。

### <a name="animations"></a>Animations
依存関係プロパティは、アニメーション化することができます。 アニメーションが適用されて実行されると、アニメーション化された値は、それ以外の場合のプロパティの値 (ローカル値など) よりも高い優先順位で動作します。

次の例は、<xref:System.Windows.Controls.Button> プロパティ上の <xref:System.Windows.Controls.Control.Background%2A> をアニメーション化します (技術的には、空白の <xref:System.Windows.Media.SolidColorBrush> を <xref:System.Windows.Controls.Control.Background%2A> として指定するために、プロパティ要素構文を使用して <xref:System.Windows.Controls.Control.Background%2A> がアニメーション化されます。すると、その <xref:System.Windows.Media.SolidColorBrush> の <xref:System.Windows.Media.SolidColorBrush.Color%2A> プロパティが直接アニメーション化されるプロパティになります)。

[!code-xaml[PropertiesOvwSupport#MiniAnimate](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page3.xaml#minianimate)]

プロパティのアニメーション化の詳細については、「[アニメーションの概要](../graphics-multimedia/animation-overview.md)」および「[ストーリーボードの概要](../graphics-multimedia/storyboards-overview.md)」を参照してください。

### <a name="metadata-overrides"></a>メタデータのオーバーライド
依存関係プロパティの特定の動作は、依存関係プロパティを最初に登録したクラスから派生させるときにプロパティのメタデータをオーバーライドすることで変更できます。 メタデータのオーバーライドは、<xref:System.Windows.DependencyProperty> 識別子に依存します。 メタデータのオーバーライドでは、プロパティを再実装する必要はありません。 メタデータの変更は、プロパティ システムでネイティブに処理されます。各クラスは、基本クラスから継承したすべてのプロパティに対して、型ごとに個別のメタデータを保持する可能性があります。

次の例は、依存関係プロパティ <xref:System.Windows.FrameworkElement.DefaultStyleKey%2A> 用にメタデータをオーバーライドします。 この特定の依存関係プロパティ メタデータのオーバーライドは、テーマから既定のスタイルを使用できるコントロールを作成する実装パターンの一部です。

[!code-csharp[PropertiesOvwSupport#OverrideMetadata](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page3.xaml.cs#overridemetadata)]
[!code-vb[PropertiesOvwSupport#OverrideMetadata](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertiesOvwSupport/visualbasic/page3.xaml.vb#overridemetadata)]

プロパティ メタデータをオーバーライドまたは取得する方法の詳細については、「[依存関係プロパティのメタデータ](dependency-property-metadata.md)」を参照してください。

### <a name="property-value-inheritance"></a>プロパティ値の継承
要素は、オブジェクト ツリー内の親から依存関係プロパティの値を継承できます。

> [!NOTE]
> プロパティ値の継承動作は、すべての依存関係プロパティにグローバルに有効にはなりません。これは、継承の計算時間がパフォーマンスに影響するからです。 プロパティ値の継承は通常、特定のシナリオでプロパティ値の継承が適切であると示されるプロパティに対してのみ有効にします。 SDK リファレンスで、依存関係プロパティの「**依存関係プロパティの情報**」セクションを調べて、依存関係プロパティで継承を行うかどうかを決定できます。

次の例ではバインディングを示し、前述のバインディングの例では示されていなかったバインディングのソースを指定する <xref:System.Windows.FrameworkElement.DataContext%2A> プロパティを設定します。 子オブジェクト内の後続のいかなるバインディングでも、ソースを指定する必要はなく、親 <xref:System.Windows.Controls.StackPanel> オブジェクトの <xref:System.Windows.FrameworkElement.DataContext%2A> から継承値を使用できます。 (または、代わりに子オブジェクトが独自の <xref:System.Windows.FrameworkElement.DataContext%2A> または <xref:System.Windows.Data.Binding.Source%2A> を <xref:System.Windows.Data.Binding> に指定するよう選択して、意図的に継承値をそのバインディング内のデータ コンテキストで使用しないようにできます)。

[!code-xaml[PropertiesOvwSupport#InheritanceContext](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page3.xaml#inheritancecontext)]

詳細については、「[プロパティ値の継承](property-value-inheritance.md)」を参照してください。

### <a name="wpf-designer-integration"></a>WPF デザイナーの統合
依存関係プロパティとして実装されているプロパティを持つカスタムコントロールは、Visual Studio のサポートに適した WPF デザイナーを受け取ります。 1 つの例として、 **[プロパティ]** ウィンドウで、直接依存関係プロパティと添付依存関係プロパティを編集できる機能が挙げられます。 詳細については、「[コントロールの作成の概要](../controls/control-authoring-overview.md)」を参照してください。

## <a name="dependency-property-value-precedence"></a>依存関係プロパティ値の優先順位
依存関係プロパティの値を取得する場合、WPF プロパティ システムに関係する他のプロパティに基づく入力のいずれかを介して、そのプロパティに設定された値を取得する可能性があります。 プロパティの値の取得方法に関するさまざまなシナリオが予測可能な方法で相互作用できるように、依存関係プロパティ値の優先順位が存在しています。

例を次に示します。 この例には、すべてのボタンとその <xref:System.Windows.Controls.Control.Background%2A> プロパティに適用され、しかし次いで 1 つのボタンをローカルに設定されている <xref:System.Windows.Controls.Control.Background%2A>値にも指定するスタイルが含まれています。

> [!NOTE]
> SDK ドキュメントでは、依存関係プロパティについて説明するときに、"ローカル値" または "ローカルに設定された値" という用語が使用される場合があります。 ローカルに設定された値は、コードでオブジェクト インスタンスに直接設定されたプロパティ値または XAML で要素の属性として設定されたプロパティ値です。  
  
原則として、最初のボタンではプロパティが 2 回設定されますが、適用される値は 1 つだけで、優先順位が最も高い値が適用されます。 ローカルに設定された値の優先順位が最も高いため (実行中のアニメーションを除きますが、この例ではアニメーションは適用されていません)、最初のボタンの背景に対するスタイル setter の値ではなくローカルに設定された値が使用されます。 2 番目のボタンにはローカル値が設定されていないため (また、優先順位がスタイル setter より高い値が他にないため)、そのボタンの背景はスタイル setter に基づきます。

[!code-xaml[PropertiesOvwSupport#MiniPrecedence](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page3.xaml#miniprecedence)]  

### <a name="why-does-dependency-property-precedence-exist"></a>依存関係プロパティの優先順位が存在する理由
通常、スタイルを常に適用し、個別の要素のローカルに設定された値を無効にすることは望ましくありません (また、一般に、スタイルまたは要素の使用は非常に困難です)。 そのため、スタイルに基づく値は、ローカルに設定された値よりも低い優先順位で動作します。 依存関係プロパティの詳細なリストおよび依存関係プロパティの有効値を決める要因については、「[依存関係プロパティ値の優先順位](dependency-property-value-precedence.md)」を参照してください。

> [!NOTE]
> WPF 要素で定義されるプロパティには、依存関係プロパティではないものが多数あります。 概してプロパティは、プロパティ システムによって可能になる 1 つ以上のシナリオ (データ バインディング、スタイル設定、アニメーション、既定値のサポート、継承、添付プロパティ、または無効化) をサポートする必要がある場合にのみ依存関係プロパティとして実装されます。

## <a name="learning-more-about-dependency-properties"></a>依存関係プロパティの詳細情報  

- 添付プロパティは、XAML で特殊な構文をサポートするプロパティの一種です。 添付プロパティは、多くの場合、共通言語ランタイム (CLR) プロパティとの対応が1:1 ではなく、必ずしも依存関係プロパティではありません。 添付プロパティの一般的な目的は、親要素と子要素がどちらも、クラス メンバー リストの一部としてそのプロパティを処理しない場合でも、子要素が親要素にプロパティ値を報告できるようにすることです。 1 つの主要なシナリオは、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] でどのように表示するかを、子要素を有効にして親に通知します。例については、「<xref:System.Windows.Controls.DockPanel.Dock%2A>」または「<xref:System.Windows.Controls.Canvas.Left%2A>」を参照してください。 詳細については、「[添付プロパティの概要](attached-properties-overview.md)」を参照してください。

- コンポーネントまたはアプリケーションの開発者は、データ バインディングやスタイルのサポートなどの機能を有効にするために、または無効化および値の強制のサポートのために、独自の依存関係プロパティを作成できます。 詳細については、「[カスタム依存関係プロパティ](custom-dependency-properties.md)」を参照してください。

- 依存関係プロパティは通常、インスタンスにアクセスできる呼び出し元がアクセス可能か、少なくとも検出可能なパブリック プロパティと見なされます。 詳細については、「[依存関係プロパティのセキュリティ](dependency-property-security.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [カスタム依存関係プロパティ](custom-dependency-properties.md)
- [読み取り専用の依存関係プロパティ](read-only-dependency-properties.md)
- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
- [WPF アーキテクチャ](wpf-architecture.md)
