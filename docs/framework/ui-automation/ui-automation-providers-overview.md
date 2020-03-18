---
title: UI オートメーション プロバイダーの概要
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, providers
- providers, UI Automation
ms.assetid: 859557b8-51e1-4d15-92e8-318d2dcdb2f7
ms.openlocfilehash: 98208f1e1fa1b540bf3880e33478854128505233
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76778783"
---
# <a name="ui-automation-providers-overview"></a>UI オートメーション プロバイダーの概要
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。  
  
 UI オートメーション プロバイダーを使用すれば、コントロールで UI オートメーション クライアント アプリケーションと通信することができます。 一般に、各コントロールまたは [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] 内のその他の要素はプロバイダーによって表現されます。 プロバイダーは、要素に関する情報を公開し、必要に応じて、クライアント アプリケーションがコントロールと対話できるようにするコントロール パターンを実装します。  
  
 通常、クライアント アプリケーションはプロバイダーと直接連動する必要はありません。 Win32、Windows フォーム、または [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] のフレームワークを使用するアプリケーションの標準コントロールのほとんどは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] システムに自動的に公開されます。 カスタム コントロールを実装するアプリケーションがそれらのコントロール用の [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロバイダーを実装することもできます。クライアント アプリケーションがそれらにアクセスするために特別な手順を実行する必要はありません。  
  
 このトピックでは、コントロール開発者が [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロバイダーを実装する方法の概要について説明します。特に、Windows フォームおよび Win32 ウィンドウのコントロールについて説明します。  
  
<a name="Types_of_Providers"></a>   
## <a name="types-of-providers"></a>プロバイダーの種類  
 UI オートメーション プロバイダーは、クライアント側プロバイダーとサーバー側プロバイダーの 2 つのカテゴリに分類されます。  
  
### <a name="client-side-providers"></a>クライアント側プロバイダー  
 クライアント側プロバイダーは、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]をサポートしていないか部分的にサポートしているアプリケーションと通信する UI オートメーション クライアントによって実装されます。 通常、クライアント側プロバイダーは、Windows メッセージを送受信することによって、プロセス境界を越えてサーバーと通信します。  
  
 Win32、Windows フォーム、または [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] アプリケーションのコントロール用の UI オートメーションプロバイダーはオペレーティングシステムの一部として提供されるため、クライアントアプリケーションは、ほとんどの場合、独自のプロバイダーを実装する必要はありません。この概要では、これらについては説明しません。  
  
### <a name="server-side-providers"></a>サーバー側プロバイダー  
 サーバー側プロバイダーは、カスタムコントロール、または Win32、Windows フォーム、[!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)]以外の UI フレームワークに基づくアプリケーションによって実装されます。  
  
 また、サーバー側プロバイダーは、サーバーからクライアントに要求するインターフェイスを [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] コア システムに公開することにより、プロセス境界を越えてクライアント アプリケーションと通信します。  
  
<a name="AutomationProviderConcepts"></a>   
## <a name="ui-automation-provider-concepts"></a>UI オートメーション プロバイダーの概念  
 ここでは、UI オートメーション プロバイダーを実装するために理解しておく必要があるいくつかの重要な概念について簡単に説明します。  
  
### <a name="elements"></a>Elements  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 要素は、UI オートメーション クライアントからアクセス可能な [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] の一部です。 例として、アプリケーション ウィンドウ、ペイン、ボタン、ツールヒント、リスト ボックス、およびリスト項目が含まれています。  
  
### <a name="navigation"></a>Navigation  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 要素は、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーとしてクライアントに公開されます。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] は、要素間を移動することによって、ツリーを構成します。 ナビゲーションは、親、兄弟、または子を指す要素ごとに、プロバイダーによって有効にされます。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーのクライアントビューの詳細については、「 [UI オートメーションツリーの概要](ui-automation-tree-overview.md)」を参照してください。  
  
### <a name="views"></a>ビュー  
 次の表に示すように、クライアントは 3 つのプリンシパル ビューで [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーを表示できます。  
  
|||  
|-|-|  
|列ビュー|すべての要素が含まれます。|  
|コントロール ビュー|コントロールである要素が含まれます。|  
|[コンテンツ] ビュー|コンテンツである要素が含まれます。|  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーのクライアントビューの詳細については、「 [UI オートメーションツリーの概要](ui-automation-tree-overview.md)」を参照してください。  
  
 コンテンツ要素またはコントロール要素としての要素の定義はプロバイダー実装で行う必要があります。 コントロール要素はコンテンツ要素であることもないこともありますが、コンテンツ要素はすべてコントロール要素です。  
  
### <a name="frameworks"></a>Frameworks  
 フレームワークは、画面のある領域で子コントロール、ヒット テスト、およびレンダリングを管理するコンポーネントです。 たとえば、Win32 ウィンドウ (HWND とも呼ばれます) は、メニューバー、ステータスバー、ボタンなどの複数の [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 要素を含むフレームワークとして機能することができます。  
  
 リストボックスやツリービューなどの Win32 コンテナーコントロールは、子項目をレンダリングしてヒットテストを実行するための独自のコードを含んでいるため、フレームワークと見なされます。 これに対して、 [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] リスト ボックスは、包含する [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] ウィンドウによってレンダリングやヒット テストが処理されるため、フレームワークではありません。  
  
 アプリケーション内の [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] はさまざまなフレームワークで構成することができます。 たとえば、HWND アプリケーションウィンドウには、ダイナミック HTML (DHTML) が含まれている場合があります。 DHTML には、コンボボックスなどのコンポーネントが含まれています。  
  
### <a name="fragments"></a>フラグメント  
 フラグメントは、特定のフレームワーク内の要素のサブツリー全体です。 サブツリーのルート ノードにある要素はフラグメント ルートと呼ばれます。 フラグメントルートには親はありませんが、他のフレームワーク (通常は Win32 ウィンドウ (HWND)) 内でホストされます。  
  
### <a name="hosts"></a>ホスト  
 すべてのフラグメントのルートノードは、要素 (通常は Win32 ウィンドウ (HWND)) でホストされている必要があります。 例外は、他の要素でホストされないデスクトップです。 カスタム コントロールのホストは、アプリケーション ウィンドウやトップ レベル コントロールのグループを含めることができるその他のウィンドウではなく、コントロール自体の HWND です。  
  
 フラグメントのホストは、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] サービスを提供するうえで重要な役割を果たします。 このホストがフラグメント ルートへの移動を可能にし、いくつかの既定のプロパティを提供するため、カスタム プロバイダーはそれらを実装する必要がありません。  
  
## <a name="see-also"></a>関連項目

- [サーバー側 UI オートメーション プロバイダーの実装](server-side-ui-automation-provider-implementation.md)
