---
title: '方法: キー付きコレクションにアクセスする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- keyed collections [Windows Forms]
- collections [Windows Forms], accessing with keys
ms.assetid: b9b79b8b-d9bf-4f8c-b9d6-9578bc3219d3
ms.openlocfilehash: 717ba9cc8605da08701de1bd13d6bc6da1c9b758
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76739616"
---
# <a name="how-to-access-keyed-collections-in-windows-forms"></a>方法 : Windows フォームのコレクションにアクセス キーを指定する

- 個々のコレクションアイテムには、キーでアクセスできます。 この機能は、通常 Windows フォームアプリケーションで使用される多くのコレクションクラスに追加されています。 次の一覧は、アクセス可能なキー付きコレクションを持つコレクションクラスの一部を示しています。  
  
- <xref:System.Windows.Forms.ListView.ListViewItemCollection>  
  
- <xref:System.Windows.Forms.ListViewItem.ListViewSubItemCollection>  
  
- <xref:System.Windows.Forms.Control.ControlCollection>  
  
- <xref:System.Windows.Forms.TabControl.TabPageCollection>  
  
- <xref:System.Windows.Forms.TreeNodeCollection>  
  
 コレクション内の項目に関連付けられているキーは、通常、項目の名前です。 次の手順では、コレクションクラスを使用して一般的なタスクを実行する方法について説明します。  
  
### <a name="to-find-and-give-focus-to-a-nested-control-in-a-control-collection"></a>コントロールコレクション内の入れ子になったコントロールを検索してフォーカスを与えるには  
  
- <xref:System.Windows.Forms.Control.ControlCollection.Find%2A> メソッドと <xref:System.Windows.Forms.Control.Focus%2A> メソッドを使用して、検索してフォーカスを与えるコントロールの名前を指定します。  
  
     [!code-csharp[System.Windows.Forms.KeyedCollectionsEx#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.KeyedCollectionsEx/CS/Form1.cs#1)]
     [!code-vb[System.Windows.Forms.KeyedCollectionsEx#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.KeyedCollectionsEx/VB/Form1.vb#1)]  
  
### <a name="to-access-an-image-in-an-image-collection"></a>イメージコレクション内のイメージにアクセスするには  
  
- アクセスするイメージの名前を指定するには、<xref:System.Windows.Forms.ImageList.ImageCollection.Item%2A> プロパティを使用します。  
  
     [!code-csharp[System.Windows.Forms.KeyedCollectionsEx#2](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.KeyedCollectionsEx/CS/Form1.cs#2)]
     [!code-vb[System.Windows.Forms.KeyedCollectionsEx#2](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.KeyedCollectionsEx/VB/Form1.vb#2)]  
  
### <a name="to-set-a-tab-page-as-the-selected-tab"></a>タブページを選択したタブとして設定するには  
  
- <xref:System.Windows.Forms.TabControl.TabPageCollection.Item%2A> プロパティと共に <xref:System.Windows.Forms.TabControl.SelectedTab%2A> プロパティを使用して、選択したタブとして設定するタブページの名前を指定します。  
  
     [!code-csharp[System.Windows.Forms.KeyedCollectionsEx#3](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.KeyedCollectionsEx/CS/Form1.cs#3)]
     [!code-vb[System.Windows.Forms.KeyedCollectionsEx#3](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.KeyedCollectionsEx/VB/Form1.vb#3)]  
  
## <a name="see-also"></a>参照

- [Windows フォームについて](getting-started-with-windows-forms.md)
- [方法: Windows フォームの ImageList コンポーネントにイメージを追加または削除する](./controls/how-to-add-or-remove-images-with-the-windows-forms-imagelist-component.md)
