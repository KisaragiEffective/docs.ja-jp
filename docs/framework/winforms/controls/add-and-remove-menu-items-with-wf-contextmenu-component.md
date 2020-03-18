---
title: ContextMenu コンポーネントを使用したメニュー項目の追加と削除
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- context menus [Windows Forms], removing items
- ContextMenu component [Windows Forms], adding items
- shortcut menus [Windows Forms], removing items
- shortcut menus [Windows Forms], examples
- context menus [Windows Forms], adding items
- shortcut menus [Windows Forms], adding items
- ContextMenu component [Windows Forms], removing items
- context menus [Windows Forms], examples
- examples [Windows Forms], context menus
ms.assetid: 426d1eaf-7fb8-4b0b-8a33-5e8721786ea4
ms.openlocfilehash: 989ab6d47ec761930a32f542b5fa1136e831f73d
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746268"
---
# <a name="how-to-add-and-remove-menu-items-with-the-windows-forms-contextmenu-component"></a>方法 : Windows フォーム ContextMenu コンポーネントのメニュー項目を追加および削除する
Windows フォームでショートカットメニュー項目を追加および削除する方法について説明します。  
  
 Windows フォーム <xref:System.Windows.Forms.ContextMenu> コンポーネントには、選択したオブジェクトに関連する頻繁に使用するコマンドのメニューが用意されています。 <xref:System.Windows.Forms.MenuItem> オブジェクトを <xref:System.Windows.Forms.Menu.MenuItems%2A> コレクションに追加することで、ショートカットメニューに項目を追加できます。  
  
 ショートカットメニューから項目を完全に削除することができます。ただし、実行時には、代わりに項目を非表示にしたり無効にしたりする方が適切な場合があります。  
  
> [!IMPORTANT]
> <xref:System.Windows.Forms.MenuStrip> と <xref:System.Windows.Forms.ContextMenuStrip> によって、以前のバージョンの <xref:System.Windows.Forms.MainMenu> および <xref:System.Windows.Forms.ContextMenu> のコントロールに置き換えられ、機能が追加されますが、<xref:System.Windows.Forms.MainMenu> と <xref:System.Windows.Forms.ContextMenu> は下位互換性と将来の使用の両方のために保持されます。  
  
### <a name="to-remove-items-from-a-shortcut-menu"></a>ショートカットメニューから項目を削除するには  
  
1. <xref:System.Windows.Forms.ContextMenu> コンポーネントの <xref:System.Windows.Forms.Menu.MenuItems%2A> コレクションの <xref:System.Windows.Forms.Menu.MenuItemCollection.Remove%2A> または <xref:System.Windows.Forms.Menu.MenuItemCollection.RemoveAt%2A> メソッドを使用して、特定のメニュー項目を削除します。  
  
    ```vb  
    ' Removes the first item in the shortcut menu.  
    ContextMenu1.MenuItems.RemoveAt(0)  
    ' Removes a particular object from the shortcut menu.  
    ContextMenu1.MenuItems.Remove(mnuItemNew)  
    ```  
  
    ```csharp  
    // Removes the first item in the shortcut menu.  
    contextMenu1.MenuItems.RemoveAt(0);  
    // Removes a particular object from the shortcut menu.  
    contextMenu1.MenuItems.Remove(mnuItemNew);  
    ```  
  
    ```cpp  
    // Removes the first item in the shortcut menu.  
    contextMenu1->MenuItems->RemoveAt(0);  
    // Removes a particular object from the shortcut menu.  
    contextMenu1->MenuItems->Remove(mnuItemNew);  
    ```  
  
     または  
  
2. <xref:System.Windows.Forms.ContextMenu> コンポーネントの `MenuItems` コレクションの `Clear` メソッドを使用して、メニューからすべての項目を削除します。  
  
    ```vb  
    ContextMenu1.MenuItems.Clear()  
    ```  
  
    ```csharp  
    contextMenu1.MenuItems.Clear();  
    ```  
  
    ```cpp  
    contextMenu1->MenuItems->Clear();  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ContextMenu>
- [ContextMenu コンポーネント](contextmenu-component-windows-forms.md)
- [ContextMenu コンポーネントの概要](contextmenu-component-overview-windows-forms.md)
