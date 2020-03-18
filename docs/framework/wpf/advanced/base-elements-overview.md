---
title: 基本要素の概要
ms.date: 03/30/2017
helpviewer_keywords:
- base elements [WPF]
ms.assetid: 2c997092-72c6-4767-bc84-74267f4eee72
ms.openlocfilehash: 823c81bf6b21b88d719503387a68ce6e7d643d61
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73740915"
---
# <a name="base-elements-overview"></a>基本要素の概要
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] のクラスの大部分は、SDK ドキュメントで基本要素クラスとして一般に参照される4つのクラスから派生します。 これらのクラスは、<xref:System.Windows.UIElement>、<xref:System.Windows.FrameworkElement>、<xref:System.Windows.ContentElement>、および <xref:System.Windows.FrameworkContentElement>です。 <xref:System.Windows.DependencyObject> クラスは、<xref:System.Windows.UIElement> との両方の共通基本クラスであるため、関連もあり <xref:System.Windows.ContentElement>  

<a name="base_apis"></a>   
## <a name="base-element-apis-in-wpf-classes"></a>WPF クラスの基本要素 API  
 <xref:System.Windows.UIElement> と <xref:System.Windows.ContentElement> はどちらも、多少異なる経路を通じて <xref:System.Windows.DependencyObject>から派生します。 このレベルでの分割は、ユーザーインターフェイスでの <xref:System.Windows.UIElement> または <xref:System.Windows.ContentElement> の使用方法と、アプリケーションでどのように機能するかについて扱います。 <xref:System.Windows.UIElement> は、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]の基になる下位レベルのグラフィックスサポートを公開するクラスであるクラス階層にも <xref:System.Windows.Media.Visual> ます。 <xref:System.Windows.Media.Visual> は、独立した四角形の画面領域を定義することで、レンダリングフレームワークを提供します。 実際には、大規模なオブジェクトモデルをサポートする要素に対して <xref:System.Windows.UIElement> が使用されます。これは、四角形の画面領域として記述できる領域と、コンテンツモデルが意図的に開いていて、さまざまな組み合わせを可能にする領域へのレンダリングとレイアウトを意図しています。要素の。 <xref:System.Windows.ContentElement> は <xref:System.Windows.Media.Visual>から派生していません。そのモデルとして、<xref:System.Windows.ContentElement> は、要素を解釈し、使用する [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] の完全な <xref:System.Windows.Media.Visual> を生成するリーダーやビューアーなど、他のものによって使用されます。 特定の <xref:System.Windows.UIElement> クラスは、コンテンツホストとして使用することを意図しています。1つ以上の <xref:System.Windows.ContentElement> クラス (<xref:System.Windows.Controls.DocumentViewer> は、このようなクラスの例) に対してホストとレンダリングを提供します。 <xref:System.Windows.ContentElement> は、少し小さいオブジェクトモデルを持つ要素の基本クラスとして使用され、<xref:System.Windows.UIElement>内でホストされる可能性のあるテキスト、情報、またはドキュメントのコンテンツをより多く使用します。  
  
### <a name="framework-level-and-core-level"></a>フレームワークレベルとコアレベル  
 <xref:System.Windows.UIElement> は <xref:System.Windows.FrameworkElement>の基本クラスとして機能し、<xref:System.Windows.ContentElement> は <xref:System.Windows.FrameworkContentElement>の基本クラスとして機能します。 この次のレベルのクラスの目的は、wpf フレームワークレベルとは別の WPF コアレベルをサポートすることです。この区分は、プレゼンテーションコアおよびプレゼンテーションフレームワークアセンブリ間での Api の分割方法にも存在します。 WPF フレームワーク レベルは、表示のためのレイアウト マネージャーの実装など、アプリケーションの基本的ニーズに対して、より完全なソリューションを提供します。 WPF コア レベルは、アセンブリの追加によるオーバーヘッドを引き起こすことなく [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の大部分を使用する方法を提供します。 これらのレベルを区別することは、ほとんどの一般的なアプリケーション開発シナリオでは非常に重要ではありません。一般に、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] Api は全体と考える必要があり、WPF フレームワークレベルと WPF コアレベルの違いについては考慮する必要はありません。 アプリケーション設計で WPF フレームワーク レベルの機能の大部分を置き換えることにした場合、たとえば開発中のソリューション全体に、独自の[!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] の構成およびレイアウトが既に実装されている場合には、これらのレベルの区分に関する理解が必要となる可能性があります。  
  
<a name="subclassing_elements"></a>   
## <a name="choosing-which-element-to-derive-from"></a>派生元の要素を選択する  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] を拡張したカスタム クラスを作成するための最も実際的な方法は、既存のクラス階層から必要な機能を可能な限り得ることのできる、いずれかの [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] クラスから派生させることです。 このセクションでは、継承元とするクラスの選択に役立つように、最も重要な要素クラスのうちの 3 つのクラスが提供する機能を示します。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] クラスから派生する一般的な理由の1つであるコントロールを実装する場合は、実際のコントロール、コントロールファミリの基本クラス、または少なくとも <xref:System.Windows.Controls.Control> の基本クラスから派生させることをお勧めします。 いくつかのガイドラインと実際の例については、「[コントロールの作成の概要](../controls/control-authoring-overview.md)」を参照してください。  
  
 コントロールを作成しているのではなく、階層の上位にあるクラスから派生する必要がある場合は、各基本要素クラスに定義された特性に関するガイドを意図した、以下のセクションを参照してください。  
  
 <xref:System.Windows.DependencyObject>から派生するクラスを作成する場合は、次の機能を継承します。  
  
- <xref:System.Windows.DependencyObject.GetValue%2A> と <xref:System.Windows.DependencyObject.SetValue%2A> のサポート、および一般的なプロパティシステムのサポート。  
  
- 依存関係プロパティと、依存関係プロパティとして実装されている添付プロパティを使用する機能。  
  
 <xref:System.Windows.UIElement>から派生するクラスを作成する場合は、<xref:System.Windows.DependencyObject>によって提供される機能に加えて、次の機能を継承します。  
  
- アニメーション化されたプロパティ値の基本的なサポート。 詳しくは、「 [アニメーションの概要](../graphics-multimedia/animation-overview.md)」をご覧ください。  
  
- 基本的な入力イベントのサポートと、コマンド実行のサポート。 詳細については、「[入力の概要](input-overview.md)」および「[コマンド実行の概要](commanding-overview.md)」を参照してください。  
  
- レイアウト システムに情報を提供するためにオーバーライドできる仮想メソッド。  
  
 <xref:System.Windows.FrameworkElement>から派生するクラスを作成する場合は、<xref:System.Windows.UIElement>によって提供される機能に加えて、次の機能を継承します。  
  
- スタイル設定とストーリーボードのサポート。 詳細については、「<xref:System.Windows.Style> と[ストーリーボードの概要](../graphics-multimedia/storyboards-overview.md)」を参照してください。  
  
- データ バインディングのサポート。 詳しくは、「[データ バインディングの概要](../data/data-binding-overview.md)」をご覧ください。  
  
- 動的リソース参照のサポート。 詳細については、「[XAML リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)」を参照してください。  
  
- プロパティ値継承のサポート、および、データ バインディング、スタイル、またはレイアウトのフレームワークの実装などのフレームワーク サービスのプロパティに関する条件をレポートする場合に役立つ、メタデータ内の他のフラグ。 詳細については、「[フレームワーク プロパティ メタデータ](framework-property-metadata.md)」を参照してください。  
  
- 論理ツリーの概念。 詳細については、「[WPF のツリー](trees-in-wpf.md)」を参照してください。  
  
- レイアウトシステムの実際の WPF フレームワークレベルの実装をサポートします。これには、レイアウトに影響を与えるプロパティの変更を検出できる <xref:System.Windows.FrameworkElement.OnPropertyChanged%2A> オーバーライドも含まれます。  
  
 <xref:System.Windows.ContentElement>から派生するクラスを作成する場合は、<xref:System.Windows.DependencyObject>によって提供される機能に加えて、次の機能を継承します。  
  
- アニメーションのサポート。 詳しくは、「 [アニメーションの概要](../graphics-multimedia/animation-overview.md)」をご覧ください。  
  
- 基本的な入力イベントのサポートと、コマンド実行のサポート。 詳細については、「[入力の概要](input-overview.md)」および「[コマンド実行の概要](commanding-overview.md)」を参照してください。  
  
 <xref:System.Windows.FrameworkContentElement>から派生するクラスを作成すると、<xref:System.Windows.ContentElement>によって提供される機能に加えて、次の機能を利用できます。  
  
- スタイル設定とストーリーボードのサポート。 詳細については、「<xref:System.Windows.Style> と[アニメーションの概要](../graphics-multimedia/animation-overview.md)」を参照してください。  
  
- データ バインディングのサポート。 詳しくは、「[データ バインディングの概要](../data/data-binding-overview.md)」をご覧ください。  
  
- 動的リソース参照のサポート。 詳細については、「[XAML リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)」を参照してください。  
  
- プロパティ値継承のサポート、および、データ バインディング、スタイル、またはレイアウトのフレームワークの実装などのフレームワーク サービスのプロパティに関する条件をレポートする場合に役立つ、メタデータ内の他のフラグ。 詳細については、「[フレームワーク プロパティ メタデータ](framework-property-metadata.md)」を参照してください。  
  
- レイアウトシステムの変更 (<xref:System.Windows.FrameworkElement.ArrangeOverride%2A>など) へのアクセスは継承されません。 レイアウトシステムの実装は、<xref:System.Windows.FrameworkElement>でのみ使用できます。 ただし、レイアウトに影響を与えるプロパティの変更を検出し、任意のコンテンツホストにレポートすることができるオーバーライド <xref:System.Windows.FrameworkElement.OnPropertyChanged%2A> を継承します。  
  
 さまざまなクラスに関してコンテンツ モデルがドキュメント化されています。 派生元として適切なクラスを見つける必要がある場合、クラスのコンテンツ モデルは、考慮する必要がある項目の候補の 1 つです。 詳細については、「[WPF のコンテンツ モデル](../controls/wpf-content-model.md)」を参照してください。  
  
<a name="other_base_classes"></a>   
## <a name="other-base-classes"></a>他の基本クラス  
  
### <a name="dispatcherobject"></a>DispatcherObject  
 <xref:System.Windows.Threading.DispatcherObject> では、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] スレッドモデルがサポートされており、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーション用に作成されたすべてのオブジェクトを <xref:System.Windows.Threading.Dispatcher>に関連付けることができます。 <xref:System.Windows.UIElement>、<xref:System.Windows.DependencyObject>、または <xref:System.Windows.Media.Visual>から派生していない場合でも、このスレッドモデルのサポートを取得するために、<xref:System.Windows.Threading.DispatcherObject> からの派生を検討する必要があります。 詳細については、「[スレッド モデル](threading-model.md)」を参照してください。  
  
### <a name="visual"></a>ビジュアル  
 <xref:System.Windows.Media.Visual> は、通常はほぼ四角形の領域で視覚的なプレゼンテーションを必要とする2D オブジェクトの概念を実装します。 <xref:System.Windows.Media.Visual> の実際のレンダリングは、他のクラス (自己完結型ではありません) で行われますが、<xref:System.Windows.Media.Visual> クラスは、さまざまなレベルでレンダリングプロセスによって使用される既知の型を提供します。 <xref:System.Windows.Media.Visual> はヒットテストを実装しますが、ヒットテストの結果を報告するイベントを公開しません (これらは <xref:System.Windows.UIElement>にあります)。 詳細については、「[ビジュアル層のプログラミング](../graphics-multimedia/visual-layer-programming.md)」を参照してください。  
  
### <a name="freezable"></a>Freezable  
 <xref:System.Windows.Freezable> は、変更できないオブジェクトが必要な場合や、パフォーマンス上の理由から必要な場合に、オブジェクトのコピーを生成する手段を提供することによって、変更可能なオブジェクトの不変性をシミュレートします。 <xref:System.Windows.Freezable> 型は、ジオメトリやブラシなどの特定のグラフィックス要素およびアニメーションに共通の基礎を提供します。 特に、<xref:System.Windows.Freezable> は <xref:System.Windows.Media.Visual>ではありません。別のオブジェクトのプロパティ値を設定するために <xref:System.Windows.Freezable> が適用されたときにサブプロパティになるプロパティを保持し、それらのサブプロパティがレンダリングに影響を与える可能性があります。 詳細については、「[Freezable オブジェクトの概要](freezable-objects-overview.md)」を参照してください。  
  
 <xref:System.Windows.Media.Animation.Animatable>  
  
 <xref:System.Windows.Media.Animation.Animatable> は、アニメーションコントロールレイヤーと一部のユーティリティメンバーを明示的に追加する <xref:System.Windows.Freezable> 派生クラスであり、アニメーション化されていないプロパティを現在アニメーション化されたプロパティと区別できるようにします。  
  
### <a name="control"></a>Control  
 <xref:System.Windows.Controls.Control> は、テクノロジに応じて、コントロールまたはコンポーネントと呼ばれるオブジェクトの種類に適した基底クラスです。 一般に [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロール クラスは、UI コントロールを直接表すクラスか、コントロールの複合に密接に参加するクラスです。 <xref:System.Windows.Controls.Control> が有効にする主な機能は、コントロールテンプレートです。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Controls.Control>
- [依存関係プロパティの概要](dependency-properties-overview.md)
- [コントロールの作成の概要](../controls/control-authoring-overview.md)
- [WPF アーキテクチャ](wpf-architecture.md)
