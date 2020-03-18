---
title: MenuStrip コントロールでのメニュー項目のマージ
ms.date: 03/30/2017
helpviewer_keywords:
- MenuStrip [Windows Forms], merging
- merging [Windows Forms], general concepts
ms.assetid: 95e113ba-f362-4dda-8a76-6d95ddc45cee
ms.openlocfilehash: 9fc2b40ef23d72db51853c124095b734a7646cda
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76739040"
---
# <a name="merging-menu-items-in-the-windows-forms-menustrip-control"></a>Windows フォームの MenuStrip コントロールへのメニュー項目のマージ
マルチドキュメントインターフェイス (MDI) アプリケーションを使用している場合は、メニュー項目またはメニュー全体を子フォームから親フォームのメニューにマージできます。  
  
 このトピックでは、MDI アプリケーションでのメニュー項目のマージに関連する基本的な概念について説明します。  
  
## <a name="general-concepts"></a>一般的な概念  
 マージプロシージャには、ターゲットとソース管理の両方が含まれます。  
  
- ターゲットは、メニュー項目をマージするメインまたは MDI 親フォームの <xref:System.Windows.Forms.MenuStrip> コントロールです。  
  
- Source は、[ターゲット] メニューにマージするメニュー項目を含む MDI 子フォームの <xref:System.Windows.Forms.MenuStrip> コントロールです。  
  
 <xref:System.Windows.Forms.MenuStrip.MdiWindowListItem%2A> プロパティは、現在の MDI 親フォームの MDI 子フォームのタイトルを設定するドロップダウンリストを持つメニュー項目を識別します。 たとえば、通常は **[ウィンドウ]** メニューで現在開いている MDI 子を一覧表示します。  
  
 <xref:System.Windows.Forms.ToolStripMenuItem.IsMdiWindowListEntry%2A> プロパティは、MDI 子フォームの <xref:System.Windows.Forms.MenuStrip> から取得したメニュー項目を識別します。  
  
 メニュー項目は、手動または自動で結合できます。 メニュー項目は両方の方法で同じ方法でマージされますが、このトピックで後述する「手動マージ」と「自動マージ」のセクションで説明したように、マージのアクティブ化は異なります。 手動マージと自動マージの両方で、マージアクションは、次のマージアクションに影響します。  
  
 <xref:System.Windows.Forms.MenuStrip> のマージでは、<xref:System.Windows.Forms.MainMenu>の場合と同様に、メニュー項目を複製するのではなく、別の <xref:System.Windows.Forms.ToolStrip> に移動します。  
  
## <a name="mergeaction-values"></a>MergeAction 値  
 ソース <xref:System.Windows.Forms.MenuStrip> のメニュー項目に対するマージアクションは、<xref:System.Windows.Forms.MergeAction> プロパティを使用して設定します。  
  
 次の表では、使用可能なマージアクションの意味と一般的な使用方法について説明します。  
  
|MergeAction 値|[説明]|一般的な使用方法|  
|-----------------------|-----------------|-----------------|  
|<xref:System.Windows.Forms.MergeAction.Append>|標準ソース項目をターゲット項目のコレクションの末尾に追加します。|プログラムの一部がアクティブになったときにメニューの最後にメニュー項目を追加する。|  
|<xref:System.Windows.Forms.MergeAction.Insert>|ソース項目の <xref:System.Windows.Forms.ToolStripItem.MergeIndex%2A> プロパティによって指定された場所で、ターゲット項目のコレクションにソース項目を追加します。|プログラムの一部がアクティブになったときに、メニューの中央または先頭にメニュー項目を追加します。<br /><br /> <xref:System.Windows.Forms.ToolStripItem.MergeIndex%2A> の値が両方のメニュー項目で同じ場合は、逆順に追加されます。 元の順序を維持するには、<xref:System.Windows.Forms.ToolStripItem.MergeIndex%2A> 適切に設定します。|  
|<xref:System.Windows.Forms.MergeAction.Replace>|一致する文字列を検索します。または、一致する文字列が見つからない場合は <xref:System.Windows.Forms.ToolStripItem.MergeIndex%2A> 値を使用し、一致するターゲットメニュー項目をソースメニュー項目に置き換えます。|ターゲットメニュー項目を、同じ名前のソースメニュー項目で置き換えます。|  
|<xref:System.Windows.Forms.MergeAction.MatchOnly>|テキストの一致を検索します。テキストの一致が見つからない場合は <xref:System.Windows.Forms.ToolStripItem.MergeIndex%2A> 値を使用し、ソースからターゲットにすべてのドロップダウン項目を追加します。|メニュー項目をサブメニューに挿入または追加したり、サブメニューからメニュー項目を削除したりするメニュー構造を構築します。 たとえば、MDI 子からメイン <xref:System.Windows.Forms.MenuStrip> **[名前を付けて保存]** メニューにメニュー項目を追加できます。<br /><br /> <xref:System.Windows.Forms.MergeAction.MatchOnly> を使用すると、操作を行わなくてもメニュー構造内を移動できます。 これにより、後続の項目を評価することができます。|  
|<xref:System.Windows.Forms.MergeAction.Remove>|テキスト一致を検索します。または、テキストの一致が見つからない場合は <xref:System.Windows.Forms.ToolStripItem.MergeIndex%2A> 値を使用して、対象から項目を削除します。|ターゲット <xref:System.Windows.Forms.MenuStrip>からメニュー項目を削除します。|  
  
## <a name="manual-merging"></a>手動マージ  
 自動マージに参加するのは <xref:System.Windows.Forms.MenuStrip> コントロールのみです。 <xref:System.Windows.Forms.ToolStrip> や <xref:System.Windows.Forms.StatusStrip> コントロールなど、他のコントロールの項目を結合するには、必要に応じてコード内の <xref:System.Windows.Forms.ToolStripManager.Merge%2A> および <xref:System.Windows.Forms.ToolStripManager.RevertMerge%2A> メソッドを呼び出すことによって、それらを手動でマージする必要があります。  
  
## <a name="automatic-merging"></a>自動マージ  
 ソースフォームをアクティブ化することで、MDI アプリケーションの自動マージを使用できます。 MDI アプリケーションで <xref:System.Windows.Forms.MenuStrip> を使用するには、<xref:System.Windows.Forms.Form.MainMenuStrip%2A> プロパティをターゲット <xref:System.Windows.Forms.MenuStrip> に設定します。これにより、ソース <xref:System.Windows.Forms.MenuStrip> で実行されるマージアクションがターゲット <xref:System.Windows.Forms.MenuStrip>に反映されます。  
  
 自動マージをトリガーするには、MDI ソースの <xref:System.Windows.Forms.MenuStrip> をアクティブにします。 アクティブ化時には、ソース <xref:System.Windows.Forms.MenuStrip> が MDI ターゲットにマージされます。 新しいフォームがアクティブになると、最後のフォームでマージが元に戻され、新しいフォームでトリガーされます。 この動作を制御するには、各 <xref:System.Windows.Forms.ToolStripItem>で必要に応じて <xref:System.Windows.Forms.ToolStripItem.MergeAction%2A> プロパティを設定し、<xref:System.Windows.Forms.MenuStrip>ごとに <xref:System.Windows.Forms.ToolStrip.AllowMerge%2A> プロパティを設定します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ToolStripManager>
- <xref:System.Windows.Forms.MenuStrip>
- [MenuStrip コントロール](menustrip-control-windows-forms.md)
- [方法: MenuStrip を使用して MDI ウィンドウの一覧を作成する](how-to-create-an-mdi-window-list-with-menustrip-windows-forms.md)
- [方法: MDI アプリケーションでメニューの自動マージを設定する](how-to-set-up-automatic-menu-merging-for-mdi-applications.md)
