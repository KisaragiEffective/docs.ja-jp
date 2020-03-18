---
title: ToolStrip コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- Toolstrip
helpviewer_keywords:
- ToolStrip control [Windows Forms], about ToolStrip control
- toolbars [Windows Forms], what's new in Windows Forms
- toolbars [Windows Forms]
- what's new [Windows Forms], toolbars
ms.assetid: 81d067ed-297c-4dad-90de-1bcac15336ec
ms.openlocfilehash: 931a6a0ea09f9b684b793c05cb1c3db8ee8fb7c7
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741078"
---
# <a name="toolstrip-control-overview-windows-forms"></a>ToolStrip コントロールの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.ToolStrip> コントロールとそれに関連付けられたクラスは、ユーザーインターフェイス要素をツールバー、ステータスバー、およびメニューに結合するための共通のフレームワークを提供します。 <xref:System.Windows.Forms.ToolStrip> コントロールは、埋め込み先でのアクティブ化と編集、カスタムレイアウト、およびラフティング (水平方向または垂直方向のスペースを共有するツールバーの機能) を含む、豊富なデザイン時エクスペリエンスを提供します。  
  
 <xref:System.Windows.Forms.ToolStrip>、以前のバージョンのコントロールに置き換えられ、機能が追加されますが、必要に応じて、下位互換性と将来の使用の両方で <xref:System.Windows.Forms.ToolBar> が保持されます。  
  
## <a name="features-of-the-toolstrip-controls"></a>ToolStrip コントロールの機能  
 <xref:System.Windows.Forms.ToolStrip> コントロールを使用して次の操作を行います。  
  
- コンテナー間で共通のユーザーインターフェイスを提供します。  
  
- ドッキング、ラフティング、テキストとイメージを含むボタン、ドロップダウンボタンとコントロール、オーバーフローボタン、<xref:System.Windows.Forms.ToolStrip> 項目の実行時の並べ替えなど、高度なユーザーインターフェイスとレイアウト機能をサポートする、簡単にカスタマイズされたツールバーを作成します。  
  
- オーバーフローおよび実行時の項目の並べ替えをサポートします。 オーバーフロー機能は、<xref:System.Windows.Forms.ToolStrip>に表示するための十分な領域がない場合に、項目をドロップダウンメニューに移動します。  
  
- 共通のレンダリングモデルを使用して、オペレーティングシステムの一般的な外観と動作をサポートします。  
  
- 他のコントロールのイベントを処理するのと同じ方法で、すべてのコンテナーと含まれる項目に対して一貫したイベントを処理します。  
  
- 項目を1つの <xref:System.Windows.Forms.ToolStrip> から別の <xref:System.Windows.Forms.ToolStrip>内にドラッグします。  
  
- <xref:System.Windows.Forms.ToolStripDropDown>に高度なレイアウトが含まれるドロップダウンコントロールとユーザーインターフェイスの種類のエディターを作成します。  
  
 <xref:System.Windows.Forms.ToolStripControlHost> クラスを使用して、<xref:System.Windows.Forms.ToolStrip> 上の他のコントロールを使用し、それらのコントロールの <xref:System.Windows.Forms.ToolStrip> 機能を取得します。  
  
 機能を拡張したり、<xref:System.Windows.Forms.ToolStripRenderer>、<xref:System.Windows.Forms.ToolStripProfessionalRenderer>、および <xref:System.Windows.Forms.ToolStripManager> と共に、<xref:System.Windows.Forms.ToolStripRenderMode> および <xref:System.Windows.Forms.ToolStripManagerRenderMode> 列挙体と共に使用して、外観と動作を変更することができます。  
  
 <xref:System.Windows.Forms.ToolStrip> コントロールは、高度な構成と拡張が可能であり、さまざまなプロパティ、メソッド、およびイベントを使用して、外観と動作をカスタマイズできます。 注目すべきメンバーを次に示します。  
  
### <a name="important-toolstrip-members"></a>重要な ToolStrip メンバー  
  
|Name|[説明]|  
|----------|-----------------|  
|<xref:System.Windows.Forms.ToolStrip.Dock%2A>|<xref:System.Windows.Forms.ToolStrip> がドッキングされている親コンテナーの端を取得または設定します。|  
|<xref:System.Windows.Forms.ToolStrip.AllowItemReorder%2A>|<xref:System.Windows.Forms.ToolStrip> クラスがドラッグ アンド ドロップおよび項目の並べ替えをプライベートで処理するかどうかを示す値を取得または設定します。|  
|<xref:System.Windows.Forms.ToolStrip.LayoutStyle%2A>|<xref:System.Windows.Forms.ToolStrip> が項目をレイアウトする方法を示す値を取得または設定します。|  
|<xref:System.Windows.Forms.ToolStripItem.Overflow%2A>|<xref:System.Windows.Forms.ToolStripItem> が <xref:System.Windows.Forms.ToolStrip> または <xref:System.Windows.Forms.ToolStripOverflowButton> にアタッチされているか、または2つの間で浮動小数点演算が可能かどうかを取得または設定します。|  
|<xref:System.Windows.Forms.ToolStrip.IsDropDown%2A>|<xref:System.Windows.Forms.ToolStripItem> がクリックされたときに、<xref:System.Windows.Forms.ToolStripItem> がドロップダウンリストに他の項目を表示するかどうかを示す値を取得します。|  
|<xref:System.Windows.Forms.ToolStrip.OverflowButton%2A>|オーバーフローが有効な <xref:System.Windows.Forms.ToolStripItem> のオーバーフロー ボタンである <xref:System.Windows.Forms.ToolStrip> を取得します。|  
|<xref:System.Windows.Forms.ToolStrip.Renderer%2A>|<xref:System.Windows.Forms.ToolStrip>の外観と動作 (ルックアンドフィール) をカスタマイズするために使用される <xref:System.Windows.Forms.ToolStripRenderer> を取得または設定します。|  
|<xref:System.Windows.Forms.ToolStrip.RenderMode%2A>|<xref:System.Windows.Forms.ToolStrip> に適用される描画スタイルを取得または設定します。|  
|<xref:System.Windows.Forms.ToolStrip.RendererChanged>|<xref:System.Windows.Forms.ToolStrip.Renderer%2A> プロパティが変更されたときに発生します。|  
  
 <xref:System.Windows.Forms.ToolStrip> コントロールの柔軟性は、多数のコンパニオンクラスを使用することによって実現されます。 次に、最も注目すべき点をいくつか示します。  
  
### <a name="important-toolstrip-companion-classes"></a>重要な ToolStrip コンパニオンクラス  
  
|Name|[説明]|  
|----------|-----------------|  
|<xref:System.Windows.Forms.MenuStrip>|を置き換えて、<xref:System.Windows.Forms.MainMenu> クラスに機能を追加します。|  
|<xref:System.Windows.Forms.StatusStrip>|を置き換えて、<xref:System.Windows.Forms.StatusBar> クラスに機能を追加します。|  
|<xref:System.Windows.Forms.ContextMenuStrip>|を置き換えて、<xref:System.Windows.Forms.ContextMenu> クラスに機能を追加します。|  
|<xref:System.Windows.Forms.ToolStripItem>|<xref:System.Windows.Forms.ToolStrip>、<xref:System.Windows.Forms.ToolStripControlHost>、または <xref:System.Windows.Forms.ToolStripDropDown> に含めることができるすべての要素のイベントおよびレイアウトを管理する抽象基本クラス。|  
|<xref:System.Windows.Forms.ToolStripContainer>|さまざまな方法でコントロールを配置できるように、フォームの各辺にパネルを備えたコンテナーを提供します。|  
|<xref:System.Windows.Forms.ToolStripRenderer>|<xref:System.Windows.Forms.ToolStrip> オブジェクトの描画機能を処理します。|  
|<xref:System.Windows.Forms.ToolStripProfessionalRenderer>|Microsoft Office スタイルの外観を提供します。|  
|<xref:System.Windows.Forms.ToolStripManager>|<xref:System.Windows.Forms.ToolStrip> のレンダリングとラフティング、および <xref:System.Windows.Forms.MenuStrip>、<xref:System.Windows.Forms.ToolStripDropDownMenu>、<xref:System.Windows.Forms.ToolStripMenuItem> の各オブジェクトのマージを制御します。|  
|<xref:System.Windows.Forms.ToolStripManagerRenderMode>|フォームに含まれる複数の <xref:System.Windows.Forms.ToolStrip> オブジェクトに適用される描画スタイル (カスタム、Windows XP、または Microsoft Office Professional) を指定します。|  
|<xref:System.Windows.Forms.ToolStripRenderMode>|フォームに含まれる1つの <xref:System.Windows.Forms.ToolStrip> オブジェクトに適用される描画スタイル (カスタム、Windows XP、または Microsoft Office Professional) を指定します。|  
|<xref:System.Windows.Forms.ToolStripControlHost>|特に <xref:System.Windows.Forms.ToolStrip> コントロールではなく、<xref:System.Windows.Forms.ToolStrip> の機能を必要とする他のコントロールをホストします。|  
|<xref:System.Windows.Forms.ToolStripItemPlacement>|<xref:System.Windows.Forms.ToolStripItem> がメイン <xref:System.Windows.Forms.ToolStrip>にレイアウトされるか、オーバーフロー <xref:System.Windows.Forms.ToolStrip>に配置されるか、またはそのどちらでもないかを指定します。|  
  
 詳細については、「 [Toolstrip テクノロジの概要](toolstrip-technology-summary.md)」と「 [toolstrip コントロールアーキテクチャ](toolstrip-control-architecture.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ToolStrip>
- <xref:System.Windows.Forms.MenuStrip>
- <xref:System.Windows.Forms.ContextMenuStrip>
- <xref:System.Windows.Forms.StatusStrip>
- <xref:System.Windows.Forms.ToolStripItem>
- <xref:System.Windows.Forms.ToolStripDropDown>
