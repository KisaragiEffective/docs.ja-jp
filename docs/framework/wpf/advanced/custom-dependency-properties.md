---
title: カスタム依存関係プロパティ
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- implementing [WPF], wrappers
- registering properties [WPF]
- properties [WPF], metadata
- metadata [WPF], for properties
- custom dependency properties [WPF]
- properties [WPF], registering
- wrappers [WPF], implementing
- dependency properties [WPF], custom
ms.assetid: e6bfcfac-b10d-4f58-9f77-a864c2a2938f
ms.openlocfilehash: 8e3ac7207a5ef05b94e97f005ecd17d5078669a4
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73740885"
---
# <a name="custom-dependency-properties"></a>カスタム依存関係プロパティ

このトピックは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] アプリケーション開発者およびコンポーネントの作成者が、カスタム依存関係プロパティを作成したくなる理由を説明し、実装手順にくわえ、プロパティのパフォーマンス、使いやすさ、または多用性を向上させることができるいくつかの実装オプションについて説明します。

<a name="prerequisites"></a>

## <a name="prerequisites"></a>必要条件

このトピックは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] クラスの既存の依存関係プロパティのコンシューマーの観点から依存関係プロパティを理解しており、「[依存関係プロパティの概要](dependency-properties-overview.md)」というトピックを読んでいることを前提としています。 このトピックの例に従うには、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] について理解し、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションの作成方法に精通している必要があります。

<a name="whatis"></a>

## <a name="what-is-a-dependency-property"></a>依存関係プロパティとは

それ以外の場合は、共通言語ランタイム (CLR) プロパティを有効にして、スタイル設定、データバインディング、継承、アニメーション、および既定値をサポートすることができます。これは、依存関係プロパティとして実装することによって行います。 依存関係プロパティは、<xref:System.Windows.DependencyProperty.Register%2A> メソッド (または <xref:System.Windows.DependencyProperty.RegisterReadOnly%2A>) を呼び出すことによって [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プロパティシステムに登録され、<xref:System.Windows.DependencyProperty> 識別子フィールドによってサポートされるプロパティです。 依存関係プロパティは <xref:System.Windows.DependencyObject> 型によってのみ使用できますが、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] クラス階層では非常に高い <xref:System.Windows.DependencyObject> であるため、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] で使用できるクラスの大部分は依存関係プロパティをサポートできます。 依存関係プロパティの詳細、およびこの SDK での記述に使用される用語と規則の詳細については、「[依存関係プロパティの概要](dependency-properties-overview.md)」を参照してください。

<a name="example_dp"></a>

## <a name="examples-of-dependency-properties"></a>依存関係プロパティの例

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] クラスに実装されている依存関係プロパティの例 <xref:System.Windows.Controls.TextBox.Text%2A> としては、<xref:System.Windows.Controls.Control.Background%2A> プロパティ、<xref:System.Windows.FrameworkElement.Width%2A> プロパティ、およびその他の多くのプロパティがあります。 クラスによって公開される各依存関係プロパティには、その同じクラスで公開される <xref:System.Windows.DependencyProperty> 型の対応するパブリック静的フィールドがあります。 これが依存関係プロパティの識別子です。 この識別子は規則を使用して命名されます。依存関係プロパティの名前と文字列 `Property` がこれに付加されます。 たとえば、<xref:System.Windows.Controls.Control.Background%2A> プロパティの対応する <xref:System.Windows.DependencyProperty> 識別子フィールドは <xref:System.Windows.Controls.Control.BackgroundProperty>です。 識別子は、登録されたときの依存関係プロパティに関する情報を格納します。この識別子は、後で <xref:System.Windows.DependencyObject.SetValue%2A>の呼び出しなど、依存関係プロパティに関連する他の操作に使用されます。

「[依存関係プロパティの概要](dependency-properties-overview.md)」で説明したように、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のすべての依存関係プロパティ (ほとんどの添付プロパティを除く) も、"ラッパー" 実装によって CLR プロパティになります。 そのため、コードから、他の CLR プロパティを使用するのと同じ方法でラッパーを定義する CLR アクセサーを呼び出すことによって、依存関係プロパティを取得または設定できます。 確立された依存関係プロパティのコンシューマーは、通常、基になるプロパティシステムへの接続ポイントである <xref:System.Windows.DependencyObject> メソッド <xref:System.Windows.DependencyObject.GetValue%2A> と <xref:System.Windows.DependencyObject.SetValue%2A>を使用しません。 代わりに、CLR プロパティの既存の実装は、識別子フィールドを適切に使用して、プロパティの `get` および `set` ラッパー実装内で <xref:System.Windows.DependencyObject.GetValue%2A> と <xref:System.Windows.DependencyObject.SetValue%2A> 呼び出されます。 カスタム依存関係プロパティを自分で実装する場合、同様の方法でラッパーを定義します。

<a name="backing_with_dp"></a>

## <a name="when-should-you-implement-a-dependency-property"></a>依存関係プロパティを実装すべき状況

クラスにプロパティを実装するときに、クラスが <xref:System.Windows.DependencyObject>から派生している限り、<xref:System.Windows.DependencyProperty> 識別子を使用してプロパティを返すことができるため、依存関係プロパティにすることができます。 シナリオのニーズによっては、プロパティを依存関係プロパティにすることが必要ない場合や適切でない場合もあります。 場合によっては、プロパティをプライベート フィールドでバッキングする一般的な手法で十分な場合もあります。 ただし、プロパティで次の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 機能の 1 つ以上をサポートする場合には、常に依存関係プロパティとしてプロパティを実装する必要があります。

- プロパティをスタイルで設定可能にする。 詳しくは、「 [スタイルとテンプレート](../controls/styling-and-templating.md)」をご覧ください。

- プロパティでデータ バインディングをサポートする。 データ バインディングの依存関係プロパティの詳細については、「[2 つのコントロールのプロパティをバインドする](../data/how-to-bind-the-properties-of-two-controls.md)」を参照してください。

- プロパティを動的リソース参照で設定可能にする。 詳細については、「[XAML リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)」を参照してください。

- 要素ツリーの親要素からプロパティ値を自動的に継承する。 この場合は、CLR アクセス用のプロパティラッパーも作成する場合でも、<xref:System.Windows.DependencyProperty.RegisterAttached%2A> メソッドに登録します。 詳細については、「[プロパティ値の継承](property-value-inheritance.md)」を参照してください。

- プロパティをアニメーション化できるようにする。 詳しくは、「 [アニメーションの概要](../graphics-multimedia/animation-overview.md)」をご覧ください。

- プロパティの前の値が、プロパティ システム、環境、または、ユーザーによって行われたアクションによって変更された場合、または読み取りおよびスタイルの使用によって変更された場合に、プロパティ システムに報告させる。 プロパティのメタデータを使用すると、プロパティ システムがプロパティ値が明らかに変更されたと判断するたびに呼び出されるコールバック メソッドをプロパティで指定できます。 関連する概念は、プロパティ値の強制型変換です。 詳しくは、「[依存関係プロパティのコールバックと検証](dependency-property-callbacks-and-validation.md)」を参照してください。

- プロパティ値の変更に、要素のビジュアルを再構成するためのレイアウト システムが必要かどうかを報告するなど、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プロセスでも使用されている確立されたメタデータ規則を使用する。 または、派生クラスが既定値などのメタデータに基づく特性を変更できるように、メタデータのオーバーライドを使用できるようする。

- カスタムコントロールのプロパティを使用して、 **[プロパティ]** ウィンドウの編集など、VISUAL Studio WPF デザイナーのサポートを受ける必要があります。 詳細については、「[コントロールの作成の概要](../controls/control-authoring-overview.md)」を参照してください。

これらのシナリオを検討するときに、完全に新しいプロパティを実装するよりも、既存の依存関係プロパティのメタデータをオーバーライドすることで、シナリオを実現できるかどうかも考慮する必要があります。 メタデータのオーバーライドが実用的かどうかは、シナリオとそのシナリオが既存の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 依存関係プロパティとクラスでの実装にどのくらい似ているかによって異なります。 既存のプロパティでメタデータをオーバーライドする方法の詳細については、「[依存関係プロパティのメタデータ](dependency-property-metadata.md)」を参照してください。

<a name="checklist"></a>

## <a name="checklist-for-defining-a-dependency-property"></a>依存関係プロパティを定義するためのチェックリスト

依存関係プロパティの定義は、次の 4 つの異なる概念で構成されます。 これらの概念は、一部は実装で最終的に 1 行のコードとして結合されるため、必ずしも厳密な手順である必要はありません。

- (省略可能) 依存関係プロパティのプロパティ メタデータを作成します。

- 所有者型とプロパティ値の型を指定して、プロパティ システムにプロパティ名を登録します。 プロパティのメタデータも指定します (使用している場合)。

- 所有者の種類で `public` `static` `readonly` フィールドとして <xref:System.Windows.DependencyProperty> 識別子を定義します。

- 依存関係プロパティの名前と一致する名前を持つ CLR "ラッパー" プロパティを定義します。 CLR "ラッパー" プロパティの `get` と `set` アクセサーを実装して、それをバッキングする依存関係プロパティと接続します。

<a name="registering"></a>

### <a name="registering-the-property-with-the-property-system"></a>プロパティ システムにプロパティを登録する

プロパティを依存関係プロパティにするためには、そのプロパティをプロパティ システムが保持するテーブルに登録し、その後のプロパティ システム操作で修飾子として使用する一意の識別子をプロパティに設定します。 これらの操作は、内部操作である場合もあれば、プロパティシステム Api を呼び出す独自のコードである場合もあります。 プロパティを登録するには、クラスの本体 (クラス内では、メンバー定義の外部) で <xref:System.Windows.DependencyProperty.Register%2A> メソッドを呼び出します。 識別子フィールドは、戻り値として <xref:System.Windows.DependencyProperty.Register%2A> メソッドの呼び出しによっても提供されます。 <xref:System.Windows.DependencyProperty.Register%2A> 呼び出しが他のメンバー定義の外部で行われる理由は、この戻り値を使用して、クラスの一部として <xref:System.Windows.DependencyProperty> 型の `public` `static` `readonly` フィールドを割り当てて作成するためです。 このフィールドは、依存関係プロパティの識別子になります。

[!code-csharp[WPFAquariumSln#RegisterAG](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFAquariumSln/CSharp/WPFAquariumObjects/Class1.cs#registerag)]
[!code-vb[WPFAquariumSln#RegisterAG](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFAquariumSln/visualbasic/wpfaquariumobjects/class1.vb#registerag)]

<a name="nameconventions"></a>

### <a name="dependency-property-name-conventions"></a>依存関係プロパティの命名規則

依存関係プロパティについては、確立されている命名規則があり、例外的な状況を除き、必ず従う必要があります。

依存関係プロパティ自体には、この例のように基本名 "AquariumGraphic" が設定されます。この例は、<xref:System.Windows.DependencyProperty.Register%2A>の最初のパラメーターとして指定されています。 この名前は、それぞれの登録型内で一意である必要があります。 基本型から継承された依存関係プロパティは、登録型の一部に既になっていると見なされ、継承されたプロパティの名前は、再度登録することはできません。 しかし、その依存関係プロパティが継承されていない場合でも、依存関係プロパティの所有者としてクラスを追加する方法があります。詳細については、「[依存関係プロパティのメタデータ](dependency-property-metadata.md)」を参照してください。

識別子フィールドを作成するときに、登録したプロパティの名前にサフィックス `Property` を付けて、このフィールドに名前を付けます。 このフィールドは依存関係プロパティの識別子で、後で独自のコードによってプロパティにアクセスする他のコードによって、ラッパーに対して実行する <xref:System.Windows.DependencyObject.SetValue%2A> および <xref:System.Windows.DependencyObject.GetValue%2A> 呼び出しの入力として使用されます。は、プロパティシステムによって許可される外部コードアクセス、および [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサによって許可される可能性があります。

> [!NOTE]
> クラス本体で依存関係プロパティを定義することは一般的な実装ですが、クラスの静的コンストラクターで依存関係プロパティを定義することもできます。 依存関係プロパティを初期化するために複数行のコードが必要な場合には、このアプローチが適している場合があります。

<a name="wrapper1"></a>

### <a name="implementing-the-wrapper"></a>"ラッパー" を実装する

ラッパーの実装では、`get` の実装で <xref:System.Windows.DependencyObject.GetValue%2A> を呼び出す必要があります。また、`set` 実装で <xref:System.Windows.DependencyObject.SetValue%2A> します (わかりやすくするために、元の登録呼び出しとフィールドもここに表示されています)。

すべての例外的な状況では、ラッパーの実装では <xref:System.Windows.DependencyObject.GetValue%2A> と <xref:System.Windows.DependencyObject.SetValue%2A> のアクションのみを実行する必要があります。 この理由については、「[XAML 読み込みと依存関係プロパティ](xaml-loading-and-dependency-properties.md)」のトピックで説明しています。

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] クラスで提供されているすべての既存のパブリックな依存関係プロパティは、この単純なラッパー実装モデルを使用します。依存関係プロパティのしくみの複雑さの大部分は、本質的にプロパティ システムの動作であるか、強制変換やプロパティ メタデータを通じたプロパティ変更のコールバックなど、その他の概念を通じて実装されます。

[!code-csharp[WPFAquariumSln#AGWithWrapper](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFAquariumSln/CSharp/WPFAquariumObjects/Class1.cs#agwithwrapper)]
[!code-vb[WPFAquariumSln#AGWithWrapper](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFAquariumSln/visualbasic/wpfaquariumobjects/class1.vb#agwithwrapper)]

この場合も、慣例により、ラッパープロパティの名前は、選択した名前と同じである必要があります。また、プロパティを登録した <xref:System.Windows.DependencyProperty.Register%2A> 呼び出しの最初のパラメーターとして指定する必要があります。 プロパティが規則に従っていない場合、すべての可能な使用を無効にする必要はありませんが、次のような注目すべき問題がいくつか発生します。

- スタイルとテンプレートの一部が機能しない。

- ほとんどのツールとデザイナーは、適切に [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] をシリアル化するため、またはプロパティごとのレベルでのデザイナー環境のサポートを提供するために命名規則に依存する必要があります。

- [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ローダーの現在の実装は、属性値を処理するときに、ラッパーを完全にバイパスして、命名規則に依存しています。 詳しくは、「[XAML 読み込みと依存関係プロパティ](xaml-loading-and-dependency-properties.md)」を参照してください。

<a name="metadata"></a>

### <a name="property-metadata-for-a-new-dependency-property"></a>新しい依存関係プロパティのプロパティ メタデータ

依存関係プロパティを登録するときに、プロパティ システムを通じて登録すると、プロパティの特性を格納するメタデータ オブジェクトが作成されます。 これらの特性の多くには、プロパティが <xref:System.Windows.DependencyProperty.Register%2A>の単純なシグネチャに登録されている場合に設定される既定値があります。 <xref:System.Windows.DependencyProperty.Register%2A> の他の署名では、プロパティを登録するときに必要なメタデータを指定できます。 依存関係プロパティに指定される最も一般的なメタデータは、プロパティを使用する新しいインスタンスに適用される既定値を与えるためのものです。

<xref:System.Windows.FrameworkElement>の派生クラスに存在する依存関係プロパティを作成する場合は、基本 <xref:System.Windows.PropertyMetadata> クラスではなく、より特殊化されたメタデータクラス <xref:System.Windows.FrameworkPropertyMetadata> を使用できます。 <xref:System.Windows.FrameworkPropertyMetadata> クラスのコンストラクターには、さまざまなメタデータ特性を組み合わせて指定できるいくつかのシグネチャがあります。 既定値のみを指定する場合は、<xref:System.Object>型の1つのパラメーターを受け取る署名を使用します。 そのオブジェクトパラメーターは、プロパティの型固有の既定値として渡します (提供される既定値は、<xref:System.Windows.DependencyProperty.Register%2A> 呼び出しで `propertyType` パラメーターとして指定した型である必要があります)。

<xref:System.Windows.FrameworkPropertyMetadata>には、プロパティのメタデータオプションフラグを指定することもできます。 これらのフラグは、登録後にプロパティ メタデータで個々のプロパティに変換され、特定の条件をレイアウト エンジンなどの他のプロセスに伝えるために使用されます。

#### <a name="setting-appropriate-metadata-flags"></a>適切なメタデータ フラグの設定

- プロパティ (またはその値の変更) が [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)]に影響を与える場合、特に、レイアウトシステムがページ内の要素のサイズを設定または表示する方法に影響を与える場合は、次のフラグを1つ以上設定します: <xref:System.Windows.FrameworkPropertyMetadataOptions.AffectsMeasure>、<xref:System.Windows.FrameworkPropertyMetadataOptions.AffectsArrange>、<xref:System.Windows.FrameworkPropertyMetadataOptions.AffectsRender>。

  - <xref:System.Windows.FrameworkPropertyMetadataOptions.AffectsMeasure> は、このプロパティに対する変更によって、親オブジェクトが親内で必要になる可能性のある領域が含まれている場合に、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] レンダリングを変更する必要があることを示します。 たとえば、"Width" プロパティには、このフラグが設定されている必要があります。

  - <xref:System.Windows.FrameworkPropertyMetadataOptions.AffectsArrange> は、このプロパティへの変更によって、通常は専用の領域を変更する必要がないが、スペース内の位置が変更されたことを示す [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] のレンダリングを変更する必要があることを示します。 たとえば、"Alignment" プロパティには、このフラグが設定されている必要があります。

  - <xref:System.Windows.FrameworkPropertyMetadataOptions.AffectsRender> は、レイアウトや測定に影響を与えずに、別のレンダリングが必要な他の変更が発生したことを示します。 "Background" など、既存の要素の色を変更するプロパティはその一例です。

  - これらのフラグは、プロパティ システムやレイアウトのコールバックの独自のオーバーライド実装のためのメタデータのプロトコルとしてよく使用されます。 たとえば、インスタンスのいずれかのプロパティが値の変更を報告し、そのメタデータで `true` として <xref:System.Windows.FrameworkPropertyMetadata.AffectsArrange%2A> た場合、<xref:System.Windows.UIElement.InvalidateArrange%2A> を呼び出す <xref:System.Windows.DependencyObject.OnPropertyChanged%2A> コールバックがあるとします。

- 上記で説明した必要なサイズを変更する方法に加え、一部のプロパティは含まれる親要素のレンダリング特性に影響する場合があります。 例として、フロードキュメントモデルで使用される <xref:System.Windows.Documents.Paragraph.MinOrphanLines%2A> プロパティがあります。このプロパティを変更すると、その段落を含むフロードキュメントの全体的なレンダリングを変更できます。 <xref:System.Windows.FrameworkPropertyMetadataOptions.AffectsParentArrange> または <xref:System.Windows.FrameworkPropertyMetadataOptions.AffectsParentMeasure> を使用して、独自のプロパティで類似したケースを特定します。

- 既定では、依存関係プロパティはデータ バインディングをサポートします。 データ バインディングにとって現実的なシナリオがない場合や、大きなオブジェクトのデータ バインディングのパフォーマンスが問題として認識される場合には、意図的にデータ バインディングを無効にすることができます。

- 既定では、依存関係プロパティのデータバインディング <xref:System.Windows.Data.Binding.Mode%2A> は既定で <xref:System.Windows.Data.BindingMode.OneWay>に設定されています。 バインドは、バインドインスタンスごとに <xref:System.Windows.Data.BindingMode.TwoWay> するようにいつでも変更できます。詳細については、「[バインディングの方向を指定する](../data/how-to-specify-the-direction-of-the-binding.md)」を参照してください。 ただし、依存関係プロパティの作成者は、プロパティが既定で <xref:System.Windows.Data.BindingMode.TwoWay> バインドモードを使用するように選択できます。 既存の依存関係プロパティの例としては、<xref:System.Windows.Controls.MenuItem.IsSubmenuOpen%2A?displayProperty=nameWithType>があります。このプロパティのシナリオでは、<xref:System.Windows.Controls.MenuItem.IsSubmenuOpen%2A> 設定ロジックと <xref:System.Windows.Controls.MenuItem> の合成が既定のテーマスタイルと対話します。 <xref:System.Windows.Controls.MenuItem.IsSubmenuOpen%2A> プロパティロジックは、データバインディングをネイティブに使用して、他の状態プロパティやメソッド呼び出しに従ってプロパティの状態を維持します。 既定で <xref:System.Windows.Data.BindingMode.TwoWay> をバインドするもう1つのプロパティの例は <xref:System.Windows.Controls.TextBox.Text%2A?displayProperty=nameWithType>です。

- <xref:System.Windows.FrameworkPropertyMetadataOptions.Inherits> フラグを設定することによって、カスタム依存関係プロパティでプロパティの継承を有効にすることもできます。 プロパティの継承は、親要素と子要素に共通のプロパティがあるシナリオに便利で、子要素に、親に設定されたのと同じ値に設定した特定のプロパティ値を持たせることは理にかなっています。 継承可能なプロパティの例として <xref:System.Windows.FrameworkElement.DataContext%2A>があります。これは、データ表示の重要なマスター詳細シナリオを有効にするためにバインド操作に使用されます。 <xref:System.Windows.FrameworkElement.DataContext%2A> 継承可能にすることで、すべての子要素もそのデータコンテキストを継承します。 プロパティ値の継承により、ページまたはアプリケーションのルートでデータ コンテキストを指定できます。すべての使用可能な子要素内のバインディングに再度指定する必要はありません。 また <xref:System.Windows.FrameworkElement.DataContext%2A> は、継承によって既定値がオーバーライドされることを示す良い例でもありますが、常に特定の子要素でローカルに設定することができます。詳細については、「[階層データでマスター詳細パターンを使用する](../data/how-to-use-the-master-detail-pattern-with-hierarchical-data.md)」を参照してください。 プロパティ値の継承にはパフォーマンスが低下する可能性があるため、控え目に使用する必要があります。詳細については、「[プロパティ値の継承](property-value-inheritance.md)」を参照してください。

- <xref:System.Windows.FrameworkPropertyMetadataOptions.Journal> フラグを設定して、ナビゲーションジャーナリングサービスで依存関係プロパティを検出または使用する必要があるかどうかを示します。 例として、<xref:System.Windows.Controls.Primitives.Selector.SelectedIndex%2A> プロパティがあります。選択コントロールで選択された項目は、履歴履歴がナビゲートされるときに保持される必要があります。

<a name="RODP"></a>

## <a name="read-only-dependency-properties"></a>読み取り専用の依存関係プロパティ

読み取り専用の依存関係プロパティを定義することができます。 ただし、読み取り専用としてプロパティを定義する理由のシナリオには、プロパティ システムに登録して、識別子を公開するための手順が異なるため、若干の違いがあります。 詳細については、「[読み取り専用の依存関係プロパティ](read-only-dependency-properties.md)」を参照してください。

<a name="CTDP"></a>

## <a name="collection-type-dependency-properties"></a>コレクション型依存関係プロパティ

コレクション型依存関係プロパティには、考慮すべき実装問題が他にもいくつかあります。 詳細については、「[コレクション型依存関係プロパティ](collection-type-dependency-properties.md)」を参照してください。

<a name="SecurityC"></a>

## <a name="dependency-property-security-considerations"></a>依存関係プロパティのセキュリティに関する考慮事項

依存関係プロパティは、パブリック プロパティとして宣言する必要があります。 依存関係プロパティ識別子フィールドは、パブリック静的フィールドとして宣言する必要があります。 他のアクセスレベル (protected など) を宣言しようとしても、依存関係プロパティは、プロパティシステム Api と組み合わせて、識別子を使用していつでもアクセスできます。 メタデータレポートや、プロパティシステムの一部である値の決定 Api (<xref:System.Windows.LocalValueEnumerator>など) が原因で、保護された識別子フィールドにアクセスできる可能性もあります。 詳細については、「[依存関係プロパティのセキュリティ](dependency-property-security.md)」を参照してください。

<a name="DPCtor"></a>

## <a name="dependency-properties-and-class-constructors"></a>依存関係プロパティとクラス コンストラクター

マネージド コード プログラミングでは、クラス コンストラクターが仮想メソッドを呼び出さないという一般的な方針があります (多くの場合は FxCop などのコード分析ツールによって適用されます)。 これは、派生クラスのコンストラクターの基本の初期化としてコンストラクターを呼び出すことができ、コンストラクターから仮想メソッドを入力することで、構築されるオブジェクトのインスタンスの初期化が不完全な状態で行われる可能性があるためです。 <xref:System.Windows.DependencyObject>から派生したクラスから派生する場合は、プロパティシステム自体がを呼び出し、仮想メソッドを内部的に公開していることに注意してください。 これらの仮想メソッドは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プロパティ システム サービスの一部です。 メソッドをオーバーライドすることで、派生クラスが値の決定に参加できるようになります。 ランタイムの初期化の潜在的な問題を回避するには、非常に特殊なコンストラクター パターンに従っている場合を除き、依存関係プロパティの値をクラスのコンストラクター内で設定しないでください。 詳細については、「[DependencyObject の安全なコンストラクター パターン](safe-constructor-patterns-for-dependencyobjects.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [依存関係プロパティの概要](dependency-properties-overview.md)
- [依存関係プロパティのメタデータ](dependency-property-metadata.md)
- [コントロールの作成の概要](../controls/control-authoring-overview.md)
- [コレクション型依存関係プロパティ](collection-type-dependency-properties.md)
- [依存関係プロパティのセキュリティ](dependency-property-security.md)
- [XAML 読み込みと依存関係プロパティ](xaml-loading-and-dependency-properties.md)
- [DependencyObject の安全なコンストラクター パターン](safe-constructor-patterns-for-dependencyobjects.md)
