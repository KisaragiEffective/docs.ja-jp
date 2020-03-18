---
title: ContextMenu コンポーネントの概要
ms.date: 03/30/2017
f1_keywords:
- ContextMenu
helpviewer_keywords:
- ContextMenu component [Windows Forms], about ContextMenu component
- context menus [Windows Forms], ContextMenu component
- shortcut menus [Windows Forms], ContextMenu component
ms.assetid: 49d6398f-d3c4-4679-84fa-1de07b68b05e
ms.openlocfilehash: 83740221894941d09d1014585513043851a518e5
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746202"
---
# <a name="contextmenu-component-overview-windows-forms"></a>ContextMenu コンポーネントの概要 (Windows フォーム)
> [!IMPORTANT]
> <xref:System.Windows.Forms.MenuStrip> と <xref:System.Windows.Forms.ContextMenuStrip> によって、以前のバージョンの <xref:System.Windows.Forms.MainMenu> および <xref:System.Windows.Forms.ContextMenu> のコントロールに置き換えられ、機能が追加されますが、<xref:System.Windows.Forms.MainMenu> と <xref:System.Windows.Forms.ContextMenu> は下位互換性と将来の使用の両方のために保持されます。  
  
 Windows フォーム <xref:System.Windows.Forms.ContextMenu> コンポーネントを使用すると、選択したオブジェクトに関連付けられている頻繁に使用するコマンドのショートカットメニューをユーザーに提供できます。 ショートカットメニューの項目は、多くの場合、アプリケーションの他の場所に表示されるメインメニューの項目のサブセットです。 ユーザーは、通常、マウスを右クリックしてショートカットメニューにアクセスできます。 Windows フォームでは、ショートカットメニューはコントロールに関連付けられています。  
  
## <a name="key-properties"></a>キー プロパティ  
 コントロールの <xref:System.Windows.Forms.Control.ContextMenu%2A> プロパティを <xref:System.Windows.Forms.ContextMenu> コンポーネントに設定することによって、ショートカットメニューをコントロールに関連付けることができます。 1つのショートカットメニューを複数のコントロールに関連付けることができますが、各コントロールにはショートカットメニューを1つしか含めることができません。  
  
 <xref:System.Windows.Forms.ContextMenu> コンポーネントのキープロパティは、<xref:System.Windows.Forms.Menu.MenuItems%2A> プロパティです。 メニュー項目を追加するには、プログラムを使用して <xref:System.Windows.Forms.MenuItem> オブジェクトを作成し、ショートカットメニューの <xref:System.Windows.Forms.Menu.MenuItemCollection> に追加します。 ショートカットメニュー内の項目は通常、他のメニューから描画されるので、ほとんどの場合、項目をコピーすることでショートカットメニューに追加します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ContextMenu>
- <xref:System.Windows.Forms.MenuStrip>
- <xref:System.Windows.Forms.ContextMenuStrip>
