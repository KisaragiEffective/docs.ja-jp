---
title: TabControl コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- TabControl
helpviewer_keywords:
- TabControl control [Windows Forms], about TabControl control
- tab pages [Windows Forms], about tab pages
- property pages [Windows Forms], Windows Forms
- Windows Forms dialog boxes [Windows Forms], tabs
ms.assetid: 2b4ea784-a39d-463c-81d8-af74ce068476
ms.openlocfilehash: 2668169488b4087673fbf2552650c23cda780642
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742832"
---
# <a name="tabcontrol-control-overview-windows-forms"></a>TabControl コントロールの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.TabControl> は、ノートの仕切りや書類キャビネットのフォルダー セットのラベルに似た、複数のタブを表示します。 タブには画像やその他のコントロールを含めることができます。 タブコントロールを使用すると、コントロールパネルの表示プロパティなど、Windows オペレーティングシステムのさまざまな場所に表示される複数ページのダイアログボックスの種類を生成できます。 また、<xref:System.Windows.Forms.TabControl> を使用して、関連するプロパティのグループを設定するために使用されるプロパティページを作成することもできます。  
  
## <a name="working-with-tabcontrol"></a>TabControl の操作  
 <xref:System.Windows.Forms.TabControl> の最も重要なプロパティは <xref:System.Windows.Forms.TabControl.TabPages%2A>であり、個々のタブが含まれています。 個々のタブは <xref:System.Windows.Forms.TabPage> オブジェクトです。 タブをクリックすると、その <xref:System.Windows.Forms.TabPage> オブジェクトの <xref:System.Windows.Forms.Control.Click> イベントが発生します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.TabControl>
- [TabControl コントロール](tabcontrol-control-windows-forms.md)
- [方法: Windows フォーム TabControl の表示形式を変更する](how-to-change-the-appearance-of-the-windows-forms-tabcontrol.md)
- [方法: タブ ページにコントロールを追加する](how-to-add-a-control-to-a-tab-page.md)
- [方法: Windows フォーム TabControl のタブを追加および削除する](how-to-add-and-remove-tabs-with-the-windows-forms-tabcontrol.md)
- [方法: タブ ページを無効化する](how-to-disable-tab-pages.md)
- [Windows フォームのダイアログ ボックス](../dialog-boxes-in-windows-forms.md)
