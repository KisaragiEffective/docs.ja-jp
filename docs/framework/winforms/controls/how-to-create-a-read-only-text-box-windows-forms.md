---
title: '方法 : 読み取り専用テキスト ボックスを作成する'
ms.date: 03/30/2017
helpviewer_keywords:
- TextBox control [Windows Forms], read-only
- read-only text boxes
- text boxes [Windows Forms], read-only
ms.assetid: 60baa9ab-fa57-44ad-bb7c-61b05aa64296
ms.openlocfilehash: 17ae9524009c687cd62fb315f842e188e120ac68
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76731272"
---
# <a name="how-to-create-a-read-only-text-box-windows-forms"></a>方法 : 読み取り専用テキスト ボックスを作成する (Windows フォーム)

編集可能な Windows フォームテキストボックスを読み取り専用のコントロールに変換できます。 たとえば、テキストボックスには通常は編集されているが、現在はアプリケーションの状態によっては編集されていない値が表示される場合があります。

## <a name="to-create-a-read-only-text-box"></a>読み取り専用のテキストボックスを作成するには

1. <xref:System.Windows.Forms.TextBox> コントロールの <xref:System.Windows.Forms.TextBoxBase.ReadOnly%2A> プロパティを `true`に設定します。 プロパティが `true`に設定されている場合でも、ユーザーはテキストボックス内のテキストをスクロールして強調表示できますが、変更は許可されません。 **Copy**コマンドはテキストボックスで機能しますが、**切り取り**と**貼り付け**のコマンドは機能しません。

    > [!NOTE]
    > <xref:System.Windows.Forms.TextBoxBase.ReadOnly%2A> プロパティは、実行時のユーザーの操作にのみ影響します。 テキストボックスのコンテンツは、実行時にプログラムによって変更することもできます。そのためには、テキストボックスの [<xref:System.Windows.Forms.TextBox.Text%2A>] プロパティを変更します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.TextBox>
- [TextBox コントロールの概要](textbox-control-overview-windows-forms.md)
- [方法: Windows フォーム TextBox コントロールでのカーソル位置を制御する](how-to-control-the-insertion-point-in-a-windows-forms-textbox-control.md)
- [方法: Windows フォームの TextBox コントロールを使用してパスワード テキスト ボックスを作成する](how-to-create-a-password-text-box-with-the-windows-forms-textbox-control.md)
- [方法: 文字列に引用符を挿入する](how-to-put-quotation-marks-in-a-string-windows-forms.md)
- [方法: Windows フォーム TextBox コントロールでテキストを選択する](how-to-select-text-in-the-windows-forms-textbox-control.md)
- [方法: Windows フォーム TextBox コントロールで複数行を表示する](how-to-view-multiple-lines-in-the-windows-forms-textbox-control.md)
- [TextBox コントロール](textbox-control-windows-forms.md)
