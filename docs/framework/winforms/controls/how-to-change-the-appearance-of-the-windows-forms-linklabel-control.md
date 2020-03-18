---
title: LinkLabel コントロールの外観を変更する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- LinkLabel properties
- LinkLabel control [Windows Forms], changing appearance of links
- links [Windows Forms], changing appearance
- examples [Windows Forms], LinkLabel control
- LinkLabel control [Windows Forms], examples
ms.assetid: fdc5854f-5162-4457-8cbe-1042feb2d132
ms.openlocfilehash: 0b38722fb1647ea215c3bb8978dd3f54b300a0e0
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746619"
---
# <a name="how-to-change-the-appearance-of-the-windows-forms-linklabel-control"></a>方法 : Windows フォーム LinkLabel コントロールの表示形式を変更する
さまざまな目的に合わせて、<xref:System.Windows.Forms.LinkLabel> コントロールによって表示されるテキストを変更することができます。 たとえば、テキストを特定の色に下線付きで表示するように設定することによって、テキストをクリックできることをユーザーに示すのが一般的です。 ユーザーがテキストをクリックすると、色が別の色に変わります。 この動作を制御するには、<xref:System.Windows.Forms.LinkLabel.LinkBehavior%2A>、<xref:System.Windows.Forms.LinkLabel.LinkArea%2A>、<xref:System.Windows.Forms.LinkLabel.LinkColor%2A>、<xref:System.Windows.Forms.LinkLabel.VisitedLinkColor%2A>、および <xref:System.Windows.Forms.LinkLabel.LinkVisited%2A> の各プロパティの5つのプロパティを設定します。  
  
### <a name="to-change-the-appearance-of-a-linklabel-control"></a>LinkLabel コントロールの外観を変更するには  
  
1. <xref:System.Windows.Forms.LinkLabel.LinkColor%2A> と <xref:System.Windows.Forms.LinkLabel.VisitedLinkColor%2A> のプロパティを、必要な色に設定します。  
  
     これは、プログラムによって、または **[プロパティ]** ウィンドウでデザイン時に行うことができます。  
  
    ```vb  
    ' You can set the color using decimal values for red, green, and blue  
    LinkLabel1.LinkColor = Color.FromArgb(0, 0, 255)  
    ' Or you can set the color using defined constants  
    LinkLabel1.VisitedLinkColor = Color.Purple  
    ```  
  
    ```csharp  
    // You can set the color using decimal values for red, green, and blue  
    linkLabel1.LinkColor = Color.FromArgb(0, 0, 255);  
    // Or you can set the color using defined constants  
    linkLabel1.VisitedLinkColor = Color.Purple;  
    ```  
  
    ```cpp  
    // You can set the color using decimal values for red, green, and blue  
    linkLabel1->LinkColor = Color::FromArgb(0, 0, 255);  
    // Or you can set the color using defined constants  
    linkLabel1->VisitedLinkColor = Color::Purple;  
    ```  
  
2. <xref:System.Windows.Forms.LinkLabel.Text%2A> プロパティを適切なキャプションに設定します。  
  
     これは、プログラムによって、または **[プロパティ]** ウィンドウでデザイン時に行うことができます。  
  
    ```vb  
    LinkLabel1.Text = "Click here to see more."  
    ```  
  
    ```csharp  
    linkLabel1.Text = "Click here to see more.";  
    ```  
  
    ```cpp  
    linkLabel1->Text = "Click here to see more.";  
    ```  
  
3. キャプションのどの部分をリンクとして示すかを決定するには、<xref:System.Windows.Forms.LinkLabel.LinkArea%2A> プロパティを設定します。  
  
     <xref:System.Windows.Forms.LinkLabel.LinkArea%2A> 値は、2つの数値、開始文字の位置、および文字数を含む <xref:System.Windows.Forms.LinkArea> で表されます。 これは、プログラムによって、または **[プロパティ]** ウィンドウでデザイン時に行うことができます。  
  
    ```vb  
    LinkLabel1.LinkArea = new LinkArea(6,4)  
    ```  
  
    ```csharp  
    linkLabel1.LinkArea = new LinkArea(6,4);  
    ```  
  
    ```cpp  
    linkLabel1->LinkArea = LinkArea(6,4);  
    ```  
  
4. <xref:System.Windows.Forms.LinkLabel.LinkBehavior%2A> プロパティを <xref:System.Windows.Forms.LinkBehavior.AlwaysUnderline>、<xref:System.Windows.Forms.LinkBehavior.HoverUnderline>、または <xref:System.Windows.Forms.LinkBehavior.NeverUnderline>に設定します。  
  
     <xref:System.Windows.Forms.LinkBehavior.HoverUnderline>に設定されている場合、<xref:System.Windows.Forms.LinkLabel.LinkArea%2A> によって決定されるキャプションの部分には、ポインターがその上にあるときにのみ下線が引かれます。  
  
5. <xref:System.Windows.Forms.LinkLabel.LinkClicked> イベントハンドラーで、<xref:System.Windows.Forms.LinkLabel.LinkVisited%2A> プロパティを `true`に設定します。  
  
     リンクが参照されている場合は、通常は色によって、何らかの形で表示を変更します。 テキストは、<xref:System.Windows.Forms.LinkLabel.VisitedLinkColor%2A> プロパティによって指定された色に変更されます。  
  
    ```vb  
    Protected Sub LinkLabel1_LinkClicked (ByVal sender As Object, _  
       ByVal e As EventArgs) Handles LinkLabel1.LinkClicked  
       ' Change the color of the link text  
       ' by setting LinkVisited to True.  
       LinkLabel1.LinkVisited = True  
       ' Then do whatever other action is appropriate  
    End Sub  
    ```  
  
    ```csharp  
    protected void LinkLabel1_LinkClicked(object sender, System.EventArgs e)  
    {  
       // Change the color of the link text by setting LinkVisited   
       // to True.  
       linkLabel1.LinkVisited = true;  
       // Then do whatever other action is appropriate  
    }  
    ```  
  
    ```cpp  
    private:  
       System::Void linkLabel1_LinkClicked(System::Object ^  sender,  
          System::Windows::Forms::LinkLabelLinkClickedEventArgs ^  e)  
       {  
          // Change the color of the link text by setting LinkVisited   
          // to True.  
          linkLabel1->LinkVisited = true;  
          // Then do whatever other action is appropriate  
       }  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.LinkLabel.LinkArea%2A>
- <xref:System.Windows.Forms.LinkLabel.LinkColor%2A>
- <xref:System.Windows.Forms.LinkLabel.VisitedLinkColor%2A>
- <xref:System.Windows.Forms.LinkLabel.LinkVisited%2A>
- [LinkLabel コントロールの概要](linklabel-control-overview-windows-forms.md)
- [方法: Windows フォーム LinkLabel コントロールでオブジェクトまたは Web ページにリンクする](link-to-an-object-or-web-page-with-wf-linklabel-control.md)
- [LinkLabel コントロール](linklabel-control-windows-forms.md)
