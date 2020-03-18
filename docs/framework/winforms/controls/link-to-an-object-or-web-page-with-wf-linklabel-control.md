---
title: LinkLabel コントロールを使用してオブジェクトまたは Web ページにリンクする
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- examples [Windows Forms], LinkLabel control
- Windows Forms, linking to objects
- Web page link control
- linking [Windows Forms], to other forms
- Windows Forms, linking to Web pages
- links [Windows Forms], to other forms
- LinkLabel control [Windows Forms], linking to object or Web page
- LinkLabel control [Windows Forms], examples
ms.assetid: 6c91c975-3cb7-4504-82f0-fc6255f8fb85
ms.openlocfilehash: 1669a9d6aba39b02d228c735701ca4e31c8f8291
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745206"
---
# <a name="how-to-link-to-an-object-or-web-page-with-the-windows-forms-linklabel-control"></a>方法 : Windows フォーム LinkLabel コントロールでオブジェクトまたは Web ページにリンクする

Windows フォーム <xref:System.Windows.Forms.LinkLabel> コントロールを使用すると、フォーム上に Web スタイルのリンクを作成できます。 リンクをクリックすると、その色を変更してリンクがアクセスされたことを示すことができます。 色の変更の詳細については、「[方法: Windows フォーム LinkLabel コントロールの外観を変更する](how-to-change-the-appearance-of-the-windows-forms-linklabel-control.md)」を参照してください。

## <a name="linking-to-another-form"></a>リンク (別のフォームに)

#### <a name="to-link-to-another-form-with-a-linklabel-control"></a>LinkLabel コントロールを使用して別のフォームにリンクするには

1. <xref:System.Windows.Forms.LinkLabel.Text%2A> プロパティを適切なキャプションに設定します。

2. キャプションのどの部分をリンクとして示すかを決定するには、<xref:System.Windows.Forms.LinkLabel.LinkArea%2A> プロパティを設定します。 どのように表示されるかは、リンクラベルの外観に関連するプロパティによって異なります。 <xref:System.Windows.Forms.LinkLabel.LinkArea%2A> 値は、2つの数値、開始文字位置、および文字数を含む <xref:System.Windows.Forms.LinkLabel.LinkArea%2A> オブジェクトで表されます。 <xref:System.Windows.Forms.LinkLabel.LinkArea%2A> プロパティは、次のような方法でプロパティウィンドウまたはコードで設定できます。

    ```vb
    ' In this code example, the link area has been set to begin
    ' at the first character and extend for eight characters.
    ' You may need to modify this based on the text entered in Step 1.
    LinkLabel1.LinkArea = New LinkArea(0, 8)
    ```

    ```csharp
    // In this code example, the link area has been set to begin
    // at the first character and extend for eight characters.
    // You may need to modify this based on the text entered in Step 1.
    linkLabel1.LinkArea = new LinkArea(0,8);
    ```

    ```cpp
    // In this code example, the link area has been set to begin
    // at the first character and extend for eight characters.
    // You may need to modify this based on the text entered in Step 1.
    linkLabel1->LinkArea = LinkArea(0,8);
    ```

3. <xref:System.Windows.Forms.LinkLabel.LinkClicked> イベントハンドラーで <xref:System.Windows.Forms.Form.Show%2A> メソッドを呼び出して、プロジェクト内の別のフォームを開き、<xref:System.Windows.Forms.LinkLabel.LinkVisited%2A> プロパティを `true`に設定します。

    > [!NOTE]
    > <xref:System.Windows.Forms.LinkLabelLinkClickedEventArgs> クラスのインスタンスは、クリックされた <xref:System.Windows.Forms.LinkLabel> コントロールへの参照を保持しているため、`sender` オブジェクトをキャストする必要はありません。

    ```vb
    Protected Sub LinkLabel1_LinkClicked(ByVal Sender As System.Object, _
       ByVal e As System.Windows.Forms.LinkLabelLinkClickedEventArgs) _
       Handles LinkLabel1.LinkClicked
       ' Show another form.
       Dim f2 As New Form()
       f2.Show
       LinkLabel1.LinkVisited = True
    End Sub
    ```

    ```csharp
    protected void linkLabel1_LinkClicked(object sender, System. Windows.Forms.LinkLabelLinkClickedEventArgs e)
    {
       // Show another form.
       Form f2 = new Form();
       f2.Show();
       linkLabel1.LinkVisited = true;
    }
    ```

    ```cpp
    private:
       void linkLabel1_LinkClicked(System::Object ^  sender,
          System::Windows::Forms::LinkLabelLinkClickedEventArgs ^  e)
       {
          // Show another form.
          Form ^ f2 = new Form();
          f2->Show();
          linkLabel1->LinkVisited = true;
       }
    ```

## <a name="linking-to-a-web-page"></a>Web ページへのリンク

<xref:System.Windows.Forms.LinkLabel> コントロールを使用して、既定のブラウザーで Web ページを表示することもできます。

#### <a name="to-start-internet-explorer-and-link-to-a-web-page-with-a-linklabel-control"></a>Internet Explorer を起動し、LinkLabel コントロールを使用して Web ページにリンクするには

1. <xref:System.Windows.Forms.LinkLabel.Text%2A> プロパティを適切なキャプションに設定します。

2. キャプションのどの部分をリンクとして示すかを決定するには、<xref:System.Windows.Forms.LinkLabel.LinkArea%2A> プロパティを設定します。

3. <xref:System.Windows.Forms.LinkLabel.LinkClicked> イベントハンドラーで、例外処理ブロックの発生時に、2番目のプロシージャを呼び出します。このプロシージャは、<xref:System.Windows.Forms.LinkLabel.LinkVisited%2A> プロパティを `true` に設定し、<xref:System.Diagnostics.Process.Start%2A> メソッドを使用して URL で既定のブラウザーを起動します。 <xref:System.Diagnostics.Process.Start%2A> メソッドを使用するには、<xref:System.Diagnostics?displayProperty=nameWithType> 名前空間への参照を追加する必要があります。

    > [!IMPORTANT]
    > 次のコードが部分信頼環境で実行されている場合 (共有ドライブなど)、`VisitLink` メソッドが呼び出されると JIT コンパイラは失敗します。 `System.Diagnostics.Process.Start` ステートメントを実行すると、リンク確認要求が失敗します。 `VisitLink` メソッドが呼び出されたときに例外をキャッチすることにより、次のコードは JIT コンパイラが失敗した場合にエラーが適切に処理されることを確認します。

    ```vb
    Private Sub LinkLabel1_LinkClicked(ByVal sender As System.Object, _
       ByVal e As System.Windows.Forms.LinkLabelLinkClickedEventArgs) _
       Handles LinkLabel1.LinkClicked
       Try
          VisitLink()
       Catch ex As Exception
          ' The error message
          MessageBox.Show("Unable to open link that was clicked.")
       End Try
    End Sub

    Sub VisitLink()
       ' Change the color of the link text by setting LinkVisited
       ' to True.
       LinkLabel1.LinkVisited = True
       ' Call the Process.Start method to open the default browser
       ' with a URL:
       System.Diagnostics.Process.Start("http://www.microsoft.com")
    End Sub
    ```

    ```csharp
    private void linkLabel1_LinkClicked(object sender, System.Windows.Forms.LinkLabelLinkClickedEventArgs e)
    {
       try
       {
          VisitLink();
       }
       catch (Exception ex )
       {
          MessageBox.Show("Unable to open link that was clicked.");
       }
    }

    private void VisitLink()
    {
       // Change the color of the link text by setting LinkVisited
       // to true.
       linkLabel1.LinkVisited = true;
       //Call the Process.Start method to open the default browser
       //with a URL:
       System.Diagnostics.Process.Start("http://www.microsoft.com");
    }
    ```

    ```cpp
    private:
       void linkLabel1_LinkClicked(System::Object ^  sender,
          System::Windows::Forms::LinkLabelLinkClickedEventArgs ^  e)
       {
          try
          {
             VisitLink();
          }
          catch (Exception ^ ex)
          {
             MessageBox::Show("Unable to open link that was clicked.");
          }
       }
    private:
       void VisitLink()
       {
          // Change the color of the link text by setting LinkVisited
          // to true.
          linkLabel1->LinkVisited = true;
          // Call the Process.Start method to open the default browser
          // with a URL:
          System::Diagnostics::Process::Start("http://www.microsoft.com");
       }
    ```

## <a name="see-also"></a>参照

- <xref:System.Diagnostics.Process.Start%2A?displayProperty=nameWithType>
- [LinkLabel コントロールの概要](linklabel-control-overview-windows-forms.md)
- [方法: Windows フォーム LinkLabel コントロールの表示形式を変更する](how-to-change-the-appearance-of-the-windows-forms-linklabel-control.md)
- [LinkLabel コントロール](linklabel-control-windows-forms.md)
