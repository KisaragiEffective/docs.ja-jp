---
title: XAML 構文の詳細
ms.date: 03/30/2017
helpviewer_keywords:
- XML [WPF], namespaces
- XAML [WPF], parsing of attributes
- parsing of attributes [WPF]
- XAML [WPF], markup extensions
- attached properties [WPF]
- tag syntax [XAML]
- markup extensions [WPF]
- XAML [WPF], object element syntax
- XAML [WPF], syntax terminology
- attached events [WPF]
- lookup semantics [WPF]
- XAML [WPF], attached events
- XAML [WPF], content syntax
- XAML [WPF], lookup semantics
- content syntax [WPF]
- object element syntax [WPF]
- syntax terminology [XAML]
- XAML [WPF], attached properties
- attributes [XAML], parsing
- XAML [WPF], tag syntax
- XAML [WPF], attribute syntax
- property element syntax [WPF]
- terminology [XAML]
- namespaces [WPF], XML
- attribute syntax [XAML]
- XAML [WPF], property element syntax
ms.assetid: 67cce290-ca26-4c41-a797-b68aabc45479
ms.openlocfilehash: 38c77086075e79c0ec5b4b1564ed753eded23b34
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77124495"
---
# <a name="xaml-syntax-in-detail"></a>XAML 構文の詳細
このトピックでは、XAML 構文の要素について説明するために使用される用語を定義します。 これらの用語は、このドキュメントの残りの部分で頻繁に使用されます。 WPF ドキュメントについては、特に、xaml を使用する他のフレームワークの場合と、xaml 言語サポートによってシステム .Xaml レベルで有効になっている基本的な XAML 概念の両方で使用されます。 このトピックでは、「 [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)」で紹介した基本的な用語について説明します。  

<a name="the_xaml_language_specification"></a>   
## <a name="the-xaml-language-specification"></a>XAML 言語仕様  
 ここで定義されている XAML 構文の用語は、XAML 言語仕様内でも定義または参照されています。 XAML は XML に基づく言語で、XML 構造ルールに従って、または拡張されます。 一部の用語は、XML 言語または XML ドキュメントオブジェクトモデルを記述するときに一般的に使用される用語に基づいて、またはによって共有されます。  
  
 XAML 言語仕様の詳細については、Microsoft ダウンロードセンターから[\[の MS xaml\]](https://download.microsoft.com/download/0/A/6/0A6F7755-9AF5-448B-907D-13985ACCF53E/[MS-XAML].pdf)をダウンロードしてください。  
  
<a name="xaml_and_clr"></a>   
## <a name="xaml-and-clr"></a>XAML と CLR  
 XAML は、マークアップ言語です。 共通言語ランタイム (CLR) は、名前によって指定されているように、ランタイム実行を可能にします。 XAML は、CLR ランタイムによって直接消費される共通言語の1つではありません。 代わりに、XAML は独自の型システムをサポートするものと考えることができます。 WPF によって使用される特定の XAML 解析システムは、CLR と CLR 型システム上に構築されます。 XAML 型は、WPF の XAML が解析されるときに実行時の表現をインスタンス化するために、CLR 型にマップされます。 このため、このドキュメントの構文に関する残りの部分には、CLR 型システムへの参照が含まれています。ただし、XAML 言語仕様では、同等の構文については説明しません。 (XAML 言語仕様レベルによっては、XAML 型を他の型システムにマップできます。これは CLR である必要はありませんが、別の XAML パーサーの作成と使用が必要になります)。  
  
#### <a name="members-of-types-and-class-inheritance"></a>型のメンバーとクラスの継承  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 型の XAML メンバーとして表示されるプロパティとイベントは、多くの場合、基本型から継承されます。 たとえば、次の例を考えてみます。 `<Button Background="Blue" .../>`。 クラス定義、リフレクション結果、またはドキュメントを参照する場合、<xref:System.Windows.Controls.Control.Background%2A> プロパティは、<xref:System.Windows.Controls.Button> クラスですぐに宣言されたプロパティではありません。 代わりに、<xref:System.Windows.Controls.Control.Background%2A> は基本 <xref:System.Windows.Controls.Control> クラスから継承されます。  
  
 XAML 要素 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のクラス継承動作は、スキーマが適用される XML マークアップの解釈から大幅に逸脱します。 クラスの継承は複雑になる可能性があります。特に、中間の基底クラスが抽象型の場合、またはインターフェイスが関係している場合に発生します。 これは、XAML 要素とその使用可能な属性のセットが、DTD や XSD 形式などの XML プログラミングで通常使用されるスキーマ型を正確かつ完全に表現することが困難な理由の1つです。 もう1つの理由は、XAML 言語自体の機能拡張および型マッピング機能によって、許容される型とメンバーのすべての固定表現が完全に排除されることです。  
  
<a name="object_element_syntax"></a>   
## <a name="object-element-syntax"></a>オブジェクト要素構文  
 *オブジェクト要素構文*は、XML 要素を宣言することで CLR クラスまたは構造体をインスタンス化する XAML マークアップ構文です。 この構文は、HTML などの他のマークアップ言語の要素構文に似ています。 オブジェクト要素の構文は、左山かっこ (\<) で始まり、その直後に、インスタンス化されるクラスまたは構造体の型名が続きます。 型名の後に0個以上の空白を含めることができます。また、0個以上の属性を object 要素で宣言することもできます。また、各属性名 = "value" ペアを区切る1つ以上のスペースを使用します。 最後に、次のいずれかが true である必要があります。  
  
- 要素とタグは、スラッシュ (/) の後に右山かっこ (>) で閉じる必要があります。  
  
- 開始タグは、右山かっこ (>) で終了する必要があります。 その他のオブジェクト要素、プロパティ要素、または内部テキストは、開始タグに従うことができます。 ここに含まれる内容は、通常、要素のオブジェクトモデルによって制限されます。 Object 要素の終了タグは、他の開始タグと終了タグのペアと適切に入れ子にして、バランスを取る必要もあります。  
  
 .NET によって実装される XAML には、オブジェクト要素を型、プロパティまたはイベントに変換する属性、および XAML 名前空間を CLR 名前空間とアセンブリにマップする一連の規則があります。 WPF と .NET では、XAML オブジェクト要素は参照アセンブリで定義されている .NET 型にマップされ、属性はそれらの型のメンバーにマップされます。 XAML で CLR 型を参照する場合は、その型の継承されたメンバーにもアクセスできます。  
  
 たとえば、次の例は、<xref:System.Windows.Controls.Button> クラスの新しいインスタンスをインスタンス化し、その属性の <xref:System.Windows.FrameworkElement.Name%2A> 属性と値を指定するオブジェクト要素構文です。  
  
 [!code-xaml[XAMLOvwSupport#SyntaxOE](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/Page1.xaml#syntaxoe)]  
  
 次の例は、XAML コンテンツプロパティ構文も含まれるオブジェクト要素構文です。 に含まれる内部テキストは、<xref:System.Windows.Controls.TextBox.Text%2A><xref:System.Windows.Controls.TextBox> XAML コンテンツプロパティを設定するために使用されます。  
  
 [!code-xaml[XAMLOvwSupport#ThisIsATextBox](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/Page1.xaml#thisisatextbox)]  
  
### <a name="content-models"></a>コンテンツモデル  
 クラスは、構文に関して XAML オブジェクト要素としての使用をサポートする場合がありますが、その要素は、コンテンツモデル全体または要素ツリーの予想される位置に配置されている場合にのみ、アプリケーションまたはページで正しく機能します。 たとえば、<xref:System.Windows.Controls.MenuItem> は通常、<xref:System.Windows.Controls.Menu>などの <xref:System.Windows.Controls.Primitives.MenuBase> 派生クラスの子としてのみ配置する必要があります。 特定の要素のコンテンツモデルは、XAML 要素として使用できるコントロールおよびその他の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] クラスのクラスページの解説の一部として記載されています。  
  
<a name="properties_of_object_elements"></a>   
## <a name="properties-of-object-elements"></a>オブジェクト要素のプロパティ  
 XAML のプロパティは、さまざまな構文で設定されます。 特定のプロパティに使用できる構文は、設定するプロパティの基になる型システムの特性によって異なります。  
  
 プロパティの値を設定することによって、実行時オブジェクトグラフに存在するオブジェクトに特徴または特性を追加します。 オブジェクト要素から作成されたオブジェクトの初期状態は、パラメーターなしのコンストラクターの動作に基づいています。 通常、アプリケーションでは、特定のオブジェクトの完全に既定のインスタンス以外のものを使用します。  
  
<a name="attribute_syntax_properties"></a>   
## <a name="attribute-syntax-properties"></a>属性の構文 (プロパティ)  
 属性構文は、既存のオブジェクト要素の属性を宣言することによってプロパティの値を設定する XAML マークアップ構文です。 属性名は、関連するオブジェクト要素をバッキングするクラスのプロパティの CLR メンバー名と一致している必要があります。 属性名の後に代入演算子 (=) が付きます。 属性値は、引用符で囲まれた文字列である必要があります。  
  
> [!NOTE]
> 代替引用符を使用して、属性内にリテラル引用符を配置できます。 たとえば、二重引用符で囲まれた文字列を宣言する手段として単一引用符を使用できます。 単一引用符と二重引用符のどちらを使用する場合でも、属性値の文字列を開いたり閉じたりするには、一致するペアを使用する必要があります。 また、特定の XAML 構文によって課される文字制限を回避するために使用できるエスケープシーケンスやその他の手法もあります。 「 [XML 文字エンティティと XAML」を](../../../desktop-wpf/xaml-services/xml-character-entities.md)参照してください。  
  
 属性構文を使用して設定するには、プロパティはパブリックである必要があり、書き込み可能である必要があります。 バッキング型システムのプロパティの値は、値型であるか、または関連するバッキング型にアクセスするときに XAML プロセッサによってインスタンス化または参照できる参照型である必要があります。  
  
 WPF XAML イベントの場合、属性名として参照されるイベントはパブリックであり、パブリックデリゲートを持つ必要があります。  
  
 プロパティまたはイベントは、含んでいるオブジェクト要素によってインスタンス化されるクラスまたは構造体のメンバーである必要があります。  
  
### <a name="processing-of-attribute-values"></a>属性値の処理  
 開始引用符と終わり引用符に含まれる文字列値は、XAML プロセッサによって処理されます。 プロパティの既定の処理動作は、基になる CLR プロパティの型によって決まります。  
  
 属性値は、この処理順序を使用して、次のいずれかによって入力されます。  
  
1. XAML プロセッサで、<xref:System.Windows.Markup.MarkupExtension>から派生した中かっこ、またはオブジェクト要素が検出された場合、参照されるマークアップ拡張機能は、値を文字列として処理するのではなく、最初に評価され、マークアップ拡張機能によって返されるオブジェクトが値として使用されます。 多くの場合、マークアップ拡張機能によって返されるオブジェクトは、既存のオブジェクトへの参照、または実行時まで評価を延期する式であり、新しくインスタンス化されたオブジェクトではありません。  
  
2. プロパティが属性付きの <xref:System.ComponentModel.TypeConverter>で宣言されている場合、またはプロパティの値の型が属性付きの <xref:System.ComponentModel.TypeConverter>で宣言されている場合、属性の文字列値は変換入力として型コンバーターに送信され、コンバーターは新しいオブジェクトインスタンスを返します。  
  
3. <xref:System.ComponentModel.TypeConverter>がない場合は、プロパティ型への直接変換が試行されます。 この最終的なレベルは、XAML 言語プリミティブ型間のパーサーネイティブ値または列挙型の名前付き定数の名前のチェック (パーサーが一致する値にアクセスする) の直接変換です。  
  
#### <a name="enumeration-attribute-values"></a>列挙属性の値  
 XAML の列挙は、本質的に XAML パーサーによって処理されます。列挙体のメンバーは、列挙体の名前付き定数の1つの文字列名を指定することによって指定する必要があります。  
  
 フラグ以外の列挙値の場合、ネイティブの動作では、属性値の文字列を処理し、列挙値のいずれかに解決します。 列挙体は、format*列挙体*では指定しません。*値*。コードの場合と同じです。 代わりに、*値*のみを指定します。*列挙*は、設定するプロパティの型によって推論されます。 *列挙体*に属性を指定する場合。*値*フォームは正しく解決されません。  
  
 フラグ列挙型の動作は、<xref:System.Enum.Parse%2A?displayProperty=nameWithType> メソッドに基づいています。 各値をコンマで区切ることによって、フラグ列挙に複数の値を指定できます。 ただし、フラグではない列挙値を組み合わせることはできません。 たとえば、コンマ構文を使用して、非フラグ列挙体の複数の条件に対して動作する <xref:System.Windows.Trigger> を作成することはできません。  
  
```xaml  
<!--This will not compile, because Visibility is not a flagwise enumeration.-->  
...  
<Trigger Property="Visibility" Value="Collapsed,Hidden">  
  <Setter ... />  
</Trigger>  
...  
```  
  
 XAML で設定可能な属性をサポートするフラグ列挙は、WPF ではほとんど発生しません。 ただし、このような列挙型の1つには <xref:System.Windows.Media.StyleSimulations>があります。 たとえば、コンマ区切りのフラグ属性構文を使用して、<xref:System.Windows.Documents.Glyphs> クラスの解説に記載されている例を変更することができます。`StyleSimulations = "BoldSimulation"` が `StyleSimulations = "BoldSimulation,ItalicSimulation"`になる可能性があります。 <xref:System.Windows.Input.KeyBinding.Modifiers%2A?displayProperty=nameWithType> は、複数の列挙値を指定できるもう1つのプロパティです。 ただし、<xref:System.Windows.Input.ModifierKeys> 列挙体は独自の型コンバーターをサポートするため、このプロパティは特別なケースになります。 修飾子の型コンバーターは、コンマ (,) ではなく、区切り記号としてプラス記号 (+) を使用します。 この変換では、"Ctrl + Alt" など、Microsoft Windows プログラミングでのキーの組み合わせを表す従来の構文がサポートされています。  
  
### <a name="properties-and-event-member-name-references"></a>プロパティとイベントメンバー名の参照  
 属性を指定すると、親オブジェクト要素に対してインスタンス化された CLR 型のメンバーとして存在する任意のプロパティまたはイベントを参照できます。  
  
 または、添付プロパティまたは添付イベントを、それを含んでいるオブジェクト要素とは無関係に参照できます。 (添付プロパティについては、次のセクションで説明します)。  
  
 また、 *typeName*を使用して、既定の名前空間を介してアクセスできる任意のオブジェクトから、任意のイベントの名前を指定することもできます。*イベント*部分修飾名;この構文では、ルーティングイベントのハンドラーのアタッチがサポートされています。このハンドラーは、子要素からのイベントルーティングを処理することを意図していますが、親要素はそのメンバーテーブルにそのイベントを持っていません。 この構文は添付イベントの構文に似ていますが、ここでのイベントは添付イベントではありません。 代わりに、修飾名を持つイベントを参照しています。 詳細については、「[ルーティングイベントの概要](routed-events-overview.md)」を参照してください。  
  
 一部のシナリオでは、属性名ではなく、属性の値としてプロパティ名が指定されることがあります。 このプロパティ名には、 *ownerType*の形式で指定されたプロパティなどの修飾子を含めることもできます。*Dependencypropertyname*。 このシナリオは、XAML でスタイルやテンプレートを作成する場合によく見られます。 属性値として指定されたプロパティ名の処理規則は異なり、設定されるプロパティの型、または特定の WPF サブシステムの動作によって管理されます。 詳細については、「[スタイルとテンプレート](../controls/styling-and-templating.md)」を参照してください。  
  
 プロパティ名のもう1つの使用方法は、属性値によってプロパティとプロパティのリレーションシップが記述されている場合です。 この機能は、データバインディングとストーリーボードターゲットに対して使用され、<xref:System.Windows.PropertyPath> クラスとその型コンバーターによって有効になります。 参照セマンティクスの詳細については、「 [PROPERTYPATH XAML 構文](propertypath-xaml-syntax.md)」を参照してください。  
  
<a name="property_element_syntax"></a>   
## <a name="property-element-syntax"></a>Property 要素の構文  
 *Property 要素の構文*は、要素の基本的な XML 構文規則とは少し異なる構文です。 XML では、属性の値は、文字列エンコード形式が使用されている唯一のバリエーションである、事実上の文字列です。 XAML では、他のオブジェクト要素をプロパティの値として割り当てることができます。 この機能は、property 要素の構文によって有効になります。 要素タグ内の属性として指定されているプロパティではなく、 *Elementtypename*の開始要素タグを使用してプロパティが指定されています。*propertyName*フォームでは、プロパティの値が内で指定され、プロパティ要素が閉じられます。  
  
 具体的には、構文は左山かっこ (\<) で始まり、その直後に、プロパティ要素の構文が含まれているクラスまたは構造体の型名が続きます。 これには、直後に1つのドット (.)、次にプロパティの名前、右山かっこ (>) が続きます。 属性構文と同様に、そのプロパティは、指定した型の宣言されたパブリックメンバー内に存在する必要があります。 プロパティに割り当てられる値は、property 要素内に格納されます。 通常、値は1つ以上のオブジェクト要素として指定されます。これは、オブジェクトを値として指定することが、property 要素の構文が対処するシナリオであるためです。 最後に、同じ*Elementtypename*を指定する、同等の終了タグを指定します。*propertyName*を適切に入れ子にし、他の要素タグとのバランスを取るには、propertyName の組み合わせを指定する必要があります。  
  
 たとえば、次に示すのは、<xref:System.Windows.Controls.Button>の <xref:System.Windows.FrameworkElement.ContextMenu%2A> プロパティの property 要素の構文です。  
  
 [!code-xaml[XAMLOvwSupport#ContextMenu](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/Page1.xaml#contextmenu)]  
  
 また、プロパティ要素内の値を内部テキストとして指定することもできます。これは、指定されているプロパティの型が <xref:System.String>などのプリミティブ値型である場合や、名前が指定されている列挙型の場合です。 これらの2つの使用方法は、これらの各ケースでより単純な属性構文を使用することもできるため、あまり一般的ではありません。 プロパティ要素に文字列を入力する場合の1つのシナリオは、XAML コンテンツプロパティではなく、UI テキストの表現に使用されるプロパティです。また、ラインフィードなどの特定の空白要素は、その UI テキストに表示される必要があります。 属性の構文では、改行を保持できませんが、有意な空白の保持がアクティブになっている限り、プロパティ要素の構文を使用できます (詳細については、「 [XAML での空白の処理](../../../desktop-wpf/xaml-services/white-space-processing.md)」を参照してください)。 もう1つのシナリオとして、 [X:Uid ディレクティブ](../../../desktop-wpf/xaml-services/xuid-directive.md)を property 要素に適用し、WPF 出力 BAML または他の手法でローカライズする必要がある値としてその中の値をマークすることができます。  
  
 プロパティ要素は、WPF 論理ツリーでは表されません。 プロパティ要素は、プロパティを設定するための特定の構文であり、それをバッキングするインスタンスまたはオブジェクトを持つ要素ではありません。 (論理ツリーの概念の詳細については、「 [WPF のツリー](trees-in-wpf.md)」を参照してください)。  
  
 属性とプロパティ要素の両方の構文がサポートされているプロパティについては、通常、2つの構文が同じ結果になります。ただし、空白の処理などの微妙な違いは構文によって多少異なります。  
  
<a name="collection_syntax"></a>   
## <a name="collection-syntax"></a>コレクションの構文  
 XAML 仕様では、値の型がコレクションであるプロパティを識別するために、XAML プロセッサの実装が必要です。 .NET での一般的な XAML プロセッサの実装は、マネージコードと CLR に基づいており、次のいずれかを使用してコレクション型を識別します。  
  
- 型は <xref:System.Collections.IList>を実装します。  
  
- 型は <xref:System.Collections.IDictionary>を実装します。  
  
- 型は <xref:System.Array> から派生します (XAML の配列の詳細については、「 [X:Array Markup Extension](../../../desktop-wpf/xaml-services/xarray-markup-extension.md)」を参照してください)。  
  
 プロパティの型がコレクションである場合、推論されたコレクション型は、マークアップでオブジェクト要素として指定する必要はありません。 代わりに、コレクション内の項目になる要素は、property 要素の1つ以上の子要素として指定されます。 これらの各項目は、読み込み中にオブジェクトに評価され、暗黙的なコレクションの `Add` メソッドを呼び出すことによってコレクションに追加されます。 たとえば、<xref:System.Windows.Style> の <xref:System.Windows.Style.Triggers%2A> プロパティは、<xref:System.Collections.IList>を実装する特殊なコレクション型 <xref:System.Windows.TriggerCollection>を受け取ります。 マークアップで <xref:System.Windows.TriggerCollection> オブジェクト要素をインスタンス化する必要はありません。 代わりに、1つ以上の <xref:System.Windows.Trigger> 項目を `Style.Triggers` property 要素内の要素として指定します。ここで <xref:System.Windows.Trigger> (または派生クラス) は、厳密に型指定された <xref:System.Windows.TriggerCollection>の項目の種類として想定される型です。  
  
 [!code-xaml[XAMLOvwSupport#SyntaxPECollection](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/Page1.xaml#syntaxpecollection)]  
  
 プロパティは、コレクション型と、その型および派生型の XAML コンテンツプロパティの両方にすることができます。詳細については、このトピックの次のセクションで説明します。  
  
 暗黙的なコレクション要素は、マークアップに要素として表示されない場合でも、論理ツリー表現にメンバーを作成します。 通常、親型のコンストラクターは、そのプロパティの1つであるコレクションのインスタンス化を実行します。最初に空のコレクションがオブジェクトツリーの一部になります。  
  
> [!NOTE]
> ジェネリックリストとディクショナリインターフェイス (<xref:System.Collections.Generic.IList%601> および <xref:System.Collections.Generic.IDictionary%602>) は、コレクションの検出ではサポートされていません。 ただし、<xref:System.Collections.IList> を直接実装するか、基本クラスとして <xref:System.Collections.Generic.Dictionary%602> するため、<xref:System.Collections.Generic.List%601> クラスを基底クラスとして使用できます。これは <xref:System.Collections.IDictionary> を直接実装するためです。  
  
 コレクション型の .NET 参照ページでは、コレクションの object 要素を意図的に省略したこの構文は、XAML 構文のセクションで暗黙のコレクション構文として示されることがあります。  
  
 ルート要素を除き、別の要素の子要素として入れ子になっている XAML ファイル内のすべてのオブジェクト要素は、実際には、次のいずれかまたは両方の要素であり、その親要素の暗黙的なコレクションプロパティのメンバーになります。、または親要素の XAML コンテンツプロパティの値を指定する要素 (XAML コンテンツプロパティについては、今後のセクションで説明します)。 つまり、マークアップページの親要素と子要素の関係は、実際にはルートの単一のオブジェクトであり、ルートの下にあるすべてのオブジェクト要素は、親のプロパティ値を提供する単一のインスタンスか、または col 内のいずれかの項目です。親のコレクション型のプロパティ値でもある lection います。 この単一ルートの概念は XML に共通しており、多くの場合、<xref:System.Windows.Markup.XamlReader.Load%2A>などの XAML を読み込む Api の動作について強化されています。  
  
 次の例は、コレクション (<xref:System.Windows.Media.GradientStopCollection>) の object 要素を明示的に指定した構文です。  
  
```xaml  
<LinearGradientBrush>  
  <LinearGradientBrush.GradientStops>  
    <GradientStopCollection>  
      <GradientStop Offset="0.0" Color="Red" />  
      <GradientStop Offset="1.0" Color="Blue" />  
    </GradientStopCollection>  
  </LinearGradientBrush.GradientStops>  
</LinearGradientBrush>  
```  
  
 常にコレクションを明示的に宣言することはできないことに注意してください。 たとえば、前に示した <xref:System.Windows.Style.Triggers%2A> の例で <xref:System.Windows.TriggerCollection> を明示的に宣言しようとすると、失敗します。 コレクションを明示的に宣言するには、コレクションクラスがパラメーターなしのコンストラクターをサポートし、<xref:System.Windows.TriggerCollection> にパラメーターなしのコンストラクターが含まれている必要があります。  
  
<a name="xaml_content_properties"></a>   
## <a name="xaml-content-properties"></a>XAML コンテンツのプロパティ  
 XAML コンテンツ構文は、クラス宣言の一部として <xref:System.Windows.Markup.ContentPropertyAttribute> を指定するクラスでのみ有効な構文です。 <xref:System.Windows.Markup.ContentPropertyAttribute> は、その型の要素 (派生クラスを含む) のコンテンツプロパティであるプロパティ名を参照します。 XAML プロセッサによって処理される場合、オブジェクト要素の開始タグと終了タグの間に存在する子要素または内部テキストは、そのオブジェクトの XAML コンテンツプロパティの値として割り当てられます。 Content プロパティの明示的なプロパティ要素を指定することは許可されていますが、この使用方法は一般に .NET リファレンスの XAML 構文のセクションには表示されません。 明示的/詳細手法では、マークアップのわかりやすさやマークアップスタイルの場合によっては、ときどき値が使用されますが、通常、コンテンツプロパティの目的は、親子として直感的に関連付けられた要素を直接入れ子にできるように、マークアップを効率化することです。 要素のその他のプロパティのプロパティ要素タグは、厳密な XAML 言語定義では "content" として割り当てられません。これらは、以前に XAML パーサーの処理順序で処理され、"コンテンツ" とは見なされません。  
  
### <a name="xaml-content-property-values-must-be-contiguous"></a>XAML コンテンツプロパティの値は連続している必要があります  
 XAML コンテンツプロパティの値は、そのオブジェクト要素の他のプロパティ要素の前または全体のいずれかに指定する必要があります。 これは、XAML コンテンツプロパティの値が文字列として指定されているか、または1つ以上のオブジェクトとして指定されている場合に当てはまります。 たとえば、次のマークアップは解析されません。  
  
```xaml  
<Button>I am a   
  <Button.Background>Blue</Button.Background>  
  blue button</Button>  
```  
  
 これは基本的に、コンテンツプロパティの property 要素構文を使用してこの構文が明示的に作成されている場合、content プロパティは2回設定されるため、これは無効です。  
  
```xaml  
<Button>  
  <Button.Content>I am a </Button.Content>  
  <Button.Background>Blue</Button.Background>  
  <Button.Content> blue button</Button.Content>  
</Button>  
```  
  
 同様に無効な例として、コンテンツプロパティがコレクションであり、子要素にプロパティ要素が混在している場合があります。  
  
```xaml  
<StackPanel>  
  <Button>This example</Button>  
  <StackPanel.Resources>  
    <SolidColorBrush x:Key="BlueBrush" Color="Blue"/>  
  </StackPanel.Resources>  
  <Button>... is illegal XAML</Button>  
</StackPanel>  
```  
  
<a name="content_properties_and_collection_syntax_combined"></a>   
## <a name="content-properties-and-collection-syntax-combined"></a>コンテンツのプロパティとコレクション構文の組み合わせ  
 複数のオブジェクト要素をコンテンツとして受け入れるには、コンテンツプロパティの型がコレクション型である必要があります。 コレクション型のプロパティ要素構文と同様に、XAML プロセッサはコレクション型である型を識別する必要があります。 要素に XAML コンテンツプロパティがあり、XAML コンテンツプロパティの型がコレクションである場合、オブジェクト要素としてマークアップで暗黙的なコレクション型を指定する必要はありません。また、XAML コンテンツプロパティをプロパティとして指定する必要もありません。契約. そのため、マークアップの見かけ上のコンテンツモデルでは、複数の子要素をコンテンツとして割り当てることができるようになりました。 <xref:System.Windows.Controls.Panel> 派生クラスのコンテンツ構文を次に示します。 すべての <xref:System.Windows.Controls.Panel> 派生クラスは、XAML コンテンツプロパティを <xref:System.Windows.Controls.Panel.Children%2A>に設定します。これには <xref:System.Windows.Controls.UIElementCollection>型の値が必要です。  
  
 [!code-xaml[XAMLOvwSupport#SyntaxContent](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page5.xaml#syntaxcontent)]  
  
 マークアップには <xref:System.Windows.Controls.Panel.Children%2A> のプロパティ要素も <xref:System.Windows.Controls.UIElementCollection> の要素も必要ないことに注意してください。 これは XAML の設計機能であり、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を定義する再帰的包含要素は、プロパティ要素のタグやコレクションオブジェクトを介在させることなく、直接の親子要素のリレーションシップを持つ入れ子になった要素のツリーとして表現されます。 実際には、マークアップでオブジェクト要素として明示的に <xref:System.Windows.Controls.UIElementCollection> を指定することはできません。 唯一の用途は暗黙のコレクションであるため、<xref:System.Windows.Controls.UIElementCollection> は、パラメーターなしのパブリックコンストラクターを公開しないため、オブジェクト要素としてインスタンス化することはできません。  
  
### <a name="mixing-property-elements-and-object-elements-in-an-object-with-a-content-property"></a>コンテンツプロパティを使用してオブジェクト内のプロパティ要素とオブジェクト要素を混在させる  
 Xaml 仕様では、オブジェクト要素内の XAML コンテンツプロパティを格納するために使用されるオブジェクト要素が連続している必要があり、混合されていない必要があることが XAML プロセッサによって宣言されています。 プロパティ要素とコンテンツの混合に対するこの制限は、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の XAML プロセッサによって適用されます。  
  
 オブジェクト要素内の最初の即時マークアップとして子オブジェクト要素を持つことができます。 その後、プロパティ要素を導入できます。 または、1つまたは複数の property 要素、次にコンテンツ、さらにプロパティ要素を指定することもできます。 ただし、プロパティ要素がコンテンツの後に続く場合は、それ以上コンテンツを導入することはできません。プロパティ要素を追加することはできません。  
  
 このコンテンツ/プロパティ要素の順序要件は、コンテンツとして使用される内部テキストには適用されません。 ただし、プロパティ要素に内部テキストが混在している場合は、大きな空白がマークアップで視覚的に検出されることが困難になるため、内部テキストを連続したままにするための適切なマークアップスタイルでもあります。  
  
<a name="xaml_namespaces"></a>   
## <a name="xaml-namespaces"></a>XAML 名前空間  
 前の構文の例では、既定の XAML 名前空間以外の XAML 名前空間が指定されていません。 一般的な [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションでは、既定の XAML 名前空間が [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 名前空間として指定されます。 既定の XAML 名前空間以外の XAML 名前空間を指定しても、同様の構文を使用できます。 ただし、既定の XAML 名前空間内でアクセスできないクラスの名前が付いている場合は、そのクラス名の前に、対応する CLR 名前空間にマップされている XAML 名前空間のプレフィックスを付ける必要があります。 たとえば `<custom:Example/>` は、`Example` クラスのインスタンスをインスタンス化するためのオブジェクト要素構文です。このクラスには、そのクラスを含む CLR 名前空間 (および場合によっては、バッキング型を含む外部アセンブリ情報) が `custom` プレフィックスにマップされています。  
  
 XAML 名前空間の詳細については、「 [WPF xaml の Xaml 名前空間と名前空間のマッピング](xaml-namespaces-and-namespace-mapping-for-wpf-xaml.md)」を参照してください。  
  
<a name="markup_extensions"></a>   
## <a name="markup-extensions"></a>マークアップ拡張機能  
 XAML は、文字列属性値またはオブジェクト要素の通常の XAML プロセッサ処理からのエスケープを有効にし、処理をバッキングクラスに延期する、マークアップ拡張機能のプログラミングエンティティを定義します。 属性構文を使用するときに XAML プロセッサのマークアップ拡張機能を識別する文字は左中かっこ ({) で、その後に右中かっこ (}) 以外の任意の文字が続きます。 左中かっこの後の最初の文字列は、特定の拡張機能の動作を提供するクラスを参照する必要があります。その部分文字列が真のクラス名の一部である場合、参照では部分文字列 "Extension" が省略される可能性があります。 その後、1つのスペースが表示され、右中かっこが検出されるまで、後続の各文字が拡張機能の実装による入力として使用されます。  
  
 .NET XAML の実装では、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] によってサポートされているすべてのマークアップ拡張機能の基礎として <xref:System.Windows.Markup.MarkupExtension> 抽象クラス、およびその他のフレームワークやテクノロジを使用します。 特に実装 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] れるマークアップ拡張機能は、他の既存のオブジェクトを参照する手段を提供すること、または実行時に評価されるオブジェクトへの遅延参照を行うことを目的としています。 たとえば、単純な WPF データバインディングは、特定のプロパティが通常受け取る値の代わりに `{Binding}` マークアップ拡張機能を指定することによって実現されます。 WPF マークアップ拡張機能の多くは、属性構文を使用できないプロパティの属性構文を有効にします。 たとえば、<xref:System.Windows.Style> オブジェクトは、入れ子になった一連のオブジェクトとプロパティを含む比較的複雑な型です。 WPF のスタイルは、通常、<xref:System.Windows.ResourceDictionary>のリソースとして定義され、リソースを要求する2つの WPF マークアップ拡張機能のいずれかを介して参照されます。 マークアップ拡張機能は、プロパティ値の評価をリソース参照に延期し、次の例に示すように、属性構文で型 <xref:System.Windows.Style>を使用して、<xref:System.Windows.FrameworkElement.Style%2A> プロパティの値を提供できるようにします。  
  
 `<Button Style="{StaticResource MyStyle}">My button</Button>`  
  
 ここでは、`StaticResource` マークアップ拡張機能の実装を提供する <xref:System.Windows.StaticResourceExtension> クラスを識別します。 次の文字列 `MyStyle` は、既定以外の <xref:System.Windows.StaticResourceExtension> コンストラクターの入力として使用されます。この場合、拡張文字列から取得したパラメーターは、要求された <xref:System.Windows.ResourceKey>を宣言します。 `MyStyle` は、リソースとして定義された <xref:System.Windows.Style> の[x:Key](../../../desktop-wpf/xaml-services/xkey-directive.md)値である必要があります。 [StaticResource マークアップ拡張機能](staticresource-markup-extension.md)の使用法では、リソースを使用して、読み込み時に静的なリソース検索ロジックによって <xref:System.Windows.Style> プロパティ値を提供するように要求します。  
  
 マークアップ拡張機能の詳細については、 「[マークアップ拡張機能と WPF XAML](markup-extensions-and-wpf-xaml.md)」を参照してください。 一般的な .NET XAML 実装で有効になっているマークアップ拡張機能とその他の XAML プログラミング機能のリファレンスについては、「 [XAML 名前空間 (x:)」を参照してください。言語機能](../../../desktop-wpf/xaml-services/namespace-language-features.md)。 WPF 固有のマークアップ拡張機能については、「 [WPF XAML 拡張](wpf-xaml-extensions.md)機能」を参照してください。  
  
<a name="attached_properties"></a>   
## <a name="attached-properties"></a>アタッチされるプロパティ  
 添付プロパティは、XAML で導入されたプログラミング概念です。プロパティは、特定の型で所有および定義できますが、任意の要素の属性またはプロパティ要素として設定できます。 添付プロパティが対象となる主なシナリオは、マークアップ構造内の子要素が、すべての要素にわたって広範囲に共有されるオブジェクトモデルを必要とせずに、親要素に情報を報告できるようにすることです。 逆に、添付プロパティは、親要素が子要素に情報を報告するために使用できます。 添付プロパティの目的と、アタッチされたプロパティを作成する方法の詳細については、「[添付プロパティの概要](attached-properties-overview.md)」を参照してください。  
  
 添付プロパティでは、 *typeName*を指定することによって、プロパティ要素の構文に似た構文を使用します、一見、。*propertyName*の組み合わせ。 次の 2 つの重要な違いがあります。  
  
- *TypeName*を使用できます。属性構文を介して添付プロパティを設定する場合でも、 *propertyName*の組み合わせ。 添付プロパティは、プロパティ名を修飾することが属性構文の要件である唯一のケースです。  
  
- 添付プロパティのプロパティ要素構文を使用することもできます。 ただし、プロパティ要素の一般的な構文では、指定する*typeName*は property 要素を含む object 要素です。 添付プロパティを参照している場合、 *typeName*は添付プロパティを定義するクラスであり、それを含んでいるオブジェクト要素ではありません。  
  
<a name="attached_events"></a>   
## <a name="attached-events"></a>添付イベント  
 添付イベントは、XAML で導入された別のプログラミング概念であり、特定の型でイベントを定義できますが、どのオブジェクト要素にもハンドラーをアタッチできます。 WOF 実装では、添付イベントを定義する型が、サービスを定義する静的な型であることがよくあります。また、アタッチされるイベントは、サービスを公開する型のルーティングイベントエイリアスによって公開されることもあります。 添付イベントのハンドラーは、属性の構文によって指定されます。 添付イベントと同様に、 *typeName*を許可するために、添付イベントの属性構文が拡張されています。*eventname* usage。ここで、 *typeName*は添付イベントインフラストラクチャの `Add` および `Remove` イベントハンドラーアクセサーを提供するクラスであり、 *eventName*はイベント名です。  
  
<a name="anatomy_of_a_xaml_page_root_element"></a>   
## <a name="anatomy-of-a-xaml-root-element"></a>XAML ルート要素の構造  
 次の表は、ルート要素の特定の属性を示す、一般的な XAML ルート要素が分割されている状態を示しています。  
  
|||  
|-|-|  
|`<Page`|ルート要素のオブジェクト要素を開いています|  
|`xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"`|既定 ([!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]) の XAML 名前空間|  
|`xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`|XAML 言語の XAML 名前空間|  
|`x:Class="ExampleNamespace.ExampleCode"`|部分クラスに定義されている分離コードにマークアップを接続する部分クラスの宣言|  
|`>`|ルートのオブジェクト要素の最後。 要素に子要素が含まれているため、オブジェクトはまだ閉じられていません|  
  
<a name="optional_and_nonrecommended_xaml_usages"></a>   
## <a name="optional-and-nonrecommended-xaml-usages"></a>省略可能で推奨されない XAML の使用  
 以下のセクションでは、xaml プロセッサによって技術的にサポートされる XAML の使用方法について説明しますが、xaml ソースを含むアプリケーションを開発するときに、ユーザーが判読できるように、XAML ファイルに影響を与える詳細な問題やその他の問題を生成します。  
  
### <a name="optional-property-element-usages"></a>省略可能なプロパティ要素の使用法  
 省略可能なプロパティ要素の使用には、XAML プロセッサによって暗黙的に考慮される要素コンテンツプロパティの明示的な書き込みが含まれます。 たとえば、<xref:System.Windows.Controls.Menu>の内容を宣言する場合、<xref:System.Windows.Controls.Menu> の <xref:System.Windows.Controls.ItemsControl.Items%2A> コレクションを `<Menu.Items>` property 要素タグとして明示的に宣言し、<xref:System.Windows.Controls.MenuItem> 内に各 `<Menu.Items>`を配置します。これは、<xref:System.Windows.Controls.Menu> のすべての子要素が <xref:System.Windows.Controls.MenuItem> である必要があり、<xref:System.Windows.Controls.ItemsControl.Items%2A> コレクションに配置されている必要があるという暗黙の XAML プロセッサ動作ではありません。 場合によっては、マークアップで表されるオブジェクト構造を視覚的に明確にするために、オプションの使用方法が役立つことがあります。 場合によっては、明示的なプロパティ要素の使用によって、技術的には機能するものの、属性値内の入れ子になったマークアップ拡張機能など、視覚的にはわかりにくいマークアップを回避できます。  
  
### <a name="full-typenamemembername-qualified-attributes"></a>完全な typeName. memberName 修飾属性  
 *TypeName*です。属性の*memberName*フォームは、ルーティングイベントのケースだけでなく、実際にはより汎用的に機能します。 ただし、その他の状況では、マークアップスタイルと読みやすさの理由だけで、フォームは不要であり、回避する必要があります。 次の例では、<xref:System.Windows.Controls.Control.Background%2A> 属性への3つの参照がまったく同じになります。  
  
 [!code-xaml[XAMLOvwSupport#TypeNameProp](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page8.xaml#typenameprop)]  
  
 `Button.Background` は、<xref:System.Windows.Controls.Button> でそのプロパティの修飾された参照が正常に (<xref:System.Windows.Controls.Control.Background%2A> がコントロールから継承された)、<xref:System.Windows.Controls.Button> が object 要素または基底クラスのクラスであるために機能します。 <xref:System.Windows.Controls.Control> クラスは実際に <xref:System.Windows.Controls.Control.Background%2A> を定義し、<xref:System.Windows.Controls.Control> は <xref:System.Windows.Controls.Button> 基底クラスであるため、`Control.Background` 機能します。  
  
 ただし、次の*typeName*は次のようになります。*memberName*フォームの例は機能しません。そのため、次のコメントが表示されます。  
  
 [!code-xaml[XAMLOvwSupport#TypeNameBadProp](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page8.xaml#typenamebadprop)]  
  
 <xref:System.Windows.Controls.Label> は <xref:System.Windows.Controls.Control>のもう1つの派生クラスであり、<xref:System.Windows.Controls.Label> object 要素内で `Label.Background` を指定した場合、この使用方法が動作します。 ただし、<xref:System.Windows.Controls.Label> は <xref:System.Windows.Controls.Button>のクラスまたは基本クラスではないため、指定された XAML プロセッサの動作では、`Label.Background` を添付プロパティとして処理します。 `Label.Background` は添付プロパティとして使用できないため、この使用法は失敗します。  
  
### <a name="basetypenamemembername-property-elements"></a>baseTypeName. memberName プロパティの要素  
 *TypeName*の方法に似ています。*memberName*フォームは、 *basetypename*という属性構文に対して機能します。*memberName*構文は、プロパティ要素構文で使用できます。 たとえば、次の構文は機能します。  
  
 [!code-xaml[XAMLOvwSupport#GoofyPE](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page8.xaml#goofype)]  
  
 ここでは、property 要素が `Button`に含まれていた場合でも、property 要素は `Control.Background` として指定されていました。  
  
 ただし、 *typeName*と同様です。属性の*memberName*フォーム、 *basetypename*。*memberName*はマークアップの形式が不適切なため、回避する必要があります。  
  
## <a name="see-also"></a>参照

- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
- [XAML 名前空間 (x:) 言語機能](../../../desktop-wpf/xaml-services/namespace-language-features.md)
- [WPF XAML 拡張機能](wpf-xaml-extensions.md)
- [依存関係プロパティの概要](dependency-properties-overview.md)
- [TypeConverters および XAML](typeconverters-and-xaml.md)
- [WPF における XAML とカスタム クラス](xaml-and-custom-classes-for-wpf.md)
