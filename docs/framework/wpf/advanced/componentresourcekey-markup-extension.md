---
title: ComponentResourceKey マークアップ拡張機能
ms.date: 03/30/2017
f1_keywords:
- ComponentResourceKey
- ComponentResourceKeyExtension
helpviewer_keywords:
- ComponentResourceKey markup extension [WPF]
- XAML [WPF], ComponentResourceKey markup extension
ms.assetid: d6bcdbe6-61b3-40a7-b381-4e02185b5a85
ms.openlocfilehash: 2ccc4f3154996a4e442a4092833f5c9ed9c8938a
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559462"
---
# <a name="componentresourcekey-markup-extension"></a>ComponentResourceKey マークアップ拡張機能
外部アセンブリから読み込まれるリソースのキーを定義して参照します。 これにより、リソース検索では、アセンブリ内の明示的なリソースディクショナリやクラスではなく、アセンブリ内の対象の型を指定できます。  
  
## <a name="xaml-attribute-usage-setting-key-compact"></a>XAML 属性の使用法 (キーの設定、コンパクト)  
  
```xml  
<object x:Key="{ComponentResourceKey {x:Type targetTypeName}, targetID}" .../>  
```  
  
## <a name="xaml-attribute-usage-setting-key-verbose"></a>XAML 属性の使用法 (キーの設定、詳細)  
  
```xml  
<object x:Key="{ComponentResourceKey TypeInTargetAssembly={x:Type targetTypeName}, ResourceID=targetID}" .../>  
```  
  
## <a name="xaml-attribute-usage-requesting-resource-compact"></a>XAML 属性の使用法 (要求元のリソース、コンパクト)  
  
```xml  
<object property="{DynamicResource {ComponentResourceKey {x:Type targetTypeName}, targetID}}" .../>  
```  
  
## <a name="xaml-attribute-usage-requesting-resource-verbose"></a>XAML 属性の使用法 (要求元のリソース、詳細)  
  
```xml  
<object property="{DynamicResource {ComponentResourceKey TypeInTargetAssembly={x:Type targetTypeName}, ResourceID=targetID}}" .../>  
```  
  
## <a name="xaml-values"></a>XAML の値  
  
|||  
|-|-|  
|`targetTypeName`|リソースアセンブリで定義されているパブリック共通言語ランタイム (CLR) 型の名前。|  
|`targetID`|リソースのキー。 リソースが検索されると、`targetID` はリソースの[X:Key ディレクティブ](../../../desktop-wpf/xaml-services/xkey-directive.md)に似ています。|  
  
## <a name="remarks"></a>コメント  
 上記の使用状況に示されているように、{`ComponentResourceKey`} マークアップ拡張機能の使用方法は次の2つの場所にあります。  
  
- コントロールの作成者によって提供される、テーマリソースディクショナリ内のキーの定義。  
  
- コントロールをテンプレートするときに、コントロールのテーマによって提供されるリソースからのプロパティ値を使用する場合に、アセンブリからテーマリソースにアクセスします。  
  
 テーマに由来するコンポーネントリソースを参照する場合は、通常、`{StaticResource}`ではなく `{DynamicResource}` を使用することをお勧めします。 これは、使用法で示されています。 テーマ自体はユーザーが変更できるため、`{DynamicResource}` をお勧めします。 テーマをサポートするために、コントロールの作成者の意図に最も近いコンポーネントリソースが必要な場合は、コンポーネントのリソース参照も動的にできるようにする必要があります。  
  
 <xref:System.Windows.ComponentResourceKey.TypeInTargetAssembly%2A> は、リソースが実際に定義されているターゲットアセンブリに存在する型を識別します。 `ComponentResourceKey` は、<xref:System.Windows.ComponentResourceKey.TypeInTargetAssembly%2A> が定義されている場所を正確に把握することとは別に定義および使用できますが、最終的には参照されたアセンブリを使用して型を解決する必要があります。  
  
 <xref:System.Windows.ComponentResourceKey> の一般的な使用方法は、クラスのメンバーとして公開されるキーを定義することです。 この使用法では、マークアップ拡張機能ではなく <xref:System.Windows.ComponentResourceKey> クラスコンストラクターを使用します。 詳細については、「[コントロールの作成の概要](../controls/control-authoring-overview.md)」の「<xref:System.Windows.ComponentResourceKey>」または「テーマリソースのキーの定義と参照」セクションを参照してください。  
  
 キーの確立とキー付きリソースの参照の両方について、属性構文は、通常、`ComponentResourceKey` マークアップ拡張機能で使用されます。  
  
 ここに示すコンパクトな構文は、マークアップ拡張機能の <xref:System.Windows.ComponentResourceKey.%23ctor%2A?displayProperty=nameWithType> コンストラクターシグネチャと位置指定パラメーターの使用に依存しています。 `targetTypeName` と `targetID` を指定する順序が重要です。 Verbose 構文は <xref:System.Windows.ComponentResourceKey.%23ctor%2A?displayProperty=nameWithType> パラメーターなしのコンストラクターに依存し、オブジェクト要素の真の属性構文に似た方法で <xref:System.Windows.ComponentResourceKey.TypeInTargetAssembly%2A> と <xref:System.Windows.ComponentResourceKey.ResourceId%2A> を設定します。 Verbose 構文では、プロパティが設定される順序は重要ではありません。 この2つの代替手段 (compact および verbose) の関係とメカニズムについては、「[マークアップ拡張機能と WPF XAML](markup-extensions-and-wpf-xaml.md)」で詳しく説明されています。  
  
 技術的には、`targetID` の値は任意のオブジェクトにすることができ、文字列である必要はありません。 ただし、WPF で最も一般的に使用されるのは、`targetID` 値を文字列であるフォームに揃え、そのような文字列を[XamlName 文法](../../../desktop-wpf/xaml-services/xamlname-grammar.md)で有効にすることです。  
  
 `ComponentResourceKey` は、オブジェクト要素構文で使用できます。 この場合、拡張機能を正しく初期化するには、<xref:System.Windows.ComponentResourceKey.TypeInTargetAssembly%2A> プロパティと <xref:System.Windows.ComponentResourceKey.ResourceId%2A> プロパティの両方の値を指定する必要があります。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] リーダーの実装では、このマークアップ拡張機能の処理は <xref:System.Windows.ComponentResourceKey> クラスによって定義されます。  
  
 `ComponentResourceKey` はマークアップ拡張機能です。 一般にマークアップ拡張機能を実装するのは、属性値をリテラル値やハンドラー名以外にエスケープする要件が存在し、その要件の適用範囲がグローバルで、特定の型やプロパティに型コンバーターを適用するだけにとどまらない場合です。 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 内のすべてのマークアップ拡張機能は、属性構文で {および} 文字を使用します。これは、マークアップ拡張機能が属性を処理する必要があることを [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサが認識する規則です。 詳細については、「[マークアップ拡張機能」および「WPF XAML](markup-extensions-and-wpf-xaml.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.ComponentResourceKey>
- <xref:System.Windows.Controls.ControlTemplate>
- [コントロールの作成の概要](../controls/control-authoring-overview.md)
- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
- [マークアップ拡張機能と WPF XAML](markup-extensions-and-wpf-xaml.md)
