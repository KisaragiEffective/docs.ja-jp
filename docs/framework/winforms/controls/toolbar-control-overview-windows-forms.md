---
title: ToolBar コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- ToolBar
helpviewer_keywords:
- toolbars [Windows Forms], about toolbars
- ToolBar control [Windows Forms], about ToolBar controls
ms.assetid: d426b203-0216-4dbe-b834-1641e50a9c29
ms.openlocfilehash: f364e052258703331496f8103b48953a90aa3a9b
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76735488"
---
# <a name="toolbar-control-overview-windows-forms"></a>ToolBar コントロールの概要 (Windows フォーム)
> [!NOTE]
> <xref:System.Windows.Forms.ToolStrip> コントロールは、<xref:System.Windows.Forms.ToolBar> コントロールに代わると共に追加の機能を提供します。ただし、<xref:System.Windows.Forms.ToolBar> コントロールは、下位互換性を保つ目的および将来使用する目的で保持されます。  
  
 Windows フォーム <xref:System.Windows.Forms.ToolBar> コントロールは、コマンドをアクティブ化するドロップダウン メニューとビットマップのボタンの行を表示するコントロール バーとしてフォームを使用します。 そのため、ツールバー ボタンをクリックすることは、メニュー コマンドを選択することと同じです。 ボタンは、プッシュ ボタン、ドロップダウン メニュー、または区切りとして表示して機能するように構成できます。 通常、ツールバーには、アプリケーションのメニュー構造の項目に対応するボタンとメニューが含まれ、アプリケーションで最も頻繁に使用される関数やコマンドにすばやくアクセスできます。  
  
## <a name="working-with-the-toolbar-control"></a>ToolBar コントロールの操作  
 <xref:System.Windows.Forms.ToolBar> コントロールは通常、親ウィンドウの上部に "ドッキング" されますが、ウィンドウの任意の辺にドッキングすることもできます。 ツール バー ボタンをマウス ポインターでポイントすると、ツールヒントが表示されます。 ツールヒントは、ボタンやメニューの目的を簡単に説明する小さなポップアップ ウィンドウです。 ツールヒントを表示するには、<xref:System.Windows.Forms.ToolBar.ShowToolTips%2A> プロパティを `true`に設定する必要があります。  
  
> [!NOTE]
> 特定のアプリケーション機能のコントロールは、アプリケーション ウィンドウの上に "フローティング" して位置変更できるツールバーとよく似ています。 Windows フォームのツール バー コントロールでは、このような操作を実行できません。  
  
 <xref:System.Windows.Forms.ToolBar.Appearance%2A> プロパティが <xref:System.Windows.Forms.ToolBarAppearance>に設定されている場合、ツールバーのボタンが浮き出し、3次元で表示されます。 ツールバーの [<xref:System.Windows.Forms.ToolBar.Appearance%2A>] プロパティを [<xref:System.Windows.Forms.ToolBarAppearance>] に設定すると、ツールバーとそのボタンの外観をフラットにすることができます。 フラットなボタンにマウス ポインターを置くと、ボタンは 3 次元表示に変わります。 ツール バー ボタンは、区切りを使用すると論理グループに分けることができます。 区切り記号は、<xref:System.Windows.Forms.ToolBarButton.Style%2A> プロパティが <xref:System.Windows.Forms.ToolBarButtonStyle>に設定されたツールバーボタンです。 区切りは、ツール バー上の空スペースとして表示されます。 ツール バーはフラットに表示され、ボタンの区切りは、ボタンの間にスペースを入れて表示されるのではなく線として表示されます。  
  
 <xref:System.Windows.Forms.ToolBar> コントロールを使用すると、<xref:System.Windows.Forms.Button> オブジェクトを <xref:System.Windows.Forms.ToolBar.Buttons%2A> コレクションに追加することで、ツールバーを作成できます。 コレクションエディターを使用して、<xref:System.Windows.Forms.ToolBar> コントロールにボタンを追加できます。各 <xref:System.Windows.Forms.Button> オブジェクトにはテキストまたはイメージが割り当てられている必要がありますが、両方を割り当てることができます。 イメージは、関連付けられた [ImageList](imagelist-component-windows-forms.md) コンポーネントから取得されます。 実行時には、<xref:System.Windows.Forms.ToolBar.ToolBarButtonCollection.Add%2A> メソッドと <xref:System.Windows.Forms.ToolBar.ToolBarButtonCollection.Remove%2A> メソッドを使用して、<xref:System.Windows.Forms.ToolBar.ToolBarButtonCollection> のボタンを追加または削除できます。 <xref:System.Windows.Forms.ToolBar>のボタンをプログラミングするには、<xref:System.Windows.Forms.ToolBarButtonClickEventArgs> クラスの <xref:System.Windows.Forms.ToolBarButtonClickEventArgs.Button%2A> プロパティを使用して、クリックされたボタンを判別するために、<xref:System.Windows.Forms.ToolBar>の <xref:System.Windows.Forms.ToolBar.ButtonClick> イベントにコードを追加します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ToolBar>
- [ToolBar コントロール](toolbar-control-windows-forms.md)
- [方法: ツール バー コントロールにボタンを追加する](how-to-add-buttons-to-a-toolbar-control.md)
- [方法: ツール バー ボタンのアイコンを定義する](how-to-define-an-icon-for-a-toolbar-button.md)
- [方法: ツール バー ボタンのメニュー イベントをトリガーする](how-to-trigger-menu-events-for-toolbar-buttons.md)
