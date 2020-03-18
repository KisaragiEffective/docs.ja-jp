---
title: ToolBar コントロール
ms.date: 03/30/2017
helpviewer_keywords:
- toolbars [Windows Forms]
- ToolBar control [Windows Forms]
ms.assetid: 6b40e9ce-6a7a-4784-bfc9-7f1d36b7462e
ms.openlocfilehash: 027c96bb49e9446244e4b08ba93c551ef043b3c0
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76735454"
---
# <a name="toolbar-control-windows-forms"></a>ToolBar コントロール (Windows フォーム)
> [!NOTE]
> <xref:System.Windows.Forms.ToolStrip> コントロールは、`ToolBar` コントロールに代わると共に追加の機能を提供します。ただし、`ToolBar` コントロールは、下位互換性を保つ目的および将来使用する目的で保持されます。  
  
 Windows フォーム `ToolBar` コントロールは、コマンドをアクティブ化するドロップダウン メニューとビットマップのボタンの行を表示するコントロール バーとしてフォームを使用します。 そのため、ツールバー ボタンのクリックは、メニュー コマンドと同じです。 ボタンは、プッシュ ボタン、ドロップダウン メニュー、または区切り記号として表示して機能するように構成できます。 通常、ツールバーには、アプリケーションのメニュー構造の項目に対応するボタンとメニューが含まれ、アプリケーションで最も頻繁に使用される関数やコマンドにすばやくアクセスできます。  
  
> [!NOTE]
> `ToolBar` コントロールの <xref:System.Windows.Forms.ToolBarButton.DropDownMenu%2A> プロパティは、<xref:System.Windows.Forms.ContextMenu> クラスのインスタンスを参照として取得します。 このプロパティは <xref:System.Windows.Forms.Menu> クラスから継承する任意のオブジェクトを受け入れるため、アプリケーションのツールバーにこの種類のボタンを実装する場合は、渡す参照を慎重に検討してください。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [ToolBar コントロールの概要](toolbar-control-overview-windows-forms.md)  
 ユーザーが操作できるカスタム ツールバーを設計できる、`ToolBar` コントロールの一般的な概念について説明します。  
  
 [方法: ツール バー コントロールにボタンを追加する](how-to-add-buttons-to-a-toolbar-control.md)  
 ボタンを `ToolBar` コントロールに追加する方法について説明します。  
  
 [方法: ツール バー ボタンのアイコンを定義する](how-to-define-an-icon-for-a-toolbar-button.md)  
 アイコンを `ToolBar` コントロールのボタン内に表示する方法について説明します。  
  
 [方法: ツール バー ボタンのメニュー イベントをトリガーする](how-to-trigger-menu-events-for-toolbar-buttons.md)  
 `ToolBar` コントロールでユーザーがクリックするボタンを解釈するコードの記述方法について説明します。  
  
 「[方法: デザイナーを使用してツールバーのボタンのアイコンを定義](how-to-define-an-icon-for-a-toolbar-button-using-the-designer.md)する」、「[方法: デザイナーを使用してツールバーコントロールにボタンを追加](how-to-add-buttons-to-a-toolbar-control-using-the-designer.md)する」も参照してください。  
  
## <a name="reference"></a>リファレンス  
 <xref:System.Windows.Forms.ToolBar> クラス  
 クラスとそのメンバーに関するリファレンス情報を提供します。  
  
## <a name="related-sections"></a>関連項目  
 [Windows フォームで使用するコントロール](controls-to-use-on-windows-forms.md)  
 Windows フォーム コントロールの完全な一覧を、使用に関する情報リンクと共に提供します。  
  
 [ToolStrip コントロール](toolstrip-control-windows-forms.md)  
 Windows フォーム アプリケーションでメニュー、コントロール、およびユーザー コントロールをホストするツールバーについて説明します。
