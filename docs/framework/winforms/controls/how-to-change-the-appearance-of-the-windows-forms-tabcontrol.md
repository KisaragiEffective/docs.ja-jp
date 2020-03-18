---
title: TabControl の外観を変更する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- icons [Windows Forms], displaying on tabs
- TabControl control [Windows Forms], changing page appearance
- tabs [Windows Forms], controlling appearance
- buttons [Windows Forms], displaying tabs as
ms.assetid: 7c6cc443-ed62-4d26-b94d-b8913b44f773
ms.openlocfilehash: 056070177e6bbaba0c93c7b94f5adfd7887be6a8
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746615"
---
# <a name="how-to-change-the-appearance-of-the-windows-forms-tabcontrol"></a>方法 : Windows フォーム TabControl の表示形式を変更する
Windows フォームのタブの外観を変更するには、<xref:System.Windows.Forms.TabControl> のプロパティと、コントロールの各タブを構成する <xref:System.Windows.Forms.TabPage> オブジェクトを使用します。 これらのプロパティを設定することにより、タブに画像を表示し、水平方向ではなく垂直方向にタブを表示し、タブの複数行を表示し、プログラムによってタブを有効または無効にすることができます。  
  
### <a name="to-display-an-icon-on-the-label-part-of-a-tab"></a>タブのラベル部分にアイコンを表示するには  
  
1. フォームに <xref:System.Windows.Forms.ImageList> コントロールを追加します。  
  
2. イメージリストにイメージを追加します。  
  
     イメージリストの詳細については、「 [Imagelist コンポーネント](imagelist-component-windows-forms.md)」および「[方法: Windows フォーム imagelist コンポーネントを使用してイメージを追加または削除する](how-to-add-or-remove-images-with-the-windows-forms-imagelist-component.md)」を参照してください。  
  
3. <xref:System.Windows.Forms.TabControl> の <xref:System.Windows.Forms.TabControl.ImageList%2A> プロパティを <xref:System.Windows.Forms.ImageList> コントロールに設定します。  
  
4. <xref:System.Windows.Forms.TabPage> の [<xref:System.Windows.Forms.TabPage.ImageIndex%2A>] プロパティを、一覧内の適切なイメージのインデックスに設定します。  
  
### <a name="to-create-multiple-rows-of-tabs"></a>複数行のタブを作成するには  
  
1. 必要なタブページの数を追加します。  
  
2. <xref:System.Windows.Forms.TabControl> の <xref:System.Windows.Forms.TabControl.Multiline%2A> プロパティを `true`に設定します。  
  
3. タブが複数の行にまだ表示されていない場合は、<xref:System.Windows.Forms.TabControl> の [<xref:System.Windows.Forms.Control.Width%2A>] プロパティを、すべてのタブよりも狭くするように設定します。  
  
### <a name="to-arrange-tabs-on-the-side-of-the-control"></a>コントロールの横にタブを配置するには  
  
- <xref:System.Windows.Forms.TabControl> の <xref:System.Windows.Forms.TabControl.Alignment%2A> プロパティを <xref:System.Windows.Forms.TabAlignment.Left> または <xref:System.Windows.Forms.TabAlignment.Right>に設定します。  
  
### <a name="to-programmatically-enable-or-disable-all-controls-on-a-tab"></a>タブのすべてのコントロールをプログラムで有効または無効にするには  
  
1. <xref:System.Windows.Forms.TabPage> の <xref:System.Windows.Forms.TabPage.Enabled%2A> プロパティを `true` または `false`に設定します。  
  
    ```vb  
    TabPage1.Enabled = False  
    ```  
  
    ```csharp  
    tabPage1.Enabled = false;  
    ```  
  
    ```cpp  
    tabPage1->Enabled = false;  
    ```  
  
### <a name="to-display-tabs-as-buttons"></a>タブをボタンとして表示するには  
  
- <xref:System.Windows.Forms.TabControl> の <xref:System.Windows.Forms.TabControl.Appearance%2A> プロパティを <xref:System.Windows.Forms.TabAppearance.Buttons> または <xref:System.Windows.Forms.TabAppearance.FlatButtons>に設定します。  
  
## <a name="see-also"></a>参照

- [TabControl コントロール](tabcontrol-control-windows-forms.md)
- [TabControl コントロールの概要](tabcontrol-control-overview-windows-forms.md)
- [方法: タブ ページにコントロールを追加する](how-to-add-a-control-to-a-tab-page.md)
- [方法: タブ ページを無効化する](how-to-disable-tab-pages.md)
- [方法: Windows フォーム TabControl のタブを追加および削除する](how-to-add-and-remove-tabs-with-the-windows-forms-tabcontrol.md)
