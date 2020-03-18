---
title: XAML 読み込みと依存関係プロパティ
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- custom dependency properties [WPF]
- dependency properties [WPF], XAML loading and
- loading XML data [WPF]
ms.assetid: 6eea9f4e-45ce-413b-a266-f08238737bf2
ms.openlocfilehash: 57c25297f995acd1ef307466258b5f4e03cc3098
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459534"
---
# <a name="xaml-loading-and-dependency-properties"></a>XAML 読み込みと依存関係プロパティ
[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサの現在の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 実装は、本質的に依存関係プロパティを認識しています。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサは、バイナリ [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] を読み込み、依存関係プロパティである属性を処理するときに、依存関係プロパティのプロパティシステムメソッドを使用します。 これにより、プロパティラッパーが効果的にバイパスされます。 カスタム依存関係プロパティを実装する場合は、この動作について考慮する必要があります。また、プロパティシステムメソッド <xref:System.Windows.DependencyObject.GetValue%2A> および <xref:System.Windows.DependencyObject.SetValue%2A>以外の他のコードをプロパティラッパーに配置しないようにする必要があります。  

<a name="prerequisites"></a>   
## <a name="prerequisites"></a>必要条件  
 このトピックでは、コンシューマーと作成者の両方の依存関係プロパティと、依存関係[プロパティの概要](dependency-properties-overview.md)と[カスタム依存関係](custom-dependency-properties.md)プロパティを理解していることを前提としています。 また、「Xaml の[概要 (WPF)」](../../../desktop-wpf/fundamentals/xaml.md)および「 [Xaml 構文の詳細」](xaml-syntax-in-detail.md)も参照してください。  
  
<a name="implementation"></a>   
## <a name="the-wpf-xaml-loader-implementation-and-performance"></a>WPF XAML ローダーの実装とパフォーマンス  
 実装上の理由から、プロパティを依存関係プロパティとして識別し、プロパティラッパーとその setter を使用するのではなく、プロパティを設定するためにプロパティシステム <xref:System.Windows.DependencyObject.SetValue%2A> メソッドにアクセスすると、計算コストが低くなります。 これは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] のプロセッサは、マークアップとさまざまな文字列の構造によって示される型およびメンバーのリレーションシップを知っているだけで、バッキングコードのオブジェクトモデル全体を推論する必要があるためです。  
  
 型は xmlns 属性とアセンブリ属性の組み合わせによって参照されますが、メンバーを識別し、属性としての設定がサポートされているかどうかを判断し、プロパティ値でサポートされる型を解決するために広範なリフレクションが必要になります。<xref:System.Reflection.PropertyInfo>を使用します。 特定の型の依存関係プロパティは、プロパティシステムを使用してストレージテーブルとしてアクセスできるため、その [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサの [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 実装では、このテーブルを使用して、特定のプロパティ*ABC*がによってより効率的に設定されることを推論します。依存関係プロパティ識別子*ABCProperty*を使用して、それを含む <xref:System.Windows.DependencyObject> 派生型に対して <xref:System.Windows.DependencyObject.SetValue%2A> を呼び出します。  
  
<a name="implications"></a>   
## <a name="implications-for-custom-dependency-properties"></a>カスタム依存関係プロパティの影響  
 プロパティ設定の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサ動作の現在の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 実装では、ラッパーが完全にバイパスされるため、カスタム依存関係プロパティのラッパーのセット定義に追加のロジックを含めないでください。 このようなロジックをセット定義に含めた場合、プロパティがコードではなく [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] で設定されていると、ロジックは実行されません。  
  
 同様に、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 処理からプロパティ値を取得する [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサのその他の側面は、ラッパーを使用するのではなく、<xref:System.Windows.DependencyObject.GetValue%2A> も使用します。 したがって、<xref:System.Windows.DependencyObject.GetValue%2A> 呼び出しを超える `get` 定義では、追加の実装も回避する必要があります。  
  
 次の例は、ラッパーを含む推奨される依存関係プロパティの定義です。ここで、プロパティ識別子は `readonly` フィールド `static` `public` として格納され、`get` 定義と `set` 定義には、必要なプロパティを超えるコードは含まれていません。依存関係プロパティのバッキングを定義するシステムメソッド。  
  
 [!code-csharp[WPFAquariumSln#AGWithWrapper](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFAquariumSln/CSharp/WPFAquariumObjects/Class1.cs#agwithwrapper)]
 [!code-vb[WPFAquariumSln#AGWithWrapper](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFAquariumSln/visualbasic/wpfaquariumobjects/class1.vb#agwithwrapper)]  
  
## <a name="see-also"></a>関連項目

- [依存関係プロパティの概要](dependency-properties-overview.md)
- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
- [依存関係プロパティのメタデータ](dependency-property-metadata.md)
- [コレクション型依存関係プロパティ](collection-type-dependency-properties.md)
- [依存関係プロパティのセキュリティ](dependency-property-security.md)
- [DependencyObject の安全なコンストラクター パターン](safe-constructor-patterns-for-dependencyobjects.md)
