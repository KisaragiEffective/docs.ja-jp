---
title: TemplateBinding のマークアップ拡張機能
ms.date: 03/30/2017
f1_keywords:
- TemplateBinding
- TemplateBindingExtension
helpviewer_keywords:
- XAML [WPF], TemplateBinding markup extension
- TemplateBinding markup extensions [WPF]
ms.assetid: 1d25bbfc-dbc2-499d-9f12-419d23d4ac6a
ms.openlocfilehash: 6d89978b907c8f124b5162c97de5edc034cf1e95
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73976671"
---
# <a name="templatebinding-markup-extension"></a>TemplateBinding のマークアップ拡張機能
コントロール テンプレート内のプロパティの値を、template 宣言されたコントロールの別のプロパティの値にリンクします。  
  
## <a name="xaml-attribute-usage"></a>XAML 属性の使用方法  
  
```xml  
<object property="{TemplateBinding sourceProperty}" .../>  
```  
  
## <a name="xaml-attribute-usage-for-setter-property-in-template-or-style"></a>XAML 属性の使用方法 (テンプレートまたはスタイルの Setter プロパティの場合)  
  
```xml  
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>  
```  
  
## <a name="xaml-values"></a>XAML 値  
  
|||  
|-|-|  
|`propertyName`|Setter 構文で設定されるプロパティの <xref:System.Windows.DependencyProperty.Name%2A?displayProperty=nameWithType>。|  
|`sourceProperty`|template 宣言された型に存在する、<xref:System.Windows.DependencyProperty.Name%2A?displayProperty=nameWithType> によって指定された別の依存関係プロパティ。<br /><br /> または<br /><br /> template 宣言された対象の型とは異なる型で定義されている "ドットダウン" プロパティ名。 これは、実際には <xref:System.Windows.PropertyPath> です。 「 [PROPERTYPATH XAML 構文](propertypath-xaml-syntax.md)」を参照してください。|  
  
## <a name="remarks"></a>Remarks  
 `TemplateBinding` は、`{Binding RelativeSource={RelativeSource TemplatedParent}, Mode=OneWay}`で構築された `Binding` に似た、テンプレートシナリオ用の[バインド](binding-markup-extension.md)の最適化された形式です。 関連するプロパティが既定で双方向のバインディングの場合でも、`TemplateBinding` は常に一方向のバインディングです。 関連する両方のプロパティは、依存関係プロパティである必要があります。 テンプレート化された親への双方向のバインドを実現するには、代わりに次の binding ステートメントを使用して `{Binding RelativeSource={RelativeSource TemplatedParent}, Mode=TwoWay, Path=MyDependencyProperty}`ます。 
  
 [RelativeSource](relativesource-markupextension.md)は、テンプレート内で相対プロパティのバインドを実行するために、`TemplateBinding` ではなくまたはと組み合わせて使用されることがある、別のマークアップ拡張機能です。  
  
 コントロールテンプレートを概念として記述する方法については、ここでは説明しません。詳細については、「[コントロールのスタイルとテンプレート](../controls/control-styles-and-templates.md)」を参照してください。  
  
 属性構文は、このマークアップ拡張機能で使用される最も一般的な構文です。 `TemplateBinding` 識別子文字列の後に設定される文字列トークンは、基になる <xref:System.Windows.TemplateBindingExtension.Property%2A> 拡張クラスの <xref:System.Windows.TemplateBindingExtension> 値として割り当てられます。  
  
 オブジェクト要素構文を使用することもできますが、現実的な用途がないためここでは触れません。 `TemplateBinding` は、評価された式を使用して setter 内で値を埋め込むために使用され、`TemplateBinding` を使用する場合、TemplateBinding が `<Setter.Property>` プロパティ要素構文を埋め込むためのオブジェクト要素構文は、必要以上に詳細です。  
  
 `TemplateBinding` は、<xref:System.Windows.TemplateBindingExtension.Property%2A> プロパティをプロパティおよび値のペアとして指定する詳細出力属性使用でも使用できます。  
  
```xml  
<object property="{TemplateBinding Property=sourceProperty}" .../>  
```  
  
 詳細出力の使用は、複数の設定可能プロパティを持つ拡張機能や、一部のプロパティがオプションである場合に役立ちます。 `TemplateBinding` には、必須の設定可能プロパティが 1 つしか存在しないため、このような詳細出力の使用は一般的ではありません。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] XAML プロセッサの実装では、このマークアップ拡張機能の処理は <xref:System.Windows.TemplateBindingExtension> クラスによって定義されます。  
  
 `TemplateBinding` はマークアップ拡張機能です。 一般にマークアップ拡張機能を実装するのは、属性値をリテラル値やハンドラー名以外にエスケープする要件が存在し、その要件の適用範囲がグローバルで、特定の型やプロパティに型コンバーターを適用するだけにとどまらない場合です。 XAML のすべてのマークアップ拡張は、属性構文で `{` と `}` 文字を使用します。これは、マークアップ拡張機能が属性を処理する必要があることを XAML プロセッサが認識する規則です。 詳細については、「[マークアップ拡張機能」および「WPF XAML](markup-extensions-and-wpf-xaml.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Style>
- <xref:System.Windows.Controls.ControlTemplate>
- [スタイルとテンプレート](../controls/styling-and-templating.md)
- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
- [マークアップ拡張機能と WPF XAML](markup-extensions-and-wpf-xaml.md)
- [RelativeSource のマークアップ拡張機能](relativesource-markupextension.md)
- [バインドのマークアップ拡張機能](binding-markup-extension.md)
