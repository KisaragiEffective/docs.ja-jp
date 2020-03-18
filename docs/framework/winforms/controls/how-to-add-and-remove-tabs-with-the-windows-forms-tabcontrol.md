---
title: TabControl を使用してタブを追加および削除する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- tabs [Windows Forms], removing from pages
- TabPage control
- TabControl control [Windows Forms], adding and removing tabs
- tabs [Windows Forms], adding to pages
- tab pages
ms.assetid: 66d4dfca-41e8-44e3-9c80-fb7ac4cb1619
ms.openlocfilehash: 8292d8441f9b47334b98736cf3282c846673dbb4
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76732719"
---
# <a name="how-to-add-and-remove-tabs-with-the-windows-forms-tabcontrol"></a>方法 : Windows フォーム TabControl のタブを追加および削除する
既定では、<xref:System.Windows.Forms.TabControl> コントロールには、2つの <xref:System.Windows.Forms.TabPage> コントロールが含まれています。 これらのタブには、<xref:System.Windows.Forms.TabControl.TabPages%2A> プロパティを使用してアクセスできます。  
  
### <a name="to-add-a-tab-programmatically"></a>プログラムによってタブを追加するには  
  
- <xref:System.Windows.Forms.TabControl.TabPages%2A> プロパティの <xref:System.Windows.Forms.TabControl.TabPageCollection.Add%2A> メソッドを使用します。  
  
    ```vb  
    Dim myTabPage As New TabPage()  
    myTabPage.Text = "TabPage" & (TabControl1.TabPages.Count + 1)  
    TabControl1.TabPages.Add(myTabPage)  
    ```  
  
    ```csharp  
    string title = "TabPage " + (tabControl1.TabCount + 1).ToString();  
    TabPage myTabPage = new TabPage(title);  
    tabControl1.TabPages.Add(myTabPage);  
    ```  
  
    ```cpp  
    String^ title = String::Concat("TabPage ",  
       (tabControl1->TabCount + 1).ToString());  
    TabPage^ myTabPage = gcnew TabPage(title);  
    tabControl1->TabPages->Add(myTabPage);  
    ```  
  
### <a name="to-remove-a-tab-programmatically"></a>プログラムによってタブを削除するには  
  
- 選択したタブを削除するには、<xref:System.Windows.Forms.TabControl.TabPages%2A> プロパティの <xref:System.Windows.Forms.TabControl.TabPageCollection.Remove%2A> メソッドを使用します。  
  
     または  
  
- すべてのタブを削除するには、<xref:System.Windows.Forms.TabControl.TabPages%2A> プロパティの <xref:System.Windows.Forms.TabControl.TabPageCollection.Clear%2A> メソッドを使用します。  
  
    ```vb  
    ' Removes the selected tab:  
    TabControl1.TabPages.Remove(TabControl1.SelectedTab)  
    ' Removes all the tabs:  
    TabControl1.TabPages.Clear()  
    ```  
  
    ```csharp  
    // Removes the selected tab:  
    tabControl1.TabPages.Remove(tabControl1.SelectedTab);  
    // Removes all the tabs:  
    tabControl1.TabPages.Clear();  
    ```  
  
    ```cpp  
    // Removes the selected tab:  
    tabControl1->TabPages->Remove(tabControl1->SelectedTab);  
    // Removes all the tabs:  
    tabControl1->TabPages->Clear();  
    ```  
  
## <a name="see-also"></a>参照

- [TabControl コントロールの概要](tabcontrol-control-overview-windows-forms.md)
- [方法: タブ ページにコントロールを追加する](how-to-add-a-control-to-a-tab-page.md)
- [方法: タブ ページを無効化する](how-to-disable-tab-pages.md)
- [方法: Windows フォーム TabControl の表示形式を変更する](how-to-change-the-appearance-of-the-windows-forms-tabcontrol.md)
