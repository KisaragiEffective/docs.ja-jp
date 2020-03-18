---
title: 依存関係プロパティ値の優先順位
ms.date: 03/30/2017
helpviewer_keywords:
- dependency properties [WPF], classes as owners
- dependency properties [WPF], metadata
- classes [WPF], owners of dependency properties
- metadata [WPF], dependency properties
ms.assetid: 1fbada8e-4867-4ed1-8d97-62c07dad7ebc
ms.openlocfilehash: a9ff3a4f6ac08a0f7ec6dd9fc26bf190f43f3584
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095204"
---
# <a name="dependency-property-value-precedence"></a>依存関係プロパティ値の優先順位
<a name="introduction"></a>このトピックでは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] プロパティ システムの動作が依存関係プロパティの値に与える影響と、システムのさまざまな部分がプロパティの有効な値に適用する優先順位について説明します。  

<a name="prerequisites"></a>   
## <a name="prerequisites"></a>前提条件  
 このトピックは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] クラスの既存の依存関係プロパティのコンシューマーの観点から依存関係プロパティを理解しており、「[依存関係プロパティの概要](dependency-properties-overview.md)」を読んでいることを前提としています。 このトピックの例について理解するには、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] および [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションの記述方法について知っておく必要もあります。  
  
<a name="intro"></a>   
## <a name="the-wpf-property-system"></a>WPF プロパティ システム  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プロパティ システムは、依存関係プロパティの値をさまざまな要因によって決定し、リアルタイム プロパティの検証や遅延バインディングなどの機能を有効にし、他のプロパティの値の変更を関連プロパティに通知する、強力な手段を提供します。 依存関係プロパティの値の決定に使われる正確な順序とロジックはかなり複雑です。 この順序を知っておくと、不要なプロパティの設定を避けることができ、依存関係プロパティの値を設定または予測しようとしても期待通りにならない正確な理由についての混乱も解決される可能性があります。  
  
<a name="multiple_sets"></a>   
## <a name="dependency-properties-might-be-set-in-multiple-places"></a>依存関係プロパティは複数の場所で "設定" される可能性がある  
 次に示すのは、同じプロパティ (<xref:System.Windows.Controls.Control.Background%2A>) に、値に影響を与える可能性のある3つの異なる "set" 操作がある [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] の例です。  
  
 [!code-xaml[PropertiesOvwSupport#DPPrecedence](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page4.xaml#dpprecedence)]  
  
 さて、赤、緑、青のどの色になるでしょうか。  
  
 アニメーション化された値および強制型変換の場合を除き、ローカル プロパティ セットは最高の優先順位で設定されます。 ローカルに値を設定する場合は、スタイルまたはコントロール テンプレートであっても、その値が採用されるものと期待できます。 この例では、<xref:System.Windows.Controls.Control.Background%2A> はローカルに Red に設定されています。 したがって、このスコープで定義されているスタイルは、そのスコープ内のその型のすべての要素に適用される暗黙的なスタイルであっても、<xref:System.Windows.Controls.Control.Background%2A> プロパティの値を指定するための最も優先順位ではありません。  Button インスタンスからローカル値の Red を削除すると、スタイルが優先されるようになり、ボタンの Background の値はスタイルから取得されます。  スタイル内ではトリガーが優先されるので、ボタンはマウスでポイントすると青になり、それ以外の場合は緑になります。  
  
<a name="listing"></a>   
## <a name="dependency-property-setting-precedence-list"></a>依存関係プロパティの設定の優先順位一覧  
 次に示すのは、依存関係プロパティの実行時の値を割り当てるときにプロパティ システムが使う明確な順序です。 優先順位が高いものから順に示されています。 この一覧では、「[依存関係プロパティの概要](dependency-properties-overview.md)」で一般化されていたものを拡張してあります。  
  
1. **プロパティ システムの強制型変換。** 強制型変換について詳しくは、後の「[強制型変換、アニメーション、基本値](#animations)」をご覧ください。  
  
2. **アクティブなアニメーション、または保留動作のアニメーション。** 実際的な効果を得るためには、プロパティのアニメーションは、基本 (アニメーション化されていない) 値 (ローカルに設定された値であっても) より高い優先順位を持つことができる必要があります。 詳しくは、後の「[強制型変換、アニメーション、基本値](#animations)」をご覧ください。  
  
3. **ローカル値。** ローカル値は、"ラッパー" プロパティを使用して設定できます。これは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]で属性またはプロパティ要素として設定したり、特定のインスタンスのプロパティを使用して <xref:System.Windows.DependencyObject.SetValue%2A> API を呼び出したりすることにも相当します。 バインドまたはリソースを使ってローカル値を設定する場合、これらは直接値が設定された場合のように優先されます。  
  
4. **TemplatedParent テンプレート プロパティ。** 要素には、テンプレートの一部として作成された場合 (<xref:System.Windows.Controls.ControlTemplate> または <xref:System.Windows.DataTemplate>)、<xref:System.Windows.FrameworkElement.TemplatedParent%2A> があります。 これが適用されるケースについて詳しくは、後の「[TemplatedParent](#templatedparent)」をご覧ください。 テンプレート内では、次の優先順位が適用されます。  
  
    1. <xref:System.Windows.FrameworkElement.TemplatedParent%2A> テンプレートからのトリガー。  
  
    2. <xref:System.Windows.FrameworkElement.TemplatedParent%2A> テンプレートのプロパティセット (通常は [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 属性)。  
  
5. **暗黙的スタイル。** `Style` プロパティのみに適用されます。 `Style` プロパティは、その要素の型と一致するキーを持つスタイル リソースによって設定されます。 そのスタイル リソースは、ページまたはアプリケーション内に存在する必要があります。暗黙的スタイル リソースの参照はテーマまでは及びません。  
  
6. **スタイルのトリガー。** ページまたはアプリケーションに含まれるスタイル内のトリガー (これらのスタイルは、明示的スタイルまたは暗黙的スタイルどちらの場合もありますが、優先順位の低い既定スタイルではありません)。  
  
7. **テンプレートのトリガー。** スタイル内のテンプレートまたは直接適用されたテンプレートからのトリガーです。  
  
8. **スタイルのセッター。** ページまたはアプリケーションのスタイル内の <xref:System.Windows.Setter> の値。  
  
9. **既定 (テーマ) のスタイル。** これが適用される場合、およびテーマ スタイルとテーマ スタイル内のテンプレートの関係について詳しくは、後の「[既定 (テーマ) のスタイル](#themestyles)」をご覧ください。 既定のスタイル内では、次の優先順位が適用されます。  
  
    1. テーマ スタイル内のアクティブなトリガー。  
  
    2. テーマ スタイル内のセッター。  
  
10. **継承。** いくつかの依存関係プロパティは親要素から子要素に値を継承するので、アプリケーション全体で各要素に値を明示的に設定する必要はありません。 詳しくは、「[プロパティ値の継承](property-value-inheritance.md)」をご覧ください。  
  
11. **依存関係プロパティのメタデータの既定値。** どの依存関係プロパティも、その特定のプロパティがプロパティ システムに登録されるときに設定される既定値を持つことができます。 また、依存関係プロパティを継承する派生クラスには、型ごとにそのメタデータ (既定値を含む) をオーバーライドするオプションがあります。 詳しくは、「[依存関係プロパティのメタデータ](dependency-property-metadata.md)」をご覧ください。 継承は既定値の前にチェックされるため、継承されるプロパティの場合、親要素の既定値の方が子要素より優先されます。  その結果、継承可能なプロパティがどこでも設定されていない場合は、ルートまたは親で指定されている既定値が、子要素の既定値の代わりに使われます。  
  
<a name="templatedparent"></a>   
## <a name="templatedparent"></a>TemplatedParent  
 優先順位の項目としての TemplatedParent は、標準アプリケーション マークアップで直接宣言された要素のプロパティには適用されません。 TemplatedParent の概念は、テンプレートの適用によって作成されるビジュアル ツリー内の子項目に対してのみ存在します。 プロパティシステムが <xref:System.Windows.FrameworkElement.TemplatedParent%2A> テンプレート内で値を検索すると、その要素を作成したテンプレートが検索されます。 <xref:System.Windows.FrameworkElement.TemplatedParent%2A> テンプレートのプロパティ値は、通常、子要素のローカル値として設定されているかのように機能しますが、テンプレートは共有される可能性があるため、この優先順位はローカル値とは異なります。 詳細については、<xref:System.Windows.FrameworkElement.TemplatedParent%2A> を参照してください。  
  
<a name="style_property"></a>   
## <a name="the-style-property"></a>スタイル プロパティ  
 前に説明した検索の順序は、使用可能なすべての依存関係プロパティ (<xref:System.Windows.FrameworkElement.Style%2A> プロパティ) に適用されます。 <xref:System.Windows.FrameworkElement.Style%2A> プロパティは、それ自体をスタイル設定できないので一意であるため、優先順位の項目 5 ~ 8 は適用されません。 また、アニメーション化または強制型変換 <xref:System.Windows.FrameworkElement.Style%2A> は推奨されません (<xref:System.Windows.FrameworkElement.Style%2A> アニメーション化するには、カスタムアニメーションクラスが必要です)。 これにより、<xref:System.Windows.FrameworkElement.Style%2A> プロパティが設定される3つの方法があります。  
  
- **明示的スタイル。** <xref:System.Windows.FrameworkElement.Style%2A> プロパティは直接設定されます。 ほとんどの場合、スタイルはインラインでは定義されず、代わりにリソースとして明示的なキーにより参照されます。 この場合、スタイル プロパティ自体は、ローカル値と同じように扱われます (優先順位の項目 3)。  
  
- **暗黙的スタイル。** <xref:System.Windows.FrameworkElement.Style%2A> プロパティが直接設定されていません。 ただし、<xref:System.Windows.FrameworkElement.Style%2A> はリソース参照シーケンス (ページ、アプリケーション) の一部のレベルに存在し、スタイルが適用される型と一致するリソースキーを使用してキーが付けられます。 この場合、<xref:System.Windows.FrameworkElement.Style%2A> プロパティ自体は、シーケンス内で項目5として指定された優先順位によって動作します。 この条件は、<xref:System.Windows.FrameworkElement.Style%2A> プロパティに対して <xref:System.Windows.DependencyPropertyHelper> を使用し、結果の <xref:System.Windows.BaseValueSource.ImplicitStyleReference> を検索することによって検出できます。  
  
- **既定のスタイル** (別称**テーマ スタイル**)。 <xref:System.Windows.FrameworkElement.Style%2A> プロパティは直接設定されず、実際には実行時まで `null` として読み取られます。 この場合、スタイルは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プレゼンテーション エンジンの一部である実行時テーマ評価によって決定されます。  
  
 テーマに含まれていない暗黙のスタイルの場合、型は厳密に一致している必要があります `MyButton` `Button`派生クラスは `Button`のスタイルを暗黙的に使用しません。  
  
<a name="themestyles"></a>   
## <a name="default-theme-styles"></a>既定 (テーマ) のスタイル  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] に付属するすべてのコントロールには、既定のスタイルがあります。 既定のスタイルはテーマによって異なる場合があり、そのため、この既定のスタイルはテーマのスタイルとも呼ばれます。  
  
 コントロールの既定のスタイルに含まれる最も重要な情報は、コントロールテンプレートです。これは、<xref:System.Windows.Controls.Control.Template%2A> プロパティのセッターとしてテーマのスタイルに存在します。 既定のスタイルにテンプレートがない場合、カスタム スタイルの一部としてカスタム テンプレートがないコントロールは、まったく表示されません。 既定のスタイルのテンプレートは、各コントロールの外観に基本的な構造を提供し、テンプレートのビジュアル ツリーで定義されているプロパティと、対応するコントロール クラスの間の接続を定義します。 各コントロールでは、テンプレートを完全に置き換えることなくコントロールの外観に影響を与えることができる、一連のプロパティが公開されています。 たとえば、<xref:System.Windows.Controls.Primitives.ScrollBar>のコンポーネントである <xref:System.Windows.Controls.Primitives.Thumb> コントロールの既定の外観を考えてみます。  
  
 <xref:System.Windows.Controls.Primitives.Thumb> には、カスタマイズ可能な特定のプロパティがあります。 <xref:System.Windows.Controls.Primitives.Thumb> の既定のテンプレートでは、いくつかの入れ子になった <xref:System.Windows.Controls.Border> コンポーネントがある基本構造体/ビジュアルツリーを作成して、ベベルの外観を作成します。 テンプレートの一部であるプロパティが <xref:System.Windows.Controls.Primitives.Thumb> クラスによるカスタマイズのために公開されるように設定されている場合、そのプロパティは、テンプレート内で[TemplateBinding](templatebinding-markup-extension.md)によって公開される必要があります。 <xref:System.Windows.Controls.Primitives.Thumb>の場合は、これらの境界のさまざまなプロパティが <xref:System.Windows.Controls.Border.Background%2A> や <xref:System.Windows.Controls.Border.BorderThickness%2A>などのプロパティにテンプレートバインドを共有します。 しかし、他の特定のプロパティや視覚的な配置は、コントロール テンプレートにハードコードされているか、またはテーマから直接取得される値にバインドされており、テンプレート全体を置き換えない限り変更できません。 一般に、プロパティがテンプレート化された親から取得され、テンプレートのバインドによって公開されない場合は、それをターゲットにする簡単な方法がないため、そのプロパティをスタイルによって調整することはできません。 ただし、適用されるテンプレートのプロパティ値継承または既定値によって、そのプロパティに影響を与えることはできます。  
  
 テーマのスタイルでは、定義のキーとして型を使います。 ただし、特定の要素インスタンスにテーマを適用すると、コントロールの <xref:System.Windows.FrameworkElement.DefaultStyleKey%2A> プロパティをチェックすることによって、この型のテーマの検索が実行されます。 これは、暗黙的スタイルで行われるリテラル型の使用とは対照的です。 <xref:System.Windows.FrameworkElement.DefaultStyleKey%2A> の値は、実装側によって変更されていない場合でも派生クラスに継承されます (プロパティを変更する場合は、プロパティレベルでオーバーライドするのではなく、プロパティメタデータの既定値を変更することをお勧めします)。 この間接参照により、基底クラスは、他の方法ではスタイルを持たない派生要素に対してテーマのスタイルを定義できます (または、さらに重要なのは、スタイル内にテンプレートを持たず、既定の外観がないということです)。 したがって、<xref:System.Windows.Controls.Button> から `MyButton` を派生させることができ、<xref:System.Windows.Controls.Button> の既定のテンプレートが引き続き取得されます。 `MyButton` のコントロールの作成者であり、別の動作が必要な場合は `MyButton` で <xref:System.Windows.FrameworkElement.DefaultStyleKey%2A> の依存関係プロパティのメタデータをオーバーライドして別のキーを返し、`MyButton` コントロールでパッケージ化する必要がある `MyButton` のテンプレートなど、関連するテーマスタイルを定義することができます。 テーマ、スタイル、コントロールの作成について詳しくは、「[コントロールの作成の概要](../controls/control-authoring-overview.md)」をご覧ください。  
  
<a name="resources"></a>   
## <a name="dynamic-resource-references-and-binding"></a>動的リソース参照とバインド  
 動的リソース参照とバインド操作には、それらが設定される場所での優先順位が適用されます。 たとえば、ローカル値に適用される動的リソースは優先順位の項目 3 に準拠し、テーマ スタイル内のプロパティ セッターに対するバインドには優先順位の項目 9 が適用されます。 動的リソース参照とバインドはどちらもアプリケーションの実行時状態から値を取得できる必要があるので、特定のプロパティに対するプロパティ値の優先順位を決定する実際のプロセスは、実行時まで拡張されます。  
  
 厳密に言うと、動的リソース参照はプロパティ システムの一部ではありませんが、上で示したシーケンスに対応する独自の参照順序を持ちます。 その優先順位について詳しくは、「[XAML リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)」をご覧ください。 基本的な優先順位をまとめると、ページ ルートの要素、アプリケーション、テーマ、システムになります。  
  
 動的リソースとバインドには設定された場所での優先順位がありますが、値は遅延されます。 その 1 つの帰結として、ローカル値に動的リソースまたはバインドを設定した場合、ローカル値を変更すると動的リソースまたはバインドが完全に置き換えられます。 <xref:System.Windows.DependencyObject.ClearValue%2A> メソッドを呼び出してローカルに設定された値をクリアした場合でも、動的リソースまたはバインドは復元されません。 実際には、動的リソースまたはバインドがある (リテラルローカル値がない) プロパティで <xref:System.Windows.DependencyObject.ClearValue%2A> を呼び出すと、<xref:System.Windows.DependencyObject.ClearValue%2A> 呼び出しによってクリアされます。  
  
<a name="setcurrentvalue"></a>   
## <a name="setcurrentvalue"></a>SetCurrentValue  
 <xref:System.Windows.DependencyObject.SetCurrentValue%2A> メソッドは、プロパティを設定するもう1つの方法ですが、優先順位ではありません。 代わりに、<xref:System.Windows.DependencyObject.SetCurrentValue%2A> では、以前の値のソースを上書きすることなく、プロパティの値を変更できます。 値をローカル値の優先順位に設定せずに値を設定する場合はいつでも <xref:System.Windows.DependencyObject.SetCurrentValue%2A> を使用できます。 たとえば、プロパティがトリガーによって設定され、<xref:System.Windows.DependencyObject.SetCurrentValue%2A>によって別の値が割り当てられた場合でも、プロパティシステムはトリガーを尊重し、トリガーのアクションが発生するとプロパティが変更されます。 <xref:System.Windows.DependencyObject.SetCurrentValue%2A> を使用すると、優先順位の高いソースを指定せずに、プロパティの値を変更できます。 同様に、<xref:System.Windows.DependencyObject.SetCurrentValue%2A> を使用して、バインディングを上書きせずにプロパティの値を変更することもできます。  
  
<a name="animations"></a>   
## <a name="coercion-animations-and-base-value"></a>強制型変換、アニメーション、基本値  
 強制型変換とアニメーションは、この SDK 全体で "ベース値" と呼ばれる値に対して動作します。 したがって、基本値とは、項目 2 に達するまで項目をさかのぼって評価されることにより決定される値です。  
  
 アニメーションの場合、アニメーションで特定の動作に対して "From" と "To" の両方が指定されていない場合、またはアニメーションが完了すると基本値に意図的に戻る場合は、基本値を使ってアニメーション化される値に影響を及ぼすことができます。 実際にどうなるのかを見るには、「[From, To, and By Animation Target Values Sample](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/TargetValues)」(アニメーション ターゲット値 From、To、By のサンプル) をご覧ください。 この例で、四角形の高さのローカル値を、初期ローカル値がアニメーションの "From" と異なるように設定してみます。 アニメーションが "From" の値を使ってすぐに開始し、開始すると基本値を置き換えることがわかります。 アニメーションは、停止 <xref:System.Windows.Media.Animation.FillBehavior>を指定することによって完了した後、アニメーションの前に見つかった値に戻るように指定できます。 その後は、通常の優先順位が基本値の決定に使用されます。  
  
 1 つのプロパティに複数のアニメーションが適用され、各アニメーションが値の優先順位の異なるポイントから定義されている場合があります。 ただし、これらのアニメーションは、優先順位の高いアニメーションから単純に適用されるのではなく、値が合成される可能性があります。 これは、アニメーションの定義方法と、アニメーション化される値の型に依存します。 プロパティのアニメーション化について詳しくは、「[アニメーションの概要](../graphics-multimedia/animation-overview.md)」をご覧ください。  
  
 強制型変換は、すべての最高レベルで適用されます。 既に実行されているアニメーションであっても値の強制型変換が適用されます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の特定の既存の依存関係プロパティには、組み込みの強制型変換があります。 カスタム依存関係プロパティの場合は、<xref:System.Windows.CoerceValueCallback> を記述し、プロパティを作成するときにメタデータの一部としてコールバックを渡すことによって、カスタム依存関係プロパティの強制型変換の動作を定義します。 派生クラスでプロパティのメタデータをオーバーライドすることにより、既存のプロパティの強制型変換の動作をオーバーライドすることもできます。 強制型変換と基本値の相互作用は、その時点で強制型変換に対する制約が存在するものとして適用されるように行われますが、基本値はそれでも保持されます。 したがって、強制型変換の制約が後で無効になった場合、強制型変換はその基本値に可能な最も近い値を返し、プロパティに対する強制型変換の影響はすべての制約が無効になるとすぐに終了する可能性があります。 強制型変換の動作について詳しくは、「[依存関係プロパティのコールバックと検証](dependency-property-callbacks-and-validation.md)」をご覧ください。  
  
<a name="triggers"></a>   
## <a name="trigger-behaviors"></a>トリガー動作  
 コントロールでは、テーマの既定のスタイルの一部としてトリガー動作が定義されていることがよくあります。 コントロールにローカル プロパティを設定すると、ユーザー駆動のイベントに対してトリガーが視覚的または動作的に応答できなくなる可能性があります。 プロパティトリガーの最も一般的な用途は、コントロールまたは状態プロパティ (<xref:System.Windows.Controls.Primitives.Selector.IsSelected%2A>など) です。 たとえば、既定では <xref:System.Windows.Controls.Button> が無効になっている場合 (<xref:System.Windows.UIElement.IsEnabled%2A> のトリガーが `false`)、テーマスタイルの <xref:System.Windows.Controls.Control.Foreground%2A> 値によって、コントロールが "淡色表示されます" と表示されます。 ただし、ローカルの <xref:System.Windows.Controls.Control.Foreground%2A> 値を設定した場合、このプロパティによってトリガーされるシナリオであっても、却下の通常の灰色の色はローカルのプロパティセットによって優先されます。 テーマ レベルのトリガー動作があるプロパティの値を設定するときは注意し、そのコントロールの目的のユーザー エクスペリエンスに過度に干渉していないことを確認する必要があります。  
  
<a name="clearvalue"></a>   
## <a name="clearvalue-and-value-precedence"></a>ClearValue と値の優先順位  
 <xref:System.Windows.DependencyObject.ClearValue%2A> メソッドは、要素に設定されている依存関係プロパティからローカルに適用された値をクリアするための便利な手段を提供します。 ただし、<xref:System.Windows.DependencyObject.ClearValue%2A> を呼び出すと、プロパティの登録時にメタデータに設定された既定値が新しい有効値になるという保証はありません。 値の優先順位に関係する他のすべての要因はアクティブなままです。 ローカルで設定された値が優先順位のシーケンスから削除されるだけです。 たとえば、プロパティがテーマスタイルによっても設定されているプロパティで <xref:System.Windows.DependencyObject.ClearValue%2A> を呼び出すと、テーマの値が、メタデータベースの既定値ではなく、新しい値として適用されます。 すべてのプロパティ値の参加者をプロセスから除外し、その値を登録済みメタデータの既定値に設定する場合は、依存関係プロパティのメタデータに対してクエリを実行することで、既定値を取得できます。その後、既定値を使用して、プロパティをローカルに設定し、<xref:System.Windows.DependencyObject.SetValue%2A>を呼び出すことができます。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.DependencyObject>
- <xref:System.Windows.DependencyProperty>
- [依存関係プロパティの概要](dependency-properties-overview.md)
- [カスタム依存関係プロパティ](custom-dependency-properties.md)
- [依存関係プロパティのコールバックと検証](dependency-property-callbacks-and-validation.md)
