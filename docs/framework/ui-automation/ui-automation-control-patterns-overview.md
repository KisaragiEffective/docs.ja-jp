---
title: UI オートメーション コントロール パターンの概要
ms.date: 03/30/2017
helpviewer_keywords:
- control patterns
- UI Automation, control patterns
ms.assetid: cc229b33-234b-469b-ad60-f0254f32d45d
ms.openlocfilehash: 252e6ccf6d2980610bb8879834db5bf9e3e5dd61
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74448845"
---
# <a name="ui-automation-control-patterns-overview"></a>UI オートメーション コントロール パターンの概要
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。  
  
 この概要では、 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] コントロール パターンについて説明します。 コントロール パターンは、コントロール型や外観に関係なく、コントロールの機能を分類したり公開したりするための手段です。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] は、コントロールの一般的な動作を表すコントロール パターンを使用します。 たとえば、呼び出し可能なコントロール (ボタンなど) には Invoke コントロール パターンを使用し、スクロール バーを持つコントロール (リスト ボックス、リスト ビュー、コンボ ボックスなど) には Scroll コントロール パターンを使用します。 コントロール パターンごとに別の機能を表すため、コントロール パターンを組み合わせて、特定のコントロールでサポートされる機能の完全なセットを表すことができます。  
  
> [!NOTE]
> 集約コントロール (親によって公開される機能のために [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] を提供する子コントロールで構築されるもの) は、各子コントロールに通常関連付けられるすべてのコントロール パターンを実装する必要があります。 一方、これらの同じコントロール パターンを子コントロールで実装する必要はありません。  
  
<a name="uiautomation_control_pattern_includes"></a>   
## <a name="ui-automation-control-pattern-components"></a>UI オートメーションのコントロール パターン コンポーネント  
 コントロール パターンは、コントロールで使用可能な機能の個々の部分を定義するために必要なメソッド、プロパティ、イベント、およびリレーションシップをサポートします。  
  
- UI オートメーション要素とその親、子、兄弟との間の関係によって、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリー内の要素の構造を記述します。  
  
- メソッドは、UI オートメーション クライアントがコントロールを操作できるようにします。  
  
- プロパティとイベントは、コントロール パターンの機能に関する情報だけでなく、コントロールの状態に関する情報も提供します。  
  
 コントロールパターンは、コンポーネントオブジェクトモデル (COM) オブジェクトに関連するインターフェイスとして [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] に関連します。 COM では、オブジェクトを照会して、サポートされているインターフェイスを確認し、それらのインターフェイスを使用して機能にアクセスできます。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]では、UI オートメーション クライアントは、サポートされるコントロール パターンをコントロールに対して確認し、サポートされているコントロール パターンによって公開されているプロパティ、メソッド、イベント、構造体を使用してコントロールとやり取りすることができます。 たとえば、複数行のエディット ボックスでは、UI オートメーション プロバイダーは <xref:System.Windows.Automation.Provider.IScrollProvider>を実装します。 クライアントは、 <xref:System.Windows.Automation.AutomationElement> が <xref:System.Windows.Automation.ScrollPattern> コントロール パターンをサポートしていることを認識すると、そのコントロール パターンによって公開されているプロパティ、メソッド、イベントを使用して、コントロールを操作したり、コントロールに関する情報にアクセスしたりできます。  
  
<a name="uiautomation_control_pattern_client_provider"></a>   
## <a name="ui-automation-providers-and-clients"></a>UI オートメーションのプロバイダーおよびクライアント  
 UI オートメーション プロバイダーは、コントロール パターンを実装して、コントロールによってサポートされる特定の機能の適切な動作を公開します。  
  
 UI オートメーション クライアントは、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] コントロール パターン クラスのメソッドとプロパティにアクセスし、それらを使用して [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]に関する情報を取得したり、 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]を操作したりします。 これらのコントロール パターン クラスは、 <xref:System.Windows.Automation> 名前空間 (たとえば、 <xref:System.Windows.Automation.InvokePattern> や <xref:System.Windows.Automation.SelectionPattern>) にあります。  
  
 クライアントは、<xref:System.Windows.Automation.AutomationElement> メソッド (<xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A?displayProperty=nameWithType> や <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A?displayProperty=nameWithType>など) または共通言語ランタイム (CLR) アクセサーを使用して、パターンの [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロパティにアクセスします。 各コントロールパターンクラスには、そのコントロールパターンを識別するフィールドメンバー (たとえば、<xref:System.Windows.Automation.InvokePattern.Pattern?displayProperty=nameWithType> や <xref:System.Windows.Automation.SelectionPattern.Pattern?displayProperty=nameWithType>) があり、<xref:System.Windows.Automation.AutomationElement.GetCachedPattern%2A> または <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A> にパラメーターとして渡すことによって、<xref:System.Windows.Automation.AutomationElement>のそのパターンを取得できます。  
  
<a name="uiautomation_control_patterns_dynamic"></a>   
## <a name="dynamic-control-patterns"></a>動的コントロール パターン  
 一部のコントロールでは、コントロール パターンの同じセットを必ずしもサポートしません。 コントロール パターンは、UI オートメーション クライアントから使用できる場合に、サポートされていると見なされます。 たとえば、複数行のエディット ボックスでは、表示可能領域に表示できる行以上のテキストが含まれている場合にのみ、垂直スクロールが有効にされます。 テキストが削除されスクロールする必要がなくなると、スクロールは無効になります。 この例では、ScrollPattern コントロール パターンは、コントロールの現在の状態 (エディット ボックスのテキストの量) に応じて動的にサポートされます。  
  
<a name="Control_Pattern_Classes_and_Interfaces"></a>   
## <a name="control-pattern-classes-and-interfaces"></a>コントロール パターン クラスとインターフェイス  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] コントロール パターンについて次の表で説明します。 またこの表には、コントロール パターンにアクセスするために UI オートメーション クライアントで使用するクラスと、それらを実装するために UI オートメーション プロバイダーで使用するインターフェイスも示します。  
  
|コントロール パターン クラス|プロバイダーのインターフェイス|説明|  
|---------------------------|------------------------|-----------------|  
|<xref:System.Windows.Automation.DockPattern>|<xref:System.Windows.Automation.Provider.IDockProvider>|ドッキング コンテナーにドッキングすることができるコントロールに使用されます。 たとえば、ツールバーやツール パレットです。|  
|<xref:System.Windows.Automation.ExpandCollapsePattern>|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider>|展開したり折りたたんだりできるコントロールに使用されます。 たとえば、 **[ファイル]** メニューなどアプリケーションのメニュー項目。|  
|<xref:System.Windows.Automation.GridPattern>|<xref:System.Windows.Automation.Provider.IGridProvider>|サイズ変更や指定したセルへの移動などグリッド機能をサポートするコントロールに使用されます。 たとえば、Windows エクスプローラーの大きいアイコンの表示や、Microsoft Word のヘッダーのない単純なテーブルなどです。|  
|<xref:System.Windows.Automation.GridItemPattern>|<xref:System.Windows.Automation.Provider.IGridItemProvider>|グリッド内にセルを持つコントロールに使用されます。 個々のセルは GridItem パターンをサポートしている必要があります。 たとえば、Microsoft Windows エクスプローラーの詳細ビューの各セルです。|  
|<xref:System.Windows.Automation.InvokePattern>|<xref:System.Windows.Automation.Provider.IInvokeProvider>|ボタンなど、呼び出すことができるコントロールに使用されます。|  
|<xref:System.Windows.Automation.MultipleViewPattern>|<xref:System.Windows.Automation.Provider.IMultipleViewProvider>|情報、データ、子の同じセットの複数の表現の間で切り替えることができるコントロールに使用されます。 たとえば、サムネイル、タイル、アイコン、リスト、詳細ビューでデータを使用できるリスト ビュー コントロール。|  
|<xref:System.Windows.Automation.RangeValuePattern>|<xref:System.Windows.Automation.Provider.IRangeValueProvider>|コントロールに適用できる値の範囲を持つコントロールに使用されます。 たとえば、年を含むスピン ボックス コントロールは 1900 から 2010 の範囲を持ち、月を表すスピン ボックス コントロールは 1 から 12 の範囲を持ちます。|  
|<xref:System.Windows.Automation.ScrollPattern>|<xref:System.Windows.Automation.Provider.IScrollProvider>|スクロールできるコントロールに使用されます。 たとえば、コントロールの表示可能領域に表示できる量より多くの情報があるとアクティブになるスクロール バーを持つコントロール。|  
|<xref:System.Windows.Automation.ScrollItemPattern>|<xref:System.Windows.Automation.Provider.IScrollItemProvider>|スクロールされるリスト内に個々の項目を持つコントロールに使用されます。 たとえば、コンボ ボックス コントロールなどスクロール リストに個々の項目を持つリスト コントロール。|  
|<xref:System.Windows.Automation.SelectionPattern>|<xref:System.Windows.Automation.Provider.ISelectionProvider>|選択コンテナー コントロールに使用されます。 たとえば、リスト ボックスやコンボ ボックス。|  
|<xref:System.Windows.Automation.SelectionItemPattern>|<xref:System.Windows.Automation.Provider.ISelectionItemProvider>|リスト ボックスやコンボ ボックスなどの選択コンテナー コントロールの個々の項目に使用されます。|  
|<xref:System.Windows.Automation.TablePattern>|<xref:System.Windows.Automation.Provider.ITableProvider>|グリッドとヘッダー情報を持つコントロールに使用されます。 たとえば、Microsoft Excel のワークシートです。|  
|<xref:System.Windows.Automation.TableItemPattern>|<xref:System.Windows.Automation.Provider.ITableItemProvider>|テーブル内の項目に使用されます。|  
|<xref:System.Windows.Automation.TextPattern>|<xref:System.Windows.Automation.Provider.ITextProvider>|テキストの情報を公開するエディット コントロールとドキュメントに使用されます。|  
|<xref:System.Windows.Automation.TogglePattern>|<xref:System.Windows.Automation.Provider.IToggleProvider>|状態を切り替えることができるコントロールに使用されます。 たとえば、チェック ボックスやチェック可能なメニュー項目。|  
|<xref:System.Windows.Automation.TransformPattern>|<xref:System.Windows.Automation.Provider.ITransformProvider>|サイズ変更、移動、または回転を行えるコントロールに使用されます。 Transform コントロール パターンの一般的な用途は、デザイナー、フォーム、グラフィカル エディター、および描画アプリケーションでの使用です。|  
|<xref:System.Windows.Automation.ValuePattern>|<xref:System.Windows.Automation.Provider.IValueProvider>|クライアントで、値の範囲をサポートしないコントロールで値を取得したり、設定したりできます。 たとえば、日時指定のピッカーなどがあります。|  
|<xref:System.Windows.Automation.WindowPattern>|<xref:System.Windows.Automation.Provider.IWindowProvider>|Windows 固有の情報を、Microsoft Windows オペレーティングシステムの基本的な概念として公開します。 ウィンドウであるコントロールの例としては、トップレベルアプリケーションウィンドウ (Microsoft Word、Microsoft Windows エクスプローラーなど)、マルチドキュメントインターフェイス (MDI) 子ウィンドウ、およびダイアログがあります。|  
  
## <a name="see-also"></a>参照

- [UI Automation Control Patterns for Clients](ui-automation-control-patterns-for-clients.md)
- [Control Pattern Mapping for UI Automation Clients](control-pattern-mapping-for-ui-automation-clients.md)
- [UI オートメーションの概要](ui-automation-overview.md)
- [UI Automation Properties for Clients](ui-automation-properties-for-clients.md)
- [UI Automation Events for Clients](ui-automation-events-for-clients.md)
