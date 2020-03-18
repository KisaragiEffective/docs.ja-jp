---
title: '方法 : TextBox コントロールのテキスト コンテンツを設定する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- text content [WPF], setting
- TextBox control [WPF], setting text content
ms.assetid: bcd25fc7-a52f-4453-b802-2c8d2b335ab8
ms.openlocfilehash: 9b16f2d99295a28725255361b0be3ef7f4245fd2
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459302"
---
# <a name="how-to-set-the-text-content-of-a-textbox-control"></a>方法 : TextBox コントロールのテキスト コンテンツを設定する

この例では、<xref:System.Windows.Controls.TextBox.Text%2A> プロパティを使用して、<xref:System.Windows.Controls.TextBox> コントロールの初期テキストコンテンツを設定する方法を示します。

> [!NOTE]
> [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] バージョンの例では、各ボタンの <xref:System.Windows.Controls.TextBox> コンテンツのテキストを囲む `<TextBox.Text>` タグを使用できますが、<xref:System.Windows.Markup.ContentPropertyAttribute> 属性が <xref:System.Windows.Controls.TextBox> によって <xref:System.Windows.Controls.TextBox.Text%2A> プロパティに適用されるため、これは必要ありません。 詳細については、「 [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)」を参照してください。

## <a name="example"></a>例

[!code-xaml[TextBox_MiscCode#_TextBoxSetTextXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/TextBox_MiscCode/CSharp/Window1.xaml#_textboxsettextxaml)]

## <a name="example"></a>例

[!code-csharp[TextBox_MiscCode#_TextBoxSetText](~/samples/snippets/csharp/VS_Snippets_Wpf/TextBox_MiscCode/CSharp/Window1.xaml.cs#_textboxsettext)]
[!code-vb[TextBox_MiscCode#_TextBoxSetText](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TextBox_MiscCode/VisualBasic/Window1.xaml.vb#_textboxsettext)]

## <a name="see-also"></a>関連項目

- [TextBox の概要](textbox-overview.md)
- [RichTextBox の概要](richtextbox-overview.md)
