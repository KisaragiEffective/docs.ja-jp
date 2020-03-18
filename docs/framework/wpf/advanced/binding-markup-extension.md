---
title: バインドのマークアップ拡張機能
ms.date: 03/30/2017
f1_keywords:
- Binding
helpviewer_keywords:
- Binding markup extensions [WPF]
- XAML [WPF], Binding markup extension
ms.assetid: 83d6e2a4-1b0c-4fc8-bd96-b5e98800ab63
ms.openlocfilehash: d0a437e756da16db978d8074c4355fd83d211b67
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73453613"
---
# <a name="binding-markup-extension"></a>バインドのマークアップ拡張機能
プロパティ値をデータバインド値にして、中間式オブジェクトを作成し、実行時に要素とそのバインディングに適用されるデータコンテキストを解釈します。  
  
## <a name="binding-expression-usage"></a>バインディング式の使用法  
  
```xaml  
<object property="{Binding}" .../>  
-or-  
<object property="{Binding  bindProp1=value1[, bindPropN=valueN]*}" ...  
/>  
-or-  
<object property="{Binding path}" .../>  
-or  
<object property="{Binding path[, bindPropN=valueN]*}" .../>  
```  
  
## <a name="syntax-notes"></a>構文に関する注意事項  
 これらの構文では、`[]` と `*` はリテラルではありません。 これらは、0個以上の*bindprop*`=`*値*のペアを使用できることを示す表記の一部であり、これらのペアと、それに続く*bindprop*`=`*値*のペアの間に `,` の区切り記号があります。  
  
 "バインディング拡張機能で設定できるバインディングプロパティ" セクションに示されているプロパティは、<xref:System.Windows.Data.Binding> オブジェクト要素の属性を使用して設定できます。 ただし、<xref:System.Windows.Data.Binding>のマークアップ拡張機能の使用は実際には行われませんが、CLR <xref:System.Windows.Data.Binding> クラスのプロパティを設定する属性の一般的な XAML 処理にすぎません。 つまり、`<Binding` *bindProp1*`="`*Value1*`"[` *Bindpropn*`="`*valuen*`"]*/>` は、<xref:System.Windows.Data.Binding> 式の使用ではなく `Binding` オブジェクト要素の使用法の属性に対して同等の構文です。 <xref:System.Windows.Data.Binding>の特定のプロパティの XAML 属性の使用方法については、.NET Framework クラスライブラリの <xref:System.Windows.Data.Binding> の関連プロパティの「XAML 属性の使用方法」セクションを参照してください。  
  
## <a name="xaml-values"></a>XAML 値  
  
|||  
|-|-|  
|`bindProp1, bindPropN`|設定する <xref:System.Windows.Data.Binding> または <xref:System.Windows.Data.BindingBase> プロパティの名前。 すべての <xref:System.Windows.Data.Binding> プロパティを `Binding` 拡張機能で設定できるわけではありません。一部のプロパティは、さらに入れ子になったマークアップ拡張機能を使用することによってのみ、`Binding` 式内で設定できます。 「バインディング拡張機能で設定できるバインディングプロパティ」を参照してください。|  
|`value1, valueN`|プロパティに設定する値。 属性値の処理は、最終的には、設定される特定の <xref:System.Windows.Data.Binding> プロパティの型とロジックに固有です。|  
|`path`|暗黙的な <xref:System.Windows.Data.Binding.Path%2A?displayProperty=nameWithType> プロパティを設定するパス文字列。 「 [PROPERTYPATH XAML 構文](propertypath-xaml-syntax.md)」も参照してください。|  
  
## <a name="unqualified-binding"></a>修飾なし {Binding}  
 「バインド式の使用法」に示されている `{Binding}` 使用法では、`null`の初期 <xref:System.Windows.Data.Binding.Path%2A?displayProperty=nameWithType> を含む既定値を使用して <xref:System.Windows.Data.Binding> オブジェクトを作成します。 これは、作成された <xref:System.Windows.Data.Binding> が実行時データコンテキストで設定されている <xref:System.Windows.Data.Binding.Path%2A?displayProperty=nameWithType> や <xref:System.Windows.Data.Binding.Source%2A?displayProperty=nameWithType> などのキーデータバインディングプロパティに依存している可能性があるため、多くのシナリオで役立ちます。 データコンテキストの概念の詳細については、「[データバインディング](../data/data-binding-wpf.md)」を参照してください。  
  
## <a name="implicit-path"></a>暗黙的なパス  
 `Binding` マークアップ拡張機能では、<xref:System.Windows.Data.Binding.Path%2A?displayProperty=nameWithType> を概念 "既定のプロパティ" として使用します。この場合、`Path=` は式に表示される必要はありません。 暗黙的なパスを使用して `Binding` 式を指定する場合は、その他の `bindProp`=`value` ペアの前に、暗黙的なパスを式の先頭に指定する必要があります。これは、<xref:System.Windows.Data.Binding> プロパティが name で指定されている場合です。 例: `{Binding PathString}`。 `PathString` は、マークアップ拡張機能の使用によって作成された <xref:System.Windows.Data.Binding> 内の <xref:System.Windows.Data.Binding.Path%2A?displayProperty=nameWithType> の値として評価される文字列です。 コンマ区切り記号の後に、`{Binding LastName, Mode=TwoWay}`などの他の名前付きプロパティを持つ暗黙的なパスを追加できます。  
  
## <a name="binding-properties-that-can-be-set-with-the-binding-extension"></a>バインディング拡張機能を使用して設定できるバインディングプロパティ  
 このトピックで示す構文では、一般的な `bindProp`=`value` 近似値を使用します。これは、<xref:System.Windows.Data.Binding> マークアップ拡張機能/式構文を使用して設定できる <xref:System.Windows.Data.BindingBase> または `Binding` の読み取り/書き込みプロパティが多数あるためです。 暗黙の <xref:System.Windows.Data.Binding.Path%2A?displayProperty=nameWithType>を除き、任意の順序で設定できます。 (`Path=`を明示的に指定することもできます。この場合、任意の順序で設定できます)。 基本的に、コンマで区切られた `value` のペアを `bindProp`=使用して、以下の一覧の0個以上のプロパティを設定できます。  
  
 これらのプロパティ値のいくつかには、XAML のテキスト構文からのネイティブな型変換をサポートしていないオブジェクト型が必要であり、属性値として設定するためにマークアップ拡張機能が必要です。 詳細については、各プロパティの .NET Framework クラスライブラリにある XAML 属性の使用方法に関するセクションを参照してください。XAML の属性構文に使用する文字列は、マークアップ拡張機能の使用をさらに追加しないと、基本的には `Binding` 式で指定した値と同じです。ただし、`bindProp`=`value` を引用符で囲まないでください。`Binding` 式。  
  
- <xref:System.Windows.Data.BindingBase.BindingGroupName%2A>: 使用可能なバインドグループを識別する文字列。 これは、比較的高度なバインド概念です。<xref:System.Windows.Data.BindingBase.BindingGroupName%2A>のリファレンスページを参照してください。  
  
- <xref:System.Windows.Data.Binding.BindsDirectlyToSource%2A>: ブール値は、`true` または `false`のいずれかになります。 既定値は、 `false`です。  
  
- <xref:System.Windows.Data.Binding.Converter%2A>: 式で `value` 文字列 =`bindProp`として設定できますが、そのためには、 [StaticResource マークアップ拡張機能](staticresource-markup-extension.md)など、値のオブジェクト参照が必要です。 この場合の値は、カスタムコンバータークラスのインスタンスです。  
  
- <xref:System.Windows.Data.Binding.ConverterCulture%2A>: 標準ベースの識別子として式で設定できます。<xref:System.Windows.Data.Binding.ConverterCulture%2A>については、リファレンストピックを参照してください。  
  
- <xref:System.Windows.Data.Binding.ConverterParameter%2A>: 式で `value` 文字列 =`bindProp`として設定できますが、これは渡されるパラメーターの型によって異なります。 値の参照型を渡す場合、この使用法には、入れ子になった[StaticResource マークアップ拡張機能](staticresource-markup-extension.md)などのオブジェクト参照が必要です。  
  
- <xref:System.Windows.Data.Binding.ElementName%2A>: 相互に排他的な and <xref:System.Windows.Data.Binding.RelativeSource%2A> と <xref:System.Windows.Data.Binding.Source%2A>;これらの各バインドプロパティは、特定のバインディング方法を表します。 「[データバインディングの概要](../data/data-binding-overview.md)」を参照してください。  
  
- <xref:System.Windows.Data.BindingBase.FallbackValue%2A>: 式で `value` 文字列 =`bindProp`として設定できますが、これは渡される値の型によって異なります。 参照型を渡す場合、には、入れ子になった[StaticResource マークアップ拡張機能](staticresource-markup-extension.md)などのオブジェクト参照が必要です。  
  
- <xref:System.Windows.Data.Binding.IsAsync%2A>: ブール値は、`true` または `false`のいずれかになります。 既定値は、 `false`です。  
  
- <xref:System.Windows.Data.Binding.Mode%2A>:*値*は、<xref:System.Windows.Data.BindingMode> 列挙体の定数名です。 たとえば、`{Binding Mode=OneWay}` のようにします。  
  
- <xref:System.Windows.Data.Binding.NotifyOnSourceUpdated%2A>: ブール値は、`true` または `false`のいずれかになります。 既定値は、 `false`です。  
  
- <xref:System.Windows.Data.Binding.NotifyOnTargetUpdated%2A>: ブール値は、`true` または `false`のいずれかになります。 既定値は、 `false`です。  
  
- <xref:System.Windows.Data.Binding.NotifyOnValidationError%2A>: ブール値は、`true` または `false`のいずれかになります。 既定値は、 `false`です。  
  
- <xref:System.Windows.Data.Binding.Path%2A>: データオブジェクトまたは一般的なオブジェクトモデルへのパスを記述する文字列。 この形式は、このトピックでは適切に記述できないオブジェクトモデルを走査するためのさまざまな規則を提供します。 「 [PROPERTYPATH XAML 構文](propertypath-xaml-syntax.md)」を参照してください。  
  
- <xref:System.Windows.Data.Binding.RelativeSource%2A>: 相互に排他的な <xref:System.Windows.Data.Binding.ElementName%2A> と <xref:System.Windows.Data.Binding.Source%2A>これらの各バインドプロパティは、特定のバインディング方法を表します。 「[データバインディングの概要](../data/data-binding-overview.md)」を参照してください。 値を指定するには、入れ子になった[RelativeSource MarkupExtension](relativesource-markupextension.md)の使用が必要です。  
  
- <xref:System.Windows.Data.Binding.Source%2A>: 相互に排他的な and <xref:System.Windows.Data.Binding.RelativeSource%2A> と <xref:System.Windows.Data.Binding.ElementName%2A>;これらの各バインドプロパティは、特定のバインディング方法を表します。 「[データバインディングの概要](../data/data-binding-overview.md)」を参照してください。 入れ子になった拡張機能の使用が必要です。通常は、キー付きリソースディクショナリからオブジェクトデータソースを参照する[StaticResource マークアップ拡張機能](staticresource-markup-extension.md)です。  
  
- <xref:System.Windows.Data.BindingBase.StringFormat%2A>: バインドされたデータの文字列形式規則を記述する文字列。 これは、比較的高度なバインド概念です。<xref:System.Windows.Data.BindingBase.StringFormat%2A>のリファレンスページを参照してください。  
  
- <xref:System.Windows.Data.BindingBase.TargetNullValue%2A>: 式で `value` 文字列 =`bindProp`として設定できますが、これは渡されるパラメーターの型によって異なります。 値の参照型を渡す場合、には、入れ子になった[StaticResource マークアップ拡張機能](staticresource-markup-extension.md)などのオブジェクト参照が必要です。  
  
- <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A>:*値*は、<xref:System.Windows.Data.UpdateSourceTrigger> 列挙体の定数名です。 たとえば、`{Binding UpdateSourceTrigger=LostFocus}` のようにします。 特定のコントロールには、このバインディングプロパティの既定値が異なる可能性があります。 以下を参照してください。<xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A>  
  
- <xref:System.Windows.Data.Binding.ValidatesOnDataErrors%2A>: ブール値は、`true` または `false`のいずれかになります。 既定値は、 `false`です。 「解説」を参照してください。  
  
- <xref:System.Windows.Data.Binding.ValidatesOnExceptions%2A>: ブール値は、`true` または `false`のいずれかになります。 既定値は、 `false`です。 「解説」を参照してください。  
  
- <xref:System.Windows.Data.Binding.XPath%2A>: XML データソースの XMLDOM へのパスを記述する文字列。 「 [XMLDataProvider と XPath クエリを使用して XML データにバインドする](../data/how-to-bind-to-xml-data-using-an-xmldataprovider-and-xpath-queries.md)」を参照してください。  
  
 `Binding` マークアップ拡張機能/`{Binding}` 式フォームを使用して設定できない <xref:System.Windows.Data.Binding> のプロパティを次に示します。  
  
- <xref:System.Windows.Data.Binding.UpdateSourceExceptionFilter%2A>: このプロパティは、コールバックの実装への参照を必要とします。 イベントハンドラー以外のコールバック/メソッドは、XAML 構文で参照できません。  
  
- <xref:System.Windows.Data.Binding.ValidationRules%2A>: プロパティは、<xref:System.Windows.Controls.ValidationRule> オブジェクトのジェネリックコレクションを受け取ります。 これは、<xref:System.Windows.Data.Binding> オブジェクト要素のプロパティ要素として表すことができますが、`Binding` 式で使用するために使用できる属性解析手法はありません。 <xref:System.Windows.Data.Binding.ValidationRules%2A>については、リファレンストピックを参照してください。  
  
- <xref:System.Windows.Data.Binding.XmlNamespaceManager%2A>  
  
## <a name="remarks"></a>Remarks  
  
> [!IMPORTANT]
> 依存関係プロパティの優先順位に関しては、`Binding` 式はローカルに設定された値と同じです。 以前に `Binding` 式を持つプロパティのローカル値を設定した場合、`Binding` は完全に削除されます。 詳細については、「[依存関係プロパティ値の優先順位](dependency-property-value-precedence.md)」を参照してください。  
  
 基本レベルでのデータバインディングの記述については、このトピックでは説明しません。 「[データバインディングの概要](../data/data-binding-overview.md)」を参照してください。  
  
> [!NOTE]
> <xref:System.Windows.Data.MultiBinding> および <xref:System.Windows.Data.PriorityBinding> では、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] の拡張構文はサポートされていません。 代わりに、プロパティ要素を使用します。 <xref:System.Windows.Data.MultiBinding> と <xref:System.Windows.Data.PriorityBinding>については、「参照トピック」を参照してください。  
  
 XAML のブール値では大文字と小文字が区別されません。 たとえば、`{Binding NotifyOnValidationError=true}` または `{Binding NotifyOnValidationError=True}`のいずれかを指定できます。  
  
 データの検証を伴うバインドは、通常、`{Binding ...}` 式としてではなく、明示的な `Binding` 要素によって指定されます。また、式に <xref:System.Windows.Data.Binding.ValidatesOnDataErrors%2A> または <xref:System.Windows.Data.Binding.ValidatesOnExceptions%2A> を設定することは珍しくありません。 これは、コンパニオンプロパティ <xref:System.Windows.Data.Binding.ValidationRules%2A> を式形式で簡単に設定できないためです。 詳細については、「[バインディングの検証の実装](../data/how-to-implement-binding-validation.md)」を参照してください。  
  
 `Binding` はマークアップ拡張機能です。 マークアップ拡張機能は、通常、属性値をリテラル値またはハンドラー名以外にエスケープする必要があり、その要件が特定の型またはプロパティに属性が設定された型コンバーターよりもグローバルである場合に実装されます。 XAML のすべてのマークアップ拡張機能は、属性構文で `{` と `}` 文字を使用します。これは、マークアップ拡張機能が文字列の内容を処理する必要があることを XAML プロセッサが認識する規則です。 詳細については、「[マークアップ拡張機能」および「WPF XAML](markup-extensions-and-wpf-xaml.md)」を参照してください。  
  
 `Binding` は、WPF の XAML 実装の拡張機能を実装する <xref:System.Windows.Data.Binding> クラスが XAML に関連しない他のいくつかのメソッドとプロパティも実装するという、例外的なマークアップ拡張機能です。 他のメンバーは、XAML マークアップ拡張機能として機能するだけでなく、多くのデータバインディングのシナリオに対応できる、より汎用性のある自己完結型のクラスを <xref:System.Windows.Data.Binding> することを目的としています。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Data.Binding>
- [データ バインディングの概要](../data/data-binding-overview.md)
- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
- [マークアップ拡張機能と WPF XAML](markup-extensions-and-wpf-xaml.md)
