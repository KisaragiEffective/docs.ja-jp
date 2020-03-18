---
title: XAML とカスタムクラス
ms.date: 03/30/2017
helpviewer_keywords:
- custom classes in XAML [WPF]
- XAML [WPF], custom classes
- classes [WPF], custom classes in XAML
ms.assetid: e7313137-581e-4a64-8453-d44e15a6164a
ms.openlocfilehash: 8dab7310826357d7fbe434002298032b8722e5b5
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744427"
---
# <a name="xaml-and-custom-classes-for-wpf"></a>WPF における XAML とカスタム クラス
共通言語ランタイム (CLR) フレームワークで実装されている XAML は、任意の共通言語ランタイム (CLR) 言語でカスタムクラスまたは構造体を定義し、そのクラスに XAML マークアップを使用してアクセスする機能をサポートしています。 同じマークアップファイル内で [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]定義型とカスタム型の組み合わせを使用できます。通常は、カスタム型を XAML 名前空間プレフィックスにマップします。 このトピックでは、カスタムクラスが XAML 要素として使用できるようにするために満たす必要がある要件について説明します。  

<a name="Custom_Classes_in_Applications_vs__in_Assemblies"></a>   
## <a name="custom-classes-in-applications-or-assemblies"></a>アプリケーションまたはアセンブリのカスタムクラス  
 XAML で使用されるカスタムクラスは、分離コード内、またはプライマリ [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] アプリケーションを生成する他のコード内、またはクラスライブラリとして使用される実行可能ファイルや DLL などの別のアセンブリのクラスとして定義できます。 これらの各方法には、特定の利点と欠点があります。  
  
- クラスライブラリを作成する利点は、このようなカスタムクラスをさまざまなアプリケーション間で共有できることです。 また、別のライブラリを使用すると、アプリケーションのバージョン管理の問題を簡単に制御でき、意図したクラスの使用が XAML ページのルート要素として使用されるクラスの作成が簡単になります。  
  
- アプリケーションでカスタムクラスを定義する利点は、この手法が比較的軽量であり、メインアプリケーションの実行可能ファイルを超える個別のアセンブリを導入した場合に発生する配置およびテストの問題を最小限に抑えることです。  
  
- 同じまたは別のアセンブリで定義されているかどうかにかかわらず、XAML で要素として使用するためには、カスタムクラスを CLR 名前空間と XML 名前空間の間でマップする必要があります。 「 [WPF xaml の Xaml 名前空間と名前空間のマッピング」を](xaml-namespaces-and-namespace-mapping-for-wpf-xaml.md)参照してください。  
  
<a name="Requirements_for_a_Custom_Class_as_a_XAML_Element"></a>   
## <a name="requirements-for-a-custom-class-as-a-xaml-element"></a>XAML 要素としてのカスタムクラスの要件  
 オブジェクト要素としてインスタンス化できるようにするには、クラスが次の要件を満たしている必要があります。  
  
- カスタムクラスはパブリックであり、既定の (パラメーターなしの) パブリックコンストラクターをサポートしている必要があります。 (構造に関する注意事項については、次のセクションを参照してください)。  
  
- カスタムクラスを入れ子にしたクラスにすることはできません。 入れ子になったクラスと CLR の一般的な使用構文の "ドット" は、添付プロパティなど、他の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] や XAML 機能に干渉します。  
  
 オブジェクトの要素の構文を有効にするだけでなく、オブジェクトの定義では、そのオブジェクトを値の型として使用する他のパブリックプロパティのプロパティ要素構文を有効にすることもできます。 これは、オブジェクトをオブジェクト要素としてインスタンス化できるようになり、そのようなプロパティのプロパティ要素の値を埋めることができるためです。  
  
### <a name="structures"></a>構造体  
 カスタム型として定義した構造体は、常に [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の XAML で構築できます。これは、すべてのプロパティ値を既定値に初期化する構造体のパラメーターなしのコンストラクターが CLR コンパイラによって暗黙的に作成されるためです。 場合によっては、構造体の既定の構築動作やオブジェクト要素の使用が望ましくないことがあります。 これは、構造体が値を結合することを意図しており、概念的には共用体として機能します。この場合、含まれる値は相互に排他的な解釈を持つ可能性があるため、そのプロパティは設定できません。 このような構造の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の例は <xref:System.Windows.GridLength>です。 通常、このような構造体では、構造体の値のさまざまな解釈やモードを作成する文字列表記規則を使用して、値を属性形式で表現できるように、型コンバーターを実装する必要があります。 また、構造体は、パラメーターなしのコンストラクターを使用してコードを構築する場合にも同様の動作を公開する必要があります。  
  
<a name="Requirements_for_Properties_of_a_Custom_Class_as_XAML"></a>   
## <a name="requirements-for-properties-of-a-custom-class-as-xaml-attributes"></a>XAML 属性としてのカスタムクラスのプロパティの要件  
 プロパティは、値渡しの型 (プリミティブなど) を参照するか、XAML プロセッサがアクセスできるパラメーターなしのコンストラクターまたは専用の型コンバーターを持つ型のクラスを使用する必要があります。 CLR XAML 実装では、XAML プロセッサは、言語プリミティブのネイティブサポートを通じて、またはバッキング型定義内の型またはメンバーへの <xref:System.ComponentModel.TypeConverterAttribute> の適用によって、このようなコンバーターを検出します。  
  
 また、プロパティは抽象クラス型またはインターフェイスを参照できます。 抽象クラスまたはインターフェイスの場合、XAML の解析に期待されるのは、インターフェイスを実装する実際のクラスインスタンス、または抽象クラスから派生した型のインスタンスをプロパティ値に入力する必要があることです。  
  
 プロパティは抽象クラスで宣言できますが、抽象クラスから派生した実用的なクラスでのみ設定できます。 これは、クラスの object 要素を作成するときに、クラスのパラメーターなしのパブリックコンストラクターが必要になるためです。  
  
### <a name="typeconverter-enabled-attribute-syntax"></a>TypeConverter Enabled 属性の構文  
 クラスレベルで専用の属性付きの型コンバーターを指定した場合、適用される型変換によって、その型をインスタンス化する必要がある任意のプロパティの属性構文が有効になります。 型コンバーターは、型のオブジェクト要素の使用を有効にしません。オブジェクト要素の使用を有効にできるのは、その型にパラメーターなしのコンストラクターが存在する場合だけです。 そのため、型コンバーターが有効になっているプロパティは、通常、プロパティ構文では使用できません。ただし、型自体がオブジェクト要素構文もサポートしている場合を除きます。 例外として、プロパティ要素構文を指定できますが、property 要素には文字列が含まれています。 この使用法は、実質的に属性の構文の使用に相当します。このような使用方法は、属性値のより堅牢な空白処理が必要な場合を除き、一般的ではありません。 たとえば、次の例は、文字列を受け取るプロパティ要素の使用法と、同等の属性の使用法を示しています。  
  
 [!code-xaml[XamlOvwSupport#GoofyTCPE](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page8.xaml#goofytcpe)]  
  
 [!code-xaml[XamlOvwSupport#GoofyTCPE2](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page8.xaml#goofytcpe2)]  
  
 属性構文が許可されていても、オブジェクト要素を含む property 要素構文が XAML で許可されていないプロパティの例は、<xref:System.Windows.Input.Cursor> 型を受け取るさまざまなプロパティです。 <xref:System.Windows.Input.Cursor> クラスには、専用の型コンバーター <xref:System.Windows.Input.CursorConverter>がありますが、パラメーターなしのコンストラクターは公開されないため、<xref:System.Windows.FrameworkElement.Cursor%2A> プロパティは、実際の <xref:System.Windows.Input.Cursor> 型が参照型である場合でも属性構文を使用してのみ設定できます。  
  
### <a name="per-property-type-converters"></a>プロパティごとの型コンバーター  
 また、プロパティ自体は、プロパティレベルで型コンバーターを宣言できます。 これにより、適切な型に基づいて、属性の入力文字列値を <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> 操作の入力として処理することにより、プロパティの型のオブジェクトをインスタンス化する "ミニ言語" が有効になります。 これは通常、XAML でプロパティの設定を有効にする唯一の手段としてではなく、便宜的なアクセサーを提供するために行われます。 ただし、パラメーターなしのコンストラクターまたは属性付きの型コンバーターを提供しない既存の CLR 型を使用する属性には、型コンバーターを使用することもできます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] API の例は、<xref:System.Globalization.CultureInfo> 型を受け取る特定のプロパティです。 この場合、以前のバージョンのフレームワークで使用されていた互換性と移行のシナリオをより適切に解決するために、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 既存の Microsoft .NET フレームワーク <xref:System.Globalization.CultureInfo> 型が使用されていましたが、<xref:System.Globalization.CultureInfo> の種類では、必要なコンストラクターまたは型レベルの変換を XAML プロパティ値として直接使用することはできませんでした。  
  
 XAML を使用するプロパティを公開するときは常に、特にコントロールの作成者である場合は、依存関係プロパティを使用してそのプロパティをバックアップすることを強くお勧めします。 これは、<xref:System.Windows.DependencyProperty> のバックアップを使用してパフォーマンスを向上させることができるため、XAML プロセッサの既存の [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] 実装を使用する場合に特に当てはまります。 依存関係プロパティは、ユーザーが XAML アクセス可能なプロパティに対して期待するプロパティのプロパティシステム機能を公開します。 これには、アニメーション、データバインディング、スタイルのサポートなどの機能が含まれます。 詳細については、「[カスタム依存関係プロパティ](custom-dependency-properties.md)」および「 [XAML 読み込みと依存関係プロパティ](xaml-loading-and-dependency-properties.md)」を参照してください。  
  
### <a name="writing-and-attributing-a-type-converter"></a>型コンバーターの記述と属性  
 場合によっては、プロパティ型の型変換を提供するために、カスタム <xref:System.ComponentModel.TypeConverter> 派生クラスを記述する必要があります。 から派生し、XAML の使用をサポートできる型コンバーターを作成する手順、および <xref:System.ComponentModel.TypeConverterAttribute>を適用する方法については、「 [TypeConverters AND XAML](typeconverters-and-xaml.md)」を参照してください。  
  
<a name="Requirements_for_Events_of_a_Custom_Class_as_XAML"></a>   
## <a name="requirements-for-xaml-event-handler-attribute-syntax-on-events-of-a-custom-class"></a>カスタムクラスのイベントに対する XAML イベントハンドラー属性構文の要件  
 CLR イベントとして使用できるようにするには、イベントを、パラメーターなしのコンストラクターをサポートするクラスのパブリックイベントとして、または派生クラスでイベントにアクセスできる抽象クラスで公開する必要があります。 簡単にルーティングイベントとして使用するためには、CLR イベントに明示的な `add` および `remove` メソッドを実装する必要があります。このメソッドは、CLR イベントシグネチャのハンドラーを追加および削除し、これらのハンドラーを <xref:System.Windows.UIElement.AddHandler%2A> および <xref:System.Windows.UIElement.RemoveHandler%2A> メソッドに転送します。 これらのメソッドは、イベントがアタッチされているインスタンス上のルーティングイベントハンドラーストアにハンドラーを追加または削除します。  
  
> [!NOTE]
> <xref:System.Windows.UIElement.AddHandler%2A>を使用してルーティングイベントのハンドラーを直接登録し、ルーティングイベントを公開する CLR イベントを意図的に定義することはできません。 通常、この方法は推奨されません。イベントはハンドラーをアタッチするための XAML 属性構文を有効にせず、生成されたクラスはその型の機能の透明度の低い XAML ビューを提供します。  
  
<a name="Collection_Properties"></a>   
## <a name="writing-collection-properties"></a>コレクションのプロパティの書き込み  
 コレクション型を受け取るプロパティには、コレクションに追加されるオブジェクトを指定できるようにする XAML 構文があります。 この構文には、2つの注目すべき機能があります。  
  
- オブジェクト要素構文では、コレクションオブジェクトであるオブジェクトを指定する必要はありません。 コレクション型を受け取る XAML でプロパティを指定するたびに、コレクション型の存在が暗黙的になります。  
  
- マークアップのコレクションプロパティの子要素は、コレクションのメンバーになるように処理されます。 通常、コレクションのメンバーへのコードアクセスは、`Add`などのリスト/ディクショナリメソッド、またはインデクサーを使用して実行されます。 ただし、XAML 構文ではメソッドまたはインデクサーがサポートされません (例外: XAML 2009 ではメソッドをサポートできますが、XAML 2009 を使用すると WPF の使用を制限できます。「 [xaml 2009 言語機能](../../../desktop-wpf/xaml-services/xaml-2009-language-features.md)」を参照してください)。 コレクションは、要素のツリーを構築するための非常に一般的な要件であり、宣言型の XAML でこれらのコレクションを設定する方法が必要です。 したがって、コレクションプロパティの子要素は、コレクションプロパティの型の値であるコレクションに追加することで処理されます。  
  
 .NET Framework XAML サービスの実装であるため、WPF XAML プロセッサは、コレクションプロパティを構成するために次の定義を使用します。 プロパティのプロパティの型は、次のいずれかを実装する必要があります。  
  
- <xref:System.Collections.IList> を実装します。  
  
- <xref:System.Collections.IDictionary> または同等の汎用 (<xref:System.Collections.Generic.IDictionary%602>) を実装します。  
  
- <xref:System.Array> から派生します (XAML の配列の詳細については、「 [X:Array Markup Extension](../../../desktop-wpf/xaml-services/xarray-markup-extension.md)」を参照してください)。  
  
- <xref:System.Windows.Markup.IAddChild> ([!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]で定義されたインターフェイス) を実装します。  
  
 CLR の各型には `Add` メソッドがあります。これは、オブジェクトグラフの作成時に、XAML プロセッサによって基になるコレクションに項目を追加するために使用されます。  
  
> [!NOTE]
> ジェネリック `List` および `Dictionary` インターフェイス (<xref:System.Collections.Generic.IList%601> および <xref:System.Collections.Generic.IDictionary%602>) は、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] XAML プロセッサによるコレクションの検出ではサポートされていません。 ただし、<xref:System.Collections.IList> を直接実装するか、基本クラスとして <xref:System.Collections.Generic.Dictionary%602> するため、<xref:System.Collections.Generic.List%601> クラスを基底クラスとして使用できます。これは <xref:System.Collections.IDictionary> を直接実装するためです。  
  
 コレクションを受け取るプロパティを宣言するときは、そのプロパティ値が型の新しいインスタンスでどのように初期化されるかに注意してください。 プロパティを依存関係プロパティとして実装していない場合は、コレクション型コンストラクターを呼び出すバッキングフィールドをプロパティに使用するのが適切です。 プロパティが依存関係プロパティの場合は、既定の型コンストラクターの一部としてコレクションプロパティを初期化する必要がある場合があります。 これは、依存関係プロパティの既定値がメタデータから取得されるためです。通常は、コレクションプロパティの初期値を静的な共有コレクションにする必要がありません。 含まれている型のインスタンスごとにコレクションインスタンスが存在している必要があります。 詳細については、「[カスタム依存関係プロパティ](custom-dependency-properties.md)」を参照してください。  
  
 コレクションプロパティのカスタムコレクション型を実装できます。 コレクションプロパティの暗黙的な処理により、カスタムコレクション型は、暗黙的に XAML で使用するために、パラメーターなしのコンストラクターを提供する必要がありません。 ただし、必要に応じて、コレクション型のパラメーターなしのコンストラクターを指定することもできます。 これは、有益な方法です。 パラメーターなしのコンストラクターを指定しない限り、コレクションをオブジェクト要素として明示的に宣言することはできません。 マークアップの作成者は、マークアップスタイルに関係なく、明示的なコレクションを表示することを好む場合があります。 また、パラメーターなしのコンストラクターを使用すると、コレクション型をプロパティ値として使用する新しいオブジェクトを作成するときに初期化要件を簡略化できます。  
  
<a name="XAMLCONtent"></a>   
## <a name="declaring-xaml-content-properties"></a>宣言 (XAML コンテンツプロパティを)  
 XAML 言語では、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] コンテンツプロパティの概念が定義されています。 オブジェクト構文で使用できる各クラスは、XAML コンテンツプロパティを1つだけ持つことができます。 クラスの XAML コンテンツプロパティとなるプロパティを宣言するには、クラス定義の一部として <xref:System.Windows.Markup.ContentPropertyAttribute> を適用します。 目的の XAML コンテンツプロパティの名前を属性の <xref:System.Windows.Markup.ContentPropertyAttribute.Name%2A> として指定します。 プロパティは、<xref:System.Reflection.PropertyInfo>などのリフレクション構成ではなく、名前で文字列として指定されます。  
  
 XAML コンテンツプロパティとなるコレクションプロパティを指定できます。 この結果、そのプロパティが使用されます。これにより、オブジェクト要素には、コレクションオブジェクトの要素またはプロパティ要素タグを介在させることなく、1つまたは複数の子要素を含めることができます。 これらの要素は、XAML コンテンツプロパティの値として処理され、バッキングコレクションインスタンスに追加されます。  
  
 一部の既存の XAML コンテンツプロパティは、`Object`のプロパティの型を使用します。 これにより、<xref:System.String> などのプリミティブ値を取得し、単一の参照オブジェクト値を取得できる XAML コンテンツプロパティが有効になります。 このモデルに従う場合は、型の決定と、可能な型の処理が型によって行われます。 <xref:System.Object> コンテンツタイプの一般的な理由は、オブジェクトコンテンツを文字列として追加する単純な方法 (既定のプレゼンテーション処理を受け取る)、または既定以外のプレゼンテーションや追加データを指定するオブジェクトの内容を追加する高度な方法の両方をサポートすることです。  
  
<a name="Serializing"></a>   
## <a name="serializing-xaml"></a>XAML のシリアル化  
 コントロールの作成者である場合など、特定のシナリオでは、XAML でインスタンス化できるすべてのオブジェクト表現を、同等の XAML マークアップにシリアル化することもできます。 このトピックでは、シリアル化の要件については説明しません。 「[コントロールの作成の概要](../controls/control-authoring-overview.md)」と「[要素ツリーとシリアル化」を](element-tree-and-serialization.md)参照してください。  
  
## <a name="see-also"></a>参照

- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
- [カスタム依存関係プロパティ](custom-dependency-properties.md)
- [コントロールの作成の概要](../controls/control-authoring-overview.md)
- [基本要素の概要](base-elements-overview.md)
- [XAML 読み込みと依存関係プロパティ](xaml-loading-and-dependency-properties.md)
