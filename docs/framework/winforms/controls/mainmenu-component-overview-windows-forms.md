---
title: MainMenu コンポーネントの概要
ms.date: 03/30/2017
f1_keywords:
- MenuItem
- MainMenu
helpviewer_keywords:
- MainMenu control [Windows Forms], about MainMenu control
- menus
ms.assetid: b41cc5a3-cc59-4996-aa3c-8dd9c17d3c90
ms.openlocfilehash: 8bc35de239429214d6b493b343d1dd6c898f4d37
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745116"
---
# <a name="mainmenu-component-overview-windows-forms"></a>MainMenu コンポーネントの概要 (Windows フォーム)
> [!IMPORTANT]
> <xref:System.Windows.Forms.MenuStrip> と <xref:System.Windows.Forms.ContextMenuStrip> によって、以前のバージョンの <xref:System.Windows.Forms.MainMenu> および <xref:System.Windows.Forms.ContextMenu> のコントロールに置き換えられ、機能が追加されますが、<xref:System.Windows.Forms.MainMenu> と <xref:System.Windows.Forms.ContextMenu> は下位互換性と将来の使用の両方のために保持されます。  
  
 Windows フォーム <xref:System.Windows.Forms.MainMenu> コンポーネントは、実行時にメニューを表示します。 メインメニューのすべてのサブメニューと個々の項目は <xref:System.Windows.Forms.MenuItem> オブジェクトです。  
  
## <a name="key-properties"></a>キー プロパティ  
 メニュー項目を既定の項目として指定するには、[<xref:System.Windows.Forms.MenuItem.DefaultItem%2A>] プロパティを `true`に設定します。 メニューがクリックされると、既定の項目が太字で表示されます。 メニュー項目の <xref:System.Windows.Forms.MenuItem.Checked%2A> プロパティは `true` または `false`のいずれかで、メニュー項目が選択されているかどうかを示します。 メニュー項目の <xref:System.Windows.Forms.MenuItem.RadioCheck%2A> プロパティによって、選択した項目の外観がカスタマイズされます。 <xref:System.Windows.Forms.MenuItem.RadioCheck%2A> が `true`に設定されている場合は、項目の横にラジオボタンが表示されます。<xref:System.Windows.Forms.MenuItem.RadioCheck%2A> が `false`に設定されている場合は、項目の横にチェックマークが表示されます。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.MainMenu>
- <xref:System.Windows.Forms.Menu>
- <xref:System.Windows.Forms.MenuItem>
- <xref:System.Windows.Forms.MenuStrip>
- <xref:System.Windows.Forms.ContextMenuStrip>
- [MenuStrip コントロールの概要](menustrip-control-overview-windows-forms.md)
