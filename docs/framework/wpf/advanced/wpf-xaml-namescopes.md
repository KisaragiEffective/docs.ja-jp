---
title: XAML 名前スコープ
ms.date: 03/30/2017
helpviewer_keywords:
- namescopes [WPF]
- styles [WPF], namescopes in
- templates [WPF], namescopes in
- APIs [WPF], name-related
- name-related APIs
- XAML [WPF], namescopes
- classes [WPF], FrameworkContentElement
ms.assetid: 52bbf4f2-15fc-40d4-837b-bb4c21ead7d4
ms.openlocfilehash: 4383492157191f61cf04a2fdd6ce27e9183bda8b
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744421"
---
# <a name="wpf-xaml-namescopes"></a>WPF XAML 名前スコープ
XAML 名前スコープは、XAML で定義されているオブジェクトを識別する概念です。 XAML 名前スコープ内の名前を使用すると、オブジェクトの XAML 定義の名前と、オブジェクトツリー内のそのインスタンスに対応するオブジェクトの間のリレーションシップを確立できます。 通常、xaml アプリケーションの個々の XAML ページルートを読み込むときに、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] マネージコード内の XAML 名前スコープが作成されます。 プログラミングオブジェクトとしての XAML 名前スコープは、<xref:System.Windows.Markup.INameScope> インターフェイスによって定義され、実際のクラス <xref:System.Windows.NameScope>によっても実装されます。  

<a name="Namescopes_in_Loaded_XAML_Applications"></a>   
## <a name="namescopes-in-loaded-xaml-applications"></a>読み込まれた XAML アプリケーションでの名前スコープ  
 より広範なプログラミングまたはコンピューターサイエンスのコンテキストでは、プログラミングの概念には、オブジェクトへのアクセスに使用できる一意の識別子または名前の原則が含まれることがよくあります。 識別子または名前を使用するシステムでは、名前スコープによって、その名前のオブジェクトが要求された場合にプロセスまたは技法が検索する境界、または識別名の一意性が強制される境界が定義されます。 これらの一般的な原則は、XAML 名前スコープに当てはまります。 WPF では、XAML 名前スコープは、ページが読み込まれるときに XAML ページのルート要素に作成されます。 ページルートから開始する XAML ページ内で指定された各名前は、関連する XAML 名前スコープに追加されます。  
  
 WPF XAML では、一般的なルート要素 (<xref:System.Windows.Controls.Page>、<xref:System.Windows.Window>など) の要素は常に XAML 名前スコープを制御します。 <xref:System.Windows.FrameworkElement> や <xref:System.Windows.FrameworkContentElement> などの要素がマークアップ内のページのルート要素である場合、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサは、<xref:System.Windows.Controls.Page> が動作する XAML 名前スコープを提供できるように、<xref:System.Windows.Controls.Page> ルートを暗黙的に追加します。  
  
> [!NOTE]
> WPF のビルドアクションでは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] マークアップ内のどの要素にも `Name` または `x:Name` 属性が定義されていない場合でも、XAML 実稼働の XAML 名前スコープが作成されます。  
  
 任意の XAML 名前スコープで同じ名前を2回使用しようとすると、例外が発生します。 分離コードがあり、コンパイルされたアプリケーションの一部である WPF XAML の場合、初期マークアップコンパイル中にページ用に生成されたクラスを作成するときに、WPF ビルドアクションによってビルド時に例外が発生します。 ビルドアクションによってマークアップコンパイルされていない XAML の場合、xaml が読み込まれると、XAML 名前スコープの問題に関連する例外が発生する可能性があります。 Xaml デザイナーは、デザイン時に XAML 名前スコープの問題を予測する場合もあります。  
  
### <a name="adding-objects-to-runtime-object-trees"></a>ランタイムオブジェクトツリーへのオブジェクトの追加  
 XAML が解析される瞬間は、WPF XAML 名前スコープが作成および定義された時点を表します。 オブジェクトを、そのツリーを生成した XAML を解析した後の特定の時点でオブジェクトツリーに追加した場合、新しいオブジェクトの `Name` または `x:Name` の値によって、XAML 名前スコープの情報が自動的に更新されることはありません。 XAML が読み込まれた後にオブジェクトの名前を WPF XAML 名前スコープに追加するには、xaml 名前スコープを定義するオブジェクトで <xref:System.Windows.Markup.INameScope.RegisterName%2A> の適切な実装を呼び出す必要があります。これは通常、XAML ページルートです。 名前が登録されていない場合は、追加されたオブジェクトを <xref:System.Windows.FrameworkElement.FindName%2A>などのメソッドを使用して名前で参照することはできません。また、この名前をアニメーションの対象として使用することはできません。  
  
 アプリケーション開発者にとって最も一般的なシナリオは、ページの現在のルートの XAML 名前スコープに名前を登録するために <xref:System.Windows.FrameworkElement.RegisterName%2A> を使用することです。 <xref:System.Windows.FrameworkElement.RegisterName%2A> は、アニメーション用のオブジェクトを対象とするストーリーボードの重要なシナリオの一部です。 詳細については、「[ストーリーボードの概要](../graphics-multimedia/storyboards-overview.md)」を参照してください。  
  
 XAML 名前スコープを定義するオブジェクト以外のオブジェクトで <xref:System.Windows.FrameworkElement.RegisterName%2A> を呼び出すと、その名前は、呼び出し元のオブジェクトが保持されている XAML 名前スコープにまだ登録されます。これは、オブジェクトを定義する XAML 名前スコープで <xref:System.Windows.FrameworkElement.RegisterName%2A> を呼び出した場合と同様です。  
  
### <a name="xaml-namescopes-in-code"></a>コード内の XAML 名前スコープ  
 XAML 名前スコープを作成し、コードで使用することができます。 Xaml 名前スコープの作成に関係する Api と概念は、純粋なコードの使用でも同じです。これは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の XAML プロセッサが XAML 自体を処理するときにこれらの Api と概念を使用するためです。 概念と API は主に、XAML で部分的または完全に定義されているオブジェクトツリー内の名前でオブジェクトを検索できるようにするために存在します。  
  
 読み込まれた XAML からではなく、プログラムによって作成されるアプリケーションでは、XAML 名前スコープを定義するオブジェクトは <xref:System.Windows.Markup.INameScope>を実装するか、<xref:System.Windows.FrameworkElement> または <xref:System.Windows.FrameworkContentElement> 派生クラスである必要があります。これは、インスタンスで XAML 名前スコープの作成をサポートするためです。  
  
 また、XAML プロセッサによって読み込まれて処理されない要素については、オブジェクトの XAML 名前スコープが既定で作成または初期化されることはありません。 後で名前を登録するオブジェクトに対して、新しい XAML 名前スコープを明示的に作成する必要があります。 XAML 名前スコープを作成するには、静的な <xref:System.Windows.NameScope.SetNameScope%2A> メソッドを呼び出します。 `dependencyObject` パラメーターとしてそれを所有するオブジェクトと、`value` パラメーターとして新しい <xref:System.Windows.NameScope.%23ctor%2A> コンストラクター呼び出しを指定します。  
  
 <xref:System.Windows.NameScope.SetNameScope%2A> の `dependencyObject` として指定されたオブジェクトが <xref:System.Windows.Markup.INameScope> 実装ではない場合、<xref:System.Windows.FrameworkElement> または <xref:System.Windows.FrameworkContentElement>、任意の子要素の <xref:System.Windows.FrameworkElement.RegisterName%2A> を呼び出すことはできません。 新しい XAML 名前スコープを明示的に作成できない場合は、<xref:System.Windows.FrameworkElement.RegisterName%2A> を呼び出すと例外が発生します。  
  
 コードで XAML 名前スコープ Api を使用する例については、「[名前スコープの定義](../graphics-multimedia/how-to-define-a-name-scope.md)」を参照してください。  
  
<a name="Namescopes_in_Styles_and_Templates"></a>   
## <a name="xaml-namescopes-in-styles-and-templates"></a>スタイルとテンプレートでの XAML 名前スコープ  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のスタイルとテンプレートを使用すると、簡単な方法でコンテンツを再利用したり、再適用したりすることができます。 ただし、スタイルやテンプレートには、テンプレートレベルで定義された XAML 名を持つ要素が含まれる場合もあります。 同じテンプレートを1ページで複数回使用することもできます。 このため、スタイルとテンプレートは、スタイルまたはテンプレートが適用されているオブジェクトツリー内の任意の場所に関係なく、独自の XAML 名前スコープを定義します。  
  
 次の例を確認してください。  
  
 [!code-xaml[XamlOvwSupport#NameScopeTemplates](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page6.xaml#namescopetemplates)]  
  
 ここでは、同じテンプレートが2つの異なるボタンに適用されます。 テンプレートに個別の XAML 名前スコープがない場合は、テンプレートで使用される `TheBorder` 名によって、XAML 名前スコープで名前の競合が発生します。 テンプレートの各インスタンス化には独自の XAML 名前スコープがあるため、この例では、インスタンス化されたテンプレートの XAML 名前スコープには名前が1つだけ含まれます。  
  
 また、スタイルは独自の XAML 名前スコープも定義します。これは、ストーリーボードの一部に特定の名前を割り当てることができるようにするためです。 これらの名前を使用すると、コントロールのカスタマイズの一部としてテンプレートを再定義した場合でも、その名前の要素を対象とするコントロール固有の動作が有効になります。  
  
 個別の XAML 名前スコープがあるため、テンプレート内の名前付き要素を検索する方が、ページ内でテンプレート化されていない名前付き要素を見つけるよりも困難です。 まず、適用するテンプレートを決定する必要があります。そのためには、テンプレートが適用されているコントロールの <xref:System.Windows.Controls.Control.Template%2A> プロパティ値を取得します。 次に、テンプレートバージョンの <xref:System.Windows.FrameworkTemplate.FindName%2A>を呼び出し、テンプレートが適用されたコントロールを2番目のパラメーターとして渡します。  
  
 コントロールの作成者が、適用されるテンプレートの特定の名前付き要素が、コントロール自体で定義されている動作のターゲットである場合に、コントロールの実装コードから <xref:System.Windows.FrameworkElement.GetTemplateChild%2A> メソッドを使用できます。 <xref:System.Windows.FrameworkElement.GetTemplateChild%2A> メソッドは保護されているので、コントロールの作成者だけがアクセスできます。  
  
 テンプレート内から作業していて、テンプレートが適用されている XAML 名前スコープにアクセスする必要がある場合は、<xref:System.Windows.FrameworkElement.TemplatedParent%2A>の値を取得し、そこで <xref:System.Windows.FrameworkElement.FindName%2A> を呼び出します。 テンプレート内での作業の例として、適用されるテンプレートの要素からイベントが発生するイベントハンドラーの実装を記述する場合があります。  
  
<a name="Namescopes_and_Name_related_APIs"></a>   
## <a name="xaml-namescopes-and-name-related-apis"></a>XAML 名前スコープと名前関連 Api  
 <xref:System.Windows.FrameworkElement> には、<xref:System.Windows.FrameworkElement.FindName%2A>、<xref:System.Windows.FrameworkElement.RegisterName%2A>、および <xref:System.Windows.FrameworkElement.UnregisterName%2A> のメソッドがあります。 これらのメソッドを呼び出すオブジェクトが XAML 名前スコープを所有している場合、メソッドは、関連する XAML 名前スコープのメソッドを呼び出します。 それ以外の場合は、親要素が XAML 名前スコープを所有しているかどうかがチェックされ、xaml 名前スコープが見つかるまで、このプロセスが再帰的に繰り返されます (XAML プロセッサの動作により、ルートに XAML 名前スコープがあることが保証されます)。 <xref:System.Windows.FrameworkContentElement> の動作は似ていますが、XAML 名前スコープを所有する <xref:System.Windows.FrameworkContentElement> がないという例外があります。 メソッドは <xref:System.Windows.FrameworkContentElement> に存在するため、呼び出しを最終的に <xref:System.Windows.FrameworkElement> の親要素に転送できます。  
  
 <xref:System.Windows.NameScope.SetNameScope%2A> は、新しい XAML 名前スコープを既存のオブジェクトにマップするために使用されます。 XAML 名前スコープをリセットまたはクリアするために <xref:System.Windows.NameScope.SetNameScope%2A> を複数回呼び出すことができますが、これは一般的な使用方法ではありません。 また、<xref:System.Windows.NameScope.GetNameScope%2A> は通常、コードからは使用されません。  
  
### <a name="xaml-namescope-implementations"></a>XAML 名前スコープの実装  
 次のクラスは <xref:System.Windows.Markup.INameScope> を直接実装します。  
  
- <xref:System.Windows.NameScope>  
  
- <xref:System.Windows.Style>  
  
- <xref:System.Windows.ResourceDictionary>  
  
- <xref:System.Windows.FrameworkTemplate>  
  
 <xref:System.Windows.ResourceDictionary> は、XAML 名または名前スコープを使用しません。キーは、ディクショナリの実装であるため、代わりにキーを使用します。 <xref:System.Windows.ResourceDictionary> が <xref:System.Windows.Markup.INameScope> を実装する唯一の理由は、実際の XAML 名前スコープと <xref:System.Windows.ResourceDictionary> がキーを処理する方法と、XAML 名前スコープが親要素によって <xref:System.Windows.ResourceDictionary> に適用されないようにするために、ユーザーコードの例外を発生させることができるためです。  
  
 <xref:System.Windows.FrameworkTemplate> と <xref:System.Windows.Style> は、明示的なインターフェイス定義によって <xref:System.Windows.Markup.INameScope> を実装します。 明示的な実装では、これらの XAML 名前スコープが <xref:System.Windows.Markup.INameScope> インターフェイスを介してアクセスされたときに従来の動作を行うことができます。これは、XAML 名前スコープが [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 内部プロセスによって伝達される方法です。 ただし、明示的なインターフェイス定義は、<xref:System.Windows.FrameworkTemplate> および <xref:System.Windows.Style>の従来の API サーフェイスの一部ではありません。 <xref:System.Windows.FrameworkTemplate> で <xref:System.Windows.Markup.INameScope> メソッドを呼び出し、直接 <xref:System.Windows.Style> する必要はほとんどなく、代わりに <xref:System.Windows.FrameworkElement.GetTemplateChild%2A>などの他の API を使用する必要があるためです。  
  
 次のクラスは、<xref:System.Windows.NameScope?displayProperty=nameWithType> ヘルパークラスを使用し、<xref:System.Windows.NameScope.NameScope%2A?displayProperty=nameWithType> 添付プロパティを使用してその XAML 名前スコープの実装に接続することによって、独自の XAML 名前スコープを定義します。  
  
- <xref:System.Windows.FrameworkElement>  
  
- <xref:System.Windows.FrameworkContentElement>  
  
## <a name="see-also"></a>参照

- [XAML 名前空間および WPF XAML の名前空間の割り当て](xaml-namespaces-and-namespace-mapping-for-wpf-xaml.md)
- [x:Name ディレクティブ](../../../desktop-wpf/xaml-services/xname-directive.md)
