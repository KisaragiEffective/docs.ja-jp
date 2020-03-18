---
title: RichTextBox コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- RichTextBox
helpviewer_keywords:
- RichTextBox control [Windows Forms], about RichTextBox control
- text boxes [Windows Forms], about text boxes
ms.assetid: 95081194-3dd4-4b84-9545-dd373e491eca
ms.openlocfilehash: 0d40967d97622b23e9dcb07e861f190327e6baba
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742396"
---
# <a name="richtextbox-control-overview-windows-forms"></a>RichTextBox コントロールの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.RichTextBox> コントロールは、書式設定を使用してテキストを表示、入力、および操作するために使用されます。 <xref:System.Windows.Forms.RichTextBox> コントロールは、<xref:System.Windows.Forms.TextBox> コントロールが行うすべての操作を実行しますが、フォント、色、およびリンクを表示することもできます。ファイルからテキストと埋め込み画像を読み込みます。指定された文字を検索します。 <xref:System.Windows.Forms.RichTextBox> コントロールは、通常、Microsoft Word などのワードプロセッシングアプリケーションに似たテキスト操作と表示機能を提供するために使用されます。 <xref:System.Windows.Forms.TextBox> コントロールと同様に、<xref:System.Windows.Forms.RichTextBox> コントロールはスクロールバーを表示できます。ただし、<xref:System.Windows.Forms.TextBox> コントロールとは異なり、既定の設定では、必要に応じて水平スクロールバーと垂直スクロールバーの両方が表示され、追加のスクロールバーの設定があります。  
  
## <a name="working-with-the-richtextbox-control"></a>RichTextBox コントロールの操作  
 <xref:System.Windows.Forms.TextBox> コントロールと同様に、表示されるテキストは <xref:System.Windows.Forms.RichTextBox.Text%2A> プロパティによって設定されます。 <xref:System.Windows.Forms.RichTextBox> コントロールには、テキストを書式設定するための多数のプロパティがあります。 これらのプロパティの詳細については、「[How to: Set Font Attributes for the Windows Forms RichTextBox Control (方法: Windows フォームの RichTextBox コントロールのフォント属性を設定)](how-to-set-font-attributes-for-the-windows-forms-richtextbox-control.md)」と「[How to: Set Indents, Hanging Indents, and Bulleted Paragraphs with the Windows Forms RichTextBox Control (方法: Windows フォームの RichTextBox コントロールを使用したインデント、ぶら下げインデント、および箇条書き段落の設定)](set-indents-hanging-indents-bulleted-paragraphs-with-wf-richtextbox.md)」参照してください。 ファイルを操作するために、<xref:System.Windows.Forms.RichTextBox.LoadFile%2A> および <xref:System.Windows.Forms.RichTextBox.SaveFile%2A> メソッドでは、プレーンテキスト、Unicode プレーンテキスト、リッチテキスト形式 (RTF) など、複数のファイル形式を表示して書き込むことができます。 使用可能なファイル形式は <xref:System.Windows.Forms.RichTextBoxStreamType>に記載されています。 <xref:System.Windows.Forms.RichTextBox.Find%2A> メソッドを使用して、テキストまたは特定の文字の文字列を検索できます。  
  
 また、<xref:System.Windows.Forms.RichTextBox.DetectUrls%2A> プロパティを `true` に設定し、<xref:System.Windows.Forms.RichTextBox.LinkClicked> イベントを処理するコードを記述することによって、Web スタイルのリンクに <xref:System.Windows.Forms.RichTextBox> コントロールを使用することもできます。 詳細については、「[How to: Display Web-Style Links with the Windows Forms RichTextBox Control (方法: Windows フォームの RichTextBox コントロールを使用して Web スタイルのリンクを表示する)](how-to-display-web-style-links-with-the-windows-forms-richtextbox-control.md)」を参照してください。 <xref:System.Windows.Forms.RichTextBox.SelectionProtected%2A> プロパティを `true`に設定することによって、コントロール内のテキストの一部またはすべてをユーザーが操作できないようにすることができます。  
  
 <xref:System.Windows.Forms.TextBoxBase.Undo%2A> メソッドと <xref:System.Windows.Forms.RichTextBox.Redo%2A> メソッドを呼び出すことによって、<xref:System.Windows.Forms.RichTextBox> コントロールでほとんどの編集操作を元に戻したり、やり直したりできます。 <xref:System.Windows.Forms.RichTextBox.CanRedo%2A> メソッドを使用すると、ユーザーが最後に元に戻した操作をコントロールに再適用できるかどうかを判断できます。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.RichTextBox>
- [RichTextBox コントロール](richtextbox-control-windows-forms.md)
- [TextBox コントロールの概要](textbox-control-overview-windows-forms.md)
