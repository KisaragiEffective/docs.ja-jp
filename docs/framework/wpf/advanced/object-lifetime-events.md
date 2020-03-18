---
title: オブジェクトの有効期間イベント
ms.date: 03/30/2017
helpviewer_keywords:
- events [WPF], ContentRendered
- events [WPF], Deactivated
- events [WPF], Unloaded
- Activated events [WPF]
- events [WPF], Loaded
- Application objects [WPF], lifetime events
- events [WPF], Activated
- ContentRendered events [WPF]
- Deactivated events [WPF]
- events [WPF], Initialized
- events [WPF], Closing
- Unloaded events [WPF]
- exit events [WPF]
- objects' lifetime events [WPF]
- Loaded events [WPF]
- Closing events [WPF]
- events [WPF], Closed
- Initialized events [WPF]
- Closed events [WPF]
- startup events [WPF]
- lifetime events of objects [WPF]
ms.assetid: face6fc7-465b-4502-bfe5-e88d2e729a78
ms.openlocfilehash: c0858a0194bc0e9efa60a42d4029bdba9f4f3fef
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458578"
---
# <a name="object-lifetime-events"></a>オブジェクトの有効期間イベント
このトピックでは、オブジェクトの有効期間における作成、使用、破棄のステージを示す特定の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] イベントについて説明します。  

<a name="prerequisites"></a>   
## <a name="prerequisites"></a>必要条件  
 このトピックは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] クラスの既存の依存関係プロパティのコンシューマーの観点から依存関係プロパティを理解しており、「[依存関係プロパティの概要](dependency-properties-overview.md)」というトピックを読んでいることを前提としています。 このトピックの例を理解するには、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] について理解し (「[XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)」を参照)、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションの記述方法を理解していることも必要です。  
  
<a name="intro"></a>   
## <a name="object-lifetime-events"></a>オブジェクトの有効期間イベント  
 Microsoft .NET Framework マネージコード内のすべてのオブジェクトは、有効期間、作成、使用、および破棄の同様のステージのセットを処理します。 また、多くのオブジェクトには、破棄フェーズの一環として発生する有効期間の終了ステージもあります。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] オブジェクト (具体的には [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] が要素として識別するビジュアル オブジェクト) にも、共通する一連のオブジェクト有効期間ステージがあります。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プログラミングおよびアプリケーション モデルでは、これらのステージが一連のイベントとして公開されます。 有効期間イベントに関しては、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] に、全般的要素、ウィンドウ要素、ナビゲーション ホスト、アプリケーション オブジェクトの 4 種類の主要オブジェクトがあります。 ウィンドウ要素とナビゲーション ホストは、さらに大きなビジュアル オブジェクト (要素) のグループにも含まれます。 このトピックでは、すべての要素に共通する有効期間イベントについて説明し、次にアプリケーション定義、ウィンドウ要素、またはナビゲーション ホストに適用される特定の有効期間イベントについて説明します。  
  
<a name="common_events"></a>   
## <a name="common-lifetime-events-for-elements"></a>要素に関する共通の有効期間イベント  
 WPF フレームワークレベルの要素 (<xref:System.Windows.FrameworkElement> または <xref:System.Windows.FrameworkContentElement>から派生したオブジェクト) には、<xref:System.Windows.FrameworkElement.Initialized>、<xref:System.Windows.FrameworkElement.Loaded>、<xref:System.Windows.FrameworkElement.Unloaded>という3つの共通の有効期間イベントがあります。  
  
### <a name="initialized"></a>初期化済み  
 最初に <xref:System.Windows.FrameworkElement.Initialized> が発生し、そのコンストラクターの呼び出しによるオブジェクトの初期化にほぼ対応します。 このイベントは初期化に応答して発生するため、オブジェクトのすべてのプロパティが設定されることが保証されます。 (例外は動的リソースやバインディングなどの式の使用であり、未評価の式になります)。すべてのプロパティが設定されていることを前提として、マークアップで定義されている入れ子になった要素によって発生する <xref:System.Windows.FrameworkElement.Initialized> のシーケンスは、最初に要素ツリーの最上位要素の順に発生し、次にルートに向かって親要素が出現するように見えます。 この順序に従うのは、親子のリレーションシップおよびコンテインメントがプロパティであるためです。その結果、親のプロパティを設定する子要素も完全に初期化されるまで、親は初期化を報告できません。  
  
 <xref:System.Windows.FrameworkElement.Initialized> イベントへの応答としてハンドラーを記述する場合は、ハンドラーがアタッチされている要素ツリー (論理ツリーまたはビジュアルツリー) の他のすべての要素 (特に親) が作成されている保証はないことを考慮する必要があります。要素. メンバー変数が null である可能性や、基になるバインディングによってデータ ソースがまだ設定されていない可能性があります (式レベルでも)。  
  
### <a name="loaded"></a>読み込み  
 次に <xref:System.Windows.FrameworkElement.Loaded> が発生します。 <xref:System.Windows.FrameworkElement.Loaded> イベントは、最終的なレンダリングの前に発生しますが、レイアウトシステムがレンダリングに必要なすべての値を計算した後に発生します。 <xref:System.Windows.FrameworkElement.Loaded> には、要素が含まれている論理ツリーが完全であり、HWND とレンダリングサーフェイスを提供するプレゼンテーションソースに接続する必要があります。 標準データバインディング (他のプロパティや直接定義されたデータソースなどのローカルソースへのバインド) は、<xref:System.Windows.FrameworkElement.Loaded>する前に発生します。 非同期データ バインディング (外部ソースまたは動的ソース) が発生する可能性もありますが、非同期であるという性質の定義から、その保証はできません。  
  
 <xref:System.Windows.FrameworkElement.Loaded> イベントが発生するメカニズムは、<xref:System.Windows.FrameworkElement.Initialized>とは異なります。 <xref:System.Windows.FrameworkElement.Initialized> イベントは要素によって発生し、完了した要素ツリーによって直接調整されることはありません。 これに対して、<xref:System.Windows.FrameworkElement.Loaded> イベントは、要素ツリー全体 (具体的には論理ツリー) 全体で組織的な労力として発生します。 ツリー内のすべての要素が、読み込まれていると見なされる状態にある場合、最初にルート要素で <xref:System.Windows.FrameworkElement.Loaded> イベントが発生します。 その後、各子要素で <xref:System.Windows.FrameworkElement.Loaded> イベントが連続して発生します。  
  
> [!NOTE]
> この動作は、表面上は、ルーティング イベントのトンネリングに似ているとも言えます。 ただし、イベントからイベントへと伝達される情報はありません。 各要素には常に <xref:System.Windows.FrameworkElement.Loaded> イベントを処理する機会があり、イベントデータを処理済みとしてマークすると、その要素を超える影響はありません。  
  
### <a name="unloaded"></a>アンロードされました  
 <xref:System.Windows.FrameworkElement.Unloaded> が最後に発生し、プレゼンテーションソースまたは削除されるビジュアル親によって開始されます。 <xref:System.Windows.FrameworkElement.Unloaded> が発生して処理されると、イベントソースの親である要素 (<xref:System.Windows.FrameworkElement.Parent%2A> プロパティによって決定される)、または論理ツリーまたはビジュアルツリーで上位にある特定の要素は、既に設定されていない可能性があります。つまり、データバインディング、リソース参照、およびスタイルになります。は、通常のランタイム値または最後の既知の実行時の値に設定することはできません。  
  
<a name="application_model_elements"></a>   
## <a name="lifetime-events-application-model-elements"></a>有効期間イベント アプリケーション モデルの要素  
 要素の共通の有効期間イベントに基づく構築は、<xref:System.Windows.Application>、<xref:System.Windows.Window>、<xref:System.Windows.Controls.Page>、<xref:System.Windows.Navigation.NavigationWindow>、および <xref:System.Windows.Controls.Frame>のアプリケーションモデル要素です。 これらの要素は、共通の有効期間イベントを拡張して、特定の目的に関連するイベントを追加します。 これらの詳細については、次のトピックで説明しています。  
  
- <xref:System.Windows.Application>:[アプリケーション管理の概要](../app-development/application-management-overview.md)。  
  
- <xref:System.Windows.Window>: [WPF ウィンドウの概要](../app-development/wpf-windows-overview.md)。  
  
- <xref:System.Windows.Controls.Page>、<xref:System.Windows.Navigation.NavigationWindow>、および <xref:System.Windows.Controls.Frame>:[ナビゲーションの概要](../app-development/navigation-overview.md)。  
  
## <a name="see-also"></a>関連項目

- [依存関係プロパティ値の優先順位](dependency-property-value-precedence.md)
- [ルーティング イベントの概要](routed-events-overview.md)
