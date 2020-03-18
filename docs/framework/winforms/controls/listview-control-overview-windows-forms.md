---
title: ListView コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- ListView
helpviewer_keywords:
- lists
- ListView control [Windows Forms], about ListView control
- list views
ms.assetid: c9ef56c1-3bb1-4101-9f4e-e95e720f2756
ms.openlocfilehash: 9308ff6646704d16b32669827a1f26bebf6958d8
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745156"
---
# <a name="listview-control-overview-windows-forms"></a>ListView コントロールの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.ListView> コントロールにはアイコン表示で項目の一覧が表示されます。 リスト ビューを使用すると、Windows エクスプローラーの右側のペインのようなユーザー インターフェイスを作成することができます。 このコントロールには、LargeIcon、SmallIcon、List、Details の4つのビューモードがあります。  
  
## <a name="what-you-can-do-with-the-listview-control"></a>ListView コントロールでできること  
  
> [!NOTE]
> 追加の表示モードであるタイルは、Windows XP と Windows Server 2003 オペレーティングシステムでのみ使用できます。 詳細については、「[方法: Windows フォーム ListView コントロールのタイルビューを有効](how-to-enable-tile-view-in-a-windows-forms-listview-control.md)にする」を参照してください。  
  
 LargeIcon モードでは、項目のテキストの横に大きいアイコンが表示されます。コントロールのサイズが十分な場合、項目は複数の列に表示されます。 SmallIcon モードは、小さいアイコンを表示する点を除いて同じです。 リストモードでは小さいアイコンが表示されますが、常に1つの列に表示されます。 詳細モードでは、複数の列に項目が表示されます。 詳細については、「[方法: Windows フォーム ListView コントロールに列を追加](how-to-add-columns-to-the-windows-forms-listview-control.md)する」を参照してください。 ビューモードは、<xref:System.Windows.Forms.ListView.View%2A> プロパティによって決定されます。 すべての表示モードでは、イメージリストのイメージを表示できます。 詳細については、「[方法: Windows フォーム ListView コントロールのアイコンを表示する](how-to-display-icons-for-the-windows-forms-listview-control.md)」を参照してください。  
  
 次の表に、<xref:System.Windows.Forms.ListView> メンバーの一部と、で有効なビューを示します。  
  
|ListView メンバー|表示|  
|---------------------|----------|  
|<xref:System.Windows.Forms.ListView.Alignment%2A> プロパティ|<xref:System.Windows.Forms.View.SmallIcon> または <xref:System.Windows.Forms.View.LargeIcon>|  
|<xref:System.Windows.Forms.ListView.AutoArrange%2A> プロパティ|<xref:System.Windows.Forms.View.SmallIcon> または <xref:System.Windows.Forms.View.LargeIcon>|  
|<xref:System.Windows.Forms.ListView.AutoResizeColumn%2A> メソッド|<xref:System.Windows.Forms.View.Details>|  
|<xref:System.Windows.Forms.ListView.Columns%2A> プロパティ|<xref:System.Windows.Forms.View.Details> または <xref:System.Windows.Forms.View.Tile>|  
|<xref:System.Windows.Forms.ListView.DrawSubItem> イベント|<xref:System.Windows.Forms.View.Details>|  
|<xref:System.Windows.Forms.ListView.FindItemWithText%2A> メソッド|<xref:System.Windows.Forms.View.Details>、<xref:System.Windows.Forms.View.List>、または <xref:System.Windows.Forms.View.Tile>|  
|<xref:System.Windows.Forms.ListView.FindNearestItem%2A> メソッド|<xref:System.Windows.Forms.View.SmallIcon> または <xref:System.Windows.Forms.View.LargeIcon>|  
|<xref:System.Windows.Forms.ListView.GetItemAt%2A> メソッド|<xref:System.Windows.Forms.View.Details> または <xref:System.Windows.Forms.View.Tile>|  
|<xref:System.Windows.Forms.ListView.Groups%2A> プロパティ|<xref:System.Windows.Forms.View.List> を除くすべてのビュー|  
|<xref:System.Windows.Forms.ListView.HeaderStyle%2A> プロパティ|[https://login.microsoftonline.com/consumers/](<xref:System.Windows.Forms.View.Details>)|  
|<xref:System.Windows.Forms.ListView.InsertionMark%2A> プロパティ|<xref:System.Windows.Forms.View.LargeIcon>、<xref:System.Windows.Forms.View.SmallIcon>、または <xref:System.Windows.Forms.View.Tile>|  
  
 <xref:System.Windows.Forms.ListView> コントロールのキープロパティは <xref:System.Windows.Forms.ListView.Items%2A>で、コントロールによって表示される項目が含まれています。 <xref:System.Windows.Forms.ListView.SelectedItems%2A> プロパティには、コントロールで現在選択されている項目のコレクションが含まれます。 ユーザーは複数の項目を選択できます。たとえば、<xref:System.Windows.Forms.ListView.MultiSelect%2A> プロパティが `true`に設定されている場合は、複数の項目を一度にドラッグアンドドロップして別のコントロールに移動できます。 <xref:System.Windows.Forms.ListView.CheckBoxes%2A> プロパティが `true`に設定されている場合、<xref:System.Windows.Forms.ListView> コントロールは、項目の横にチェックボックスを表示できます。  
  
 <xref:System.Windows.Forms.ListView.Activation%2A> プロパティは、リスト内の項目をアクティブにするためにユーザーが実行する必要のあるアクションの種類を決定します。オプションは、<xref:System.Windows.Forms.ItemActivation.Standard>、<xref:System.Windows.Forms.ItemActivation.OneClick>、および <xref:System.Windows.Forms.ItemActivation.TwoClick>です。 <xref:System.Windows.Forms.ItemActivation.OneClick> ライセンス認証を行うには、1回のクリックで項目をアクティブ化する必要があります。 <xref:System.Windows.Forms.ItemActivation.TwoClick> ライセンス認証を行うには、ユーザーがダブルクリックして項目をアクティブにする必要があります。1回のクリックで、項目のテキストの色を変更します。 <xref:System.Windows.Forms.ItemActivation.Standard> ライセンス認証を行うには、ユーザーがダブルクリックして項目をアクティブにする必要がありますが、項目は外観を変更しません。  
  
 <xref:System.Windows.Forms.ListView> コントロールでは、グループ化、タイルビュー、挿入マークなど、Windows XP プラットフォームで使用できる視覚スタイルやその他の機能もサポートされています。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ListView>
- [ListView コントロール](listview-control-windows-forms.md)
- [方法: Windows フォーム ListView コントロールで項目を追加および削除する](how-to-add-and-remove-items-with-the-windows-forms-listview-control.md)
- [方法: Windows フォーム ListView コントロールに列を追加する](how-to-add-columns-to-the-windows-forms-listview-control.md)
- [方法: Windows フォーム ListView コントロールのアイコンを表示する](how-to-display-icons-for-the-windows-forms-listview-control.md)
- [方法: Windows フォーム ListView コントロールの列にサブ項目を表示する](how-to-display-subitems-in-columns-with-the-windows-forms-listview-control.md)
- [方法: Windows フォーム ListView コントロール内の項目を選択する](how-to-select-an-item-in-the-windows-forms-listview-control.md)
- [方法: Windows フォーム ListView コントロールの項目をグループ化する](how-to-group-items-in-a-windows-forms-listview-control.md)
- [方法: Windows フォーム ListView コントロールに挿入マークを表示する](how-to-display-an-insertion-mark-in-a-windows-forms-listview-control.md)
- [方法: ListView コントロールに検索機能を追加する](how-to-add-search-capabilities-to-a-listview-control.md)
- [方法: TreeView コントロールまたは ListView コントロール (Windows フォーム) にカスタム情報を追加する](add-custom-information-to-a-treeview-or-listview-control-wf.md)
- [方法: Windows フォームでマルチペイン ユーザー インターフェイスを作成する](how-to-create-a-multipane-user-interface-with-windows-forms.md)
