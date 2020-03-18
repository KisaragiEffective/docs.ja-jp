---
title: '方法 : SaveFileDialog コンポーネントを使用してファイルを保存する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- saving files
- SaveFileDialog component [Windows Forms], saving files
- files [Windows Forms], saving
- OpenFile method [Windows Forms], saving files with SaveFileDialog component
ms.assetid: 02e8f409-b83f-4707-babb-e71f6b223d90
ms.openlocfilehash: 32de7f7e38195271e179d4fae3884b7a39f37c45
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76868084"
---
# <a name="how-to-save-files-using-the-savefiledialog-component"></a>方法 : SaveFileDialog コンポーネントを使用してファイルを保存する

<xref:System.Windows.Forms.SaveFileDialog> コンポーネントを使用すると、ファイルシステムを参照したり、保存するファイルを選択したりできます。 このダイアログ ボックスは、ユーザーがダイアログ ボックス内で選択したファイルのパスと名前を返します。 ただし、ファイルを実際にディスクに書き込むためのコードを記述する必要があります。

### <a name="to-save-a-file-using-the-savefiledialog-component"></a>SaveFileDialog コンポーネントを使用してファイルを保存するには

- **[ファイルの保存]** ダイアログ ボックスを表示し、ユーザーによって選択されたファイルを保存するメソッドを呼び出します。

  <xref:System.Windows.Forms.SaveFileDialog> コンポーネントの <xref:System.Windows.Forms.SaveFileDialog.OpenFile%2A> メソッドを使用して、ファイルを保存します。 このメソッドは、書き込み可能な <xref:System.IO.Stream> オブジェクトを提供します。

  次の例では、<xref:System.Windows.Forms.DialogResult> プロパティを使用してファイルの名前を取得し、<xref:System.Windows.Forms.OpenFileDialog.OpenFile%2A> メソッドを使用してファイルを保存します。 <xref:System.Windows.Forms.SaveFileDialog.OpenFile%2A> メソッドは、ファイルを書き込むストリームを提供します。

  次の例では、イメージが割り当てられた <xref:System.Windows.Forms.Button> コントロールがあります。 このボタンをクリックすると、<xref:System.Windows.Forms.SaveFileDialog> コンポーネントが、.gif、.jpeg、.bmp の種類のファイルを許可するフィルターでインスタンス化されます。 [ファイルの保存] ダイアログ ボックスでこれらの種類のファイルが選択されると、ボタンのイメージが保存されます。

  > [!IMPORTANT]
  > <xref:System.Windows.Forms.FileDialog.FileName%2A> プロパティを取得または設定するには、アセンブリに <xref:System.Security.Permissions.FileIOPermission?displayProperty=nameWithType> クラスによって付与された特権レベルが必要です。 部分的に信頼されたコンテキストで実行している場合、プロセスは、特権がないために例外をスローする可能性があります。 詳細については、「 [Code Access Security Basics](../../misc/code-access-security-basics.md)」を参照してください。

  この例では、フォームに <xref:System.Windows.Forms.ButtonBase.Image%2A> プロパティが .gif、.jpeg、または .bmp のファイルに設定された <xref:System.Windows.Forms.Button> コントロールがあることを前提としています。

  > [!NOTE]
  > <xref:System.Windows.Forms.FileDialog> クラスの <xref:System.Windows.Forms.FileDialog.FilterIndex%2A> プロパティ (継承が原因で、<xref:System.Windows.Forms.SaveFileDialog> クラスの一部である) は、1から始まるインデックスを使用します。 これは、ファイルをバイナリ形式ではなくプレーン テキストで保存する場合など、データを特定の形式で保存するコードを記述する場合に重要です。 このプロパティは、次のコード例に示されています。

  ```vb
  Private Sub Button2_Click(ByVal sender As System.Object, _
  ByVal e As System.EventArgs) Handles Button2.Click
      ' Displays a SaveFileDialog so the user can save the Image
      ' assigned to Button2.
      Dim saveFileDialog1 As New SaveFileDialog()
      saveFileDialog1.Filter = "JPeg Image|*.jpg|Bitmap Image|*.bmp|Gif Image|*.gif"
      saveFileDialog1.Title = "Save an Image File"
      saveFileDialog1.ShowDialog()

      ' If the file name is not an empty string open it for saving.
      If saveFileDialog1.FileName <> "" Then
        ' Saves the Image via a FileStream created by the OpenFile method.
        Dim fs As System.IO.FileStream = Ctype _
            (saveFileDialog1.OpenFile(), System.IO.FileStream)
        ' Saves the Image in the appropriate ImageFormat based upon the
        ' file type selected in the dialog box.
        ' NOTE that the FilterIndex property is one-based.
        Select Case saveFileDialog1.FilterIndex
            Case 1
              Me.button2.Image.Save(fs, _
                  System.Drawing.Imaging.ImageFormat.Jpeg)

            Case 2
              Me.button2.Image.Save(fs, _
                  System.Drawing.Imaging.ImageFormat.Bmp)

            Case 3
              Me.button2.Image.Save(fs, _
                  System.Drawing.Imaging.ImageFormat.Gif)
          End Select

          fs.Close()
      End If
  End Sub
  ```

  ```csharp
  private void button2_Click(object sender, System.EventArgs e)
  {
      // Displays a SaveFileDialog so the user can save the Image
      // assigned to Button2.
      SaveFileDialog saveFileDialog1 = new SaveFileDialog();
      saveFileDialog1.Filter = "JPeg Image|*.jpg|Bitmap Image|*.bmp|Gif Image|*.gif";
      saveFileDialog1.Title = "Save an Image File";
      saveFileDialog1.ShowDialog();

      // If the file name is not an empty string open it for saving.
      if(saveFileDialog1.FileName != "")
      {
        // Saves the Image via a FileStream created by the OpenFile method.
        System.IO.FileStream fs =
            (System.IO.FileStream)saveFileDialog1.OpenFile();
        // Saves the Image in the appropriate ImageFormat based upon the
        // File type selected in the dialog box.
        // NOTE that the FilterIndex property is one-based.
        switch(saveFileDialog1.FilterIndex)
        {
            case 1 :
            this.button2.Image.Save(fs,
              System.Drawing.Imaging.ImageFormat.Jpeg);
            break;

            case 2 :
            this.button2.Image.Save(fs,
              System.Drawing.Imaging.ImageFormat.Bmp);
            break;

            case 3 :
            this.button2.Image.Save(fs,
              System.Drawing.Imaging.ImageFormat.Gif);
            break;
        }

      fs.Close();
      }
  }
  ```

  ```cpp
  private:
      System::Void button2_Click(System::Object ^ sender,
        System::EventArgs ^ e)
      {
        // Displays a SaveFileDialog so the user can save the Image
        // assigned to Button2.
        SaveFileDialog ^ saveFileDialog1 = new SaveFileDialog();
        saveFileDialog1->Filter =
            "JPeg Image|*.jpg|Bitmap Image|*.bmp|Gif Image|*.gif";
        saveFileDialog1->Title = "Save an Image File";
        saveFileDialog1->ShowDialog();
        // If the file name is not an empty string, open it for saving.
        if(saveFileDialog1->FileName != "")
        {
            // Saves the Image through a FileStream created by
            // the OpenFile method.
            System::IO::FileStream ^ fs =
              safe_cast\<System::IO::FileStream*>(
              saveFileDialog1->OpenFile());
            // Saves the Image in the appropriate ImageFormat based on
            // the file type selected in the dialog box.
            // Note that the FilterIndex property is one based.
            switch(saveFileDialog1->FilterIndex)
            {
              case 1 :
                  this->button2->Image->Save(fs,
                    System::Drawing::Imaging::ImageFormat::Jpeg);
                  break;
              case 2 :
                  this->button2->Image->Save(fs,
                    System::Drawing::Imaging::ImageFormat::Bmp);
                  break;
              case 3 :
                  this->button2->Image->Save(fs,
                    System::Drawing::Imaging::ImageFormat::Gif);
                  break;
            }
        fs->Close();
        }
      }
  ```

  (ビジュアルC#とビジュアルC++)フォームのコンストラクターに次のコードを配置して、イベントハンドラーを登録します。

  ```csharp
  this.button2.Click += new System.EventHandler(this.button2_Click);
  ```

  ```cpp
  this->button2->Click += gcnew
      System::EventHandler(this, &Form1::button2_Click);
  ```

  ファイルストリームの書き込みの詳細については、「<xref:System.IO.FileStream.BeginWrite%2A>」および「<xref:System.IO.FileStream.Write%2A>」を参照してください。

  > [!NOTE]
  > <xref:System.Windows.Forms.RichTextBox> コントロールなどの特定のコントロールには、ファイルを保存する機能があります。

## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.SaveFileDialog>
- [SaveFileDialog コンポーネント](savefiledialog-component-windows-forms.md)
