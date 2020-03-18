---
title: RichTextBox コントロールのフォント属性を設定する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- .rtf files [Windows Forms], formatting in RichTextBox control
- fonts [Windows Forms], changing attributes in RichTextBox control
- RTF files [Windows Forms], formatting in RichTextBox control
- RichTextBox control [Windows Forms], setting font attributes
- text [Windows Forms]
- text boxes [Windows Forms], formatting text
- formatting [Windows Forms]
ms.assetid: 2bc23ddb-0529-4489-a1a2-ad253cb43f9a
ms.openlocfilehash: f27256c155223df576ee3c42e6bf65270c870b0f
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744853"
---
# <a name="how-to-set-font-attributes-for-the-windows-forms-richtextbox-control"></a>方法: Windows フォームの RichTextBox コントロールのフォント属性を設定する
Windows フォーム <xref:System.Windows.Forms.RichTextBox> コントロールには、表示するテキストを書式設定するためのさまざまなオプションがあります。 <xref:System.Windows.Forms.RichTextBox.SelectionFont%2A> プロパティを使用して、選択した文字を太字、下線、または斜体にすることができます。 また、このプロパティを使用して、選択した文字のサイズと書体を変更することもできます。 <xref:System.Windows.Forms.RichTextBox.SelectionColor%2A> プロパティを使用すると、選択した文字の色を変更できます。  
  
### <a name="to-change-the-appearance-of-characters"></a>文字の外観を変更するには  
  
1. <xref:System.Windows.Forms.RichTextBox.SelectionFont%2A> プロパティを適切なフォントに設定します。  
  
     ユーザーがアプリケーションでフォントファミリ、サイズ、およびタイプフェイスを設定できるようにするには、通常、<xref:System.Windows.Forms.FontDialog> コンポーネントを使用します。 概要については、「[FontDialog Component Overview](fontdialog-component-overview-windows-forms.md)」 (FontDialog コンポーネントの概要) を参照してください。  
  
2. <xref:System.Windows.Forms.RichTextBox.SelectionColor%2A> プロパティを適切な色に設定します。  
  
     ユーザーがアプリケーションの色を設定できるようにするには、通常、<xref:System.Windows.Forms.ColorDialog> コンポーネントを使用します。 概要については、「[ColorDialog Component Overview](colordialog-component-overview-windows-forms.md)」 (ColorDialog コンポーネントの概要) を参照してください。  
  
    ```vb  
    RichTextBox1.SelectionFont = New Font("Tahoma", 12, FontStyle.Bold)  
    RichTextBox1.SelectionColor = System.Drawing.Color.Red  
    ```  
  
    ```csharp  
    richTextBox1.SelectionFont = new Font("Tahoma", 12, FontStyle.Bold);  
    richTextBox1.SelectionColor = System.Drawing.Color.Red;  
    ```  
  
    ```cpp  
    richTextBox1->SelectionFont =  
       gcnew System::Drawing::Font("Tahoma", 12, FontStyle::Bold);  
    richTextBox1->SelectionColor = System::Drawing::Color::Red;  
    ```  
  
    > [!NOTE]
    > これらのプロパティは選択したテキストにのみ影響します。テキストが選択されていない場合は、現在の挿入ポイントの場所に入力されるテキストに影響します。 プログラムによるテキストの選択の詳細については、「<xref:System.Windows.Forms.TextBoxBase.Select%2A>」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.RichTextBox>
- [RichTextBox コントロール](richtextbox-control-windows-forms.md)
- [Windows フォームで使用するコントロール](controls-to-use-on-windows-forms.md)
