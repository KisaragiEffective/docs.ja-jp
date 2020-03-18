---
title: TextBox コントロールで複数の行を表示する
ms.date: 03/30/2017
helpviewer_keywords:
- newline
- end of line
- ScrollBars property [Windows Forms], in TextBox control
- CRLF
- MultiLine property in TextBox control
- line-feed
- TextBox control [Windows Forms], viewing multiple lines
- carriage return
ms.assetid: 43173201-0b74-4067-a472-605029ca5f35
ms.openlocfilehash: 61ea671c1e86fa8254bfc1b043a46f3b7aa6af1d
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76728276"
---
# <a name="how-to-view-multiple-lines-in-the-windows-forms-textbox-control"></a>方法 : Windows フォーム TextBox コントロールで複数行を表示する
既定では、Windows フォーム <xref:System.Windows.Forms.TextBox> コントロールには1行のテキストが表示され、スクロールバーは表示されません。 テキストが使用可能な領域を超えている場合は、テキストの一部だけが表示されます。 この既定の動作を変更するには、<xref:System.Windows.Forms.TextBox.Multiline%2A>、<xref:System.Windows.Forms.TextBoxBase.WordWrap%2A>、および <xref:System.Windows.Forms.TextBox.ScrollBars%2A> の各プロパティを適切な値に設定します。  
  
### <a name="to-display-a-carriage-return-in-the-textbox-control"></a>TextBox コントロールに復帰を表示するには  
  
- 複数行の <xref:System.Windows.Forms.TextBox>に復帰を表示するには、<xref:System.Environment.NewLine%2A> プロパティを使用します。  
  
     エスケープ文字 (\\) の解釈は言語固有であることに注意してください。 Visual Basic は、キャリッジリターンとラインフィード文字の組み合わせに `Chr$(13) & Chr$(10)` を使用します。  
  
### <a name="to-view-multiple-lines-in-the-textbox-control"></a>TextBox コントロールで複数の行を表示するには  
  
1. <xref:System.Windows.Forms.TextBox.Multiline%2A> プロパティを `true` に設定します。 <xref:System.Windows.Forms.TextBoxBase.WordWrap%2A> が `true` (既定値) の場合、コントロールのテキストは1つ以上の段落として表示されます。それ以外の場合は、リストとして表示され、コントロールの端で一部の行がクリップされることがあります。  
  
2. <xref:System.Windows.Forms.TextBox.ScrollBars%2A> プロパティに適切な値を設定します。  
  
    |値|[説明]|  
    |-----------|-----------------|  
    |<xref:System.Windows.Forms.ScrollBars.None>|この値は、ほとんど常にコントロールに収まらない段落の場合に使用します。 テキストが長すぎて一度に表示できない場合、ユーザーはマウスポインターを使用してコントロール内を移動できます。|  
    |<xref:System.Windows.Forms.ScrollBars.Horizontal>|この値は、行の一覧を表示する場合に使用します。一部の行は <xref:System.Windows.Forms.TextBox> コントロールの幅よりも長くなる場合があります。|  
    |<xref:System.Windows.Forms.ScrollBars.Both>|リストがコントロールの高さよりも長い場合は、この値を使用します。|  
  
3. <xref:System.Windows.Forms.TextBoxBase.WordWrap%2A> プロパティに適切な値を設定します。  
  
    |値|[説明]|  
    |-----------|-----------------|  
    |`false`|コントロール内のテキストは自動的に折り返されないため、改行に達するまで右にスクロールします。 上のスクロールバーまたは <xref:System.Windows.Forms.ScrollBars.Both>を <xref:System.Windows.Forms.ScrollBars.Horizontal> 選択した場合は、この値を使用します。|  
    |`true` (規定値)|水平スクロールバーは表示されません。 この値は、1つまたは複数の段落を表示するために、<xref:System.Windows.Forms.ScrollBars.Vertical> のスクロールバーまたは <xref:System.Windows.Forms.ScrollBars.None>を選択した場合に使用します。|  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.TextBox>
- [TextBox コントロールの概要](textbox-control-overview-windows-forms.md)
- [方法: Windows フォーム TextBox コントロールでのカーソル位置を制御する](how-to-control-the-insertion-point-in-a-windows-forms-textbox-control.md)
- [方法: Windows フォームの TextBox コントロールを使用してパスワード テキスト ボックスを作成する](how-to-create-a-password-text-box-with-the-windows-forms-textbox-control.md)
- [方法: 読み取り専用テキスト ボックスを作成する](how-to-create-a-read-only-text-box-windows-forms.md)
- [方法: 文字列に引用符を挿入する](how-to-put-quotation-marks-in-a-string-windows-forms.md)
- [方法: Windows フォーム TextBox コントロールでテキストを選択する](how-to-select-text-in-the-windows-forms-textbox-control.md)
- [TextBox コントロール](textbox-control-windows-forms.md)
