---
title: ボタンコントロールを選択する方法
ms.date: 03/30/2017
helpviewer_keywords:
- Button control [Windows Forms], selecting
ms.assetid: fe2fc058-5118-4f70-b264-6147d64a7a8d
ms.openlocfilehash: 145166d182f1ec51068ab3e0c23c12b471b69231
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76740004"
---
# <a name="ways-to-select-a-windows-forms-button-control"></a>Windows フォームの Button コントロールを選択する方法
Windows フォームボタンは、次の方法で選択できます。  
  
- マウスを使用してボタンをクリックします。  
  
- コード内のボタンの <xref:System.Windows.Forms.Control.Click> イベントを呼び出します。  
  
- TAB キーを押してボタンにフォーカスを移動し、SPACE キーまたは ENTER キーを押してボタンを選択します。  
  
- ボタンのアクセスキー (ALT + 下線付きの文字) を押します。 アクセスキーの詳細については、「[方法: Windows フォームコントロールのアクセスキーを作成する](how-to-create-access-keys-for-windows-forms-controls.md)」を参照してください。  
  
- ボタンがフォームの "accept" ボタンの場合は、別のコントロールがフォーカスを持っている場合でも、ENTER キーを押すとボタンが選択されます。ただし、他のコントロールが別のボタン、複数行のテキストボックス、または ENTER キーをトラップするカスタムコントロールである場合は例外です。  
  
- ボタンがフォームの [キャンセル] ボタンの場合、ESC キーを押すと、別のコントロールにフォーカスがある場合でも、ボタンが選択されます。  
  
- <xref:System.Windows.Forms.Button.PerformClick%2A> メソッドを呼び出して、プログラムでボタンを選択します。  
  
## <a name="see-also"></a>参照

- [Button コントロールの概要](button-control-overview-windows-forms.md)
- [方法: Windows フォームのボタンのクリックに応答する](how-to-respond-to-windows-forms-button-clicks.md)
- [Button コントロール](button-control-windows-forms.md)
