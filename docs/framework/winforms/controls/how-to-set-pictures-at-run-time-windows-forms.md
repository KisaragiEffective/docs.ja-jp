---
title: '方法 : 実行時にピクチャを設定する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- pictures [Windows Forms], setting display
- examples [Windows Forms], PictureBox control
- bitmaps [Windows Forms], displaying in PictureBox control [Windows Forms]
- PictureBox control [Windows Forms], adding images
- images [Windows Forms], adding with PictureBox control [Windows Forms]
- PictureBox control [Windows Forms], adding pictures
ms.assetid: 18ca41d0-68a5-4660-985e-a6c1fbc01d76
ms.openlocfilehash: bd0509c05fd9c1cfc0c631fcd613c64d20296f6b
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746739"
---
# <a name="how-to-set-pictures-at-run-time-windows-forms"></a>方法 : 実行時にピクチャを設定する (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.PictureBox> コントロールによって表示されるイメージをプログラムで設定できます。  
  
### <a name="to-set-a-picture-programmatically"></a>プログラムによって画像を設定するには  
  
- <xref:System.Drawing.Image> クラスの <xref:System.Drawing.Image.FromFile%2A> メソッドを使用して、<xref:System.Windows.Forms.PictureBox.Image%2A> プロパティを設定します。  
  
     次の例では、イメージの場所に設定されたパスが [マイドキュメント] フォルダーです。 これは、Windows オペレーティングシステムを実行しているほとんどのコンピューターにこのディレクトリが含まれると想定できるためです。 また、このようにすることで、最小限のシステム アクセス レベルしか持たないユーザーもアプリケーションを安全に実行できるようになります。 次の例では、フォームに <xref:System.Windows.Forms.PictureBox> コントロールが既に追加されていることを前提としています。  
  
    ```vb  
    Private Sub LoadNewPict()  
       ' You should replace the bold image   
       ' in the sample below with an icon of your own choosing.  
       PictureBox1.Image = Image.FromFile _  
       (System.Environment.GetFolderPath _  
       (System.Environment.SpecialFolder.Personal) _  
       & "\Image.gif")  
    End Sub  
    ```  
  
    ```csharp  
    private void LoadNewPict(){  
       // You should replace the bold image   
       // in the sample below with an icon of your own choosing.  
       // Note the escape character used (@) when specifying the path.  
       pictureBox1.Image = Image.FromFile  
       (System.Environment.GetFolderPath  
       (System.Environment.SpecialFolder.Personal)  
       + @"\Image.gif");  
    }  
    ```  
  
    ```cpp  
    private:  
       void LoadNewPict()  
       {  
          // You should replace the bold image   
          // in the sample below with an icon of your own choosing.  
          pictureBox1->Image = Image::FromFile(String::Concat(  
             System::Environment::GetFolderPath(  
             System::Environment::SpecialFolder::Personal),  
             "\\Image.gif"));  
       }  
    ```  
  
### <a name="to-clear-a-graphic"></a>グラフィックをクリアするには  
  
- まず、イメージによって使用されているメモリを解放し、グラフィックをクリアします。 ガベージコレクションでは、メモリ管理が問題になると、メモリが解放されます。  
  
    ```vb  
    If Not (PictureBox1.Image Is Nothing) Then  
       PictureBox1.Image.Dispose()  
       PictureBox1.Image = Nothing  
    End If  
    ```  
  
    ```csharp  
    if (pictureBox1.Image != null)   
    {  
       pictureBox1.Image.Dispose();  
       pictureBox1.Image = null;  
    }  
    ```  
  
    ```cpp  
    if (pictureBox1->Image != nullptr)  
    {  
       pictureBox1->Image->Dispose();  
       pictureBox1->Image = nullptr;  
    }  
    ```  
  
    > [!NOTE]
    > この方法で <xref:System.Drawing.Image.Dispose%2A> メソッドを使用する理由の詳細については、「[アンマネージリソースのクリーンアップ](../../../standard/garbage-collection/unmanaged.md)」を参照してください。  
  
     このコードは、デザイン時にグラフィックがコントロールに読み込まれた場合でも、イメージをクリアします。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.PictureBox>
- <xref:System.Drawing.Image.FromFile%2A?displayProperty=nameWithType>
- [PictureBox コントロールの概要](picturebox-control-overview-windows-forms.md)
- [方法: デザイナーを使用してピクチャを読み込む](how-to-load-a-picture-using-the-designer-windows-forms.md)
- [方法: 実行時にピクチャのサイズまたは配置を変更する](how-to-modify-the-size-or-placement-of-a-picture-at-run-time-windows-forms.md)
- [PictureBox コントロール](picturebox-control-windows-forms.md)
