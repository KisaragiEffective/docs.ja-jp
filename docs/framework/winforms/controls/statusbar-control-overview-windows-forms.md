---
title: StatusBar コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- StatusBar
helpviewer_keywords:
- StatusBar control [Windows Forms], about StatusBar control
- status bars
ms.assetid: b7b9852c-633d-4416-bb2e-94852b989c6c
ms.openlocfilehash: b0b97bc3cb0291871e1fd113d82d0b480b53cdba
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742866"
---
# <a name="statusbar-control-overview-windows-forms"></a>StatusBar コントロールの概要 (Windows フォーム)
> [!IMPORTANT]
> <xref:System.Windows.Forms.StatusStrip> コントロールと <xref:System.Windows.Forms.ToolStripStatusLabel> コントロールは、<xref:System.Windows.Forms.StatusBar> および <xref:System.Windows.Forms.StatusBarPanel> コントロールに対して機能を置き換え、追加します。ただし、<xref:System.Windows.Forms.StatusBar> および <xref:System.Windows.Forms.StatusBarPanel> のコントロールは、下位互換性と将来の使用の両方のために保持されます (選択した場合)。  
  
 Windows フォーム[StatusBar コントロール](statusbar-control-windows-forms.md)は、フォーム上で通常はウィンドウの下部に表示される領域として使用されます。この領域では、アプリケーションはさまざまな種類の状態情報を表示できます。 <xref:System.Windows.Forms.StatusBar> コントロールには、ステータスバーパネルを表示して、状態を示すテキストやアイコン、またはプロセスが動作していることを示す一連のアイコンをアニメーションに表示することができます。たとえば、Microsoft Word では、ドキュメントが保存されていることを示しています。  
  
## <a name="using-the-statusbar-control"></a>StatusBar コントロールの使用  
 Internet Explorer は、ハイパーリンク上でマウスがロールオーバーしたときに、ステータスバーを使用してページの URL を示します。Microsoft Word では、ページの場所、セクションの場所、および上書きやリビジョンの追跡などの編集モードに関する情報が提供されます。また、Visual Studio はステータスバーを使用して、ドッキングされたウィンドウをドッキングまたはフローティングとして操作する方法を示すなど、状況に応じた情報を提供します。  
  
 ステータスバーに1つのメッセージを表示するには、[<xref:System.Windows.Forms.StatusBar.ShowPanels%2A>] プロパティを `false` (既定値) に設定し、ステータスバーの [<xref:System.Windows.Forms.StatusBar.Text%2A>] プロパティをステータスバーに表示するテキストに設定します。 ステータスバーをパネルに分割して、複数の種類の情報を表示するには、<xref:System.Windows.Forms.StatusBar.ShowPanels%2A> プロパティを `true` に設定し、<xref:System.Windows.Forms.StatusBar.StatusBarPanelCollection>の <xref:System.Windows.Forms.StatusBar.StatusBarPanelCollection.Add%2A> メソッドを使用します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.StatusBar>
- <xref:System.Windows.Forms.ToolStripStatusLabel>
- [方法: Windows フォームの StatusBar コントロールでクリックされたパネルを確認する](determine-which-panel-wf-statusbar-control-was-clicked.md)
