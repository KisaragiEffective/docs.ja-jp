---
title: StaticResource のマークアップ拡張機能
ms.date: 03/30/2017
f1_keywords:
- StaticResource
- StaticResourceExtension
helpviewer_keywords:
- XAML [WPF], StaticResource markup extension
- StaticResource markup extensions [WPF]
ms.assetid: 97af044c-71f1-4617-9a94-9064b68185d2
ms.openlocfilehash: aa7f69e8871295006c3c5a9c7d0a70d0ecbd6d7e
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559821"
---
# <a name="staticresource-markup-extension"></a>StaticResource のマークアップ拡張機能
既に定義されているリソースへの参照を検索することによって、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] property 属性の値を提供します。 このリソースの参照動作は、読み込み時の参照に似ています。これにより、現在の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページのマークアップから以前に読み込まれたリソースと、そのリソース値がランタイムオブジェクトのプロパティ値として生成されます。  
  
## <a name="xaml-attribute-usage"></a>XAML 属性の使用  
  
```xml  
<object property="{StaticResource key}" .../>  
```  
  
## <a name="xaml-object-element-usage"></a>XAML オブジェクト要素の使用  
  
```xml  
<object>  
  <object.property>  
<StaticResource ResourceKey="key" .../>  
  </object.property>  
</object>  
```  
  
## <a name="xaml-values"></a>XAML の値  
  
|||  
|-|-|  
|`key`|要求されたリソースのキー。 このキーは、リソースがマークアップで作成された場合は[X:Key ディレクティブ](../../../desktop-wpf/xaml-services/xkey-directive.md)によって最初に割り当てられたか、またはリソースがコードで作成された場合に <xref:System.Windows.ResourceDictionary.Add%2A?displayProperty=nameWithType> を呼び出すときに `key` パラメーターとして指定されました。|  
  
## <a name="remarks"></a>コメント  
  
> [!IMPORTANT]
> `StaticResource` は、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイル内でさらに構文的に定義されているリソースへの前方参照を試行することはできません。 これを行おうとすると、サポートされていません。このような参照が失敗しない場合でも、前方参照を試行すると、<xref:System.Windows.ResourceDictionary> を表す内部ハッシュテーブルが検索されるときに、読み込み時間のパフォーマンスが低下します。 最良の結果を得るには、前方参照を回避できるように、リソースディクショナリの構成を調整します。 前方参照を回避できない場合は、代わりに[Dynamicresource マークアップ拡張機能](dynamicresource-markup-extension.md)を使用してください。  
  
 指定された <xref:System.Windows.StaticResourceExtension.ResourceKey%2A> は、既存のリソースに対応する必要があります。これは、ページ、アプリケーション、使用可能なコントロールテーマと外部リソース、またはシステムリソースの、 [X:Key ディレクティブ](../../../desktop-wpf/xaml-services/xkey-directive.md)で識別されます。 リソースルックアップは、この順序で実行されます。 静的リソースと動的リソースのリソース検索動作の詳細については、「 [XAML リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)」を参照してください。  
  
 リソースキーは、 [XamlName 文法](../../../desktop-wpf/xaml-services/xamlname-grammar.md)で定義されている任意の文字列にすることができます。 リソースキーは、<xref:System.Type>など、他の種類のオブジェクトにすることもできます。 <xref:System.Type> キーは、暗黙的なスタイルキーを使用して、テーマによってコントロールをスタイル設定する方法の基本となります。 詳細については、「[コントロールの作成の概要](../controls/control-authoring-overview.md)」を参照してください。  
  
 リソースを参照する別の宣言的な方法は、 [Dynamicresource マークアップ拡張機能](dynamicresource-markup-extension.md)です。  
  
 属性構文は、このマークアップ拡張機能で使用される最も一般的な構文です。 `StaticResource` 識別子文字列の後に設定される文字列トークンは、基になる <xref:System.Windows.StaticResourceExtension.ResourceKey%2A> 拡張クラスの <xref:System.Windows.StaticResourceExtension> 値として割り当てられます。  
  
 `StaticResource` は、オブジェクト要素構文で使用できます。 この場合は、<xref:System.Windows.StaticResourceExtension.ResourceKey%2A> プロパティの値を指定する必要があります。  
  
 `StaticResource` は、<xref:System.Windows.StaticResourceExtension.ResourceKey%2A> プロパティをプロパティおよび値のペアとして指定する詳細出力属性使用でも使用できます。  
  
```xml  
<object property="{StaticResource ResourceKey=key}" .../>  
```  
  
 詳細出力の使用は、複数の設定可能プロパティを持つ拡張機能や、一部のプロパティがオプションである場合に役立ちます。 `StaticResource` には、必須の設定可能プロパティが 1 つしか存在しないため、このような詳細出力の使用は一般的ではありません。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサの実装では、このマークアップ拡張機能の処理は <xref:System.Windows.StaticResourceExtension> クラスによって定義されます。  
  
 `StaticResource` はマークアップ拡張機能です。 一般にマークアップ拡張機能を実装するのは、属性値をリテラル値やハンドラー名以外にエスケープする要件が存在し、その要件の適用範囲がグローバルで、特定の型やプロパティに型コンバーターを適用するだけにとどまらない場合です。 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 内のすべてのマークアップ拡張機能は、属性構文で {および} 文字を使用します。これは、マークアップ拡張機能が属性を処理する必要があることを [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサが認識する規則です。 詳細については、「[マークアップ拡張機能」および「WPF XAML](markup-extensions-and-wpf-xaml.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [スタイルとテンプレート](../controls/styling-and-templating.md)
- [XAML の概要 (WPF)](xaml-overview-wpf.md)
- [マークアップ拡張機能と WPF XAML](markup-extensions-and-wpf-xaml.md)
- [XAML リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)
- [リソースとコード](resources-and-code.md)
