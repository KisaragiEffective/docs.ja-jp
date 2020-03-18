---
title: DynamicResource のマークアップ拡張機能
ms.date: 03/30/2017
f1_keywords:
- DynamicResource
- DynamicResourceExtension
helpviewer_keywords:
- XAML [WPF], DynamicResource markup extension
- DynamicResource markup extensions [WPF]
ms.assetid: 7324f243-03af-4c2b-b0db-26ac6cdfcbe4
ms.openlocfilehash: f8b05f314be84e6104f1a9c7fe2edfdf826e51da
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559449"
---
# <a name="dynamicresource-markup-extension"></a>DynamicResource のマークアップ拡張機能
定義されたリソースへの参照としてその値を延期することによって、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] property 属性の値を提供します。 このリソースの参照動作は、ランタイム参照に似ています。  
  
## <a name="xaml-attribute-usage"></a>XAML 属性の使用  
  
```xml  
<object property="{DynamicResource key}" .../>  
```  
  
## <a name="xaml-property-element-usage"></a>XAML プロパティ要素の使用  
  
```xml  
<object>  
  <object.property>  
    <DynamicResource ResourceKey="key" .../>  
  </object.property>  
</object>  
```  
  
## <a name="xaml-values"></a>XAML の値  
  
|||  
|-|-|  
|`key`|要求されたリソースのキー。 このキーは、リソースがマークアップで作成された場合は[X:Key ディレクティブ](../../../desktop-wpf/xaml-services/xkey-directive.md)によって最初に割り当てられたか、またはリソースがコードで作成された場合に <xref:System.Windows.ResourceDictionary.Add%2A?displayProperty=nameWithType> を呼び出すときに `key` パラメーターとして指定されました。|  
  
## <a name="remarks"></a>コメント  
 `DynamicResource` は、初期コンパイル中に一時式を作成するため、オブジェクトを構築するために要求されたリソース値が実際に必要になるまで、リソースの参照を延期します。 これは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページが読み込まれた後に発生する可能性があります。 リソース値は、現在のページスコープから始まるすべてのアクティブなリソースディクショナリに対するキー検索に基づいて検出され、コンパイルからのプレースホルダー式の代わりに使用されます。  
  
> [!IMPORTANT]
> 依存関係プロパティの優先順位に関しては、`DynamicResource` 式は動的リソース参照が適用される位置と同じです。 以前にローカル値として `DynamicResource` 式を持つプロパティのローカル値を設定した場合、`DynamicResource` は完全に削除されます。 詳細については、「[依存関係プロパティ値の優先順位](dependency-property-value-precedence.md)」を参照してください。  
  
 特定のリソースアクセスシナリオは、 [StaticResource のマークアップ拡張機能](staticresource-markup-extension.md)ではなく `DynamicResource` に特に適しています。 `DynamicResource`との相対的な利点とパフォーマンスへの影響については、「[XAML リソース](xaml-resources.md)」を参照して`StaticResource`ください。  
  
 指定された <xref:System.Windows.DynamicResourceExtension.ResourceKey%2A> は、ページ、アプリケーション、使用可能なコントロールのテーマと外部リソース、またはシステムリソースについて、 [X:Key ディレクティブ](../../../desktop-wpf/xaml-services/xkey-directive.md)によって決定された既存のリソースに対応する必要があります。 静的リソースと動的リソースのリソース検索の詳細については、「 [XAML リソース](xaml-resources.md)」を参照してください。  
  
 リソースキーは、 [XamlName 文法](../../../desktop-wpf/xaml-services/xamlname-grammar.md)で定義されている任意の文字列にすることができます。 リソースキーは、<xref:System.Type>など、他の種類のオブジェクトである場合もあります。 <xref:System.Type> キーは、テーマによってコントロールをスタイル設定する方法の基本となります。 詳細については、「[コントロールの作成の概要](../controls/control-authoring-overview.md)」を参照してください。  
  
 <xref:System.Windows.FrameworkElement.FindResource%2A>などのリソース値を参照するための Api は、`DynamicResource`によって使用されるのと同じリソース参照ロジックに従います。  
  
 リソースを参照する別の宣言的な方法は、 [StaticResource マークアップ拡張機能](staticresource-markup-extension.md)です。  
  
 属性構文は、このマークアップ拡張機能で使用される最も一般的な構文です。 `DynamicResource` 識別子文字列の後に設定される文字列トークンは、基になる <xref:System.Windows.DynamicResourceExtension.ResourceKey%2A> 拡張クラスの <xref:System.Windows.DynamicResourceExtension> 値として割り当てられます。  
  
 `DynamicResource` は、オブジェクト要素構文で使用できます。 この場合は、<xref:System.Windows.DynamicResourceExtension.ResourceKey%2A> プロパティの値を指定する必要があります。  
  
 `DynamicResource` は、<xref:System.Windows.DynamicResourceExtension.ResourceKey%2A> プロパティをプロパティおよび値のペアとして指定する詳細出力属性使用でも使用できます。  
  
```xml  
<object property="{DynamicResource ResourceKey=key}" .../>  
```  
  
 詳細出力の使用は、複数の設定可能プロパティを持つ拡張機能や、一部のプロパティがオプションである場合に役立ちます。 `DynamicResource` には、必須の設定可能プロパティが 1 つしか存在しないため、このような詳細出力の使用は一般的ではありません。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサの実装では、このマークアップ拡張機能の処理は <xref:System.Windows.DynamicResourceExtension> クラスによって定義されます。  
  
 `DynamicResource` はマークアップ拡張機能です。 一般にマークアップ拡張機能を実装するのは、属性値をリテラル値やハンドラー名以外にエスケープする要件が存在し、その要件の適用範囲がグローバルで、特定の型やプロパティに型コンバーターを適用するだけにとどまらない場合です。 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 内のすべてのマークアップ拡張機能は、属性構文で {および} 文字を使用します。これは、マークアップ拡張機能が属性を処理する必要があることを [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサが認識する規則です。 詳細については、「[マークアップ拡張機能」および「WPF XAML](markup-extensions-and-wpf-xaml.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [XAML リソース](xaml-resources.md)
- [リソースとコード](resources-and-code.md)
- [x:Key ディレクティブ](../../../desktop-wpf/xaml-services/xkey-directive.md)
- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
- [マークアップ拡張機能と WPF XAML](markup-extensions-and-wpf-xaml.md)
- [StaticResource のマークアップ拡張機能](staticresource-markup-extension.md)
- [マークアップ拡張機能と WPF XAML](markup-extensions-and-wpf-xaml.md)
