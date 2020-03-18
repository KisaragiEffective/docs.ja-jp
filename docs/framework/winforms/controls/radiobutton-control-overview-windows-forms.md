---
title: RadioButton コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- RadioButton
helpviewer_keywords:
- RadioButton control [Windows Forms], about RadioButton control
- RadioButton control [Windows Forms], determining state
- radio buttons [Windows Forms], determining state
- radio buttons [Windows Forms], about radio buttons
ms.assetid: cd11f0c2-d098-4022-adf9-1455bc166a13
ms.openlocfilehash: bbc93046b69d39caadde4986ff56f067fc206a9e
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741135"
---
# <a name="radiobutton-control-overview-windows-forms"></a>RadioButton コントロールの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.RadioButton> コントロールは、複数の相互に排他的な選択肢のセットをユーザーに提示します。 オプションボタンやチェックボックスは同様に機能するように見えますが、重要な違いがあります。ユーザーがラジオボタンを選択したときに、同じグループ内の他のオプションボタンを選択することはできません。 これに対して、任意の数のチェックボックスを選択できます。 ラジオボタングループを定義すると、ユーザーに "次のような選択肢があります" と表示されます。  
  
## <a name="using-the-control"></a>コントロールの使用  
 <xref:System.Windows.Forms.RadioButton> コントロールがクリックされると、その <xref:System.Windows.Forms.RadioButton.Checked%2A> プロパティが `true` に設定され、<xref:System.Windows.Forms.Control.Click> イベントハンドラーが呼び出されます。 <xref:System.Windows.Forms.RadioButton.Checked%2A> プロパティの値が変更されると、<xref:System.Windows.Forms.RadioButton.CheckedChanged> イベントが発生します。 <xref:System.Windows.Forms.RadioButton.AutoCheck%2A> プロパティが `true` (既定値) に設定されている場合、オプションボタンが選択されていると、グループ内の他のすべてのユーザーが自動的にクリアされます。 このプロパティは、通常、選択したオプションボタンが許可されているオプションであることを確認するために検証コードを使用する場合にのみ `false` に設定されます。 コントロール内に表示されるテキストは、<xref:System.Windows.Forms.Control.Text%2A> プロパティで設定されます。このプロパティには、アクセスキーのショートカットを含めることができます。 アクセスキーを使用すると、ユーザーは ALT キーを押しながらアクセスキーを押してコントロールを "クリック" できます。 詳細については、「[方法: Windows フォームコントロールのアクセスキーを作成](how-to-create-access-keys-for-windows-forms-controls.md)する」および「[方法: Windows フォームコントロールによって表示されるテキストを設定する](how-to-set-the-text-displayed-by-a-windows-forms-control.md)」を参照してください。  
  
 <xref:System.Windows.Forms.RadioButton> コントロールは、<xref:System.Windows.Forms.RadioButton.Appearance%2A> プロパティが <xref:System.Windows.Forms.Appearance.Button>に設定されている場合、選択されている場合には押されたようなコマンドボタンのように表示されます。 オプションボタンでは、<xref:System.Windows.Forms.ButtonBase.Image%2A> と <xref:System.Windows.Forms.ButtonBase.ImageList%2A> のプロパティを使用してイメージを表示することもできます。 詳細については、「[方法: Windows フォームコントロールによって表示されるイメージを設定する](how-to-set-the-image-displayed-by-a-windows-forms-control.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.RadioButton>
- [Panel コントロールの概要](panel-control-overview-windows-forms.md)
- [GroupBox コントロールの概要](groupbox-control-overview-windows-forms.md)
- [CheckBox コントロールの概要](checkbox-control-overview-windows-forms.md)
- [方法: Windows フォーム コントロールのアクセス キーを作成する](how-to-create-access-keys-for-windows-forms-controls.md)
- [方法: Windows フォーム コントロールによって表示されるテキストを設定する](how-to-set-the-text-displayed-by-a-windows-forms-control.md)
- [方法: セットとして機能する Windows フォーム RadioButton コントロールをグループ化する](how-to-group-windows-forms-radiobutton-controls-to-function-as-a-set.md)
- [RadioButton コントロール](radiobutton-control-windows-forms.md)
