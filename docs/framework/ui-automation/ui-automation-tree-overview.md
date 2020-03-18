---
title: UI オートメーション ツリーの概要
ms.date: 03/30/2017
helpviewer_keywords:
- automation tree
- UI Automation, tree
ms.assetid: 03b98058-bdb3-47a0-8ff7-45e6cdf67166
ms.openlocfilehash: f541aab7ed5aae48b943ba5699366fe6a3f21a4c
ms.sourcegitcommit: 9a97c76e141333394676bc5d264c6624b6f45bcf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2020
ms.locfileid: "75741394"
---
# <a name="ui-automation-tree-overview"></a>UI オートメーション ツリーの概要
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。  
  
 支援技術製品とテスト スクリプトは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーを移動して [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] とその要素に関する情報を収集します。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリー内には、現在のデスクトップを表すルート要素 (<xref:System.Windows.Automation.AutomationElement.RootElement%2A>) があり、その子要素はアプリケーションウィンドウを表します。 これらの子要素のそれぞれに、メニュー、ボタン、ツールバー、リスト ボックスなどの [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] の構成部分を表す要素を含めることができます。 さらに、これらの要素には、リスト項目などの要素を含めることができます。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーは、固定された構造体ではなく、数千もの要素が含まれる場合もあるため、その全体像を見ることはほとんどありません。 その一部は必要に応じてビルドされ、要素の追加、移動、削除に伴って変更されます。  
  
 UI オートメーション プロバイダーは [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーをサポートするために、ルート (通常は、ウィンドウでホストされます) とサブツリーからなるフラグメント内の項目間のナビゲーションを実装しています。 ただし、プロバイダーは、コントロール間のナビゲーションには関与しません。 これは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] コアにより、既定のウィンドウ プロバイダーからの情報を使用して管理されます。  
  
<a name="uiautomation_tree_view"></a>   
## <a name="views-of-the-automation-tree"></a>オートメーション ツリーのビュー  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーをフィルター処理することで、特定のクライアントに関連する <xref:System.Windows.Automation.AutomationElement> オブジェクトだけが含まれるビューを作成できます。 この手法により、クライアントは特定のニーズに合わせて、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] で表現される構造体をカスタマイズできます。  
  
 クライアントがビューをカスタマイズする方法には、スコープ設定とフィルター処理の 2 つがあります。 スコープ設定では、基本要素を起点としてビューの範囲を定義します。たとえば、アプリケーションがデスクトップの直接の子だけを検索するか、アプリケーション ウィンドウのすべての子孫を検索するかを定義できます。 フィルター処理では、ビューに含める要素の種類を定義します。  
  
 UI オートメーション プロバイダーは、要素にプロパティ (<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty> および <xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty> プロパティを含む) を定義することで、フィルター処理をサポートしています。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] は、次の 3 つの既定のビューを提供します。 これらのビューは、実行されるフィルター処理の種類によって定義されます。すべてのビューのスコープは、アプリケーションによって定義されます。 さらに、アプリケーションではプロパティに他のフィルターを適用して、たとえば有効にされているコントロールだけをコントロール ビューに含めることもできます。  
  
<a name="uiautomation_raw_view"></a>   
### <a name="raw-view"></a>列ビュー  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーの未加工ビューは、デスクトップをルートとする <xref:System.Windows.Automation.AutomationElement> オブジェクトの完全なツリーです。 未加工ビューは、アプリケーションのネイティブ プログラムによる構造に忠実に従っているため、使用できるビューの中では最も詳細なビューです。 また、ツリーの他のビューは、未加工ビューに基づいて構築されます。 このビューは基になる [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] フレームワークに依存しているため、[!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] ボタンの未加工ビューには、Win32 ボタンとは異なる未加工のビューがあります。  
  
 未加工ビューを取得するには、プロパティを指定せずに要素を検索するか、<xref:System.Windows.Automation.TreeWalker.RawViewWalker> を使用してツリーを移動します。  
  
<a name="uiautomation_control_view"></a>   
### <a name="control-view"></a>コントロール ビュー  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーのコントロール ビューは、[!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] をエンド ユーザーに説明してエンド ユーザーがアプリケーションと対話できるよう支援するという、支援技術製品のタスクを簡素化します。コントロール ビューは、エンド ユーザーが認識する [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 構造に密接に対応しているためです。  
  
 コントロール ビューは、未加工ビューのサブセットです。 コントロール ビューには、エンド ユーザーが対話型として理解する、または [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] のコントロールの論理構造に貢献する、未加工ビューのすべての [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 項目が含まれています。 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] の論理構造に貢献する一方、自身は対話型ではない [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 項目の例としては、リスト ビューなどの項目コンテナー、ヘッダー、ツールバー、メニュー、ステータス バーが挙げられます。 レイアウトや装飾の目的のためだけに使用される非対話型項目は、コントロール ビューには表示されません。 その一例は、パネルです。パネルは、ダイアログでコントロールをレイアウトするためだけに使用され、情報はまったく含まれません。 コントロール ビューに表示される非対話型の項目は、情報を提供するグラフィックとダイアログ内の静的テキストです。 コントロール ビューに含まれる非対話型の項目がキーボード フォーカスを受け取ることはできません。  
  
 コントロール ビューを取得するには、<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsControlElement%2A> プロパティが `true` に設定された要素を検索するか、<xref:System.Windows.Automation.TreeWalker.ControlViewWalker> を使用してツリーを移動します。  
  
<a name="uiautomation_content_view"></a>   
### <a name="content-view"></a>コンテンツ ビュー  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーのコンテンツ ビューは、コントロール ビューのサブセットです。 コンテンツ ビューには、ユーザー インターフェイスで実際の情報を伝える [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 項目が含まれています。これには、キーボード フォーカスを受け取ることができる [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 項目、そして [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 項目のラベルではないテキストが含まれます。 たとえば、ドロップダウン コンボ ボックス内の値は、エンドユーザーが使用する情報を表すため、コンテンツ ビューに表示されます。 コンテンツ ビューでは、コンボ ボックスとリスト ボックスはどちらも、[!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 項目のコレクションとして表現され、これらの項目の中から 1 つ、場合によっては複数の項目を選択できます。 コンテンツ ビューの目的は、ユーザーに提示するデータ (つまりコンテンツ) を表示することなので、このビューは、どの項目が常に開かれていて、どの項目が展開または折りたたむことができるかは重要ではありません。  
  
 コンテンツ ビューを取得するには、<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsContentElement%2A> プロパティが `true` に設定された要素を検索するか、<xref:System.Windows.Automation.TreeWalker.ContentViewWalker> を使用してツリーを移動します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Automation.AutomationElement>
- [UI オートメーションの概要](ui-automation-overview.md)
