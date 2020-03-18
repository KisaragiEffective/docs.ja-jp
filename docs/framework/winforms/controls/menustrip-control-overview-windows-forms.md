---
title: MenuStrip コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- MenuStrip
helpviewer_keywords:
- MenuStrip control [Windows Forms], about MenuStrip control
- menus [Windows Forms], creating
ms.assetid: f45516e5-bf01-4468-b851-d45f4c33c055
ms.openlocfilehash: a536d13cb7be3f4e4e4a085e1a4da1b0899b3a0c
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76734470"
---
# <a name="menustrip-control-overview-windows-forms"></a>MenuStrip コントロールの概要 (Windows フォーム)
メニューは、共通のテーマによってグループ化されたコマンドを保持することによって、ユーザーに機能を公開します。  
  
 <xref:System.Windows.Forms.MenuStrip> コントロールは、.NET Framework のバージョン2.0 で導入されました。 <xref:System.Windows.Forms.MenuStrip> コントロールを使用すると、Microsoft Office にあるようなメニューを簡単に作成できます。  
  
 <xref:System.Windows.Forms.MenuStrip> コントロールでは、マルチドキュメントインターフェイス (MDI) とメニューのマージ、ツールヒント、およびオーバーフローがサポートされています。 アクセスキー、ショートカットキー、チェックマーク、画像、および区分線を追加することで、メニューの使いやすさと読みやすさを向上させることができます。  
  
 <xref:System.Windows.Forms.MenuStrip> コントロールは、<xref:System.Windows.Forms.MainMenu> コントロールに置き換えられ、機能が追加されます。ただし、<xref:System.Windows.Forms.MainMenu> コントロールは、旧バージョンとの互換性を維持するために残されています。  
  
## <a name="ways-to-use-the-menustrip-control"></a>MenuStrip コントロールを使用する方法  
 <xref:System.Windows.Forms.MenuStrip> コントロールを使用して次の操作を行います。  
  
- テキストとイメージの順序付けと配置、ドラッグアンドドロップ操作、MDI、オーバーフロー、メニューコマンドにアクセスするための代替モードなど、高度なユーザーインターフェイスとレイアウト機能をサポートする、カスタマイズされた、一般的に使用されるメニューを作成します。  
  
- オペレーティングシステムの一般的な外観と動作をサポートします。  
  
- 他のコントロールのイベントを処理するのと同じ方法で、すべてのコンテナーと含まれる項目に対して一貫したイベントを処理します。  
  
 次の表は、<xref:System.Windows.Forms.MenuStrip> と関連クラスのいくつかの重要なプロパティを示しています。  
  
|プロパティ|[説明]|  
|--------------|-----------------|  
|<xref:System.Windows.Forms.MenuStrip.MdiWindowListItem%2A>|MDI 子フォームの一覧を表示するために使用される <xref:System.Windows.Forms.ToolStripMenuItem> を取得または設定します。|  
|<xref:System.Windows.Forms.ToolStripItem.MergeAction%2A?displayProperty=nameWithType>|MDI アプリケーションの親メニューと子メニューをマージする方法を取得または設定します。|  
|<xref:System.Windows.Forms.ToolStripItem.MergeIndex%2A?displayProperty=nameWithType>|MDI アプリケーションのメニュー内のマージされた項目の位置を取得または設定します。|  
|<xref:System.Windows.Forms.Form.IsMdiContainer%2A?displayProperty=nameWithType>|フォームが MDI 子フォームのコンテナーかどうかを示す値を取得または設定します。|  
|<xref:System.Windows.Forms.MenuStrip.ShowItemToolTips%2A>|<xref:System.Windows.Forms.MenuStrip>にツールヒントを表示するかどうかを示す値を取得または設定します。|  
|<xref:System.Windows.Forms.MenuStrip.CanOverflow%2A>|<xref:System.Windows.Forms.MenuStrip> がオーバーフロー機能をサポートするかどうかを示す値を取得または設定します。|  
|<xref:System.Windows.Forms.ToolStripMenuItem.ShortcutKeys%2A>|<xref:System.Windows.Forms.ToolStripMenuItem> に関連付けられたショートカット キーを取得または設定します。|  
|<xref:System.Windows.Forms.ToolStripMenuItem.ShowShortcutKeys%2A>|<xref:System.Windows.Forms.ToolStripMenuItem> に関連付けられたショートカット キーを <xref:System.Windows.Forms.ToolStripMenuItem> の横に表示するかどうかを示す値を取得または設定します。|  
  
 次の表は、重要な <xref:System.Windows.Forms.MenuStrip> コンパニオンクラスを示しています。  
  
|クラス|[説明]|  
|-----------|-----------------|  
|<xref:System.Windows.Forms.ToolStripMenuItem>|<xref:System.Windows.Forms.MenuStrip> または <xref:System.Windows.Forms.ContextMenuStrip> に表示される選択可能なオプションを表します。|  
|<xref:System.Windows.Forms.ContextMenuStrip>|ショートカット メニューを表します。|  
|<xref:System.Windows.Forms.ToolStripDropDown>|ユーザーが <xref:System.Windows.Forms.ToolStripDropDownButton> または上位レベルのメニュー項目をクリックしたときに表示される一覧から1つの項目を選択できるようにするコントロールを表します。|  
|<xref:System.Windows.Forms.ToolStripDropDownItem>|クリックしたときにドロップダウン項目を表示する <xref:System.Windows.Forms.ToolStripItem> から派生したコントロールの基本機能を提供します。|  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ToolStrip>
- <xref:System.Windows.Forms.MenuStrip>
- <xref:System.Windows.Forms.ContextMenuStrip>
- <xref:System.Windows.Forms.StatusStrip>
- <xref:System.Windows.Forms.ToolStripItem>
- <xref:System.Windows.Forms.ToolStripDropDown>
