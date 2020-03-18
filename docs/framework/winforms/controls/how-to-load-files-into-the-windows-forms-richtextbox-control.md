---
title: RichTextBox コントロールへのファイルの読み込み
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- text boxes [Windows Forms], displaying files
- examples [Windows Forms], text boxes
- .rtf files [Windows Forms], opening in RichTextBox control
- RTF files [Windows Forms], opening in RichTextBox control
- text files [Windows Forms], displaying in RichTextBox control
- .rtf files [Windows Forms], displaying in RichTextBox control
- RichTextBox control [Windows Forms], opening files
- RTF files [Windows Forms], displaying in RichTextBox control
ms.assetid: c03451be-f285-4428-a71a-c41e002cc919
ms.openlocfilehash: c31e004ea4cd0821b5f18f0ab0fe2708e6ac4b59
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736286"
---
# <a name="how-to-load-files-into-the-windows-forms-richtextbox-control"></a>方法 : Windows フォームの RichTextBox コントロールにファイルを読み込む

Windows フォームの <xref:System.Windows.Forms.RichTextBox> コントロールには、プレーン テキスト、Unicode のプレーン テキスト、リッチ テキスト形式 (RTF) ファイルを表示できます。 それには、 <xref:System.Windows.Forms.RichTextBox.LoadFile%2A> メソッドを呼び出します。 また、 <xref:System.Windows.Forms.RichTextBox.LoadFile%2A> メソッドを使用してストリームからデータを読み込むこともできます。 詳細については、<xref:System.Windows.Forms.RichTextBox.LoadFile%28System.IO.Stream%2CSystem.Windows.Forms.RichTextBoxStreamType%29> を参照してください。

### <a name="to-load-a-file-into-the-richtextbox-control"></a>RichTextBox コントロールにファイルを読み込むには

1. <xref:System.Windows.Forms.OpenFileDialog> コンポーネントを使用して、開くファイルのパスを決定します。 概要については、「 [OpenFileDialog コンポーネントの概要](openfiledialog-component-overview-windows-forms.md)」を参照してください。

2. 読み込むファイルと、必要に応じてファイルの種類を指定して、 <xref:System.Windows.Forms.RichTextBox.LoadFile%2A> コントロールの <xref:System.Windows.Forms.RichTextBox> メソッドを呼び出します。 次の例では、読み込むファイルは <xref:System.Windows.Forms.OpenFileDialog> コンポーネントの <xref:System.Windows.Forms.FileDialog.FileName%2A> プロパティから取得されます。 引数にファイル名だけを指定してメソッドを呼び出すと、ファイルの種類は RTF と見なされます。 別の種類のファイルを指定するには、2 番目の引数として <xref:System.Windows.Forms.RichTextBoxStreamType> 列挙型の値を指定します。

    次の例では、ボタンがクリックされたときに <xref:System.Windows.Forms.OpenFileDialog> コンポーネントが表示されます。 次に、選択されたファイルが開き、 <xref:System.Windows.Forms.RichTextBox> コントロールに表示されます。 この例ではフォームにボタン`btnOpenFile`があることを前提としています。

    ```vb
    Private Sub btnOpenFile_Click(ByVal sender As System.Object, _
       ByVal e As System.EventArgs) Handles btnOpenFile.Click
         If OpenFileDialog1.ShowDialog() = DialogResult.OK Then
           RichTextBox1.LoadFile(OpenFileDialog1.FileName, _
              RichTextBoxStreamType.RichText)
          End If
    End Sub
    ```

    ```csharp
    private void btnOpenFile_Click(object sender, System.EventArgs e)
    {
       if(openFileDialog1.ShowDialog() == DialogResult.OK)
       {
         richTextBox1.LoadFile(openFileDialog1.FileName, RichTextBoxStreamType.RichText);
       }
    }
    ```

    ```cpp
    private:
       void btnOpenFile_Click(System::Object ^  sender,
          System::EventArgs ^  e)
       {
          if(openFileDialog1->ShowDialog() == DialogResult::OK)
          {
             richTextBox1->LoadFile(openFileDialog1->FileName,
                RichTextBoxStreamType::RichText);
          }
       }
    ```

    (ビジュアルC#、ビジュアルC++)フォームのコンストラクターに次のコードを配置して、イベントハンドラーを登録します。

    ```csharp
    this.btnOpenFile.Click += new System.EventHandler(this. btnOpenFile_Click);
    ```

    ```cpp
    this->btnOpenFile->Click += gcnew
       System::EventHandler(this, &Form1::btnOpenFile_Click);
    ```

    > [!IMPORTANT]
    > このプロセスを実行するには、アセンブリに対して <xref:System.Security.Permissions.FileIOPermission?displayProperty=nameWithType> クラスで特権レベルが許可されていることが必要な場合があります。 部分的に信頼されたコンテキストで実行している場合、プロセスは、特権がないため例外をスローする可能性があります。 詳しくは、「[コード アクセス セキュリティの基礎](../../misc/code-access-security-basics.md)」をご覧ください。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.RichTextBox.LoadFile%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.RichTextBox>
- [RichTextBox コントロール](richtextbox-control-windows-forms.md)
- [Windows フォームで使用するコントロール](controls-to-use-on-windows-forms.md)
